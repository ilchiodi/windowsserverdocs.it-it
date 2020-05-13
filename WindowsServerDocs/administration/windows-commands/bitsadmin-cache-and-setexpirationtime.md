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
ms.openlocfilehash: eeb1dbd0439a1a39711e2a074ada4c772b9ca016
ms.sourcegitcommit: aed942d11f1a361fc1d17553a4cf190a864d1268
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2020
ms.locfileid: "83235042"
---
# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin cache e setexpirationtime

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
