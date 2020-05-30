---
title: Manage-bde (pacchetto)
description: Argomento di riferimento per il comando Manage-bde del pacchetto, che genera un pacchetto di chiavi per un'unità.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4d0956154d6b20d5ceedb44d0781614f8da5fb1
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222882"
---
# <a name="manage-bde-keypackage"></a>Manage-bde (pacchetto)

Genera un pacchetto di chiavi per un'unità. Il pacchetto della chiave è utilizzabile in combinazione con lo strumento di ripristino per ripristinare l'unità danneggiata.

## <a name="syntax"></a>Sintassi

```
manage-bde -keypackage [<drive>] [-ID <keyprotectoryID>] [-path <pathtoexternalkeydirectory>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -ID | Crea un pacchetto di chiavi usando la protezione con chiave con l'identificatore specificato da questo valore ID. **Suggerimento:** Usare il comando **Manage-bde – protectors – get** insieme alla lettera di unità per cui si vuole creare un pacchetto di chiavi per ottenere un elenco di GUID disponibili da usare come valore ID. |
| -percorso | Specifica il percorso in cui salvare il pacchetto di chiavi creato. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per creare un pacchetto di chiavi per l'unità C, in base alla protezione con chiave identificata dal GUID e salvare il pacchetto della chiave in F:\Folder, digitare:

```
manage-bde -keypackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
