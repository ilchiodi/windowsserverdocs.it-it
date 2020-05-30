---
title: sblocco automatico gestione-BDE
description: Argomento di riferimento per il comando Manage-bde autounlock, che gestisce lo sblocco automatico delle unità dati protette da BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a214ba868e04a81e6282dc919c93ab626ef26725
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84223008"
---
# <a name="manage-bde-autounlock"></a>sblocco automatico gestione-BDE

Gestisce il sblocco automatico delle unità dati protette da BitLocker.

## <a name="syntax"></a>Sintassi

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -abilitare | Abilita lo sblocco automatico per un'unità di dati. |
| -Disabilita | Disabilita lo sblocco automatico per un'unità di dati. |
| -clearallkeys | Rimuove tutte le stored chiavi esterne nelle unità del sistema operativo. |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per abilitare lo sblocco automatico dell'unità dati E, digitare:

```
manage-bde –autounlock -enable E:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)