---
title: if
description: Argomento di riferimento per il comando if, che esegue l'elaborazione condizionale nei programmi batch.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6f73e784d6fb394db258a056f38045b6a545469
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818551"
---
# <a name="if"></a>if

Esegue un'elaborazione condizionale nei programmi batch.

## <a name="syntax"></a>Sintassi

```
if [not] ERRORLEVEL <number> <command> [else <expression>]
if [not] <string1>==<string2> <command> [else <expression>]
if [not] exist <filename> <command> [else <expression>]
```

Se sono abilitate le estensioni dei comandi, utilizzare la sintassi seguente:

```
if [/i] <string1> <compareop> <string2> <command> [else <expression>]
if cmdextversion <number> <command> [else <expression>]
if defined <variable> <command> [else <expression>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| not | Specifica che il comando deve essere effettuato solo se la condizione è false. |
| ERRORLEVEL`<number>` | Specifica una condizione true solo se il programma precedente eseguito da cmd. exe ha restituito un codice di uscita uguale o maggiore di un *numero*. |
| `<command>` | Specifica il comando che deve essere eseguito se viene soddisfatta la condizione precedente. |
| `<string1>==<string2>` | Specifica una condizione true solo se *String1* e *string2* sono uguali. Questi valori possono essere stringhe letterali o variabili di batch, ad esempio `%1` . Non è necessario racchiuderlo tra virgolette stringhe letterali. |
| esiste`<filename>` | Specifica una condizione true se il nome file specificato esiste. |
| `<compareop>` | Specifica un operatore di confronto di tre lettere, tra cui:<ul><li>**EQU** -uguale a</li><li>**NEQ** -diverso da</li><li>**LSS** -minore di</li><li>**Leq** -minore o uguale a</li><li>**GTR** -maggiore di</li><li>**GEQ** -maggiore o uguale a</li></ul> |
| /i | Forza confronti per ignorare la distinzione tra stringhe. È possibile utilizzare **/i** nel `string1==string2` formato **if**. Questi confronti sono generici, in quanto se sia *String1* che *string2* sono costituiti solo da cifre numeriche, le stringhe vengono convertite in numeri e viene eseguito un confronto numerico. |
| CMDEXTVERSION`<number>` | Specifica di una condizione true soltanto se il numero di versione interno associato alle estensioni di comando di Cmd.exe è uguale a o maggiore del numero specificato. La prima versione è 1. Aumenta in modo incrementale uno quando vengono aggiunti miglioramenti significativi alle estensioni del comando. Il **cmdextversion** condizionale è mai true quando comando le estensioni sono disabilitate (per impostazione predefinita, comando estensioni sono abilitate). |
| defined `<variable>` | Specifica una condizione true se è definita una *variabile* . |
| `<expression>` | Specifica una riga di comando e i parametri da passare al comando in un **else** clausola. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Se la condizione specificata in una clausola **if** è true, viene eseguito il comando che segue la condizione. Se la condizione è false, il comando nella clausola **if** viene ignorato e il comando esegue qualsiasi comando specificato nella clausola **else** .

- Quando si arresta un programma, viene restituito un codice di uscita. Per usare i codici di uscita come condizioni, usare il parametro **errorlevel** .

- Se si utilizza **definito**, le seguenti tre variabili vengono aggiunte all'ambiente: **% errorlevel %**, **% cmdcmdline %**, e **% cmdextversion %**.

  - **% errorlevel%**: si espande in una rappresentazione di stringa del valore corrente della variabile di ambiente ERRORLEVEL. Questa variabile presuppone che non esista già una variabile di ambiente esistente con il nome ERRORLEVEL. In caso contrario, si otterrà il valore ERRORLEVEL.

  - **% cmdcmdline%**: si espande nella riga di comando originale passata a cmd. exe prima di qualsiasi elaborazione da parte di cmd. exe. Si presuppone che non esista già una variabile di ambiente esistente con il nome CMDCMDLINE. In caso contrario, si otterrà il valore CMDCMDLINE.

  - **% cmdextversion%**: si espande nella rappresentazione di stringa del valore corrente di **cmdextversion**. Si presuppone che non esista già una variabile di ambiente esistente con il nome CMDEXTVERSION. In caso contrario, si otterrà il valore CMDEXTVERSION.

- È necessario utilizzare il **else** clausola nella stessa riga di comando dopo il **Se**.

### <a name="examples"></a>Esempi

Per visualizzare il messaggio **Impossibile trovare il file di dati se non è possibile trovare il file Product. dat**, digitare:

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

Per visualizzare il valore della variabile di ambiente ERRORLEVEL dopo l'esecuzione di un file batch, digitare le righe seguenti nel file batch:

```
goto answer%errorlevel%
:answer1
echo The program returned error level 1
goto end
:answer0
echo The program returned error level 0
goto end
:end
echo Done!
```

Per passare all'etichetta OK se il valore della variabile di ambiente ERRORLEVEL è minore o uguale a 1, digitare:

```
if %errorlevel% LEQ 1 goto okay
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando goto](goto.md)
