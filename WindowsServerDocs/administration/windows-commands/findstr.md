---
title: findstr
description: Argomento di riferimento per il comando findstr, che consente di cercare modelli di testo nei file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f8d353b6d3aee77960b208d89372aee5dca07e3
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436117"
---
# <a name="findstr"></a>findstr

Cerca modelli di testo nei file.

## <a name="syntax"></a>Sintassi

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<file>] [/c:<string>] [/g:<file>] [/d:<dirlist>] [/a:<colorattribute>] [/off[line]] <strings> [<drive>:][<path>]<filename>[ ...]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| / b | Se è all'inizio di una riga, corrisponde al modello di testo. |
| /e | Se alla fine di una riga, corrisponde al modello di testo. |
| /l | Le stringhe di ricerca letteralmente processi. |
| /r | Processi di ricercano di stringhe come espressioni regolari. Si tratta dell'impostazione predefinita. |
| /s | Cerca la directory corrente e tutte le sottodirectory. |
| /i | Ignora le lettere maiuscole e minuscole durante la ricerca della stringa. |
| /x | Stampa le righe che corrispondono esattamente. |
| /v | Stampa solo le righe che non contengono una corrispondenza. |
| /n | Visualizza il numero di ogni riga corrispondente. |
| /m | Se un file contiene una corrispondenza, viene stampato solo il nome del file. |
| /o | Offset carattere viene stampato prima di ogni riga corrispondente. |
| / p | Ignora i file con caratteri non stampabili. |
| / [offline] | Non ignorare i file che sono impostato l'attributo non in linea. |
| /f`<file>` | Ottiene un elenco di file dal file specificato. |
| /c`<string>` | Utilizza il testo specificato come stringa di ricerca letterale. |
| /g`<file>` | Ottiene ricerca stringhe dal file specificato. |
| /d`<dirlist>` | Cerca l'elenco di directory specificato. Ogni directory devono essere separati da un punto e virgola (;), ad esempio `dir1;dir2;dir3`. |
| /a`<colorattribute>` | Specifica gli attributi di colore con due cifre esadecimali. Tipo `color /?` Per ulteriori informazioni. |
| `<strings>` | Specifica il testo da cercare nel *nome file*. Obbligatorio. |
| `[\<drive>:][<path>]<filename>[ ...]` | Specifica il percorso e un file o file da cercare. Nome di almeno un file è obbligatorio. |
| /? | Visualizza la Guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Tutte le opzioni della riga di comando **findstr** devono precedere le *stringhe* e il *nome file* nella stringa di comando.

- Le espressioni regolari usano sia i caratteri letterali che i metadati per trovare modelli di testo, anziché stringhe esatte di caratteri.

  - Un carattere letterale è un carattere che non ha un significato speciale nella sintassi delle espressioni regolari. corrisponde invece a un'occorrenza di tale carattere. Ad esempio, lettere e numeri sono caratteri letterali.

  - Un metacarattere è un simbolo con un significato speciale, ovvero un operatore o un delimitatore, nella sintassi delle espressioni regolari.

    I metadati accettati sono:

    | Meta-carattere | Value |
    | -------------- | ----- |
    | `.` | Carattere **jolly** -qualsiasi carattere |
    | `*` | **Repeat** -zero o più occorrenze del carattere o della classe precedente. |
    | `^` | **Posizione iniziale della riga** : inizio della riga. |
    | `$` | **Posizione finale riga** -fine della riga. |
    | `[class]` | **Classe Character** : qualsiasi carattere in un set. |
    | `[^class]` | **Classe inversa** : qualsiasi carattere non presente in un set. |
    | `[x-y]` | **Range** : qualsiasi carattere compreso nell'intervallo specificato. |
    | `\x` | **Escape** : utilizzo letterale di un metacarattere. |
    | `<string` | **Inizio posizione parola** -inizio della parola. |
    | `string>` | Fine **posizione parola** -fine della parola. |

    I caratteri speciali nella sintassi delle espressioni regolari sono della massima potenza quando vengono utilizzati insieme. Usare, ad esempio, la combinazione dei caratteri jolly ( `.` ) e REPEAT ( `*` ) per trovare la corrispondenza con qualsiasi stringa di caratteri:`.*`

    Usare l'espressione seguente come parte di un'espressione più grande per trovare la corrispondenza con qualsiasi stringa che inizia con *b* e termina con l' *ing*:`b.*ing`

- Per cercare più stringhe in un set di file, è necessario creare un file di testo contenente ogni criterio di ricerca su una riga distinta.

- Utilizzare gli spazi per separare più stringhe di ricerca, a meno che l'argomento è preceduto da **/c**.

### <a name="examples"></a>Esempi

Per cercare *Hello* o *here* nel file *x. y*, digitare:

```
findstr hello there x.y
```

Per cercare *Hello* nel file *x. y*, digitare:

```
findstr /c:hello there x.y
```

Per trovare tutte le occorrenze delle *finestre* di Word (con una lettera maiuscola iniziale W) nel file *proposta. txt*, digitare:

```
findstr Windows proposal.txt
```

Per eseguire la ricerca in tutti i file nella directory corrente e in tutte le sottodirectory che contengono la parola *Windows*, indipendentemente dalla lettera maiuscola, digitare:

```
findstr /s /i Windows *.*
```

Per trovare tutte le occorrenze di righe che iniziano con *per* e sono precedute da zero o più spazi (ad esempio in un ciclo di programma del computer) e per visualizzare il numero di riga in cui viene trovata ogni occorrenza, digitare:

```
findstr /b /n /r /c:^ *FOR *.bas
```

Per elencare i file esatti in cui si desidera eseguire la ricerca in un file di testo, utilizzare i criteri di ricerca nel file *String List. txt*per cercare i file elencati in *FileList. txt*e quindi archiviare i risultati nel file *results. out*, digitare:

```
findstr /g:stringlist.txt /f:filelist.txt > results.out
```

Per elencare ogni file contenente il Word *computer* all'interno della directory corrente e di tutte le sottodirectory, indipendentemente dal caso, digitare:

```
findstr /s /i /m <computer> *.*
```

Per elencare tutti i file che contengono Word computer e altre parole che iniziano con comp, ad esempio complimento e compete, digitare:

```
findstr /s /i /m <comp.* *.*
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)