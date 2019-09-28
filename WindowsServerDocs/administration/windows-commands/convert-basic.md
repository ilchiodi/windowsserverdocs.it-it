---
title: convert basic
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c4b81126f4a623d841bb5868f786678d7b093581
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379126"
---
# <a name="convert-basic"></a>convert basic



Converte un disco dinamico vuoto in un disco di base.

Per istruzioni su come utilizzare questo comando, vedere la pagina relativa alla [modifica di un disco dinamico in un disco di base](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Sintassi

```
convert basic [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco di base. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o i volumi prima di convertire il disco.
> -   Per eseguire questa operazione, è necessario selezionare un disco dinamico. Usare il comando **Seleziona disco** per selezionare un disco dinamico e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per convertire il disco dinamico selezionato in Basic, digitare:
```
convert basic
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

