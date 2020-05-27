---
title: dir FTP
description: Argomento di riferimento per il comando FTP dir, che visualizza un elenco di file di directory e sottodirectory in un computer remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a120691964b7303cf3241ffef2f11d81573ba4d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819861"
---
# <a name="ftp-dir"></a>dir FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di file di directory e sottodirectory in un computer remoto.

## <a name="syntax"></a>Sintassi

```
dir [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| `[<remotedirectory>]` | Specifica la directory per il quale si desidera visualizzare un elenco. Se non viene specificata alcuna directory, viene utilizzata la directory di lavoro corrente nel computer remoto. |
| `[<localfile>]` | Specifica un file locale in cui archiviare la visualizzazione della directory. Se non viene specificato un file locale, i risultati vengono visualizzati sullo schermo. |

### <a name="examples"></a>Esempi

Per visualizzare un elenco di directory per *dir1* nel computer remoto, digitare:

```
dir dir1
```

Per salvare un elenco della directory corrente nel computer remoto nel file locale *dirlist. txt*, digitare:

```
dir . dirlist.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
