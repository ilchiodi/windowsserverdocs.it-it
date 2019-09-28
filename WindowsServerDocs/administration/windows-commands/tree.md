---
title: tree
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22875e63526dc3465021c9aa990f6cea388b81e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385624"
---
# <a name="tree"></a>tree



Visualizza la struttura di directory di un percorso o del disco in un'unità graficamente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> \<Drive:|Specifica l'unità che contiene il disco per il quale si desidera visualizzare la struttura di directory.|
|\<Path >|Specifica la directory per la quale si desidera visualizzare la struttura di directory.|
|/f|Visualizza i nomi dei file in ogni directory.|
|/a|Specifica che l' **albero** deve usare caratteri di testo anziché caratteri grafici per visualizzare le linee che collegano le sottodirectory.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

La struttura visualizzata dall' **albero** dipende dai parametri specificati al prompt dei comandi. Se non si specifica un'unità o un percorso, **Tree** Visualizza la struttura ad albero che inizia con la directory corrente dell'unità corrente.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare i nomi di tutte le sottodirectory del disco nell'unità corrente, digitare:
```
tree \
```
Per visualizzare una schermata alla volta, i file di tutte le directory sull'unità C, digitare:
```
tree c:\ /f | more 
```
Per stampare un elenco di tutte le directory nell'unità C, digitare:
```
tree c:\ /f  prn 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)