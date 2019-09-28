---
title: Send_1 FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd9f658b4fbfa5f6c9fa9a58fb0c524ad53627bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376005"
---
# <a name="ftp-send_1"></a>FTP: Send_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file locale nel computer remoto utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
send <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |                    Descrizione                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         Specifica il file locale da copiare.         |
| <remoteFile> | Specifica il nome da utilizzare nel computer remoto. |

## <a name="remarks"></a>Note  
- Il comando **Send** è identico al comando **put** .  
- Se *FileRemoto* non è specificato, al file viene assegnato il nome *LocalFile* .  
  ## <a name="BKMK_Examples"></a>Esempi  
  Copiare il file **test. txt** locale e denominarlo **test1. txt** nel computer remoto.  
  ```  
  send test.txt test1.txt  
  ```  
  Copiare il file locale **Program. exe** nel computer remoto.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
