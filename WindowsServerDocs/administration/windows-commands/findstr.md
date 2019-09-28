---
title: findstr
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 547a0abf658ef826cca8c87d451144181f8dac7d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377195"
---
# <a name="findstr"></a>findstr

Cerca modelli di testo nei file.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ b|Se è all'inizio di una riga, corrisponde al modello di testo.|
|/e|Se alla fine di una riga, corrisponde al modello di testo.|
|/l|Le stringhe di ricerca letteralmente processi.|
|/r|Processi di ricercano di stringhe come espressioni regolari. Questa è l'impostazione predefinita.|
|/s|Cerca la directory corrente e tutte le sottodirectory.|
|/i|Ignora le lettere maiuscole e minuscole durante la ricerca della stringa.|
|/x|Stampa le righe che corrispondono esattamente.|
|/v|Consente di stampare solo le righe che non contengono una corrispondenza.|
|/n|Visualizza il numero di ogni riga corrispondente.|
|/m|Se un file contiene una corrispondenza, viene stampato solo il nome del file.|
|/o|Offset carattere viene stampato prima di ogni riga corrispondente.|
|/ p|Ignora i file con caratteri non stampabili.|
|/ [offline]|Non ignorare i file che sono impostato l'attributo non in linea.|
|/f: \<File >|Ottiene un elenco di file dal file specificato.|
|/c: \<String >|Utilizza il testo specificato come stringa di ricerca letterale.|
|/g: \<File >|Ottiene ricerca stringhe dal file specificato.|
|/d: \<DirList >|Cerca l'elenco di directory specificato. Ogni directory devono essere separati da un punto e virgola (;), ad esempio `dir1;dir2;dir3`.|
|/a: \<ColorAttribute >|Specifica gli attributi di colore con due cifre esadecimali. Tipo `color /?` Per ulteriori informazioni.|
|\<Strings >|Specifica il testo da cercare nella *FileName*. Obbligatorio.|
|[\<Drive >:] [<Path>] <FileName> [...]|Specifica il percorso e un file o file da cercare. Nome di almeno un file è obbligatorio.|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- Tutti **findstr** devono precedere le opzioni della riga di comando *stringhe* e *FileName* nella stringa di comando.
- Espressioni regolari utilizzano sia i caratteri letterali e metacaratteri per individuare modelli di testo, anziché stringhe di caratteri esatte. Un carattere letterale è un carattere che non hanno un significato speciale nella sintassi delle espressioni regolari, corrisponde a un'occorrenza del carattere desiderato. Ad esempio, lettere e numeri sono caratteri letterali. Un metacarattere è un simbolo con un significato speciale (un operatore o delimitatore) nella sintassi delle espressioni regolari.

  Nella tabella seguente sono elencati i metacaratteri che **findstr** accetta.  

  |Metacaratteri|Value|
  |-------------|-----|
  |.|Jolly: qualsiasi carattere|
  |*|Ripeti: zero o più occorrenze del carattere precedente o della classe|
  |^|Posizione della riga: inizio della riga|
  |$|Posizione della riga: fine della riga|
  |[classe]|Classe di caratteri: qualsiasi carattere in un set|
  |[^ classe]|Classe inversa: qualsiasi carattere non in un set|
  |[x-y]|Intervallo: qualsiasi carattere compreso nell'intervallo specificato|
  |\x|Escape: utilizzo letterale di una metacarattere x|
  |stringa \\ <|Posizione nella parola: inizio della parola|
  |stringa @ no__t-0|Posizione nella parola: fine della parola|

  I caratteri speciali nella sintassi delle espressioni regolari sono della massima potenza quando vengono utilizzati insieme. Ad esempio, utilizzare la seguente combinazione del carattere jolly (.) e ripetere il carattere (*) per cercare una stringa di caratteri:

  ```
  .*
  ``` 

  Utilizzare l'espressione seguente come parte di un'espressione più ampia per trovare qualsiasi stringa che inizia con "b" e termina con "ing": 

  ```
  b.*ing
  ```

## <a name="examples"></a>Esempi

Utilizzare gli spazi per separare più stringhe di ricerca, a meno che l'argomento è preceduto da **/c**.

Per eseguire la ricerca di "hello" o "there" nel file x. y, digitare:

```
findstr "hello there" x.y 
```

Per cercare "buona notte" nel file x. y, digitare:

```
findstr /c:"hello there" x.y 
```

Per trovare tutte le occorrenze della parola "Windows" (con iniziale maiuscola W) nel file proposta. txt, digitare:

```
findstr Windows proposal.txt 
```

Per cercare tutti i file nella directory corrente e tutte le sottodirectory contenenti la parola Windows, indipendentemente dal caso lettera, digitare:

```
findstr /s /i Windows *.* 
```

Per trovare tutte le righe che iniziano con "FOR" e sono precedute da zero o più spazi (ad esempio un ciclo di programma del computer) e per visualizzare il numero di riga in cui si trova ogni occorrenza, digitare:

```
findstr /b /n /r /c:"^ *FOR" *.bas 
```

Per cercare più stringhe in un set di file, creare un file di testo che contiene ogni criterio di ricerca in una riga separata. È anche possibile elencare i file che si desidera eseguire la ricerca in un file di testo. Ad esempio, per utilizzare i criteri di ricerca nel file Stringlist.txt, cercare i file elencati in Filelist. txt e quindi archiviare i risultati nel file di risultati. out, tipo:

```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```

Per elencare tutti i file contenenti la parola "computer" all'interno della directory corrente e tutte le sottodirectory, indipendentemente dal caso, digitare:

```
findstr /s /i /m "\<computer\>" *.*
```

Per elencare tutti i file che contiene la parola "computer" e altre parole che iniziano con "comp", (ad esempio "apprezzamento" e "competizione"), tipo:

```
findstr /s /i /m "\<comp.*" *.*
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)