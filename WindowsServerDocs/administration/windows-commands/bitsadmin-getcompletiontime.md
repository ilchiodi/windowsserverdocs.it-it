---
title: bitsadmin getcompletiontime
description: Argomento di riferimento per il comando Bitsadmin getcompletiontime, che consente di recuperare l'ora in cui il processo ha terminato il trasferimento dei dati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b3721401e450ae60fb77534f8eb845ff5ac3443
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718113"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

Recupera l'ora in cui il processo ha completato il trasferimento dei dati.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare l'ora in cui il processo denominato *myDownloadJob* ha terminato il trasferimento dei dati:

```
bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
