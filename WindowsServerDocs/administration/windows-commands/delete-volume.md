---
title: delete volume
description: Argomento di riferimento per il comando Elimina volume, che consente di eliminare il volume selezionato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59856e89ff96d2881040365d157540dc62c1aeb0
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993102"
---
# <a name="delete-volume"></a>delete volume

Elimina il volume selezionato. Prima di iniziare, è necessario selezionare un volume per la riuscita dell'operazione. Utilizzare il [Selezionare volume](select-volume.md) comando per selezionare un volume e spostare lo stato attivo a esso.

> [!IMPORTANT]
> Non è possibile eliminare il volume di sistema, il volume di avvio o un volume che contiene il file di paging attivo o il dump di arresto anomalo (dump della memoria).

## <a name="syntax"></a>Sintassi

```
delete volume [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per eliminare il volume con lo stato attivo, digitare:

```
delete volume
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [select volume](select-volume.md)

- [comando Delete](delete.md)