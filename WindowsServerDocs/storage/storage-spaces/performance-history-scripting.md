---
title: Scripting con cronologia delle prestazioni di spazi di archiviazione diretta
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 78ecb64cac789751abf9fd3f55b4a1fcbbe4dad2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2018
ms.locfileid: "8843109"
---
# Scripting con PowerShell e spazi di archiviazione diretta cronologia delle prestazioni

> Si applica a: Windows Server Insider Preview build 17692 e successive

In Windows Server 2019, [Spazi di archiviazione diretta](storage-spaces-direct-overview.md) registra e archivia completa [la cronologia delle prestazioni](performance-history.md) per le macchine virtuali, server, unità, volumi, schede di rete e altro ancora. Cronologia delle prestazioni è facile eseguire una query e il processo di PowerShell in modo è possibile passare rapidamente da *dati non elaborati* a *effettive risposte* alle domande come:

1. Sono stati qualsiasi CPU viene utilizzata la scorsa settimana?
2. È qualsiasi disco fisico e possiede Latenza anomala?
3. Le macchine virtuali consumano più archiviazione IOPS al momento?
4. È la larghezza di banda saturi?
5. Quando questo volume verrà eseguiti all'esterno di spazio libero?
6. Nell'ultimo mese, quale le macchine virtuali usato la maggior parte della memoria?

Il `Get-ClusterPerf` cmdlet è concepito per la creazione di script. Il metodo accetta l'input dal cmdlet, come `Get-VM` o `Get-PhysicalDisk` dalla pipeline di gestire associazione ed è possibile reindirizzare l'output nel cmdlet utilità, come `Sort-Object`, `Where-Object`, e `Measure-Object` per comporre rapidamente query potenti.

**Questo argomento fornisce e spiega 6 script di esempio che rispondere alle 6 domande precedente.** Sono presenti modelli che puoi applicare per trovare picchi, trovare medie, tracciare linee di tendenza, Esegui valore anomalo rilevamento e altro ancora, per un'ampia gamma di dati e periodi per i pagamenti. Vengono forniti come codice starter gratuito per copiare, estendere e riutilizzare.

   > [!NOTE]
   > Per brevità, gli script di esempio omettono elementi come la gestione degli errori che si prevede di codice di PowerShell di alta qualità. Destinati principalmente per trovare ispirazione e la formazione invece di produzione usare.

## Esempio 1: CPU, vedo puoi!

Questo esempio Usa il `ClusterNode.Cpu.Usage` serie dal `LastWeek` intervallo di tempo per mostrare il massimo ("limite massimo"), l'utilizzo della CPU, minimo e medio per ogni server del cluster. Esegue anche l'analisi del quartile semplice per mostrare l'utilizzo di ore CPU quanti è stata oltre al 25%, al 50% e al 75% negli ultimi giorni 8.

### Screenshot

Nello screenshot seguente, vediamo che *02 Server* era un picco sconosciuto scorsa settimana:

