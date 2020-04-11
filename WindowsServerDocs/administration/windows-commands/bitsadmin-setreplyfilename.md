---
title: bitsadmin setreplyfilename
description: Windows Commands Topic for **BITSAdmin setreplyfilename**, che specifica il percorso del file che contiene il server upload-Reply.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c476073cb22ff66bcefc75a45fcd0526cdf3d25
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122732"
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
| lavoro | Nome visualizzato o GUID del processo. |
| file_path | Percorso in cui inserire il server di caricamento-risposta. |

## <a name="examples"></a>Esempi

Nell'esempio seguente viene impostato il percorso del file di caricamento-risposta per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /setreplyfilename myDownloadJob c:\upload-reply
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)