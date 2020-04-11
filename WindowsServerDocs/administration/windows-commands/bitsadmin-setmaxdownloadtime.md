---
title: setmaxdownloadtime Bitsadmin
description: Windows Commands Topic for **BITSAdmin setmaxdownloadtime**, che imposta il timeout di download in secondi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f07931dfb9fabaec272384dced6d60f1335b6a94
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122916"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>setmaxdownloadtime Bitsadmin

Imposta il timeout del download in secondi.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setmaxdownloadtime <job> <timeout>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |
| timeout | Lunghezza del timeout di download, in secondi. |

## <a name="examples"></a>Esempi

Nell'esempio seguente imposta il timeout per il processo denominato *myDownloadJob* su 10 secondi.

```
C:\>bitsadmin /setmaxdownloadtime myDownloadJob 10
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)