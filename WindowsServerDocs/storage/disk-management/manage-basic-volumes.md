---
title: Gestire volumi di base
description: Questo articolo descrive come gestire volumi di base
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c75d887a6427673319999522b890d523f4276871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870962"
---
# <a name="manage-basic-volumes"></a>Gestire volumi di base

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un disco di base è un disco fisico contenente partizioni primarie, partizioni estese o unità logiche. Le partizioni e le unità logiche nei dischi di base sono note come volumi di base. È possibile creare volumi di base solo su dischi di base.

È possibile aggiungere spazio a unità logiche e partizioni primarie esistenti estendendole nello spazio adiacente, contiguo non allocato sullo stesso disco. Per estendere un volume di base, deve essere formattato con il file system NTFS. È possibile estendere un'unità logica all'interno di spazio libero contiguo nella partizione estesa che lo contiene. Se si estende un'unità logica oltre lo spazio libero disponibile nella partizione estesa, la partizione estesa cresce per contenere l'unità logica, purché la partizione estesa venga seguita da spazio non allocato contiguo.

## <a name="see-also"></a>Vedere anche

-   [Assegnare un percorso cartella del punto di montaggio in un'unità](assign-a-mount-point-folder-path-to-a-drive.md)
-   [Estendere un Volume di base](extend-a-basic-volume.md)
-   [Compattazione di un Volume di base](shrink-a-basic-volume.md)