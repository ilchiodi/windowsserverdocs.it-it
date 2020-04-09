---
title: assign
description: Windows Commands argomento for **assign**, che assegna una lettera di unità o un punto di montaggio al volume con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4745b0472e2c8ee7a4034d9a06d395d6089db6f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851304"
---
# <a name="assign"></a>assign

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Assegna una lettera di unità o un punto di montaggio al volume con lo stato attivo.

## <a name="syntax"></a>Sintassi

```
assign [{letter=<d> | mount=<path>}] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `letter=<d>` | Lettera di unità che si desidera assegnare al volume. |
| `mount=<path>` | Percorso del punto di montaggio che si desidera assegnare al volume. Per istruzioni sull'uso di questo comando, vedere [assegnare un percorso della cartella del punto di montaggio a un'unità](https://go.microsoft.com/fwlink/?LinkId=207059). |
| NOERR | solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="remarks"></a>Note

- Se non viene specificata alcuna lettera di unità o punto di montaggio, viene assegnata la lettera di unità successiva disponibile. Se la lettera di unità o il punto di montaggio è già in uso, viene generato un errore.

- Tramite il comando assign è possibile modificare la lettera di unità associata a un'unità rimovibile.

- Non è possibile assegnare lettere di unità ai volumi di sistema, ai volumi di avvio o ai volumi che contengono il file di paging. Non è inoltre possibile assegnare una lettera di unità a una partizione OEM (Original Equipment Manufacturer) o a una partizione GPT (GUID Partition Table) diversa da una partizione di dati di base.

- Per eseguire questa operazione, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per assegnare la lettera E al volume nello stato attivo, digitare:
```
assign letter=e
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Chiave sintassi della riga di comando] (command-line-syntax-key.md

