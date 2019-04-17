---
title: Cronologia delle prestazioni di spazi di archiviazione diretta
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239258"
---
# Cronologia delle prestazioni di spazi di archiviazione diretta

> Si applica a: Windows Server 2019

Cronologia delle prestazioni è una nuova funzionalità che offre agli amministratori di [Spazi di archiviazione diretta](storage-spaces-direct-overview.md) facile accesso alle calcolo cronologica, memoria, rete e le misurazioni di archiviazione tra i server host, unità, volumi, le macchine virtuali e altro ancora. Cronologia delle prestazioni viene raccolto automaticamente e archiviata in cluster per fino a un anno.

   > [!IMPORTANT]
   > Questa funzionalità è una novità in Windows Server 2019. Non è disponibile in Windows Server 2016.

## Informazioni di base

Cronologia delle prestazioni verrà raccolti per impostazione predefinita con spazi di archiviazione diretta in Windows Server 2019. Non è necessario installare, configurare o avviare nulla. Non è necessaria una connessione Internet, System Center non è obbligatorio e un database esterno non è obbligatorio.

Per visualizzare la cronologia delle prestazioni del cluster graficamente, utilizzare [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Cronologia delle prestazioni in Windows Admin Center](media/performance-history/perf-history-in-wac.png)

Per eseguire una query ed elaborare a livello di codice, Usa la nuova `Get-ClusterPerf` cmdlet. Vedi [l'utilizzo di PowerShell](#usage-in-powershell).

## Che cos'è raccolti

Cronologia delle prestazioni verrà raccolti per 7 tipi di oggetti:

![Tipi di oggetti](media/performance-history/types-of-object.png)

Ogni tipo di oggetto ha molti serie: ad esempio, `ClusterNode.Cpu.Usage` vengono raccolti per ogni server.

Per informazioni dettagliate di ciò che vengono raccolti per ogni tipo di oggetto e come interpretarli, fare riferimento a questi argomenti secondari:

| Oggetto             | Serie                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Unità             | [Che cos'è raccolti per le unità](performance-history-for-drives.md)                     |
| Schede di rete   | [Che cos'è raccolti per le schede di rete](performance-history-for-network-adapters.md) |
| Server            | [Che cos'è raccolti per server](performance-history-for-servers.md)                   |
| Dischi rigidi virtuali | [Ciò che vengono raccolti per i dischi rigidi virtuali](performance-history-for-vhds.md)           |
| Macchine virtuali   | [Che cos'è raccolti per le macchine virtuali](performance-history-for-vms.md)              |
| Volumi            | [Che cos'è raccolti per i volumi](performance-history-for-volumes.md)                   |
| Cluster           | [Che cos'è raccolti per cluster](performance-history-for-clusters.md)                 |

Numero di serie sono aggregati per gli oggetti peer al relativo elemento padre: ad esempio, `NetAdapter.Bandwidth.Inbound` viene raccolto separatamente per ogni scheda di rete e aggregati al server complessiva. Analogamente `ClusterNode.Cpu.Usage` vengono aggregati al cluster complessiva; E così via.

## Soglie

Cronologia delle prestazioni viene archiviata per fino a un anno, con una granularità al livello decrescente. Per ora più recente, le misurazioni sono disponibili ogni 10 secondi. Successivamente, vengono unite in modo intelligente (Calcola la media dei o somma, come appropriato) nella meno granulare serie che si estendono su più tempo. Per il giorno più recente, le misurazioni sono disponibili cinque minuti; per settimana più recente, ogni 15 minuti. E così via.

In Windows Admin Center, è possibile selezionare l'intervallo di tempo in alto a destra sopra il grafico.

![Soglie in Windows Admin Center](media/performance-history/timeframes-in-honolulu.png)

In PowerShell, Usa il `-TimeFrame` parametro.

Ecco le soglie disponibili:

| Intervallo di tempo   | Frequenza di misurazione | Mantenuti |
|-------------|-----------------------|--------------|
| `LastHour`  | Ogni 10 secondi         | 1 ora       |
| `LastDay`   | Ogni 5 minuti       | 25 ore     |
| `LastWeek`  | Ogni 15 minuti      | 8 giorni       |
| `LastMonth` | Ogni ora          | 35 giorni      |
| `LastYear`  | Ogni giorno 1           | 400 giorni     |

## Utilizzo di PowerShell

Usa il `Get-ClusterPerformanceHistory` cmdlet alla cronologia delle prestazioni di query e il processo di PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Usare l'alias **Get-ClusterPerf** per salvare alcune pressioni di tasti.

### Esempio

Ottenere l'utilizzo della CPU della macchina virtuale *MyVM* per l'ultima ora:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Per esempi più avanzati, Vedi gli [script di esempio](performance-history-scripting.md) pubblicato che forniscono il codice di partenza per trovare i valori di picco, calcolare medie, tracciare linee di tendenza, Esegui valore anomalo rilevamento e altro ancora.

### Specificare l'oggetto

È possibile specificare l'oggetto desiderato dalla pipeline. Questa versione funziona con 7 tipi di oggetti:

| Oggetto dalla pipeline | Esempio     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Se non si specifica, viene restituita cronologia delle prestazioni per il cluster complessiva.

### Specificare la serie

È possibile specificare la serie desiderato con questi parametri:


| Parametro                 | Esempio                       | List                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [Che cos'è raccolti per le unità](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [Che cos'è raccolti per le schede di rete](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [Che cos'è raccolti per server](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [Ciò che vengono raccolti per i dischi rigidi virtuali](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [Che cos'è raccolti per le macchine virtuali](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [Che cos'è raccolti per i volumi](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [Che cos'è raccolti per cluster](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Usa completamento tramite tab per individuare serie disponibili.

Se non si specifica, viene restituito ogni serie disponibili per l'oggetto specificato.

### Specificare l'intervallo di tempo

Puoi specificare l'intervallo di tempo della cronologia degli desiderato con il `-TimeFrame` parametro.

   > [!TIP]
   > Usa completamento tramite tab per individuare soglie disponibili.

Se non specifichi, il `MostRecent` misurazione viene restituito.

## Come funziona

### Archiviazione cronologia delle prestazioni

Subito dopo aver abilitato spazi di archiviazione diretta, un volume di circa 10 GB denominato `ClusterPerformanceHistory` viene creato e viene eseguita non esiste un'istanza del motore di archiviazione Extensible (noto anche come Microsoft JET). Questo database leggero archivia la cronologia delle prestazioni senza coinvolgimento amministratore o gestione.

![Contratti multilicenza per l'archiviazione di cronologia delle prestazioni](media/performance-history/perf-history-volume.png)

Il volume è supportato da spazi di archiviazione e Usa semplici e bidirezionale mirror o la resilienza con mirroring a tre vie, a seconda del numero dei nodi del cluster. Il ripristino dopo errori di unità o server come qualsiasi altro volume in spazi di archiviazione diretta.

Il volume ReFS Usa ma non è Cluster Shared Volume (CSV), in modo che viene visualizzato solo sul nodo del proprietario gruppo Cluster. Oltre a viene creato automaticamente, c'è niente di speciale su questo volume: vederla, esplorarlo, ridimensionarlo o eliminarlo (scelta non consigliata). Se qualcosa va storto, vedere [risoluzione dei problemi](#troubleshooting). 

### Oggetto individuazione e la raccolta dati

Cronologia delle prestazioni automaticamente individua pertinenti oggetti, ad esempio macchine virtuali, ovunque nel cluster e inizia relativi contatori delle prestazioni di streaming. I contatori sono aggregati, sincronizzati e inseriti nel database. Streaming viene eseguito in modo continuo ed è ottimizzata per un impatto minimo di sistema.

Raccolta viene gestita tramite il servizio di integrità, che è altamente disponibile: se il nodo in cui è in esecuzione va verso il basso, viene ripristinato minuto in un secondo momento un altro nodo del cluster. Cronologia delle prestazioni potrà scadano brevemente, ma riprenderà automaticamente. Puoi vedere servizio integrità e il relativo nodo proprietario eseguendo `Get-ClusterResource Health` in PowerShell.

### Gestione degli spazi vuoti misura

Quando le misurazioni vengono unite in serie meno granulare che si estendono su più tempo, come descritto in [soglie](#Timeframes), vengono esclusi periodi di dati mancanti. Ad esempio, se il server è stato premuto per 30 minuti, quindi in esecuzione al 50% della CPU per i successivi 30 minuti, il `ClusterNode.Cpu.Usage` Media per l'ora verrà registrato correttamente come pari al 50% (non 25%).

### Estendibilità e personalizzazione

Cronologia delle prestazioni è adatto scripting. Utilizzare PowerShell per eseguire il pull qualsiasi Cronologia disponibile direttamente dal database di creare report automatizzati o continui, Esporta la cronologia di salvaguardia, rollio effetti grafici personalizzati e così via. Vedi gli [script di esempio](performance-history-scripting.md) pubblicato per il codice starter utile.

Non è possibile raccogliere la cronologia per gli altri oggetti, soglie o serie.

La frequenza di misurazione e il periodo di conservazione non sono attualmente configurabili.

## Avviare o arrestare la cronologia delle prestazioni

### Come faccio ad abilitare questa funzionalità?

A meno che non è `Stop-ClusterPerformanceHistory`, la cronologia delle prestazioni è abilitata per impostazione predefinita.

Per abilitare nuovamente, Esegui questo cmdlet di PowerShell come amministratore:

```PowerShell
Start-ClusterPerformanceHistory
```

### Come si può disabilitare questa funzionalità?

Per interrompere la raccolta cronologia delle prestazioni, Esegui questo cmdlet di PowerShell come amministratore:

```PowerShell
Stop-ClusterPerformanceHistory
```

Per eliminare le misurazioni esistente, Usa il `-DeleteHistory` flag:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Durante la distribuzione iniziale, è possibile impedire la cronologia delle prestazioni di avvio impostando la `-CollectPerformanceHistory` parametro di `Enable-ClusterStorageSpacesDirect` a `$False`.

## Risoluzione dei problemi

### Il cmdlet non funziona

Un messaggio di errore come "*il termine ' Get-ClusterPerf' non è riconosciuto come il nome di un cmdlet*" significa che la funzionalità non è disponibile o installato. Verificare di disporre delle build di Windows Server Insider Preview 17692 o versione successiva, che hai installato Clustering di Failover e che stai usando spazi di archiviazione diretta.

   > [!NOTE]
   > Questa funzionalità non è disponibile in Windows Server 2016 o versioni precedenti.

### Nessun dato disponibile 

Se un grafico mostra "*dati non disponibili*", come illustrato, ecco come risolvere i problemi di:

![Nessun dato disponibile](media/performance-history/no-data-available.png)

1. Se l'oggetto è stato appena aggiunto o creato, Attendi per poter essere individuati (fino a 15 minuti).

2. Aggiornare la pagina o attendere il successivo aggiornamento in background (fino a 30 secondi).

3. Alcuni oggetti speciali vengono esclusi dalla cronologia delle prestazioni: ad esempio macchine virtuali che non sono in cluster e i volumi che non usano il file System Volume condiviso Cluster (CSV). Controlla l'argomento secondario per il tipo di oggetto, come la [cronologia delle prestazioni per i volumi](performance-history-for-volumes.md), per la stampa.

4. Se il problema persiste, Apri PowerShell come amministratore ed eseguire il `Get-ClusterPerf` cmdlet. Il cmdlet include la logica per identificare i problemi comuni, la risoluzione dei problemi, ad esempio se il volume ClusterPerformanceHistory mancante e vengono fornite istruzioni la correzione.

5. Se il comando nel passaggio precedente restituisce valori, è possibile provare a riavviare il servizio integrità (che raccoglie la cronologia delle prestazioni) eseguendo `Stop-ClusterResource Health ; Start-ClusterResource Health` in PowerShell.

## Vedi anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
