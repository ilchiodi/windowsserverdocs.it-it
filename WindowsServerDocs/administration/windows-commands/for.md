---
title: per
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e275726c-035f-4a74-8062-013c37f5ded1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7040e4cb8e0f38e58ce5e868535dcfb2d897fbd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844534"
---
# <a name="for"></a>per



Esegue il comando specificato per ogni file in un set di file.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|{%%\|%}\<variabile >|Obbligatoria. Rappresenta un parametro sostituibile. Usare un solo segno di percentuale ( **%** ) per eseguire il comando **for** al prompt dei comandi. Utilizzare i segni di percentuale ( **%%** ) per eseguire il **per** comando all'interno di un file batch. Le variabili sono tra maiuscole e minuscole e devono essere rappresentati con un valore alfabetico, ad esempio **%a**, **%b**, o **%c**.|
|(\<set >)|Obbligatoria. Specifica uno o più file, directory, o stringhe di testo o un intervallo di valori su cui eseguire il comando. Le parentesi sono obbligatorie.|
|Comando \<>|Obbligatoria. Specifica il comando che si desidera eseguire orizzontale su ogni file, directory o stringa di testo o sull'intervallo di valori inclusi nel *impostare*.|
|\<OpzioniRigaComando >|Specifica le opzioni della riga di comando che si desidera utilizzare con il comando specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- Utilizzo **di per**

  È possibile usare il comando **for** in un file batch o direttamente dal prompt dei comandi.
- Uso dei parametri batch

  Gli attributi seguenti si applicano al comando **for** :  
  - Il **per** comando sostituisce **%** <em>variabile</em> o **%%** <em>variabile</em> con ogni stringa di testo nel set specificato fino a quando il comando specificato elabora tutti i file.
  - I nomi delle variabili sono tra maiuscole e minuscole, globale e non superiore a 52 possono essere attive contemporaneamente.
  - Per evitare confusione con i parametri batch **0** tramite **%9**, è possibile utilizzare qualsiasi carattere per *variabile* tranne i numeri da 0 a 9. Per semplici file batch, un singolo carattere, ad esempio **;** funzionerà.
  - È possibile utilizzare più valori per *variabile* nei file batch complessi per distinguere le diverse variabili sostituibili.
