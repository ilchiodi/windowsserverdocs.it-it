---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: Fsutil reparsepoint
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: fe274ad9a6dffc72607102d3430ba7527d3cc558
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376848"
---
# <a name="fsutil-reparsepoint"></a>Fsutil reparsepoint
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Esegue una query o Elimina i reparse point.  Il comando **fsutil reparsepoint** viene in genere utilizzato dai professionisti del supporto tecnico.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>Parametri

| Parametro  |                                                                Descrizione                                                                |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|   query    |            Recupera i dati di reparse point associati al file o alla directory identificata dall'handle specificato.             |
|   Elimina   | Elimina un reparse point dal file o dalla directory identificato dall'handle specificato, ma non elimina il file o la directory. |
| <FileName> |             Specifica il percorso completo del file, inclusi il nome file e l'estensione, ad esempio C:\documents\filename.txt.             |

## <a name="remarks"></a>Note

-   I reparse point sono oggetti file system NTFS che hanno un attributo definibile che contiene dati definiti dall'utente e vengono usati per estendere le funzionalità nel sottosistema di input/output (I/O).

-   I reparse point vengono usati per i punti di giunzione di directory e i punti di montaggio del volume. Vengono inoltre utilizzati dal driver di filtro di file system per contrassegnare determinati file come speciali per tale driver.

-   Quando un programma imposta un reparse point, archivia questi dati, oltre a un tag reparse, che identifica in modo univoco i dati archiviati. Quando il file system apre un file con un reparse point, tenta di trovare il filtro file system associato. Se il filtro file system viene trovato, il filtro elabora il file come indicato dai dati di analisi. Se non viene trovato alcun filtro di file system, l'operazione di apertura file non riesce.

## <a name="BKMK_examples"></a>Esempi
Per recuperare i dati di reparse point associati a C:\Server, digitare:

```
fsutil reparsepoint query c:\server
```

Per eliminare un reparse point da un file o da una directory specificata, usare il formato seguente:

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


