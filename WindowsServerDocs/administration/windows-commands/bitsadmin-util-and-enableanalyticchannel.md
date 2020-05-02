---
title: bitsadmin util e enableanalyticchannel
description: Argomento di riferimento per il comando Bitsadmin util e enableanalyticchannel, che Abilita o Disabilita il canale analitico client BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1f5c8c924d1011928aca6ec1bcebd4d71abb015
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707766"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util e enableanalyticchannel

Abilita o disabilita il canale analitico client BITS.

## <a name="syntax"></a>Sintassi

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| Parametro | Descrizione |
| --------- | ---------- |
| TRUE o FALSE | **True** attiva la convalida del contenuto per il file specificato, mentre **false** lo disattiva. |

## <a name="examples"></a>Esempi

Per attivare o disattivare il canale analitico client BITS.

```
bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando util Bitsadmin](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
