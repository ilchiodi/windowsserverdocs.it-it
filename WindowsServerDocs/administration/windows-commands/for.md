---
title: per
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e275726c-035f-4a74-8062-013c37f5ded1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13a44bc3497b44d60bd4d351e389d493a50f1269
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869462"
---
# <a name="for"></a>per



Esegue il comando specificato per ogni file in un set di file.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|{%%\|%}\<Variable>|Obbligatorio. Rappresenta un parametro sostituibile. Utilizzare un simbolo di percentuale singolo (**%**) per svolgere il **per** comando al prompt dei comandi. Utilizzare i segni di percentuale (**%%**) per eseguire il **per** comando all'interno di un file batch. Le variabili sono tra maiuscole e minuscole e devono essere rappresentati con un valore alfabetico, ad esempio **%a**, **%b**, o **%c**.|
|(\<Set>)|Obbligatorio. Specifica uno o più file, directory, o stringhe di testo o un intervallo di valori su cui eseguire il comando. Le parentesi sono obbligatorie.|
|\<Comando >|Obbligatorio. Specifica il comando che si desidera eseguire orizzontale su ogni file, directory o stringa di testo o sull'intervallo di valori inclusi nel *impostare*.|
|\<CommandLineOptions>|Specifica le opzioni della riga di comando che si desidera utilizzare con il comando specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Usando **per**

    È possibile usare la **per** comando all'interno di un file batch o direttamente dal prompt dei comandi.
-   Utilizzo di parametri di batch

    Gli attributi seguenti si applicano per la **per** comando:  
    -   Il **per** comando sostituisce **% * * * variabili* o **% % * * * variabili* con ogni stringa di testo nel set specificato fino a quando il comando specificato elabora tutti i file.
    -   I nomi delle variabili sono tra maiuscole e minuscole, globale e non superiore a 52 possono essere attive contemporaneamente.
    -   Per evitare confusione con i parametri batch **0** tramite **%9**, è possibile utilizzare qualsiasi carattere per *variabile* tranne i numeri da 0 a 9. Per semplici file batch, un singolo carattere, ad esempio **;** funzionerà.
    -   È possibile utilizzare più valori per *variabile* nei file batch complessi per distinguere le diverse variabili sostituibili.
