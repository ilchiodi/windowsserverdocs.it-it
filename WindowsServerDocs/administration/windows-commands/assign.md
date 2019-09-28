---
title: assign
description: Windows Commands argomento for **assign** -assegna una lettera di unità o un punto di montaggio al volume con lo stato attivo.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3e07edcd4ac4ddf5eca1e57da17df441043d15f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382682"
---
# <a name="assign"></a>assign

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

assegna una lettera di unità o un punto di montaggio al volume con lo stato attivo.

## <a name="syntax"></a>Sintassi
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Parametri

|  Parametro   |                                                                                                                                 Descrizione                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  lettera = <d>  |                                                                                                             Lettera di unità che si desidera assegnare al volume.                                                                                                              |
| Mount = <path> | Percorso del punto di montaggio che si desidera assegnare al volume.<br /><br />per istruzioni su come usare questo comando, vedere [assegnare un percorso della cartella del punto di montaggio a un'unità](https://go.microsoft.com/fwlink/?LinkId=207059) (<https://go.microsoft.com/fwlink/?LinkId=207059>). |
|    NOERR     |                                    Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                     |

## <a name="remarks"></a>Note
- Se non viene specificata alcuna lettera di unità o punto di montaggio, viene assegnata la lettera di unità successiva disponibile. Se la lettera di unità o il punto di montaggio è già in uso, viene generato un errore.
- Tramite il comando assign è possibile modificare la lettera di unità associata a un'unità rimovibile.
- Non è possibile assegnare lettere di unità ai volumi di sistema, ai volumi di avvio o ai volumi che contengono il file di paging. Non è inoltre possibile assegnare una lettera di unità a una partizione OEM (Original Equipment Manufacturer) o a una partizione GPT (GUID Partition Table) diversa da una partizione di dati di base.
- Per eseguire questa operazione, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.
  ## <a name="BKMK_examples"></a>Esempi
  Per assegnare la lettera E al volume nello stato attivo, digitare:
  ```
  assign letter=e
  ```

