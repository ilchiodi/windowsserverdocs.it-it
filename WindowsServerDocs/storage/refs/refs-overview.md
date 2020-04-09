---
title: Panoramica del file system ReFS (Resilient File System)
ms.prod: windows-server
ms.author: gawatu
manager: mchad
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 06/17/2019
ms.openlocfilehash: 8d32ef6bc4ce169ff73f9ab147783ac0607617f2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857544"
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

- La parità con accelerazione con mirroring con **[accelerazione speculare](./mirror-accelerated-parity.md)** offre prestazioni elevate e capacità di archiviazione efficiente per i dati. 

    - Per garantire prestazioni elevate e spazio di archiviazione efficiente in termini di capacità, ReFS divide un volume in due gruppi di archiviazione logica, noti come livelli. Questi livelli possono disporre di propri tipi di resilienza e unità consentendone quindi l'ottimizzazione delle prestazioni o della capacità. Alcune configurazioni di esempio sono: 
    
      | Livello prestazioni | Livello capacità |
      | ---------------- | ----------------- |
      | Unità SSD con mirroring | Unità HDD con mirroring |
      | Unità SSD con mirroring | Unità SSD con parità |
      | Unità SSD con mirroring | Unità HDD con parità |
            
    - Una volta configurati questi livelli, ReFS li utilizza per fornire un'archiviazione veloce di dati ad accesso frequente e un'archiviazione efficiente in termini di capacità per i dati ad accesso sporadico:
        - Tutte le scritture vengono eseguite nel livello prestazioni. I grandi gruppi di dati che rimangono in questo livello verranno spostati in modo efficiente nel livello capacità in tempo reale.
        - Se si usa una distribuzione ibrida (combinando unità flash e HDD), [la cache in spazi di archiviazione diretta](../storage-spaces/understand-the-cache.md) consente di accelerare le letture, riducendo l'effetto della caratteristica di frammentazione dei dati dei carichi di lavoro virtualizzati. In caso contrario, se si usa una distribuzione di tutti i flash, le letture si verificano anche nel livello di prestazioni.

> [!NOTE]
> Per le distribuzioni di Server, la parità accelerata con mirroring è supportata solo in [Spazi di archiviazione diretta](../storage-spaces/storage-spaces-direct-overview.md). È consigliabile usare la parità con accelerazione con mirroring solo con i carichi di lavoro di archiviazione e di backup. Per carichi di lavoro a prestazioni elevate e virtualizzate, è consigliabile utilizzare i mirror a tre vie per ottenere prestazioni migliori.

- **Operazioni VM con accelerazione**: ReFS introduce una nuova funzionalità destinata in particolar modo al miglioramento delle prestazioni di carichi di lavoro virtualizzati:
    - [Clonazione di blocchi](./block-cloning.md): la clonazione di blocchi consente di accelerare le operazioni di copia, consentendo rapide operazioni di unione del checkpoint VM a basso impatto.
    - VDL di tipo sparse: VDL di tipo sparse consente a ReFS di azzerare rapidamente i file, riducendo il tempo necessario per creare dischi rigidi virtuali fissi da decine di minuti a pochi secondi.

- **Dimensioni variabili del cluster**: ReFS supporta dimensioni del cluster di 4 KB e 64 KB. 4 KB è la dimensione del cluster consigliata per la maggior parte delle distribuzioni, ma i cluster di 64 KB sono appropriati per grandi carichi di lavoro I/O sequenziali.

### <a name="scalability"></a>Scalabilità

