---
title: Disco virtuale di tipo merge
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc20fcaf6e511bb25156996bddc3357f99195875
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437417"
---
# <a name="merge-vdisk"></a>Disco virtuale di tipo merge

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Unisce un differenziazione disco rigido virtuale (VHD) con il disco rigido virtuale padre corrispondente. Il VHD padre verrà modificato per includere le modifiche dal disco rigido virtuale differenze.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintassi
> ```
> merge vdisk depth=<n>
> ```
> ### <a name="parameters"></a>Parametri
> 
> | Parametro |                                                                                    Descrizione                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | depth=<n> | Indica il numero di file di disco rigido virtuale padre da unire. Ad esempio, **profondità = 1** indica che il disco rigido virtuale differenze verrà unito con un livello della catena di differenziazione. |
> 
> ## <a name="remarks"></a>Note
> - Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.
> - Questo parametro consente di modificare il disco rigido virtuale padre. Di conseguenza, gli altri dischi rigidi virtuali differenze che dipendono l'elemento padre non saranno validi.
>   ## <a name="BKMK_Examples"></a>Esempi
>   Per unire un disco rigido virtuale differenze con il proprio disco rigido virtuale padre, digitare:
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>Riferimenti aggiuntivi
> - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [compact vdisk](compact-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [Detach vdisk](detach-vdisk.md)
-   [expand vdisk](expand-vdisk.md)
-   [Selezionare vdisk](select-vdisk.md)
-   [list_1](list_1.md)
