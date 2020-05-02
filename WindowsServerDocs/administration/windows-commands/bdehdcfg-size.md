---
title: dimensioni BdeHdCfg
description: Argomento di riferimento per il comando BdeHdCfg size, che specifica la dimensione della partizione di sistema durante la creazione di una nuova unità di sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27662a46045a8701b40697198cddf1dcb1b51c1e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718618"
---
# <a name="bdehdcfg-size"></a>BdeHdCfg: dimensioni

Specifica le dimensioni della partizione di sistema durante la creazione di una nuova unità di sistema. Se non si specifica una dimensione, verrà utilizzato automaticamente il valore predefinito di 300 MB. La dimensione minima dell'unità di sistema è 100 MB. Se si prevede di archiviare Ripristino di sistema o altri strumenti sulla partizione di sistema, è necessario aumentare di conseguenza la dimensione.

> [!NOTE]
> Il comando **size** non può essere combinato con `target <drive_letter> merge` il comando.

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink} -size <size_in_mb>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<size_in_mb>` | Indica il numero di megabyte (MB) da utilizzare per la nuova partizione. |

## <a name="examples"></a>Esempi

Per allocare 500 MB all'unità di sistema predefinita:

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
