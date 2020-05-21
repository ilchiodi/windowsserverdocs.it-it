---
title: per
description: Argomento di riferimento per il comando for, che esegue un comando specificato per ogni file, all'interno di un set di file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e275726c-035f-4a74-8062-013c37f5ded1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24ef5bc159e67862d419bd2728b14585f8b095d4
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437016"
---
# <a name="for"></a>per

Esegue un comando specificato per ogni file, all'interno di un set di file.

## <a name="syntax"></a>Sintassi

```
for {%% | %}<variable> in (<set>) do <command> [<commandlineoptions>]
```

### <a name="parameters"></a>Parametri

| Parametro | Description |
| --------- | ----------- |
| `{%% | %}<variable>` | Obbligatorio. Rappresenta un parametro sostituibile. Usare un solo segno di percentuale ( `%` ) per eseguire il comando **for** al prompt dei comandi. Utilizzare i segni di percentuale (`%%`) per eseguire il **per** comando all'interno di un file batch. Le variabili fanno distinzione tra maiuscole e minuscole e devono essere rappresentate con un valore alfabetico, ad esempio **% a**, **% b**o **% c**. |
| (`<set>`) | Obbligatorio. Specifica uno o più file, directory, o stringhe di testo o un intervallo di valori su cui eseguire il comando. È necessario utilizzare le parentesi. |
| `<command>` | Obbligatorio. Specifica il comando che si desidera eseguire in ogni file, directory o stringa di testo oppure nell'intervallo di valori inclusi nel *set*. |
| `<commandlineoptions>` | Specifica le opzioni della riga di comando che si desidera utilizzare con il comando specificato. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- È possibile utilizzare questo comando in un file batch o direttamente dal prompt dei comandi.

- Gli attributi seguenti si applicano al comando **for** :

  - Questo comando sostituisce `% variable` o `%% variable` con ogni stringa di testo nel set specificato fino a quando il comando specificato non elabora tutti i file.

  - I nomi delle variabili sono tra maiuscole e minuscole, globale e non superiore a 52 possono essere attive contemporaneamente.

  - Per evitare confusione con i parametri di batch, `%0` tramite `%9` è possibile utilizzare qualsiasi carattere per la *variabile* eccetto i numeri da **0** a **9**. Per i file batch semplici, un singolo carattere come funzionerà `%%f` .

  - È possibile usare più valori per la *variabile* in file batch complessi per distinguere le diverse variabili sostituibili.

