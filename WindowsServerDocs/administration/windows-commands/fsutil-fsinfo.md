---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: Fsutil fsinfo
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 56e27c386451c561de8f62e523e2d1e59a8ce84c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844304"
---
# <a name="fsutil-fsinfo"></a>Fsutil fsinfo
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Elenca tutte le unità, esegue una query sul tipo di unità, esegue query sulle informazioni sul volume, esegue query su informazioni sul volume specifiche di NTFS o query file system statistiche.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <VolumePath>
fsutil fsinfo [ntfsinfo] <RootPath>
fsutil fsinfo [statistics] <VolumePath>
fsutil fsinfo [volumeinfo] <RootPath>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|unità|Elenca tutte le unità del computer.|
|DriveType|Esegue una query su un'unità e ne elenca il tipo, ad esempio l'unità CD-ROM.|
|ntfsinfo|Elenca le informazioni sul volume specifico NTFS per il volume specificato, ad esempio il numero di settori, i cluster totali, i cluster gratuiti e l'inizio e la fine della zona MFT.|
|sectorinfo|Elenca le informazioni sulle dimensioni e sull'allineamento del settore dell'hardware.|
|statistiche|Elenca file system statistiche per il volume specificato, ad esempio i metadati, il file di log e le letture e le Scritture MFT.|
|volumeinfo|Elenca le informazioni per il volume specificato, ad esempio il file system e se il volume supporta nomi file con distinzione tra maiuscole e minuscole, Unicode nei nomi file, quote disco o è un volume DirectAccess (DAX).|
|< "VolumePath" >|Specifica la lettera di unità (seguita da due punti).|
|< "RootPathname" >|Specifica la lettera di unità (seguita da due punti) dell'unità radice.|

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi
Per elencare tutte le unità del computer, digitare:

```
fsutil fsinfo drives
```

Viene visualizzato un output simile al seguente:

```
Drives: A:\ C:\ D:\ E:\       
```

Per eseguire una query sul tipo di unità C, digitare:

```
fsutil fsinfo drivetype c:
```

I risultati possibili della query includono:

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

Per eseguire una query sulle informazioni sul volume per il volume E, digitare:

```
fsinfo volumeinfo e:\
```

Viene visualizzato un output simile al seguente:

```
Volume Name :Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
.
.
.
Supports Named Streams
Is DAX Volume
```

Per eseguire una query sull'unità F per informazioni sul volume specifiche di NTFS, digitare:

```
fsutil fsinfo ntfsinfo f:
```

Viene visualizzato un output simile al seguente:

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

Per eseguire una query sull'hardware sottostante file system per informazioni sul settore, digitare:

```
fsinfo sectorinfo d:
```

Viene visualizzato un output simile al seguente:

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector :                                 4096
PhysicalBytesPerSectorForAtomicity :                    4096
.
.
.
Trim Not Supported
DAX capable
```

Per eseguire una query sulle statistiche di file system per l'unità E, digitare:

```
fsinfo statistics e:
```

Viene visualizzato un output simile al seguente:

```
File System Type :     NTFS
Version :              1
UserFileReads :        75021
UserFileReadBytes :    1305244512
.
.
.
LogFileWriteBytes :    180936704       
```

## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[fsutil](Fsutil.md)


