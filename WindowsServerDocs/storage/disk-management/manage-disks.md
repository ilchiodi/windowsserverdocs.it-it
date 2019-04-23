---
title: Gestire dischi
description: In questo articolo viene descritto come gestire dischi
ms.date: 12/21/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f4698dac683ff3769eb4403ae2750ad38a301022
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846192"
---
# <a name="manage-disks"></a>Gestire dischi

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento e nei relativi sottoargomenti illustra l'uso del disco di gestione per gestire i dischi in un computer e include informazioni sull'inizializzazione dei nuovi dischi, la conversione di dischi tra gli stili di partizione diverse, e come Windows gestisce lo stato online dei nuovi dischi.

## <a name="online-and-offline-status"></a>Stato online e offline

Gestione disco viene visualizzato se un disco è online (disponibile), o non in linea.

In Windows, per impostazione predefinita, tutti i dischi appena individuati vengono portati online con accesso in lettura e scrittura. In Windows Server, per impostazione predefinita, i dischi appena individuati vengono portati online con accesso in lettura e scrittura a meno che non si trovino in un bus condiviso (ad esempio, SCSI, iSCSI, Serial Attached SCSI o Fibre Channel). I dischi in un bus condiviso sono offline la prima volta che vengono rilevati.

Se un disco è offline, è necessario portarlo online prima di poterlo inizializzare o creare volumi su di esso.

Per connettere un disco o portarlo offline, fare clic sul nome del disco e quindi selezionando l'azione appropriata.





## <a name="see-also"></a>Vedere anche

-   [Inizializzare nuovi dischi](initialize-new-disks.md)
-   [Spostare i dischi in un altro Computer](move-disks-to-another-computer.md)
-   [Modificare un disco dinamico in un disco di base](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Modificare un disco Master Boot Record in una tabella di partizione GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Modificare un disco GUID Partition Table in un Record di avvio principale](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gestire dischi rigidi virtuali](manage-virtual-hard-disks.md)