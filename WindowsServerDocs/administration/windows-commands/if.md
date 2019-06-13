---
title: if
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9751dfe3e0cb0965cc2c5169ea19e0f08110b0ff
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438105"
---
# <a name="if"></a>if



Esegue un'elaborazione condizionale nei programmi batch.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
if [not] ERRORLEVEL <Number> <Command> [else <Expression>]
if [not] <String1>==<String2> <Command> [else <Expression>]
if [not] exist <FileName> <Command> [else <Expression>]
```
Se sono abilitate le estensioni dei comandi, utilizzare la sintassi seguente:
```
if [/i] <String1> <CompareOp> <String2> <Command> [else <Expression>]
if cmdextversion <Number> <Command> [else <Expression>]
if defined <Variable> <Command> [else <Expression>]
```

## <a name="parameters"></a>Parametri

|        Parametro        |                                                                                                                                                                                                                Descrizione                                                                                                                                                                                                                 |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           not           |                                                                                                                                                                              Specifica che il comando deve essere effettuato solo se la condizione è false.                                                                                                                                                                              |
|  ERRORLEVEL \<numero >   |                                                                                                                                                      Specifica una condizione true solo se il precedente programma eseguito da Cmd.exe ha restituito un codice di uscita uguale o maggiore di *numero*.                                                                                                                                                       |
|       \<Comando >        |                                                                                                                                                                            Specifica il comando che deve essere eseguito se viene soddisfatta la condizione precedente.                                                                                                                                                                             |
|  \<String1 > = =<String2>  |                                                                                                             Specifica di una condizione true soltanto se *String1* e *stringa2* sono uguali. Questi valori possono essere stringhe letterali o variabili batch (ad esempio, %1). Non è necessario racchiuderlo tra virgolette stringhe letterali.                                                                                                              |
|    Esistono \<nomefile >    |                                                                                                                                                                                       Specifica una condizione true se il nome file specificato esiste.                                                                                                                                                                                        |
|      \<CompareOp>       |                                                                               Specifica un operatore di confronto di tre lettere. Nell'elenco seguente rappresenta i valori validi per *OpConfronto*:</br>**EQU** uguale a</br>**NEQ** non è uguale a</br>**LSS** minore di</br>**LEQ** minore o uguale a</br>**GTR** maggiore di</br>**GEQ** maggiore o uguale a                                                                                |
|           /i            |                                                            Forza confronti per ignorare la distinzione tra stringhe.  È possibile utilizzare **/i** sul <em>String1</em> **==** <em>String2</em> forma di **Se**. Questi confronti sono generici, in quanto se entrambi *String1* e *String2* sono costituiti da cifre numeriche, solo le stringhe vengono convertite in numeri e viene eseguito un confronto numerico.                                                            |
| CMDEXTVERSION \<numero > | Specifica di una condizione true soltanto se il numero di versione interno associato alle estensioni di comando di Cmd.exe è uguale a o maggiore del numero specificato. La prima versione è 1. Aumenta in modo incrementale uno quando vengono aggiunti miglioramenti significativi alle estensioni del comando. Il **cmdextversion** condizionale è mai true quando comando le estensioni sono disabilitate (per impostazione predefinita, comando estensioni sono abilitate). |
|   definito \<variabile >   |                                                                                                                                                                                            Specifica una condizione true se *variabile* è definito.                                                                                                                                                                                            |
|      \<Expression>      |                                                                                                                                                                   Specifica una riga di comando e i parametri da passare al comando in un **else** clausola.                                                                                                                                                                   |
|           /?            |                                                                                                                                                                                                    Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                    |

## <a name="remarks"></a>Note

-   Se la condizione specificata un **Se** è true, il comando che segue la condizione viene eseguito. Se la condizione è false, il comando di Se clausola viene ignorata e il comando esegue qualsiasi comando specificato nel **else** clausola.
-   Quando si arresta un programma, viene restituito un codice di uscita. Per utilizzare i codici di uscita come condizioni, utilizzare **errorlevel**.
-   Se si utilizza **definito**, le seguenti tre variabili vengono aggiunte all'ambiente: **% errorlevel %** , **% cmdcmdline %** , e **% cmdextversion %** .  
    -   **% errorlevel %** si espande in una rappresentazione di stringa del valore corrente della variabile di ambiente ERRORLEVEL. Si presuppone che non vi sia una variabile di ambiente esistente con il nome ERRORLEVEL, ovvero se è presente, verrà visualizzato invece il valore ERRORLEVEL.
    -   **% cmdcmdline %** si espande nella riga di comando originale passati al Cmd.exe prima di qualsiasi elaborazione tramite Cmd.exe. Si presuppone che non vi sia una variabile di ambiente esistente denominata, ovvero se è presente, si otterrà il valore CMDCMDLINE invece.
    -   **% cmdextversion %** si espande nella rappresentazione di stringa del valore corrente di **cmdextversion**. Si presuppone che non vi sia una variabile di ambiente esistente denominata, ovvero se è presente, si otterrà il valore CMDEXTVERSION invece.
-   È necessario utilizzare il **else** clausola nella stessa riga di comando dopo il **Se**.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare il messaggio "Impossibile trovare file di dati" Se non viene trovato il file di prodotto. dat, digitare:
```
if not exist product.dat echo Cannot find data file 
```
Per formattare un disco nell'unità e visualizzare un messaggio di errore se si verifica un errore durante il processo di formattazione, digitare le righe seguenti in un file batch:
```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```
Per eliminare il file di prodotto. dat dalla directory corrente o visualizzare un messaggio se non viene trovato prodotto. dat, digitare le righe seguenti in un file batch:
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> Queste righe possono essere combinate in una singola riga, come indicato di seguito:
> ```
> IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
> ```
> Per visualizzare il valore della variabile di ambiente ERRORLEVEL dopo l'esecuzione di un file batch, digitare le righe seguenti nel file batch:
> ```
> goto answer%errorlevel%
> :answer1
> echo Program had return code 1
> :answer0
> echo Program had return code 0
> goto end
> :end
> echo Done! 
> ```
> Per passare all'etichetta di "okay" se il valore della variabile di ambiente ERRORLEVEL è minore o uguale a 1, tipo:
> ```
> if %errorlevel% LEQ 1 goto okay
> ```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[If](if.md)

[Vai a](goto.md)