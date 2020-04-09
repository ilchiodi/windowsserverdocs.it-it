---
title: convert basic
description: Windows Commands argomento for Convert Basic, che converte un disco dinamico vuoto in un disco di base.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e0f6f5f04373042956d83bc9136c884c268e591
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847314"
---
# <a name="convert-basic"></a>convert basic

Converte un disco dinamico vuoto in un disco di base.

Per istruzioni su come usare questo comando, vedere [riportare un disco dinamico in un disco di base](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Sintassi

```
convert basic [noerr]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco di base. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o i volumi prima di convertire il disco.

-   Per eseguire questa operazione, è necessario selezionare un disco dinamico. Usare il comando **Seleziona disco** per selezionare un disco dinamico e spostare lo stato attivo a esso.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per convertire il disco dinamico selezionato in Basic, digitare:
```
convert basic
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

