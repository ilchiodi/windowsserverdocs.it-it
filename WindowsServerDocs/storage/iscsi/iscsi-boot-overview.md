---
ms.assetid: 134840f3-c416-4a10-ad73-ef7855b206f7
title: Panoramica dell'avvio di destinazioni iSCSI
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: MicGray-MS
manager: dongill
ms.author: micgray
ms.date: 10/11/2016
ms.openlocfilehash: 958d8a71e6fe62ec9d256be132aef4edf00942db
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="iscsi-target-boot-overview"></a>Panoramica dell'avvio di destinazioni iSCSI

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Server di destinazione iSCSI in Windows Server è in grado di avviare centinaia di computer da una singola immagine del sistema operativo archiviata in una posizione centralizzata. Si ottengono così miglioramenti a livello di efficienza, gestibilità, disponibilità e sicurezza.  
  
## <a name="BKMK_OVER"></a>Descrizione delle funzionalità  
Con i dischi rigidi virtuali differenze, è possibile usare una singola immagine del sistema operativo \("immagine master"\) per avviare fino a 256 computer. Si supponga, ad esempio, di aver distribuito Windows Server con un'immagine del sistema operativo di circa 20 GB e di aver usato due unità disco con mirroring come volume di avvio. Sarebbero stati necessari circa 10 TB di spazio di archiviazione per la sola l'immagine del sistema operativo per l'avvio di 256 computer. Con Server di destinazione iSCSI, verranno usati 40 GB per l'immagine di base del sistema operativo e 2 GB per i dischi rigidi virtuali differenze per ogni istanza del server, per un totale di 552 GB per le immagini del sistema operativo. Ciò significa un risparmio di oltre il 90% di spazio di archiviazione per le sole immagini del sistema operativo.  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
L'uso di un'immagine del sistema operativo controllata offre i vantaggi seguenti:  
  
**Gestione più semplice e sicura.** Alcune organizzazioni richiedono che i dati vengano protetti collocando le risorse di archiviazione in una posizione centralizzata con accesso fisico sottoposto a restrizioni. In questo scenario, i server accedono ai dati, inclusa l'immagine del sistema operativo, in remoto. Con Server di destinazione iSCSI, gli amministratori possono gestire centralmente le immagini di avvio del sistema operativo e controllare le applicazioni da usare per l'immagine master.  
  
**Distribuzione rapida.** Poiché l'immagine master viene preparata con Sysprep, quando i computer vengono avviati dall'immagine master saltano la fase di copia e installazione dei file che avviene durante l'installazione di Windows e passano direttamente alla fase di personalizzazione. Nel corso di test interni Microsoft, sono stati distribuiti 256 computer in 34 minuti.  
  
**Ripristino veloce.** Poiché le immagini del sistema operativo sono ospitate nel computer che esegue Server di destinazione iSCSI, se è necessario sostituire un client senza dischi, il nuovo computer può puntare all'immagine del sistema operativo e avviarsi immediatamente.  
  
> [!NOTE]  
> Diversi fornitori offrono una soluzione di avvio \(SAN\), che può essere usata da Server di destinazione iSCSI in Windows Server su hardware di base.  
  
## <a name="BKMK_HARD"></a>Requisiti hardware  
Server di destinazione iSCSI non richiede hardware speciale per la verifica funzionale. Nei datacenter con distribuzioni su larga scala, la progettazione deve essere convalidata per l'hardware specifico. Come riferimento, test interni Microsoft hanno indicato che per una distribuzione di 256 computer sono stati necessari 24 dischi a 15.000 RPM in una configurazione RAID 10 per l'archiviazione. Una larghezza di banda di 10 GB è ottimale. Una stima generale è di 60 server di avvio iSCSI per ogni scheda di rete da 1 GB.  
  
Per questo scenario non è necessaria una scheda di rete ed è possibile usare un caricatore di avvio software (ad esempio il firmware di avvio open source iPXE).  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
È possibile installare Server di destinazione iSCSI come parte del servizio ruolo Servizi file e iSCSI in Server Manager.

> [!NOTE]
> L'avvio di Nano Server da iSCSI (da un'implementazione Windows Server di destinazione iSCSI o un'implementazione di teze parti) non è supportato.

## <a name="see-also"></a>Vedere anche
* [iSCSI Target Server](https://technet.microsoft.com/library/hh848272(v=ws.11).aspx)
* [Cmdlet dell'iniziatore iSCSI](https://technet.microsoft.com/library/hh826099(v=wps.640).aspx)
* [Cmdlet del Server di destinazione iSCSI](https://technet.microsoft.com/library/jj612803(v=wps.630).aspx)