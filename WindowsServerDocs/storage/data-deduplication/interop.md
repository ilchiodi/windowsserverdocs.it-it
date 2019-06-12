---
ms.assetid: 60fca6b2-f1c0-451f-858f-2f6ab350d220
title: Interoperabilità di Deduplicazione dati
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/16/2016
ms.openlocfilehash: b82e02b7896c3795ae7470ca03bb8d19a8d5e403
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455418"
---
# <a name="data-deduplication-interoperability"></a>Interoperabilità di Deduplicazione dati

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2019

## <a name="supported"></a>Supportato

### <a name="refs"></a>ReFS
Deduplicazione dati è supportata a partire da Windows Server 2019. 

### <a name="failover-clustering"></a>Clustering di failover

Il [clustering di failover](../..//failover-clustering/failover-clustering-overview.md) è completamente supportato se in ogni nodo del cluster è installata la [funzionalità Deduplicazione dati](install-enable.md#install-dedup). Altre note importanti:

* [I processi di deduplicazione dei dati avviati manualmente](run.md#running-dedup-jobs-manually) devono essere eseguiti sul nodo del proprietario per il volume condiviso cluster.
* I processi pianificati di Deduplicazione dati vengono archiviati nell'attività cluster pianificata in modo che se un volume deduplicato viene eseguito da un altro nodo, il processo pianificato verrà applicato al successivo intervallo pianificato.
* Deduplicazione dati interagisce completamente con la funzionalità di [aggiornamento in sequenza del sistema operativo in cluster](../..//failover-clustering/cluster-operating-system-rolling-upgrade.md).
* Deduplicazione dati è completamente supportata nei volumi di [Spazi di archiviazione diretta](../storage-spaces/storage-spaces-direct-overview.md) formattati con NTFS (mirror o parità). La deduplicazione non è supportata nei volumi a più livelli. Per altre informazioni, vedere [Data Deduplication on ReFS](#unsupported) (Deduplicazione dati in ReFS).

### <a name="storage-replica"></a>Replica archiviazione
La [replica di archiviazione](../storage-replica/storage-replica-overview.md) è completamente supportata. La funzionalità Deduplicazione dati deve essere configurata in modo da non essere eseguita sulla copia secondaria.

### <a name="branchcache"></a>BranchCache
È possibile ottimizzare l'accesso ai dati in rete abilitando [BranchCache](../../networking/branchcache/branchcache.md) su server e client. Quando un sistema abilitato per BranchCache comunica su una rete WAN con un file server remoto che esegue Deduplicazione dati, tutti i file deduplicati sono già indicizzati e con hash. Di conseguenza, le richieste di dati da una filiale vengono calcolate rapidamente. È simile alla preindicizzazione o al prehashing di un server abilitato per BranchCache.

### <a name="dfs-replication"></a>Replica DFS
La funzionalità Deduplicazione dati funziona con Replica DFS (file system distribuito). L'ottimizzazione o l'annullamento dell'ottimizzazione di un file non attiverà una replica, perché il file non viene modificato. Per i salvataggi attraverso la rete, Replica DFS usa RDC (Remote Differential Compression), non i blocchi nell'archivio blocchi. I file per la replica possono anche essere ottimizzati tramite deduplicazione se la replica usa Deduplicazione dati.

### <a name="quotas"></a>Quote
Deduplicazione dati non supporta la creazione di una quota rigida su una cartella radice del volume in cui è abilitata anche la deduplicazione. Quando una quota rigida è presente in una radice del volume, lo spazio disponibile effettivo sul volume e lo spazio limitato dalla quota non corrispondono. Ciò può compromettere l'esito dei processi di ottimizzazione. È comunque possibile creare una quota flessibile su una cartella radice del volume in cui è abilitata la deduplicazione. 

Quando la quota è abilitata su un volume deduplicato, la quota usa la dimensione logica del file anziché la dimensione fisica del file. L'utilizzo delle quote (incluse le eventuali soglie) non cambia quando un file viene elaborato per la deduplicazione. Tutte le altre funzionalità delle quote, incluse le quote flessibili della radice del volume e le quote nelle sottocartelle, funzionano normalmente durante l'uso della deduplicazione.

### <a name="windows-server-backup"></a>Windows Server Backup
Windows Server Backup consente di eseguire il backup di un volume ottimizzato "così com'è", ovvero senza rimuovere i dati deduplicati. I passaggi seguenti spiegano come eseguire il backup di un volume e come ripristinare un volume o i file selezionati da un volume.
1. Installare Windows Server Backup.  
    ```PowerShell
    Install-WindowsFeature -Name Windows-Server-Backup
    ```

2. Eseguire il backup del volume E: in un altro volume, eseguendo il comando seguente e sostituendo i nomi di volume corretti per la situazione specifica.  
    ```PowerShell
    wbadmin start backup –include:E: -backuptarget:F: -quiet
    ```
3. Ottenere l'ID versione del backup appena creato.

    ```PowerShell
    wbadmin get versions
    ```

    Questo ID versione di output sarà una stringa di data e ora, ad esempio: 08/18/2016-06:22.

4. Ripristinare l'intero volume.
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:Volume  -items:E: -recoveryTarget:E:
    ```

    **--OR--**  

    Ripristinare una particolare cartella (in questo caso la cartella E:\Docs):
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:File  -items:E:\Docs  -recursive
    ```

## <a name="unsupported"></a>File modifiche disco non supportato

### <a name="windows-10-client-os"></a>Windows 10 (SO client)
La funzionalità Deduplicazione dati non è supportata in Windows 10. Esistono diversi post di blog popolari nella community di Windows che descrivono come rimuovere i file binari da Windows Server 2016 e installarli su Windows 10, ma questo scenario non è stato convalidato come parte dello sviluppo di Deduplicazione dati. [Vota per questo elemento per Windows 10 vNext su Windows Server Storage UserVoice](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/9011008-add-deduplication-support-to-client-os).

### <a name="windows-search"></a>Windows Search
Windows Search non supporta Deduplicazione dati. Deduplicazione dati usa reparse point che Windows Search non può indicizzare, pertanto tutti i file deduplicati vengono ignorati ed esclusi dall'indice. Di conseguenza, i risultati della ricerca potrebbero essere incompleti per i volumi deduplicati. [Vota per questo elemento per Windows Server vNext su Windows Server Storage UserVoice](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/17888647-make-windows-search-service-work-with-data-dedupli).

### <a name="robocopy"></a>Robocopy
Non è consigliabile eseguire Robocopy con Deduplicazione dati perché alcuni comandi di Robocopy possono danneggiare l'Archivio blocchi. L'archivio blocchi è archiviato nella cartella System Volume Information per un volume. Se la cartella viene eliminata, i file ottimizzati (reparse point) copiati dal volume di origine vengono danneggiati poiché i blocchi di dati non vengono copiati nel volume di destinazione.
