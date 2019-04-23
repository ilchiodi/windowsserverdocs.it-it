---
title: set_2
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f5523646fddbfec31cb3900fc09230efc1c7813
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862612"
---
# <a name="set2"></a>set_2



Imposta il contesto, opzioni, la modalità dettagliata e file di metadati per la creazione di copie shadow. Se utilizzata senza parametri, **impostare** Elenca tutte le impostazioni correnti.

## <a name="syntax"></a>Sintassi

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>Set di comandi secondari

|Sottocomando|Descrizione|
|-----------|-----------|
|context|Imposta il contesto per la creazione di copie shadow. Visualizzare [impostare il contesto](set-context.md) per la sintassi e parametri.|
|Opzione|Imposta le opzioni per la creazione di copie shadow. Visualizzare [impostare l'opzione](set-option.md) per la sintassi e parametri.|
|verbose|Attiva o disattiva la modalità di output dettagliato. Visualizzare [impostare dettagliato](set-verbose.md) per la sintassi e parametri.|
|metadati|Imposta il nome e percorso del file di metadati creazione shadow. Visualizzare [impostare i metadati](set-metadata.md) per la sintassi e parametri.|
|/?|Visualizza la guida al prompt dei comandi.|

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)