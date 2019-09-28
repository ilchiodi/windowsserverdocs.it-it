---
title: ls_1 FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d183f6a014273b78befd14c8d3208508948ffc54
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376246"
---
# <a name="ftp-ls_1"></a>FTP: ls_1

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco abbreviato di file e le sottodirectory del computer remoto.   
## <a name="syntax"></a>Sintassi  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parametri  

|      Parametro      |                                                                       Descrizione                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | Specifica la directory per il quale si desidera visualizzare un elenco. Se non viene specificata alcuna directory, viene utilizzata la directory di lavoro corrente nel computer remoto. |
|    [<LocalFile>]    |               Specifica un file locale in cui archiviare l'elenco. Se non viene specificato un file locale, i risultati vengono visualizzati sullo schermo.               |

## <a name="BKMK_Examples"></a>Esempi  
Visualizzare un breve elenco dei file e le sottodirectory del computer remoto.  
```  
ls  
```  
Ottenere un elenco abbreviato di directory di **dir1** nel computer remoto e salvarlo in un file locale denominato **dirlist. txt**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
