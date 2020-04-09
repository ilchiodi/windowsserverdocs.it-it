---
title: color
description: Argomento dei comandi di Windows per color, che modifica i colori di primo piano e di sfondo nella finestra del prompt dei comandi per la sessione corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e89c20d90a3b812fa67b597c4c205d34e725785
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847484"
---
# <a name="color"></a>color

Modifiche di primo piano e sfondo colori nella finestra del prompt dei comandi per la sessione corrente. Se utilizzata senza parametri, **colore** Ripristina la finestra prompt dei comandi predefinita i colori di sfondo e primo piano.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
color [[<B>]<F>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<B >|Specifica il colore di sfondo.|
|\<> F|Specifica il colore di primo piano.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Nella tabella seguente sono elencate le cifre esadecimali valide che è possibile utilizzare come valori per *B* e *F*.

|Valore|Colore|
|-----|-----|
|0|nero|
|1|Blue|
|2|Verde|
|3|Verde acqua|
|4|Rosso|
|5|Purple|
|6|Yellow|
|7|Vuoto|
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

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
