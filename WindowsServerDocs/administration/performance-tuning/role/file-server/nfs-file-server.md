---
title: Ottimizzazione delle prestazioni per i File server NFS
description: Ottimizzazione delle prestazioni per i File server NFS
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: RoopeshB, NedPyle
ms.date: 10/16/2017
ms.openlocfilehash: 06a2a7206d3673046bd5a926a657bac91b02bf5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879012"
---
# <a name="performance-tuning-nfs-file-servers"></a>I File server NFS l'ottimizzazione delle prestazioni

## <a href="" id="servicesnfs"></a>Servizi per il modello NFS


Le sezioni seguenti forniscono informazioni su Microsoft Services per il modello di File System NFS (Network) per la comunicazione tra client e server. Poiché NFS v3 e v2 NFS sono ancora più diffusa versioni del protocollo, tutte le chiavi del Registro di sistema, ad eccezione MaxConcurrentConnectionsPerIp si applicano a v2 NFS e NFS v3 solo.

Nessuna ottimizzazione del Registro di sistema è necessario per il protocollo NFS versione 4.1.

### <a name="service-for-nfs-model-overview"></a>Servizi per Panoramica del modello di NFS

Microsoft Services for NFS offre una soluzione di condivisione file per le aziende che dispongono di un ambiente misto Windows e UNIX. Questo modello di comunicazione è costituito da computer client e un server. Le applicazioni sul client richiedono file che si trovano nel server tramite il redirector (Rdbss) e mini-redirector NFS (nfsrdr). Il mini redirector Usa il protocollo NFS per inviare la richiesta tramite TCP/IP. Il server riceve più richieste dai client tramite TCP/IP e il routing delle richieste per il file system locale (sys), con cui si accede lo stack di archiviazione.

La figura seguente illustra il modello di comunicazione per NFS.

