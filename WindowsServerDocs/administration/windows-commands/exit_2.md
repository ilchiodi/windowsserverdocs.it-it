---
title: exit
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e47dfa42f2bacb3fe9f12d1da9163bcf828e9488
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725700"
---
# <a name="exit"></a>exit

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chiude il programma Cmd.exe (l'interprete dei comandi) o lo script batch corrente.  
  
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

## <a name="examples"></a>Esempi  
Per chiudere l'interprete dei comandi, Cmd.exe, digitare:  
```  
exit  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  

