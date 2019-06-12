---
title: Panoramica del file system ReFS (Resilient File System)
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: mchad
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 06/07/2019
ms.openlocfilehash: fed23c999c67ba81b3bbb821170a748ed5eaa7b8
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812021"
---
# <a name="resilient-file-system-refs-overview"></a>Panoramica del file system ReFS (Resilient File System)

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canale semestrale)

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

- **[Parità con accelerazione mirror](./mirror-accelerated-parity.md)**  -parità con accelerazione Mirror offre sia ad alte prestazioni e anche archiviare in modo efficiente la capacità per i dati. 

    - Per garantire prestazioni elevate e spazio di archiviazione efficiente in termini di capacità, ReFS divide un volume in due gruppi di archiviazione logica, noti come livelli. Questi livelli possono disporre di propri tipi di resilienza e unità consentendone quindi l'ottimizzazione delle prestazioni o della capacità. Alcune configurazioni di esempio sono: 
    
      | Livello prestazioni | Livello capacità |
      | ---------------- | ----------------- |
      | Unità SSD con mirroring | Unità HDD con mirroring |
      | Unità SSD con mirroring | Unità SSD con parità |
      | Unità SSD con mirroring | Unità HDD con parità |
            
    - Una volta configurati questi livelli, ReFS li utilizza per fornire un'archiviazione veloce di dati ad accesso frequente e un'archiviazione efficiente in termini di capacità per i dati ad accesso sporadico:
        - Tutte le scritture vengono eseguite nel livello prestazioni. I grandi gruppi di dati che rimangono in questo livello verranno spostati in modo efficiente nel livello capacità in tempo reale.
        - Se si usa una distribuzione ibrida (combinazione di flash e unità HDD), [cache in spazi di archiviazione diretta](../storage-spaces/understand-the-cache.md) aiuta ad accelerare le letture, riducendo l'impatto dei dati caratteristici di frammentazione virtualizzata i carichi di lavoro. In caso contrario, se si usa una distribuzione di all-flash, anche verificarsi letture nel livello di prestazioni.

> [!NOTE]
> Per le distribuzioni di Server, la parità accelerata con mirroring è supportata solo in [Spazi di archiviazione diretta](../storage-spaces/storage-spaces-direct-overview.md). È consigliabile usare la parità con accelerazione mirror con dell'archivio e backup dei carichi di lavoro solo. Per carichi di lavoro casuale virtualizzato e gli altri ad alte prestazioni, è consigliabile usare i mirror a tre vie per ottenere prestazioni migliori.

- **Operazioni VM con accelerazione**: ReFS introduce una nuova funzionalità destinata in particolar modo al miglioramento delle prestazioni di carichi di lavoro virtualizzati:
    - [Clonazione di blocchi](./block-cloning.md): la clonazione di blocchi consente di accelerare le operazioni di copia, consentendo rapide operazioni di unione del checkpoint VM a basso impatto.
    - VDL di tipo sparse: VDL di tipo sparse consente a ReFS di azzerare rapidamente i file, riducendo il tempo necessario per creare dischi rigidi virtuali fissi da decine di minuti a pochi secondi.

- **Dimensioni variabili del cluster**: ReFS supporta dimensioni del cluster di 4 KB e 64 KB. 4 KB è la dimensione del cluster consigliata per la maggior parte delle distribuzioni, ma i cluster di 64 KB sono appropriati per grandi carichi di lavoro I/O sequenziali.

### <a name="scalability"></a>Scalabilità

