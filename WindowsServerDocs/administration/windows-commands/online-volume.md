---
title: volume online
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb2ee396e4fa8a2e61001df0d979d85dabe1aa32
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723426"
---
# <a name="online-volume"></a>volume online



Visualizza i volumi attualmente offline allo stato online

> [!IMPORTANT]
> Questo comando non è disponibile nelle edizioni di Windows Vista.

> [!IMPORTANT]
> Questo comando avrà esito negativo se viene utilizzato in un volume di sola lettura.

## <a name="syntax"></a>Sintassi

```
online volume [noerr]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Osservazioni

-   Questo comando viene eseguito sui volumi che non sono riuscita, hanno esito negativo o sono in stato di errore di ridondanza.
-   Per questo comando abbia esito positivo, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.

## <a name="examples"></a>Esempi

Per portare il volume con lo stato attivo in linea, digitare:
```
online volume
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

