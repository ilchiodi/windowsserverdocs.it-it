---
title: convert mbr
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47001415158b3bdb0b06af9114b995a6f0634da1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379069"
---
# <a name="convert-mbr"></a>convert mbr



Converte un disco di base vuoto con lo stile di partizione GPT (tabella di partizione GUID) in un disco di base con lo stile di partizione MBR (master boot record).

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco MBR. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o i volumi prima di convertire il disco.

Per istruzioni su come usare questo comando, vedere [modificare un disco della tabella di partizione GUID in un disco di record di avvio principale](https://go.microsoft.com/fwlink/?LinkId=207050) (https://go.microsoft.com/fwlink/?LinkId=207050).

## <a name="syntax"></a>Sintassi

```
convert mbr [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Per eseguire questa operazione, è necessario selezionare un disco di base. Usare il comando **Seleziona disco** per selezionare un disco di base e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per convertire un disco di base dallo stile di partizione GPT allo stile di partizione MBR, digitare >:
```
convert mbr
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

