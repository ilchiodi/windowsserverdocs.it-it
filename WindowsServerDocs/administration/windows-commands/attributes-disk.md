---
title: disco attributi
description: Argomento di riferimento per il comando del disco attributi, che Visualizza, imposta o cancella gli attributi di un disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3d378439b30328e4df48020fa4b3288f7af31c6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718900"
---
# <a name="attributes-disk"></a>disco attributi

Visualizza, imposta o cancella gli attributi di un disco. Quando questo comando viene utilizzato per visualizzare gli attributi correnti di un disco, l'attributo del disco di avvio indica il disco utilizzato per avviare il computer. Per un mirror dinamico, viene visualizzato il disco che contiene il plex di avvio del volume di avvio.

> [!IMPORTANT]
> Per avere esito positivo, è necessario selezionare un disco per il comando del **disco attributi** . Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="syntax"></a>Sintassi

```
attributes disk [{set | clear}] [readonly] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| set | Imposta l'attributo specificato del disco con lo stato attivo. |
| deselezionare | Cancella l'attributo specificato del disco con lo stato attivo. |
| readonly | Specifica che il disco è di sola lettura. |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per visualizzare gli attributi del disco selezionato, digitare:

```
attributes disk
```

Per impostare il disco selezionato come di sola lettura, digitare:

```
attributes disk set readonly
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Seleziona disco](select-disk.md)
