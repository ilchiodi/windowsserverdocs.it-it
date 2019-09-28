---
title: disco online
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3d798bf34ec2f9d2f01b5470c4ec52f936674135
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372507"
---
# <a name="online-disk"></a>disco online



Visualizza i dischi che sono attualmente offline allo stato online.

> [!IMPORTANT]
> Questo comando non è disponibile nelle edizioni di Windows Vista.

> [!IMPORTANT]
> Questo comando avrà esito negativo se viene utilizzato in un disco di sola lettura.

Per istruzioni su come utilizzare questo comando, vedere [riattivare un disco dinamico mancante o offline](https://go.microsoft.com/fwlink/?LinkId=207046) (https://go.microsoft.com/fwlink/?LinkId=207046).

## <a name="syntax"></a>Sintassi

```
online disk [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Se utilizzato senza parametri in Windows Vista, questo comando opera su un gruppo di dischi. Per i dischi di base, non è mai più di un disco per ogni gruppo. Per i dischi dinamici, il gruppo include tutti i dischi dinamici non esterni.
-   Per i dischi di base, il comando tenterà di portare online il disco selezionato e tutti i volumi presenti sul disco.
-   Per i dischi dinamici, il comando tenterà di portare in linea tutti i dischi che non sono contrassegnati come esterna nel computer locale. Inoltre tenterà di portare in linea tutti i volumi sul set di dischi dinamici.
-   Se un disco dinamico in un gruppo di dischi viene portato in linea ed è il solo disco del gruppo, il gruppo originale è stato ricreato e il disco viene spostato a tale gruppo. Se sono presenti altri dischi nel gruppo e sono in linea, il disco viene semplicemente aggiunto nel gruppo di.
-   Se il gruppo di un disco selezionato contiene volumi con mirroring o RAID-5, questo comando Risincronizza anche questi volumi.
-   È necessario selezionare un disco per questo comando abbia esito positivo. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per portare il disco in linea, digitare:
```
online disk
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

