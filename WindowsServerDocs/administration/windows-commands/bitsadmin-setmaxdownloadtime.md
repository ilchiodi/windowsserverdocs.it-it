---
title: bitsadmin setmaxdownloadtime
description: Argomento di riferimento per il comando Bitsadmin setmaxdownloadtime, che imposta il timeout di download in secondi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8192826570c9dae6aa9d286596336c3e589c9cbd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719683"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime

Imposta il timeout del download in secondi.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setmaxdownloadtime <job> <timeout>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| timeout | Lunghezza del timeout di download, in secondi. |

## <a name="examples"></a>Esempi

Per impostare il timeout per il processo denominato *myDownloadJob* su 10 secondi.

```
bitsadmin /setmaxdownloadtime myDownloadJob 10
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
