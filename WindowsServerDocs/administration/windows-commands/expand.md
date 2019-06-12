---
title: expand
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84fd3693ab41780f7092d74228a06503f9bca74f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439393"
---
# <a name="expand"></a>expand

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

si espande uno o più file compressi. È possibile utilizzare questo comando per recuperare file compressi dai dischi originali.  
## <a name="syntax"></a>Sintassi  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
### <a name="parameters"></a>Parametri  

|  Parametro  |                                                                                                                                                                   Descrizione                                                                                                                                                                    |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /r      |                                                                                                                                                             file ridenominazioni espansi.                                                                                                                                                              |
|   origine    |                                                                              Specifica i file da espandere. *Origine* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi. È possibile usare caratteri jolly ( **\\** \* oppure **?** ).                                                                               |
| destinazione | Specifica in cui i file da espandere.<br /><br />Se *origine* costituito da più file e non si specifica **/r**, *destinazione* deve essere una directory.<br /><br />*Destinazione* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi.<br /><br />File di destinazione &#124; Specifica del percorso. |
|     /i      |                                                                                                   Rinomina file espansi, ma ignora la struttura di directory.<br /><br />Questo parametro si applica a:  Windows Server 2008 R2 e Windows 7.                                                                                                    |
|     /d      |                                                                                                                              Visualizza un elenco dei file nel percorso di origine. Non espandere o estrarre i file.                                                                                                                              |
|     /f:     |                                                                                                                 Specifica i file in un file cabinet (CAB) che si desidera espandere. È possibile usare caratteri jolly ( **\\** \* oppure **?** ).                                                                                                                 |
|     /?      |                                                                                                                                                       Visualizza la guida al prompt dei comandi.                                                                                                                                                       |

## <a name="remarks"></a>Note  
- Usando **espandere** nella Console di ripristino  
  Il **espandere** comando con parametri diversi, è disponibile dalla Console di ripristino. Per altre informazioni sulla Console di ripristino, vedere [articolo 314058](https://support.microsoft.com/kb/314058) nella Microsoft Knowledge Base.  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
  [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
