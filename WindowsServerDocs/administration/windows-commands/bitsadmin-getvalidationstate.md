---
title: bitsadmin getvalidationstate
description: Argomento di riferimento per il comando Bitsadmin getvalidationstate, che segnala lo stato di convalida del contenuto del file specificato all'interno del processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca753b20a1b7834d2e05d4ff8729a08332256f8c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717455"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate

Segnala lo stato di convalida del contenuto del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getvalidationstate <job> <file_index>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| file_index | Inizia da 0. |

## <a name="examples"></a>Esempi

Per recuperare lo stato di convalida del contenuto del file 2 nel processo denominato *myDownloadJob*:

```
bitsadmin /getvalidationstate myDownloadJob 1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
