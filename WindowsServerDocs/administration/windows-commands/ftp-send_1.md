---
title: Invio FTP
description: Argomento di riferimento per il comando di invio FTP, che consente di copiare un file locale nel computer remoto utilizzando il tipo di trasferimento di file corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12ce45a0eb26e1aa4a0d7daace831751e1b67f4a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820301"
---
# <a name="ftp-send"></a>Invio FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file locale nel computer remoto utilizzando il tipo di trasferimento di file corrente.

> [!NOTE]
> Questo comando è lo stesso del [comando FTP PUT](ftp-put.md).

## <a name="syntax"></a>Sintassi

```
send <localfile> [<remotefile>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<localfile>` | Specifica il file locale da copiare. |
| `<remotefile>` | Specifica il nome da utilizzare nel computer remoto. Se non si specifica un *FileRemoto*, il file otterrà il nome *LocalFile* . |

### <a name="examples"></a>Esempi

Per copiare il file *test. txt* locale e denominarlo *test1. txt* nel computer remoto, digitare:

```
send test.txt test1.txt
```

Per copiare il file *Program. exe* locale nel computer remoto, digitare:

```
send program.exe
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
