---
title: bitsadmin setvalidationstate
description: Argomento di riferimento per il comando Bitsadmin setvalidationstate, che imposta lo stato di convalida del contenuto del file specificato all'interno del processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3f22dc09eb1f70ce3c1ebd80fd6ba721e864377
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720458"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate

Imposta lo stato di convalida del contenuto del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setvalidationstate <job> <file_index> <TRUE|FALSE>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| Processo | Nome visualizzato o GUID del processo. |
| file_index | Inizia da 0. |
| TRUE o FALSE | **True** attiva la convalida del contenuto per il file specificato, mentre **false** lo disattiva. |

## <a name="examples"></a>Esempi

Per impostare lo stato di convalida del contenuto del file 2 su TRUE per il processo denominato *myDownloadJob*:

```
bitsadmin /setvalidationstate myDownloadJob 2 TRUE
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
