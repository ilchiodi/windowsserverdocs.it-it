---
title: colore
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed792e4626897945e688f1c54767d7680ade6d99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379243"
---
# <a name="color"></a>colore



Modifiche di primo piano e sfondo colori nella finestra del prompt dei comandi per la sessione corrente. Se utilizzata senza parametri, **colore** Ripristina la finestra prompt dei comandi predefinita i colori di sfondo e primo piano.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
color [[<B>]<F>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<B >|Specifica il colore di sfondo.|
|\<F >|Specifica il colore di primo piano.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Nella tabella seguente sono elencate le cifre esadecimali valide che è possibile utilizzare come valori per *B* e *F*.

|Value|Colore|
|-----|-----|
|0|Nero|
|1|Blu|
|2|Verde|
|3|Aqua|
|4|Rosso|
|5|Viola|
|6|Giallo|
|7|Bianco|
|8|Grigio|
|9|Azzurro|
|A|Verde chiaro|
|B|Azzurro|
|C|Rosso|
|D|Viola chiaro|
|E|Giallo|
|F|Sfondo bianco|
    
-   Non utilizzare caratteri di spazio tra *B* e *F*.
-   Se si specifica solo una cifra esadecimale, il colore corrispondente viene utilizzato come colore di primo piano e il colore di sfondo è impostato il colore predefinito.
-   Per impostare il colore predefinito della finestra prompt dei comandi, fare clic nell'angolo superiore sinistro della finestra del prompt dei comandi, fare clic su **impostazioni predefinite**, fare clic sui **colori** scheda e quindi scegliere i colori che si desidera utilizzare per il **testo visualizzato sullo schermo** e **dello sfondo**.
-   Se *B* e *F* sono identici, il **colore** comando imposta ERRORLEVEL su 1, e di primo piano o il colore di sfondo viene apportata alcuna modifica.

## <a name="BKMK_examples"></a>Esempi

Per modificare il colore di sfondo finestra prompt dei comandi in grigio e il colore di primo piano sul rosso, digitare:
```
color 84
```
Per modificare il colore di primo piano finestra prompt dei comandi in giallo, digitare:
```
color e
```

> [!NOTE]
> In questo esempio, lo sfondo è impostato il colore predefinito perché è stata specificata solo da una cifra esadecimale.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
