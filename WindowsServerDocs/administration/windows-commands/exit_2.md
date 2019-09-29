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
ms.openlocfilehash: 105bf572c1ebeb37ea59ff8bc5c04121d2442341
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377352"
---
# <a name="exit"></a>exit

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

esce dal programma cmd. exe (l'interprete dei comandi) o dallo script batch corrente.  
Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).  
## <a name="syntax"></a>Sintassi  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parametri  

| Parametro  |                                                                                         Descrizione                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     / b     |                                      chiude lo script batch corrente anziché uscire da cmd. exe. Se eseguita in uno script batch, viene chiuso Cmd.exe.                                      |
| <exitCode> | Specifica un numero. Se **/b** è specificato, la variabile di ambiente ERRORLEVEL è impostata su tale numero. Se sono in fase di chiusura **Cmd.exe**, il codice di uscita del processo è impostato su tale numero. |
|     /?     |                                                                             Visualizza la guida al prompt dei comandi.                                                                             |

## <a name="BKMK_examples"></a>Esempi  
Per chiudere l'interprete dei comandi, Cmd.exe, digitare:  
```  
exit  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  

