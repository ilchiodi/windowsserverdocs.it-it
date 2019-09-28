---
title: newdriveletter BdeHdCfg
description: Argomento dei comandi di Windows per BdeHdCfg newdriveletter-assegna una nuova lettera di unità alla parte di un'unità usata come unità di sistema.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2abd4a686f358b5dd844514735edb3ffaa13845
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382227"
---
# <a name="bdehdcfg-newdriveletter"></a>BdeHdCfg: newdriveletter



Assegna una nuova lettera di unità alla parte di un'unità utilizzata come unità di sistema. Per un esempio di come è possibile usare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<DriveLetter >|Definisce la lettera di unità che verrà assegnata all'unità di destinazione specificata.|

## <a name="remarks"></a>Note

Come procedura consigliata, è consigliabile non assegnare una lettera di unità all'unità di sistema.

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene mostrata l'unità predefinita a cui viene assegnata la lettera di unità P.
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)