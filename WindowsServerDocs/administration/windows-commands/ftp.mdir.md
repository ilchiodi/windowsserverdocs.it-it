---
title: FTP mdir
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4ec445c3e367a46dc40d10a37c0b3b8e53a10e3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438337"
---
# <a name="ftp-mdir"></a>FTP: mdir

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di directory di file e le sottodirectory in una directory remota.   
## <a name="syntax"></a>Sintassi  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |                               Descrizione                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Specifica la directory o file per il quale si desidera visualizzare un elenco.   |
| <LocalFile>  | Specifica un file locale per archiviare l'elenco. Questo parametro è obbligatorio. |

## <a name="remarks"></a>Note  
- È possibile utilizzare **mdir** per specificare più file.  
- Specifica di *FileRemoto*  
  Digitare un segno meno ( **-** ) usare la directory di lavoro corrente nel computer remoto.  
- Specifica un *FileLocale*  
  Digitare un segno meno ( **-** ) per visualizzare la voce sullo schermo.  
  ## <a name="BKMK_Examples"></a>Esempi  
  Visualizzare un elenco di directory **dir1** e **dir2** sullo schermo  
  ```  
  mdir dir1 dir2 -  
  ```  
  Salvare l'elenco delle directory combinato **dir1** e **dir2** in un file locale denominato **Elendir. txt.**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
