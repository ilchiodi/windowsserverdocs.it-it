---
title: assign
description: Argomento di riferimento per il comando assign, che assegna una lettera di unità o un punto di montaggio al volume con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f17c22a0052ade6f16e7842813a04c95e76b57ab
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718991"
---
# <a name="assign"></a>assign

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Assegna una lettera di unità o un punto di montaggio al volume con lo stato attivo. È anche possibile usare questo comando per modificare la lettera di unità associata a un'unità rimovibile. Se non viene specificata alcuna lettera di unità o punto di montaggio, viene assegnata la lettera di unità successiva disponibile. Se la lettera di unità o il punto di montaggio è già in uso, viene generato un errore.

Per eseguire questa operazione, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.

> [!IMPORTANT]
> Non è possibile assegnare lettere di unità ai volumi di sistema, ai volumi di avvio o ai volumi che contengono il file di paging. Non è inoltre possibile assegnare una lettera di unità a una partizione OEM (Original Equipment Manufacturer) o a una partizione GPT (GUID Partition Table) diversa da una partizione di dati di base.

## <a name="syntax"></a>Sintassi

```
assign [{letter=<d> | mount=<path>}] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `letter=<d>` | Lettera di unità che si desidera assegnare al volume. |
| `mount=<path>` | Percorso del punto di montaggio che si desidera assegnare al volume. Per istruzioni sull'uso di questo comando, vedere [assegnare un percorso della cartella del punto di montaggio a un'unità](https://docs.microsoft.com/windows-server/storage/disk-management/assign-a-mount-point-folder-path-to-a-drive). |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per assegnare la lettera E al volume nello stato attivo, digitare:

```
assign letter=e
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando select volume](select-volume.md)