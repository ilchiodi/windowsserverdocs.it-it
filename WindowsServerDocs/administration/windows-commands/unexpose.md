---
title: Annulla l'esposizione
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe9cb5dfd8ae6c71fdc72ddc1e8421229f98f5d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837472"
---
# <a name="unexpose"></a>Annulla l'esposizione



Unexposes una copia shadow esposta tramite il **esporre** comando. La copia shadow esposta può essere specificata dal relativo ID Shadow, lettera di unità, una condivisione o un punto di montaggio.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ShadowID>|Unexposes la copia shadow specificata per l'ID di Shadow specificato.|
|\<Unità: >|Unexposes la copia shadow associata con la lettera di unità specificata (ad esempio, l'unità P).|
|\<Share>|La copia shadow associata la condivisione specificata unexposes (ad esempio, \\ \\ *MachineName*\).|
|\<MountPoint>|La copia shadow associata al punto di montaggio specificato unexposes (ad esempio, C:\shadowcopy\).|

## <a name="remarks"></a>Note

-   È possibile utilizzare un alias esistente o una variabile di ambiente al posto di *IDShadow*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

## <a name="BKMK_examples"></a>Esempi

Per annullare l'esposizione della copia shadow associata P unità, digitare:
```
unexpose P:
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)