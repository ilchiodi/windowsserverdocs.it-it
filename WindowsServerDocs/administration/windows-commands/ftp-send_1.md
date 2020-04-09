---
title: send_1 FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: df6ea9eda6fced91babca37639cd6efe981ba7de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843004"
---
# <a name="ftp-send_1"></a>FTP: send_1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="remarks"></a>Note  
- Il comando **Send** è identico al comando **put** .  
- Se *FileRemoto* non è specificato, al file viene assegnato il nome *LocalFile* .  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
  Copiare il file **test. txt** locale e denominarlo **test1. txt** nel computer remoto.  
  ```  
  send test.txt test1.txt  
  ```  
  Copiare il file locale **Program. exe** nel computer remoto.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Altre informazioni di riferimento  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
