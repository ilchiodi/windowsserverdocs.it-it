---
title: bitsadmin getnoprogresstimeout
description: Argomento di riferimento per il comando Bitsadmin getnoprogresstimeout, che consente di recuperare il periodo di tempo, in secondi, durante il quale il servizio tenterà di trasferire il file dopo che si è verificato un errore temporaneo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ee0377bde8a438f23ca571bc9859deef92f18fb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717818"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout

Recupera l'intervallo di tempo, in secondi, durante il quale il servizio tenterà di trasferire il file dopo che si è verificato un errore temporaneo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getnoprogresstimeout <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare il valore di timeout dello stato di avanzamento per il processo denominato *myDownloadJob*:

```
bitsadmin /getnoprogresstimeout myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
