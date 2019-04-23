---
title: shrink
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 071fa0cd92bcf93537bfb1ce602792266372e911
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845782"
---
# <a name="shrink"></a>shrink

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando di Diskpart compattazione riduce le dimensioni del volume selezionato da quello specificato. Questo comando rende disponibile spazio libero su disco dallo spazio inutilizzato alla fine del volume.

## <a name="syntax"></a>Sintassi
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|desired=<n>|Specifica la quantità di spazio in megabyte (MB) per ridurre le dimensioni del volume desiderata.|
|minimo =<n>|Specifica la quantità minima di spazio in MB per ridurre le dimensioni del volume.|
|querymax|Restituisce la quantità massima di spazio in MB che il volume può essere ridotto. Questo valore può cambiare se applicazioni stanno utilizzando il volume.|
|NOWAIT|forza il comando per restituire immediatamente, mentre il processo di compattazione è ancora in corso.|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|
## <a name="remarks"></a>Note
-   È possibile ridurre le dimensioni di un volume solo se viene formattato con il file system NTFS o se non dispone di un file system.
-   Questo comando funziona su volumi di base e su volumi dinamici semplici o con spanning.
-   Se una quantità desiderata non è specificata, il volume verrà sottratta la quantità minima (se specificato).
-   Se una quantità minima non è specificata, il volume verrà sottratta la quantità desiderata (se specificato).
-   Se viene specificata né una quantità minima né una quantità desiderata, il volume verrà ridotta quanto più possibile.
-   Se si specifica una quantità minima ma non è sufficiente spazio libero è disponibile, il comando avrà esito negativo.
-   Per eseguire questa operazione, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.
-   Questo comando non funziona su partizioni OEM (OEM), system interfaccia EFI (Extensible Firmware Interface) o le partizioni di ripristino.
## <a name="BKMK_examples"></a>Esempi
Per ridurre le dimensioni del volume selezionato in base al valore più grande possibile tra 500 e 250 MB, digitare:
```
shrink desired=500 minimum=250
```
Per visualizzare il numero massimo di MB che è possibile ridurre il volume, digitare:
```
shrink querymax
```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
