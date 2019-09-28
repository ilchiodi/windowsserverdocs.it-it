---
title: Elimina disco
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 886e84edf80227d42ad77e36aed388b9e853ca62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378675"
---
# <a name="delete-disk"></a>Elimina disco



Elimina un disco dinamico mancante dall'elenco dei dischi.

Per istruzioni su come usare questo comando, vedere [rimuovere un disco dinamico mancante](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Sintassi

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|
|eseguire l'override|Consente a DiskPart di eliminare tutti i volumi semplici sul disco. Se il disco contiene metà di un volume con mirroring, la metà del mirror sul disco viene eliminata. Il comando Elimina override disco ha esito negativo se il disco è membro di un volume RAID-5.|

## <a name="BKMK_examples"></a>Esempi

Per eliminare un disco dinamico mancante dall'elenco dei dischi, digitare:
```
delete disk
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

