---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: File wim di fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c9186721ce4d3a549964e420cbc16d4893a1859d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826042"
---
# <a name="fsutil-wim"></a>File wim di fsutil
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

Fornisce funzioni per individuare e gestire i file di backup Windows immagine WIM.

## <a name="syntax"></a>Sintassi

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|EnumFiles|Enumera i file WIM supportato.|
|\<nome dell'unità >|Specifica il nome dell'unità.|
|\<origine dati >|Specifica l'origine dati.|
|enumwims|Enumera i file WIM di backup.|
|queryfile|Le query se il file è supportato da WIM e in questo caso, vengono visualizzati i dettagli sui file WIM.|
|\<filename>|Specifica il nome del file.|
|removewim|Rimuove un file WIM da file di backup.|




### <a name="examples"></a>Esempi

Per enumerare i file per l'unità c: origine dati 0, digitare:

```
fsutil wim enumfiles C: 0
```

Per enumerare i file WIM di backup per l'unità c:, digitare:

```
fsutil wim enumwims C:
```

Per vedere se un file è supportato da WIM, digitare:

```
fsutil wim C:\Windows\Notepad.exe
```

Per rimuovere il file WIM da eseguire il backup di file per volume c: e origine dati 2, digitare:

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)