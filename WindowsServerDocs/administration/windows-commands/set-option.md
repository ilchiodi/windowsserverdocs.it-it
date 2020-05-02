---
title: Opzione SET
description: Argomento di riferimento per l'opzione set, che consente di impostare le opzioni per la creazione di copie shadow.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4aba049e29cd74450467cf28057a2ff4e4a7094
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721896"
---
# <a name="set-option"></a>Opzione SET

Imposta le opzioni per la creazione di copie shadow. Se utilizzata senza parametri, **impostare l'opzione** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

### <a name="parameters"></a>Parametri

|     Parametro     |                                                                                                  Descrizione                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [differenziale   |                                                                                                     plessi                                                                                                     |
|  trasportabili  |                       Specifica che la copia shadow non deve essere importata. Il file CAB dei metadati può successivamente essere utilizzato per importare la copia shadow allo stesso o a un altro computer.                       |
| [rollbackrecover] |                     Segnala writer da utilizzare *salvataggio* durante il **PostSnapshot** evento. Questa operazione è utile se la copia shadow verrà utilizzata per il rollback, ad esempio con data mining.                      |
|   [txfrecover]    |                                                               Richieste per effettuare la copia shadow coerenti durante la creazione di VSS.                                                                |
|  [noautorecover]  | Gli autori si arresta e il file system di eseguire le modifiche di ripristino per la copia shadow in uno stato consistente. **Noautorecover** non può essere utilizzato con **txfrecover** o **rollbackrecover**. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)