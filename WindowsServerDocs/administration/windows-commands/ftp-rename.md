---
title: ridenominazione FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5dc5006c82df8417a8652a9c0ba20f7f1a002e7f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725102"
---
# <a name="ftp-rename"></a>FTP: Rinomina

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rinomina i file remoti.   
## <a name="syntax"></a>Sintassi  
```  
rename <FileName> <NewFileName>  
```  
#### <a name="parameters"></a>Parametri  

|   Parametro   |                 Descrizione                 |
|---------------|---------------------------------------------|
|  <FileName>   | Specifica il file che si desidera rinominare. |
| <NewFileName> |        Specifica il nuovo nome del file.         |

## <a name="examples"></a>Esempi  
Rinominare il file remoto **example. txt** in **Example1. txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
