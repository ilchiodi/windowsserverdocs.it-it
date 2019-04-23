---
title: ftp send_1
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3dca11f0d534eb875a71fa2c39cdd4dc674ad788
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862122"
---
# <a name="ftp-send1"></a>ftp: send_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file locale nel computer remoto usando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
send <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|<LocalFile>|Specifica il file locale da copiare.|  
|<remoteFile>|Specifica il nome da utilizzare nel computer remoto.|  
## <a name="remarks"></a>Note  
-   Il **inviare** è identico al comando il **put** comando.  
-   Se *FileRemoto* non viene specificato, il file viene assegnato il *FileLocale* nome.  
## <a name="BKMK_Examples"></a>Esempi  
copiare il file locale **test. txt** e denominarlo **test1.txt** nel computer remoto.  
```  
send test.txt test1.txt  
```  
copiare il file locale **program.exe** al computer remoto.  
```  
send program.exe  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
