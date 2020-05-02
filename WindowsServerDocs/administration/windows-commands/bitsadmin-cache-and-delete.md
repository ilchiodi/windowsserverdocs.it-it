---
title: bitsadmin cache e delete
description: Argomento di riferimento per la cache Bitsadmin e il comando DELETE, che elimina una voce della cache specifica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62c0c3d5b2cc188e8a8987c7ca502cdeaf932410
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718452"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin cache e delete

Elimina una voce della cache specifica.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| recordID | GUID associato alla voce della cache. |

## <a name="examples"></a>Esempi

Per eliminare la voce della cache con il RecordID di {6511FB02-E195-40A2-B595-E8E2F8F47702}:

```
bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando cache Bitsadmin](bitsadmin-cache.md)
