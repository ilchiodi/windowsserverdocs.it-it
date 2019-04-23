---
title: Elimina disco
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e401133854118e82a45fd5e01466288ae41f67da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863912"
---
# <a name="delete-disk"></a>Elimina disco



Elimina un disco dinamico mancante nell'elenco dei dischi.

Per istruzioni su come usare questo comando, vedere [rimuovere un disco dinamico mancanti](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Sintassi

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|
|eseguire l'override|Consente a DiskPart eliminare tutti i volumi semplici su disco. Se il disco contiene la metà di un volume con mirroring, la metà del volume con mirroring su disco viene eliminata. Il comando di sostituzione del disco di eliminazione ha esito negativo se il disco è un membro di un volume RAID-5.|

## <a name="BKMK_examples"></a>Esempi

Per eliminare un disco dinamico mancante nell'elenco dei dischi, digitare:
```
delete disk
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

