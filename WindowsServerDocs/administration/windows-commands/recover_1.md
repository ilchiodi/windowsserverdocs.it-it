---
title: recuperare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 261bfd79d74323ad324246e21b84a5eb798ebcdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867182"
---
# <a name="recover"></a>recuperare



Aggiorna lo stato di tutti i dischi in un gruppo di dischi, tentare di ripristinare i dischi in un gruppo di dischi non è valido e risincronizza volumi con mirroring e RAID-5 volumi contenenti i dati non aggiornati.

> [!IMPORTANT]
> Questo comando di DiskPart non è disponibile in qualsiasi edizione di Windows Vista.

## <a name="syntax"></a>Sintassi

```
recover [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Questo comando viene eseguito in un gruppo di dischi.
-   Questo comando si applica solo a gruppi di dischi dinamici. Se questo comando viene utilizzato in un gruppo con un disco di base, non verrà restituito un errore ma non verrà eseguita alcuna azione.
-   Questo comando funziona su dischi non sono riusciti o errori. Funziona anche sui volumi che hanno avuto esito negativo, esito negativo, o in stato di errore di ridondanza.
-   Per questo comando abbia esito positivo, è necessario selezionare un disco facente parte di un gruppo di dischi. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per ripristinare il gruppo di dischi che contiene il disco con lo stato attivo, digitare:
```
recover
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

