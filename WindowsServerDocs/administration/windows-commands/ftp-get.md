---
title: FTP Get
description: Argomento dei comandi di Windows per FTP Get
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0b4dc41ec29edfb94661176a5ccaf651584fc43
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843484"
---
# <a name="ftp-get"></a>FTP: Get

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file remoto nel computer locale utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
get <remoteFile> [<LocalFile>]  
```  
#### <a name="parameters"></a>Parametri  

|   Parametro   |                                                              Descrizione                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   Specifica il file remoto da copiare.                                                   |
| [<LocalFile>] | Specifica il nome del file da utilizzare nel computer locale. Se *LocalFile* non è specificato, al file viene assegnato il nome *FileRemoto* . |

## <a name="remarks"></a>Note  
Il **ottenere** è identico al comando il **recv** comando.  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
copiare **test. txt** nel computer locale usando il tipo di trasferimento di file corrente.  
```  
get test.txt  
```  
copiare **test. txt** nel computer locale come **test1. txt** usando il tipo di trasferimento di file corrente.  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
