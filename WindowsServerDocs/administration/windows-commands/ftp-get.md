---
title: FTP Get
description: Argomento dei comandi di Windows per FTP Get
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cc74b56fa849a25ed2f4e4a37d339b1da87c24f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376393"
---
# <a name="ftp-get"></a>FTP: Get

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file remoto nel computer locale utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parametri  

|   Parametro   |                                                              Descrizione                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   Specifica il file remoto da copiare.                                                   |
| [<LocalFile>] | Specifica il nome del file da utilizzare nel computer locale. Se *LocalFile* non è specificato, al file viene assegnato il nome *FileRemoto* . |

## <a name="remarks"></a>Note  
Il **ottenere** è identico al comando il **recv** comando.  
## <a name="BKMK_Examples"></a>Esempi  
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
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
