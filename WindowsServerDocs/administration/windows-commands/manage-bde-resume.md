---
title: Manage-bde resume
description: Argomento di riferimento per il comando Manage-bde resume, che riprende la crittografia o la decrittografia BitLocker dopo che è stata sospesa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ca3cd1ca-6f2c-4190-b68f-27816635facb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17a41a0a5c97bb20c1010c968e495ffbc81649cf
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222112"
---
# <a name="manage-bde-resume"></a>Manage-bde resume

Riprende la decrittografia o crittografia BitLocker dopo che è stata sospesa.

## <a name="syntax"></a>Sintassi

```
manage-bde -resume [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per riprendere la crittografia BitLocker nell'unità C, digitare:

```
manage-bde –resume C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde on](manage-bde-on.md)

- [comando Manage-bde off](manage-bde-off.md)

- [comando di sospensione Manage-bde](manage-bde-pause.md)

- [comando Manage-bde](manage-bde.md)
