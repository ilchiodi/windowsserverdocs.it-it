---
title: FTP PUT
description: Argomento dei comandi di Windows per FTP-PUT
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: 019a81364dbedb443a3a23d5c5a6f8db1496d83d
ms.sourcegitcommit: 479ad84a0d6c7c7b8308122b8bac8308cb36fe9b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391720"
---
# <a name="ftp-put"></a>FTP: Put

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file locale nel computer remoto utilizzando il tipo di trasferimento di file corrente.
## <a name="syntax"></a>Sintassi
```
put <LocalFile> [<remoteFile>]
```
### <a name="parameters"></a>Parametri

|    Parametro     |                    Descrizione                    |
|------------------|---------------------------------------------------|
|   `<LocalFile>`  |         Specifica il file locale da copiare.         |
| `[<remoteFile>]` | Specifica il nome da utilizzare nel computer remoto. |

## <a name="remarks"></a>Note
- Il comando **put** è identico al comando **Send** .
- Se *FileRemoto* non è specificato, al file viene assegnato il nome *LocalFile* .
  ## <a name="examples"></a><a name="BKMK_Examples"></a>Esempi
  Copiare il file **test. txt** locale e denominarlo **test1. txt** nel computer remoto.
  ```
  put test.txt test1.txt
  ```
  Copiare il file locale **Program. exe** nel computer remoto.
  ```
  put program.exe
  ```
  ## <a name="additional-references"></a>riferimenti aggiuntivi
- [FTP: ASCII](ftp-ascii.md)
- [FTP: binario](ftp-binary.md)
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
