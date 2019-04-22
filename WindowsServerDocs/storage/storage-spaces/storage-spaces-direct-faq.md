---
title: Spazi di archiviazione di diretta - domande frequenti
description: Informazioni su quanto riguarda gli spazi di archiviazione diretta
keywords: Spazi di archiviazione
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818912"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>Spazi di archiviazione di diretta - domande frequenti (FAQ)

Questo articolo elenca alcuni comuni e le domande frequenti relative a [spazi di archiviazione diretta](storage-spaces-direct-overview.md).

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>Quando si usa spazi di archiviazione diretta con 3 nodi, è possibile ottenere le prestazioni e livelli di capacità.

Sì, è possibile ottenere sia un livello di capacità e prestazioni in una configurazione di spazi di archiviazione diretta-2 o 3 nodi. Tuttavia, è necessario assicurarsi di avere 2 dispositivi di capacità. Ciò significa che è necessario usare tutti i tre tipi di dispositivi: NVME, unità SSD e HDD.
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>File system Refs fornisce tiaring in tempo reale con spazi di archiviazione diretta. REFS offre la stessa funzionalità con spazi di archiviazione condiviso nel 2016?

No, si otterranno in tempo reale la suddivisione in livelli con spazi di archiviazione condivisa 2016. Si tratta solo di spazi di archiviazione diretta. 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>È possibile usare un file system NTFS con spazi di archiviazione diretta?
  
Sì, è possibile usare il file system NTFS con spazi di archiviazione diretta. Tuttavia, è consigliabile REFS. NTFS non prevede la suddivisione in livelli in tempo reale. 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>Ho configurato spazi di archiviazione diretta cluster a 2 nodi, in cui il disco virtuale è configurato come la resilienza a 2 vie. Se aggiunge un nuovo dominio di errore, la resilienza del disco virtuale esistente, cambierà?

Dopo aver aggiunto il nuovo dominio di errore, i nuovi dischi virtuali creati passerà a 3 vie. Tuttavia, il disco virtuale esistente rimarrà un disco con mirroring a 2 vie. È possibile copiare i dati per i nuovi dischi virtuali da volumi esistenti per ottenere i vantaggi della resilienza di nuovo.
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>Spazi di archiviazione diretta è stato creato utilizzando il autoconfig:0 commutatore e il pool creato manualmente. Quando si prova a eseguire una query il pool di spazi di archiviazione diretta per creare un nuovo volume, viene visualizzato un messaggio con la dicitura "Nuovamente Enable-ClusterS2D." Cosa dovrei fare?

Per impostazione predefinita, quando si configurano spazi di archiviazione diretta usando il cmdlet enable-S2D, il cmdlet esegue tutto automaticamente. Crea il pool e i livelli. Quando si usa autoconfig:0, tutto ciò che deve essere eseguito manualmente. Se è stato creato solo il pool, il livello non vengono necessariamente creati. Se si hanno non creato in tutti i livelli o non creare livelli in modo corrispondente per i dispositivi collegati, si riceverà un messaggio di errore "Enable-ClusterS2D anche in questo caso". È consigliabile non utilizzare l'opzione di configurazione automatica in un ambiente di produzione. 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>È possibile aggiungere un disco di rotazione (HDD) al pool di spazi di archiviazione diretta, dopo aver creato gli spazi di archiviazione diretta con dispositivi SSD?

No. Per impostazione predefinita, se si usa il tipo di dispositivo singola per creare il pool, ma non sarebbe configurare dischi cache e tutti i dischi vengono usati per la capacità. È possibile aggiungere dischi NVME alla configurazione e i dischi NVME dovrebbe essere configurati per la cache.
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>Ho configurato un dominio di errore 2-rack: RACK 1 ha 2 domini di errore, 2 RACK 1 dominio di errore. Ogni server dispone di 4 dispositivi di 100 GB di capacità. È possibile usare tutti i 1.200 GB di spazio dal pool?

No, è possibile usare solo da 800 GB. In un dominio di errore di rack, è necessario assicurarsi di avere una configurazione a 2 vie, in modo che ogni Matteo e relativo ' s land duplicati in un diverso rack.
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>Ciò che la dimensione della cache è necessario quando configurare spazi di archiviazione diretta?

La cache deve essere dimensionata per contenere il working set (i dati che si sta attivamente letti o scritti in qualsiasi momento) delle applicazioni e carichi di lavoro.

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>Come è possibile determinare le dimensioni della cache che è usata da spazi di archiviazione diretta?

Usare l'utilità PerfMon incorporata per esaminare i mancati riscontri nella cache. Esaminare la cache mancati riscontri letture/sec dal contatore del disco del Cluster archiviazione ibrida. Tenere presente che se troppe operazioni di lettura sono mancanti della cache, la cache è di dimensioni insufficienti e che è possibile espanderlo. 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>È disponibile una calcolatrice che mostra le dimensioni esatte dei dischi che sono da riservare per la cache, la capacità e la resilienza che consentirebbe a pianificare meglio?

È possibile utilizzare il calcolatore di spazi di archiviazione per semplificare la pianificazione. È disponibile all'indirizzo http://aka.ms/s2dcalc.
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>Che cos'è la migliore configurazione che si consigli durante la configurazione 3 rack e 6 server?

Utilizzare 2 server in ogni rack per ottenere la resilienza del disco virtuale di un mirror a 3 vie. Tenere presente che la configurazione di rack funzionerebbe correttamente solo se si specifica la configurazione per il sistema operativo modo in cui che si trova nel rack. 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>È possibile abilitare la modalità di manutenzione per un disco specifico in un server specifico nel cluster di spazi di archiviazione diretta?

Sì, è possibile abilitare la modalità di manutenzione archiviazione su un disco specifico e un dominio di errore specifico. Il comando Enable-StorageMaintenanceMode viene richiamato automaticamente quando si posiziona un nodo. È possibile abilitarlo per uno specifico disco eseguendo il comando seguente:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>Spazi di archiviazione diretta è disponibile in my hardware?

È consigliabile contattare il fornitore dell'hardware per verificare il supporto. I fornitori di hardware testare la soluzione nel proprio hardware e un commento sulla se è supportato o non. Ad esempio, al momento di questo documento, i server, ad esempio R730 / R730xd / R630 che sono presenti più di 8 unità slot in grado di supportare servizi SES e sono compatibili con spazi di archiviazione diretta. Dell supporta solo il HBA330 con spazi di archiviazione diretta. R620 non supporta servizi SES e non è compatibile con spazi di archiviazione diretta.

Per l'hardware più informazioni sul supporto, visitare il sito seguente: Catalogo di Windows Server
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>Come spazi di archiviazione diretta apporta utilizzare SES?

Spazi di archiviazione diretta Usa mapping dei servizi SES (SCSI Enclosure) per assicurarsi che allocazioni di memoria dei dati e i metadati viene distribuito tra i domini di errore in modo resiliente. Se l'hardware non supporta servizi SES, non esiste un mapping delle enclosure e l'inserimento di dati non è resiliente.
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>Il comando che è possibile utilizzare per controllare l'ambito per un disco virtuale fisico?
  
Questa regola:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
