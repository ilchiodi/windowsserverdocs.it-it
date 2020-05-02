---
title: Espandi vdisk
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c2380045de45397888777f58e3420c75bb6915ae
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725698"
---
# <a name="expand-vdisk"></a>Espandi vdisk

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

espande un disco rigido virtuale (VHD) per le dimensioni specificate.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintassi
> ```
> expand vdisk maximum=<n>
> ```
> ### <a name="parameters"></a>Parametri
> 
> |  Parametro  |                      Descrizione                      |
> |-------------|-------------------------------------------------------|
> | massimo =<n> | Specifica la nuova dimensione per il disco rigido Virtuale in megabyte (MB). |
> 
> ## <a name="remarks"></a>Osservazioni
> - Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un volume e spostare lo stato attivo a esso.
>   ## <a name="examples"></a>Esempi
>   Per espandere il disco rigido Virtuale selezionato 20 GB, digitare:
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Riferimenti aggiuntivi
> - - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [compatta vdisk](compact-vdisk.md)

-   [Scollega disco virtuale](detach-vdisk.md)
-   [Dettagli vdisk](detail-vdisk.md)
-   [Disco virtuale di tipo merge](merge-vdisk.md)
-   [Seleziona vdisk](select-vdisk.md)
-   [list_1](list_1.md)
