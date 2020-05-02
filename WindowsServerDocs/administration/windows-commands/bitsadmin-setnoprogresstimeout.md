---
title: bitsadmin setnoprogresstimeout
description: Argomento di riferimento per il comando Bitsadmin setnoprogresstimeout, che consente di impostare il periodo di tempo, in secondi, durante il quale il servizio tenta di trasferire il file dopo che si è verificato un errore temporaneo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 398882cf795e98dc0bbc0fb81006d3406fded707
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720112"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Imposta il periodo di tempo, in secondi, durante il quale BITS tenta di trasferire il file dopo il primo errore temporaneo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setnoprogresstimeout <job> <timeoutvalue>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| TimeOutValue | Tempo di attesa di BITS per il trasferimento di un file dopo il primo errore, in secondi. |

### <a name="remarks"></a>Osservazioni

- L'intervallo di timeout "nessun avanzamento" inizia quando il processo rileva il primo errore temporaneo.

- L'intervallo di timeout si arresta o viene reimpostato quando viene trasferito un byte di dati.

- Se l'intervallo di timeout "nessun avanzamento" supera il valore di *TimeOutValue*, il processo viene inserito in uno stato di errore irreversibile.

## <a name="examples"></a>Esempi

Per impostare il valore di timeout "No Progress" su 20 secondi, per il processo denominato *myDownloadJob*:

```
bitsadmin /setnoprogresstimeout myDownloadJob 20
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
