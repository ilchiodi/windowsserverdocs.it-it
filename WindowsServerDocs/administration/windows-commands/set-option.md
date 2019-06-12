---
title: Opzione set
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1c4756627d19d296d02fa11ac67ef80080ddf318
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441363"
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
|   [backup differenziali   |                                                                                                     plex]                                                                                                     |
|  [trasportabili]  |                       Specifica che la copia shadow non deve essere importata. Il file CAB dei metadati può successivamente essere utilizzato per importare la copia shadow allo stesso o a un altro computer.                       |
| [rollbackrecover] |                     Segnala writer da utilizzare *salvataggio* durante il **PostSnapshot** evento. Ciò è utile se la copia shadow verrà utilizzata per eseguire il rollback (ad esempio, con il data mining).                      |
|   [txfrecover]    |                                                               Richieste per effettuare la copia shadow coerenti durante la creazione di VSS.                                                                |
|  [noautorecover]  | Gli autori si arresta e il file system di eseguire le modifiche di ripristino per la copia shadow in uno stato consistente. **Noautorecover** non può essere utilizzato con **txfrecover** o **rollbackrecover**. |

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)