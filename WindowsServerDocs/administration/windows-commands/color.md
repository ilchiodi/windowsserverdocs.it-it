---
title: color
description: Argomento di riferimento per il comando color, che consente di modificare i colori di primo piano e di sfondo nella finestra del prompt dei comandi per la sessione corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94b5a1e4ca4d4a01ea714adc45e64a6efaa32aa6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82711938"
---
# <a name="color"></a>color

Modifiche di primo piano e sfondo colori nella finestra del prompt dei comandi per la sessione corrente. Se utilizzata senza parametri, **colore** Ripristina la finestra prompt dei comandi predefinita i colori di sfondo e primo piano.

## <a name="syntax"></a>Sintassi

```
color [[<b>]<f>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<b>` | Specifica il colore di sfondo. |
| `<f>` | Specifica il colore di primo piano. |
| /? | Visualizza la guida al prompt dei comandi. |

Dove:

Nella tabella seguente sono elencate le cifre esadecimali valide che è possibile utilizzare come valori per `<b>` e `<f>`:

| valore | Colore |
| ----- | ----- |
| 0 | Nero |
| 1 | Blu |
| 2 | Green |
| 3 | Verde acqua |
| 4 | Rosso |
| 5 | Viola |
| 6 | Giallo |
| 7 | bianco |
| 8 | Grigio |
| 9 | Azzurro |
| a | Verde chiaro |
| b | Azzurro |
| c | Rosso |
| d | Viola chiaro |
| e | Giallo |
| f | Sfondo bianco |

#### <a name="remarks"></a>Osservazioni

- Non usare caratteri di spazio `<b>` tra `<f>`e.

- Se si specifica solo una cifra esadecimale, il colore corrispondente viene utilizzato come colore di primo piano e il colore di sfondo è impostato il colore predefinito.

- Per impostare il colore predefinito della finestra del prompt dei comandi, selezionare l'angolo superiore sinistro della finestra del **prompt dei comandi** , selezionare **impostazioni predefinite**, selezionare la scheda **colori** , quindi selezionare i colori da utilizzare per il **testo della schermata** e lo **sfondo dello schermo**.

- Se `<b>` e `<f>` sono dello stesso valore di colore, ERRORLEVEL è impostato su `1`e non viene apportata alcuna modifica al colore di primo piano o di sfondo.

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