![modello di comunicazione NFS](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>Regolazione dei parametri per i file server NFS

Il seguente REG\_le impostazioni del Registro di sistema DWORD possono influenzare le prestazioni di file server NFS:

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    Il valore predefinito è 0. Questo parametro determina se i file vengono aperti per il FILE\_RANDOM\_l'accesso o per il FILE\_SEQUENZIALE\_ONLY, a seconda delle caratteristiche del carico di lavoro i/o. Impostare questo valore su 1 per forzare i file da aprire per FILE\_RANDOM\_accesso. FILE\_RANDOM\_accesso impedisce la gestione di sistema e della cache di file di prelettura.

    >[!NOTE]
    > Questa impostazione deve essere valutata con attenzione perché possono avere impatto potenziale su cache di file system aumento delle dimensioni.


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    L'impostazione predefinita è 5. Questo parametro controlla la durata di una voce della cache NFS nella cache handle di file. Il parametro fa riferimento alle voci della cache che hanno un file system NTFS open associato gestire. Durata effettiva corrisponde approssimativamente a RdWrHandleLifeTime moltiplicato RdWrThreadSleepTime. Il valore minimo è 1 e il valore massimo è 60.

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    L'impostazione predefinita è 5. Questo parametro controlla la durata di una voce della cache NFS nella cache handle di file. Il parametro fa riferimento alle voci della cache che non è un file system NTFS open associato gestire. Servizi per NFS utilizza le voci della cache per archiviare gli attributi di file per un file senza mantenere un handle aperto con il file system. Durata effettiva corrisponde approssimativamente a RdWrNfsHandleLifeTime moltiplicato RdWrThreadSleepTime. Il valore minimo è 1 e il valore massimo è 60.

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    L'impostazione predefinita è 5. Questo parametro controlla la durata di una condivisione NFS lettura della voce della cache nella cache handle di file. Durata effettiva corrisponde approssimativamente a RdWrNfsReadHandlesLifeTime moltiplicato RdWrThreadSleepTime. Il valore minimo è 1 e il valore massimo è 60.

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    L'impostazione predefinita è 5. Questo parametro controlla l'intervallo di attesa prima dell'esecuzione del thread di pulizia della cache di handle di file. Il valore è espresso in tick, ed è deterministica. Un tick equivale a circa 100 nanosecondi. Il valore minimo è 1 e il valore massimo è 60.

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    Il valore predefinito è 4. Questo parametro specifica la memoria massima deve essere utilizzato da voci della cache di handle di file. Il valore minimo è 1 e il valore massimo è 1\*1024\*1024\*1024 (1073741824).

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    Il valore predefinito è 0. Questo parametro specifica se le pagine fisiche allocate per la dimensione della cache specificata dal FileHandleCacheSizeInMB sono bloccate in memoria. Impostando questo valore su 1 abilita questa attività. Le pagine sono bloccate in memoria (non di paging su disco), che migliora le prestazioni di risolvere gli handle di file, ma riduce la memoria disponibile per le applicazioni.

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    Il valore predefinito è 64. Questo parametro specifica il numero massimo di handle per volume per la cache di lettura dei dati. Le voci della cache di lettura vengono create solo su sistemi con più di 1 GB di memoria. Il valore minimo è 0 e il valore massimo è 0xFFFFFFFF.

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    Il valore predefinito è 1. Questo parametro controlla se gli handle specificate da Server NFS File effettuati l'accesso a livello di crittografia. Impostarlo su 0 disabilita la firma di handle.

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    Il valore predefinito è 60. Questo parametro è un timeout di software che controlla la durata della memorizzazione nella cache di dati NFS V3 instabile di scrittura. Il valore minimo è 1 e il valore massimo è 600. Durata effettiva corrisponde approssimativamente a RdWrNfsDeferredWritesFlushDelay moltiplicato RdWrThreadSleepTime.

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    Il valore predefinito è 1 (abilitato). Questo parametro controlla se gli handle aperti durante NFS V2 e V3 creare e MKDIR RPC procedura i gestori vengono mantenuti nel file di gestiscono della cache. Impostare questo valore su 0 per disabilitarla aggiunge voci alla cache nel server crea e MKDIR percorsi del codice.

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    Aumenta il numero di thread di lavoro ritardato che vengono creati per la coda di lavoro specificato. Ritardo worker thread processo gli elementi di lavoro che non sono considerati critico e che può avere eseguito il paging durante l'attesa di elementi di lavoro relativi stack di memoria. Un numero insufficiente di thread consente di ridurre la frequenza alla quale il lavoro vengono gestiti gli elementi; un valore troppo elevato utilizza inutilmente le risorse di sistema.

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    L'impostazione predefinita in Windows Server 2012 e Windows Server 2012 R2 è 2. Nelle versioni precedenti a Windows Server 2012, il valore predefinito è 0. Questo parametro determina se NTFS genera un nome breve 8.3 la convenzione di denominazione (MSDOS) per i nomi file lunghi e per i nomi di file che contengono caratteri dal set di caratteri esteso. Se il valore di questa voce è 0, i file possono avere due nomi: il nome specificato dall'utente e il nome breve che genera l'errore NTFS. Se il nome utente specificato segue la convenzione di denominazione 8.3, NTFS non genera un nome breve. Un valore pari a 2 indica che questo parametro può essere configurato per ogni volume.

    >[!NOTE]
    > Il volume di sistema ha 8.3 abilitata per impostazione predefinita. Tutti gli altri volumi in Windows Server 2012 e Windows Server 2012 R2 hanno 8.3 disabilitata per impostazione predefinita. Se si modifica questo valore non modifica il contenuto di un file, ma evita la creazione di attributo nome breve per il file, che viene modificato anche come NTFS Visualizza e gestisce il file. Per la maggior parte dei file server, l'impostazione consigliata è 1 (disabilitato).


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    Il valore predefinito è 1. Questo commutatore globali di sistema riduce il carico dei / o disco e alla bassa latenza disabilitando l'aggiornamento del timbro data e ora di ultimo accesso al file o directory.

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    Il valore predefinito del parametro MaxConcurrentConnectionsPerIp è 16. È possibile aumentare questo valore fino a un massimo di 8192 per aumentare il numero di connessioni per ogni indirizzo IP.
