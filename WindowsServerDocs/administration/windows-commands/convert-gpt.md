---
title: convert gpt
description: Windows Commands Topic for Convert GPT, che converte un disco di base vuoto con lo stile di partizione MBR (master boot record) in un disco di base con lo stile di partizione GPT (tabella di partizione GUID).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c1ffe61245f7752ccc81d21d513fa00acd7b68b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847284"
---
# <a name="convert-gpt"></a>convert gpt

Converte un disco di base vuoto con lo stile di partizione MBR (master boot record) in un disco di base con lo stile di partizione GPT (tabella di partizione GUID).

Per istruzioni su come usare questo comando, vedere [modificare un disco di record di avvio principale in un disco della tabella di partizione GUID](https://go.microsoft.com/fwlink/?LinkId=207049) (https://go.microsoft.com/fwlink/?LinkId=207049).

## <a name="syntax"></a>Sintassi

```
convert gpt [noerr]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco GPT. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o i volumi prima di convertire il disco.
> -   Le dimensioni minime del disco necessarie per la conversione in GPT sono pari a 128 megabyte.
> -   Per eseguire questa operazione, è necessario selezionare un disco MBR di base. Usare il comando **Seleziona disco** per selezionare un disco di base e spostare lo stato attivo a esso.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per convertire un disco di base dallo stile di partizione MBR nello stile di partizione GPT, digitare:
```
convert gpt
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

