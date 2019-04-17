---
title: Panoramica del file system ReFS (Resilient File System)
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: dmoss
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 1/3/2016
ms.assetid: 
ms.openlocfilehash: 4460df978f2a3d5e30e43b82cf952ed37f3d91a9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="resilient-file-system-refs-overview"></a>Panoramica del file system ReFS (Resilient File System)
>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il file system ReFS (Resilient File System) è il file system più recente di Microsoft, progettato per ottimizzare la disponibilità dei dati, ridimensionare in modo efficiente a set di dati grandi in vari carichi di lavoro e per garantire l'integrità dei dati per mezzo della resilienza ai danneggiamenti. Cerca di risolvere una serie di scenari di archiviazione a espansione e stabilire una base per le innovazioni future. 

## <a name="key-benefits"></a>Principali vantaggi

### <a name="resiliency"></a>Resilienza 
ReFS introduce nuove funzionalità in grado di rilevare con precisione i danneggiamenti e di risolverli, rimanendo, nel contempo, online. In questo modo vengono fornite una maggiore integrità e disponibilità dei dati: 

- **Flussi di integrità**: ReFS usa checksum per i metadati e facoltativamente per i dati dei file. In questo modo, ha la possibilità di rilevare in modo affidabile i danneggiamenti. 
- **Integrazione di Spazi di archiviazione**: quando utilizzato insieme a uno spazio parità o mirror, ReFS è in grado di ripristinare automaticamente i danneggiamenti rilevati usando la copia alternativa dei dati fornita da Spazi di archiviazione. I processi di ripristino sono entrambi localizzati nell'area del danneggiamento ed eseguiti online, senza alcun tempo di inattività per i volumi.
- **Recupero dati**: se un volume viene danneggiato e non esiste una copia alternativa dei dati danneggiati, ReFS rimuove i dati danneggiati dallo spazio dei nomi. ReFS mantiene il volume online gestendo, nel contempo, la maggior parte dei danneggiamenti non correggibili. Tuttavia, esistono rari casi per cui ReFS deve portare offline il volume.
- **Correzione degli errori proattiva**: oltre a convalidare i dati prima della lettura e della scrittura, ReFS introduce uno scanner di integrità dei dati, noto come <i>strumento di pulitura</i>. Questo strumento di pulitura analizza periodicamente il volume, identificando i danneggiamenti latenti e attivando in modo proattivo il ripristino dei dati danneggiati. 


### <a name="performance"></a>Prestazioni
Oltre a fornire miglioramenti della resilienza, ReFS introduce nuove funzionalità per carichi di lavoro sensibili alle prestazioni e virtualizzati. L'ottimizzazione del livello in tempo reale, la clonazione dei blocchi e VDL di tipo sparse sono ottimi esempi delle funzionalità in evoluzione di ReFS, progettate per supportare carichi di lavoro dinamici e diversi.

- **[Parità accelerata con mirroring](./mirror-accelerated-parity.md)**: la parità accelerata con mirroring offre prestazioni elevate e spazio di archiviazione efficiente in termini di capacità per i dati. 

    - Per garantire prestazioni elevate e spazio di archiviazione efficiente in termini di capacità, ReFS divide un volume in due gruppi di archiviazione logica, noti come livelli. Questi livelli possono disporre di propri tipi di resilienza e unità consentendone quindi l'ottimizzazione delle prestazioni o della capacità. Alcune configurazioni di esempio sono: 
    
    
    | Livello prestazioni | Livello capacità |
    |----------------|-----------------|
     Unità SSD con mirroring | Unità HDD con mirroring |
     Unità SSD con mirroring | Unità SSD con parità |
     Unità SSD con mirroring | Unità HDD con parità |   
            
    - Una volta configurati questi livelli, ReFS li utilizza per fornire un'archiviazione veloce di dati ad accesso frequente e un'archiviazione efficiente in termini di capacità per i dati ad accesso sporadico:
        - Tutte le scritture vengono eseguite nel livello prestazioni. I grandi gruppi di dati che rimangono in questo livello verranno spostati in modo efficiente nel livello capacità in tempo reale.
        - Se si utilizza una [distribuzione ibrida](../storage-spaces/choosing-drives.md) (combinazione di unità flash e HDD), [la cache in Spazi di archiviazione diretta](../storage-spaces/understand-the-cache.md) contribuirà ad accelerare le letture, riducendo l'effetto della caratteristica di frammentazione dei dati dei carichi di lavoro virtualizzati. In caso contrario, se si utilizza una distribuzione all-flash, le letture verranno eseguite anche nel livello prestazioni. 

>[!NOTE]
>Per le distribuzioni di Server, la parità accelerata con mirroring è supportata solo in [Spazi di archiviazione diretta](../storage-spaces/storage-spaces-direct-overview.md).

- **Operazioni VM con accelerazione**: ReFS introduce una nuova funzionalità destinata in particolar modo al miglioramento delle prestazioni di carichi di lavoro virtualizzati:
    - [Clonazione di blocchi](./block-cloning.md): la clonazione di blocchi consente di accelerare le operazioni di copia, consentendo rapide operazioni di unione del checkpoint VM a basso impatto. 
    - VDL di tipo sparse: VDL di tipo sparse consente a ReFS di azzerare rapidamente i file, riducendo il tempo necessario per creare dischi rigidi virtuali fissi da decine di minuti a pochi secondi.


