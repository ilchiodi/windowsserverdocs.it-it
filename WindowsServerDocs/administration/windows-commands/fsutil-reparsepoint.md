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
ms.openlocfilehash: 05c203ef610dda0443ddc845245a4072e617f7a6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725432"
---
# <a name="fsutil-reparsepoint"></a>Fsutil reparsepoint
> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Esegue una query o Elimina i reparse point.  Il comando **fsutil reparsepoint** viene in genere utilizzato dai professionisti del supporto tecnico.



## <a name="syntax"></a>Sintassi

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

### <a name="parameters"></a>Parametri

| Parametro  |                                                                Descrizione                                                                |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|   query    |            Recupera i dati di reparse point associati al file o alla directory identificata dall'handle specificato.             |
|   eliminare   | Elimina un reparse point dal file o dalla directory identificato dall'handle specificato, ma non elimina il file o la directory. |
| <FileName> |             Specifica il percorso completo del file, inclusi il nome file e l'estensione, ad esempio C:\documents\filename.txt.             |

## <a name="remarks"></a>Osservazioni

-   I reparse point sono oggetti file system NTFS che hanno un attributo definibile che contiene dati definiti dall'utente e vengono usati per estendere le funzionalità nel sottosistema di input/output (I/O).

-   I reparse point vengono usati per i punti di giunzione di directory e i punti di montaggio del volume. Vengono inoltre utilizzati dal driver di filtro di file system per contrassegnare determinati file come speciali per tale driver.

-   Quando un programma imposta un reparse point, archivia questi dati, oltre a un tag reparse, che identifica in modo univoco i dati archiviati. Quando il file system apre un file con un reparse point, tenta di trovare il filtro file system associato. Se il filtro file system viene trovato, il filtro elabora il file come indicato dai dati di analisi. Se non viene trovato alcun filtro di file system, l'operazione di apertura file non riesce.

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi
Per recuperare i dati di reparse point associati a C:\Server, digitare:

```
fsutil reparsepoint query c:\server
```

Per eliminare un reparse point da un file o da una directory specificata, usare il formato seguente:

```
fsutil reparsepoint delete c:\server
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


