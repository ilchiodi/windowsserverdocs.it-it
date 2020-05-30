---
title: gestione-blocco BDE
description: Argomento di riferimento per il comando Manage-bde lock, che blocca un'unità protetta da BitLocker per impedire l'accesso a tale unità, a meno che non venga fornita la chiave di sblocco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8858e61-3a7e-4d03-8c98-5c09853f35e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 340dc7eb07eaab2583e8b325042803fd8fc82184
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222871"
---
# <a name="manage-bde-lock"></a>gestione-blocco BDE

Blocca un'unità protetta da BitLocker per impedire l'accesso a esso a meno che non viene fornita la chiave di sblocco.

## <a name="syntax"></a>Sintassi

```
manage-bde -lock [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Per bloccare l'unità dati D, digitare:

```
manage-bde –lock D:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
