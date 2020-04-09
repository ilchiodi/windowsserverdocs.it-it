---
title: Elimina disco
description: Windows Commands argomento for delete disk, che elimina un disco dinamico mancante dall'elenco di dischi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a767e0689d5fbabb193df37528a0909ab63a1ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846654"
---
# <a name="delete-disk"></a>Elimina disco

Elimina un disco dinamico mancante dall'elenco dei dischi.

Per istruzioni su come usare questo comando, vedere [rimuovere un disco dinamico mancante](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Sintassi

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|
|override|Consente a DiskPart di eliminare tutti i volumi semplici sul disco. Se il disco contiene metà di un volume con mirroring, la metà del mirror sul disco viene eliminata. Il comando Elimina override disco ha esito negativo se il disco è membro di un volume RAID-5.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per eliminare un disco dinamico mancante dall'elenco dei dischi, digitare:
```
delete disk
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

