---
title: Creazione di script con cronologia delle prestazioni di spazi di archiviazione diretta
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816322"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>Creazione di script con PowerShell e spazi di archiviazione diretta della cronologia delle prestazioni

> Si applica a: Build di Windows Server Insider Preview 17692 e versioni successive

In Windows Server 2019, [spazi di archiviazione diretta](storage-spaces-direct-overview.md) record e archivi estesi [Cronologia prestazioni](performance-history.md) per le macchine virtuali, i server, le unità, i volumi, schede di rete e altro ancora. Cronologia delle prestazioni è semplice eseguire query e processo in PowerShell in modo che è possibile passare rapidamente da *dati non elaborati* al *le risposte effettive* a domande come:

1. Sono stati fatti eventuali picchi di CPU la settimana scorsa?
2. Qualsiasi disco fisico presenta Latenza anomala?
3. Le VM che utilizzano il massimo numero di IOPS dell'archiviazione subito?
4. La larghezza di banda di rete è satura?
5. Quando questo volume verrà eseguito all'esterno di spazio disponibile?
6. Nell'ultimo mese, quali le macchine virtuali usate la maggior quantità di memoria?

Il `Get-ClusterPerf` cmdlet è progettato per la creazione di script. Accetta l'input dal cmdlet come `Get-VM` oppure `Get-PhysicalDisk` tramite la pipeline per gestire l'associazione ed è possibile inviare tramite pipe l'output ai cmdlet di utilità, ad esempio `Sort-Object`, `Where-Object`, e `Measure-Object` rapidamente comporre query avanzate.

**In questo argomento contiene e illustra gli script di esempio 6 che rispondono alle 6 domande precedente.** Presentano i modelli che possono essere applicate per trovare i picchi, trovare le medie, tracciare linee di tendenza, eseguire degli outlier, rilevamento e altro ancora, per un'ampia gamma di dati e gli intervalli di tempo. Vengono fornite come codice starter gratuito per poter copiare, estendere e riutilizzare.

   > [!NOTE]
   > Per brevità, gli script di esempio omettono elementi quali la gestione degli errori che si prevede di codice PowerShell di alta qualità. Sono destinati principalmente ispirazione ed education invece di produzione usare.

## <a name="sample-1-cpu-i-see-you"></a>Esempio 1: CPU, viene visualizzato è!

Questo esempio Usa la `ClusterNode.Cpu.Usage` serie provenienti dal `LastWeek` intervallo di tempo per mostrare il valore massimo ("livello più alto"), minima e Media della CPU per tutti i server del cluster. Questo metodo esegue analisi quartile semplice per illustrare l'utilizzo di quante ore della CPU è superiore a 25%, 50% e il 75% negli ultimi 8 giorni.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente, si nota che *Server-02* aveva un picco inspiegabile ultima settimana:

