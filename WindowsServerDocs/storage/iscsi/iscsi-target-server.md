---
title: iSCSI Target Server Overview
TOCTitle: iSCSI Target Server
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 14dd373b71c1be2f1a4ff157b9c631530532fc80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837942"
---
# <a name="iscsi-target-server-overview"></a>Panoramica di Server di destinazione iSCSI

Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento fornisce una breve panoramica di Server di destinazione, un servizio ruolo in Windows Server che consente di rendere disponibile tramite il protocollo iSCSI archiviazione iSCSI. Ciò è utile per fornire l'accesso all'archiviazione nel server di Windows per i client che non possono comunicare tramite il protocollo SMB di condivisione file di Windows nativo.

Server di destinazione iSCSI è la soluzione ideale nei casi seguenti:

* **Avvio da rete e senza dischi**   usando schede di rete che supportano l'avvio o un caricatore software, è possibile distribuire centinaia di server senza dischi. Con Server di destinazione iSCSI, la distribuzione è veloce. Nei test interni Microsoft sono stati distribuiti 256 computer in 34 minuti. Usando dischi rigidi virtuali differenze, è possibile risparmiare fino al 90% dello spazio di archiviazione usato per le immagini del sistema operativo. Questa soluzione è ideale per grandi distribuzioni di immagini di sistema operativo identiche, ad esempio su macchine virtuali che eseguono Hyper-V o in cluster HPC (High Performance Computing).

* **Archiviazione di applicazioni server**   alcune applicazioni richiedono l'archiviazione a blocchi. Server di destinazione iSCSI può fornire a queste applicazioni un'archiviazione a blocchi continuamente disponibile. Dato che l'archiviazione è accessibile in remoto, consente inoltre di consolidare l'archiviazione a blocchi per sedi centrali o succursali.

* **Archiviazione eterogenea**   Server di destinazione iSCSI supporta iniziatori iSCSI non Microsoft, semplificando la condivisione dell'archiviazione nei server in un ambiente software misto.

* **Gli ambienti di sviluppo, test, demo e laboratorio**   quando il Server di destinazione iSCSI è abilitato, un computer che eseguono il sistema operativo Windows Server diventa un dispositivo di archiviazione blocchi accessibile dalla rete. Questa soluzione è utile per il testing delle applicazioni prima della distribuzione in una rete SAN (Storage Area Network).

## <a name="block-storage-requirements"></a>Requisiti di archiviazione a blocchi

Abilitando Server di destinazione iSCSI per fornire archiviazione a blocchi, è possibile sfruttare la rete Ethernet esistente. Non è necessario disporre di hardware aggiuntivo. Se la disponibilità elevata è un criterio importante, è consigliabile impostare un cluster a disponibilità elevata. È necessario disporre di archiviazione condivisa per un cluster a disponibilità elevata, sotto forma di soluzione hardware per l'archiviazione Fibre Channel o come array di archiviazione SAS (Serial Attached SCSI).

Se si abilita il clustering guest, è necessario fornire l'archiviazione a blocchi. Tutti i server che eseguono software Windows Server con Server di destinazione iSCSI possono fornire l'archiviazione a blocchi.

## <a name="see-also"></a>Vedere anche

[Destinazione blocco archiviazione iSCSI, procedura](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh848268(v%3dws.11))  
[What ' s New in destinazione Server iSCSI in Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn305893(v%3dws.11))

