---
title: ricezione FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ec35a2044945e3d39a2a78d39923de3a56eb18d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376124"
---
# <a name="ftp-recv"></a>FTP: ricezione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file remoto nel computer locale utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
recv <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parametri  

|   Parametro   |                   Descrizione                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Specifica il file remoto da copiare.        |
| [<LocalFile>] | Specifica il nome da utilizzare nel computer locale. |

## <a name="remarks"></a>Note  
- Il comando **ricezione** è identico al comando **Get** .  
- Se *LocalFile* non è specificato, al file viene assegnato il nome *FileRemoto* .  
  ## <a name="BKMK_Examples"></a>Esempi  
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
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
