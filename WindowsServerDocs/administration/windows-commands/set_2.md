---
title: set_2
description: Argomento dei comandi di Windows per set_2, che imposta il contesto, le opzioni, la modalità dettagliata e il file di metadati per la creazione di copie shadow.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa467625997824a11b2303572a063d591f59bdd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834384"
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
|context|Imposta il contesto per la creazione di copie shadow. Vedere [impostare il contesto](set-context.md) per la sintassi e i parametri.|
|opzione|Imposta le opzioni per la creazione della copia shadow. Per la sintassi e i parametri, vedere [set Option](set-option.md) .|
|verbose|Attiva o disattiva la modalità di output dettagliata. Vedere [set verbose](set-verbose.md) per la sintassi e i parametri.|
|metadata|Imposta il nome e il percorso del file di metadati per la creazione di Shadow. Vedere [impostare i metadati](set-metadata.md) per la sintassi e i parametri.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)