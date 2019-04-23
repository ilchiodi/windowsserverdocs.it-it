---
title: selezionare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c3723dd414adca68c22011ef3f6be02eb6531d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889902"
---
# <a name="select"></a>selezionare



Sposta lo stato attivo su un disco, una partizione, un volume o disco rigido virtuale (VHD).

## <a name="syntax"></a>Sintassi

```
select disk
select partition
select volume
select vdisk
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[Selezionare disco](select-disk.md)|Sposta lo stato attivo su un disco.|
|[Selezionare una partizione](select-partition.md)|Sposta lo stato attivo a una partizione.|
|[Selezionare volume](select-volume.md)|Sposta lo stato attivo su un volume.|
|[Selezionare vdisk](select-vdisk.md)|Sposta lo stato attivo a un disco rigido Virtuale.|

## <a name="remarks"></a>Note

-   Se si seleziona un volume con una partizione corrispondente, verrà selezionata automaticamente la partizione.
-   Se è selezionata una partizione con un volume corrispondente, il volume verrà selezionato automaticamente.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

