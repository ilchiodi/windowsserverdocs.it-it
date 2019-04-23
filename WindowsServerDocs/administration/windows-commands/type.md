---
title: Tipo
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 4ceb7365d34a2aeca21d1a699730a589f98fd549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887402"
---
# <a name="type"></a>Tipo


Nella shell dei comandi di Windows, **tipo** è incorporata nel comando che consente di visualizzare il contenuto di un file di testo. Usare la **tipo** comando per visualizzare un file di testo senza modificarlo.


In PowerShell **tipo** è un alias predefinito per il **[Get-Content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** che visualizza anche il contenuto di un file, ma con una sintassi diversa.


Per esempi di come usare questo comando all'interno della shell dei comandi di Windows (Cmd.exe), vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
type [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName>|Specifica il percorso e nome del file o dei file che si desidera visualizzare. Separare più nomi di file con uno spazio.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Se *FileName* contiene spazi, racchiuderlo tra virgolette (ad esempio, "File nome contenente Spaces.txt").
-   Se si visualizza un file binario o un file che viene creato da un programma, si possono vedere strano caratteri sullo schermo, inclusi i caratteri di avanzamento carta e i simboli di sequenza di escape. Questi caratteri rappresentano i codici di controllo che vengono usati nel file binario. In generale, evitare di usare la **tipo** comando per visualizzare i file binari.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare il contenuto di un file denominato vacanze. mar, digitare:
```
type holiday.mar 
```
Per visualizzare il contenuto di un file di lunga durato denominata vacanze. mar una schermata alla volta, digitare:
```
type holiday.mar | more 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
