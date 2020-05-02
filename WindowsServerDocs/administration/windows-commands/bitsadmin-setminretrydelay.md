---
title: bitsadmin setminretrydelay
description: Argomento di riferimento per il comando Bitsadmin setminretrydelay, che consente di impostare il periodo di tempo minimo, espresso in secondi, che BITS attende dopo aver rilevato un errore temporaneo prima di provare a trasferire il file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fc54b4466d8f0bac12bd42ebf6c5e2c66087a15
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720117"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Imposta il periodo di tempo minimo, espresso in secondi, di attesa dei bit dopo che si è verificato un errore temporaneo prima di provare a trasferire il file.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| retrydelay | Tempo minimo di attesa dei bit dopo un errore durante il trasferimento, in secondi. |

## <a name="examples"></a>Esempi

Per impostare il ritardo minimo tra tentativi e 35 secondi per il processo denominato *myDownloadJob*:

```
bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
