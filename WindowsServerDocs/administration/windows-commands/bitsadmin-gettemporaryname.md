---
title: bitsadmin gettemporaryname
description: Argomento di riferimento per il comando Bitsadmin gettemporaryname, che indica il nome di file temporaneo del file specificato all'interno del processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7780691f37fb78f1553fa993fd408d224be39ff
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717488"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname

Restituisce il nome del file temporaneo del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /gettemporaryname <job> <file_index>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| file_index | Inizia da 0. |

## <a name="examples"></a>Esempi

Per segnalare il nome file temporaneo del file 2 per il processo denominato *myDownloadJob*:

```
bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
