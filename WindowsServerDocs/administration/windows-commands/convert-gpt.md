---
title: convert gpt
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e838f68162b6faabf2ecbc7dea2ce840235890c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859112"
---
# <a name="convert-gpt"></a>convert gpt



Converte un disco di base vuoto con lo stile di partizione avvio principale (MBR) record in un disco di base con lo stile di partizione GUID partizione GPT (tabella).

Per istruzioni su come usare questo comando, vedere [convertire un disco MBR Master in un disco GPT](https://go.microsoft.com/fwlink/?LinkId=207049) (https://go.microsoft.com/fwlink/?LinkId=207049).

## <a name="syntax"></a>Sintassi

```
convert gpt [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco GPT. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o volumi prima di convertire il disco.
-   La dimensione minima necessaria per la conversione in GPT è 128 MB.
-   Per eseguire questa operazione, è necessario selezionare un disco MBR di base. Usare la **disco selezionare** comando per selezionare un disco di base e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per convertire un disco di base di stile di partizione MBR in stile di partizione GPT, digitare:
```
convert gpt
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

