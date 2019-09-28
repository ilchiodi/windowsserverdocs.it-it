---
title: Ottimizzazione delle prestazioni per i file server NFS
description: Ottimizzazione delle prestazioni per i file server NFS
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: RoopeshB, NedPyle
ms.date: 10/16/2017
ms.openlocfilehash: 07e5005c1bc38e791e847c8965cbc9a6c0ac96f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355177"
---
# <a name="performance-tuning-nfs-file-servers"></a>Ottimizzazione delle prestazioni di file server NFS

## <a href="" id="servicesnfs"></a>Modello di servizi per NFS


Nelle sezioni seguenti vengono fornite informazioni sul modello di Microsoft Services for Network File System (NFS) per la comunicazione client-server. Poiché NFS V2 e NFS v3 sono ancora le versioni più diffuse del protocollo, tutte le chiavi del registro di sistema, ad eccezione di MaxConcurrentConnectionsPerIp, si applicano solo a NFS V2 e NFS v3.

Per il protocollo NFS v 4.1 non è necessaria alcuna ottimizzazione del registro di sistema.

### <a name="service-for-nfs-model-overview"></a>Panoramica del modello di servizio per NFS

Microsoft Services for NFS fornisce una soluzione di condivisione file per le aziende che hanno un ambiente misto Windows e UNIX. Questo modello di comunicazione è costituito da computer client e server. Applicazioni nei file di richiesta client che si trovano sul server tramite il redirector (Rdbss. sys) e il mini-redirector NFS (Nfsrdr. sys). Il mini-redirector usa il protocollo NFS per inviare la richiesta tramite TCP/IP. Il server riceve più richieste dai client tramite TCP/IP e instrada le richieste al file system locale (NTFS. sys), che accede allo stack di archiviazione.

Nella figura seguente viene illustrato il modello di comunicazione per NFS.

