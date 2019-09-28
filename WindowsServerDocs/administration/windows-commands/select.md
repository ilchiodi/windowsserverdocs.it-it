---
title: selezionare
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7dc3bc8775f971968f096ba4344348e77c112cfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384115"
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
|[Selezione disco](select-disk.md)|Sposta lo stato attivo su un disco.|
|[Seleziona partizione](select-partition.md)|Sposta lo stato attivo a una partizione.|
|[Seleziona volume](select-volume.md)|Sposta lo stato attivo su un volume.|
|[Seleziona vdisk](select-vdisk.md)|Sposta lo stato attivo a un disco rigido Virtuale.|

## <a name="remarks"></a>Note

-   Se si seleziona un volume con una partizione corrispondente, verrà selezionata automaticamente la partizione.
-   Se è selezionata una partizione con un volume corrispondente, il volume verrà selezionato automaticamente.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

