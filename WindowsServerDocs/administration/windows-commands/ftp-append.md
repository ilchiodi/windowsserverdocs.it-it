---
title: aggiunta di FTP
description: 'Aggiungi argomento i comandi di Windows per il servizio ftp '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9580d725120bb32a9b915d37cdbc173bfb17b859
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438838"
---
# <a name="ftp-append"></a>FTP: append

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge un file locale in un file nel computer remoto utilizzando il tipo di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
append <LocalFile> [remoteFile]  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |                               Descrizione                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Specifica il file locale da aggiungere.                     |
| [remoteFile] | Specifica il file nel computer remoto a cui <LocalFile> viene aggiunto. |

## <a name="remarks"></a>Note  
Se *FileRemoto* viene omesso, il *FileLocale* nome viene usato al posto del nome file remoto.  
## <a name="BKMK_Examples"></a>Esempi  
aggiungere file1. txt file2 nel computer remoto.  
```  
append file1.txt file2.txt  
```  
aggiungere il file1 locale in un file denominato file1. txt nel computer remoto.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
