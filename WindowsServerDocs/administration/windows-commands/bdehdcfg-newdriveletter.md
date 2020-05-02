---
title: newdriveletter BdeHdCfg
description: Argomento di riferimento per il comando BdeHdCfg newdriveletter, che assegna una nuova lettera di unità alla parte di un'unità utilizzata come unità di sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da09ae1469c6fc8370e6bd0f2f7a8f3efd8dc4f0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718671"
---
# <a name="bdehdcfg-newdriveletter"></a>BdeHdCfg: newdriveletter

Assegna una nuova lettera di unità alla parte di un'unità utilizzata come unità di sistema. Come procedura consigliata, è consigliabile non assegnare una lettera di unità all'unità di sistema.

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -newdriveletter <drive_letter>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---------| ----------- |
| `<drive_letter>` | Definisce la lettera di unità che verrà assegnata all'unità di destinazione specificata. |

## <a name="examples"></a>Esempi

Per assegnare l'unità predefinita, la lettera `P`di unità:

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
