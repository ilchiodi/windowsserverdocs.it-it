---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Fsutil Wim
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: d4a8f2c008c1a28e498edb7726a8c209e91f41af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843924"
---
# <a name="fsutil-wim"></a>Fsutil Wim
>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows 10

Fornisce funzioni per individuare e gestire i file supportati dall'immagine Windows (WIM).

## <a name="syntax"></a>Sintassi

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|EnumFiles|Enumera i file supportati da WIM.|
|nome unità \<>|Specifica il nome dell'unità.|
|\<origine dati >|Specifica l'origine dati.|
|enumwims|Enumera il backup dei file WIM.|
|queryfile|Esegue una query se il file è supportato da WIM e, in tal caso, Visualizza i dettagli sul file WIM.|
|\<filename >|Specifica il nome del file.|
|removewim|Rimuove un file WIM dai file di backup.|




### <a name="examples"></a>Esempi

Per enumerare i file per l'unità C: dall'origine dati 0, digitare:

```
fsutil wim enumfiles C: 0
```

Per enumerare il backup dei file WIM per l'unità C:, digitare:

```
fsutil wim enumwims C:
```

Per verificare se un file è supportato da WIM, digitare:

```
fsutil wim C:\Windows\Notepad.exe
```

Per rimuovere il file WIM dai file di backup per il volume C: e l'origine dati 2, digitare:

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>Altri riferimenti
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)