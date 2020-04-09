---
title: title
description: Windows Commands argomento per title, che consente di creare un titolo per la finestra del prompt dei comandi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aae18e97daaef226443c5f18c6c401a4e2b82b59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832804"
---
# <a name="title"></a>title

Crea un titolo della finestra del prompt dei comandi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
title [<String>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<stringa >|Specifica il titolo della finestra del prompt dei comandi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per creare il titolo della finestra per i programmi di batch, includere il **titolo** comando all'inizio di un file batch.
-   Dopo aver impostato un titolo della finestra, è possibile reimpostare solo utilizzando il **titolo** comando.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nel seguente script di esempio, il titolo della finestra del prompt dei comandi viene modificato in aggiornamento dei file mentre il file batch esegue il comando **Copy** . Dopo l'esecuzione del comando, viene visualizzato il testo `Files Updated` e il titolo della finestra del prompt dei comandi viene nuovamente modificato al prompt dei comandi.
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)