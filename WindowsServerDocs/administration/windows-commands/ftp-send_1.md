---
title: send_1 FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 04617246c05edde127db01ce1a0fe692eb0aceb1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725101"
---
# <a name="ftp-send_1"></a>FTP: send_1

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file locale nel computer remoto utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
send <LocalFile> [<remoteFile>]  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro   |                    Descrizione                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         Specifica il file locale da copiare.         |
| <remoteFile> | Specifica il nome da utilizzare nel computer remoto. |

## <a name="remarks"></a>Osservazioni  
- Il comando **Send** è identico al comando **put** .  
- Se *FileRemoto* non è specificato, al file viene assegnato il nome *LocalFile* .  
  ## <a name="examples"></a>Esempi  
  Copiare il file **test. txt** locale e denominarlo **test1. txt** nel computer remoto.  
  ```  
  send test.txt test1.txt  
  ```  
  Copiare il file locale **Program. exe** nel computer remoto.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
