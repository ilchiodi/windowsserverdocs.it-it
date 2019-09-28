---
title: FTP PUT
description: 'Argomento dei comandi di Windows per * * * *- '
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
ms.date: 10/16/2017
ms.openlocfilehash: 15c1734322d3642ebc85891b71c6ad68100d514d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376068"
---
# <a name="ftp-put"></a>FTP: Put

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file locale nel computer remoto utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
put <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parametri  

|   Parametro    |                    Descrizione                    |
|----------------|---------------------------------------------------|
|  <LocalFile>   |         Specifica il file locale da copiare.         |
| [<remoteFile>] | Specifica il nome da utilizzare nel computer remoto. |

## <a name="remarks"></a>Note  
- Il comando **put** è identico al comando **Send** .  
- Se *FileRemoto* non è specificato, al file viene assegnato il nome *LocalFile* .  
  ## <a name="BKMK_Examples"></a>Esempi  
  Copiare il file **test. txt** locale e denominarlo **test1. txt** nel computer remoto.  
  ```  
  put test.txt test1.txt  
  ```  
  Copiare il file locale **Program. exe** nel computer remoto.  
  ```  
  put program.exe  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [FTP: ASCII](ftp-ascii.md)  
- [FTP: binario](ftp-binary.md)  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
