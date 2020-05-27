---
title: Aggiunta FTP
description: Argomento di riferimento per il comando FTP Append, che aggiunge un file locale a un file nel computer remoto utilizzando l'impostazione del tipo di file corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d1b6ab4a6ae0c1654d4335d24f135b2893bdcb7
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819141"
---
# <a name="ftp-append"></a>Aggiunta FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge un file locale in un file nel computer remoto utilizzando il tipo di file corrente.

## <a name="syntax"></a>Sintassi

```
append <localfile> [remotefile]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<localfile>` | Specifica il file locale da aggiungere. |
| fileremoto | Specifica il file nel computer remoto a cui <localfile> viene aggiunto. Se non si usa questo parametro, il `<localfile>` nome viene usato al posto del nome del file remoto. |

### <a name="examples"></a>Esempi

Per aggiungere *file1. txt* a *file2. txt* nel computer remoto, digitare:

```
append file1.txt file2.txt
```

Per aggiungere il file *file1. txt* locale a un file denominato *file1. txt* nel computer remoto.

```
append file1.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