-   Specifica un gruppo di file

    Il *impostare* parametri possono rappresentare un singolo gruppo di file o gruppi diversi di file. È possibile usare caratteri jolly (**&#42;** e **?**) per specificare un file di set. Di seguito sono insiemi di file valido.  
    ```
    (*.doc) 
    (*.doc *.txt *.me)
    (jan*.doc jan*.rpt feb*.doc feb*.rpt)
    (ar??1991.* ap??1991.*)
    ```  
    Quando si usa la **per** comando, il primo valore nel *Set* sostituisce **% * * * variabili* o **% % * * * variabili*e quindi il valore specificato comando elabora questo valore. Il processo continua finché tutti i file (o gruppi di file) che corrispondono al *impostare* valore vengono elaborati.
-   Usando il **nelle** e **si** parole chiave

    **Nelle** e **effettuare** non sono parametri, ma è necessario utilizzarli con **per**. Se si omette una di queste parole chiave, viene visualizzato un messaggio di errore.
-   Utilizzo di moduli aggiuntivi di **per**

    Se sono state abilitate le estensioni dei comandi (che è l'impostazione predefinita), le seguenti ulteriori forme di **per** sono supportate:  
    -   Solo le directory

        Se *impostata* contiene caratteri jolly (**&#42;** oppure **?**), specificato *comando* esegue per ogni directory (invece di un set file in una directory specificata) che corrisponde a *impostare*.

        La sintassi è:  
        ```
        for /d {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>] 
        ```  
    -   Ricorsivo

        Analizza l'albero di directory che ha origine nel *Drive*:*Path* ed esegue il **per** istruzione in ogni directory dell'albero. Se viene specificata alcuna directory dopo **/r**, la directory corrente viene utilizzata come directory radice. Se *impostare* è costituito solo da un punto (.), enumera solo la struttura di directory.

        La sintassi è:  
        ```
        for /r [[<Drive>:]<Path>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
        ```  
    -   L'iterazione di un intervallo di valori

        Usare una variabile iterativa per impostare il valore iniziale (*avviare*&) e quindi procedere con un determinato intervallo di valori fino a quando il valore supera il valore di fine impostato (*End*&). **/l** eseguirà l'iterazione confrontando *avviare*# con *Fine*#. Se *avviare*# è minore di *End*# eseguirà il comando. Quando la variabile iterativa supera *End*#, la shell dei comandi di uscire dal ciclo. È inoltre possibile utilizzare un valore negativo *passaggio*# per scorrere un intervallo di valori decrescenti. Ad esempio, (1,1,5) genera la sequenza 1 2 3 4 5 e (5,-1,1) genera la sequenza 1 2 3 4 di 5.

        La sintassi è:  
        ```
        for /l {%%|%}<Variable> in (<Start#>,<Step#>,<End#>) do <Command> [<CommandLineOptions>]
        ```  
    -   L'iterazione e l'analisi del file

        Usare analisi dei file per l'output del comando processo, stringhe e contenuto del file.  Utilizzare le variabili iterative per definire il contenuto o le stringhe che si desidera esaminare e utilizzare i vari *ParolechiaveAnalisi* le opzioni per modificare ulteriormente l'analisi.  Utilizzare il *ParolechiaveAnalisi* il token di opzione per specificare quali token devono essere passati come variabili iterative. Si noti che quando viene utilizzata senza l'opzione token **/f** esaminerà solo il primo token.

        L'analisi del file è costituito da leggere l'output, stringa o il contenuto del file, quindi suddividerlo in singole righe di testo e l'analisi di ogni riga in zero o più token. Il **per** ciclo viene quindi chiamato con il valore della variabile iterativo impostato sul token. Per impostazione predefinita, **/f** passa il primo spazio separato token da ogni riga di ogni file. Le righe vuote vengono ignorate.

        La sintassi sono:  
        ```
        for /f ["<ParsingKeywords>"] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
        for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ("<LiteralString>") do <Command> [<CommandLineOptions>]
        for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ('<Command>') do <Command> [<CommandLineOptions>]
        ```  
        Il *impostare* argomento specifica uno o più nomi di file. Ogni file è aperto, leggere ed elaborati prima di passare al file successivo contenuto *impostare*. Per ignorare il comportamento di analisi predefinito, specificare *ParolechiaveAnalisi*. Si tratta di una stringa tra virgolette contenente uno o più parole chiave per specificare diverse opzioni di analisi.

        Se si utilizza il **usebackq** opzione, utilizzare una delle sintassi seguenti:  
        ```
        for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ("<Set>") do <Command> [<CommandLineOptions>]
        for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ('<LiteralString>') do <Command> [<CommandLineOptions>]
        for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in (`<Command>`) do <Command> [<CommandLineOptions>]
        ```  
        Nella tabella seguente sono elencate le parole chiave di analisi che è possibile utilizzare per *ParolechiaveAnalisi*.  
        |Parola chiave|Descrizione|
        |-------|-----------|
        |eol=\<c>|Specifica un carattere di fine riga (un solo carattere).|
        |skip=\<N>|Specifica il numero di righe da ignorare all'inizio del file.|
        |delims=\<xxx>|Specifica un set di delimitatori. Sostituisce il set di delimitatori predefinito di spazio e tabulazione.|
        |token =\<X, Y,. M, N >|Specifica i token da ogni riga deve essere passato il **per** ciclo per ogni iterazione. Di conseguenza, i nomi delle variabili aggiuntive vengono allocati. *M*–*N* Specifica un intervallo tra il *M*th tramite il *N*token. Se l'ultimo carattere la **token =** stringa è un asterisco (**&#42;**), un'altra variabile viene allocata e riceve il testo rimanente sulla riga dopo l'ultimo token che viene analizzato.|
        |usebackq|Specifica per: esecuzione di una stringa racchiusa tra virgolette back un comando, usare una stringa tra virgolette come stringa letterale o, per nomi file lunghi che contengono spazi, consentire nomi di file in  *\<impostare\>*, a ogni essere racchiuso tra parentesi virgolette doppie.|
    -   Sostituzione delle variabili

        Nella tabella seguente sono elencati sintattici facoltativi (per qualsiasi variabile **ho**).  
        |Variabile con modificatore|Descrizione|
        |----------------------|-----------|
        |% ~ I|Si espande **avvii** che rimuove le virgolette di chiusura ("").|
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
-   Analisi di una stringa

    È possibile usare la **per /f** logica in una stringa di analisi eseguendo il wrapping *\<LiteralString\>* nelle: virgolette doppie (*senza* " usebackq") o virgolette (*con* "usebackq"), ad esempio, ("MyString") o ('MyString'). *\<LiteralString\>*  viene considerato come una singola riga di input da un file. Durante l'analisi *\<LiteralString\>* in virgolette doppie, simboli di comando (ad esempio **\\ \& \| \> \< \^**) vengono trattati come caratteri normali.
-   Output di analisi

    È possibile usare la **per /f** comando per analizzare l'output di un comando inserendo un racchiusi tra virgolette back *\<comando\>* racchiusi tra parentesi. Viene considerato come una riga di comando, che viene passata a un elemento figlio Cmd.exe. L'output viene acquisito in memoria e analizzato come se si tratta di un file.

## <a name="BKMK_examples"></a>Esempi

Utilizzare **per** in un file batch, utilizzare la sintassi seguente:
```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```
Per visualizzare il contenuto di tutti i file nella directory corrente che hanno l'estensione doc o txt utilizzando la variabile sostituibile **%f**, tipo:
```
for %f in (*.doc *.txt) do type %f 
```
Nell'esempio precedente, ogni file con estensione doc o txt nella directory corrente viene sostituito con il **%f** variabile fino a quando non viene visualizzato il contenuto di ogni file. Per utilizzare questo comando in un file batch, sostituire ogni occorrenza di **%f** con **% %**. In caso contrario, la variabile viene ignorata e viene visualizzato un messaggio di errore.

Per analizzare un file, ignorando le righe di commento, tipo:
```
for /f "eol=; tokens=2,3* delims=," %i in (myfile.txt) do @echo %i %j %k
```
Questo comando consente di analizzare ogni riga in MyFile. txt. Ignora le righe che iniziano con un punto e virgola e passa il token secondo e terzo da ogni riga per il **per** corpo (i token vengono separati da virgole o spazi). Il corpo del **per** istruzione fa riferimento **avvii** per ottenere il token, il secondo **%j** per ottenere il terzo token e **%k** per ottenere tutti i token rimanenti. Se i nomi dei file che viene fornito contiene spazi, utilizzare le virgolette intorno al testo (ad esempio, "nome File"). Per utilizzare le virgolette, è necessario utilizzare **usebackq**. In caso contrario, le virgolette doppie vengono interpretate come definizione di una valore letterale stringa da analizzare.

**%i** viene dichiarato in modo esplicito nel **per** istruzione. **%j** e **%k** in modo implicito vengono dichiarate utilizzando **token =**. È possibile utilizzare **token =** per specificare fino a 26 token, a condizione che non provoca un tentativo di dichiarare una variabile superiore alla lettera "z" o "Z".

Nell'esempio seguente enumera i nomi delle variabili di ambiente nell'ambiente corrente. Per analizzare l'output di un comando inserendo *impostare* tra parentesi, digitare:
```
for /f "usebackq delims==" %i in ('set') do @echo %i 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
