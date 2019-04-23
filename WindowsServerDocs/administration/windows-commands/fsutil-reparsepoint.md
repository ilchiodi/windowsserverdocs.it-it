---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: Fsutil reparsepoint
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 940131a02a5cd3a6122022cf9b0dff3281d1dabf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847942"
---
# <a name="fsutil-reparsepoint"></a>Fsutil reparsepoint
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Le query o le eliminazioni reparse point.  Il **reparsepoint fsutil** comando viene usato in genere professionisti del supporto tecnico.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|query|Recupera i dati di punto di analisi che sono associati il file o directory identificata dall'handle specificato.|
|Elimina|Elimina un reparse point dal file o della directory è identificato dall'handle specificato, ma non elimina il file o directory.|
|<FileName>|Specifica il percorso completo del file incluso il nome del file e l'estensione, ad esempio C:\documents\filename.txt.|

## <a name="remarks"></a>Note

-   I punti di analisi sono file system NTFS gli oggetti di sistema che hanno un attributo definibile che contiene dati definiti dall'utente e vengono usate per estendere le funzionalità del sottosistema di input/output (i/o).

-   Analisi punti vengono utilizzati per i punti di giunzione directory e punti di montaggio del volume. Vengono inoltre utilizzati dal driver di filtro di file system per contrassegnare determinati file come speciali per tale driver.

-   Quando un programma imposta un reparse point, archivia i dati, oltre a un tag di reparse, che identifica in modo univoco i dati che vengono archiviati. Quando il file system si apre un file con un reparse point, tenta di individuare il filtro di sistema di file associato. Se viene trovato il filtro del file system, il filtro elabora il file come indicato dai dati di analisi. Se non viene trovato alcun filtro del file system, l'operazione di apertura File ha esito negativo.

## <a name="BKMK_examples"></a>Esempi
Per recuperare i dati di punto di analisi associati C:\Server, digitare:

```
fsutil reparsepoint query c:\server
```

Per eliminare un reparse point da un file o directory, usare il formato seguente:

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


