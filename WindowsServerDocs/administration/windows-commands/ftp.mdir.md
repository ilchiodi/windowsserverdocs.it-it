---
title: mdir FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 08aa5bb216a3d0155c100c761e476bb963e59311
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375855"
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
  digitare un trattino ( **-** ) per utilizzare la directory di lavoro corrente nel computer remoto.  
- Specifica un *FileLocale*  
  digitare un trattino ( **-** ) per visualizzare l'elenco sullo schermo.  
  ## <a name="BKMK_Examples"></a>Esempi  
  Visualizza un elenco di directory di **dir1** e **dir2** sullo schermo  
  ```  
  mdir dir1 dir2 -  
  ```  
  Salvare l'elenco di directory combinate di **dir1** e **dir2** in un file locale denominato **dirlist. txt**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
