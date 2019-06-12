---
title: mls_1 FTP
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7a379ead9c56af096e121048a8c0f596f6879bb0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438547"
---
# <a name="ftp-mls1"></a>FTP: mls_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di visualizzare un elenco abbreviato di file e le sottodirectory in una directory remota.   
## <a name="syntax"></a>Sintassi  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |                       Descrizione                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | Specifica il file per cui si desidera visualizzare un elenco. |
| <LocalFile>  |  Specifica un file locale in cui archiviare l'elenco.  |

## <a name="remarks"></a>Note  
- Specificando *FileRemoti*  
  Digitare un segno meno ( **-** ) usare la directory di lavoro corrente nel computer remoto.  
- Specifica di *FileLocale*  
  Digitare un segno meno ( **-** ) per visualizzare la voce sullo schermo.  
  ## <a name="BKMK_Examples"></a>Esempi  
  Visualizzare un elenco abbreviato di file e sottodirectory relative **dir1** e **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Salvare un elenco abbreviato di file e sottodirectory relative **dir1** e **dir2** nel file locale **Elendir. txt.**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
