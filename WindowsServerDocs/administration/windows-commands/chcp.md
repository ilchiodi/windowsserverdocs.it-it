---
title: chcp
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 622d4b64128c7e39cc761e4f5e9d69cf54383760
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819202"
---
# <a name="chcp"></a>chcp



Modifica la tabella codici della console attiva. Se utilizzata senza parametri, **chcp** Visualizza il numero della tabella codici della console attiva.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
chcp [<NNN>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<NNN>|Specifica la tabella codici.|
|/?|Visualizza la guida al prompt dei comandi.|

La tabella seguente elenca ogni codice supportate pagina e il paese/area geografica o lingua:

|Tabella codici|Paese/area geografica o lingua|
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

## <a name="BKMK_examples"></a>Esempi

Per visualizzare l'impostazione della tabella codici attiva, digitare:
```
chcp
```
Viene visualizzato un messaggio simile al seguente:

`Active code page: 437`

Per modificare la tabella codici attiva a 850 (multilingue), digitare:
```
chcp 850
```
Se la tabella codici specificata non è valida, viene visualizzato il messaggio di errore seguente:

`Invalid code page`

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
