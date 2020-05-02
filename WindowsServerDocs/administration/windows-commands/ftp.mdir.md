---
title: mdir FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54adcd1d28079487c5e238c72413d8269447944e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725021"
---
# <a name="ftp-mdir"></a>FTP: mdir

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di directory di file e le sottodirectory in una directory remota.   
## <a name="syntax"></a>Sintassi  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro   |                               Descrizione                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Specifica la directory o file per il quale si desidera visualizzare un elenco.   |
| <LocalFile>  | Specifica un file locale per archiviare l'elenco. Questo parametro è obbligatorio. |

## <a name="remarks"></a>Osservazioni  
- È possibile utilizzare **mdir** per specificare più file.  
- Specifica di *FileRemoto*  
  digitare un trattino**-**() per utilizzare la directory di lavoro corrente nel computer remoto.  
- Specifica un *FileLocale*  
  digitare un trattino**-**() per visualizzare l'elenco sullo schermo.  
  ## <a name="examples"></a>Esempi  
  Visualizza un elenco di directory di **dir1** e **dir2** sullo schermo  
  ```  
  mdir dir1 dir2 -  
  ```  
  Salvare l'elenco di directory combinate di **dir1** e **dir2** in un file locale denominato **dirlist. txt**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