ReFS è progettato per supportare set di dati molto grandi (nell'ordine di milioni di terabyte) senza influire negativamente sulle prestazioni, ottenendo una scalabilità maggiore rispetto ai file system precedenti.

## <a name="supported-deployments"></a>Distribuzioni supportate

Microsoft ha sviluppato NTFS in modo specifico per l'uso per utilizzo generico con un'ampia gamma di configurazioni e i carichi di lavoro, tuttavia, per i clienti in modo speciale che richiede la disponibilità, resilienza e/o scala che fornisce riferimenti, Microsoft supporta ReFS per l'utilizzo con le configurazioni e gli scenari seguenti. 

> [!NOTE]
> Devono usare tutte le configurazioni supportate di ReFS [catalogo di Windows Server](https://www.WindowsServerCatalog.com) certified hardware e rispettare i requisiti dell'applicazione.

### <a name="storage-spaces-direct"></a>Spazi di archiviazione diretta

La distribuzione di ReFS in Spazi di archiviazione diretta è consigliata per i carichi di lavoro virtualizzati o per i dispositivi NAS: 
- Parità accelerata con mirroring e [la cache in Spazi di archiviazione diretta](../storage-spaces/understand-the-cache.md) offrono prestazioni elevate e un'archiviazione efficiente in termini di capacità. 
- L'introduzione della clonazione dei blocchi e di VDL di tipo sparse accelera in modo significativo le operazioni di file .vhdx, quali la creazione, l'unione e l'espansione.
- I flussi di integrità, il ripristino online e copie di dati alternativi abilitano ReFS e spazi di archiviazione diretta per congiuntamente per rilevare e correggere eventuali danni ai supporti di archiviazione all'interno di dati e metadati e controller di archiviazione. 
- ReFS offre la funzionalità di ridimensionare e supportare set di dati di notevoli dimensioni. 

### <a name="storage-spaces"></a>Spazi di archiviazione

- I flussi di integrità, il ripristino online e copie di dati alternativi abilitano ReFS e [spazi di archiviazione](../storage-spaces/overview.md) a congiuntamente per rilevare e correggere eventuali danni ai supporti di archiviazione all'interno di dati e metadati e controller di archiviazione.
- Le distribuzioni di spazi di archiviazione possono inoltre utilizzare la clonazione in blocco e la scalabilità offerte in ReFS.
- La distribuzione di ReFS in spazi di archiviazione con enclosure SAS condivisi è adatta per ospitare i dati di archiviazione e l'archiviazione dei documenti degli utenti.

> [!NOTE]
> Archiviazione spazi supporta locale non rimovibile collegato direttamente tramite BusTypes SATA, SAS o NVME o collegate tramite HBA (noto anche come controller RAID in modalità pass-through).

### <a name="basic-disks"></a>Dischi di base

La distribuzione di ReFS nei dischi di base è ideale per le applicazioni che implementano le proprie soluzioni software, disponibilità e resilienza. 
- Le applicazioni che introducono le proprie soluzioni di disponibilità e resilienza del software possono sfruttare i flussi di integrità, la clonazione in blocchi e la possibilità di ridimensionare e supportare set di dati di grandi dimensioni. 

> [!NOTE]
> I dischi di base includono locale non rimovibile collegato direttamente tramite BusTypes SATA, SAS, NVME o RAID. 

### <a name="backup-target"></a>Destinazione di backup

Distribuzione di ReFS come destinazione di backup è consigliabile ideale per applicazioni e hardware che implementano le proprie soluzioni di resilienza e disponibilità.
- Le applicazioni che introducono le proprie soluzioni di disponibilità e resilienza del software possono sfruttare i flussi di integrità, la clonazione in blocchi e la possibilità di ridimensionare e supportare set di dati di grandi dimensioni.

> [!NOTE]
> Destinazioni di backup includono le configurazioni supportate precedente. Per informazioni dettagliate di supporto su Fiber Channel e iSCSI SANs, contattare i produttori di array di archiviazione e dell'applicazione. Per SAN, se sono richieste, funzionalità quali thin provisioning, TRIM/UNMAP o Offloaded Data Transfer (ODX) NTFS deve essere utilizzato.   

## <a name="feature-comparison"></a>Confronto delle funzionalità

### <a name="limits"></a>Limiti

| Funzionalità       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Lunghezza massima del nome del file | 255 caratteri Unicode  | 255 caratteri Unicode               |
| Lunghezza massima del nome del percorso |32.000 caratteri Unicode | 32.000 caratteri Unicode                |
| Dimensione massime file | 35 PB (petabyte)  | 256 TB               |
| Dimensioni massime volume | 35 PB                           | 256 TB                |

### <a name="functionality"></a>Funzionalità

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>In ReFS e NTFS sono disponibili le seguenti funzionalità:

| Funzionalità       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Crittografia BitLocker | Yes | Yes |
| Deduplicazione dati | Sì<sup>1</sup> | Yes |
| Supporto di Volume condiviso cluster | Sì<sup>2</sup> | Yes |
| Soft link | Yes | Yes |
| Supporto per cluster di failover | Yes | Yes |
| Elenchi di controllo di accesso (ACL) | Yes | Yes |
| Journal USN | Yes | Yes |
| Notifiche di modifiche | Yes | Yes |
| Punti di giunzione | Yes | Yes |
| Punti di montaggio | Yes | Yes |
| Reparse point | Yes | Yes |
| Snapshot del volume | Yes | Yes |
| ID file | Yes | Yes |
| Oplock | Yes | Yes |
| File sparse | Yes | Yes |
| Flussi denominati | Yes | Yes |
| Thin provisioning | Sì<sup>3</sup> | Yes |
| Trim/Unmap | Sì<sup>3</sup> | Yes |
1. Disponibile in Windows Server, versione 1709 e successive.
2. Disponibile in Windows Server 2012 R2 e versioni successive.
3. Solo gli spazi di archiviazione

#### <a name="the-following-features-are-only-available-on-refs"></a>Le seguenti funzionalità sono disponibili solo in ReFS:

| Funzionalità       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Clonazione dei blocchi | Yes | No |
| VDL di tipo sparse | Yes | No |
| Parità accelerata con mirror| Sì (in Spazi di archiviazione diretta) | No |

#### <a name="the-following-features-are-unavailable-on-refs-at-this-time"></a>In ReFS non sono al momento disponibili le seguenti funzionalità:

| Funzionalità       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Compressione del file system | No | Yes |
| Crittografia del file system | No | Yes |
| Transazioni | No | Yes |
| Collegamenti reali | No | Yes |
| ID oggetto | No | Yes |
| ODX (ODX) | No | Yes |
| Nomi brevi | No | Yes |
| Attributi estesi | No | Yes |
| Quote disco | No | Yes |
| Di avvio | No | Yes |
| Supporto dei file di pagina | No | Yes |
| Supportato in supporti rimovibili | No | Yes |

## <a name="see-also"></a>Vedere anche

-   [Panoramica di spazi diretti di archiviazione](../storage-spaces/storage-spaces-direct-overview.md)
-   [Clonazione dei blocchi reFS](block-cloning.md)
-   [Flussi di integrità di reFS](integrity-streams.md)