---
title: title
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42094e0f1231fee5ac9ef0ec9184ba685c8846b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385791"
---
# <a name="title"></a>title



Crea un titolo della finestra del prompt dei comandi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
title [<String>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<String >|Specifica il titolo della finestra del prompt dei comandi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per creare il titolo della finestra per i programmi di batch, includere il **titolo** comando all'inizio di un file batch.
-   Dopo aver impostato un titolo della finestra, è possibile reimpostare solo utilizzando il **titolo** comando.

## <a name="BKMK_examples"></a>Esempi

Nello script di esempio seguente, il titolo della finestra del prompt dei comandi viene modificato in "File di aggiornamento" mentre è in esecuzione il file batch di **Copia** comando. Dopo aver eseguito il comando, il testo `Files Updated` viene visualizzato e il titolo della finestra del prompt dei comandi viene nuovamente impostato su "Prompt dei comandi".
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)