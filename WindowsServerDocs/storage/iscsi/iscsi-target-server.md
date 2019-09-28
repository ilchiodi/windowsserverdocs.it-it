---
title: iSCSI Target Server Overview
TOCTitle: iSCSI Target Server
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 638343abf782983020a3301898920470ffcd5952
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403056"
---
# <a name="iscsi-target-server-overview"></a>Panoramica di server di destinazione iSCSI

Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento fornisce una breve panoramica del server di destinazione iSCSI, un servizio ruolo di Windows Server che consente di rendere disponibile l'archiviazione tramite il protocollo iSCSI. Questa operazione è utile per fornire l'accesso all'archiviazione in Windows Server per i client che non riescono a comunicare tramite il protocollo di condivisione file di Windows nativo, SMB.

Server di destinazione iSCSI è la soluzione ideale nei casi seguenti:

* **Avvio di rete e senza dischi**   di tramite schede di rete che supportano l'avvio o un caricatore software, è possibile distribuire centinaia di server senza dischi. Con Server di destinazione iSCSI, la distribuzione è veloce. Nei test interni Microsoft sono stati distribuiti 256 computer in 34 minuti. Usando dischi rigidi virtuali differenze, è possibile risparmiare fino al 90% dello spazio di archiviazione usato per le immagini del sistema operativo. Questa soluzione è ideale per grandi distribuzioni di immagini di sistema operativo identiche, ad esempio su macchine virtuali che eseguono Hyper-V o in cluster HPC (High Performance Computing).

* **Archiviazione di applicazioni Server**@no__t le applicazioni 1Some richiedono l'archiviazione a blocchi. Server di destinazione iSCSI può fornire a queste applicazioni un'archiviazione a blocchi continuamente disponibile. Dato che l'archiviazione è accessibile in remoto, consente inoltre di consolidare l'archiviazione a blocchi per sedi centrali o succursali.

* **Archiviazione eterogenea**@no__t server di destinazione 1iSCSI supporta iniziatori iSCSI non Microsoft, semplificando la condivisione dell'archiviazione nei server in un ambiente software misto.

* **Ambienti di sviluppo, test, dimostrazione e lab**@no__t 1when server di destinazione iSCSI è abilitato, un computer che esegue il sistema operativo Windows Server diventa un dispositivo di archiviazione a blocchi accessibile dalla rete. Questa soluzione è utile per il testing delle applicazioni prima della distribuzione in una rete SAN (Storage Area Network).

## <a name="block-storage-requirements"></a>Requisiti di archiviazione a blocchi

Abilitando Server di destinazione iSCSI per fornire archiviazione a blocchi, è possibile sfruttare la rete Ethernet esistente. Non è necessario disporre di hardware aggiuntivo. Se la disponibilità elevata è un criterio importante, è consigliabile impostare un cluster a disponibilità elevata. È necessario disporre di archiviazione condivisa per un cluster a disponibilità elevata, sotto forma di soluzione hardware per l'archiviazione Fibre Channel o come array di archiviazione SAS (Serial Attached SCSI).

Se si abilita il clustering guest, è necessario fornire l'archiviazione a blocchi. Tutti i server che eseguono software Windows Server con Server di destinazione iSCSI possono fornire l'archiviazione a blocchi.

## <a name="see-also"></a>Vedere anche

[Archiviazione a blocchi di destinazione iSCSI, procedura](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh848268(v%3dws.11))  
[Novità di server di destinazione iSCSI in Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn305893(v%3dws.11))

