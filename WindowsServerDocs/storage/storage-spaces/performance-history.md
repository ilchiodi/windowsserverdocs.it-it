---
title: Cronologia delle prestazioni per spazi di archiviazione diretta
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870862"
---
# <a name="performance-history-for-storage-spaces-direct"></a>Cronologia delle prestazioni per spazi di archiviazione diretta

> Si applica a: Windows Server 2019

Cronologia delle prestazioni è una nuova funzionalità che offra [spazi di archiviazione diretta](storage-spaces-direct-overview.md) accedere facilmente agli amministratori a cronologiche misurazioni di calcolo, memoria, rete e archiviazione in server host, le unità, volumi, macchine virtuali e altro ancora. Cronologia delle prestazioni verrà raccolti automaticamente e archiviata nel cluster per un massimo di un anno.

   > [!IMPORTANT]
   > Questa funzionalità è stata introdotta in Windows Server 2019. Non è disponibile in Windows Server 2016.

## <a name="get-started"></a>Informazioni di base

Cronologia delle prestazioni verrà raccolti per impostazione predefinita con spazi di archiviazione diretta in Windows Server 2019. È necessario non installare, configurare o avviare alcuna operazione. Non è necessaria una connessione Internet, System Center non è obbligatorio e un database esterno non è necessario.

Per visualizzare graficamente la cronologia delle prestazioni del cluster, usare [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Cronologia delle prestazioni in Windows Admin Center](media/performance-history/perf-history-in-wac.png)

