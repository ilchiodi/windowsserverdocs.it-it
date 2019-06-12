---
title: Inserire FTP
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4d602c685b7eac5d18c88bc0f6709b189cc61a77
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438460"
---
# <a name="ftp-put"></a>ftp: put

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file locale nel computer remoto usando il tipo di trasferimento di file corrente.   
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
- Il **inserito** è identico al comando il **inviare** comando.  
- Se *FileRemoto* non viene specificato, il file viene assegnato il *FileLocale* nome.  
  ## <a name="BKMK_Examples"></a>Esempi  
  copiare il file locale **test. txt** e denominarlo **test1.txt** nel computer remoto.  
  ```  
  put test.txt test1.txt  
  ```  
  copiare il file locale **program.exe** al computer remoto.  
  ```  
  put program.exe  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [ftp: ascii](ftp-ascii.md)  
- [ftp: binary](ftp-binary.md)  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
