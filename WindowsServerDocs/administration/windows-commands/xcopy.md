---
title: xcopy
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 01/05/2019
ms.openlocfilehash: 54697b1c967d3e21583977418383d5a372e6f5d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859402"
---
# <a name="xcopy"></a>xcopy



Copia i file e directory, incluse le sottodirectory

Per esempi di come usare questo comando, vedere [esempi](xcopy.md#BKMK_examples)

## <a name="syntax"></a>Sintassi

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<origine >|Obbligatorio. Specifica il percorso e i nomi dei file da copiare. Questo parametro deve includere un'unità o un percorso.|
|[\<Destination>]|Specifica la destinazione dei file da copiare. Questo parametro può includere una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi.|
|/w|Visualizza un messaggio e attende una risposta prima di iniziare a copiare i file:</br>**Premere un tasto qualsiasi per iniziare la copia dei file**|
|/ p|Viene richiesto di confermare se si desidera creare ogni file di destinazione.|
|/c|Ignora gli errori.|
|/v|Verifica ogni file durante la scrittura nel file di destinazione per assicurarsi che i file di destinazione siano identici ai file di origine.|
|/q|Evita la visualizzazione delle **xcopy** messaggi.|
|/f|Consente di visualizzare i nomi di file di origine e destinazione durante la copia.|
|/l|Visualizza un elenco di file che si desidera copiare.|
|/g|Crea decrittografata *destinazione* file quando la destinazione non supporta la crittografia.|
|/d [: MM-gg-aaaa]|Copia origine file modificati a decorrere solo la data specificata. Se non si include un *MM-GG-AAAA* , valore **xcopy** copia tutti *origine* file più recenti rispetto a esistenti *destinazione* file. Questa opzione della riga di comando consente di aggiornare i file che sono stati modificati.|
|/u|Copia i file dalla *origine* presenti nel *destinazione* solo.|
|/i|Se *origine* è una directory o contiene caratteri jolly e *destinazione* non esiste, **xcopy** presuppone *destinazione* specifica un nome della directory e crea una nuova directory. Quindi **xcopy** copia tutti i file specificati nella nuova directory. Per impostazione predefinita **xcopy** chiede di specificare se *destinazione* è un file o una directory.|
|/s|Copia directory e sottodirectory, a meno che non sono vuote. Se si omette **/s**, **xcopy** funziona all'interno di una singola directory.|
|/e|Copia tutte le sottodirectory, anche se sono vuote. Uso **/e** con il **/s** e **/t** opzioni della riga di comando.|
|/t|Copia la struttura delle sottodirectory (vale a dire, l'albero), non i file. Per copiare le directory vuote, è necessario includere il **/e** opzione della riga di comando.|
|/k|Copia i file e li conserva l'attributo di sola lettura sul *destinazione* file se è presente nel *origine* file. Per impostazione predefinita **xcopy** rimuove l'attributo di sola lettura.|
|/r|Copia i file di sola lettura.|
|/h|Copia i file con nascosto e gli attributi di file di sistema. Per impostazione predefinita **xcopy** non copia nascosta o di file di sistema|
|/a|Copia solo *origine* con loro archivia i file di set di attributi. **/a** non modifica l'attributo di file di archivio del file di origine. Per informazioni su come impostare l'attributo di file di archivio usando **attrib**, vedere [ulteriori riferimenti](xcopy.md#BKMK_addref).|
|/m|Le copie *origine* con loro archivia i file di set di attributi. A differenza **/a**, **/m** consente di disattivare gli attributi di file di archivio nei file specificati nell'origine. Per informazioni su come impostare l'attributo di file di archivio usando **attrib**, vedere [ulteriori riferimenti](xcopy.md#BKMK_addref).|
|/n|Crea copie usando il file brevi NTFS o i nomi di directory. **/n** è obbligatorio quando si copiano file o directory da un volume NTFS in un volume FAT o quando il file system FAT file convenzione di denominazione di sistema (vale a dire, il formato 8.3) è obbligatorio nel *destinazione* file system. Il *destinazione* può essere il sistema di file system FAT che NTFS.|
|/o|Copia file della proprietà e le informazioni di accesso discrezionale controllo elenco (DACL).|
|/x|Copia file informazioni elenco (SACL) di controllo di accesso di sistema e le impostazioni di controllo (implica **/o**).|
|/exclude:FileName1[+[FileName2][+[FileName3]( \)]|Specifica un elenco di file. Specificare almeno un file. Ogni file contiene le stringhe di ricerca con ogni stringa in una riga separata nel file.</br>Quando una delle stringhe corrispondono a qualsiasi parte del percorso assoluto del file da copiare, il file risulterà excuded la copia. Ad esempio, specificando la stringa **obj** escluderà tutti i file di sotto della directory **obj** o tutti i file con il **obj** estensione.|
|/y|Evita la visualizzazione verrà richiesto di confermare che si desidera sovrascrivere un file di destinazione esistente.|
|/-y|Viene chiesto di confermare che si desidera sovrascrivere un file di destinazione esistente.|
|/z|Copia in una rete in modalità riavviabile.|
|/ b|Copia il collegamento simbolico anziché i file. Questo parametro è stato introdotto in Windows Vista®.|
|/j|Copia i file senza memorizzarlo nel buffer. Consigliato per i file molto grandi. Questo parametro è stato aggiunto in Windows Server 2008 R2.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Utilizzando **/z**

    Se si perde la connessione durante la fase di copia (ad esempio, se il server si interrompe la connessione), riprende quando viene ristabilita la connessione. **/z** inoltre visualizza la percentuale dell'operazione di copia completata per ogni file.
-   Usando **/y** nella variabile di ambiente COPYCMD.

    È possibile usare **/y** nella variabile di ambiente COPYCMD. È possibile eseguire l'override di questo comando usando **/-y** nella riga di comando. Per impostazione predefinita, chiesto di sovrascrivere.
-   Copia i file crittografati

    Copiare i file crittografati in un volume che non supporta EFS genera un errore. Decrittografare i file prima di tutto o copiare i file in un volume che supporta il sistema EFS.
-   Aggiunta di file

    Per aggiungere file, specificare un solo file di destinazione, ma più file di origine (vale a dire usando caratteri jolly o file1 + file2 + file3).
-   Valore predefinito per *destinazione*

    Se si omette *destinazione*, il **xcopy** comando Copia i file nella directory corrente.
-   Che specifica se *destinazione* è un file o directory

    Se *destinazione* non contiene una directory esistente e non termina con una barra rovesciata (\), viene visualizzato il messaggio seguente:  
    ```
    Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
    ```  
    Se si desidera che i file da copiare in un file, premere F. Premere D se si desidera che i file da copiare in una directory.

    È possibile eliminare questo messaggio mediante il **/i** opzione della riga di comando, che fa sì che **xcopy** presupporre che la destinazione è una directory se l'origine è più di un file o una directory.
-   Usando il **xcopy** comando per impostare l'attributo archive per *destinazione* file

    Il **xcopy** comando Crea file con il set di attributi di archiviazione, se questo attributo è stato impostato nel file di origine. Per altre informazioni sugli attributi di file e **attrib**, vedere [ulteriori riferimenti](xcopy.md#BKMK_addref).
-   Confronto tra **xcopy** e **dell'operazione**

    Se si dispone di un disco che contiene i file nelle sottodirectory e si desidera copiarlo in un disco che presenta un formato diverso, usare il **xcopy** comando anziché **verrà**. Poiché il **verrà** comando consente di copiare i dischi dalla traccia, il disco di origine e destinazione debba avere lo stesso formato. Il **xcopy** comando non presenta questo requisito. Uso **xcopy** a meno che non è necessaria una copia di immagine disco completa.
-   Codici di uscita per **xcopy**

    Per elaborare i codici di uscita restituiti da **xcopy**, utilizzare il **ErrorLevel** parametro il **se** riga di comando in un file batch. Per un esempio di un file batch che elabora i uscita utilizzando i codici **se**, vedere [ulteriori riferimenti](xcopy.md#BKMK_addref). La tabella seguente elenca ogni codice di uscita e una descrizione.  
    |Codice di uscita|Descrizione|
    |---------|-----------|
    |0|I file sono stati copiati senza errori.|
    |1|Nessun file trovato da copiare.|
    |2|L'utente ha premuto CTRL + C per terminare **xcopy**.|
    |4|Si è verificato un errore di inizializzazione. Non c'è sufficiente memoria o spazio su disco o immesso un nome di unità non valida o una sintassi non valida nella riga di comando.|
    |5|Errore di scrittura sul disco.|

## <a name="BKMK_examples"></a>Esempi

**1.** Per copiare tutti i file e sottodirectory, inclusi eventuali sottodirectory vuota, da unità A unità B, digitare:
```
xcopy a: b: /s /e 
```
**2.** Per includere eventuali file nascosti o sistema nell'esempio precedente, aggiungere il **/h** opzione della riga di comando come indicato di seguito:
```
xcopy a: b: /s /e /h
```
**3.** Per aggiornare i file nella directory \Reports. con i file nella directory \Rawdata che sono stati modificati dopo il 29 dicembre 1993, digitare:
```
xcopy \rawdata \reports /d:12-29-1993
```
**4.** Per aggiornare tutti i file esistenti in \Reports nell'esempio precedente, indipendentemente dalla data, digitare:
```
xcopy \rawdata \reports /u
```
**5.** Per ottenere un elenco dei file da copiare dal comando precedente (vale a dire, senza copiare effettivamente i file), tipo:
```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```
Il file xcopy. out sono elencati tutti i file che deve essere copiato.

**6.** Per copiare la directory \Customer e tutte le sottodirectory nella directory \\ \\Public\Address sulla rete unità h per mantenere l'attributo di sola lettura e richieste quando viene creato un nuovo file:, digitare:
```
xcopy \customer h:\public\address /s /e /k /p
```
**7.** Per eseguire il comando precedente, assicurarsi che **xcopy** crea la directory \Address se non esiste e non visualizzare il messaggio visualizzato quando si crea una nuova directory, aggiungere il **/i** della riga di comando opzione come indicato di seguito:
```
xcopy \customer h:\public\address /s /e /k /p /i
```
**8.** È possibile creare un file batch per eseguire **xcopy** operazioni e l'utilizzo di batch **se** comando per elaborare il codice di uscita se si verifica un errore. Ad esempio, il programma batch seguente usa i parametri sostituibili per il **xcopy** i parametri di origine e di destinazione:
```
@echo off
rem COPYIT.BAT transfers all files in all subdirectories of
rem the source drive or directory (%1) to the destination
rem drive or directory (%2)
xcopy %1 %2 /s /e
if errorlevel 4 goto lowmemory
if errorlevel 2 goto abort
if errorlevel 0 goto exit
:lowmemory
echo Insufficient memory to copy files or
echo invalid drive or command-line syntax.
goto exit
:abort
echo You pressed CTRL+C to end the copy operation.
goto exit
:exit 
```
Per usare il programma batch precedente per copiare tutti i file nella directory C:\Prgmcode e nelle relative sottodirectory in unità B, digitare:
```
copyit c:\prgmcode b:
```
Comando interprete sostituti **C:\Prgmcode** per *%1* e **b:** per *%2*, quindi Usa **xcopy**con il **/e** e **/s** opzioni della riga di comando. Se **xcopy** rileva un errore, il programma batch legge il codice di uscita e si sposta l'etichetta indicata nelle rispettive caselle **ERRORLEVEL IF** istruzione, quindi viene visualizzato il messaggio appropriato e di chiudere il file batch.

**9.** In questo esempio tutti i non vuota, le directory più file il cui nome corrisponde al modello specificato, con il simbolo di asterisco.
```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```
Nell'esempio precedente, il valore di questo parametro di origine specifico **.\\ Sommario\*yml** copiare la stessa anche se 3 file relativi caratteri a due percorsi **.\\**  sono state rimosse. Tuttavia, vengono copiato alcun file se il carattere jolly asterisco è stata rimossa dal parametro di origine, rendendo appena **.\\ TOC.yml**.

#### <a name="BKMK_addref"></a>Riferimenti aggiuntivi

-   [Copia](copy.md)
-   [Sposta](move.md)
-   [Dir](dir.md)
-   [Attrib](attrib.md)
-   [Diskcopy](diskcopy.md)
-   [If](if.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
