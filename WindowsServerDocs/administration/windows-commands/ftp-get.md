---
title: FTP Get
description: Argomento di riferimento per FTP Get
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37423f81fd72e79cebcdf169160ad3e8033b69b2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725290"
---
# <a name="ftp-get"></a>FTP: Get

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="remarks"></a>Osservazioni  
Il **ottenere** è identico al comando il **recv** comando.  
## <a name="examples"></a>Esempi  
copiare **test. txt** nel computer locale usando il tipo di trasferimento di file corrente.  
```  
get test.txt  
```  
copiare **test. txt** nel computer locale come **test1. txt** usando il tipo di trasferimento di file corrente.  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
