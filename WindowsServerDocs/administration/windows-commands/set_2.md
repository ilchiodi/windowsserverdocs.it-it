---
title: set_2
description: Argomento di riferimento per set_2, che imposta il contesto, le opzioni, la modalità dettagliata e il file di metadati per la creazione di copie shadow.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 655f379dd8c2d633aad0cbb470b17c6ccb90c4f7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721875"
---
# <a name="set_2"></a>set_2

Imposta il contesto, le opzioni, la modalità dettagliata e il file di metadati per la creazione della copia shadow. Se utilizzata senza parametri, **impostare** elenca tutte le impostazioni correnti.

## <a name="syntax"></a>Sintassi

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>Imposta sottocomandi

|Sottocomando|Descrizione|
|-----------|-----------|
|contesto|Imposta il contesto per la creazione di copie shadow. Vedere [impostare il contesto](set-context.md) per la sintassi e i parametri.|
|Opzione|Imposta le opzioni per la creazione della copia shadow. Per la sintassi e i parametri, vedere [set Option](set-option.md) .|
|verbose|Attiva o disattiva la modalità di output dettagliata. Vedere [set verbose](set-verbose.md) per la sintassi e i parametri.|
|metadata|Imposta il nome e il percorso del file di metadati per la creazione di Shadow. Vedere [impostare i metadati](set-metadata.md) per la sintassi e i parametri.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)