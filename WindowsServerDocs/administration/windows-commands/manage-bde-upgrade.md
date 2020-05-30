---
title: aggiornamento di Manage-bde
description: Argomento di riferimento per il comando Manage-bde upgrade, che aggiorna la versione di BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23bfa824-6ff0-44cc-9b8b-b199a769fb8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e1789e119db8a2886c19dab9acfe4c7b12d651d
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222136"
---
# <a name="manage-bde-upgrade"></a>aggiornamento di Manage-bde

Aggiorna la versione di BitLocker.

## <a name="syntax"></a>Sintassi

```
manage-bde -upgrade [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per aggiornare la crittografia BitLocker nell'unità C, digitare:

```
manage-bde –upgrade C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
