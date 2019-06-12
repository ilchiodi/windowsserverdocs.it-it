---
title: assign
description: Argomento i comandi di Windows per **assegnare** -assegna un punto di montaggio o lettera di unità al volume con lo stato attivo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e7f680fe93e846f5b916cf3210a7ca61f190674
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435306"
---
# <a name="assign"></a>assign

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Assegna un punto di montaggio o lettera di unità al volume con lo stato attivo.

## <a name="syntax"></a>Sintassi
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Parametri

|  Parametro   |                                                                                                                                 Descrizione                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  letter=<d>  |                                                                                                             La lettera di unità che si desidera assegnare al volume.                                                                                                              |
| mount=<path> | Il percorso del punto di montaggio da assegnare al volume.<br /><br />Per istruzioni su come usare questo comando, vedere [assegnare un percorso cartella del punto di montaggio in un'unità](https://go.microsoft.com/fwlink/?LinkId=207059) (<https://go.microsoft.com/fwlink/?LinkId=207059>). |
|    NOERR     |                                    Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                     |

## <a name="remarks"></a>Note
- Se non viene specificato alcun punto di montaggio o lettera di unità, viene assegnata la lettera di unità disponibile successiva. Se il punto di montaggio o lettera di unità è già in uso, viene generato un errore.
- Usando il comando di assegnazione, è possibile modificare la lettera di unità associata a un'unità rimovibile.
- È possibile assegnare lettere di unità per i volumi del sistema, i volumi di avvio o volumi che contengono il file di paging. Inoltre, è possibile assegnare una lettera di unità a una partizione Original Equipment Manufacturer (OEM) o qualsiasi partizione di tabella di partizione GUID (gpt) diverso da una partizione di dati di base.
- Per eseguire questa operazione, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.
  ## <a name="BKMK_examples"></a>Esempi
  Per assegnare una lettera E al volume attivo, digitare:
  ```
  assign letter=e
  ```