ReFS è progettato per supportare set di dati molto grandi (nell'ordine di milioni di terabyte) senza influire negativamente sulle prestazioni, ottenendo una scalabilità maggiore rispetto ai file system precedenti.

## <a name="supported-deployments"></a>Distribuzioni supportate

Microsoft ha sviluppato NTFS in modo specifico per un uso generico con un'ampia gamma di configurazioni e carichi di lavoro, tuttavia, per i clienti che richiedono in modo specifico la disponibilità, la resilienza e/o la scalabilità fornita da ReFS, Microsoft supporta ReFS per l'uso con le configurazioni e gli scenari seguenti. 

> [!NOTE]
> Tutte le configurazioni supportate da ReFS devono usare hardware certificato del [Catalogo di Windows Server](https://www.WindowsServerCatalog.com) e soddisfare i requisiti dell'applicazione.

### <a name="storage-spaces-direct"></a>Spazi di archiviazione diretta

La distribuzione di ReFS in Spazi di archiviazione diretta è consigliata per i carichi di lavoro virtualizzati o per i dispositivi NAS: 
- Parità accelerata con mirroring e [la cache in Spazi di archiviazione diretta](../storage-spaces/understand-the-cache.md) offrono prestazioni elevate e un'archiviazione efficiente in termini di capacità. 
- L'introduzione della clonazione dei blocchi e di VDL di tipo sparse accelera in modo significativo le operazioni di file .vhdx, quali la creazione, l'unione e l'espansione.
- I flussi di integrità, il ripristino in linea e le copie di dati alternative consentono a ReFS e Spazi di archiviazione diretta di eseguire congiuntamente per rilevare e correggere i danneggiamenti del controller di archiviazione e dei supporti di archiviazione all'interno di metadati e dati. 
- ReFS offre la funzionalità di ridimensionare e supportare set di dati di notevoli dimensioni. 

### <a name="storage-spaces"></a>Spazi di archiviazione

- I flussi di integrità, il ripristino in linea e le copie di dati alternativi consentono a ReFS e [spazi di archiviazione](../storage-spaces/overview.md) di rilevare e correggere i danneggiamenti del controller di archiviazione e dei supporti di archiviazione sia nei metadati che nei dati.
- Le distribuzioni di spazi di archiviazione possono inoltre utilizzare la clonazione in blocco e la scalabilità offerte in ReFS.
- La distribuzione di ReFS in spazi di archiviazione con enclosure SAS condivise è adatta per l'hosting di dati di archiviazione e l'archiviazione di documenti utente.

> [!NOTE]
> Spazi di archiviazione supporta la connessione diretta locale non rimovibile tramite BusTypes SATA, SAS, NVME o collegato tramite HBA (noto anche come controller RAID in modalità pass-through).

### <a name="basic-disks"></a>Dischi di base

La distribuzione di ReFS sui dischi Basic è più adatta per le applicazioni che implementano soluzioni di disponibilità e resilienza del software. 
- Le applicazioni che introducono le proprie soluzioni di disponibilità e resilienza del software possono sfruttare i flussi di integrità, la clonazione in blocchi e la possibilità di ridimensionare e supportare set di dati di grandi dimensioni. 

> [!NOTE]
> I dischi di base includono il collegamento diretto locale non rimovibile tramite BusTypes SATA, SAS, NVME o RAID. 

### <a name="backup-target"></a>Destinazione di backup

La distribuzione di ReFS come destinazione di backup è più adatta per le applicazioni e l'hardware che implementano soluzioni di disponibilità e resilienza.
- Le applicazioni che introducono le proprie soluzioni di disponibilità e resilienza del software possono sfruttare i flussi di integrità, la clonazione in blocchi e la possibilità di ridimensionare e supportare set di dati di grandi dimensioni.

> [!NOTE]
> Le destinazioni di backup includono le configurazioni supportate sopra. Per informazioni dettagliate sul supporto su Fibre Channel e San iSCSI, contattare i fornitori di applicazioni e array di archiviazione. Per San, se sono necessarie funzionalità quali thin provisioning, TRIM/annullare o Offloaded Trasferimento dati (ODX), è necessario usare NTFS.   

## <a name="feature-comparison"></a>Confronto delle funzionalità

### <a name="limits"></a>Limiti

| Caratteristica       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Lunghezza massima del nome del file | 255 caratteri Unicode  | 255 caratteri Unicode               |
| Lunghezza massima del nome del percorso |32.000 caratteri Unicode | 32.000 caratteri Unicode                |
| Dimensione massime file | 35 PB (petabyte)  | 256 TB               |
| Dimensioni massime volume | 35 PB                           | 256 TB                |

### <a name="functionality"></a>Funzionalità

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>In ReFS e NTFS sono disponibili le seguenti funzionalità:

| Funzionalità       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Crittografia BitLocker | Sì | Sì |
| Deduplicazione dati | Sì<sup>1</sup> | Sì |
| Supporto di Volume condiviso cluster | Sì<sup>2</sup> | Sì |
| Collegamenti simbolici | Sì | Sì |
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
| Thin provisioning | Sì<sup>3</sup> | Sì |
| Trim/annullare | Sì<sup>3</sup> | Sì |
1. Disponibile in Windows Server, versione 1709 e versioni successive.
2. Disponibile in Windows Server 2012 R2 e versioni successive.
3. Solo spazi di archiviazione

#### <a name="the-following-features-are-only-available-on-refs"></a>Le seguenti funzionalità sono disponibili solo in ReFS:

| Funzionalità       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Clonazione dei blocchi | Sì | No |
| VDL di tipo sparse | Sì | No |
| Parità accelerata con mirror| Sì (in Spazi di archiviazione diretta) | No |

#### <a name="the-following-features-are-unavailable-on-refs-at-this-time"></a>In ReFS non sono al momento disponibili le seguenti funzionalità:

| Funzionalità       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Compressione del file system | No | Sì |
| Crittografia del file system | No | Sì |
| Transazioni | No | Sì |
| Collegamenti reali | No | Sì |
| ID oggetto | No | Sì |
| Trasferimento dati Offloaded (ODX) | No | Sì |
| Nomi brevi | No | Sì |
| Attributi estesi | No | Sì |
| Quote disco | No | Sì |
| Di avvio | No | Sì |
| Supporto file di paging | No | Sì |
| Supportato in supporti rimovibili | No | Sì |

## <a name="see-also"></a>Vedere anche

- [Consigli sulle dimensioni del cluster per ReFS e NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Panoramica di Spazi di archiviazione diretta](../storage-spaces/storage-spaces-direct-overview.md)
- [Clonazione blocco ReFS](block-cloning.md)
- [Flussi di integrità ReFS](integrity-streams.md)