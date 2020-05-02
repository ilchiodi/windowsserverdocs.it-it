---
title: shrink
description: Argomento di riferimento per la compattazione DiskPart, che consente di ridurre le dimensioni del volume selezionato in base al valore specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 357a2320bf8b26130c9aa148d513edff6f1e85db
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721804"
---
# <a name="shrink"></a>shrink

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando di compattazione DiskPart riduce la dimensione del volume selezionato in base al valore specificato. Questo comando rende disponibile spazio libero su disco dallo spazio inutilizzato alla fine del volume.

## <a name="syntax"></a>Sintassi
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
### <a name="parameters"></a>Parametri

|  Parametro  |                                                                                             Descrizione                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desiderata =<n> |                                                     Specifica la quantità di spazio desiderata in megabyte (MB) per ridurre le dimensioni del volume.                                                     |
| valore minimo =<n> |                                                           Specifica la quantità minima di spazio in MB per ridurre le dimensioni del volume in base a.                                                           |
|  QueryMax   |                       Restituisce la quantità massima di spazio in MB in base alla quale è possibile ridurre il volume. Questo valore può cambiare se le applicazioni accedono al volume.                        |
|   NOWAIT    |                                                       impone la restituzione immediata del comando mentre il processo di compattazione è ancora in corso.                                                        |
|    NOERR    | solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="remarks"></a>Osservazioni
- È possibile ridurre le dimensioni di un volume solo se formattato con il file system NTFS o se non dispone di un file system.
- Questo comando funziona su volumi di base e su volumi dinamici semplici o con spanning.
- Se non si specifica una quantità desiderata, il volume verrà ridotto in base all'importo minimo (se specificato).
- Se non viene specificato un valore minimo, il volume verrà ridotto in base alla quantità desiderata (se specificato).
- Se non si specifica né un importo minimo né un importo desiderato, il volume verrà ridotto per quanto possibile.
- Se viene specificato un importo minimo, ma non è disponibile spazio libero sufficiente, il comando avrà esito negativo.
- Per eseguire questa operazione, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.
- Questo comando non funziona su partizioni OEM (Original Equipment Manufacturer), partizioni di sistema Extensible Firmware Interface (EFI) o partizioni di ripristino.
  ## <a name="examples"></a>Esempi
  Per ridurre le dimensioni del volume selezionato in base alla quantità massima possibile compresa tra 250 e 500 megabyte, digitare:
  ```
  shrink desired=500 minimum=250
  ```
  Per visualizzare il numero massimo di MB per cui è possibile ridurre il volume, digitare:
  ```
  shrink querymax
  ```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
