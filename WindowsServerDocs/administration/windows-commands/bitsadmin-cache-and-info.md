---
title: bitsadmin cache e info
description: Argomento di riferimento per la cache Bitsadmin e il comando info, che consente di scaricare una voce della cache specifica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a50e6575a5496ff9f7bcd6a0dc429c7960c6933
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718351"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin cache e info

Dump di una voce della cache specifica.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>Parametri

| Paramreter | Descrizione |
| -------------- | -------------- |
| recordID | GUID associato alla voce della cache. |

## <a name="examples"></a>Esempi

Per eseguire il dump della voce della cache con il valore recordID di {6511FB02-E195-40A2-B595-E8E2F8F47702}:

```
bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando cache Bitsadmin](bitsadmin-cache.md)