- **Dimensioni variabili del cluster**: ReFS supporta dimensioni del cluster di 4 KB e 64 KB. 4 KB è la dimensione del cluster consigliata per la maggior parte delle distribuzioni, ma i cluster di 64 KB sono appropriati per grandi carichi di lavoro I/O sequenziali.
    
    
### **<a name="scalability"></a>Scalabilità**
ReFS è progettato per supportare set di dati molto grandi (nell'ordine di milioni di terabyte) senza influire negativamente sulle prestazioni, ottenendo una scalabilità maggiore rispetto ai file system precedenti. 

## <a name="supported-deployments"></a>Distribuzioni supportate

### <a name="storage-spaces-direct"></a>Spazi di archiviazione diretta ###

La distribuzione di ReFS in Spazi di archiviazione diretta è consigliata per i carichi di lavoro virtualizzati o per i dispositivi NAS: 
- Parità accelerata con mirroring e [la cache in Spazi di archiviazione diretta](../storage-spaces/understand-the-cache.md) offrono prestazioni elevate e un'archiviazione efficiente in termini di capacità. 
- L'introduzione della clonazione dei blocchi e di VDL di tipo sparse accelera in modo significativo le operazioni di file .vhdx, quali la creazione, l'unione e l'espansione.
- I flussi di integrità, il ripristino online e le copie di dati alternative consentono a ReFS e a Spazi di archiviazione diretta di collaborare per rilevare e correggere insieme i danneggiamenti sia nei metadati sia nei dati. 
- ReFS offre la funzionalità di ridimensionare e supportare set di dati di notevoli dimensioni. 

### <a name="storage-spaces-with-sas-drive-enclosures"></a>Spazi di archiviazione con alloggiamenti di unità SAS ###
La distribuzione di ReFS in Spazi di archiviazione con alloggiamenti SAS condivisi è appropriata per l'hosting di dati di archiviazione e l'archiviazione dei documenti dell'utente:
- I flussi di integrità, il ripristino online e le copie di dati alternative consentono a ReFS e a Spazi di archiviazione diretta di collaborare per rilevare e correggere insieme i danneggiamenti sia nei metadati sia nei dati. 
- Le distribuzioni di spazi di archiviazione possono inoltre utilizzare la clonazione in blocco e la scalabilità offerte in ReFS.

### <a name="basic-disks"></a>Dischi di base ###
La distribuzione di ReFS in dischi di base è particolarmente adatta alle applicazioni che implementano le proprie soluzioni di resilienza e disponibilità del software. 
- Le applicazioni che introducono le proprie soluzioni di disponibilità e resilienza del software possono sfruttare i flussi di integrità, la clonazione in blocchi e la possibilità di ridimensionare e supportare set di dati di grandi dimensioni. 

>[!NOTE]
>ReFS non è supportata sull'archiviazione SAN.


## <a name="feature-comparison"></a>Confronto delle funzionalità

### <a name="limits"></a>Limiti

| Funzionalità       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Lunghezza massima del nome del file | 255caratteri Unicode  | 255caratteri Unicode               |
| Lunghezza massima del nome del percorso |32.000caratteri Unicode | 32.000caratteri Unicode                |
| Dimensione massime file | 18 EB (exabyte)  | 18 EB (exabyte)                |
| Dimensioni massime volume | 4,7 ZB (zettabyte)                           | 256 TB                |


### <a name="functionality"></a>Funzionalità

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>In ReFS e NTFS sono disponibili le seguenti funzionalità:

| Funzionalità       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Crittografia BitLocker | Sì | Sì |
| Supporto di Volume condiviso cluster | Sì | Sì |
| Soft link | Sì | Sì |
| Supporto per cluster di failover | Sì | Sì |
| Elenchi di controllo di accesso (ACL) | Sì | Sì |
| Journal USN | Sì | Sì |
| Notifiche di modifiche | Sì | Sì |
| Punti di giunzione | Sì | Sì |
| Punti di montaggio | Sì | Sì |
| Reparse point | Sì | Sì |
| Snapshot del volume | Sì | Sì |
| ID file | Sì | Sì |
| Oplock | Sì | Sì |
| File sparse | Sì | Sì |
| Flussi denominati | Sì | Sì |

#### <a name="the-following-features-are-only-available-on-refs"></a>Le seguenti funzionalità sono disponibili solo in ReFS:

| Funzionalità       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Clonazione dei blocchi | Sì | No |
| VDL di tipo sparse | Sì | No |
| Parità accelerata con mirroring| Sì (in Spazi di archiviazione diretta) | No |

#### <a name="the-following-features-are-unavailable-on-refs-at-this-time"></a>In ReFS non sono al momento disponibili le seguenti funzionalità:

| Funzionalità       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Compressione del file system | No | Sì |
| Crittografia del file system | No | Sì |
| Deduplicazione dati | No | Sì |
| Transazioni | No | Sì |
| Collegamenti reali | No | Sì |
| ID oggetto | No | Sì |
| Nomi brevi | No | Sì |
| Attributi estesi | No | Sì |
| Quote disco | No | Sì |
| Di avvio | No | Sì |
| Supportato in supporti rimovibili | No | Sì |
| Livelli di archiviazione NTFS | No | Sì |


## <a name="see-also"></a>Vedere anche

-   [Clonazione dei blocchi ReFS](block-cloning.md)
-   [Flussi di integrità ReFS](integrity-streams.md)
-   [Panoramica di Spazi di archiviazione diretta](../storage-spaces/storage-spaces-direct-overview.md)
