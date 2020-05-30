---
title: Manage-bde (identificatore)
description: Argomento di riferimento per il comando Manage-bde seidentifier, che imposta il campo identificatore unità sull'unità sul valore specificato nell'impostazione specificare gli identificatori univoci per l'organizzazione Criteri di gruppo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b4a21df9d177d7bf6813abb0d418d7355d5e59a
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222601"
---
# <a name="manage-bde-setidentifier"></a>Manage-bde (identificatore)

Imposta il campo dell'identificatore di unità sull'unità per il valore specificato nel **agli identificatori univoci per l'organizzazione** impostazione criteri di gruppo.

## <a name="syntax"></a>Sintassi

```
manage-bde –setidentifier <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Per impostare il campo identificatore unità BitLocker per C, digitare:

```
manage-bde –setidentifier C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)

- [Guida di ripristino di BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-recovery-guide-plan)
