---
title: mls_1 FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca3b8e04dd4a152b2d1bf8ce1ca8006d70186116
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843294"
---
# <a name="ftp-mls_1"></a>FTP: mls_1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco abbreviato di file e sottodirectory in una directory remota.   
## <a name="syntax"></a>Sintassi  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro   |                       Descrizione                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | Specifica il file per il quale si desidera visualizzare un elenco. |
| <LocalFile>  |  Specifica un file locale in cui archiviare l'elenco.  |

## <a name="remarks"></a>Note  
- Specifica di *remoteFiles*  
  digitare un trattino ( **-** ) per utilizzare la directory di lavoro corrente nel computer remoto.  
- Specifica di *LocalFile*  
  digitare un trattino ( **-** ) per visualizzare l'elenco sullo schermo.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
  Visualizza un elenco abbreviato di file e sottodirectory per **dir1** e **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Salvare un elenco abbreviato di file e sottodirectory per **dir1** e **dir2** nel file locale **dirlist. txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Altre informazioni di riferimento  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
