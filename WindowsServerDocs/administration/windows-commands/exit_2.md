---
title: exit
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13cf7a7658394e59ce6cc7e66c3083cd3d359574
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844884"
---
# <a name="exit"></a>exit

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chiude il programma Cmd.exe (l'interprete dei comandi) o lo script batch corrente.  
Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).  
## <a name="syntax"></a>Sintassi  
```  
exit [/b] [<exitCode>]  
```  
### <a name="parameters"></a>Parametri  

| Parametro  |                                                                                         Descrizione                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     / b     |                                      Chiude lo script batch corrente anziché uscire Cmd.exe. Se eseguita in uno script batch, viene chiuso Cmd.exe.                                      |
| `<exitCode>` | Specifica un numero. Se **/b** è specificato, la variabile di ambiente ERRORLEVEL è impostata su tale numero. Se sono in fase di chiusura **Cmd.exe**, il codice di uscita del processo è impostato su tale numero. |
|     /?     |                                                                             Visualizza la guida al prompt dei comandi.                                                                             |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Per chiudere l'interprete dei comandi, Cmd.exe, digitare:  
```  
exit  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  

