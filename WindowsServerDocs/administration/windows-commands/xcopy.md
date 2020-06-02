---
title: xcopy
description: Argomento di riferimento per xcopy, che consente di copiare file e directory, incluse le sottodirectory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 01/05/2019
ms.openlocfilehash: c55d6ae5ff701555eb9bfb7135ffa28692bd4391
ms.sourcegitcommit: 4894649cc47dfa535306cc334871f81155198f76
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2020
ms.locfileid: "84254724"
---
# <a name="xcopy"></a>xcopy

Copia i file e le directory, incluse le sottodirectory.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Source>|Obbligatorio. Specifica il percorso e i nomi dei file che si desidera copiare. Questo parametro deve includere un'unità o un percorso.|
|[\<Destination>]|Specifica la destinazione dei file che si desidera copiare. Questo parametro può includere una lettera di unità e due punti, un nome di directory, un nome di file o una combinazione di questi.|
|/W|Visualizza il messaggio seguente e attende la risposta prima di iniziare a copiare i file:</br>**Premere un tasto qualsiasi per iniziare a copiare i file**|
|/ p|Viene richiesto di confermare se si desidera creare ogni file di destinazione.|
|/C|Ignora gli errori.|
|/v|Verifica che ogni file venga scritto nel file di destinazione per assicurarsi che i file di destinazione siano identici a quelli dei file di origine.|
|/q|Evita la visualizzazione dei messaggi **xcopy** .|
|/f|Visualizza i nomi dei file di origine e di destinazione durante la copia.|
|/l|Visualizza un elenco di file da copiare.|
|/g|Crea file di *destinazione* decrittografati quando la destinazione non supporta la crittografia.|
|/d [: MM-gg-aaaa]|Copia i file di origine modificati solo dopo la data specificata. Se non si include un valore *mm-gg-aaaa* , **xcopy** copia tutti i file di *origine* più recenti rispetto ai file di *destinazione* esistenti. Questa opzione della riga di comando consente di aggiornare i file che sono stati modificati.|
|/U|Copia i file dall' *origine* esistente solo nella *destinazione* .|
|/i|Se l' *origine* è una directory o contiene caratteri jolly e la *destinazione* non esiste, **xcopy** presuppone che la *destinazione* specifichi un nome di directory e crea una nuova directory. Quindi, **xcopy** copia tutti i file specificati nella nuova directory. Per impostazione predefinita, **tramite Xcopy** viene richiesto di specificare se la *destinazione* è un file o una directory.|
|/s|Copia le directory e le sottodirectory, a meno che non siano vuote. Se si omette **/s**, **xcopy** funziona all'interno di un'unica directory.|
|/e|Copia tutte le sottodirectory, anche se sono vuote. Usare **/e** con le opzioni della riga di comando **/s** e **/t** .|
|/t|Copia solo la struttura della sottodirectory (ovvero l'albero), non i file. Per copiare directory vuote, è necessario includere l'opzione della riga di comando **/e** .|
|/k|Copia i file e mantiene l'attributo di sola lettura nei file di *destinazione* , se presente nei file di *origine* . Per impostazione predefinita, **xcopy** rimuove l'attributo di sola lettura.|
|/r|Copia i file di sola lettura.|
|/h|Copia i file con gli attributi nascosti e di file di sistema. Per impostazione predefinita, **xcopy** non copia i file nascosti o di sistema|
|/a|Copia solo i file di *origine* per i quali sono impostati gli attributi del file di archivio. **/a** non modifica l'attributo del file di archivio del file di origine. Per informazioni su come impostare l'attributo del file di archivio usando **attrib**, vedere [riferimenti aggiuntivi](#additional-references).|
|/m|Copia i file di *origine* per i quali sono impostati gli attributi del file di archivio. Diversamente da **/a**, **/m** disattiva gli attributi del file di archivio nei file specificati nell'origine. Per informazioni su come impostare l'attributo del file di archivio usando **attrib**, vedere [riferimenti aggiuntivi](#additional-references).|
|/n|Crea copie usando i nomi di file o di directory brevi NTFS. **/n** è obbligatorio quando si copiano file o directory da un volume NTFS a un volume FAT o quando è richiesta la convenzione di denominazione FAT file System (ovvero 8,3 caratteri) nel file System di *destinazione* . Il file system di *destinazione* può essere FAT o NTFS.|
|/o|Copia la proprietà del file e le informazioni relative all'elenco di controllo di accesso discrezionale (DACL).|
|/x|Copia le impostazioni di controllo del file e le informazioni dell'elenco di controllo di accesso di sistema (SACL) (implica **/o**).|
|/exclude: FileName1 [+ [FileName2] [+ [FileName3] ( \) ]|Specifica un elenco di file. È necessario specificare almeno un file. Ogni file conterrà stringhe di ricerca con ogni stringa in una riga separata del file.</br>Quando una qualsiasi delle stringhe corrisponde a qualsiasi parte del percorso assoluto del file da copiare, il file verrà escluso dalla copia. Se ad esempio si specifica la stringa **obj** , tutti i file sotto la directory **obj** o tutti i file con estensione **obj** vengono esclusi.|
|/y|Evita la richiesta di conferma della sovrascrittura di un file di destinazione esistente.|
|/-y|Richiede di confermare che si desidera sovrascrivere un file di destinazione esistente.|
|/z|Copia in una rete in modalità riavviabile.|
|/ b|Copia il collegamento simbolico anziché i file. Questo parametro è stato introdotto in Windows Vista®.|
|/j|Copia i file senza memorizzare nel buffer. Consigliato per file di grandi dimensioni. Questo parametro è stato aggiunto in Windows Server 2008 R2.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Commenti

- Utilizzando **/z**

  Se si perde la connessione durante la fase di copia, ad esempio se il server passa in modalità offline alla connessione, riprende dopo aver ristabilito la connessione. **/z** Visualizza inoltre la percentuale di operazione di copia completata per ogni file.

- Uso di **/y** nella variabile di ambiente COPYCMD.

  È possibile usare **/y** nella variabile di ambiente COPYCMD. È possibile eseguire l'override di questo comando usando **/-y** nella riga di comando. Per impostazione predefinita, viene richiesto di sovrascrivere.

- Copia di file crittografati

  La copia di file crittografati in un volume che non supporta EFS genera un errore. Decrittografare i file prima o copiare i file in un volume che supporta EFS.

- Aggiunta di file

  Per aggiungere file, specificare un singolo file per la destinazione, ma più file per l'origine, ovvero usando caratteri jolly o il formato file1 + file2 + file3.

- Valore predefinito per la *destinazione*

  Se si omette la *destinazione*, il comando **xcopy** copia i file nella directory corrente.

- Specifica se la *destinazione* è un file o una directory

  Se la *destinazione* non contiene una directory esistente e non termina con una barra rovesciata ( \) , viene visualizzato il messaggio seguente:

  ```
  Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
  ```

Premere F se si desidera che il file o i file vengano copiati in un file. Premere D se si desidera che il file o i file vengano copiati in una directory.

  È possibile disattivare questo messaggio utilizzando l'opzione della riga di comando **/i** , che fa in modo che **xcopy** presupponga che la destinazione sia una directory se l'origine è più di un file o una directory.
- Utilizzo del comando **xcopy** per impostare l'attributo Archive per i file di *destinazione*

  Il comando **xcopy** crea file con l'attributo Archive impostato, indipendentemente dal fatto che l'attributo sia stato impostato nel file di origine. Per altre informazioni sugli attributi dei file e su **attrib**, vedere [riferimenti aggiuntivi](#additional-references).

- Confronto tra **xcopy** e **diskcopy**

  Se si dispone di un disco che contiene file nelle sottodirectory e si desidera copiarlo su un disco con un formato diverso, utilizzare il comando **xcopy** anziché **diskcopy**. Poiché il comando **diskcopy** copia i dischi in base alla traccia, i dischi di origine e di destinazione devono avere lo stesso formato. Il comando **xcopy** non presenta questo requisito. Utilizzare **xcopy** a meno che non sia necessaria una copia completa dell'immagine del disco.

- Codici di uscita per **xcopy**

  Per elaborare i codici di uscita restituiti da **xcopy**, usare il parametro **errorlevel** nella riga di comando **if** in un programma batch. Per un esempio di un programma batch che elabora i codici di uscita usando **if**, vedere [riferimenti aggiuntivi](#additional-references). Nella tabella seguente sono elencati i codici di uscita e una descrizione.

  |Codice di uscita|Descrizione|
  |---------|-----------|
  |0|I file sono stati copiati senza errori.|
  |1|Nessun file trovato per la copia.|
  |2|L'utente ha premuto CTRL + C per terminare **xcopy**.|
  |4|Si è verificato un errore di inizializzazione. Memoria o spazio su disco insufficiente oppure è stato immesso un nome di unità non valido o una sintassi non valida nella riga di comando.|
  |5|Si è verificato un errore di scrittura del disco.|

## <a name="examples"></a>Esempio

**1.** per copiare tutti i file e le sottodirectory (incluse eventuali sottodirectory vuote) dall'unità a all'unità B, digitare:

```
xcopy a: b: /s /e
```

**2.** per includere tutti i file di sistema o nascosti nell'esempio precedente, aggiungere l'opzione della riga di comando<strong>/h</strong> come indicato di seguito:

```
xcopy a: b: /s /e /h
```

**3.** per aggiornare i file nella directory \Rapporti con i file nella directory \Operaz che sono stati modificati a partire dal 29 dicembre 1993, digitare:

```
xcopy \rawdata \reports /d:12-29-1993
```

**4.** per aggiornare tutti i file presenti in \Rapporti nell'esempio precedente, indipendentemente dalla data, digitare:

```
xcopy \rawdata \reports /u
```

**5.** per ottenere un elenco dei file da copiare con il comando precedente, ovvero senza copiare effettivamente i file, digitare:

```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```

Il file xcopy. out elenca tutti i file che devono essere copiati.

**6.** per copiare la directory \Customer e tutte le sottodirectory nella directory \\ \\ Public\Address sull'unità di rete H:, mantenere l'attributo di sola lettura e quando viene creato un nuovo file in h:, digitare:

```
xcopy \customer h:\public\address /s /e /k /p
```

**7.** per eseguire il comando precedente, assicurarsi che **xcopy** crei la directory \Indiriz se non esiste ed elimini il messaggio visualizzato quando si crea una nuova directory, aggiungere l'opzione della riga di comando **/i** come indicato di seguito:

```
xcopy \customer h:\public\address /s /e /k /p /i
```

**8.** è possibile creare un programma batch per eseguire operazioni **xcopy** e utilizzare il comando batch **if** per elaborare il codice di uscita se si verifica un errore. Ad esempio, il programma batch seguente usa parametri sostituibili per i parametri di origine e destinazione **xcopy** :

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

Per usare il programma batch precedente per copiare tutti i file nella directory C:\Prgmcode e nelle relative sottodirectory nell'unità B, digitare:

```
copyit c:\prgmcode b:
```

L'interprete dei comandi sostituisce **C:\Prgmcode** per *%1* e **B:** per *%2*, quindi utilizza **xcopy** con le opzioni della riga di comando **/e** e **/s** . Se **xcopy** rileva un errore, il programma batch legge il codice di uscita e passa all'etichetta indicata nell'istruzione **if ERRORLEVEL** appropriata, quindi Visualizza il messaggio appropriato e termina dal programma batch.

**9.** in questo esempio vengono copiate tutte le directory non vuote, oltre ai file il cui nome corrisponde al modello specificato con il simbolo asterisco.

```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```

Nell'esempio precedente, questo particolare valore del parametro di origine **. \\ TOC \* . yml** copia gli stessi 3 file anche se i due caratteri del **percorso \\ .** sono stati rimossi. Tuttavia, non viene copiato alcun file se il carattere jolly asterisco è stato rimosso dal parametro source, rendendolo semplicemente **. \\ TOC. yml**.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Copia](copy.md)
- [Spostamento](move.md)
- [Dir](dir.md)
- [Attrib](attrib.md)
- [DISKCOPY](diskcopy.md)
- [Se](if.md)
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
