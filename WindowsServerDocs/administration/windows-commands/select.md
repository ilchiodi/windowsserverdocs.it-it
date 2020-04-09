---
title: select
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9ad7051978f4b509f54bf783f71943b65617bc7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834654"
---
# <a name="select"></a>select



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
|[Seleziona partizione](select-partition.md)|Sposta lo stato attivo a una partizione.|
|[Seleziona volume](select-volume.md)|Sposta lo stato attivo su un volume.|
|[Seleziona vdisk](select-vdisk.md)|Sposta lo stato attivo a un disco rigido Virtuale.|

## <a name="remarks"></a>Note

-   Se si seleziona un volume con una partizione corrispondente, verrà selezionata automaticamente la partizione.
-   Se è selezionata una partizione con un volume corrispondente, il volume verrà selezionato automaticamente.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

