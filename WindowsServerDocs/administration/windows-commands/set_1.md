---
title: set
description: Argomento di riferimento per set, che consente di visualizzare, impostare o rimuovere le variabili di ambiente cmd. exe.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5de835dd591e1f70acc896772df501d5ddbdf237
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819391"
---
# <a name="set"></a>set

Visualizza, imposta o rimuove le variabili di ambiente cmd. exe. Se utilizzata senza parametri, **impostare** consente di visualizzare le impostazioni delle variabili di ambiente corrente.



## <a name="syntax"></a>Sintassi

```
set [<Variable>=[<String>]]
set [/p] <Variable>=[<PromptString>]
set /a <Variable>=<Expression>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> variabile|Specifica la variabile di ambiente per impostare o modificare.|
|\<> stringa|Specifica la stringa da associare alla variabile di ambiente specificato.|
|/ p|Imposta il valore di *variabile* a una riga di input immessi dall'utente.|
|\<> StringaPrompt|Facoltativa. Specifica un messaggio per richiedere l'input dell'utente. Questo parametro viene utilizzato con il **/p** opzione della riga di comando.|
|/a|Set *stringa* su un'espressione numerica valutata.|
|\<> espressione|Specifica un'espressione numerica. Vedere la sezione Osservazioni per gli operatori validi che possono essere utilizzati in *espressione*.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

- Utilizzando **impostare** con le estensioni abilitate

  Quando sono abilitate le estensioni dei comandi (predefinito) e si esegue **impostare** con un valore, vengono visualizzate tutte le variabili che iniziano con tale valore.
- Utilizzo di caratteri speciali

  I caratteri **<** ,,, **>** **|** **&** , **^** sono caratteri speciali della shell dei comandi e devono essere preceduti dal carattere di escape ( **^** ) o racchiusi tra virgolette quando vengono usati nella *stringa* , ad esempio **StringaContenenteSimbolo&symbol**). Se si usano le virgolette per racchiudere una stringa contenente uno dei caratteri speciali, le virgolette vengono impostate come parte del valore della variabile di ambiente.
- Uso delle variabili di ambiente

  Utilizzare le variabili di ambiente per controllare il comportamento di alcuni programmi e file batch e per controllare il modo Windows e del sottosistema MS-DOS viene visualizzata e funziona. Il **impostare** comando viene spesso utilizzato nel file Autoexec per impostare le variabili di ambiente.
- Visualizzare le impostazioni dell'ambiente corrente

  Quando si digita il **impostare** comando, vengono visualizzate le impostazioni di ambiente corrente. In genere, queste impostazioni includono le variabili di ambiente COMSPEC e PATH, che vengono utilizzate per individuare i programmi sul disco. PROMPT e DIRCMD sono altre due variabili di ambiente utilizzate da Windows.
- Utilizzo di parametri

  Quando si specificano valori per *variabile* e *stringa*, specificato *variabile* valore viene aggiunto all'ambiente e *stringa* è associata a tale variabile. Se la variabile esiste già nell'ambiente, il nuovo valore stringa sostituisce il precedente valore di stringa.

  Se si specifica solo una variabile e un segno di uguale (senza *stringa*) per il **impostare** comando, il *stringa* valore associato alla variabile viene cancellata (come se la variabile non è presente).
- Uso di **/a**

  Nella tabella seguente sono elencati gli operatori supportati per **/a** in ordine decrescente di precedenza.

  |        Operatore         | Operazione eseguita  |
  |-------------------------|----------------------|
  |           ( )           |       Raggruppamento       |
  |          ! ~ -          |        Unaria         |
  |         \* / %          |      Aritmetico      |
  |           + -           |      Aritmetico      |
  |          << >>          |    Spostamento logico     |
  |            &            |     AND bit per bit      |
  |            ^            | OR esclusivo bit per bit |
  |                         |                      |
  | = \*=/=% = + =-= &= ^ = |      = <<= >>=       |
  |            ,            | Separatore di espressione |

  Se si usano gli operatori logici ( **&&** o **||** ) o modulo ( **%** ), racchiudere la stringa dell'espressione tra virgolette. Qualsiasi stringa non numerica nell'espressione è considerati i nomi delle variabili di ambiente e i relativi valori vengono convertiti in numeri prima di essere elaborati. Se si specifica un nome di variabile di ambiente che non è definito nell'ambiente corrente, viene assegnato un valore pari a zero, che consente di eseguire operazioni aritmetiche con valori di variabili di ambiente senza utilizzare % per recuperare un valore.

  Se si esegue **set /a** dalla riga di comando all'esterno di uno script di comandi, viene visualizzato il valore finale dell'espressione.

  I valori numerici sono numeri decimali, a meno che non con il prefisso × 0 per i numeri esadecimali o 0 per i numeri ottali. Di conseguenza, 0 × 12 è identico a 18, che corrisponde al 022.
- Supporto di espansione della variabile di ambiente ritardata

  Supporto di espansione delle variabili di ambiente ritardata è disabilitato per impostazione predefinita, ma è possibile abilitarla o disabilitarla utilizzando **/v cmd**.
- Utilizzo di estensioni di comando

  Quando sono abilitate le estensioni dei comandi (predefinito) e si esegue **impostare** da solo, Visualizza tutte le variabili di ambiente corrente. Se si esegue **impostare** con un valore, verranno visualizzate le variabili che corrispondono al valore.
- Utilizzando **impostare** nei file batch

  Quando si creano file batch, è possibile utilizzare **impostare** per creare variabili e quindi utilizzarli nello stesso modo all'utilizzo di variabili numerate **0** tramite **%9**. È inoltre possibile utilizzare le variabili **0** tramite **%9** come input per **impostare**.
- La chiamata a un **impostare** variabile da un file batch

  Quando si chiama un valore di variabile da un file batch, racchiudere il valore con i segni di percentuale ( **%** ). Ad esempio, se il programma batch Crea una variabile di ambiente denominata BAUD, è possibile utilizzare la stringa associata a BAUD come parametro sostituibile digitando **% baud %** al prompt dei comandi.
- Utilizzando **impostare** dalla Console di ripristino

  Il **impostare** comando con parametri diversi, è disponibile dalla Console di ripristino.

## <a name="examples"></a>Esempi

Per impostare una variabile di ambiente denominata TEST ^ 1, digitare:
```
set testVar=test^^1
```

> [!NOTE]
> Il **impostare** comando Assegna tutto ciò che segue il segno di uguale (=) al valore della variabile. Se si digita:
> ```
> set testVar=test^1
> ```
> Si ottiene il risultato seguente:
> ```
> testVar=test^1
> ```
> Per impostare una variabile di ambiente denominata TEST & 1, digitare:
> ```
> set testVar=test^&1
> ```
> Per impostare una variabile di ambiente denominata INCLUDE, in modo che la stringa C:\Inc (la directory \Inc sull'unità C) è associata, digitare:
> ```
> set include=c:\inc
> ```
> È quindi possibile usare la stringa C:\Inc nei file batch racchiudendo il nome tra i segni di percentuale ( **%** ). Ad esempio, si potrebbe includere il comando seguente in un file batch in modo che sia possibile visualizzare il contenuto della directory che è associata a una variabile di ambiente INCLUDE:
> ```
> dir %include%
> ```
> In questo comando viene elaborato, la stringa C:\Inc sostituisce **% include %**.

È inoltre possibile utilizzare **impostare** in un file batch che aggiunge una nuova directory alla variabile di ambiente PATH. Ad esempio:
```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```
Per visualizzare un elenco di tutte le variabili di ambiente che iniziano con la lettera P, digitare:
```
set p
```

> [!NOTE]
> Questo comando richiede estensioni del comando, che sono abilitati per impostazione predefinita.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)