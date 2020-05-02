---
title: delete volume
description: Argomento di riferimento per delete volume, che elimina il volume selezionato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9a8ae0fc863cec5c1a3f6debccf8201e96badd0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716687"
---
# <a name="delete-volume"></a>delete volume

Elimina il volume selezionato.

## <a name="syntax"></a>Sintassi

```
delete volume [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="remarks"></a>Osservazioni

-   Non puoi eliminare il volume di sistema, il volume di avvio o qualsiasi volume che contiene il file di paging attivo o il dump di arresto anomalo del sistema (immagine della memoria).
-   Per eseguire questa operazione, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.

## <a name="examples"></a>Esempi

Per eliminare il volume con lo stato attivo, digitare:
```
delete volume
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

