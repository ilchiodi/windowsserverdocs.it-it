---
title: fsutil file
description: Argomento di riferimento per il comando fsutil file, che consente di trovare un file in base al nome utente, di eseguire query sugli intervalli allocati per un file, impostare il nome breve di un file, impostare la lunghezza dei dati valida di un file, impostare zero dati per un file o creare un nuovo file.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: e9be8f6d21b89d1017371b9697e1227122826a7d
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435876"
---
# <a name="fsutil-file"></a>fsutil file

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Trova un file in base al nome utente (se sono abilitate le quote disco), esegue query sugli intervalli allocati per un file, imposta il nome breve di un file, imposta la lunghezza dei dati valida di un file, imposta zero dati per un file o crea un nuovo file.

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

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| CreateNew | Crea un file con il nome e le dimensioni specificati, con contenuto costituito da zeri. |
| `<length>` | Specifica la lunghezza di dati valida del file. |
| findbysid | Trova i file che appartengono a un utente specificato nei volumi NTFS in cui sono abilitate le quote disco. |
| `<username>` | Specifica il nome utente o il nome di accesso dell'utente. |
| `<directory>` | Specifica il percorso completo della directory, ad esempio C:\Users. |
| optimizemetadata | In questo modo viene eseguita una compattazione immediata dei metadati per un determinato file. |
| /a | Analizzare i metadati del file prima e dopo l'ottimizzazione. |
| queryallocranges | Esegue una query sugli intervalli allocati per un file in un volume NTFS. Utile per determinare se un file ha aree di tipo sparse. |
| offset =`<offset>` | Specifica l'inizio dell'intervallo da impostare su zero. |
| lunghezza =`<length>` | Specifica la lunghezza dell'intervallo (in byte). |
| queryextents | Esegue una query sugli extent per un file. |
| /r | Se <filename> è un reparse point, aprirlo anziché la relativa destinazione. |
| `<startingvcn>` | Specifica il primo VCN per eseguire una query. Se omesso, iniziare da VCN 0. |
| `<numvcns>` | Numero di VCNs di cui eseguire la query. Se omesso o 0, eseguire una query fino a EOF. |
| queryfileid | Esegue una query sull'ID file di un file in un volume NTFS. |
| `<volume>` | Specifica il volume come nome dell'unità seguito da due punti. |
| queryfilenamebyid | Visualizza un nome di collegamento casuale per un ID file specificato in un volume NTFS. Poiché un file può avere più di un nome di collegamento che punta a tale file, non è garantito quale collegamento al file verrà fornito come risultato della query per il nome file. |
| `<fileid>` | Specifica l'ID del file in un volume NTFS. |
| queryoptimizemetadata | Esegue una query sullo stato dei metadati di un file. |
| queryvaliddata | Esegue una query sulla lunghezza dei dati valida per un file. |
| /d | Visualizza informazioni dettagliate sui dati validi. |
| seteof | Imposta il EOF del file specificato. |
| seshortname | Imposta il nome breve (8,3 nome file di lunghezza carattere) per un file in un volume NTFS. |
| `<shortname>` | Specifica il nome breve del file. |
| setvaliddata | Imposta la lunghezza dei dati valida per un file in un volume NTFS. |
| `<datalength>` | Specifica la lunghezza del file in byte. |
| setzerodata | Imposta un intervallo (specificato in base all' *offset* e alla *lunghezza*) del file su zero, che consente di svuotare il file. Se il file è di tipo sparse, viene eseguito il commit delle unità di allocazione sottostanti. |

#### <a name="remarks"></a>Osservazioni

- In NTFS esistono due concetti importanti di lunghezza dei file: il marcatore di fine file (EOF) e la lunghezza dei dati valida (VDL). Il EOF indica la lunghezza effettiva del file. Il VDL identifica la lunghezza dei dati validi su disco. Tutte le letture tra VDL e EOF restituiscono automaticamente 0 per mantenere il requisito di riutilizzo dell'oggetto C2.

- Il parametro **setvaliddata** è disponibile solo per gli amministratori perché richiede il privilegio di esecuzione delle attività di manutenzione del volume (SeManageVolumePrivilege). Questa funzionalità è necessaria solo per gli scenari avanzati di System Area Network e multimediali. Il parametro **setvaliddata** deve essere un valore positivo maggiore di VDL corrente, ma minore delle dimensioni correnti del file.

    È utile per i programmi impostare un VDL nei casi seguenti:

    - Scrittura di cluster non elaborati direttamente su disco tramite un canale hardware. Ciò consente al programma di informare il file system che questo intervallo contiene dati validi che possono essere restituiti all'utente.

    - Creazione di file di grandi dimensioni quando le prestazioni sono un problema. In questo modo si evita il tempo necessario per riempire il file con zeri quando il file viene creato o esteso.

### <a name="examples"></a>Esempi

Per trovare i file di proprietà di *scottb* nell'unità C, digitare:

```
fsutil file findbysid scottb c:\users
```

Per eseguire una query sugli intervalli allocati per un file in un volume NTFS, digitare:

```
fsutil file queryallocranges offset=1024 length=64 c:\temp\sample.txt
```

Per ottimizzare i metadati per un file, digitare:

```
fsutil file optimizemetadata C:\largefragmentedfile.txt
```

Per eseguire una query sugli extent per un file, digitare:

```
fsutil file queryextents C:\Temp\sample.txt
```

Per impostare EOF per un file, digitare:

```
fsutil file seteof C:\testfile.txt 1000
```

Per impostare il nome breve del file *LongFileName. txt* nell'unità C in *LONGFILE. txt*, digitare:

```
fsutil file setshortname c:\longfilename.txt longfile.txt
```

Per impostare la lunghezza dei dati valida su *4096 byte* per un file denominato *TestFile. txt* in un volume NTFS, digitare:

```
fsutil file setvaliddata c:\testfile.txt 4096
```

Per impostare un intervallo di un file in un volume NTFS su zero, digitare:

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
