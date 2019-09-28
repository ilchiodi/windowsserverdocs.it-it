---
title: compatta vdisk
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7efd40fa4b822636eda9f4082b5f561b452d3846
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379296"
---
# <a name="compact-vdisk"></a>compatta vdisk

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Riduce le dimensioni fisiche di un file di disco rigido virtuale (VHD) ad espansione dinamica. Questo parametro è utile perché i dischi rigidi virtuali aumento delle dimensioni a espansione dinamica è possibile aggiungere file, ma non automaticamente riducono le dimensioni quando si eliminano file.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintassi
> ```
> compact vdisk
> ```
> ## <a name="remarks"></a>Note
> - Per eseguire questa operazione, è necessario selezionare un disco rigido Virtuale ad espansione dinamica. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.
> - È solo possibile compattare i dischi rigidi virtuali che vengono scollegati o collegati in sola lettura a espansione dinamica.
>   ## <a name="BKMK_Examples"></a>Esempi
>   Per compattare un disco rigido Virtuale ad espansione dinamica, digitare:
>   ```
>   compact vdisk
>   ```
>   ## <a name="additional-references"></a>Riferimenti aggiuntivi
> - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
> - [Connetti vdisk](attach-vdisk.md)

-   [Dettagli vdisk](detail-vdisk.md)
-   [Scollega vdisk](detach-vdisk.md)
-   [Espandi vdisk](expand-vdisk.md)
-   [Unisci vdisk](merge-vdisk.md)
-   [Seleziona vdisk](select-vdisk.md)
-   [list_1](list_1.md)
