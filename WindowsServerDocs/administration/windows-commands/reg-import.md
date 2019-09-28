---
title: importazione reg
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e1c7920a64469717c30cfcddda7b8002db5ba10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384727"
---
# <a name="reg-import"></a>importazione reg



Copia il contenuto di un file che contiene esportate le sottochiavi del Registro di sistema, le voci e valori nel Registro di sistema del computer locale.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Reg import FileName
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> FileName|Specifica il nome e percorso del file di contenuto deve essere copiato nel Registro di sistema del computer locale. Questo file deve essere creato in anticipo tramite **reg export**.|
|/?|Visualizza la Guida per **reg import** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **reg import** operazione.

|Value|Descrizione|
|-----|-----------|
|0|Riuscito|
|1|Errore|

## <a name="BKMK_examples"></a>Esempi

Per importare le voci del Registro di sistema dal file AppBkUp, digitare:
```
reg import AppBkUp.reg
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)