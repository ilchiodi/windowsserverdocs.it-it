---
title: in linea del disco
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c30d0853ff0ae065f02c0ee198c8cdcb90c950b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858062"
---
# <a name="online-disk"></a>in linea del disco



Visualizza i dischi che sono attualmente offline allo stato online.

> [!IMPORTANT]
> Questo comando non è disponibile in alcuna edizione di Windows Vista.

> [!IMPORTANT]
> Questo comando avrà esito negativo se viene usato in un disco di sola lettura.

Per istruzioni su come usare questo comando, vedere [riattivare un disco mancante o non in linea dinamica](https://go.microsoft.com/fwlink/?LinkId=207046) (https://go.microsoft.com/fwlink/?LinkId=207046).

## <a name="syntax"></a>Sintassi

```
online disk [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Quando utilizzata senza parametri in Windows Vista, questo comando viene eseguito in un gruppo di dischi. Per i dischi di base, non è mai più di un disco per ogni gruppo. Per i dischi dinamici, il gruppo include tutti i dischi dinamici non esterno.
-   Per i dischi di base, il comando tenterà di portare online il disco selezionato e tutti i volumi presenti sul disco.
-   Per i dischi dinamici, il comando tenterà di portare in linea tutti i dischi che non sono contrassegnati come esterna nel computer locale. Inoltre tenterà di portare in linea tutti i volumi sul set di dischi dinamici.
-   Se un disco dinamico in un gruppo di dischi viene portato in linea ed è il solo disco del gruppo, il gruppo originale è stato ricreato e il disco viene spostato a tale gruppo. Se sono presenti altri dischi nel gruppo e sono in linea, il disco viene semplicemente aggiunto nel gruppo di.
-   Se il gruppo di un disco selezionato contiene volumi con mirroring o RAID-5, questo comando esegue la risincronizzazione anche tali volumi.
-   È necessario selezionare un disco per questo comando abbia esito positivo. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per portare il disco in linea, digitare:
```
online disk
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

