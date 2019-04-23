---
title: Convertire dinamica
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 353c1e4558ab2b0c948ec78c0cd87b579c738ec8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841612"
---
# <a name="convert-dynamic"></a>Convertire dinamica



Converte un disco di base in un disco dinamico.

Per istruzioni su come usare questo comando, vedere [modificare un disco di base in un disco dinamico](https://go.microsoft.com/fwlink/?LinkId=207047) (https://go.microsoft.com/fwlink/?LinkId=207047).

## <a name="syntax"></a>Sintassi

```
convert dynamic [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Tutte le partizioni esistenti nel disco di base più volumi semplici.
-   Per eseguire questa operazione, è necessario selezionare un disco di base. Usare la **disco selezionare** comando per selezionare un disco di base e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per convertire un disco di base in un disco dinamico, digitare:
```
convert dynamic
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

