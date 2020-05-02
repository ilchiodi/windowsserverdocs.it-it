---
title: albero
description: Argomento di riferimento per Tree, che consente di visualizzare graficamente la struttura di directory di un percorso o del disco in un'unità.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94b0429dadc3965c7e41ad5aa881fc902988ec9b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721281"
---
# <a name="tree"></a>albero

Visualizza la struttura di directory di un percorso o del disco in un'unità graficamente.



## <a name="syntax"></a>Sintassi

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> unità:|Specifica l'unità che contiene il disco per il quale si desidera visualizzare la struttura di directory.|
|\<> percorso|Specifica la directory per la quale si desidera visualizzare la struttura di directory.|
|/f|Visualizza i nomi dei file in ogni directory.|
|/a|Specifica che l' **albero** deve usare caratteri di testo anziché caratteri grafici per visualizzare le linee che collegano le sottodirectory.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

La struttura visualizzata dall' **albero** dipende dai parametri specificati al prompt dei comandi. Se non si specifica un'unità o un percorso, **Tree** Visualizza la struttura ad albero che inizia con la directory corrente dell'unità corrente.

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)