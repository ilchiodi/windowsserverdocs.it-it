---
title: findstr
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97cc58d2b87190c43137e8b193f0217fb98c006c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725605"
---
# <a name="findstr"></a>findstr

Cerca modelli di testo nei file.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ b|Se è all'inizio di una riga, corrisponde al modello di testo.|
|/e|Se alla fine di una riga, corrisponde al modello di testo.|
|/l|Le stringhe di ricerca letteralmente processi.|
|/r|Processi di ricercano di stringhe come espressioni regolari. Si tratta dell'impostazione predefinita.|
|/s|Cerca la directory corrente e tutte le sottodirectory.|
|/i|Ignora le lettere maiuscole e minuscole durante la ricerca della stringa.|
|/x|Stampa le righe che corrispondono esattamente.|
|/v|Consente di stampare solo le righe che non contengono una corrispondenza.|
|/n|Visualizza il numero di ogni riga corrispondente.|
|/m|Se un file contiene una corrispondenza, viene stampato solo il nome del file.|
|/o|Offset carattere viene stampato prima di ogni riga corrispondente.|
|/ p|Ignora i file con caratteri non stampabili.|
|/ [offline]|Non ignorare i file che sono impostato l'attributo non in linea.|
|/f:\<> file|Ottiene un elenco di file dal file specificato.|
|/c:\<stringa>|Utilizza il testo specificato come stringa di ricerca letterale.|
|/g:\<> file|Ottiene ricerca stringhe dal file specificato.|
|/d:\<dirlist>|Cerca l'elenco di directory specificato. Ogni directory devono essere separati da un punto e virgola (;), ad esempio `dir1;dir2;dir3`.|
|/a:\<ColorAttribute>|Specifica gli attributi di colore con due cifre esadecimali. Tipo `color /?` Per ulteriori informazioni.|
|\<Stringhe>|Specifica il testo da cercare nella *FileName*. Obbligatorio.|
|[\<Unità>:] [<Path>]<FileName>[ ...]|Specifica il percorso e un file o file da cercare. Nome di almeno un file è obbligatorio.|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

- Tutti **findstr** devono precedere le opzioni della riga di comando *stringhe* e *FileName* nella stringa di comando.
- Espressioni regolari utilizzano sia i caratteri letterali e metacaratteri per individuare modelli di testo, anziché stringhe di caratteri esatte. Un carattere letterale è un carattere che non hanno un significato speciale nella sintassi delle espressioni regolari, corrisponde a un'occorrenza del carattere desiderato. Ad esempio, lettere e numeri sono caratteri letterali. Un metacarattere è un simbolo con un significato speciale (un operatore o delimitatore) nella sintassi delle espressioni regolari.

  Nella tabella seguente sono elencati i metacaratteri che **findstr** accetta.  

  |Metacaratteri|valore|
  |-------------|-----|
  |.|Jolly: qualsiasi carattere|
  |*|Ripeti: zero o più occorrenze del carattere precedente o della classe|
  |^|Posizione della riga: inizio della riga|
  |$|Posizione della riga: fine della riga|
  |[classe]|Classe di caratteri: qualsiasi carattere in un set|
  |[^ classe]|Classe inversa: qualsiasi carattere non in un set|
  |[x-y]|Intervallo: qualsiasi carattere compreso nell'intervallo specificato|
  |\x|Escape: utilizzo letterale di una metacarattere x|
  |\\Stringa di<|Posizione nella parola: inizio della parola|
  |string\>|Posizione nella parola: fine della parola|

  I caratteri speciali nella sintassi delle espressioni regolari sono della massima potenza quando vengono utilizzati insieme. Ad esempio, utilizzare la seguente combinazione del carattere jolly (.) e ripetere il carattere (*) per cercare una stringa di caratteri:

  ```
  .*
  ``` 

  Usare l'espressione seguente come parte di un'espressione più grande per trovare la corrispondenza con qualsiasi stringa che inizia con b e termina con l'ing: 

  ```
  b.*ing
  ```

## <a name="examples"></a>Esempi

Utilizzare gli spazi per separare più stringhe di ricerca, a meno che l'argomento è preceduto da **/c**.

Per cercare Hello o Here nel file x. y, digitare:

```
findstr hello there x.y 
```

Per cercare Hello nel file x. y, digitare:

```
findstr /c:hello there x.y 
```

Per trovare tutte le occorrenze delle finestre di Word (con una lettera maiuscola iniziale W) nel file proposta. txt, digitare:

```
findstr Windows proposal.txt 
```

Per cercare tutti i file nella directory corrente e tutte le sottodirectory contenenti la parola Windows, indipendentemente dal caso lettera, digitare:

```
findstr /s /i Windows *.* 
```

Per trovare tutte le occorrenze di righe che iniziano con per e sono precedute da zero o più spazi (ad esempio in un ciclo di programma del computer) e per visualizzare il numero di riga in cui viene trovata ogni occorrenza, digitare:

```
findstr /b /n /r /c:^ *FOR *.bas 
```

Per cercare più stringhe in un set di file, creare un file di testo che contiene ogni criterio di ricerca in una riga separata. È anche possibile elencare i file che si desidera eseguire la ricerca in un file di testo. Ad esempio, per utilizzare i criteri di ricerca nel file Stringlist.txt, cercare i file elencati in Filelist. txt e quindi archiviare i risultati nel file di risultati. out, tipo:

```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```

Per elencare ogni file contenente il Word computer all'interno della directory corrente e di tutte le sottodirectory, indipendentemente dal caso, digitare:

```
findstr /s /i /m \<computer\> *.*
```

Per elencare tutti i file che contengono Word computer e altre parole che iniziano con comp, ad esempio complimento e compete, digitare:

```
findstr /s /i /m \<comp.* *.*
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)