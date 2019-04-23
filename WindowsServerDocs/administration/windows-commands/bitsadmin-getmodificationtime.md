---
title: bitsadmin getmodificationtime
description: Argomento i comandi di Windows per **bitsadmin getmodificationtime** -recupera l'ultima ora il processo è stato modificato o dei dati è stati trasferiti.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4257f0ae4868b2f18221ab99268384f778c4bbbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837022"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime



Recupera l'ultima volta che il processo è stato modificato o dei dati è stati trasferiti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetModificationTime <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera l'ora dell'ultima modifica per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetModificationTime myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)