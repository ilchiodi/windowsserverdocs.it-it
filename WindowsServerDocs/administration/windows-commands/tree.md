---
title: tree
description: Windows Commands Topic for Tree, che visualizza la struttura di directory di un percorso o del disco in un'unità, graficamente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14b9a4dfd5c84b55a32dbc3f6fd7e8a2cc00c7ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832674"
---
# <a name="tree"></a>tree

Visualizza la struttura di directory di un percorso o del disco in un'unità graficamente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> unità \<:|Specifica l'unità che contiene il disco per il quale si desidera visualizzare la struttura di directory.|
|Percorso \<>|Specifica la directory per la quale si desidera visualizzare la struttura di directory.|
|/f|Visualizza i nomi dei file in ogni directory.|
|/a|Specifica che l' **albero** deve usare caratteri di testo anziché caratteri grafici per visualizzare le linee che collegano le sottodirectory.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

La struttura visualizzata dall' **albero** dipende dai parametri specificati al prompt dei comandi. Se non si specifica un'unità o un percorso, **Tree** Visualizza la struttura ad albero che inizia con la directory corrente dell'unità corrente.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)