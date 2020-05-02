---
title: Proprietà
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9615918bb7fab45018f40b409427ab12fc3eddb7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721983"
---
# <a name="select"></a>Proprietà



Sposta lo stato attivo su un disco, una partizione, un volume o disco rigido virtuale (VHD).

## <a name="syntax"></a>Sintassi

```
select disk
select partition
select volume
select vdisk
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[Selezione disco](select-disk.md)|Sposta lo stato attivo su un disco.|
|[Selezionare una partizione](select-partition.md)|Sposta lo stato attivo a una partizione.|
|[Seleziona volume](select-volume.md)|Sposta lo stato attivo su un volume.|
|[Selezionare disco virtuale](select-vdisk.md)|Sposta lo stato attivo a un disco rigido Virtuale.|

## <a name="remarks"></a>Osservazioni

-   Se si seleziona un volume con una partizione corrispondente, verrà selezionata automaticamente la partizione.
-   Se è selezionata una partizione con un volume corrispondente, il volume verrà selezionato automaticamente.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