![Screenshot di PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>Come funziona

L'output dalla `Get-ClusterPerf` pipe perfettamente in integrato `Measure-Object` cmdlet, è sufficiente specificare il `Value` proprietà. Con relativo `-Maximum`, `-Minimum`, e `-Average` flags, `Measure-Object` ci offre le prime tre colonne quasi per gratuitamente. Per eseguire l'analisi quartile, è possibile inviare tramite pipe a `Where-Object` e il conteggio quanti valori sono stati `-Gt` (maggiore) 25, 50 o 75. L'ultimo passaggio consiste nel beautify con `Format-Hours` e `Format-Percent` funzioni helper – certamente facoltativo.

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

## <a name="sample-2-fire-fire-latency-outlier"></a>Esempio 2: Incendio, incendi, outlier di latenza

Questo esempio Usa la `PhysicalDisk.Latency.Average` serie provenienti dal `LastHour` intervallo di tempo per individuare gli outlier statistici, definita come unità con una latenza media oraria supera + 3σ (tre deviazioni standard) sopra la media di popolazione.

   > [!IMPORTANT]
   > Per brevità, questo script non implementa misure di protezione contro varianza bassa, non consente di gestire i dati mancanti parziali, non distingue dal modello o del firmware e così via. Prestare uso del buon senso e non fare affidamento su questo script da solo per determinare se si desidera sostituire un disco rigido. Viene presentato qui solo a scopo didattico.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente, viene visualizzato che alcun outlier non sono:

![Screenshot di PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>Come funziona

In primo luogo, Microsoft esclude unità quasi inattiva o meno inattiva controllando che `PhysicalDisk.Iops.Total` è costantemente `-Gt 1`. Per ogni unità disco rigido attivo, si invia tramite pipe relativi `LastHour` intervallo di tempo, costituito da 360 misure a intervalli di 10 secondi, a `Measure-Object -Average` per ottenere la latenza media nell'ultima ora. In questo modo il popolamento.

Implementiamo le [formula ampiamente noto](http://www.mathsisfun.com/data/standard-deviation.html) per trovare il valore medio `μ` e la deviazione standard `σ` della popolazione. Per ogni unità disco rigido attivo, si confronta la latenza media per la media di popolazione e dividere la deviazione standard. È possibile mantenere i valori non elaborati, digitiamo `Sort-Object` i risultati, ma usare `Format-Latency` e `Format-StandardDeviation` le funzioni helper per beautify cosa vi mostreremo – certamente facoltativi.

Se un'unità è più + 3σ, abbiamo `Write-Host` in rosso; se non vengono installati, in verde.

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

## <a name="sample-3-noisy-neighbor-thats-write"></a>Esempio 3: Noisy neighbor? È stata scrittura completata.

Cronologia delle prestazioni può rispondere a domande sulle *subito*anche. Le nuove misure sono disponibili in tempo reale, ogni 10 secondi. Questo esempio Usa il `VHD.Iops.Total` serie provenienti dal `MostRecent` intervallo di tempo per identificare il più occupato (alcuni potrebbero ad esempio "maggiore impegno") delle macchine virtuali utilizzano più risorse di archiviazione IOPS, in ogni host in cluster e Mostra la suddivisione di lettura/scrittura del loro attività.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente, sono visualizzate le macchine virtuali di Top 10 dall'attività di archiviazione:

![Screenshot di PowerShell](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>Come funziona

A differenza `Get-PhysicalDisk`, il `Get-VM` cmdlet non è compatibile con cluster e che restituisce solo le macchine virtuali nel server locale. Per eseguire query da ogni server in parallelo, si eseguono il wrapping la chiamata in `Invoke-Command (Get-ClusterNode).Name { ... }`. Per tutte le macchine Virtuali, otteniamo il `VHD.Iops.Total`, `VHD.Iops.Read`, e `VHD.Iops.Write` misurazioni. Se non viene specificata la `-TimeFrame` parametro, si ottiene il `MostRecent` singolo punto dati per ognuno.

   > [!TIP]
   > Queste serie riflettano la somma dell'attività della macchina virtuale su tutti i relativi file VHD/VHDX. Questo è un esempio in cui la cronologia delle prestazioni viene automaticamente aggregata per noi. Per ottenere i dettagli per ogni file VHD/VHDX, è possibile inviare tramite pipe un individuo `Get-VHD` in `Get-ClusterPerf` invece la macchina virtuale.

I risultati di tutti i server si riuniscono come `$Output`, che è possibile `Sort-Object` e quindi `Select-Object -First 10`. Si noti che `Invoke-Command` decora risultati con un `PsComputerName` proprietà utilizzata per indicare loro provenienza, che possiamo utilizzare sapere dove è in esecuzione la macchina virtuale.

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

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>Esempio 4: Come si dice, "25 gig il è la nuova 10-gig"

Questo esempio Usa la `NetAdapter.Bandwidth.Total` serie provenienti dal `LastDay` intervallo di tempo a cercare dei segnali della saturazione della rete, definita come > 90% della larghezza di banda massima teorica. Per ogni scheda di rete del cluster, confronta l'utilizzo della larghezza di banda osservati più elevato nell'ultimo giorno a raggiungere la velocità di collegamento indicato.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente, noteremo che uno *NX Fabrikam-4 Pro n. 2* ha avuto un picco nell'ultimo giorno:

![Screenshot di PowerShell](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>Come funziona

Si ripete nostri `Invoke-Command` trucco in precedenza per `Get-NetAdapter` in ogni server e pipe in `Get-ClusterPerf`. Lungo il percorso, si ottiene due proprietà importanti: relativi `LinkSpeed` stringa, ad esempio "10 Gbps" e relativa raw `Speed` integer, ad esempio 10000000000. Usiamo `Measure-Object` per ottenere la media e massima del giorno precedente (promemoria: ogni misura nel `LastDay` intervallo di tempo rappresenta 5 minuti) e moltiplicare per 8 bit per byte per ottenere un confronto di mele con mele.

   > [!NOTE]
   > Alcuni fornitori, ad esempio Chelsio, includono attività di diretto a memoria remota (RDMA) di accesso nella loro *scheda di rete* contatori delle prestazioni, in modo che è incluso nel `NetAdapter.Bandwidth.Total` serie. Altri, ad esempio Mellanox, non potranno essere. Se il fornitore non, aggiunta semplicemente il `NetAdapter.Bandwidth.RDMA.Total` serie nella versione di questo script.

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

## <a name="sample-5-make-storage-trendy-again"></a>Esempio 5: Rendere nuovamente tendenza archiviazione!

Per esaminare le tendenze di macro, viene mantenuta la cronologia delle prestazioni per fino a 1 anno. Questo esempio Usa la `Volume.Size.Available` serie provenienti dal `LastYear` intervallo di tempo per determinare la frequenza con cui memoria è esaurita e stima quando sarà completa.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente, noteremo la *Backup* volume aggiunge circa 15 GB al giorno:

![Screenshot di PowerShell](media/performance-history/Show-StorageTrend.png)

In questo caso, raggiunge la capacità massima in un altro 42 giorni.

### <a name="how-it-works"></a>Come funziona

Il `LastYear` intervallo di tempo è un punto dati al giorno. Anche se è necessario solo rigorosamente due punti per adattare una linea di tendenza, in pratica è preferibile richiedere altre informazioni, ad esempio 14 giorni. Usiamo `Select-Object -Last 14` per configurare una matrice di *(x, y)* punti, per *x* compreso nell'intervallo [1, 14]. Con questi punti, Implementiamo il semplice [algoritmo lineare dei minimi quadrati](http://mathworld.wolfram.com/LeastSquaresFitting.html) trovare `$A` e `$B` che parametrizzare la linea più adatta *y = ax + b*. Benvenuti in liceo tutto il mondo nuovamente.

Divisione del volume `SizeRemaining` proprietà in base alla tendenza (l'inclinazione `$A`) consente di stimare controlla il numero di giorni, all'aliquota corrente di aumento delle dimensioni di archiviazione, fino a quando il volume è pieno. Il `Format-Bytes`, `Format-Trend`, e `Format-Days` funzioni helper beautify l'output.

   > [!IMPORTANT]
   > Questa stima è lineare e si basa solo sulle 14 misurazioni giornaliere più recente. Sono disponibili più tecniche sofisticate e accurate. Prestare uso del buon senso e non fare affidamento su questo script da solo per determinare se investire nell'espansione dell'archiviazione. Viene presentato qui solo a scopo didattico.

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

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>Esempio 6: Comporta di memoria, è possibile eseguire ma non è possibile nascondere

Perché è la cronologia delle prestazioni raccolti e archiviati in modo centralizzato per l'intero cluster, si è mai necessario unire dati da più macchine, indipendentemente da come numero di volte le macchine virtuali spostano tra gli host. Questo esempio Usa la `VM.Memory.Assigned` serie provenienti dal `LastMonth` intervallo di tempo per identificare le macchine virtuali che utilizzano più memoria negli ultimi 35 giorni.

### <a name="screenshot"></a>Screenshot

Nella schermata seguente, viene visualizzato Top 10 macchine virtuali dall'utilizzo della memoria ultimo mese:

![Screenshot di PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>Come funziona

Si ripete nostri `Invoke-Command` trucco, introdotta in precedenza, a `Get-VM` in ogni server. Usiamo `Measure-Object -Average` per ottenere la media mensile per ogni macchina virtuale, quindi `Sort-Object` seguita da `Select-Object -First 10` per ottenere la classifica. (O forse è il nostro *voleva più* elenco?)

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

La procedura è terminata. Questi esempi si spera inspire si e iniziare subito. Cronologia delle prestazioni di spazi di archiviazione diretta e il potente, di scripting orientato `Get-ClusterPerf` cmdlet, puoi porre – e rispondere. – complesso domande durante la gestione e monitoraggio dell'infrastruttura di Windows Server 2019.

## <a name="see-also"></a>Vedere anche

- [Introduzione a Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)
- [Cronologia delle prestazioni](performance-history.md)
