---
title: stato gestione-BDE
description: Argomento di riferimento per il comando Manage-bde status, che fornisce informazioni su tutte le unità del computer, indipendentemente dal fatto che siano protette con BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32af92ad8f1f12ce006e41f70c6ca4afcff3eb00
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222599"
---
# <a name="manage-bde-status"></a>stato gestione-BDE

Fornisce informazioni su tutte le unità del computer; indipendentemente dal fatto che siano protetti con BitLocker, tra cui:

- Dimensione

- Versione di BitLocker

- Stato di conversione

- Percentuale crittografata

- Metodo di crittografia

- Stato protezione

- Stato di blocco

- Campo di identificazione

- Protezioni con chiave

## <a name="syntax"></a>Sintassi

```
manage-bde -status [<drive>] [-protectionaserrorlevel] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -protectionaserrorlevel | Fa in modo che lo strumento da riga di comando Manage-bde invii il codice restituito **0** se il volume è protetto e **1** se il volume non è protetto. utilizzato più di frequente per gli script batch per determinare se un'unità è protetta da BitLocker. È inoltre possibile utilizzare **-p** come una versione abbreviata di questo comando. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per visualizzare lo stato dell'unità C, digitare:

```
manage-bde –status C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
