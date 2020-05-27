---
title: FTP PUT
description: Argomento di riferimento per il comando FTP PUT, che consente di copiare un file locale nel computer remoto usando il tipo di trasferimento di file corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: aee76b95ac538868122d5137958723326575eb18
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820371"
---
# <a name="ftp-put"></a>FTP PUT

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file locale nel computer remoto utilizzando il tipo di trasferimento di file corrente.

> [!NOTE]
> Questo comando è lo stesso del [comando di invio FTP](ftp-send_1.md).

## <a name="syntax"></a>Sintassi

```
put <localfile> [<remotefile>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<localfile>` | Specifica il file locale da copiare. |
| `[<remotefile>]` | Specifica il nome da utilizzare nel computer remoto. Se non si specifica un *FileRemoto*, il file fornisce il nome *LocalFile* .|

### <a name="examples"></a>Esempi

Per copiare il file *test. txt* locale e denominarlo *test1. txt* nel computer remoto, digitare:

```
put test.txt test1.txt
```

Per copiare il file *Program. exe* locale nel computer remoto, digitare:

```
put program.exe
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando binario FTP](ftp-binary.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
