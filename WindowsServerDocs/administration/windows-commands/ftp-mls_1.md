---
title: mls_1 FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a84a0f8f3121ea19876744e9ef04bebf5f9fcb08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376264"
---
# <a name="ftp-mls_1"></a>FTP: mls_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco abbreviato di file e sottodirectory in una directory remota.   
## <a name="syntax"></a>Sintassi  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |                       Descrizione                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | Specifica il file per il quale si desidera visualizzare un elenco. |
| <LocalFile>  |  Specifica un file locale in cui archiviare l'elenco.  |

## <a name="remarks"></a>Note  
- Specifica di *remoteFiles*  
  digitare un trattino ( **-** ) per utilizzare la directory di lavoro corrente nel computer remoto.  
- Specifica di *LocalFile*  
  digitare un trattino ( **-** ) per visualizzare l'elenco sullo schermo.  
  ## <a name="BKMK_Examples"></a>Esempi  
  Visualizza un elenco abbreviato di file e sottodirectory per **dir1** e **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Salvare un elenco abbreviato di file e sottodirectory per **dir1** e **dir2** nel file locale **dirlist. txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
