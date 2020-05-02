---
title: setlocal
description: Argomento di riferimento per setlocale, che avvia la localizzazione delle variabili di ambiente in un file batch.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: def4e8c58399af498e3c097f53e526fbedf9de0d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721857"
---
# <a name="setlocal"></a>setlocal

Avvia la localizzazione delle variabili di ambiente in un file batch. Localizzazione continua fino a quando un corrispondente **endlocal** del comando o viene raggiunta la fine del file batch.



## <a name="syntax"></a>Sintassi

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>Argomenti

|Argomento|Descrizione|
|--------|-----------|
|ENABLEEXTENSIONS|Abilita le estensioni ai comandi fino a corrispondenti **endlocal** comando viene rilevato, indipendentemente dall'impostazione prima il **setlocal** comando è stato eseguito.|
|DISABLEEXTENSIONS|Disabilita le estensioni ai comandi fino a quando la corrispondenza **endlocal** comando viene rilevato, indipendentemente dall'impostazione prima di **setlocal** comando è stato eseguito.|
|ENABLEDELAYEDEXPANSION|Consente l'espansione della variabile di ambiente ritardata fino a quando la corrispondenza **endlocal** comando viene rilevato, indipendentemente dall'impostazione prima di **setlocal** comando è stato eseguito.|
|DISABLEDELAYEDEXPANSION|Disabilita l'espansione della variabile di ambiente ritardata fino a quando la corrispondenza **endlocal** comando viene rilevato, indipendentemente dall'impostazione prima di **setlocal** comando è stato eseguito.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Utilizzando **setlocal**

    Quando si utilizza **setlocal** all'esterno di un file batch o script, non ha alcun effetto.
-   Modifica le variabili di ambiente

    Utilizzare **setlocal** per modificare le variabili di ambiente quando si esegue un file batch. Le modifiche apportate dopo l'esecuzione dell'ambiente **setlocal** sono locali rispetto al file batch. Il programma Cmd.exe Ripristina le impostazioni precedenti quando rileva un **endlocal** comando o raggiunge la fine del file batch.
-   Comandi di annidamento

    È possibile avere più di una **setlocal** o **endlocal** comando in un file batch (vale a dire nidificati comandi).
-   Test per le estensioni ai comandi nei file batch

    Il **setlocal** comando imposta la variabile ERRORLEVEL. Se si passa {**enableextensions** | **disableextensions**} o {**ENABLEDELAYEDEXPANSION** | **disabledelayedexpansion**}, la variabile ERRORLEVEL è impostata su **0** (zero). In caso contrario, è impostato su **1**. È possibile utilizzare queste informazioni negli script batch per determinare se le estensioni sono disponibili, come illustrato nell'esempio seguente:  
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```  
    Poiché **cmd** non è impostata la variabile ERRORLEVEL quando le estensioni ai comandi sono disabilitate, il **verificare** comando Inizializza la variabile ERRORLEVEL su un valore diverso da zero quando viene usato con un argomento non valido. Inoltre, se si utilizza il **setlocal** comando con argomenti {**enableextensions** | **disableextensions**} o {**enabledelayedexpansion** | **disabledelayedexpansion**} e non imposta la variabile ERRORLEVEL su **1**, le estensioni ai comandi non sono disponibili.

## <a name="examples"></a>Esempi

È possibile localizzare le variabili di ambiente in un file batch, come illustrato nello script di esempio seguente:
```
rem *******Begin Comment**************
rem This program starts the superapp batch program on the network,
rem directs the output to a file, and displays the file
rem in Notepad.
rem *******End Comment**************
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)