---
title: bdehdcfg newdriveletter
description: Argomento i comandi di Windows per newdriveletter bdehdcfg - assegna una nuova lettera di unità alla parte di un'unità usata come unità di sistema.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cd40942dfb724d46c0fa9a43c4646e1db09d2a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887142"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg: newdriveletter



Assegna una nuova lettera di unità alla parte di un'unità usata come unità di sistema. Per un esempio di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<DriveLetter>|Definisce la lettera di unità che verrà assegnata all'unità di destinazione specificato.|

## <a name="remarks"></a>Note

Come procedura consigliata, è consigliabile non assegnare una lettera di unità per l'unità di sistema.

## <a name="BKMK_Examples"></a>Esempi

L'esempio seguente mostra l'unità predefinita viene assegnata la lettera di unità P.
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)