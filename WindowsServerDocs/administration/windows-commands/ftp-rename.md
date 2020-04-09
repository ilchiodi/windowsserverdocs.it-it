---
title: ridenominazione FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbe159f2833ce52921b46e46881a1d7aed8c5df8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843024"
---
# <a name="ftp-rename"></a>FTP: Rinomina

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Rinominare il file remoto **example. txt** in **Example1. txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
