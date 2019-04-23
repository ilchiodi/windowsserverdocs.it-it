---
title: tree
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: de22de1c9d62ba79c1aa68248109cca88009703a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872632"
---
# <a name="tree"></a>tree



Visualizza graficamente la struttura di directory di un percorso o del disco in un'unità.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Drive>:|Specifica l'unità che contiene il disco per il quale si desidera visualizzare la struttura di directory.|
|\<Path>|Specifica la directory per il quale si desidera visualizzare la struttura di directory.|
|/f|Visualizza i nomi dei file in ogni directory.|
|/a|Specifica che **albero** consiste nell'utilizzare caratteri di testo invece di caratteri grafici per visualizzare le linee che collegano le sottodirectory.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

La struttura visualizzata da **albero** dipende dalla fase i parametri specificati al prompt dei comandi. Se non si specifica un'unità o percorso **albero** Visualizza l'inizio della struttura ad albero con la directory corrente dell'unità corrente.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare i nomi di tutte le sottodirectory nel disco nell'unità corrente, digitare:
```
tree \
```
Per visualizzare, una schermata alla volta, i file in tutte le directory sull'unità C, digitare:
```
tree c:\ /f | more 
```
Per stampare un elenco di tutte le directory sull'unità C, digitare:
```
tree c:\ /f  prn 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)