- Specifica di un gruppo di file

  Il parametro *set* può rappresentare un singolo gruppo di file o diversi gruppi di file. È possibile utilizzare caratteri jolly ( **&#42;** e **?** ) per specificare un set di file. Di seguito sono insiemi di file valido.  
  ```
  (*.doc) 
  (*.doc *.txt *.me)
  (jan*.doc jan*.rpt feb*.doc feb*.rpt)
  (ar??1991.* ap??1991.*)
  ```  
  Quando si utilizza il **per** comando, il primo valore in *impostare* sostituisce **%** <em>variabile</em> o **%%** <em>variabile</em>, e quindi il comando specificato elabora questo valore. Il processo continua finché tutti i file (o gruppi di file) che corrispondono al *impostare* valore vengono elaborati.
- Uso delle parole chiave **in** e **do**

  **In** e **do** non sono parametri, ma è necessario utilizzarli con **per**. Se si omette una di queste parole chiave, viene visualizzato un messaggio di errore.
- Uso di forme aggiuntive di **per**

  Se sono abilitate le estensioni dei comandi (impostazione predefinita), sono supportate le seguenti forme aggiuntive di **per** :  
  - Solo directory

    Se *set* contiene caratteri jolly ( **&#42;** o **?** ), il *comando* specificato viene eseguito per ogni directory (invece di un set di file in una directory specificata) che corrisponde a *set*.

    La sintassi è:  
    ```
    for /d {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>] 
    ```  
  - Ricorsivo

    Consente di esaminare l'albero di directory radice in *unità*:*percorso* ed eseguire l'istruzione **for** in ogni directory dell'albero. Se viene specificata alcuna directory dopo **/r**, la directory corrente viene utilizzata come directory radice. Se *impostare* è costituito solo da un punto (.), enumera solo la struttura di directory.

    La sintassi è:  
    ```
    for /r [[<Drive>:]<Path>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    ```  
  - Iterazione di un intervallo di valori

    Usare una variabile iterativa per impostare il valore iniziale (*Start*#), quindi eseguire un'istruzione alla volta in un intervallo di valori impostato fino a quando il valore non supera il valore finale impostato (*fine*#). **/l** eseguirà l'iterazione confrontando *avviare*# con *Fine*#. Se *avviare*# è minore di *End*# eseguirà il comando. Quando la variabile iterativa supera *End*#, la shell dei comandi di uscire dal ciclo. È inoltre possibile utilizzare un valore negativo *passaggio*# per scorrere un intervallo di valori decrescenti. Ad esempio, (1,1,5) genera la sequenza 1 2 3 4 5 e (5,-1,1) genera la sequenza 1 2 3 4 di 5.

    La sintassi è:  
    ```
    for /l {%%|%}<Variable> in (<Start#>,<Step#>,<End#>) do <Command> [<CommandLineOptions>]
    ```  
  - Iterazione e analisi dei file

    Usare l'analisi dei file per elaborare l'output del comando, le stringhe e il contenuto del file.  Utilizzare le variabili iterative per definire il contenuto o le stringhe che si desidera esaminare e utilizzare i vari *ParolechiaveAnalisi* le opzioni per modificare ulteriormente l'analisi.  Utilizzare il *ParolechiaveAnalisi* il token di opzione per specificare quali token devono essere passati come variabili iterative. Si noti che quando viene utilizzata senza l'opzione token **/f** esaminerà solo il primo token.

    L'analisi del file è costituito da leggere l'output, stringa o il contenuto del file, quindi suddividerlo in singole righe di testo e l'analisi di ogni riga in zero o più token. Il **per** ciclo viene quindi chiamato con il valore della variabile iterativo impostato sul token. Per impostazione predefinita, **/f** passa il primo spazio separato token da ogni riga di ogni file. Le righe vuote vengono ignorate.

    La sintassi sono:  
    ```
    for /f [<ParsingKeywords>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    for /f [<ParsingKeywords>] {%%|%}<Variable> in (<LiteralString>) do <Command> [<CommandLineOptions>]
    for /f [<ParsingKeywords>] {%%|%}<Variable> in ('<Command>') do <Command> [<CommandLineOptions>]
    ```  
    Il *impostare* argomento specifica uno o più nomi di file. Ogni file è aperto, leggere ed elaborati prima di passare al file successivo contenuto *impostare*. Per ignorare il comportamento di analisi predefinito, specificare *ParolechiaveAnalisi*. Si tratta di una stringa tra virgolette contenente uno o più parole chiave per specificare diverse opzioni di analisi.

    Se si utilizza il **usebackq** opzione, utilizzare una delle sintassi seguenti:  
    ```
    for /f [usebackq <ParsingKeywords>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    for /f [usebackq <ParsingKeywords>] {%%|%}<Variable> in ('<LiteralString>') do <Command> [<CommandLineOptions>]
    for /f [usebackq <ParsingKeywords>] {%%|%}<Variable> in (`<Command>`) do <Command> [<CommandLineOptions>]
    ```  
    Nella tabella seguente sono elencate le parole chiave di analisi che è possibile utilizzare per *ParolechiaveAnalisi*.  

    |      Parola chiave      |                                                                                                                                                                                                          Descrizione                                                                                                                                                                                                          |
    |-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |     EOL =\<c >      |                                                                                                                                                                                   Specifica un carattere di fine riga (un solo carattere).                                                                                                                                                                                    |
    |     Ignora =\<N >     |                                                                                                                                                                              Specifica il numero di righe da ignorare all'inizio del file.                                                                                                                                                                              |
    |   delims =\<xxx >   |                                                                                                                                                                     Specifica un set di delimitatori. Sostituisce il set di delimitatori predefinito di spazio e tabulazione.                                                                                                                                                                      |
    | token =\<X, Y, M – N > | Specifica i token da ogni riga deve essere passato il **per** ciclo per ogni iterazione. Di conseguenza, i nomi delle variabili aggiuntive vengono allocati. *M*–*N* Specifica un intervallo tra il *M*th tramite il *N*token. Se l'ultimo carattere nella stringa **Tokens =** è un asterisco ( **&#42;** ), viene allocata una variabile aggiuntiva che riceve il testo rimanente nella riga dopo l'ultimo token analizzato. |
    |     usebackq      |                                                                                             Specifica: eseguire una stringa con virgolette indietro come comando, utilizzare una stringa racchiusa tra virgolette singole come stringa letterale oppure, per i nomi di file lunghi che contengono spazi, consentire i nomi di file in *\<Set\>* , a ognuno di essi racchiuso tra virgolette doppie.                                                                                              |


  - Sostituzione di variabili

    La tabella seguente elenca la sintassi facoltativa (per qualsiasi variabile **i**).  

    |Variabile con modificatore|Descrizione|
    |----------------------|-----------|
    |% ~ I|Espande il segno **% i** che rimuove le virgolette circostanti ().|
    |% ~ fI|Si espande **avvii** in un percorso completo.|
    |% ~ inserimento delle dipendenze|Si espande **avvii** per solo una lettera di unità.|
    |% ~ pi greco|Si espande **avvii** solo in un percorso.|
    |% ~ nI|Si espande **avvii** a solo un nome di file.|
    |% ~ xI|Si espande **avvii** solo un'estensione del nome file.|
    |% ~ sI|Espande percorso che contenga solo i nomi brevi.|
    |% ~ aI|Si espande **avvii** per gli attributi del file del file.|
    |% ~ tI|Si espande **avvii** alla data e ora del file.|
    |% ~ zI|Si espande **avvii** per le dimensioni del file.|
    |% ~ $PATH: I|Cerca directory elencate nella variabile di ambiente PATH ed espande **avvii** per il nome completo della directory prima disponibile. Se il nome di variabile di ambiente non è definito o non è possibile trovare il file con la ricerca, questo modificatore si espande in una stringa vuota.|

    Nella tabella seguente sono elencate le combinazioni di modificatori che è possibile utilizzare per ottenere risultati composti.  

    |Variabile con modificatori combinati|Descrizione|
    |--------------------------------|-----------|
    |% ~ dpI|Si espande **avvii** a una lettera di unità e un solo percorso.|
    |% ~ nxI|Si espande **avvii** a un nome di file e solo l'estensione.|
    |% ~ fsI|Si espande **avvii** in un percorso completo con solo nomi brevi.|
    |% ~ dp PERCORSO$: I|Cerca le directory in cui sono elencate nella variabile di ambiente PATH per **%i** ed espande la lettera di unità e percorso della prima occorrenza trovata.|
    |% ~ ftzaI|Si espande **avvii** a una riga di output è simile a **dir**.|

    Negli esempi precedenti, è possibile sostituire **%i** e il PERCORSO con altri valori validi. Un valido **per** nome di variabile termina il **%~** sintassi.

    Tramite i nomi delle variabili lettere maiuscole, ad esempio **%i**, è possibile rendere il codice più leggibile ed evitare confusione con i modificatori, che non distinguono tra maiuscole e minuscole.
- Analisi di una stringa

  È possibile usare la logica di analisi **per/f** su una stringa immediata eseguendo il wrapping di *\<LiteralString\>* in: virgolette doppie (*senza* usebackq) o tra virgolette singole (*con* usebackq), ad esempio (String) o (' String '). *\<\>LiteralString* viene considerato come una singola riga di input da un file. Quando si analizzano *\<LiteralString\>* tra virgolette doppie, i simboli dei comandi, ad esempio **\\ \& \|** \> \< \^, vengono considerati caratteri ordinari.
- Analisi dell'output

  È possibile utilizzare il comando **per/f** per analizzare l'output di un comando inserendo un comando racchiuso tra virgolette *\<\>* tra parentesi. Viene considerato come una riga di comando, che viene passata a un elemento figlio Cmd.exe. L'output viene acquisito in memoria e analizzato come se si tratta di un file.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Utilizzare **per** in un file batch, utilizzare la sintassi seguente:
```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```
Per visualizzare il contenuto di tutti i file nella directory corrente che hanno l'estensione doc o txt utilizzando la variabile sostituibile **%f**, tipo:
```
for %f in (*.doc *.txt) do type %f 
```
Nell'esempio precedente, ogni file con estensione doc o txt nella directory corrente viene sostituito con il **%f** variabile fino a quando non viene visualizzato il contenuto di ogni file. Per utilizzare questo comando in un file batch, sostituire ogni occorrenza di **%f** con **% %** . In caso contrario, la variabile viene ignorata e viene visualizzato un messaggio di errore.

Per analizzare un file, ignorando le righe di commento, tipo:
```
for /f eol=; tokens=2,3* delims=, %i in (myfile.txt) do @echo %i %j %k
```
Questo comando consente di analizzare ogni riga in MyFile. txt. Ignora le righe che iniziano con un punto e virgola e passa il token secondo e terzo da ogni riga per il **per** corpo (i token vengono separati da virgole o spazi). Il corpo del **per** istruzione fa riferimento **avvii** per ottenere il token, il secondo **%j** per ottenere il terzo token e **%k** per ottenere tutti i token rimanenti. Se i nomi file forniti contengono spazi, racchiudere il testo tra virgolette, ad esempio il nome del file. Per utilizzare le virgolette, è necessario utilizzare **usebackq**. In caso contrario, le virgolette doppie vengono interpretate come definizione di una valore letterale stringa da analizzare.

**%i** viene dichiarato in modo esplicito nel **per** istruzione. **%j** e **%k** in modo implicito vengono dichiarate utilizzando **token =** . È possibile utilizzare i **token =** per specificare fino a 26 token, a condizione che non venga eseguito un tentativo di dichiarare una variabile maggiore della lettera Z o z.

Nell'esempio seguente enumera i nomi delle variabili di ambiente nell'ambiente corrente. Per analizzare l'output di un comando inserendo *impostare* tra parentesi, digitare:
```
for /f usebackq delims== %i in ('set') do @echo %i 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
