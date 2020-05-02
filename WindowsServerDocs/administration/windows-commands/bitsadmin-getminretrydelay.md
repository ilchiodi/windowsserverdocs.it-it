---
title: bitsadmin getminretrydelay
description: Argomento di riferimento per il comando Bitsadmin getminretrydelay, che consente di recuperare il tempo, in secondi, di attesa del servizio dopo aver rilevato un errore temporaneo prima di provare a trasferire il file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a50b9d98fe0b873dc58b8e86dc672a8f4157208a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717851"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

Recupera l'intervallo di tempo, in secondi, durante il quale il servizio resterà in attesa di un errore temporaneo prima di tentare di trasferire il file.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare il ritardo minimo dei tentativi per il processo denominato *myDownloadJob*:

```
bitsadmin /getminretrydelay myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
