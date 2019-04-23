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
ms.openlocfilehash: 81678768bb2b5fcfd7f2f2d067562e78e93dc549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848852"
---
# <a name="set-option"></a>Opzione set



Imposta le opzioni per la creazione di copie shadow. Se utilizzata senza parametri, **impostare l'opzione** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[backup differenziali | plex]|Specifica il tipo di copia shadow per creare per il provider.|
|[trasportabili]|Specifica che la copia shadow non deve essere importata. Il file CAB dei metadati può successivamente essere utilizzato per importare la copia shadow allo stesso o a un altro computer.|
|[rollbackrecover]|Segnala writer da utilizzare *salvataggio* durante il **PostSnapshot** evento. Ciò è utile se la copia shadow verrà utilizzata per eseguire il rollback (ad esempio, con il data mining).|
|[txfrecover]|Richieste per effettuare la copia shadow coerenti durante la creazione di VSS.|
|[noautorecover]|Gli autori si arresta e il file system di eseguire le modifiche di ripristino per la copia shadow in uno stato consistente. **Noautorecover** non può essere utilizzato con **txfrecover** o **rollbackrecover**.|

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)