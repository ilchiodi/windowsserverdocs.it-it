---
title: Espandere vdisk
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cf8fd0b05ca5baeeee4fadd670adb3169130d04e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837402"
---
# <a name="expand-vdisk"></a>Espandere vdisk

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

si espande un disco rigido virtuale (VHD) per le dimensioni specificate.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.
## <a name="syntax"></a>Sintassi
```
expand vdisk maximum=<n>
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|numero massimo =<n>|Specifica la nuova dimensione per il disco rigido Virtuale in megabyte (MB).|
## <a name="remarks"></a>Note
-   Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un volume e spostare lo stato attivo a esso.
## <a name="BKMK_Examples"></a>Esempi
Per espandere il disco rigido Virtuale selezionato 20 GB, digitare:
```
expand vdisk maximum=20000
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)
-   [compact vdisk](compact-vdisk.md)

-   [Detach vdisk](detach-vdisk.md)
-   [detail vdisk](detail-vdisk.md)
-   [Merge vdisk](merge-vdisk.md)
-   [Selezionare vdisk](select-vdisk.md)
-   [list_1](list_1.md)
