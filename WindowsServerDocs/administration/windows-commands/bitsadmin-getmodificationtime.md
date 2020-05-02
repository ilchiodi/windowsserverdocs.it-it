---
title: bitsadmin getmodificationtime
description: Argomento di riferimento per il comando Bitsadmin getmodificationtime, che recupera l'ultima volta in cui il processo è stato modificato o il trasferimento dei dati è riuscito.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bab8c317917894a351c03df1efefb17842ecb7d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717834"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime

Recupera l'ultima volta in cui il processo è stato modificato o il trasferimento dei dati è riuscito.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getmodificationtime <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare l'ora dell'Ultima modifica per il processo denominato *myDownloadJob*:

```
bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
