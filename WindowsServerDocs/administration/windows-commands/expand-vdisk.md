---
title: Espandi vdisk
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8fb5d7b58b7aa4bc9557086c73020820cfa04284
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377288"
---
# <a name="expand-vdisk"></a>Espandi vdisk

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

espande un disco rigido virtuale (VHD) per le dimensioni specificate.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintassi
> ```
> expand vdisk maximum=<n>
> ```
> ## <a name="parameters"></a>Parametri
> 
> |  Parametro  |                      Descrizione                      |
> |-------------|-------------------------------------------------------|
> | massimo = <n> | Specifica la nuova dimensione per il disco rigido Virtuale in megabyte (MB). |
> 
> ## <a name="remarks"></a>Note
> - Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un volume e spostare lo stato attivo a esso.
>   ## <a name="BKMK_Examples"></a>Esempi
>   Per espandere il disco rigido Virtuale selezionato 20 GB, digitare:
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Riferimenti aggiuntivi
> - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
> - [Connetti vdisk](attach-vdisk.md)
> - [compatta vdisk](compact-vdisk.md)

-   [Scollega vdisk](detach-vdisk.md)
-   [Dettagli vdisk](detail-vdisk.md)
-   [Unisci vdisk](merge-vdisk.md)
-   [Seleziona vdisk](select-vdisk.md)
-   [list_1](list_1.md)