- Il parametro *set* può rappresentare un singolo gruppo di file o diversi gruppi di file. È possibile utilizzare caratteri jolly (**&#42;** e **?**) per specificare un set di file. Di seguito sono insiemi di file valido.

  ```
  (*.doc)
  (*.doc *.txt *.me)
  (jan*.doc jan*.rpt feb*.doc feb*.rpt)
  (ar??1991.* ap??1991.*)
  ```

- Quando si usa questo comando, il primo valore in *set* sostituisce `% variable` o `%% variable` , quindi il comando specificato elabora questo valore. Questa operazione continua fino a quando non vengono elaborati tutti i file (o gruppi di file) che corrispondono al valore *impostato* .

- **In** e **non** sono parametri, ma è necessario utilizzarli con questo comando. Se si omette una di queste parole chiave, viene visualizzato un messaggio di errore.

- Se sono abilitate le estensioni dei comandi (impostazione predefinita), sono supportate le seguenti forme aggiuntive di **per** :

  - **Solo directory:** Se *set* contiene caratteri jolly (**&#42;** o **?**), il *comando* specificato viene eseguito per ogni directory (invece di un set di file in una directory specificata) che corrisponde a *set*. La sintassi è:

    ```
    for /d {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    ```

  - **Ricorsivo:** Consente di esaminare l'albero di directory radice in *unità*:*percorso* ed eseguire l'istruzione **for** in ogni directory dell'albero. Se viene specificata alcuna directory dopo **/r**, la directory corrente viene utilizzata come directory radice. Se *set* è solo un punto (.), enumera solo l'albero di directory. La sintassi è:

    ```
    for /r [[<drive>:]<path>] {%%|%}<variable> in (<set>) do <command> [<commandlinepptions>]
    ```

  - **Iterazione di un intervallo di valori:** Usare una variabile iterativa per impostare il valore iniziale (*Start*#), quindi eseguire un'istruzione alla volta in un intervallo di valori impostato fino a quando il valore non supera il valore finale impostato (*fine*#). **/l** eseguirà il iterativo confrontando *Start*# con *end*#. Se *Start*# è minore di *end*#, il comando verrà eseguito. Quando la variabile iterativa supera *end*#, la shell dei comandi chiude il ciclo. È anche possibile usare un *passaggio*negativo # per scorrere un intervallo in valori decrescenti. Ad esempio, (1,1,5) genera la sequenza 1 2 3 4 5 e (5,-1,1) genera la sequenza 1 2 3 4 di 5. La sintassi è:

    ```
    for /l {%%|%}<variable> in (<start#>,<step#>,<end#>) do <command> [<commandlinepptions>]
    ```

  - **Iterazione e analisi dei file:** Usare l'analisi dei file per elaborare l'output del comando, le stringhe e il contenuto del file. Utilizzare variabili iterative per definire il contenuto o le stringhe che si desidera esaminare e utilizzare le varie opzioni *ParolechiaveAnalisi* per modificare ulteriormente l'analisi.  Usare l'opzione token *ParolechiaveAnalisi* per specificare quali token devono essere passati come variabili iterative. Si noti che quando viene utilizzata senza l'opzione token **/f** esaminerà solo il primo token.

    L'analisi del file è costituito da leggere l'output, stringa o il contenuto del file, quindi suddividerlo in singole righe di testo e l'analisi di ogni riga in zero o più token. Il **per** ciclo viene quindi chiamato con il valore della variabile iterativo impostato sul token. Per impostazione predefinita, **/f** passa il primo spazio separato token da ogni riga di ogni file. Le righe vuote vengono ignorate.

    La sintassi sono:

    ```
    for /f [<parsingkeywords>] {%%|%}<variable> in (<set>) do <command> [<commandlinepptions>]
    for /f [<parsingkeywords>] {%%|%}<variable> in (<literalstring>) do <command> [<commandlinepptions>]
    for /f [<parsingkeywords>] {%%|%}<variable> in ('<command>') do <command> [<commandlinepptions>]
    ```

    L'argomento *set* specifica uno o più nomi di file. Ogni file viene aperto, letto ed elaborato prima del passaggio al file successivo nel *set*. Per eseguire l'override del comportamento di analisi predefinito, specificare *ParolechiaveAnalisi*. Si tratta di una stringa tra virgolette contenente uno o più parole chiave per specificare diverse opzioni di analisi.

    Se si utilizza il **usebackq** opzione, utilizzare una delle sintassi seguenti:

    ```
    for /f [usebackq <parsingkeywords>] {%%|%}<variable> in (<Set>) do <command> [<commandlinepptions>]
    for /f [usebackq <parsingkeywords>] {%%|%}<variable> in ('<LiteralString>') do <command> [<commandlinepptions>]
    for /f [usebackq <parsingkeywords>] {%%|%}<variable> in (`<command>`) do <command> [<commandlinepptions>]
    ```

    La tabella seguente elenca le parole chiave di analisi che è possibile usare per *ParolechiaveAnalisi*.

    | Parola chiave | Descrizione |
    | ------- | ----------- |
    | EOL =`<c>` | Specifica un carattere di fine riga (un solo carattere). |
    | Ignora =`<n>` | Specifica il numero di righe da ignorare all'inizio del file. |
    | delims =`<xxx>` | Specifica un set di delimitatori. Sostituisce il set di delimitatori predefinito di spazio e tabulazione. |
    | token =`<x,y,m–n>` | Specifica i token da ogni riga deve essere passato il **per** ciclo per ogni iterazione. Di conseguenza, i nomi delle variabili aggiuntive vengono allocati. *m-n* specifica un intervallo, dal *m*° attraverso i *n*token. Se l'ultimo carattere nella stringa **Tokens =** è un asterisco (**&#42;**), viene allocata una variabile aggiuntiva che riceve il testo rimanente sulla riga dopo l'ultimo token analizzato. |
    | usebackq | Specifica di eseguire una stringa con virgolette indietro come comando, utilizzare una stringa racchiusa tra virgolette singole come stringa letterale oppure, per i nomi di file lunghi che contengono spazi, consentire i nomi di file in `<set>` a ognuno di essi racchiusi tra virgolette doppie. |

  - **Sostituzione variabile:** La tabella seguente elenca la sintassi facoltativa (per qualsiasi variabile **i**):

    | Variabile con modificatore | Descrizione |
    | ---------------------- | ----------- |
    |` %~I` | Espande `%I` che rimuove tutte le virgolette circostanti. |
    | `%~fI `| Si espande `%I` in un nome di percorso completo. |
    | `%~dI `| Si espande `%I` solo a una lettera di unità. |
    | `%~pI` | Si espande `%I` solo in un percorso. |
    | `%~nI `| Si espande `%I` solo in un nome file. |
    | `%~xI` | Si espande `%I` solo in un'estensione del nome di file. |
    | `%~sI` | Espande percorso che contenga solo i nomi brevi. |
    | `%~aI` | Si espande `%I` agli attributi del file. |
    | `%~tI` | Si espande `%I` alla data e all'ora del file. |
    | `%~zI` | Si espande `%I` fino alla dimensione del file. |
    | `%~$PATH:I` | Cerca nelle directory elencate nella variabile di ambiente PATH ed espande `%I` il nome completo della prima directory trovata. Se il nome di variabile di ambiente non è definito o non è possibile trovare il file con la ricerca, questo modificatore si espande in una stringa vuota. |

    Nella tabella seguente sono elencate le combinazioni di modificatori che è possibile utilizzare per ottenere risultati composti.

    | Variabile con modificatori combinati | Descrizione |
    | -------------------------------- | ----------- |
    | `%~dpI `| Si espande `%I` solo a una lettera di unità e a un percorso. |
    | `%~nxI` | Si espande `%I` solo con un nome file e un'estensione. |
    | `%~fsI` | Si espande `%I` in un nome di percorso completo con solo nomi brevi. |
    | `%~dp$PATH:I` | Esegue la ricerca nelle directory elencate nella variabile di ambiente PATH per `%I` ed espande la lettera di unità e il percorso del primo oggetto trovato. |
    | `%~ftzaI` | Si espande `%I` in una riga di output simile a **dir**. |

    Negli esempi precedenti è possibile sostituire e il `%I` percorso con altri valori validi. Un oggetto valido **per** il nome della variabile termina la **%~** sintassi.

    Usando nomi di variabili maiuscole, ad esempio `%I` , è possibile rendere il codice più leggibile ed evitare confusione con i modificatori, che non fanno distinzione tra maiuscole e minuscole.

- **Analisi di una stringa:** È possibile usare la `for /f` logica di analisi su una stringa immediata eseguendo il wrapping `<literalstring>` in: virgolette doppie (*senza* usebackq) o tra virgolette singole (*con* usebackq), ad esempio (stringa) o (' stringa '). `<literalstring>`viene considerato come una singola riga di input da un file. Quando `<literalstring>` si esegue l'analisi tra virgolette doppie, i simboli dei comandi (ad esempio, `\ & | > < ^` ) vengono trattati come caratteri ordinari.

- **Output di analisi:** È possibile utilizzare il `for /f` comando per analizzare l'output di un comando inserendo tra virgolette `<command>` tra parentesi. Viene considerato come una riga di comando, che viene passata a un elemento figlio Cmd.exe. L'output viene acquisito in memoria e analizzato come se si tratta di un file.

## <a name="examples"></a>Esempi

Utilizzare **per** in un file batch, utilizzare la sintassi seguente:

```
for {%%|%}<variable> in (<set>) do <command> [<commandlineoptions>]
```

Per visualizzare il contenuto di tutti i file nella directory corrente che hanno l'estensione doc o txt utilizzando la variabile sostituibile **%f**, tipo:

```
for %f in (*.doc *.txt) do type %f
```

Nell'esempio precedente, ogni file con estensione doc o txt nella directory corrente viene sostituito con il **%f** variabile fino a quando non viene visualizzato il contenuto di ogni file. Per utilizzare questo comando in un file batch, sostituire ogni occorrenza di **%f** con **% %**. In caso contrario, la variabile viene ignorata e viene visualizzato un messaggio di errore.

Per analizzare un file, ignorando le righe di commento, tipo:

```
for /f eol=; tokens=2,3* delims=, %i in (myfile.txt) do @echo %i %j %k
```

Questo comando analizza ogni riga in *MyFile. txt*. Ignora le righe che iniziano con un punto e virgola e passa il token secondo e terzo da ogni riga per il **per** corpo (i token vengono separati da virgole o spazi). Il corpo del **per** istruzione fa riferimento **avvii** per ottenere il token, il secondo **%j** per ottenere il terzo token e **%k** per ottenere tutti i token rimanenti. Se i nomi file forniti contengono spazi, racchiudere il testo tra virgolette, ad esempio il nome del file. Per utilizzare le virgolette, è necessario utilizzare **usebackq**. In caso contrario, le virgolette doppie vengono interpretate come definizione di una valore letterale stringa da analizzare.

**%i** viene dichiarato in modo esplicito nel **per** istruzione. **%j** e **%k** in modo implicito vengono dichiarate utilizzando **token =**. È possibile utilizzare i **token =** per specificare fino a 26 token, a condizione che non venga eseguito un tentativo di dichiarare una variabile maggiore della lettera Z o z.

Per analizzare l'output di un comando inserendo il *set* tra parentesi, digitare:

```
for /f usebackq delims== %i in ('set') do @echo %i
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
