---
title: exit
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b3490c6bc95a762bf2cb1da70f389fb8f583344f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819492"
---
# <a name="exit"></a>exit

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

esce dal programma Cmd.exe (l'interprete dei comandi) o lo script batch corrente.  
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).  
## <a name="syntax"></a>Sintassi  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/ b|Chiude lo script batch corrente anziché uscire Cmd.exe. Se eseguita in uno script batch, viene chiuso Cmd.exe.|  
|<exitCode>|Specifica un numero. Se **/b** è specificato, la variabile di ambiente ERRORLEVEL è impostata su tale numero. Se sono in fase di chiusura **Cmd.exe**, il codice di uscita del processo è impostato su tale numero.|  
|/?|Visualizza la guida al prompt dei comandi.|  
## <a name="BKMK_examples"></a>Esempi  
Per chiudere l'interprete dei comandi, Cmd.exe, digitare:  
```  
exit  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  
