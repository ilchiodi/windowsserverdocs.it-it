---
title: Opzione set
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b9174f219654e99eb9441abe3342c31b5089ef5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384055"
---
# <a name="set-option"></a>Opzione set



Imposta le opzioni per la creazione di copie shadow. Se utilizzata senza parametri, **impostare l'opzione** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>Parametri

|     Parametro     |                                                                                                  Descrizione                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [differenziale   |                                                                                                     plessi                                                                                                     |
|  trasportabili  |                       Specifica che la copia shadow non deve essere importata. Il file CAB dei metadati può successivamente essere utilizzato per importare la copia shadow allo stesso o a un altro computer.                       |
| [rollbackrecover] |                     Segnala writer da utilizzare *salvataggio* durante il **PostSnapshot** evento. Questa operazione è utile se la copia shadow verrà utilizzata per il rollback, ad esempio con data mining.                      |
|   [txfrecover]    |                                                               Richieste per effettuare la copia shadow coerenti durante la creazione di VSS.                                                                |
|  [noautorecover]  | Gli autori si arresta e il file system di eseguire le modifiche di ripristino per la copia shadow in uno stato consistente. **Noautorecover** non può essere utilizzato con **txfrecover** o **rollbackrecover**. |

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)