![modello di comunicazione NFS](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>Parametri di ottimizzazione per i file server NFS

Le impostazioni del registro di sistema REG @ no__t-0DWORD seguenti possono influire sulle prestazioni dei file server NFS:

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    Il valore predefinito è 0. Questo parametro determina se i file vengono aperti per il FILE @ no__t-0RANDOM @ no__t-1ACCESS o per il FILE @ no__t-2SEQUENTIAL @ no__t-3ONLY, a seconda delle caratteristiche di I/O del carico di lavoro. Impostare questo valore su 1 per forzare l'apertura dei file per il FILE @ no__t-0RANDOM @ no__t-1ACCESS. Il FILE @ no__t-0RANDOM @ no__t-1ACCESS impedisce la prelettura del file system e del gestore della cache.

    >[!NOTE]
    > Questa impostazione deve essere valutata attentamente perché potrebbe avere un impatto potenziale sull'aumento della cache di file di sistema.


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    Il valore predefinito è 5. Questo parametro controlla la durata di una voce della cache NFS nella cache dell'handle di file. Il parametro fa riferimento alle voci della cache a cui è associato un handle di file NTFS aperto. La durata effettiva è approssimativamente uguale a RdWrHandleLifeTime moltiplicato per RdWrThreadSleepTime. Il valore minimo è 1 e il valore massimo è 60.

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    Il valore predefinito è 5. Questo parametro controlla la durata di una voce della cache NFS nella cache dell'handle di file. Il parametro fa riferimento alle voci della cache a cui non è associato un handle di file NTFS aperto. Services for NFS usa queste voci di cache per archiviare gli attributi di file per un file senza mantenere un handle aperto con la file system. La durata effettiva è approssimativamente uguale a RdWrNfsHandleLifeTime moltiplicato per RdWrThreadSleepTime. Il valore minimo è 1 e il valore massimo è 60.

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    Il valore predefinito è 5. Questo parametro controlla la durata di una voce della cache di lettura NFS nella cache degli handle di file. La durata effettiva è approssimativamente uguale a RdWrNfsReadHandlesLifeTime moltiplicato per RdWrThreadSleepTime. Il valore minimo è 1 e il valore massimo è 60.

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    Il valore predefinito è 5. Questo parametro controlla l'intervallo di attesa prima di eseguire il thread di pulizia nella cache dell'handle di file. Il valore è in cicli e non è deterministico. Un segno di selezione è equivalente a circa 100 nanosecondi. Il valore minimo è 1 e il valore massimo è 60.

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    Il valore predefinito è 4. Questo parametro specifica la quantità massima di memoria che deve essere utilizzata dalle voci della cache dell'handle di file. Il valore minimo è 1 e il valore massimo è 1 @ no__t-01024 @ no__t-11024 @ no__t-21024 (1073741824).

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    Il valore predefinito è 0. Questo parametro specifica se le pagine fisiche allocate per le dimensioni della cache specificate da FileHandleCacheSizeInMB sono bloccate in memoria. Se si imposta questo valore su 1, questa attività verrà abilitata. Le pagine sono bloccate in memoria (non da paging a disco), che migliora le prestazioni della risoluzione degli handle di file, ma riduce la memoria disponibile per le applicazioni.

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    Il valore predefinito è 64. Questo parametro specifica il numero massimo di handle per volume per la cache dei dati di lettura. Le voci della cache di lettura vengono create solo nei sistemi con più di 1 GB di memoria. Il valore minimo è 0 e il valore massimo è 0xFFFFFFFF.

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    Il valore predefinito è 1. Questo parametro controlla se gli handle forniti dal file server NFS sono firmati a crittografia. L'impostazione su 0 Disabilita la firma dell'handle.

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    Il valore predefinito è 60. Questo parametro è un timeout soft che controlla la durata della memorizzazione nella cache dei dati di scrittura instabile NFS v3. Il valore minimo è 1 e il valore massimo è 600. La durata effettiva è approssimativamente uguale a RdWrNfsDeferredWritesFlushDelay moltiplicato per RdWrThreadSleepTime.

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    Il valore predefinito è 1 (abilitato). Questo parametro controlla se gli handle aperti durante la creazione dei gestori di stored procedure per NFS V2 e V3 e MKDIR sono conservati nella cache degli handle di file. Impostare questo valore su 0 per disabilitare l'aggiunta di voci alla cache nei percorsi di codice CREATE e MKDIR.

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    Aumenta il numero di thread di lavoro ritardati creati per la coda di lavoro specificata. I thread di lavoro ritardati elaborano gli elementi di lavoro che non sono considerati critici per il tempo e che possono essere sottoponiti a paging in attesa di elementi di lavoro. Un numero insufficiente di thread riduce la frequenza con cui gli elementi di lavoro vengono serviti; un valore troppo elevato utilizza inutilmente le risorse di sistema.

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    Il valore predefinito in Windows Server 2012 e Windows Server 2012 R2 è 2. Nelle versioni precedenti a Windows Server 2012, il valore predefinito è 0. Questo parametro determina se NTFS genera un nome breve nella convenzione di denominazione 8dot3 (MSDOS) per i nomi di file lunghi e per i nomi di file che contengono caratteri del set di caratteri esteso. Se il valore di questa voce è 0, i file possono avere due nomi: il nome specificato dall'utente e il nome breve generato da NTFS. Se il nome specificato dall'utente segue la convenzione di denominazione 8dot3, NTFS non genera un nome breve. Il valore 2 indica che questo parametro può essere configurato per volume.

    >[!NOTE]
    > Il volume di sistema ha 8dot3 abilitato per impostazione predefinita. Per tutti gli altri volumi di Windows Server 2012 e Windows Server 2012 R2 8dot3 è disabilitato per impostazione predefinita. La modifica di questo valore non comporta la modifica del contenuto di un file, ma evita la creazione di attributi con nome breve per il file, che modifica anche il modo in cui NTFS Visualizza e gestisce il file. Per la maggior parte dei file server, l'impostazione consigliata è 1 (disabilitata).


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    Il valore predefinito è 1. Questa opzione globale di sistema riduce il carico e le latenze di I/O del disco disabilitando l'aggiornamento della data e dell'ora dell'ultimo accesso al file o alla directory.

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    Il valore predefinito del parametro MaxConcurrentConnectionsPerIp è 16. È possibile aumentare questo valore fino a un massimo di 8192 per aumentare il numero di connessioni per ogni indirizzo IP.
