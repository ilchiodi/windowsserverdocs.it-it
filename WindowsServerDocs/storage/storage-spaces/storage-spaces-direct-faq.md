---
title: Domande frequenti su Spazi di archiviazione diretta
description: Scopri come Spazi di archiviazione diretta
keywords: Spazi di archiviazione
ms.prod: windows-server
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: df9dac8c761a83a13fb937a99cba3697dce95201
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402799"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>Domande frequenti su Spazi di archiviazione diretta

In questo articolo vengono elencate alcune domande frequenti e frequenti relative a [spazi di archiviazione diretta](storage-spaces-direct-overview.md).

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>Quando si usa Spazi di archiviazione diretta con 3 nodi, è possibile ottenere livelli di prestazioni e capacità?

Sì, è possibile ottenere un livello di prestazioni e capacità in una configurazione di Spazi di archiviazione diretta a 2 o 3 nodi. Tuttavia, è necessario assicurarsi di disporre di 2 dispositivi Capacity. Ciò significa che è necessario usare tutti e tre i tipi di dispositivi: NVME, SSD e HDD.
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>Refs file system fornisce tiaring in tempo reale con Spazi di archiviazione diretta. REFS fornisce la stessa funzionalità con spazi di archiviazione condivisi in 2016?

No, non si otterrà la suddivisione in livelli in tempo reale con spazi di archiviazione condivisi con 2016. Questa operazione è solo per Spazi di archiviazione diretta. 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>È possibile utilizzare un file system NTFS con Spazi di archiviazione diretta?
  
Sì, è possibile usare il file system NTFS con Spazi di archiviazione diretta. È tuttavia consigliabile usare REFS. NTFS non fornisce la suddivisione in livelli in tempo reale. 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>Sono stati configurati 2 nodi Spazi di archiviazione diretta cluster, in cui il disco virtuale è configurato come resilienza del mirroring a 2 vie. Se si aggiunge un nuovo dominio di errore, la resilienza del disco virtuale esistente cambierà?

Dopo aver aggiunto il nuovo dominio di errore, i nuovi dischi virtuali creati passeranno al mirroring a tre vie. Il disco virtuale esistente, tuttavia, rimarrà un disco con mirroring a 2 vie. È possibile copiare i dati nei nuovi dischi virtuali dai volumi esistenti per sfruttare i vantaggi della nuova resilienza.
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>Il Spazi di archiviazione diretta è stato creato con la configurazione automatica: 0 passare a e il pool creato manualmente. Quando si tenta di eseguire una query nel pool di Spazi di archiviazione diretta per creare un nuovo volume, viene visualizzato un messaggio che indica che "Enable-ClusterS2D di nuovo". Cosa dovrei fare?

Per impostazione predefinita, quando si configura Spazi di archiviazione diretta usando il cmdlet Enable-S2D, il cmdlet esegue tutte le operazioni necessarie. Crea il pool e i livelli. Quando si usa la configurazione automatica: 0, è necessario eseguire tutte le operazioni manualmente. Se è stato creato solo il pool, il livello non viene necessariamente creato. Si riceverà un messaggio di errore "Enable-ClusterS2D di nuovo" se non sono stati creati livelli in tutti i livelli o non sono stati creati in un modo corrispondente ai dispositivi collegati. Si consiglia di non utilizzare l'opzione di configurazione automatica in un ambiente di produzione. 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>È possibile aggiungere un disco rotante (HDD) al pool di Spazi di archiviazione diretta dopo aver creato Spazi di archiviazione diretta con i dispositivi SSD?

No. Per impostazione predefinita, se si usa il tipo di dispositivo singolo per creare il pool, non verranno configurati i dischi della cache e tutti i dischi verrebbero usati per la capacità. È possibile aggiungere dischi NVME alla configurazione e i dischi NVME sono configurati per la cache.
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>Ho configurato un dominio di errore a 2 rack: RACK 1 ha 2 domini di errore, RACK 2 ha 1 dominio di errore. Ogni server dispone di 4 dispositivi con capacità di 100 GB. È possibile usare tutti i 1.200 GB di spazio del pool?

No, è possibile usare solo 800 GB. In un dominio di errore del rack è necessario assicurarsi di disporre di una configurazione con mirroring a 2 vie, in modo che ogni mandrino e il relativo terreno duplicato si trovino in un rack diverso.
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>Quali sono le dimensioni della cache quando si configurano Spazi di archiviazione diretta?

La cache deve essere dimensionata in modo da contenere i working set (i dati che vengono letti attivamente o scritti in un determinato momento) delle applicazioni e dei carichi di lavoro.

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>Come è possibile determinare la dimensione della cache utilizzata da Spazi di archiviazione diretta?

Utilizzare l'utilità PerfMon predefinita per esaminare i mancati riscontri nella cache. Esaminare le letture del mancato riscontro nella cache al secondo dal contatore del disco ibrido di archiviazione cluster. Tenere presente che se nella cache mancano troppe letture, la cache è sottodimensionata e potrebbe essere necessario espanderla. 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>Esiste una calcolatrice che mostra le dimensioni esatte dei dischi accantonati per la cache, la capacità e la resilienza che mi consentirebbe di pianificare meglio?

È possibile usare il calcolatore di spazi di archiviazione per semplificare la pianificazione. È disponibile all'indirizzo http://aka.ms/s2dcalc.
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>Qual è la configurazione migliore consigliata quando si configurano 6 Server e 3 rack?

Usare 2 server in ogni rack per ottenere la resilienza dei dischi virtuali di un mirroring a 3 vie. Tenere presente che la configurazione del rack funziona correttamente solo se si fornisce la configurazione al sistema operativo nel modo in cui viene inserita nel rack. 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>È possibile abilitare la modalità di manutenzione per un disco specifico in un server specifico in Spazi di archiviazione diretta cluster?

Sì, è possibile abilitare la modalità di manutenzione dell'archiviazione su un disco specifico e un dominio di errore specifico. Il comando Enable-StorageMaintenanceMode viene richiamato automaticamente quando si sospende un nodo. È possibile abilitarlo per un disco specifico eseguendo il comando seguente:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>Spazi di archiviazione diretta è supportato nell'hardware?

Si consiglia di contattare il fornitore dell'hardware per verificare il supporto. I fornitori di hardware testano la soluzione sull'hardware e commentano se sono supportati o meno. Al momento della stesura di questo articolo, ad esempio, i server come R730/R730xd/R630 con più di 8 slot di unità possono supportare SES e sono compatibili con Spazi di archiviazione diretta. Dell supporta solo HBA330 con Spazi di archiviazione diretta. R620 non supporta SES e non è compatibile con Spazi di archiviazione diretta.

Per ulteriori informazioni sul supporto hardware, visitare il sito Web seguente: Catalogo di Windows Server
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>In che modo Spazi di archiviazione diretta utilizza SES?

Spazi di archiviazione diretta usa il mapping dei Servizi Enclosure SCSI (SES) per assicurarsi che le solette di dati e i metadati vengano distribuiti tra i domini di errore in modo resiliente. Se l'hardware non supporta SES, non esiste alcun mapping delle enclosure e la posizione dei dati non è resiliente.
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>Quale comando è possibile usare per controllare l'extent fisico per un disco virtuale?
  
Questo:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
