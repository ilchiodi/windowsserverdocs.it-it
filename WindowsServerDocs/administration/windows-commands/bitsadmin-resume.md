---
title: bitsadmin resume
description: Argomento di riferimento per il comando Bitsadmin Resume, che attiva un processo nuovo o sospeso nella coda di trasferimento.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba4cd57ddeeb3c35ca0871c2953fd409ddb57e73
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716999"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

Attiva un processo nuovo o sospeso nella coda di trasferimento. Se il processo è stato ripreso per errore o semplicemente è necessario sospendere il processo, è possibile usare l'opzione di [sospensione Bitsadmin](bitsadmin-suspend.md) per sospendere il processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per riprendere il processo denominato *myDownloadJob*:

```
bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando di sospensione Bitsadmin](bitsadmin-suspend.md)

- [comando Bitsadmin](bitsadmin.md)
