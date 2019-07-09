---
title: Gestire i dischi
description: Questo articolo descrive come gestire i dischi
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 344dd363e970b195abe20fcb69e741c450fc7a21
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812403"
---
# <a name="manage-disks"></a>Gestire i dischi

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento e i relativi argomenti secondari descrivono l'uso di Gestione disco per gestire i dischi in un computer e include informazioni sull'inizializzazione di nuovi dischi, la conversione di dischi tra i diversi stili di partizione e il modo in cui Windows gestisce lo stato online dei nuovi dischi.

## <a name="online-and-offline-status"></a>Stato online e offline

Gestione disco indica se un disco è online (disponibile) o offline.

Per impostazione predefinita, in Windows tutti i dischi appena individuati vengono portati online con accesso in lettura e scrittura. Per impostazione predefinita, in Windows Server i dischi appena individuati vengono portati online con accesso in lettura e scrittura a meno che non si trovino in un bus condiviso, ad esempio SCSI, iSCSI, Serial Attached SCSI o Fibre Channel. I dischi in un bus condiviso saranno offline la prima volta che vengono rilevati.

Se un disco è offline, devi portarlo online prima di poterlo inizializzare o creare volumi su di esso.

Per portare un disco online o offline, fai clic con il pulsante destro del mouse sul nome del disco e quindi scegli l'azione appropriata.

## <a name="see-also"></a>Vedi anche

-   [Inizializzare nuovi dischi](initialize-new-disks.md)
-   [Spostare dischi in un altro computer](move-disks-to-another-computer.md)
-   [Convertire nuovamente un disco dinamico in un disco di base](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Modificare un disco MBR in disco GPT](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Modificare un disco GPT in disco MBR](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gestire dischi rigidi virtuali](manage-virtual-hard-disks.md)