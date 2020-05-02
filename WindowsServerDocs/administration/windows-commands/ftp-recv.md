---
title: ricezione FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ece259f2d48e18f6a789d51b1df7089490f2fa1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725121"
---
# <a name="ftp-recv"></a>FTP: ricezione

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="remarks"></a>Osservazioni  
- Il comando **ricezione** è identico al comando **Get** .  
- Se *LocalFile* non è specificato, al file viene assegnato il nome *FileRemoto* .  
  ## <a name="examples"></a>Esempi  
  copiare **test. txt** nel computer locale usando il tipo di trasferimento di file corrente.  
  ```  
  recv test.txt  
  ```  
  copiare **test. txt** nel computer locale come **test1. txt** usando il tipo di trasferimento di file corrente.  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [FTP: ASCII](ftp-ascii.md)  
- [FTP: binario](ftp-binary.md)  
- [FTP: Get](ftp-get.md)  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
