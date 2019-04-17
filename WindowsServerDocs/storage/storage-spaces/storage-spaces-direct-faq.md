---
title: Spazi di archiviazione di diretti - domande frequenti
description: Scopri come circa spazi di archiviazione diretta
keywords: Spazi di archiviazione
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066885"
---
# Spazi di archiviazione diretta - domande frequenti domande frequenti

Questo articolo sono elencate alcune domande comuni e domande frequenti relative a [Spazi di archiviazione diretta](storage-spaces-direct-overview.md).

## Quando si usano spazi di archiviazione diretta con 3 nodi, è possibile ottenere le prestazioni e i livelli di capacità.

Sì, puoi ottenere prestazioni sia livello capacità in una configurazione di spazi di archiviazione diretta-2 o 3 nodi. Tuttavia, è necessario assicurarsi che hai 2 dispositivi di capacità. Questo significa che è necessario utilizzare tutti i tre tipi di dispositivi: NVME, SSD e HDD.
 
## File system di Refs offre tiaring in tempo reale con spazi di archiviazione diretta. REFS offre le stesse funzionalità con spazi di archiviazione condivise in 2016?

No, non otterrai il collegamento a livelli con spazi di archiviazione condivisa 2016 in tempo reale. Questo è solo per spazi di archiviazione diretta. 
 
## È possibile utilizzare un file system NTFS con spazi di archiviazione diretta?
  
Sì, è possibile utilizzare il file system NTFS con spazi di archiviazione diretta. Tuttavia, è consigliabile REFS. NTFS non fornisce il collegamento a livelli in tempo reale. 
 
## Ho configurato spazi di archiviazione diretta cluster a 2 nodi, in cui il disco virtuale è configurato come la resilienza con mirroring a 2 vie. Se aggiunge un nuovo dominio di errore, verrà modificato la resilienza del disco virtuale esistente?

Dopo aver aggiunto il nuovo dominio di errore, i nuovi dischi virtuali che crei raggiungerà mirroring a 3 vie. Tuttavia, il disco virtuale esistente rimarrà un disco con mirroring a 2 vie. Puoi copiare i dati per i nuovi dischi virtuali dai volumi esistenti per sfruttare i vantaggi della resilienza di nuovo.
 
## Lo spazi di archiviazione diretta è stato creato usando il commutatore autoconfig:0 e il pool creati manualmente. Quando si tenta di interrogare il pool di spazi di archiviazione diretta per creare un nuovo volume, viene visualizzato un messaggio indicante, "Nuovamente Enable-ClusterS2D." Cosa dovrei fare?

Per impostazione predefinita, quando è possibile configurare spazi di archiviazione diretta tramite il cmdlet enable-S2D, il cmdlet esegue automaticamente tutti gli elementi. Crea il pool e i livelli. Quando si utilizza autoconfig:0, tutto ciò che deve essere eseguita manualmente. Se hai creato solo il pool, il livello non viene necessariamente creato. Se hai creato non livelli affatto o non creati i livelli in modo da corrispondere ai dispositivi collegati, riceverai un messaggio di errore "Nuovamente Enable-ClusterS2D". Ti consigliamo di non usare il passaggio di configurazione automatica in un ambiente di produzione. 
 
## È possibile aggiungere un disco di rotazione (HDD) al pool di spazi di archiviazione diretta dopo aver creato spazi di archiviazione diretta con i dispositivi SSD?

No. Per impostazione predefinita, se usi il tipo di dispositivo singolo per creare il pool, non configura i dischi di cache e tutti i dischi sono utile usare per la capacità. È possibile aggiungere i dischi NVME alla configurazione e i dischi NVME potrebbero essere configurati per la cache.
 
## Ho configurato un dominio di errore di 2-rack: 1 RACK ha 2 domini di errore, 2 RACK 1 dominio di errore. Ogni server dispone di 4 dispositivi 100 GB di capacità. Posso usare tutte le 1200 GB di spazio dal pool di?

No, è possibile utilizzare solo 800 GB. In un dominio di errore rack, è necessario assicurarsi che hai una configurazione di mirroring a 2 vie, in modo che ogni Matteo e il relativo terra duplicato su diversi rack.
 
## Ciò che le dimensioni della cache presta quando configurare spazi di archiviazione diretta?

Dimensioni della cache dovrebbe essere tali da accogliere il working set (i dati che si sta attivamente vengono letti o scritti in un determinato momento) delle applicazioni e carichi di lavoro.

## Come posso determinare le dimensioni della cache è utilizzata da spazi di archiviazione diretta?

Utilizzare l'utilità predefinito PerfMon per esaminare i mancati riscontri della cache. Esamina la cache miss letture/sec dal contatore Cluster Storage Hybrid Disk. Ricorda che se un numero eccessivo di mancati riscontri della cache, la cache è insufficienti e vuoi per espanderla. 
 
## Esiste una calcolatrice che mostra le dimensioni esatte con i dischi che vengono ritirate per la cache, capacità e resilienza che consentirebbe me pianificare meglio?

È possibile utilizzare la Calcolatrice di spazi di archiviazione per agevolare la tua pianificazione. È disponibile in http://aka.ms/s2dcalc.
 
## Che cos'è la configurazione ottimale che consigliamo di durante la configurazione 6 server e 3 rack?

Usa 2 server su ognuno dei rack per ottenere la resilienza del disco virtuale di un mirroring a 3 vie. Ricorda che la configurazione rack funzionerà correttamente solo se stai fornendo la configurazione del sistema operativo nel modo che si trova sul rack. 
 
## È possibile abilitare la modalità di manutenzione per un disco specifico su uno specifico server in cluster di spazi di archiviazione diretta?

Sì, puoi abilitare la modalità di manutenzione archiviazione su un disco specifico e un dominio di errore specifici. Il comando Enable-StorageMaintenanceMode viene richiamato automaticamente quando si sospende un nodo. È possibile abilitare per uno specifico disco eseguendo il comando seguente:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## Spazi di archiviazione diretta è disponibile in hardware?

Ti consigliamo di contattare il fornitore dell'hardware per verificare il supporto. I fornitori di hardware testare la soluzione sul loro hardware e il commento sul se è supportato o No. Ad esempio, al momento di questo articolo, server, ad esempio R730 / R730xd / R630 che hanno più di 8 slot unità può supportare SES e sono compatibili con spazi di archiviazione diretta. Dell supporta solo la HBA330 con spazi di archiviazione diretta. R620 non supporta SES e non è compatibile con spazi di archiviazione diretta.

Informazioni di supporto per più componenti hardware, visita il sito Web seguente: catalogo di Windows Server
 
## Come si spazi di archiviazione diretta avvale di SES?

Spazi di archiviazione diretta utilizza il mapping di servizi SES (SCSI Enclosure Services) per assicurarti che siano distribuiti in tavolette dei dati e i metadati tra i domini di errore in modo resiliente. Se l'hardware non supporta SES, non esiste alcun mapping delle enclosure e il posizionamento dei dati non è flessibile.
 
## Il comando che è possibile utilizzare per controllare l'estensione fisica di un disco virtuale?
  
Questa regola:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
