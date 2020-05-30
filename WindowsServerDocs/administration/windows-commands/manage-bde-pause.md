---
title: 'gestione: Sospendi BDE'
description: Argomento di riferimento per il comando Manage-bde pause, che sospende la crittografia o la decrittografia di BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: efda0e08-b9ff-4e71-83d8-bb666b3032bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8eaa28d6be61c5a06698a39be02cd20ff8b72a8
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222849"
---
# <a name="manage-bde-pause"></a>gestione: Sospendi BDE

Pause BitLocker crittografia o decrittografia.

## <a name="syntax"></a>Sintassi

```
manage-bde -pause [<volume>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<volume>` | Specifica una lettera di unità seguita da due punti, un percorso GUID volume o un volume montato. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per sospendere la crittografia BitLocker nell'unità C, digitare:

```
manage-bde pause C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde on](manage-bde-on.md)

- [comando Manage-bde off](manage-bde-off.md)

- [comando Manage-bde resume](manage-bde-resume.md)

- [comando Manage-bde](manage-bde.md)
