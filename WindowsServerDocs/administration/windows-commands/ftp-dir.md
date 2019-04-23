---
title: dir FTP
description: Argomento i comandi di Windows per la directory ftp
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78ac8ac5e9fc4894f55401bb234aa98de981adf7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835252"
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
Visualizzare una visualizzazione per la directory **dir1** nel computer remoto.  
```  
dir dir1  
```  
Salvare un elenco di directory corrente nel computer remoto nel file locale **Elendir. txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
