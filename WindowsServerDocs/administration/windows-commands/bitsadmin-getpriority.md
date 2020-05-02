---
title: bitsadmin getpriority
description: Argomento di riferimento per il comando Bitsadmin GetPriority, che recupera la priorità del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 38f92e83ccf5b048d168ce6a21c6026f490b18bf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717674"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

Recupera la priorità del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

#### <a name="output"></a>Output

La priorità restituita per questo comando può essere:

- **FOREGROUND**

- **ALTA**

- **NORMALE**

- **BASSO**

- **SCONOSCIUTO**

## <a name="examples"></a>Esempi

Per recuperare la priorità per il processo denominato *myDownloadJob*:

```
bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
