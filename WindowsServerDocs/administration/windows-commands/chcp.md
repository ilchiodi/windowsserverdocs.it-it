---
title: chcp
description: Windows Commands Topic for CHCP, che modifica la tabella codici della console attiva.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e644cf8544d135c5d21c344b0fd0a3364c7f89c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847944"
---
# <a name="chcp"></a>chcp

Modifica la tabella codici della console attiva. Se utilizzata senza parametri, **chcp** Visualizza il numero della tabella codici della console attiva.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
chcp [<NNN>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<NNN >|Specifica la tabella codici.|
|/?|Visualizza la guida al prompt dei comandi.|

La tabella seguente elenca ogni codice supportate pagina e il paese/regione o lingua:

|Tabella codici|Paese/regione o lingua|
|---------|--------------------------|
|437|Stati Uniti|
|850|Multilingue (latino I)|
|852|Slavo (latino II)|
|855|Cirillico (russo)|
|857|Turco|
|860|Portoghese|
|861|Islandese|
|863|Francese (Canada)|
|865|Area lingue nordiche|
|866|Russo|
|869|Greco moderno|
|936|Cinese|

## <a name="remarks"></a>Note

-   Solo la tabella codici OEM (OEM) viene installata con Windows viene visualizzato correttamente in una finestra del prompt dei comandi che utilizza caratteri Raster. Le altre tabelle codici vengono visualizzati correttamente in modalità a schermo intero o in windows il prompt dei comandi che utilizzano tipi di carattere TrueType.
-   Non è necessario preparare le tabelle codici (ad esempio MS-DOS).
-   I programmi che si avvia dopo aver assegnano una nuova pagina codice, utilizzare la nuova pagina di codice. Tuttavia (eccetto Cmd.exe) i programmi avviati prima di assegnano il nuovo codice pagina Usa la tabella codici originale.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare l'impostazione della tabella codici attiva, digitare:
```
chcp
```
Viene visualizzato un messaggio simile al seguente:

`Active code page: 437`

Per impostare la tabella codici attiva su 850 (multilingue), digitare:
```
chcp 850
```
Se la tabella codici specificata non è valida, viene visualizzato il messaggio di errore seguente:

`Invalid code page`

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
