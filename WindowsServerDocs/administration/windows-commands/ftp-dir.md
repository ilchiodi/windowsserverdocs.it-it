---
title: dir FTP
description: Argomento dei comandi di Windows per la directory FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64f227ea9806f169c2df1698382cfce6e7ac3257
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843574"
---
# <a name="ftp-dir"></a>FTP: dir

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di file di directory e sottodirectory in un computer remoto.   
## <a name="syntax"></a>Sintassi  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<remotedirectory>]|Specifica la directory per il quale si desidera visualizzare un elenco. Se non viene specificata alcuna directory, viene utilizzata la directory di lavoro corrente nel computer remoto.|  
|[<LocalFile>]|Specifica un file locale in cui archiviare la visualizzazione della directory. Se non viene specificato un file locale, i risultati vengono visualizzati sullo schermo.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Visualizza un elenco di directory per **dir1** nel computer remoto.  
```  
dir dir1  
```  
Salvare un elenco della directory corrente nel computer remoto nel file locale **dirlist. txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
