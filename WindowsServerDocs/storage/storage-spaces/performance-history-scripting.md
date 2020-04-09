---
title: Creazione di script con Spazi di archiviazione diretta cronologia delle prestazioni
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4c25ed4112035fa729ccf17792a846263ec68dfc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856174"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>Creazione di script con PowerShell e Spazi di archiviazione diretta cronologia delle prestazioni

> Si applica a: Windows Server 2019

In Windows Server 2019, [spazi di archiviazione diretta](storage-spaces-direct-overview.md) registra e archivia una [cronologia delle prestazioni](performance-history.md) completa per le macchine virtuali, i server, le unità, i volumi, le schede di rete e altro ancora. La cronologia delle prestazioni è facile da interrogare ed elaborare in PowerShell in modo da poter passare rapidamente da *dati non elaborati* alle *risposte effettive* alle domande, ad esempio:

1. Ci sono picchi di CPU della settimana scorsa?
2. Il disco fisico presenta una latenza anormale?
3. Quali VM utilizzano attualmente la maggior parte degli IOPS di archiviazione?
4. La larghezza di banda della rete è satura?
5. Quando si esaurisce lo spazio disponibile in questo volume?
6. Nel mese precedente, quali macchine virtuali usavano la maggior parte della memoria?

Il cmdlet `Get-ClusterPerf` viene compilato per lo scripting. Accetta input da cmdlet come `Get-VM` o `Get-PhysicalDisk` dalla pipeline per gestire l'associazione ed è possibile reindirizzare l'output in cmdlet di utilità come `Sort-Object`, `Where-Object`e `Measure-Object` per comporre rapidamente query potenti.

**Questo argomento fornisce e illustra 6 script di esempio che rispondono alle 6 domande precedenti.** Presentano i modelli che è possibile applicare per trovare i picchi, le medie, le linee di tendenza del tracciato, l'esecuzione del rilevamento degli outlier e altro ancora in diversi dati e intervalli di tempo. Vengono fornite come codice di avvio gratuito per la copia, l'estensione e il riutilizzo.

   > [!NOTE]
   > Per brevità, gli script di esempio ometteno elementi come la gestione degli errori che ci si potrebbe aspettare di codice PowerShell di alta qualità. Sono progettate principalmente per l'ispirazione e la formazione anziché per l'uso in produzione.

## <a name="sample-1-cpu-i-see-you"></a>Esempio 1: CPU.

In questo esempio viene utilizzata la serie `ClusterNode.Cpu.Usage` dall'intervallo di tempo `LastWeek` per visualizzare il valore massimo ("limite massimo"), minimo e medio di utilizzo della CPU per ogni server del cluster. Esegue anche un'analisi quartile semplice per mostrare il numero di ore di utilizzo della CPU oltre il 25%, il 50% e il 75% negli ultimi 8 giorni.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente si noterà che il *server-02* presenta un picco non spiegato della scorsa settimana:

