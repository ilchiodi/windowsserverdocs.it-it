---
title: Disco virtuale di tipo merge
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1315b82510ae356f80d5b519c0676d0d156ab9fd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839624"
---
# <a name="merge-vdisk"></a>Disco virtuale di tipo merge

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
> | Depth =<n> | Indica il numero di file VHD padre da unire insieme. Ad esempio, **depth = 1** indica che il disco rigido virtuale differenze verrà unito a un livello della catena di differenziazione. |
> 
> ## <a name="remarks"></a>Note
> - Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.
> - Questo parametro modifica il VHD padre. Di conseguenza, altri dischi rigidi virtuali differenze che dipendono dall'elemento padre non saranno più validi.
>   ## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
>   Per unire un disco rigido virtuale differenze con il VHD padre, digitare:
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>Altre informazioni di riferimento
> - - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
> - [Connetti vdisk](attach-vdisk.md)
> - [compatta vdisk](compact-vdisk.md)

-   [Dettagli vdisk](detail-vdisk.md)
-   [Scollega vdisk](detach-vdisk.md)
-   [Espandi vdisk](expand-vdisk.md)
-   [Seleziona vdisk](select-vdisk.md)
-   [list_1](list_1.md)
