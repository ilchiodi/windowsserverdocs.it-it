---
ms.assetid: f15c02d7-1cbd-4eba-a571-0ea34ab93ef4
title: Esecuzione della deduplicazione dati
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 86aff55b4c548ccf4fcbb04cc477dd63a889bebd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403210"
---
# <a name="running-data-deduplication"></a>Esecuzione della deduplicazione dati

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016

## <a id="running-dedup-jobs-manually"></a>Esecuzione manuale dei processi di deduplicazione dati

Ogni processo pianificato di Deduplicazione dati può essere eseguito manualmente con i cmdlet PowerShell seguenti:
* [`Start-DedupJob`](https://technet.microsoft.com/library/hh848442.aspx): Avvia un nuovo processo di deduplicazione dati
* [`Stop-DedupJob`](https://technet.microsoft.com/library/hh848439.aspx): Arresta un processo di deduplicazione dati già in corso (o lo rimuove dalla coda)
* [`Get-DedupJob`](https://technet.microsoft.com/library/hh848452.aspx): Mostra tutti i processi di deduplicazione dati attivi e in coda

Tutte le [impostazioni disponibili quando si pianifica un processo di deduplicazione dati](advanced-settings.md#modifying-job-schedules-available-settings) sono disponibili anche quando si avvia un processo manualmente, ad eccezione delle impostazioni di pianificazione specifiche. Ad esempio, per avviare manualmente un processo di [ottimizzazione](understand.md#job-info-optimization) con priorità elevata e utilizzo massimo della CPU e della memoria, eseguire il comando PowerShell seguente con privilegi di amministratore:

```PowerShell
Start-DedupJob -Type Optimization -Volume <Your-Volume-Here> -Memory 100 -Cores 100 -Priority High
```

## <a id="monitoring-dedup"></a>Monitoraggio della deduplicazione dei dati

### <a id="monitoring-dedup-job-successes"></a>Processi riusciti

Poiché la deduplicazione dati usa un modello di postelaborazione, è importante che i [processi di deduplicazione dati](understand.md#job-info) abbiano esito positivo. Un modo semplice per controllare lo stato del processo più recente consiste nell'usare il cmdlet PowerShell [`Get-DedupStatus`](https://technet.microsoft.com/library/hh848437.aspx). Controllare periodicamente i campi seguenti:

* Per il [processo di ottimizzazione](understand.md#job-info-optimization), esaminare `LastOptimizationResult` (0 = esito positivo), `LastOptimizationResultMessage` e `LastOptimizationTime` (dovrebbero essere recenti).
* Per il [processo di Garbage Collection](understand.md#job-info-gc), esaminare `LastGarbageCollectionResult` (0 = esito positivo), `LastGarbageCollectionResultMessage` e `LastGarbageCollectionTime` (dovrebbero essere recenti).
* Per il [processo di pulitura dell'integrità](understand.md#job-info-scrubbing), esaminare `LastScrubbingResult` (0 = esito positivo), `LastScrubbingResultMessage` e `LastScrubbingTime` (dovrebbero essere recenti).

> [!Note]  
> Altri dettagli sui processi con esiti positivi e negativi sono disponibili nel Visualizzatore eventi Windows in `\Applications and Services Logs\Windows\Deduplication\Operational`.

### <a id="monitoring-dedup-optimization-rates"></a>Frequenze di ottimizzazione

Uno degli indicatori dell'esito negativo del [processo di ottimizzazione](understand.md#job-info-optimization) è una frequenza di ottimizzazione tendente al basso, che può indicare che il processo di ottimizzazione non riesce a stare al passo con la frequenza delle modifiche o con la varianza. È possibile controllare la frequenza di ottimizzazione usando il cmdlet PowerShell [`Get-DedupStatus`](https://technet.microsoft.com/library/hh848437.aspx).

> [!Important]
> `Get-DedupStatus` include due campi rilevanti per la frequenza di ottimizzazione: `OptimizedFilesSavingsRate` e `SavingsRate`. Questi sono entrambi valori importanti di cui tenere traccia, ma ciascuno ha un significato univoco.
> - `OptimizedFilesSavingsRate` si applica solo ai file che sono "in-Policy" per l'ottimizzazione (`space used by optimized files after optimization / logical size of optimized files`).
> - `SavingsRate` si applica all'intero volume (`space used by optimized files after optimization / total logical size of the optimization`).

## <a id="disabling-dedup"></a>Disabilitazione della deduplicazione dati
Per disattivare Deduplicazione dati, eseguire il [processo di annullamento dell'ottimizzazione](understand.md#job-info-unoptimization). Per annullare l'ottimizzazione del volume, eseguire il comando seguente:

```PowerShell
Start-DedupJob -Type Unoptimization -Volume <Desired-Volume>
```

> [!Important]  
> Il processo di annullamento dell'ottimizzazione avrà esito negativo se il volume non dispone di spazio sufficiente per contenere i dati non ottimizzati.

## <a id="faq"></a>Domande frequenti
**È disponibile un Management Pack System Center Operations Manager per il monitoraggio della deduplicazione dei dati?**  
Sì. Deduplicazione dati può essere monitorata tramite System Center Management Pack per il file server. Per altre informazioni, vedere il documento [Guide for System Center Management Pack for File Server 2012 R2](https://download.microsoft.com/download/6/F/7/6F7A33B9-9383-48ED-9252-23C2C8AD1BDA/MPGuide_FileServer2012R2.doc) (Guida di System Center Management Pack per File server 2012 R2).
