---
title: writer
description: Argomento comandi di Windows per Writer, che verifica che un writer o un componente sia incluso o escluda un writer o un componente dalla procedura di backup o ripristino.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb13de162b8e5eb8150d145a4afacccf47bb25f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828984"
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
|   verifica   | Verifica che il writer o il componente specificato sia incluso nella procedura di backup o di ripristino. La procedura di backup o ripristino non riuscirà se il writer o il componente non è incluso. |
|  exclude   |                                                   Esclude il writer o il componente specificato dalla procedura di backup o di ripristino.                                                    |
| [writer\<> |                                                                                     <Component>]                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per verificare un writer specificandone il GUID (per questo esempio, 4dc3bdd4-AB48-4d07-ADB0-3bee2926fd7f), digitare:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Per escludere un writer con il nome writer del sistema, digitare:
```
writer exclude System Writer
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)