---
title: MLS FTP
description: Argomento di riferimento per il comando FTP MLS, che consente di visualizzare un elenco abbreviato di file e sottodirectory in una directory remota.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 372c0b9a42fdfb8600083a301b71c37ada43c014
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820394"
---
# <a name="ftp-mls"></a>MLS FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco abbreviato di file e sottodirectory in una directory remota.

## <a name="syntax"></a>Sintassi

```
mls <remotefile>[ ] <localfile>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<remotefile>` | Specifica il file per il quale si desidera visualizzare un elenco. Quando si specifica *remoteFiles*, utilizzare un trattino per rappresentare la directory di lavoro corrente nel computer remoto. |
| `<localfile>` | Specifica un file locale in cui archiviare l'elenco. Quando si specifica *LocalFile*, usare un trattino per visualizzare l'elenco sullo schermo. |

### <a name="examples"></a>Esempi

Per visualizzare un elenco abbreviato di file e sottodirectory per *dir1* e *dir2*, digitare:

```
mls dir1 dir2 -
```

Per salvare un elenco abbreviato di file e sottodirectory per *dir1* e *dir2* nel file locale *dirlist. txt*, digitare:

```
mls dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
