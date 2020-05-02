---
title: bitsadmin cache e setexpirationtime
description: Argomento di riferimento per la cache Bitsadmin e il comando setexpirationtime, che imposta l'ora di scadenza della cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84679eadc750637fb720a458d9663219dc1492a4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718303"
---
> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin cache e setexpirationtime

Imposta l'ora di scadenza della cache.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| secondi | Numero di secondi fino alla scadenza della cache. |

## <a name="examples"></a>Esempi

Per impostare la scadenza della cache tra 60 secondi:

```
bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando cache Bitsadmin](bitsadmin-cache.md)
