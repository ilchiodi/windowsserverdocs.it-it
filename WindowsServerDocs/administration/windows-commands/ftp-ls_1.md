---
title: FTP ls
description: Argomento di riferimento per il comando FTP ls, che consente di visualizzare un elenco abbreviato di file e sottodirectory dal computer remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ae913f001c3ddffce9ff81c9c5c5fd32f436da5
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820171"
---
# <a name="ftp-ls"></a>FTP ls

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco abbreviato di file e le sottodirectory del computer remoto.

## <a name="syntax"></a>Sintassi

```
ls [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| `[<remotedirectory>]` | Specifica la directory per il quale si desidera visualizzare un elenco. Se non viene specificata alcuna directory, viene utilizzata la directory di lavoro corrente nel computer remoto. |
| `[<localfile>]` | Specifica un file locale in cui archiviare l'elenco. Se non viene specificato un file locale, i risultati vengono visualizzati sullo schermo. |

### <a name="examples"></a>Esempi

Per visualizzare un elenco abbreviato di file e sottodirectory dal computer remoto, digitare:

```
ls
```

Per ottenere un elenco abbreviato di directory di *dir1* nel computer remoto e salvarlo in un file locale denominato *dirlist. txt*, digitare:

```
ls dir1 dirlist.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
