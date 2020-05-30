---
title: Manage-bde wipefreespace
description: Argomento di riferimento per il comando Manage-bde wipefreespace, che cancella lo spazio disponibile nel volume rimuovendo i frammenti di dati che potrebbero esistere nello spazio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09bc59ab631dd1fa2177b50aa4187b30239571bd
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222128"
---
# <a name="manage-bde-wipefreespace"></a>Manage-bde wipefreespace

Cancella lo spazio disponibile nel volume, rimuovendo tutti i frammenti di dati che potrebbero esistere nello spazio. L'esecuzione di questo comando su un volume crittografato utilizzando il metodo di crittografia **solo spazio utilizzato** garantisce lo stesso livello di protezione del metodo di crittografia **completo del volume** di crittografia.

## <a name="syntax"></a>Sintassi

```
manage-bde -wipefreespace|-w [<drive>] [-cancel] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -Annulla | Annulla la cancellazione di spazio che è in corso. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per eliminare lo spazio disponibile sull'unità C, digitare: \

```
manage-bde -w C:
```

```
manage-bde -wipefreespace C:
```

Per annullare la cancellazione dello spazio disponibile sull'unità C, digitare:

```
manage-bde -w -cancel C:
```

```
manage-bde -wipefreespace -cancel C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
