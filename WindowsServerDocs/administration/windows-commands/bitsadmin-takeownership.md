---
title: bitsadmin takeownership
description: Argomento di riferimento per il comando Bitsadmin TakeOwnership, che consente a un utente con privilegi amministrativi di assumere la proprietà del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5369cb3fa143ebde77ae8cabf04b9a38eed5b9c5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720437"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Consente a un utente con privilegi amministrativi di assumere la proprietà del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /takeownership <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per assumere la proprietà del processo denominato *myDownloadJob*:

```
bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