Per eseguire query ed elaborarli a livello di codice, usare il nuovo `Get-ClusterPerf` cmdlet. Visualizzare [utilizzo di PowerShell](#usage-in-powershell).

## <a name="whats-collected"></a>Inventario raccolto

Cronologia delle prestazioni verrà raccolti per 7 tipi di oggetti:

![Tipi di oggetti](media/performance-history/types-of-object.png)

Ogni tipo di oggetto dispone di molte serie: ad esempio, `ClusterNode.Cpu.Usage` viene raccolto per ogni server.

Per informazioni dettagliate su ciò che vengono raccolti per ogni tipo di oggetto e su come interpretarle, vedere questi argomenti secondari:

| Object             | serie                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Unità             | [Inventario raccolto per le unità](performance-history-for-drives.md)                     |
| Schede di rete   | [Inventario raccolto per le schede di rete](performance-history-for-network-adapters.md) |
| Server            | [Informazioni raccolte per i server](performance-history-for-servers.md)                   |
| Dischi rigidi virtuali | [Informazioni raccolte per i dischi rigidi virtuali](performance-history-for-vhds.md)           |
| Macchine virtuali   | [Inventario raccolto per le macchine virtuali](performance-history-for-vms.md)              |
| Volumi            | [Informazioni raccolte per i volumi](performance-history-for-volumes.md)                   |
| Cluster           | [Informazioni raccolte per i cluster](performance-history-for-clusters.md)                 |

Numero di serie vengono aggregati tra gli oggetti peer per l'elemento padre: ad esempio, `NetAdapter.Bandwidth.Inbound` vengono raccolti per ogni scheda di rete separatamente e aggregati al server, complessivo allo stesso modo `ClusterNode.Cpu.Usage` vengono aggregati ai cluster nel suo complesso; e così via.

## <a name="timeframes"></a>Intervalli di tempo

Cronologia delle prestazioni verrà archiviata per un massimo di un anno, con una granularità di riduzione. Per l'ora più recente, le misure sono disponibili ogni dieci secondi. Successivamente, vengono unite in modo intelligente (per il calcolo della media o la somma, come appropriato) in serie meno granulari che si estendono su più tempo. Per il giorno più recente, le misure sono disponibili ogni cinque minuti; per la settimana più recente, ogni quindici minuti; E così via.

In Windows Admin Center, è possibile selezionare l'intervallo di tempo nell'angolo superiore destro sopra il grafico.

![Intervalli di tempo in Windows Admin Center](media/performance-history/timeframes-in-honolulu.png)

In PowerShell, usare il `-TimeFrame` parametro.

Di seguito sono disponibili intervalli di tempo:

| Periodo   | Frequenza di unità di misura | Conservati per |
|-------------|-----------------------|--------------|
| `LastHour`  | Ogni 10 secondi         | 1 ora       |
| `LastDay`   | Ogni 5 minuti       | 25 ore     |
| `LastWeek`  | Ogni 15 minuti      | 8 giorni       |
| `LastMonth` | Ogni ora          | 35 giorni      |
| `LastYear`  | 1 giorno           | 400 giorni     |

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare il `Get-ClusterPerformanceHistory` cmdlet alla cronologia delle prestazioni di query e del processo in PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Usare la **Get-ClusterPerf** alias salvare alcune sequenze di tasti.

### <a name="example"></a>Esempio

Recupera l'utilizzo della CPU della macchina virtuale *MyVM* nell'ultima ora:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Per esempi più avanzati, vedere pubblicato [script di esempio](performance-history-scripting.md) che forniscono il codice di avvio per trovare i valori di picco, calcolare le medie, tracciare linee di tendenza, eseguire outlier, rilevamento e altro ancora.

### <a name="specify-the-object"></a>Specificare l'oggetto

È possibile specificare l'oggetto desiderato dalla pipeline. Questo codice funziona con 7 tipi di oggetti:

| Oggetto da pipeline | Esempio     |
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


| Parametro                 | Esempio                       | List                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [Inventario raccolto per le unità](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [Inventario raccolto per le schede di rete](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [Informazioni raccolte per i server](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [Informazioni raccolte per i dischi rigidi virtuali](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [Inventario raccolto per le macchine virtuali](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [Informazioni raccolte per i volumi](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [Informazioni raccolte per i cluster](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Usare completamento tramite tasto tab per individuare serie disponibili.

Se non si specifica, viene restituito ogni serie disponibili per l'oggetto specificato.

### <a name="specify-the-timeframe"></a>Specificare l'intervallo di tempo

È possibile specificare l'intervallo di tempo della cronologia, considerando però le `-TimeFrame` parametro.

   > [!TIP]
   > Usare completamento tramite tasto tab per individuare gli intervalli di tempo disponibile.

Se non si specifica, il `MostRecent` misura viene restituita.

## <a name="how-it-works"></a>Come funziona

### <a name="performance-history-storage"></a>Archiviazione della cronologia delle prestazioni

Poco dopo aver abilitato spazi di archiviazione diretta, un volume di circa 10 GB denominato `ClusterPerformanceHistory` viene creato e un'istanza di Extensible Storage Engine (noto anche come Microsoft JET) viene eseguito il provisioning non esiste. Questo database leggero archivia la cronologia delle prestazioni senza alcun coinvolgimento dell'amministratore o gestione.

![Volume di archiviazione della cronologia delle prestazioni](media/performance-history/perf-history-volume.png)

Il volume è supportato da spazi di archiviazione e utilizza mirror semplice, bidirezionale, o la resilienza mirror a tre vie, a seconda del numero di nodi nel cluster. Si procederà al ripristino dopo errori di unità o server esattamente come qualsiasi altro volumi in spazi di archiviazione diretta.

Il volume Usa ReFS ma non di Volume condiviso Cluster (CSV), quindi viene visualizzata solo nel nodo proprietario del gruppo di Cluster. Oltre a viene creato automaticamente, non c'è niente di speciale su questo volume: è possibile visualizzarlo, esplorarlo, ridimensionarlo o eliminarlo (scelta non consigliata). Se si verificano problemi, vedere [Troubleshooting](#troubleshooting). 

### <a name="object-discovery-and-data-collection"></a>Raccolta di dati e di individuazione oggetti

Cronologia prestazioni automaticamente individua gli oggetti pertinenti, ad esempio le macchine virtuali, in un punto qualsiasi nel cluster e avvia il flusso dei contatori delle prestazioni. I contatori vengono aggregati, sincronizzati e inseriti nel database. Il flusso è in esecuzione continua ed è ottimizzato per impatto sul sistema minimo.

Raccolta viene gestita dal servizio di integrità, disponibilità elevata: se il nodo in cui è in esecuzione diventa inattivo, l'operazione verrà ripresa minuti in un secondo momento un altro nodo del cluster. Cronologia delle prestazioni potrebbe scadere brevemente, ma l'operazione verrà ripresa automaticamente. È possibile visualizzare il servizio integrità e il relativo nodo proprietario eseguendo `Get-ClusterResource Health` in PowerShell.

### <a name="handling-measurement-gaps"></a>Gestione degli spazi vuoti di misurazione

Quando misurazioni vengono unite in serie meno granulari che si estendono su più tempo, come descritto in [intervalli di tempo](#Timeframes), sono esclusi i periodi di dati mancanti. Ad esempio, se il server era inattivo per 30 minuti, quindi l'esecuzione al 50% della CPU per i successivi 30 minuti, il `ClusterNode.Cpu.Usage` medio per ora verrà registrata correttamente perché il 50% (non % 25).

### <a name="extensibility-and-customization"></a>Estensibilità e personalizzazione

Cronologia delle prestazioni è facile integrazione con la creazione di script. Usare PowerShell per estrarre tutte le cronologie disponibili direttamente dal database per compilare report automatizzati o avvisi, Esporta la cronologia per maggiore sicurezza, distribuire il proprio visualizzazioni e così via. Vedere pubblicato [script di esempio](performance-history-scripting.md) per codice di avvio utili.

Non è possibile raccogliere cronologia per gli oggetti aggiuntivi, gli intervalli di tempo o serie.

La frequenza di unità di misura e un periodo di conservazione non sono attualmente configurabile.

## <a name="start-or-stop-performance-history"></a>Avviare o arrestare la cronologia delle prestazioni

### <a name="how-do-i-enable-this-feature"></a>Come si abilita questa funzionalità?

A meno che non si `Stop-ClusterPerformanceHistory`, la cronologia delle prestazioni è abilitata per impostazione predefinita.

Per abilitarlo nuovamente, eseguire questo cmdlet di PowerShell come amministratore:

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>Come si disabilita questa funzionalità?

Per arrestare la raccolta di cronologia delle prestazioni, eseguire questo cmdlet di PowerShell come amministratore:

```PowerShell
Stop-ClusterPerformanceHistory
```

Per eliminare le misurazioni esistente, usare il `-DeleteHistory` flag:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Durante la distribuzione iniziale, è possibile impedire la cronologia delle prestazioni di avvio impostando il `-CollectPerformanceHistory` del parametro `Enable-ClusterStorageSpacesDirect` a `$False`.

## <a name="troubleshooting"></a>Risoluzione dei problemi

### <a name="the-cmdlet-doesnt-work"></a>Il cmdlet non funziona

Un messaggio di errore, ad esempio "*il termine 'Get-ClusterPerf' non è riconosciuto come nome di un cmdlet*" significa che la funzionalità non è disponibile o è installata. Verificare di avere Windows Server Insider Preview build 17692 o versione successiva, che è installato il Clustering di Failover e che si esegue spazi di archiviazione diretta.

   > [!NOTE]
   > Questa funzionalità non è disponibile in Windows Server 2016 o versioni precedenti.

### <a name="no-data-available"></a>Nessun dato disponibile 

Se viene visualizzato un grafico a "*non sono disponibili dati*" come indicato in figura, ecco come risolvere i problemi:

![Nessun dato disponibile](media/performance-history/no-data-available.png)

1. Se l'oggetto è stato appena aggiunto o creato, attendere per poter essere individuati (fino a 15 minuti).

2. Aggiornare la pagina o attendere il successivo aggiornamento in background (fino a 30 secondi).

3. Alcuni oggetti speciali vengono esclusi dalla cronologia delle prestazioni, ad esempio, macchine virtuali in cluster e volumi che non usano il file System Volume condiviso Cluster (CSV). Controllare l'argomento secondario per il tipo di oggetto, ad esempio [cronologia delle prestazioni per i volumi](performance-history-for-volumes.md), per fermarsi.

4. Se il problema persiste, aprire PowerShell come amministratore ed eseguire il `Get-ClusterPerf` cmdlet. Il cmdlet include la risoluzione dei problemi per la logica per identificare i problemi comuni, ad esempio se non è presente il volume ClusterPerformanceHistory e vengono fornite istruzioni di monitoraggio e aggiornamento.

5. Se il comando nel passaggio precedente non restituisce nulla, è possibile provare a riavviare il servizio integrità (che raccoglie la cronologia delle prestazioni) eseguendo `Stop-ClusterResource Health ; Start-ClusterResource Health` in PowerShell.

## <a name="see-also"></a>Vedere anche

- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)
