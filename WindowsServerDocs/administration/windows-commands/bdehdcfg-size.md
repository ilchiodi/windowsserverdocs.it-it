---
title: dimensioni BdeHdCfg
description: Windows Commands Topic for **BdeHdCfg size**, che specifica la dimensione della partizione di sistema quando viene creata una nuova unità di sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c72bdfd0b8bf4dfd0aa36885ceb7fd249a18c332
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851024"
---
# <a name="bdehdcfg-size"></a>BdeHdCfg: dimensioni

Specifica le dimensioni della partizione di sistema durante la creazione di una nuova unità di sistema.

Per un esempio di come è possibile usare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<SizeinMB>` | Indica il numero di megabyte (MB) da utilizzare per la nuova partizione. |

## <a name="remarks"></a>Note

Se non si specifica una dimensione, verrà utilizzato automaticamente il valore predefinito di 300 MB. La dimensione minima dell'unità di sistema è 100 MB. Se si prevede di archiviare Ripristino di sistema o altri strumenti sulla partizione di sistema, è necessario aumentare di conseguenza la dimensione.

> [!NOTE]
> Il comando **size** non può essere combinato con il comando `<DriveLetter>` **merge** di **destinazione** .

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo del comando **size** per allocare 500 MB all'unità di sistema predefinita.

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)