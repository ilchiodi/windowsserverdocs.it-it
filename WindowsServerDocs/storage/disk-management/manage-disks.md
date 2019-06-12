---
title: Gestire dischi
description: In questo articolo viene descritto come gestire dischi
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 344dd363e970b195abe20fcb69e741c450fc7a21
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812403"
---
# <a name="manage-disks"></a>Gestire dischi

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento e nei relativi sottoargomenti illustra l'uso del disco di gestione per gestire i dischi in un computer e include informazioni sull'inizializzazione dei nuovi dischi, la conversione di dischi tra gli stili di partizione diverse, e come Windows gestisce lo stato online dei nuovi dischi.

## <a name="online-and-offline-status"></a>Stato online e offline

Gestione disco viene visualizzato se un disco è online (disponibile), o non in linea.

In Windows, per impostazione predefinita, tutti i dischi appena individuati vengono portati online con accesso in lettura e scrittura. In Windows Server, per impostazione predefinita, i dischi appena individuati vengono portati online con accesso in lettura e scrittura a meno che non si trovino in un bus condiviso (ad esempio, SCSI, iSCSI, Serial Attached SCSI o Fibre Channel). I dischi in un bus condiviso sono offline la prima volta che vengono rilevati.

Se un disco è offline, è necessario portarlo online prima di poterlo inizializzare o creare volumi su di esso.

Per connettere un disco o portarlo offline, fare clic sul nome del disco e quindi selezionando l'azione appropriata.

## <a name="see-also"></a>Vedere anche

-   [Inizializzare nuovi dischi](initialize-new-disks.md)
-   [Spostare i dischi in un altro Computer](move-disks-to-another-computer.md)
-   [Convertire nuovamente un disco dinamico in un disco di base](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Modificare un disco Master Boot Record in una tabella di partizione GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Modificare un disco GUID Partition Table in un Record di avvio principale](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gestire dischi rigidi virtuali](manage-virtual-hard-disks.md)