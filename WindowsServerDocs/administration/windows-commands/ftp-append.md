---
title: Aggiunta FTP
description: Argomento comandi di Windows per aggiunta FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44ce1d6e7259dc8745da35ed462e6378f0fce8ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843824"
---
# <a name="ftp-append"></a>FTP: Accoda

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Accoda un file locale a un file nel computer remoto utilizzando l'impostazione del tipo di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
append <LocalFile> [remoteFile]  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro   |                               Descrizione                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Specifica il file locale da aggiungere.                     |
| Fileremoto | Specifica il file nel computer remoto a cui <LocalFile> viene aggiunto. |

## <a name="remarks"></a>Note  
Se *FileRemoto* viene omesso, al posto del nome del file remoto viene usato il nome *LocalFile* .  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
aggiungere file1. txt a file2. txt nel computer remoto.  
```  
append file1.txt file2.txt  
```  
aggiungere il file file1. txt locale a un file denominato file1. txt nel computer remoto.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
