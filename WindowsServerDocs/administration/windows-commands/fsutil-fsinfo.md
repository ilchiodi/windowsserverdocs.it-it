---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: Fsutil fsinfo
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 434dfde2286538367fb96d168b06983cb4357067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873042"
---
# <a name="fsutil-fsinfo"></a>Fsutil fsinfo
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Elenca tutte le unità, esegue una query del tipo di unità, informazioni sul volume, informazioni sul volume NTFS specifiche o esegue una query delle statistiche di sistema di file.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <VolumePath>
fsutil fsinfo [ntfsinfo] <RootPath>
fsutil fsinfo [statistics] <VolumePath>
fsutil fsinfo [volumeinfo] <RootPath>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|Unità|Elenca tutte le unità del computer.|
|DriveType|Un'unità di una query e visualizza il tipo, ad esempio unità CD-ROM.|
|ntfsinfo|Visualizza le informazioni specifiche di volume NTFS per il volume specificato, ad esempio il numero di settori, complessivo cluster, i cluster gratuiti e inizio e alla fine della zona MFT.|
|sectorinfo|Elenca le informazioni sulle dimensioni di settore e l'allineamento dell'hardware.|
|Statistiche|Elenca le statistiche di sistema per il volume specificato, ad esempio metadati, file di log e MFT letture e scritture di file.|
|volumeinfo|Elenca le informazioni per il volume specificato, ad esempio il file system e se il volume supporta nomi di file tra maiuscole e minuscole, unicode nei nomi di file, le quote del disco o un volume DirectAccess (DAX).|
|<"VolumePath">|Specifica la lettera di unità (seguita da due punti).|
|<"RootPathname">|Specifica la lettera di unità (seguita da due punti) dell'unità radice.|

## <a name="BKMK_examples"></a>Esempi
Per elencare tutte le unità del computer, digitare:

```
fsutil fsinfo drives
```

Output simile a verrà visualizzato il seguente:

```
Drives: A:\ C:\ D:\ E:\       
```

Per eseguire una query il tipo di unità dell'unità C, digitare:

```
fsutil fsinfo drivetype c:
```

Possibili risultati della query includono:

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

Per recuperare le informazioni sul volume per il volume E, digitare:

```
fsinfo volumeinfo e:\
```

Output simile a verrà visualizzato il seguente:

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

Unità di query F per informazioni sul volume NTFS specifici, digitare:

```
fsutil fsinfo ntfsinfo f:
```

Output simile a verrà visualizzato il seguente:

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

Per eseguire query su hardware sottostante del file system per le informazioni sul settore, digitare:

```
fsinfo sectorinfo d:
```

Output simile a verrà visualizzato il seguente:

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

Per recuperare le statistiche di sistema di file per l'unità E, digitare:

```
fsinfo statistics e:
```

Output simile a verrà visualizzato il seguente:

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

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)
[Fsutil](Fsutil.md)


