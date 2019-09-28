---
title: Writer
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c00f6067cd5f6cf741cddbd6d62c5bcbb1f37a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361865"
---
# <a name="writer"></a>Writer



Verifica che un writer o un componente sia incluso o escluda un writer o un componente dalla procedura di backup o ripristino. Se utilizzata senza parametri, **writer** Visualizza la guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Parametri

| Parametro  |                                                                                      Descrizione                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | Verifica che il writer o il componente specificato sia incluso nella procedura di backup o di ripristino. La procedura di backup o ripristino non riuscirà se il writer o il componente non è incluso. |
|  exclude   |                                                   Esclude il writer o il componente specificato dalla procedura di backup o di ripristino.                                                    |
| [\<Writer > |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>Esempi

Per verificare un writer specificandone il GUID (per questo esempio, 4dc3bdd4-AB48-4d07-ADB0-3bee2926fd7f), digitare:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Per escludere un writer con il nome "writer di sistema", digitare:
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)