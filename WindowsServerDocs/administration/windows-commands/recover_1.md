---
title: recover
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a77e83f1a7143a82fd626390c7373dc87afdb17
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722615"
---
# <a name="recover"></a>recover



Aggiorna lo stato di tutti i dischi in un gruppo di dischi, tenta di ripristinare i dischi in un gruppo di dischi non valido e risincronizza i volumi con mirroring e i volumi RAID-5 con dati non aggiornati.

> [!IMPORTANT]
> Questo comando di DiskPart non è disponibile in qualsiasi edizione di Windows Vista.

## <a name="syntax"></a>Sintassi

```
recover [noerr]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Osservazioni

-   Questo comando viene eseguito in un gruppo di dischi.
-   Questo comando si applica solo a gruppi di dischi dinamici. Se questo comando viene utilizzato in un gruppo con un disco di base, non verrà restituito un errore ma non verrà eseguita alcuna azione.
-   Questo comando funziona su dischi non sono riusciti o errori. Funziona anche sui volumi che hanno avuto esito negativo, esito negativo, o in stato di errore di ridondanza.
-   Per questo comando abbia esito positivo, è necessario selezionare un disco facente parte di un gruppo di dischi. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="examples"></a>Esempi

Per ripristinare il gruppo di dischi che contiene il disco con lo stato attivo, digitare:
```
recover
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