![Screenshot di PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>Come funziona

L'output di `Get-ClusterPerf` pipe nel cmdlet `Measure-Object` predefinito, è sufficiente specificare la proprietà `Value`. Con i flag `-Maximum`, `-Minimum`e `-Average`, `Measure-Object` ci fornisce le prime tre colonne quasi gratis. Per eseguire l'analisi dei quartili, è possibile inviare tramite pipe a `Where-Object` e contare il numero di valori `-Gt` (maggiore di) 25, 50 o 75. L'ultimo passaggio consiste nell'abbellimento con `Format-Hours` e `Format-Percent` funzioni helper, certamente facoltative.

### <a name="script"></a>Script

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

## <a name="sample-2-fire-fire-latency-outlier"></a>Esempio 2: Fire, Fire, latence outlier

Questo esempio usa la serie `PhysicalDisk.Latency.Average` dall'intervallo di tempo `LastHour` per cercare gli outlier statistici, definiti come unità con una latenza media oraria che supera +3 σ (tre deviazioni standard) oltre la media della popolazione.

   > [!IMPORTANT]
   > Per brevità, questo script non implementa misure di sicurezza contro la varianza bassa, non gestisce i dati parziali mancanti, non distingue dal modello o dal firmware e così via. Esercitare una valutazione appropriata e non affidarsi solo a questo script per determinare se sostituire un disco rigido. Viene presentato qui solo a scopo didattico.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente si noterà che non sono presenti outlier:

![Screenshot di PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>Come funziona

In primo luogo, vengono escluse le unità inattive o quasi inattive controllando che `PhysicalDisk.Iops.Total` sia `-Gt 1`costantemente. Per ogni HDD attivo, viene inviato tramite pipe il relativo intervallo di tempo `LastHour`, costituito da 360 misure a intervalli di 10 secondi, per `Measure-Object -Average` per ottenere la latenza media nell'ultima ora. Questa operazione consente di configurare la popolazione.

Si implementa la [formula diffusa](http://www.mathsisfun.com/data/standard-deviation.html) per trovare la media `μ` e la deviazione standard `σ` della popolazione. Per ogni HDD attivo, viene confrontata la latenza media con la media della popolazione e la divisione in base alla deviazione standard. Si mantengono i valori non elaborati, quindi è possibile `Sort-Object` i risultati, ma si usano `Format-Latency` e `Format-StandardDeviation` funzioni helper per abbellire quello che verrà visualizzato, certamente facoltativo.

Se un'unità è maggiore di +3 σ, `Write-Host` in rosso; in caso contrario, in verde.

### <a name="script"></a>Script

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

## <a name="sample-3-noisy-neighbor-thats-write"></a>Esempio 3: Neighbor fastidioso? Questa è la scrittura.

La cronologia delle prestazioni può rispondere anche a domande su *questo momento*. Le nuove misurazioni sono disponibili in tempo reale, ogni 10 secondi. Questo esempio usa la serie `VHD.Iops.Total` dall'intervallo di tempo `MostRecent` per identificare l'attività più occupata (alcune potrebbero avere macchine virtuali "più rumorose") che utilizzano la maggior parte delle operazioni di i/o al secondo in ogni host del cluster e mostrano la suddivisione in lettura/scrittura della relativa attività.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente vengono visualizzate le prime 10 macchine virtuali per attività di archiviazione:

![Screenshot di PowerShell](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>Come funziona

A differenza di `Get-PhysicalDisk`, il cmdlet `Get-VM` non è compatibile con i cluster: restituisce solo le macchine virtuali nel server locale. Per eseguire una query da ogni server in parallelo, viene eseguito il wrapping della chiamata in `Invoke-Command (Get-ClusterNode).Name { ... }`. Per ogni macchina virtuale vengono ottenute le misurazioni `VHD.Iops.Total`, `VHD.Iops.Read`e `VHD.Iops.Write`. Se non si specifica il parametro `-TimeFrame`, viene ottenuto il `MostRecent` singolo punto dati per ogni.

   > [!TIP]
   > Queste serie riflettono la somma dell'attività della macchina virtuale a tutti i relativi file VHD/VHDX. Questo è un esempio in cui la cronologia delle prestazioni viene aggregata automaticamente. Per ottenere la ripartizione per VHD/VHDX, è possibile inviare tramite pipe un singolo `Get-VHD` in `Get-ClusterPerf` invece che nella macchina virtuale.

I risultati di ogni server sono combinati `$Output`, che è possibile `Sort-Object` e quindi `Select-Object -First 10`. Si noti che `Invoke-Command` decora i risultati con una proprietà `PsComputerName` che indica da dove provengono, che è possibile stampare per individuare il punto in cui la macchina virtuale è in esecuzione.

### <a name="script"></a>Script

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

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>Esempio 4: come dicono, "25-Gig è il nuovo 10 Gig"

In questo esempio viene utilizzata la serie `NetAdapter.Bandwidth.Total` dall'intervallo di tempo `LastDay` per individuare i segnali della saturazione della rete, definiti come > 90% della larghezza di banda massima teorica. Per ogni scheda di rete nel cluster, viene confrontato l'utilizzo della larghezza di banda osservato più elevato nell'ultimo giorno fino alla velocità di collegamento indicata.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente si noterà che un #2 di *Fabrikam NX-4 Pro* ha raggiunto il picco nell'ultimo giorno:

![Screenshot di PowerShell](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>Come funziona

Si ripete il `Invoke-Command` trick da sopra per `Get-NetAdapter` in ogni server e pipe in `Get-ClusterPerf`. Lungo il percorso vengono prese due proprietà rilevanti: la stringa `LinkSpeed` come "10 Gbps" e il relativo `Speed` integer non elaborato, ad esempio 10 miliardi. Si usa `Measure-Object` per ottenere la media e il picco dell'ultimo giorno (promemoria: ogni misura nell'intervallo di tempo `LastDay` rappresenta 5 minuti) e moltiplicata per 8 bit per byte per ottenere un confronto tra mele e mele.

   > [!NOTE]
   > Alcuni fornitori, ad esempio Chelsio, includono attività di accesso diretto a memoria remota (RDMA) nei contatori delle prestazioni della *scheda di rete* , quindi sono inclusi nella serie `NetAdapter.Bandwidth.Total`. Altri, ad esempio Mellanox, non possono. Se il fornitore non lo è, è sufficiente aggiungere la serie `NetAdapter.Bandwidth.RDMA.Total` nella versione di questo script.

### <a name="script"></a>Script

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

## <a name="sample-5-make-storage-trendy-again"></a>Esempio 5: rendere di nuovo l'archivio di tendenza.

Per esaminare le tendenze delle macro, la cronologia delle prestazioni viene mantenuta per un massimo di 1 anno. In questo esempio viene utilizzata la serie `Volume.Size.Available` dall'intervallo di tempo `LastYear` per determinare la velocità di riempimento dello spazio di archiviazione e la stima del momento in cui sarà piena.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente viene visualizzato il volume di *backup* che aggiunge circa 15 GB al giorno:

![Screenshot di PowerShell](media/performance-history/Show-StorageTrend.png)

A questa velocità, raggiungerà la capacità in altri 42 giorni.

### <a name="how-it-works"></a>Come funziona

L'intervallo di tempo `LastYear` dispone di un punto dati al giorno. Sebbene siano necessari solo due punti per adattarsi a una linea di tendenza, in pratica è preferibile richiederne altri, ad esempio 14 giorni. Si usa `Select-Object -Last 14` per configurare una matrice di punti *(x, y)* , per *x* nell'intervallo [1, 14]. Con questi punti, viene implementato l' [algoritmo lineare minimo lineare](http://mathworld.wolfram.com/LeastSquaresFitting.html) per trovare `$A` e `$B` che parametrizzano la linea più adatta *y = ax + b*. Benvenuti ad High School.

La divisione della proprietà `SizeRemaining` del volume in base alla tendenza (la pendenza `$A`) consente di stimare in modo grossolano il numero di giorni, alla frequenza attuale di crescita dell'archiviazione, fino a quando il volume non è pieno. Le funzioni di supporto `Format-Bytes`, `Format-Trend`e `Format-Days` abbelliscono l'output.

   > [!IMPORTANT]
   > Questa stima è lineare e si basa solo sulle 14 misure giornaliere più recenti. Sono disponibili tecniche più sofisticate e accurate. Esercitare una valutazione efficace e non affidarsi solo a questo script per determinare se investire nell'espansione dell'archiviazione. Viene presentato qui solo a scopo didattico.

### <a name="script"></a>Script

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

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>Esempio 6: memoria porco, è possibile eseguire, ma non è possibile nascondere

Poiché la cronologia delle prestazioni viene raccolta e archiviata centralmente per l'intero cluster, non è mai necessario unire i dati da computer diversi, indipendentemente dal numero di volte in cui le macchine virtuali si spostano tra gli host. In questo esempio viene utilizzata la serie `VM.Memory.Assigned` dall'intervallo di tempo `LastMonth` per identificare le macchine virtuali che utilizzano la maggior quantità di memoria negli ultimi 35 giorni.

### <a name="screenshot"></a>Screenshot

Lo screenshot seguente mostra le prime 10 macchine virtuali in base all'utilizzo della memoria del mese scorso:

![Screenshot di PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>Come funziona

Si ripete il `Invoke-Command` Trick, introdotto in precedenza, per `Get-VM` in ogni server. Si usa `Measure-Object -Average` per ottenere la media mensile per ogni macchina virtuale, quindi `Sort-Object` seguito da `Select-Object -First 10` per ottenere la classifica. O forse è l'elenco *più desiderato* ?

### <a name="script"></a>Script

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

Ecco fatto! Speriamo che questi esempi ispirino l'utente e contribuiscano a iniziare. Grazie alla cronologia delle prestazioni Spazi di archiviazione diretta e al potente cmdlet `Get-ClusterPerf` di scripting, avrai la possibilità di richiedere e rispondere. -domande complesse durante la gestione e il monitoraggio dell'infrastruttura di Windows Server 2019.

## <a name="see-also"></a>Vedere anche

- [Guida introduttiva a Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Cronologia delle prestazioni](performance-history.md)
