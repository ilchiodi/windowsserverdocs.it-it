---
title: ricezione FTP
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5bfd68dcb745ebf7ef239883aa1c5322241b32df
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438428"
---
# <a name="ftp-recv"></a>FTP: medie

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un file remoto nel computer locale usando il tipo di trasferimento di file corrente.   
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
- Il **recv** è identico al comando il **ottenere** comando.  
- Se *LocalFile* non viene specificato, il file viene assegnato il *FileRemoto* nome.  
  ## <a name="BKMK_Examples"></a>Esempi  
  Copia **txt** nel computer locale usando il tipo di trasferimento di file corrente.  
  ```  
  recv test.txt  
  ```  
  Copia **test. txt** nel computer locale come **test1.txt** utilizzando il file corrente tipo di trasferimento.  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [ftp: ascii](ftp-ascii.md)  
- [ftp: binary](ftp-binary.md)  
- [ftp: get](ftp-get.md)  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
