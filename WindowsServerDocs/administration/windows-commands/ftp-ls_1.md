---
title: ls_1 FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ff63272fe3c5e59965b04bef258a06fee2da0c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843364"
---
# <a name="ftp-ls_1"></a>FTP: ls_1

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco abbreviato di file e le sottodirectory del computer remoto.   
## <a name="syntax"></a>Sintassi  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>Parametri  

|      Parametro      |                                                                       Descrizione                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | Specifica la directory per il quale si desidera visualizzare un elenco. Se non viene specificata alcuna directory, viene utilizzata la directory di lavoro corrente nel computer remoto. |
|    [<LocalFile>]    |               Specifica un file locale in cui archiviare l'elenco. Se non viene specificato un file locale, i risultati vengono visualizzati sullo schermo.               |

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Visualizzare un breve elenco dei file e le sottodirectory del computer remoto.  
```  
ls  
```  
Ottenere un elenco abbreviato di directory di **dir1** nel computer remoto e salvarlo in un file locale denominato **dirlist. txt**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
