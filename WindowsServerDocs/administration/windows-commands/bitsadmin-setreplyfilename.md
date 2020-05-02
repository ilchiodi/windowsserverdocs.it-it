---
title: bitsadmin setreplyfilename
description: Argomento di riferimento per il comando Bitsadmin setreplyfilename, che specifica il percorso del file che contiene il server di caricamento-risposta.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f0bd184db274dc915817ff3e26ae2c686190c27
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720483"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Specifica il percorso del file che contiene il server di caricamento-risposta.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setreplyfilename <job> <file_path>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| file_path | Percorso in cui inserire il server di caricamento-risposta. |

## <a name="examples"></a>Esempi

Per impostare il percorso del file di caricamento-risposta per il processo denominato *myDownloadJob*:

```
bitsadmin /setreplyfilename myDownloadJob c:\upload-reply
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
