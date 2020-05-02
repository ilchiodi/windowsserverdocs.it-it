---
title: importazione reg
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e7e033091752f97086fd27fcb94e62469f0cced
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722549"
---
# <a name="reg-import"></a>importazione reg



Copia il contenuto di un file che contiene esportate le sottochiavi del Registro di sistema, le voci e valori nel Registro di sistema del computer locale.



## <a name="syntax"></a>Sintassi

```
Reg import FileName
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> FileName|Specifica il nome e percorso del file di contenuto deve essere copiato nel Registro di sistema del computer locale. Questo file deve essere creato in anticipo tramite **reg export**.|
|/?|Visualizza la Guida per **reg import** al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Nella tabella seguente sono elencati i valori restituiti per il **reg import** operazione.

|valore|Descrizione|
|-----|-----------|
|0|Operazione completata|
|1|Errore|

## <a name="examples"></a>Esempi

Per importare le voci del Registro di sistema dal file AppBkUp, digitare:
```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)