---
title: mdir FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb024d063761f1e817f02fdd7301376dd167846
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842714"
---
# <a name="ftp-mdir"></a>FTP: mdir

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di directory di file e le sottodirectory in una directory remota.   
## <a name="syntax"></a>Sintassi  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro   |                               Descrizione                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Specifica la directory o file per il quale si desidera visualizzare un elenco.   |
| <LocalFile>  | Specifica un file locale per archiviare l'elenco. Parametro obbligatorio. |

## <a name="remarks"></a>Note  
- È possibile utilizzare **mdir** per specificare più file.  
- Specifica di *FileRemoto*  
  digitare un trattino ( **-** ) per utilizzare la directory di lavoro corrente nel computer remoto.  
- Specifica un *FileLocale*  
  digitare un trattino ( **-** ) per visualizzare l'elenco sullo schermo.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
  Visualizza un elenco di directory di **dir1** e **dir2** sullo schermo  
  ```  
  mdir dir1 dir2 -  
  ```  
  Salvare l'elenco di directory combinate di **dir1** e **dir2** in un file locale denominato **dirlist. txt**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Altre informazioni di riferimento  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
