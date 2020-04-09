---
title: Cronologia prestazioni per Spazi di archiviazione diretta
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: ab9b6016d49725b7f25d2ad3c40bd6265ac811a9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856154"
---
# <a name="performance-history-for-storage-spaces-direct"></a>Cronologia prestazioni per Spazi di archiviazione diretta

> Si applica a: Windows Server 2019

La cronologia delle prestazioni è una nuova funzionalità che consente agli amministratori di [spazi di archiviazione diretta](storage-spaces-direct-overview.md) di accedere agevolmente alle misurazioni di calcolo, memoria, rete e archiviazione cronologiche tra server host, unità, volumi, macchine virtuali e altro ancora. La cronologia delle prestazioni viene raccolta automaticamente e archiviata nel cluster per un massimo di un anno.

   > [!IMPORTANT]
   > Questa funzionalità è una novità di Windows Server 2019. Non è disponibile in Windows Server 2016.

## <a name="get-started"></a>Attività iniziali

Per impostazione predefinita, la cronologia delle prestazioni viene raccolta con Spazi di archiviazione diretta in Windows Server 2019. Non è necessario installare, configurare o avviare alcun elemento. Non è necessaria una connessione Internet, System Center non è necessario e non è necessario un database esterno.

Per visualizzare graficamente la cronologia delle prestazioni del cluster, usare l'interfaccia di [amministrazione di Windows](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Cronologia delle prestazioni nell'interfaccia di amministrazione di Windows](media/performance-history/perf-history-in-wac.png)

