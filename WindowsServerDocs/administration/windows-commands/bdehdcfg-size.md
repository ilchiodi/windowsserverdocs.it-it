---
title: bdehdcfg dimensioni
description: Argomento i comandi di Windows - specifica la dimensione della partizione di sistema quando viene creata una nuova unità di sistema.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d024bb4092f93782300d6afb9053cee1da32629a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817522"
---
# <a name="bdehdcfg-size"></a>bdehdcfg: size



Specifica le dimensioni della partizione di sistema quando viene creata una nuova unità di sistema. Per un esempio di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<SizeinMB >|Indica il numero di megabyte (MB) da utilizzare per la nuova partizione.|

## <a name="remarks"></a>Note

Se non si specifica una dimensione, verrà utilizzato automaticamente il valore predefinito di 300 MB. La dimensione minima dell'unità di sistema è 100 MB. Se si prevede di archiviare Ripristino di sistema o altri strumenti sulla partizione di sistema, è necessario aumentare di conseguenza la dimensione.

> [!NOTE]
> Il **dimensioni** comando non può essere combinato con il **destinazione** \<LetteraUnità > **merge** comando.

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **dimensioni** comando allocare 500 MB a unità di sistema predefinito.
```
bdehdcfg -target default -size 500
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)