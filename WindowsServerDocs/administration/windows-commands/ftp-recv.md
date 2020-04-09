---
title: ricezione FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fec409741e00bb3e6f61808630e5141ce4ec78f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842964"
---
# <a name="ftp-recv"></a>FTP: ricezione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file remoto nel computer locale utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
recv <remoteFile> [<LocalFile>]  
```  
#### <a name="parameters"></a>Parametri  

|   Parametro   |                   Descrizione                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Specifica il file remoto da copiare.        |
| [<LocalFile>] | Specifica il nome da utilizzare nel computer locale. |

## <a name="remarks"></a>Note  
- Il comando **ricezione** è identico al comando **Get** .  
- Se *LocalFile* non è specificato, al file viene assegnato il nome *FileRemoto* .  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
  copiare **test. txt** nel computer locale usando il tipo di trasferimento di file corrente.  
  ```  
  recv test.txt  
  ```  
  copiare **test. txt** nel computer locale come **test1. txt** usando il tipo di trasferimento di file corrente.  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>Altre informazioni di riferimento  
- [FTP: ASCII](ftp-ascii.md)  
- [FTP: binario](ftp-binary.md)  
- [FTP: Get](ftp-get.md)  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
