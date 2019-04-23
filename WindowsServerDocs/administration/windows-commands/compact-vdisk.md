---
title: compact vdisk
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba04bdc8c469ef80853b6092b8745defa1f3f19e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877302"
---
# <a name="compact-vdisk"></a>compact vdisk

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Riduce le dimensioni fisiche di un file di disco rigido virtuale (VHD) ad espansione dinamica. Questo parametro è utile perché i dischi rigidi virtuali aumento delle dimensioni a espansione dinamica è possibile aggiungere file, ma non automaticamente riducono le dimensioni quando si eliminano file.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.
## <a name="syntax"></a>Sintassi
```
compact vdisk
```
## <a name="remarks"></a>Note
-   Per eseguire questa operazione, è necessario selezionare un disco rigido Virtuale ad espansione dinamica. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.
-   È solo possibile compattare i dischi rigidi virtuali che vengono scollegati o collegati in sola lettura a espansione dinamica.
## <a name="BKMK_Examples"></a>Esempi
Per compattare un disco rigido Virtuale ad espansione dinamica, digitare:
```
compact vdisk
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [Detach vdisk](detach-vdisk.md)
-   [expand vdisk](expand-vdisk.md)
-   [Merge vdisk](merge-vdisk.md)
-   [Selezionare vdisk](select-vdisk.md)
-   [list_1](list_1.md)
