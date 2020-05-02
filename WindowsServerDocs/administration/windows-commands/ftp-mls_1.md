---
title: mls_1 FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27f74d4c1d03cb4d9f665566f69485e80f8eccdc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725199"
---
# <a name="ftp-mls_1"></a>FTP: mls_1

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="remarks"></a>Osservazioni  
- Specifica di *remoteFiles*  
  digitare un trattino**-**() per utilizzare la directory di lavoro corrente nel computer remoto.  
- Specifica di *LocalFile*  
  digitare un trattino**-**() per visualizzare l'elenco sullo schermo.  
  ## <a name="examples"></a>Esempi  
  Visualizza un elenco abbreviato di file e sottodirectory per **dir1** e **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Salvare un elenco abbreviato di file e sottodirectory per **dir1** e **dir2** nel file locale **dirlist. txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
