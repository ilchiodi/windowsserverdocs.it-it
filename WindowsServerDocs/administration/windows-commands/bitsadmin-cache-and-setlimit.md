---
title: bitsadmin cache e setlimit
description: Argomento di riferimento per il comando Bitsadmin cache e selimit, che imposta il limite delle dimensioni della cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4c41102bfb87ff6d48113c4e85a821b821b5b01
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718283"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin cache e setlimit

Imposta il limite delle dimensioni della cache.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| percent | Limite della cache definito come percentuale dello spazio totale su disco rigido. |

## <a name="examples"></a>Esempi

Per impostare il limite delle dimensioni della cache su 50%:

```
bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando cache Bitsadmin](bitsadmin-cache.md)
