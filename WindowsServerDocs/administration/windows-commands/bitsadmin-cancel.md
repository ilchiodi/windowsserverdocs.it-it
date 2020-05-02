---
title: bitsadmin cancel
description: Argomento di riferimento per il comando Bitsadmin Cancel, che rimuove il processo dalla coda di trasferimento ed Elimina tutti i file temporanei associati al processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95fefbc4a9731c2ccbac22adc27f8231a7f36138
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718259"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Rimuove il processo dalla coda di trasferimento ed Elimina tutti i file temporanei associati al processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per rimuovere il processo *myDownloadJob* dalla coda di trasferimento:

```
bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
