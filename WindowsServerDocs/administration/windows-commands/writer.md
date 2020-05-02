---
title: writer
description: Argomento di riferimento per Writer, che verifica che un writer o un componente sia incluso o escluda un writer o un componente dalla procedura di backup o ripristino.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aed202ac774b17041f48df24333565727b110c53
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720636"
---
# <a name="writer"></a>writer



Verifica che un writer o un componente sia incluso o escluda un writer o un componente dalla procedura di backup o ripristino. Se utilizzata senza parametri, **writer** Visualizza la guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

### <a name="parameters"></a>Parametri

| Parametro  |                                                                                      Descrizione                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | Verifica che il writer o il componente specificato sia incluso nella procedura di backup o di ripristino. La procedura di backup o ripristino non riuscirà se il writer o il componente non è incluso. |
|  exclude   |                                                   Esclude il writer o il componente specificato dalla procedura di backup o di ripristino.                                                    |
| [\<Writer> |                                                                                     <Component>]                                                                                      |

## <a name="examples"></a>Esempi

Per verificare un writer specificandone il GUID (per questo esempio, 4dc3bdd4-AB48-4d07-ADB0-3bee2926fd7f), digitare:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Per escludere un writer con il nome writer del sistema, digitare:
```
writer exclude System Writer
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)