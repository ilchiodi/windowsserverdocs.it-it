---
title: bitsadmin setpriority
description: Argomento di riferimento per il comando Bitsadmin sepriority, che imposta la priorità del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 556a1d94700780ea22acc1e4c2f32961c0e43342
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717259"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

Imposta la priorità del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setpriority <job> <priority>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| priority | Imposta la priorità del processo, tra cui:<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |

## <a name="examples"></a>Esempi

Per impostare la priorità per il processo denominato *myDownloadJob* su Normal:

```
bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
