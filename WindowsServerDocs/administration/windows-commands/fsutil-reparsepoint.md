---
title: fsutil reparsepoint
description: Argomento di riferimento per il comando fsutil reparsepoint, che consente di eseguire query o eliminare i reparse point.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 56ca18cc4f3b4cdfd9021eb8361d980bb855bdc3
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435726"
---
# <a name="fsutil-reparsepoint"></a>fsutil reparsepoint

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Esegue una query o Elimina i reparse point.  Il comando **fsutil reparsepoint** viene in genere utilizzato dai professionisti del supporto tecnico.

I reparse point sono oggetti file system NTFS che hanno un attributo definibile, che contiene dati definiti dall'utente. Sono usati per:

- Estendere la funzionalità nel sottosistema di input/output (I/O).

- Fungere da punti di giunzione di directory e punti di montaggio del volume.

- Contrassegnare determinati file come speciali per un driver di filtro file system.

## <a name="syntax"></a>Sintassi

```
fsutil reparsepoint [query] <filename>
fsutil reparsepoint [delete] <filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| query | Recupera i dati di reparse point associati al file o alla directory identificata dall'handle specificato. |
| eliminare | Elimina un reparse point dal file o dalla directory identificato dall'handle specificato, ma non elimina il file o la directory. |
| `<filename>` | Specifica il percorso completo del file, inclusi il nome file e l'estensione, ad esempio *C:\documents\filename.txt*. |

#### <a name="remarks"></a>Osservazioni

- Quando un programma imposta un reparse point, archivia questi dati, oltre a un tag reparse, che identifica in modo univoco i dati archiviati. Quando il file system apre un file con un reparse point, tenta di trovare il filtro file system associato. Se il filtro file system viene trovato, il filtro elabora il file come indicato dai dati di analisi. Se non viene trovato alcun filtro di file system, l'operazione di **apertura file** non riesce.

### <a name="examples"></a>Esempi

Per recuperare i dati di reparse point associati a *c:\Server*, digitare:

```
fsutil reparsepoint query c:\server
```

Per eliminare un reparse point da un file o da una directory specificata, usare il formato seguente:

```
fsutil reparsepoint delete c:\server
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
