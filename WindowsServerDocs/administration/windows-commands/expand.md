---
title: expand
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b757f630e08249b1c716803a07cd9a163d7398d2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725686"
---
# <a name="expand"></a>expand

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

espande uno o più file compressi. È possibile utilizzare questo comando per recuperare file compressi dai dischi originali.  
## <a name="syntax"></a>Sintassi  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro  |                                                                                                                                                                   Descrizione                                                                                                                                                                    |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /r      |                                                                                                                                                             Rinomina i file espansi.                                                                                                                                                              |
|   source    |                                                                              Specifica i file da espandere. *Origine* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi. È possibile utilizzare caratteri jolly (**\\** \* o **?**).                                                                               |
| destination | Specifica in cui i file da espandere.<p>Se l' *origine* è costituita da più file e non si specifica **/r**, la *destinazione* deve essere una directory.<p>*Destinazione* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi.<p>File di destinazione & #124; Specifica del percorso. |
|     /i      |                                                                                                   Rinomina i file espansi ma ignora la struttura di directory.<p>Questo parametro si applica a: Windows Server 2008 R2 e Windows 7.                                                                                                    |
|     /d      |                                                                                                                              Visualizza un elenco dei file nel percorso di origine. Non espandere o estrarre i file.                                                                                                                              |
|     /f:     |                                                                                                                 Specifica i file in un file cabinet (CAB) che si desidera espandere. È possibile utilizzare caratteri jolly (**\\** \* o **?**).                                                                                                                 |
|     /?      |                                                                                                                                                       Visualizza la guida al prompt dei comandi.                                                                                                                                                       |

## <a name="remarks"></a>Osservazioni  
- Uso di **expand** nella console di ripristino  
  Il comando **Espandi** , con parametri diversi, è disponibile dalla console di ripristino. Per ulteriori informazioni sulla console di ripristino, vedere l' [articolo 314058](https://support.microsoft.com/kb/314058) della Microsoft Knowledge base.  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
  - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
