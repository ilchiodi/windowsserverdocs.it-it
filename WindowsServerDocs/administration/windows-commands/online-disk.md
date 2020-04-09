---
title: disco online
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c61d852ba71329c3d7345d74fd352a6c19436cec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837894"
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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Se utilizzato senza parametri in Windows Vista, questo comando opera su un gruppo di dischi. Per i dischi di base, non è mai più di un disco per ogni gruppo. Per i dischi dinamici, il gruppo include tutti i dischi dinamici non esterni.
-   Per i dischi di base, il comando tenterà di portare online il disco selezionato e tutti i volumi presenti sul disco.
-   Per i dischi dinamici, il comando tenterà di portare in linea tutti i dischi che non sono contrassegnati come esterna nel computer locale. Inoltre tenterà di portare in linea tutti i volumi sul set di dischi dinamici.
-   Se un disco dinamico in un gruppo di dischi viene portato in linea ed è il solo disco del gruppo, il gruppo originale è stato ricreato e il disco viene spostato a tale gruppo. Se sono presenti altri dischi nel gruppo e sono in linea, il disco viene semplicemente aggiunto nel gruppo di.
-   Se il gruppo di un disco selezionato contiene volumi con mirroring o RAID-5, questo comando Risincronizza anche questi volumi.
-   È necessario selezionare un disco per questo comando abbia esito positivo. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per portare il disco in linea, digitare:
```
online disk
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

