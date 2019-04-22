---
title: Considerazioni di creazione modulo di PowerShell
description: Considerazioni di creazione modulo di PowerShell
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 37dd860019b91daf70947dba93d20274048487a0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818722"
---
# <a name="powershell-module-authoring-considerations"></a>Considerazioni di creazione modulo di PowerShell

Questo documento include alcune linee guida relative al modo in cui viene creato un modulo per ottenere prestazioni ottimali.

## <a name="module-manifest-authoring"></a>Creazione del manifesto di modulo

Un manifesto del modulo che non usa le seguenti linee guida può avere un impatto significativo sulle prestazioni generali di PowerShell, anche se non viene usato il modulo in una sessione.

Comando auto-discovery analizza ogni modulo per determinare i comandi esportati dal modulo e l'analisi può essere costosa.
I risultati dell'analisi del modulo vengono memorizzati nella cache per ogni utente, ma la cache non è disponibile alla prima esecuzione, ossia uno scenario tipico con i contenitori.
Durante l'analisi di modulo, se possono determinare completamente i comandi esportati dal manifesto di analisi più costoso del modulo possono essere evitato.

### <a name="guidelines"></a>Linee guida

* Nel manifesto del modulo, non usare caratteri jolly nel `AliasesToExport`, `CmdletsToExport`, e `FunctionsToExport` voci.

* Se il modulo non Esporta i comandi di un tipo specifico, specificare in modo esplicito nel manifesto specificando `@()`.
Manca un oppure `$null` voce equivale a specificare il carattere jolly `*`.

Di seguito debba essere evitati laddove possibile:

```PowerShell
@{
    FunctionsToExport = '*'

    # Also avoid omitting an entry, it is equivalent to using a wildcard
    # CmdletsToExport = '*'
    # AliasesToExport = '*'
}
```

In alternativa, usare:

```PowerShell
@{
    FunctionsToExport = 'Format-Hex', 'Format-Octal'
    CmdletsToExport = @()  # Specify an empty array, not $null
    AliasesToExport = @()  # Also ensure all three entries are present
}
```

## <a name="avoid-cdxml"></a>Evitare CDXML

Prima di decidere come implementare un modulo, esistono tre opzioni principali:

* Binario (in genere C#)
* Script (PowerShell)
* CDXML (un file XML CIM di wrapping)

Se la velocità di caricamento di un modulo è importante, CDXML è più o meno un decisamente più lento rispetto a un modulo binario.

Un modulo binario consente di caricare più velocemente perché viene compilata anticipatamente e possono usare NGen per la compilazione JIT una sola volta per ogni macchina.

Un modulo di script viene caricato un po' più lento rispetto a un modulo binario in genere poiché PowerShell è necessario analizzare lo script prima di compilare ed eseguirla.

Un modulo CDXML è in genere molto più lento rispetto a un modulo di script perché innanzitutto necessario analizzare un file XML che viene quindi generato gran parte dello script di PowerShell che viene quindi analizzato e compilato.

