---
title: Considerazioni sulla creazione di moduli di PowerShell
description: Considerazioni sulla creazione di moduli di PowerShell
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: jasonsh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 25b202e56286b7c26c3150642a656eb31a120808
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851934"
---
# <a name="powershell-module-authoring-considerations"></a>Considerazioni sulla creazione di moduli di PowerShell

Questo documento include alcune linee guida relative alla modalità di creazione di un modulo per ottenere prestazioni ottimali.

## <a name="module-manifest-authoring"></a>Creazione del manifesto del modulo

Un manifesto del modulo che non usa le linee guida seguenti può avere un notevole effetto sulle prestazioni generali di PowerShell anche se il modulo non viene usato in una sessione.

Il rilevamento automatico dei comandi analizza ogni modulo per individuare i comandi esportati dal modulo e questa analisi può essere costosa.
I risultati dell'analisi dei moduli vengono memorizzati nella cache per utente, ma la cache non è disponibile alla prima esecuzione, che è uno scenario tipico con i contenitori.
Durante l'analisi del modulo, se i comandi esportati possono essere completamente determinati dal manifesto, è possibile evitare un'analisi più costosa del modulo.

### <a name="guidelines"></a>Linee guida

* Nel manifesto del modulo, non usare caratteri jolly nelle voci `AliasesToExport`, `CmdletsToExport`e `FunctionsToExport`.

* Se il modulo non Esporta comandi di un determinato tipo, specificarlo in modo esplicito nel manifesto specificando `@()`.
Una voce mancante o `$null` equivale a specificare il carattere jolly `*`.

Quando possibile, è consigliabile evitare quanto segue:

```PowerShell
@{
    FunctionsToExport = '*'

    # Also avoid omitting an entry, it is equivalent to using a wildcard
    # CmdletsToExport = '*'
    # AliasesToExport = '*'
}
```

Usare invece:

```PowerShell
@{
    FunctionsToExport = 'Format-Hex', 'Format-Octal'
    CmdletsToExport = @()  # Specify an empty array, not $null
    AliasesToExport = @()  # Also ensure all three entries are present
}
```

## <a name="avoid-cdxml"></a>Evitare CDXML

Quando si decide come implementare il modulo, sono disponibili tre opzioni principali:

* Binario (in C#genere)
* Script (PowerShell)
* CDXML (codice CIM per il wrapping di file XML)

Se la velocità di caricamento del modulo è importante, CDXML è approssimativamente un ordine di grandezza più lento rispetto a un modulo binario.

Un modulo binario carica il più velocemente perché viene compilato in anticipo e può usare NGen per compilare JIT una volta per ogni computer.

Un modulo di script in genere carica un po' più lentamente rispetto a un modulo binario perché PowerShell deve analizzare lo script prima di compilarlo ed eseguirlo.

Un modulo CDXML è in genere molto più lento rispetto a un modulo di script perché deve prima analizzare un file XML che quindi genera un po' di script di PowerShell che viene quindi analizzato e compilato.

