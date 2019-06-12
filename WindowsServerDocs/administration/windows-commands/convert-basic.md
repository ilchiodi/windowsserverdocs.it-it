---
title: convert basic
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: df1f999499154366304d59e0573ba921ab1af83d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434246"
---
# <a name="convert-basic"></a>convert basic



Converte un disco dinamico vuoto in un disco di base.

Per istruzioni su come usare questo comando, vedere [modificare un disco dinamico in un disco di base](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Sintassi

```
convert basic [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco di base. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o volumi prima di convertire il disco.
> -   Per eseguire questa operazione, è necessario selezionare un disco dinamico. Usare la **disco selezionare** comando per selezionare un disco dinamico e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per convertire il disco dinamico selezionato in basic, digitare:
```
convert basic
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

