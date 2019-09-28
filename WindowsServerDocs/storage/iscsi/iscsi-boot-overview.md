---
ms.assetid: 134840f3-c416-4a10-ad73-ef7855b206f7
title: Panoramica dell'avvio di destinazioni iSCSI
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 9c359c7929ff90968d16e92b1128f4fc8ebde37c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402986"
---
# <a name="iscsi-target-boot-overview"></a>Panoramica dell'avvio di destinazioni iSCSI

> Si applica a: Windows Server 2016

Server di destinazione iSCSI in Windows Server è in grado di avviare centinaia di computer da una singola immagine del sistema operativo archiviata in una posizione centralizzata. Si ottengono così miglioramenti a livello di efficienza, gestibilità, disponibilità e sicurezza.  
  
## <a name="BKMK_OVER"></a>Descrizione della funzionalità  
Utilizzando dischi rigidi virtuali differenze \(VHDs @ no__t-1, è possibile utilizzare una singola immagine del sistema operativo \(Il "immagine master" \) per avviare fino a 256 computer. Si supponga, ad esempio, di aver distribuito Windows Server con un'immagine del sistema operativo di circa 20 GB e di aver usato due unità disco con mirroring per agire come volume di avvio. Sarebbero stati necessari circa 10 TB di spazio di archiviazione per la sola l'immagine del sistema operativo per l'avvio di 256 computer. Con Server di destinazione iSCSI, verranno usati 40 GB per l'immagine di base del sistema operativo e 2 GB per i dischi rigidi virtuali differenze per ogni istanza del server, per un totale di 552 GB per le immagini del sistema operativo. Ciò significa un risparmio di oltre il 90% di spazio di archiviazione per le sole immagini del sistema operativo.  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
L'uso di un'immagine del sistema operativo controllata offre i vantaggi seguenti:  
  
**Gestione più semplice e sicura.** Alcune organizzazioni richiedono che i dati vengano protetti collocando le risorse di archiviazione in una posizione centralizzata con accesso fisico sottoposto a restrizioni. In questo scenario, i server accedono ai dati, inclusa l'immagine del sistema operativo, in remoto. Con Server di destinazione iSCSI, gli amministratori possono gestire centralmente le immagini di avvio del sistema operativo e controllare le applicazioni da usare per l'immagine master.  
  
**Distribuzione rapida.** Poiché l'immagine master viene preparata con Sysprep, quando i computer vengono avviati dall'immagine master saltano la fase di copia e installazione dei file che avviene durante l'installazione di Windows e passano direttamente alla fase di personalizzazione. Nel corso di test interni Microsoft, sono stati distribuiti 256 computer in 34 minuti.  
  
**Ripristino veloce.** Poiché le immagini del sistema operativo sono ospitate nel computer che esegue Server di destinazione iSCSI, se è necessario sostituire un client senza dischi, il nuovo computer può puntare all'immagine del sistema operativo e avviarsi immediatamente.  
  
> [!NOTE]  
> Diversi fornitori offrono una soluzione di avvio \(SAN\), che può essere usata da Server di destinazione iSCSI in Windows Server su hardware di base.  
  
## <a name="BKMK_HARD"></a>Requisiti hardware  
Server di destinazione iSCSI non richiede hardware speciale per la verifica funzionale. Nei data center con grandi distribuzioni @ no__t-0scale, la progettazione deve essere convalidata in base a hardware specifico. Per riferimento, i test interni di Microsoft hanno indicato che per una distribuzione di 256 computer è necessario 15.000 @ no__t-0RPM disks in una configurazione RAID 10 per l'archiviazione. Una larghezza di banda di 10 GB è ottimale. Una stima generale è di 60 server di avvio iSCSI per ogni scheda di rete da 1 GB.  
  
Per questo scenario non è necessaria una scheda di rete ed è possibile usare un caricatore di avvio software \(ad esempio il firmware di avvio open source iPXE\).  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
È possibile installare Server di destinazione iSCSI come parte del servizio ruolo Servizi file e iSCSI in Server Manager.

> [!NOTE]
> L'avvio di Nano Server da iSCSI (da un'implementazione Windows Server di destinazione iSCSI o un'implementazione di teze parti) non è supportato.

## <a name="see-also"></a>Vedere anche
* [Server di destinazione iSCSI](https://technet.microsoft.com/library/hh848272(v=ws.11).aspx)
* [cmdlet dell'iniziatore iSCSI](https://technet.microsoft.com/library/hh826099(v=wps.640).aspx)
* [cmdlet per server di destinazione iSCSI](https://technet.microsoft.com/library/jj612803(v=wps.630).aspx)