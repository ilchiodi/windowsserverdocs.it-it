---
title: convert mbr
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da8d62567863bc38a5aa0b35a8f3fe4ee24cc888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834612"
---
# <a name="convert-mbr"></a>convert mbr



Converte un disco di base vuoto con lo stile di partizione della tabella di partizione GUID (GPT) in un disco di base con lo stile di partizione avvio principale (MBR) record.

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco MBR. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o volumi prima di convertire il disco.

Per istruzioni su come usare questo comando, vedere [modificare un disco GPT in un disco MBR Master](https://go.microsoft.com/fwlink/?LinkId=207050) (https://go.microsoft.com/fwlink/?LinkId=207050).

## <a name="syntax"></a>Sintassi

```
convert mbr [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Per eseguire questa operazione, è necessario selezionare un disco di base. Usare la **disco selezionare** comando per selezionare un disco di base e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per convertire un disco di base di stile di partizione GPT in stile di partizione MBR, digitare >:
```
convert mbr
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

