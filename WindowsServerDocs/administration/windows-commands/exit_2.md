---
title: exit
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6d3a4d0bfc74644f6fda43abe57e0e4e7c1264a
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791225"
---
# <a name="exit"></a>exit

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chiude il programma Cmd.exe (l'interprete dei comandi) o lo script batch corrente.  
Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).  
## <a name="syntax"></a>Sintassi  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parametri  

| Parametro  |                                                                                         Descrizione                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     / b     |                                      Chiude lo script batch corrente anziché uscire Cmd.exe. Se eseguita in uno script batch, viene chiuso Cmd.exe.                                      |
| `<exitCode>` | Specifica un numero. Se **/b** è specificato, la variabile di ambiente ERRORLEVEL è impostata su tale numero. Se sono in fase di chiusura **Cmd.exe**, il codice di uscita del processo è impostato su tale numero. |
|     /?     |                                                                             Visualizza la guida al prompt dei comandi.                                                                             |

## <a name="BKMK_examples"></a>Esempi  
Per chiudere l'interprete dei comandi, Cmd.exe, digitare:  
```  
exit  
```  
## <a name="additional-references"></a>riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  

