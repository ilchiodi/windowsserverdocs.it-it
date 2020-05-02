---
title: bitsadmin getreplyfilename
description: Argomento di riferimento per il comando Bitsadmin getreplyfilename, che ottiene il percorso del file che contiene il server di caricamento-risposta per il processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: daed755e0ddc045174b98a8d4f9ee84da155cba6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717598"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Ottiene il percorso del file che contiene il server di caricamento-risposta per il processo.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getreplyfilename <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare il nome file di upload-Reply per il processo denominato *myDownloadJob*:

```
bitsadmin /getreplyfilename myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