Per eseguire una query ed elaborarla a livello di codice, usare il cmdlet New `Get-ClusterPerf`. Vedere [utilizzo in PowerShell](#usage-in-powershell).

## <a name="whats-collected"></a>Elementi raccolti

La cronologia delle prestazioni viene raccolta per 7 tipi di oggetti:

![Tipi di oggetti](media/performance-history/types-of-object.png)

Ogni tipo di oggetto ha molte serie, ad esempio `ClusterNode.Cpu.Usage` viene raccolto per ogni server.

Per informazioni dettagliate sugli elementi raccolti per ogni tipo di oggetto e su come interpretarli, vedere questi argomenti secondari:

| Oggetto             | Serie                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Unità             | [Elementi raccolti per le unità](performance-history-for-drives.md)                     |
| Schede di rete   | [Elementi raccolti per le schede di rete](performance-history-for-network-adapters.md) |
| Server            | [Elementi raccolti per i server](performance-history-for-servers.md)                   |
| Dischi rigidi virtuali | [Elementi raccolti per i dischi rigidi virtuali](performance-history-for-vhds.md)           |
| Macchine virtuali   | [Elementi raccolti per le macchine virtuali](performance-history-for-vms.md)              |
| Volumi            | [Elementi raccolti per i volumi](performance-history-for-volumes.md)                   |
| Cluster           | [Elementi raccolti per i cluster](performance-history-for-clusters.md)                 |

Molte serie vengono aggregate tra gli oggetti peer e il relativo elemento padre, ad esempio `NetAdapter.Bandwidth.Inbound` viene raccolta separatamente per ogni scheda di rete e aggregata al server globale; Analogamente `ClusterNode.Cpu.Usage` viene aggregato al cluster complessivo; E così via.

## <a name="timeframes"></a>Tempi

La cronologia delle prestazioni viene archiviata per un massimo di un anno, con una granularità ridotta. Per l'ora più recente, le misurazioni sono disponibili ogni dieci secondi. Successivamente, vengono uniti in modo intelligente (calcolando la media o sommando, a seconda dei casi) in serie meno granulari che si estendono più tempo. Per il giorno più recente, le misurazioni sono disponibili ogni cinque minuti; per la settimana più recente, ogni quindici minuti; E così via.

Nell'interfaccia di amministrazione di Windows è possibile selezionare l'intervallo di tempo in alto a destra sopra il grafico.

![Intervalli di tempo nell'interfaccia di amministrazione di Windows](media/performance-history/timeframes-in-honolulu.png)

In PowerShell usare il parametro `-TimeFrame`.

Di seguito sono riportati gli intervalli di tempo disponibili:

| Periodo   | Frequenza di misurazione | Conservati per |
|-------------|-----------------------|--------------|
| `LastHour`  | Ogni 10 secondi         | 1 ora       |
| `LastDay`   | Ogni 5 minuti       | 25 ore     |
| `LastWeek`  | Ogni 15 minuti      | 8 giorni       |
| `LastMonth` | Ogni ora          | 35 giorni      |
| `LastYear`  | Ogni 1 giorno           | 400 giorni     |

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare il cmdlet `Get-ClusterPerformanceHistory` per eseguire query ed elaborare la cronologia delle prestazioni in PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Usare l'alias **Get-ClusterPerf** per salvare alcune sequenze di tasti.

### <a name="example"></a>Esempio

Ottenere l'utilizzo della CPU della macchina virtuale *MyVM* nell'ultima ora:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Per esempi più avanzati, vedere gli [script di esempio](performance-history-scripting.md) pubblicati che forniscono il codice di avvio per trovare i valori di picco, calcolare le medie, tracciare le linee di tendenza, eseguire il rilevamento degli outlier e altro ancora.

### <a name="specify-the-object"></a>Specificare l'oggetto

È possibile specificare l'oggetto desiderato dalla pipeline. Funziona con 7 tipi di oggetti:

| Oggetto dalla pipeline | Esempio     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Se non si specifica, viene restituita la cronologia delle prestazioni per il cluster complessivo.

### <a name="specify-the-series"></a>Specificare la serie

È possibile specificare la serie desiderata con questi parametri:


| Parametro                 | Esempio                       | Elenco                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [Elementi raccolti per le unità](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [Elementi raccolti per le schede di rete](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [Elementi raccolti per i server](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [Elementi raccolti per i dischi rigidi virtuali](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [Elementi raccolti per le macchine virtuali](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [Elementi raccolti per i volumi](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [Elementi raccolti per i cluster](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Usare il completamento tramite tasto TAB per individuare le serie disponibili.

Se non si specifica, vengono restituite tutte le serie disponibili per l'oggetto specificato.

### <a name="specify-the-timeframe"></a>Specificare l'intervallo di tempo

È possibile specificare l'intervallo di tempo della cronologia desiderato con il parametro `-TimeFrame`.

   > [!TIP]
   > Usare il completamento tramite tasto TAB per individuare gli intervalli di tempo disponibili.

Se non si specifica, viene restituita la misura `MostRecent`.

## <a name="how-it-works"></a>Come funziona

### <a name="performance-history-storage"></a>Archiviazione cronologia prestazioni

Subito dopo l'abilitazione di Spazi di archiviazione diretta, viene creato un volume di circa 10 GB denominato `ClusterPerformanceHistory` e viene effettuato il provisioning di un'istanza del motore di archiviazione estendibile (noto anche come Microsoft JET). Questo database semplifica la memorizzazione della cronologia delle prestazioni senza alcuna gestione o coinvolgimento dell'amministratore.

![Volume per archiviazione cronologia prestazioni](media/performance-history/perf-history-volume.png)

Il volume è supportato da spazi di archiviazione e usa il mirroring semplice, a due vie o la resilienza del mirror a tre vie, a seconda del numero di nodi nel cluster. Viene ripristinato in seguito a errori dell'unità o del server come qualsiasi altro volume in Spazi di archiviazione diretta.

Il volume USA ReFS ma non è Volume condiviso cluster (CSV), quindi viene visualizzato solo nel nodo proprietario gruppo cluster. Oltre alla creazione automatica, non c'è niente di speciale per questo volume: è possibile visualizzarlo, sfogliarlo, ridimensionarlo o eliminarlo (scelta non consigliata). Se si verifica un problema, vedere [risoluzione dei problemi](#troubleshooting).

### <a name="object-discovery-and-data-collection"></a>Individuazione oggetti e raccolta dati

Cronologia prestazioni individua automaticamente gli oggetti rilevanti, ad esempio le macchine virtuali, in qualsiasi punto del cluster e inizia a trasmettere i contatori delle prestazioni. I contatori sono aggregati, sincronizzati e inseriti nel database. Il flusso viene eseguito in modo continuo ed è ottimizzato per un effetto minimo sul sistema.

La raccolta viene gestita dal Servizio integrità, che è a disponibilità elevata: se il nodo in cui è in esecuzione diventa inattivo, riprenderà i momenti successivi in un altro nodo del cluster. La cronologia delle prestazioni può scadere brevemente, ma verrà ripresa automaticamente. È possibile visualizzare il Servizio integrità e il relativo nodo proprietario eseguendo `Get-ClusterResource Health` in PowerShell.

### <a name="handling-measurement-gaps"></a>Gestione dei gap di misurazione

Quando le misurazioni sono unite in serie meno granulari che si estendono più tempo, come descritto in [intervalli](#timeframes)di tempo, i periodi di dati mancanti vengono esclusi. Se, ad esempio, il server è inattivo per 30 minuti e quindi viene eseguito alla CPU del 50% per i successivi 30 minuti, la media `ClusterNode.Cpu.Usage` per l'ora verrà registrata correttamente come 50% (non 25%).

### <a name="extensibility-and-customization"></a>Estendibilità e personalizzazione

La cronologia delle prestazioni è intuitiva per gli script. Usare PowerShell per eseguire il pull di qualsiasi cronologia disponibile direttamente dal database per creare report automatizzati o avvisi, esportare la cronologia per la sicurezza, eseguire il Rolling delle visualizzazioni e così via. Per un codice di avvio utile, vedere gli [script di esempio](performance-history-scripting.md) pubblicati.

Non è possibile raccogliere la cronologia per oggetti, intervalli di tempo o serie aggiuntivi.

La frequenza di misurazione e il periodo di memorizzazione non sono attualmente configurabili.

## <a name="start-or-stop-performance-history"></a>Avviare o arrestare la cronologia delle prestazioni

### <a name="how-do-i-enable-this-feature"></a>Ricerca per categorie abilitare questa funzionalità?

A meno che non si `Stop-ClusterPerformanceHistory`, la cronologia delle prestazioni è abilitata per impostazione predefinita.

Per riabilitarla, eseguire questo cmdlet di PowerShell come amministratore:

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>Ricerca per categorie disabilitare questa funzionalità?

Per interrompere la raccolta della cronologia delle prestazioni, eseguire questo cmdlet di PowerShell come amministratore:

```PowerShell
Stop-ClusterPerformanceHistory
```

Per eliminare le misurazioni esistenti, usare il flag `-DeleteHistory`:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Durante la distribuzione iniziale, è possibile impedire l'avvio della cronologia delle prestazioni impostando il parametro `-CollectPerformanceHistory` di `Enable-ClusterStorageSpacesDirect` su `$False`.

## <a name="troubleshooting"></a>Risoluzione dei problemi

### <a name="the-cmdlet-doesnt-work"></a>Il cmdlet non funziona

Un messaggio di errore come il*termine ' Get-ClusterPerf ' non è riconosciuto come nome di un cmdlet*"significa che la funzionalità non è disponibile o installata. Verificare di disporre di Windows Server Insider Preview Build 17692 o versione successiva, che sia stato installato il clustering di failover e che sia in esecuzione Spazi di archiviazione diretta.

   > [!NOTE]
   > Questa funzionalità non è disponibile in Windows Server 2016 o versioni precedenti.

### <a name="no-data-available"></a>Nessun dato disponibile 

Se un grafico mostra "*Nessun dato disponibile*" come illustrato, di seguito viene illustrato come risolvere i problemi:

![Nessun dato disponibile](media/performance-history/no-data-available.png)

1. Se l'oggetto è stato appena aggiunto o creato, attenderne l'individuazione (fino a 15 minuti).

2. Aggiornare la pagina o attendere il successivo aggiornamento in background (fino a 30 secondi).

3. Alcuni oggetti speciali vengono esclusi dalla cronologia delle prestazioni, ad esempio macchine virtuali non in cluster e volumi che non utilizzano il file System Volume condiviso cluster (CSV). Controllare l'argomento secondario per il tipo di oggetto, come la [cronologia delle prestazioni per i volumi](performance-history-for-volumes.md), per la stampa.

4. Se il problema persiste, aprire PowerShell come amministratore ed eseguire il cmdlet `Get-ClusterPerf`. Il cmdlet include la logica di risoluzione dei problemi per identificare i problemi comuni, ad esempio se il volume ClusterPerformanceHistory è mancante e fornisce istruzioni per la correzione.

5. Se il comando nel passaggio precedente non restituisce nulla, è possibile provare a riavviare il Servizio integrità (che raccoglie la cronologia delle prestazioni) eseguendo `Stop-ClusterResource Health ; Start-ClusterResource Health` in PowerShell.

## <a name="see-also"></a>Vedere anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
