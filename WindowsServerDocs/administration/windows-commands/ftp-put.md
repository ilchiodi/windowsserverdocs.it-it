---
title: FTP PUT
description: Argomento dei comandi di Windows per FTP-PUT
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: ecd579a313fe1cad1b8a5b4a622aaaec2d6a6d63
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843134"
---
# <a name="ftp-put"></a>FTP: Put

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file locale nel computer remoto utilizzando il tipo di trasferimento di file corrente.
## <a name="syntax"></a>Sintassi
```
put <LocalFile> [<remoteFile>]
```
#### <a name="parameters"></a>Parametri

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
  ## <a name="additional-references"></a>Altre informazioni di riferimento
- [FTP: ASCII](ftp-ascii.md)
- [FTP: binario](ftp-binary.md)
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
