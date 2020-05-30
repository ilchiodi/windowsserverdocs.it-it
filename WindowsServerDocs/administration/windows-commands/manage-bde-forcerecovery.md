---
title: Manage-bde forcerecovery
description: Argomento di riferimento per il comando Manage-bde forcerecovery, che forza un'unità protetta da BitLocker in modalità di ripristino al riavvio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eecae37c-c9a3-46c5-b615-a0ace1f1d778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e152a18137925bd197e672531ccef47e7b2a7119
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222895"
---
# <a name="manage-bde-forcerecovery"></a>Manage-bde forcerecovery

Forza un'unità protetta da BitLocker in modalità di ripristino al riavvio. Questo comando Elimina tutte le protezioni con chiave correlati al TPM di Trusted Platform Module dall'unità. Al riavvio del computer, solo una password o una chiave di ripristino può essere utilizzata per sbloccare l'unità.

## <a name="syntax"></a>Sintassi

```
manage-bde –forcerecovery <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Per far sì che BitLocker venga avviato in modalità di ripristino sull'unità C, digitare:

```
manage-bde –forcerecovery C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
