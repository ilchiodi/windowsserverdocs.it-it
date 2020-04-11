---
title: Bitsadmin util e enableanalyticchannel
description: Windows Commands Topic for **Bitsadmin util and enableanalyticchannel**, che Abilita o Disabilita il canale analitico client BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8ff1f835415979036fdc0f8aa637fe693e57d46
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122689"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>Bitsadmin util e enableanalyticchannel

Abilita o disabilita il canale analitico client BITS.

## <a name="syntax"></a>Sintassi

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| Parametro | Descrizione |
| --------- | ---------- |
| TRUE o FALSE | **True** attiva la convalida del contenuto per il file specificato, mentre **false** lo disattiva. |

## <a name="examples"></a>Esempi

L'esempio seguente Abilita il canale analitico client BITS.

```
C:\>bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)