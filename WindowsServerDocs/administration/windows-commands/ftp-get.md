---
title: FTP Get
description: Argomento di riferimento per il comando FTP get, che consente di copiare un file remoto nel computer locale utilizzando il tipo di trasferimento di file corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7254cac15afc446695f22ee1a63f2f4573d3565
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819711"
---
# <a name="ftp-get"></a>FTP Get

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file remoto nel computer locale utilizzando il tipo di trasferimento di file corrente.

> [!NOTE]
> Questo comando è identico al [comando FTP ricezione](ftp-recv.md).

## <a name="syntax"></a>Sintassi

```
get <remotefile> [<localfile>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<remotefile>` | Specifica il file remoto da copiare. |
| `[<localfile>]` | Specifica il nome del file da utilizzare nel computer locale. Se *LocalFile* non è specificato, al file viene assegnato il nome *FileRemoto*. |

### <a name="examples"></a>Esempi

Per copiare *test. txt* nel computer locale utilizzando il trasferimento di file corrente, digitare:

```
get test.txt
```

Per copiare *test. txt* nel computer locale come *test1. txt* usando il trasferimento di file corrente, digitare:

```
get test.txt test1.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando ricezione FTP](ftp-recv.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando binario FTP](ftp-binary.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
