---
title: ren
description: Informazioni su come rinominare un file o una directory con il comando ren.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 235497b09f44f9077b7f622f7f2b68a0bc49af86
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836044"
---
# <a name="ren"></a>ren

Rinomina file o directory. Questo comando è analogo a come il **rinominare** comando.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<unità >:] [percorso\<>]\<FileName1 >|Specifica il percorso e nome del file o set di file che si desidera rinominare. *Filename1* può includere caratteri jolly ( **&#42;** e **?** ).|
|\<FileName2 >|Specifica il nuovo nome per il file. È possibile utilizzare caratteri jolly per specificare un nuovo nome per più file.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- È possibile specificare una nuova unità o percorso quando la ridenominazione dei file.
- Non è possibile utilizzare il **ren** comando per rinominare i file su unità disco o spostare file in una directory diversa.
- È possibile utilizzare caratteri jolly ( **&#42;** e **?** ) in entrambi i parametri *filename* . I caratteri sono rappresentati da caratteri jolly in *FileName2* siano identici ai caratteri corrispondenti in *FileName1*.
- *FileName2* deve essere un nome file univoco. Se *FileName2* corrisponde a un nome di file esistente, **ren** Visualizza il messaggio seguente:  
  ```
  Duplicate file name or file not found
  ```

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi

Per modificare tutte le estensioni di file con estensione txt nella directory corrente alle estensioni di file con estensione doc, digitare:
```
ren *.txt *.doc 
```
Per modificare il nome di una directory da Chap10 in Paragr10, digitare:
```
ren chap10 part10 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)