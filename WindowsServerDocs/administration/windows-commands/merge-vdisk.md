---
title: Disco virtuale di tipo merge
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bfcdde34d2c7dd6146222d04e982aa1ec8009c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723996"
---
# <a name="merge-vdisk"></a>Disco virtuale di tipo merge

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Unisce un disco rigido virtuale differenze (VHD) con il VHD padre corrispondente. Il VHD padre verrà modificato in modo da includere le modifiche dal disco rigido virtuale differenze.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintassi
> ```
> merge vdisk depth=<n>
> ```
> #### <a name="parameters"></a>Parametri
> 
> | Parametro |                                                                                    Descrizione                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | profondità =<n> | Indica il numero di file VHD padre da unire insieme. Ad esempio, **depth = 1** indica che il disco rigido virtuale differenze verrà unito a un livello della catena di differenziazione. |
> 
> ## <a name="remarks"></a>Osservazioni
> - Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.
> - Questo parametro modifica il VHD padre. Di conseguenza, altri dischi rigidi virtuali differenze che dipendono dall'elemento padre non saranno più validi.
>   ## <a name="examples"></a>Esempi
>   Per unire un disco rigido virtuale differenze con il VHD padre, digitare:
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>Riferimenti aggiuntivi
> - - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [compatta vdisk](compact-vdisk.md)

-   [Dettagli vdisk](detail-vdisk.md)
-   [Scollega disco virtuale](detach-vdisk.md)
-   [Espandi vdisk](expand-vdisk.md)
-   [Seleziona vdisk](select-vdisk.md)
-   [list_1](list_1.md)
