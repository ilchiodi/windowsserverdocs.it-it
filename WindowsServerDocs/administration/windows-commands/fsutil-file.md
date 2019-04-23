---
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
title: Fsutil file
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ffaf02f74f20f4eb94b94d8f0ffc51f26a62390e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828122"
---
# <a name="fsutil-file"></a>Fsutil file
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Trova un file in base al nome utente (se sono abilitate le quote disco), gli intervalli allocati per un file di una query, imposta nome breve del file, la lunghezza di dati validi di un file, imposta su zero i dati per un file o crea un nuovo file.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil file [createnew] <filename> <length>
fsutil file [findbysid] <username> <directory>
fsutil file [optimizemetadata] [/A] <filename>
fsutil file [queryallocranges] offset=<offset> length=<length> <filename>
fsutil file [queryextents] [/R] <filename> [<startingvcn> [<numvcns>]]
fsutil file [queryfileid] <filename>
fsutil file [queryfilenamebyid] <volume> <fileid>
fsutil file [queryoptimizemetadata] <filename>
fsutil file [queryvaliddata] [/R] [/D] <filename>
fsutil file [seteof] <filename> <length>
fsutil file [setshortname] <filename> <shortname>
fsutil file [setvaliddata] <filename> <datalength>
fsutil file [setzerodata] offset=<offset> length=<length> <filename>

```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|CreateNew|Crea un file con il nome specificato e le dimensioni, con il contenuto che è costituito da zeri.|
|\<filename>|Specifica il percorso completo del file incluso il nome del file e l'estensione, ad esempio C:\documents\filename.txt.|
|\<length>|Specifica la lunghezza del file dati validi.|
|findbysid|Trova i file che appartengono a un utente specifico nei volumi NTFS in cui sono abilitate le quote disco.|
|\<nome utente >|Specifica il nome utente o nome di accesso dell'utente.|
|\<directory>|Specifica il percorso completo per directory, ad esempio C:\users.|
|optimizemetadata|Questa operazione viene eseguita una compattazione immediata dei metadati per un determinato file.|
|/A|Analizzare i metadati del file prima e dopo l'ottimizzazione.|
|queryallocranges|Esegue una query gli intervalli allocati per un file in un volume NTFS. Utile per determinare se un file contiene le aree di tipo sparse.|
|offset=\<offset>|Specifica l'inizio dell'intervallo che deve essere impostato su zero.|
|length=\<length>|Specifica la lunghezza dell'intervallo (in byte).|
|queryextents|Gli extent di un file di una query.|
|/R|Se <filename> è un reparse point punto, aprire anziché il valore di destinazione.|
|\<startingvcn>|Specifica VCN prima alla query. Se omesso, partono da VCN 0.|
|\<numvcns>|Numero di VCNs per eseguire una query. Se omesso o è uguale a 0, query fino alla fine del file.|
|queryfileid|Esegue una query dell'ID file di un file in un volume NTFS.<br /><br />Questo parametro si applica a:  Windows Server 2008 R2 e Windows 7.|
|\<volume>|Specifica il volume come nome di unità seguita da due punti.|
|queryfilenamebyid|Visualizza il nome di un collegamento casuale per un ID di file specificato in un volume NTFS. Poiché il file può avere più di un nome di collegamento che punta al file, non è garantito quale collegamento del file verrà fornito come risultato della query per il nome del file.<br /><br />Questo parametro si applica a:  Windows Server 2008 R2 e Windows 7.|
|\<fileid>|Specifica l'ID del file in un volume NTFS.|
|queryoptimizemetadata|Esegue una query lo stato dei metadati di un file.|
|queryvaliddata|La lunghezza dei dati validi per un file di una query.|
|/D|Visualizzare informazioni dettagliate dati validi.|
|seteof|Imposta la fine del file del file specificato.|
|setshortname|Imposta il nome breve (nome file di lunghezza dei caratteri in formato 8.3) per un file in un volume NTFS.|
|\<shortname>|Specifica il nome del file brevi.|
|setvaliddata|Imposta la lunghezza di dati validi per un file in un volume NTFS.|
|\<datalength>|Specifica la lunghezza del file in byte.|
|setzerodata|Imposta un intervallo (specificato da *Offset* e *lunghezza*) del file da zero, che svuota il file. Se il file è un file sparse, le unità di allocazione sottostante vengono annullate.|

## <a name="remarks"></a>Note

-   In NTFS, esistono due concetti fondamentali della lunghezza del file: il marcatore di fine del file (EOF) e la lunghezza di Data valido (VDL). La fine del file indica la lunghezza effettiva del file. Il VDL identifica la lunghezza dei dati validi su disco. Eventuali operazioni di lettura tra VDL ed EOF automaticamente restituire 0 per mantenere l'oggetto C2 riutilizzare requisito.

-   Il **setvaliddata** parametro è disponibile solo per gli amministratori perché richiede il privilegio di attività (SeManageVolumePrivilege) eseguire la manutenzione volume. Questa funzionalità è richiesto solo per gli scenari di rete SAN multimedia e avanzati. Il **setvaliddata** parametro deve essere un valore positivo maggiore di VDL corrente, ma minore delle dimensioni di file corrente.

    È utile per i programmi impostare un VDL quando:

    -   Scrittura di un cluster non elaborato direttamente su disco tramite un canale di hardware. In questo modo il programma informare il file system che questo intervallo contenga dati validi che possono essere restituiti all'utente.

    -   Creazione di file di grandi dimensioni quando le prestazioni costituiscono un problema. Si evita così il tempo che necessario per compilare il file con zeri quando il file viene creato o esteso.

## <a name="BKMK_examples"></a>Esempi
Per trovare i file che sono di proprietà utente sull'unità C, digitare:

```
fsutil file findbysid scottb c:\users  
```

Per eseguire una query gli intervalli allocati per un file in un volume NTFS, digitare:

```
fsutil file queryallocranges offset=1024 length=64 c:\temp\sample.txt  
```

Per ottimizzare i metadati per un file, digitare:

```
fsutil file optimizemetadata C:\largefragmentedfile.txt
```

Per eseguire una query gli extent di un file, digitare:

```
fsutil file queryextents C:\Temp\sample.txt
```

Per impostare la fine del file per un file, digitare:

```
fsutil file seteof C:\testfile.txt 1000
```

Per impostare il nome breve del file txt sull'unità C per filelung. txt, digitare:

```
fsutil file setshortname c:\longfilename.txt longfile.txt  
```

Per impostare la lunghezza dei dati validi a 4096 byte per un file denominato TestFile. txt in un volume NTFS, digitare:

```
fsutil file setvaliddata c:\testfile.txt 4096  
```

Per impostare un intervallo di un file in un volume NTFS su un valore vuoto, gli zeri, digitare:

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt  
```

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


