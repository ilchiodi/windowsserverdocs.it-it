---
title: Espandi vdisk
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 272714372a35f7f205b5a2e70cb2f2669b3a0634
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844894"
---
# <a name="expand-vdisk"></a>Espandi vdisk

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
> ## <a name="remarks"></a>Note
> - Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un volume e spostare lo stato attivo a esso.
>   ## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
>   Per espandere il disco rigido Virtuale selezionato 20 GB, digitare:
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Altre informazioni di riferimento
> - - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
> - [Connetti vdisk](attach-vdisk.md)
> - [compatta vdisk](compact-vdisk.md)

-   [Scollega vdisk](detach-vdisk.md)
-   [Dettagli vdisk](detail-vdisk.md)
-   [Unisci vdisk](merge-vdisk.md)
-   [Seleziona vdisk](select-vdisk.md)
-   [list_1](list_1.md)
