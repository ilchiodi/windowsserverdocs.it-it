---
title: dir FTP
description: Argomento dei comandi di Windows per la directory FTP
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c47971c52135d79ce62f935bfed981f6eefcecaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376438"
---
# <a name="ftp-dir"></a>FTP: dir

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di file di directory e sottodirectory in un computer remoto.   
## <a name="syntax"></a>Sintassi  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<remotedirectory>]|Specifica la directory per il quale si desidera visualizzare un elenco. Se non viene specificata alcuna directory, viene utilizzata la directory di lavoro corrente nel computer remoto.|  
|[<LocalFile>]|Specifica un file locale in cui archiviare la visualizzazione della directory. Se non viene specificato un file locale, i risultati vengono visualizzati sullo schermo.|  
## <a name="BKMK_Examples"></a>Esempi  
Visualizza un elenco di directory per **dir1** nel computer remoto.  
```  
dir dir1  
```  
Salvare un elenco della directory corrente nel computer remoto nel file locale **dirlist. txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
