---
title: Comprendere e vedere risincronizzazione di archiviazione
description: Informazioni dettagliate su come trovarla in Windows Server 2019 e quando si verifica risincronizzazione di archiviazione.
keywords: Spazi di archiviazione diretta, risincronizzazione di archiviazione, risincronizzare, archiviazione, S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 748eccd10fc0c3c962d6e64ff6ead08017ac1947
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2019
ms.locfileid: "9009670"
---
# Comprendere e monitorare risincronizzazione di archiviazione

>Si applica a: Windows Server 2019

Gli avvisi di risincronizzazione archiviazione sono una nuova funzionalità di [Spazi di archiviazione diretta](storage-spaces-direct-overview.md) in Windows Server 2019 che consente al servizio di integrità di generare un errore quando il dispositivo di archiviazione è resyncing. L'avviso è utile per informare l'utente quando viene eseguito risincronizzazione, in modo da non accidentalmente più server verso il basso (che potrebbe causare più domini di errore a essere interessati, risultante nel cluster passando verso il basso). 

In questo argomento vengono fornite in background e istruzioni per comprendere e Vedi risincronizzazione di archiviazione in un cluster di failover di Windows Server con spazi di archiviazione diretta.

## Risincronizzazione conoscenza

Iniziamo con un semplice esempio per comprendere come archiviazione Ottiene sincronizzata. Tieni presente che qualsiasi condivisione (solo per l'unità locali) soluzione di archiviazione distribuite Mostra questo comportamento. Come illustrato di seguito, se un nodo server diventa non disponibile, quindi le relative unità non essere aggiornati fino a quando non ritorna online - ciò vale per qualsiasi architettura iper-convergenti. 

Si supponga che si desidera memorizzare la stringa "HELLO". 

![ASCII della stringa "hello"](media/understand-storage-resync/hello.png)

Asssuming che abbiamo la resilienza con mirroring a tre vie, abbiamo tre copie di questa stringa. A questo punto, se si disattiverà server #1 temporaneamente (per la manutenzione), quindi non è possibile accedere copia #1.

![Non può accedere copia #1](media/understand-storage-resync/copy1.png)

Si supponga che aggiorniamo la nostra stringa da "HELLO" a "Guida!" In questo momento.

![ASCII della stringa "Guida!"](media/understand-storage-resync/help.png)

Una volta che aggiorniamo la stringa, copia #2 e 3 # venga aggiornato correttamente. Tuttavia, copia #1 ancora non sono accessibili perché server #1 è temporaneamente inattivo (per la manutenzione). 

![GIF di scrittura copiare #2 e #2 "](media/understand-storage-resync/write.gif)

A questo punto, abbiamo copia #1 con i dati che non è sincronizzato. Il sistema operativo Usa granulare area dirty monitoraggio per tenere traccia ai bit che non sono sincronizzati. In questo modo quando #1 server ritorna online, abbiamo possiamo sincronizzare le modifiche, la lettura dei dati da copia #2 o 3 # e sovrascrivere i dati nella copia #1. Il vantaggio di questo approccio è che è solo necessario copiare i dati non aggiornati, invece di resyncing tutti i dati dal server #2 o server #3.

![GIF di sovrascrivere copiare #1 "](media/understand-storage-resync/overwrite.gif)

Pertanto, questo spiega come dati ottiene sincronizzati. Ma cosa viene indicato a livello generale? Si supponga per questo esempio di disporre di un cluster iper-convergenti tre server. Quando il server #1 è in modalità manutenzione, verrà visualizzata come verso il basso. Quando si riattiva server #1 verso l'alto, inizierà resyncing tutta l'archiviazione con la registrazione granulare area dirty (illustrata sopra). Una volta nuovamente sincronizzati i dati, verranno visualizzati tutti i server come disponibile.

![GIF di visualizzazione di amministratore di risincronizzazione"](media/understand-storage-resync/admin.gif)

## Come monitorare risincronizzazione di archiviazione in Windows Server 2019

Ora che hai identificato come funziona la risincronizzazione di archiviazione, diamo un'occhiata a come questa viene visualizzata in Windows Server 2019. Abbiamo aggiunto un errore di nuovo al [Servizio di integrità](../../failover-clustering/health-service-overview.md) che verrà visualizzato quando il dispositivo di archiviazione è resyncing.

Per visualizzare questo errore in PowerShell, eseguire:

``` PowerShell
Get-HealthFault
```

Questo è un errore di nuovo in Windows Server 2019 e verrà visualizzato in PowerShell, nel report, la convalida del cluster e qualsiasi altro che si basa su errori di integrità. 

Per ottenere una visualizzazione più approfondita, eseguire query nel database di serie di tempo in PowerShell come indicato di seguito:

```PowerShell
Get-ClusterNode | Get-ClusterPerf -ClusterNodeSeriesName ClusterNode.Storage.Degraded
```
Ecco un output di esempio:

```
Object Description: ClusterNode Server1

Series                       Time                Value Unit
------                       ----                ----- ----
ClusterNode.Storage.Degraded 01/11/2019 16:26:48     214 GB
```

In particolare, Windows Admin Center utilizza gli errori di integrità per impostare lo stato e il colore dei nodi del cluster. Pertanto, questo errore nuovo causerà nodi del cluster per la transizione da rosso (giù) a giallo (resyncing) in verde (su), invece di essere disponibile direttamente dal rosso, verde nel Dashboard di HCI.

![Immagine della visualizzazione di Visual Studio 2016 2019 di risincronizzazione"](media/understand-storage-resync/compare.png)

Visualizzando lo stato di risincronizzazione complessivo di archiviazione, è possibile conoscere in modo accurato la quantità di dati è sincronizzato e indica se il sistema sta effettuando l'avanzamento. Quando si apre Windows Admin Center e passa al *Dashboard*, vedrai il nuovo avviso come indicato di seguito:

![Immagine dell'avviso in Windows Admin Center"](media/understand-storage-resync/alert.png)

L'avviso è utile per informare l'utente quando viene eseguito risincronizzazione, in modo da non accidentalmente più server verso il basso (che potrebbe causare più domini di errore a essere interessati, risultante nel cluster passando verso il basso). 

Se si passa alla pagina *server* in Windows Admin Center, fai clic su *inventario*e quindi sceglie uno specifico server, è possibile ottenere una visualizzazione più dettagliata di aspetto questo risincronizzazione dell'archiviazione su una base per ogni server. Se si passa al server e visualizzare il grafico di *archiviazione* , vedrai la quantità di dati che devono essere ripristinato in una linea *viola* con numero esatto immediatamente di sopra. Questa quantità verrà aumentare quando il server è verso il basso (più dati deve essere resynced) e ridurre gradualmente quando il server ritorna online (sono da sincronizzare i dati). Quando la quantità di dati che deve essere ripristino è 0, il dispositivo di archiviazione è eseguita resyncing - sei libero di eseguire un server verso il basso, se è necessario. Screenshot di questa esperienza in Windows Admin Center è illustrato di seguito:

![Immagine della visualizzazione di server in Windows Admin Center"](media/understand-storage-resync/server.png)

## Come visualizzare risincronizzazione di archiviazione in Windows Server 2016

Come puoi vedere, l'avviso è particolarmente utile per ottenere una visione di che cosa accade a livello di archiviazione. In realtà sono riepilogate le informazioni che è possibile ottenere dal cmdlet Get-StorageJob, che restituisce informazioni sui processi modulo di archiviazione a esecuzione prolungata, ad esempio un'operazione di ripristino in uno spazio di archiviazione. Di seguito è riportato un esempio:

```PowerShell
Get-StorageJob
```

Ecco l'output di esempio:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Questa visualizzazione è molto più granulare dato che sono elencati i processi di archiviazione per ogni volume, puoi vedere l'elenco dei processi in esecuzione e puoi tenere traccia dello stato di avanzamento singoli. Questo cmdlet funziona in Windows Server 2016 sia 2019.

## Vedi anche

- [Disconnessione del server a scopo di manutenzione](maintain-servers.md)
- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)