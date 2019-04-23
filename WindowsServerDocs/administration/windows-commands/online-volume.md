---
title: volume online
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bacba1e204f1eee2e3d4772ff9024aedbfc4fed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882002"
---
# <a name="online-volume"></a>volume online



Visualizza i volumi attualmente offline allo stato online

> [!IMPORTANT]
> Questo comando non è disponibile in alcuna edizione di Windows Vista.

> [!IMPORTANT]
> Questo comando avrà esito negativo se viene usato in un volume di sola lettura.

## <a name="syntax"></a>Sintassi

```
online volume [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Questo comando viene eseguito sui volumi che non sono riuscita, hanno esito negativo o sono in stato di errore di ridondanza.
-   Per questo comando abbia esito positivo, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per portare il volume con lo stato attivo in linea, digitare:
```
online volume
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