![Screenshot di PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### Come funziona

L'output di `Get-ClusterPerf` pipe 48px in predefiniti per il `Measure-Object` cmdlet, è sufficiente specificare il `Value` proprietà. Con il relativo `-Maximum`, `-Minimum`, e `-Average` flag, `Measure-Object` otteniamo le prime tre colonne quasi per senza costi aggiuntivi. Per eseguire l'analisi del quartile, abbiamo possiamo pipe a `Where-Object` e contare il numero di valori erano `-Gt` (maggiore) 25, 50 o 75. L'ultimo passaggio consiste nel beautify con `Format-Hours` e `Format-Percent` funzioni helper – sicuramente facoltativo.

### Script

Ecco lo script:

```
Function Format-Hours {
    Param (
        $RawValue
    )
    # Weekly timeframe has frequency 15 minutes = 4 points per hour
    [Math]::Round($RawValue/4)
}

Function Format-Percent {
    Param (
        $RawValue
    )
    [String][Math]::Round($RawValue) + " " + "%"
}

$Output = Get-ClusterNode | ForEach-Object {
    $Data = $_ | Get-ClusterPerf -ClusterNodeSeriesName "ClusterNode.Cpu.Usage" -TimeFrame "LastWeek"

    $Measure = $Data | Measure-Object -Property Value -Minimum -Maximum -Average
    $Min = $Measure.Minimum
    $Max = $Measure.Maximum
    $Avg = $Measure.Average

    [PsCustomObject]@{
        "ClusterNode"    = $_.Name
        "MinCpuObserved" = Format-Percent $Min
        "MaxCpuObserved" = Format-Percent $Max
        "AvgCpuObserved" = Format-Percent $Avg
        "HrsOver25%"     = Format-Hours ($Data | Where-Object Value -Gt 25).Length
        "HrsOver50%"     = Format-Hours ($Data | Where-Object Value -Gt 50).Length
        "HrsOver75%"     = Format-Hours ($Data | Where-Object Value -Gt 75).Length
    }
}

$Output | Sort-Object ClusterNode | Format-Table
```

## Esempio 2: Tiro, tiro, valore anomalo latenza

Questo esempio Usa il `PhysicalDisk.Latency.Average` serie dal `LastHour` intervallo di tempo per cercare anomali statistiche definito come unità con una latenza media oraria superiore + 3σ (tre deviazioni standard) sopra la media della popolazione.

   > [!IMPORTANT]
   > Per brevità, questo script non implementa misure di protezione contro varianza bassa, non gestisce dati mancanti parziali, non è possibile distinguerle dal modello o firmware e così via. Tieni esercitare buon e non fare affidamento su questo script da solo per determinare se si desidera sostituire un disco rigido. Venga presentato qui solo a scopo didattico.

### Screenshot

Nello screenshot seguente, vediamo che non sono presenti valori alcun erratici:

![Screenshot di PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### Come funziona

Prima di tutto, abbiamo escludere unità di inattività o quasi inattiva verificando che `PhysicalDisk.Iops.Total` è in modo coerente `-Gt 1`. Per ogni unità disco rigido attivo, ti invia pipe relativa `LastHour` intervallo di tempo, comprende le 360 misurazioni 10 intervalli secondi, a `Measure-Object -Average` per ottenere la latenza media nell'ultima ora. Ciò consente di configurare il nostro popolazione.

Implementiamo la [formula ampiamente noti](http://www.mathsisfun.com/data/standard-deviation.html) per trovare la Media `μ` e deviazione `σ` della popolazione. Per ogni unità disco rigido attivo, abbiamo confrontare la latenza media alla media della popolazione e dividere per la deviazione standard. Abbiamo mantenere i valori non elaborati, eseguiamo `Sort-Object` i risultati, ma usa `Format-Latency` e `Format-StandardDeviation` funzioni helper per beautify cosa ti mostreremo – sicuramente facoltativi.

Se le unità più + 3σ, abbiamo `Write-Host` in rosso; Se non, in verde.

### Script

Ecco lo script:

```
Function Format-Latency {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("s", "ms", "μs", "ns") # Petabits, just in case!
    Do { $RawValue *= 1000 ; $i++ } While ( $RawValue -Lt 1 )
    # Return
    [String][Math]::Round($RawValue, 2) + " " + $Labels[$i]
}

Function Format-StandardDeviation {
    Param (
        $RawValue
    )
    If ($RawValue -Gt 0) {
        $Sign = "+"
    }
    Else {
        $Sign = "-"
    }
    # Return
    $Sign + [String][Math]::Round([Math]::Abs($RawValue), 2) + "σ"
}

$HDD = Get-StorageSubSystem Cluster* | Get-PhysicalDisk | Where-Object MediaType -Eq HDD

$Output = $HDD | ForEach-Object {

    $Iops = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Iops.Total" -TimeFrame "LastHour"
    $AvgIops = ($Iops | Measure-Object -Property Value -Average).Average

    If ($AvgIops -Gt 1) { # Exclude idle or nearly idle drives

        $Latency = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Latency.Average" -TimeFrame "LastHour"
        $AvgLatency = ($Latency | Measure-Object -Property Value -Average).Average

        [PsCustomObject]@{
            "FriendlyName"  = $_.FriendlyName
            "SerialNumber"  = $_.SerialNumber
            "MediaType"     = $_.MediaType
            "AvgLatencyPopulation" = $null # Set below
            "AvgLatencyThisHDD"    = Format-Latency $AvgLatency
            "RawAvgLatencyThisHDD" = $AvgLatency
            "Deviation"            = $null # Set below
            "RawDeviation"         = $null # Set below
        }
    }
}

If ($Output.Length -Ge 3) { # Minimum population requirement

    # Find mean μ and standard deviation σ
    $μ = ($Output | Measure-Object -Property RawAvgLatencyThisHDD -Average).Average
    $d = $Output | ForEach-Object { ($_.RawAvgLatencyThisHDD - $μ) * ($_.RawAvgLatencyThisHDD - $μ) }
    $σ = [Math]::Sqrt(($d | Measure-Object -Sum).Sum / $Output.Length)

    $FoundOutlier = $False

    $Output | ForEach-Object {
        $Deviation = ($_.RawAvgLatencyThisHDD - $μ) / $σ
        $_.AvgLatencyPopulation = Format-Latency $μ
        $_.Deviation = Format-StandardDeviation $Deviation
        $_.RawDeviation = $Deviation
        # If distribution is Normal, expect >99% within 3σ
        If ($Deviation -Gt 3) {
            $FoundOutlier = $True
        }
    }

    If ($FoundOutlier) {
        Write-Host -BackgroundColor Black -ForegroundColor Red "Oh no! There's an HDD significantly slower than the others."
    }
    Else {
        Write-Host -BackgroundColor Black -ForegroundColor Green "Good news! No outlier found."
    }

    $Output | Sort-Object RawDeviation -Descending | Format-Table FriendlyName, SerialNumber, MediaType, AvgLatencyPopulation, AvgLatencyThisHDD, Deviation

}
Else {
    Write-Warning "There aren't enough active drives to look for outliers right now."
}
```

## Esempio 3: Fastidiosi? Ecco scrittura!

Cronologia delle prestazioni possa rispondere a domande relative *al momento*, troppo. Le misurazioni nuove sono disponibili in tempo reale, ogni 10 secondi. Questo esempio Usa il `VHD.Iops.Total` serie dal `MostRecent` intervallo di tempo per identificare il più attivi (alcuni potrebbe pronunciare "che provocano maggiore disturbo") uso più in ogni host nel cluster e Mostra la suddivisione in lettura/scrittura le proprie attività di archiviazione IOPS, le macchine virtuali.

### Screenshot

Nello screenshot seguente, vediamo le macchine virtuali primi 10 dall'attività di archiviazione:

![Screenshot di PowerShell](media/performance-history/Show-TopIopsVMs.png)

### Come funziona

A differenza di `Get-PhysicalDisk`, `Get-VM` cmdlet non è compatibile con cluster, viene restituito solo macchine virtuali nel server locale. Per eseguire una query da ogni server in parallelo, wrapping nostro chiamata in `Invoke-Command (Get-ClusterNode).Name { ... }`. Per ogni macchina virtuale, otteniamo le `VHD.Iops.Total`, `VHD.Iops.Read`, e `VHD.Iops.Write` le misurazioni. Non specificando il `-TimeFrame` parametro, otteniamo le `MostRecent` singolo punto dati per ognuno.

   > [!TIP]
   > Queste serie rifletteranno la somma di tutti i relativi file VHD/VHDX dell'attività della macchina virtuale. Questo è un esempio in cui la cronologia delle prestazioni viene automaticamente aggregata per noi. Per ottenere la suddivisione per ogni file VHD/VHDX, è possibile reindirizzare un singolo `Get-VHD` in `Get-ClusterPerf` invece della macchina virtuale.

I risultati da ogni server convergono come `$Output`, che è possibile `Sort-Object` e quindi `Select-Object -First 10`. Si noti che `Invoke-Command` aggiunga risultati con un `PsComputerName` proprietà che indicano la provenienza, che possiamo stampare sapere in cui è in esecuzione la macchina virtuale.

### Script

Ecco lo script:

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Iops {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = (" ", "K", "M", "B", "T") # Thousands, millions, billions, trillions...
        Do { if($RawValue -Gt 1000){$RawValue /= 1000 ; $i++ } } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-VM | ForEach-Object {
        $IopsTotal = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Total"
        $IopsRead  = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Read"
        $IopsWrite = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Write"
        [PsCustomObject]@{
            "VM" = $_.Name
            "IopsTotal" = Format-Iops $IopsTotal.Value
            "IopsRead"  = Format-Iops $IopsRead.Value
            "IopsWrite" = Format-Iops $IopsWrite.Value
            "RawIopsTotal" = $IopsTotal.Value # For sorting...
        }
    }
}

$Output | Sort-Object RawIopsTotal -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, IopsTotal, IopsRead, IopsWrite
```

## Esempio 4: Come si dice, "25 GB è il nuovo 10-GB"

Questo esempio Usa il `NetAdapter.Bandwidth.Total` serie dal `LastDay` definita come intervallo di tempo per cercare segni di saturazione di rete, > 90% della larghezza di banda massima teorica. Per ogni scheda di rete del cluster, confronta l'utilizzo della larghezza di banda osservati più alta nell'ultimo giorno a raggiungere la velocità di collegamento dichiarato.

### Screenshot

Nello screenshot seguente, vediamo che uno *Fabrikam NX-4 Pro n. 2* un massimo nell'ultimo giorno:

![Screenshot di PowerShell](media/performance-history/Show-NetworkSaturation.png)

### Come funziona

Abbiamo ripetere il nostro `Invoke-Command` perché l'operazione da sopra `Get-NetAdapter` in ogni server e pipe in `Get-ClusterPerf`. Lungo il percorso, abbiamo afferrare due proprietà pertinente: il `LinkSpeed` stringa come "10 Gbps" e il relativo raw `Speed` integer come 10000000000. Usiamo `Measure-Object` per ottenere la media e picco dall'ultimo giorno (promemoria: ogni misurazione nel `LastDay` intervallo di tempo rappresenta 5 minuti) e per 8 bit per byte per ottenere un confronto mele a mele.

   > [!NOTE]
   > Alcuni fornitori, ad esempio Chelsio, includono attività (RDMA) l'accesso diretto a memoria remota nel relativi contatori delle prestazioni di *Scheda di rete* , pertanto è incluso nel `NetAdapter.Bandwidth.Total` serie. Altri utenti, ad esempio schede Mellanox, potrebbe non. Se il fornitore non, è sufficiente aggiunta il `NetAdapter.Bandwidth.RDMA.Total` serie nella tua versione di questo script.

### Script

Ecco lo script:

```
$Output = Invoke-Command (Get-ClusterNode).Name {

    Function Format-BitsPerSec {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("bps", "kbps", "Mbps", "Gbps", "Tbps", "Pbps") # Petabits, just in case!
        Do { $RawValue /= 1000 ; $i++ } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-NetAdapter | ForEach-Object {

        $Inbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Inbound" -TimeFrame "LastDay"
        $Outbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Outbound" -TimeFrame "LastDay"

        If ($Inbound -Or $Outbound) {

            $InterfaceDescription = $_.InterfaceDescription
            $LinkSpeed = $_.LinkSpeed
    
            $MeasureInbound = $Inbound | Measure-Object -Property Value -Maximum
            $MaxInbound = $MeasureInbound.Maximum * 8 # Multiply to bits/sec
    
            $MeasureOutbound = $Outbound | Measure-Object -Property Value -Maximum
            $MaxOutbound = $MeasureOutbound.Maximum * 8 # Multiply to bits/sec
    
            $Saturated = $False
    
            # Speed property is Int, e.g. 10000000000
            If (($MaxInbound -Gt (0.90 * $_.Speed)) -Or ($MaxOutbound -Gt (0.90 * $_.Speed))) {
                $Saturated = $True
                Write-Warning "In the last day, adapter '$InterfaceDescription' on server '$Env:ComputerName' exceeded 90% of its '$LinkSpeed' theoretical maximum bandwidth. In general, network saturation leads to higher latency and diminished reliability. Not good!"
            }
    
            [PsCustomObject]@{
                "NetAdapter"  = $InterfaceDescription
                "LinkSpeed"   = $LinkSpeed
                "MaxInbound"  = Format-BitsPerSec $MaxInbound
                "MaxOutbound" = Format-BitsPerSec $MaxOutbound
                "Saturated"   = $Saturated
            }
        }
    }
}

$Output | Sort-Object PsComputerName, InterfaceDescription | Format-Table PsComputerName, NetAdapter, LinkSpeed, MaxInbound, MaxOutbound, Saturated
```

## Esempio 5: Ottimizzare archiviazione trendy nuovamente.

Per esaminare le tendenze di macro, viene mantenuta la cronologia delle prestazioni per fino a 1 anno. Questo esempio Usa il `Volume.Size.Available` serie dal `LastYear` intervallo di tempo per determinare la frequenza con cui è in esaurimento archiviazione e stima quando sarà completa.

### Screenshot

Nello screenshot seguente, vediamo che il volume *Backup* aggiunge circa 15 GB per ogni giorno:

![Screenshot di PowerShell](media/performance-history/Show-StorageTrend.png)

Con questa frequenza, raggiungerà la capacità in un altro 42 giorni.

### Come funziona

Il `LastYear` intervallo di tempo è un punto di dati per ogni giorno. Anche se è necessario solo strettamente due punti per adattare una riga di tendenza, nella pratica è preferibile richiedere ulteriori informazioni, ad esempio 14 giorni. Usiamo `Select-Object -Last 14` per configurare una matrice di punti *(x, y)* , per *x* nell'intervallo [1, 14]. Con questi punti, Implementiamo l' [algoritmo lineare minimi quadrati](http://mathworld.wolfram.com/LeastSquaresFitting.html) molto semplice per trovare `$A` e `$B` che i parametri per la riga di Adatta *y = ax + b*. Benvenuti in scuole tutto nuovamente.

Dividere il volume `SizeRemaining` proprietà per la tendenza (l'inclinazione `$A`) consente di stimare controlla il numero di giorni, con la tariffa corrente della crescita di archiviazione, fino a quando il volume è pieno. Il `Format-Bytes`, `Format-Trend`, e `Format-Days` funzioni helper beautify l'output.

   > [!IMPORTANT]
   > Questa stima è lineare e in base solo le misurazioni giornaliere 14 più recente. Sono presenti più sofisticate e precisa tecniche. Tieni esercitare buon e non fare affidamento su questo script da solo per determinare se a investire in espandendo il dispositivo di archiviazione. Venga presentato qui solo a scopo didattico.

### Script

Ecco lo script:

```

Function Format-Bytes {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
    Do { $RawValue /= 1024 ; $i++ } While ( $RawValue -Gt 1024 )
    # Return
    [String][Math]::Round($RawValue) + " " + $Labels[$i]
}

Function Format-Trend {
    Param (
        $RawValue
    )
    If ($RawValue -Eq 0) {
        "0"
    }
    Else {
        If ($RawValue -Gt 0) {
            $Sign = "+"
        }
        Else {
            $Sign = "-"
        }
        # Return
        $Sign + $(Format-Bytes [Math]::Abs($RawValue)) + "/day"
    }
}

Function Format-Days {
    Param (
        $RawValue
    )
    [Math]::Round($RawValue)
}

$CSV = Get-Volume | Where-Object FileSystem -Like "*CSV*"

$Output = $CSV | ForEach-Object {

    $N = 14 # Require 14 days of history

    $Data = $_ | Get-ClusterPerf -VolumeSeriesName "Volume.Size.Available" -TimeFrame "LastYear" | Sort-Object Time | Select-Object -Last $N

    If ($Data.Length -Ge $N) {

        # Last N days as (x, y) points
        $PointsXY = @()
        1..$N | ForEach-Object {
            $PointsXY += [PsCustomObject]@{ "X" = $_ ; "Y" = $Data[$_-1].Value }
        }

        # Linear (y = ax + b) least squares algorithm
        $MeanX = ($PointsXY | Measure-Object -Property X -Average).Average
        $MeanY = ($PointsXY | Measure-Object -Property Y -Average).Average
        $XX = $PointsXY | ForEach-Object { $_.X * $_.X }
        $XY = $PointsXY | ForEach-Object { $_.X * $_.Y }
        $SSXX = ($XX | Measure-Object -Sum).Sum - $N * $MeanX * $MeanX
        $SSXY = ($XY | Measure-Object -Sum).Sum - $N * $MeanX * $MeanY
        $A = ($SSXY / $SSXX)
        $B = ($MeanY - $A * $MeanX)
        $RawTrend = -$A # Flip to get daily increase in Used (vs decrease in Remaining)
        $Trend = Format-Trend $RawTrend

        If ($RawTrend -Gt 0) {
            $DaysToFull = Format-Days ($_.SizeRemaining / $RawTrend)
        }
        Else {
            $DaysToFull = "-"
        }
    }
    Else {
        $Trend = "InsufficientHistory"
        $DaysToFull = "-"
    }

    [PsCustomObject]@{
        "Volume"     = $_.FileSystemLabel
        "Size"       = Format-Bytes ($_.Size)
        "Used"       = Format-Bytes ($_.Size - $_.SizeRemaining)
        "Trend"      = $Trend
        "DaysToFull" = $DaysToFull
    }
}

$Output | Format-Table
```

## Esempio 6: Comporta memoria, è possibile eseguire, ma non è possibile nascondere

Perché è la cronologia delle prestazioni raccolti e archiviati in modo centralizzato per l'intero cluster, puoi mai dover unire insieme i dati da computer diversi, indipendentemente da come molte volte le macchine virtuali si spostano tra host. Questo esempio Usa il `VM.Memory.Assigned` serie dal `LastMonth` intervallo di tempo per identificare le macchine virtuali che utilizzano più memoria negli ultimi 35 giorni.

### Screenshot

Nello screenshot seguente, vediamo le macchine virtuali primi 10 dall'utilizzo della memoria ultimo mese:

![Screenshot di PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### Come funziona

Abbiamo ripetere il nostro `Invoke-Command` perché l'operazione, introdotto sopra, a `Get-VM` in ogni server. Usiamo `Measure-Object -Average` per ottenere la media mensile per ogni macchina virtuale, quindi `Sort-Object` seguito da `Select-Object -First 10` per ottenere la classifica. (O magari è il nostro list? *Più voleva* )

### Script

Ecco lo script:

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Bytes {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        Do { if( $RawValue -Gt 1024 ){ $RawValue /= 1024 ; $i++ } } While ( $RawValue -Gt 1024 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }
    
    Get-VM | ForEach-Object {
        $Data = $_ | Get-ClusterPerf -VMSeriesName "VM.Memory.Assigned" -TimeFrame "LastMonth"
        If ($Data) {
            $AvgMemoryUsage = ($Data | Measure-Object -Property Value -Average).Average
            [PsCustomObject]@{
                "VM" = $_.Name
                "AvgMemoryUsage" = Format-Bytes $AvgMemoryUsage.Value
                "RawAvgMemoryUsage" = $AvgMemoryUsage.Value # For sorting...
            }
        }
    }
}

$Output | Sort-Object RawAvgMemoryUsage -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, AvgMemoryUsage
```

Tutto qui. Questi esempi ispirare fortuna puoi e informazioni utili per iniziare. Con spazi di archiviazione diretta cronologia delle prestazioni e la potente, scripting adatto `Get-ClusterPerf` cmdlet, sono abilitate per chiedere – e la risposta! -complesso domande come puoi gestire e monitorare l'infrastruttura di Windows Server 2019.

## Vedi anche

- [Introduzione a Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Cronologia delle prestazioni](performance-history.md)
