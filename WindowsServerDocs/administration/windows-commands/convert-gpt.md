---
title: convert gpt
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9a6392cbcff618c642b9d0f168fe555e8be9e759
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379087"
---
# <a name="convert-gpt"></a>convert gpt



Converte un disco di base vuoto con lo stile di partizione MBR (master boot record) in un disco di base con lo stile di partizione GPT (tabella di partizione GUID).

Per istruzioni su come utilizzare questo comando, vedere la pagina relativa alla [modifica di un disco di record di avvio principale in un disco della tabella di partizione GUID](https://go.microsoft.com/fwlink/?LinkId=207049) (https://go.microsoft.com/fwlink/?LinkId=207049).

## <a name="syntax"></a>Sintassi

```
convert gpt [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco GPT. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o i volumi prima di convertire il disco.
> -   Le dimensioni minime del disco necessarie per la conversione in GPT sono pari a 128 megabyte.
> -   Per eseguire questa operazione, è necessario selezionare un disco MBR di base. Usare il comando **Seleziona disco** per selezionare un disco di base e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per convertire un disco di base dallo stile di partizione MBR nello stile di partizione GPT, digitare:
```
convert gpt
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

