---
title: Aggiunta FTP
description: 'Argomento comandi di Windows per aggiunta FTP '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52d16b878ff5fb165fd851b227dcc361c9da3a80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376640"
---
# <a name="ftp-append"></a>FTP: Accoda

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Accoda un file locale a un file nel computer remoto utilizzando l'impostazione del tipo di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
append <LocalFile> [remoteFile]  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |                               Descrizione                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Specifica il file locale da aggiungere.                     |
| Fileremoto | Specifica il file nel computer remoto a cui <LocalFile> viene aggiunto. |

## <a name="remarks"></a>Note  
Se *FileRemoto* viene omesso, al posto del nome del file remoto viene usato il nome *LocalFile* .  
## <a name="BKMK_Examples"></a>Esempi  
aggiungere file1. txt a file2. txt nel computer remoto.  
```  
append file1.txt file2.txt  
```  
aggiungere il file file1. txt locale a un file denominato file1. txt nel computer remoto.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
