---
title: Comprendere e vedere risincronizzazione archiviazione
description: Informazioni dettagliate su quando viene eseguita la risincronizzazione dell'archiviazione e su come visualizzarla in Windows Server 2019.
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 1209a3e08922a380ef46d3be6d28ce489b748f22
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820934"
---
# <a name="understand-and-monitor-storage-resync"></a>Comprendere e monitorare risincronizzazione di archiviazione

>Si applica a: Windows Server 2019

Gli avvisi di risincronizzazione dell'archiviazione sono una nuova funzionalità di [spazi di archiviazione diretta](storage-spaces-direct-overview.md) in Windows Server 2019 che consente all'servizio integrità di generare un errore durante la risincronizzazione dell'archiviazione. L'avviso è utile per notificare quando è in corso la risincronizzazione, in modo che non vengano accidentalmente rilevati più server, il che potrebbe causare l'indisponibilità di più domini di errore, causando il rallentamento del cluster. 

Questo argomento fornisce informazioni generali e procedure per comprendere e vedere risincronizzazione dell'archiviazione in un cluster di failover di Windows Server con Spazi di archiviazione diretta.

## <a name="understanding-resync"></a>Informazioni sulla risincronizzazione

Iniziamo con un semplice esempio per comprendere il modo in cui l'archiviazione non viene sincronizzata. Tenere presente che la soluzione di archiviazione distribuita (solo unità locale) non è in grado di mostrare questo comportamento. Come si vedrà di seguito, se un nodo del server diventa inattivo, le relative unità non verranno aggiornate fino a quando non tornerà online. questa condizione è valida per qualsiasi architettura iperconvergente. 

Si supponga di voler archiviare la stringa "HELLo". 

![ASCII della stringa "Hello"](media/understand-storage-resync/hello.png)

Asssuming che abbiamo una resilienza del mirror a tre vie, abbiamo tre copie di questa stringa. A questo punto, se si tiene temporaneamente il server #1 (per maintanence), non è possibile accedere a Copy #1.

![Non è possibile accedere alla copia #1](media/understand-storage-resync/copy1.png)

Si supponga di aggiornare la stringa da "HELLo" a "HELP!" al momento.

![ASCII della stringa "Help!"](media/understand-storage-resync/help.png)

Una volta aggiornata la stringa, copia #2 e #3 verranno aggiornati correttamente. Tuttavia, la copia #1 non è ancora accessibile perché il server #1 è temporaneamente inattivo (per maintanence). 

![Gif di scrittura per copiare #2 e #2 "](media/understand-storage-resync/write.gif)

A questo punto sono presenti #1 di copia con dati non sincronizzati. Il sistema operativo usa il rilevamento granulare dell'area sporca per tenere traccia dei bit che non sono sincronizzati. In questo modo, quando il server #1 torna online, è possibile sincronizzare le modifiche leggendo i dati da copia #2 o #3 e sovrascrivendo i dati in copia #1. I vantaggi di questo approccio sono la necessità di copiare solo i dati non aggiornati, anziché risincronizzare tutti i dati dal server #2 o dal #3 server.

![Gif di sovrascrittura per la copia #1 "](media/understand-storage-resync/overwrite.gif)

Quindi, spiega in che modo i dati non vengono sincronizzati. Ma che cosa è simile a un livello elevato? Si supponga per questo esempio che si disponga di un cluster iperconvergente con tre server. Quando il #1 server è in fase di manutenzione, sarà possibile visualizzarlo come inattivo. Quando si riportano i #1 server, viene avviata la risincronizzazione di tutte le relative archiviazioni utilizzando il rilevamento granulare dell'area dirty (descritto in precedenza). Una volta sincronizzati tutti i dati, tutti i server verranno visualizzati come in alto.

![Gif della visualizzazione amministratore della risincronizzazione "](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>Come monitorare la risincronizzazione dell'archiviazione in Windows Server 2019

Ora che si è appreso come funziona la risincronizzazione dell'archiviazione, esaminiamo come viene visualizzata in Windows Server 2019. È stato aggiunto un nuovo errore all' [servizio integrità](../../failover-clustering/health-service-overview.md) che verrà visualizzato quando lo spazio di archiviazione viene risincronizzato.

Per visualizzare questo errore in PowerShell, eseguire:

``` PowerShell
Get-HealthFault
```

Si tratta di un nuovo errore in Windows Server 2019, che verrà visualizzato in PowerShell, nel report di convalida del cluster e in qualsiasi altra posizione in cui si basano sugli errori di integrità. 

Per ottenere una visualizzazione più approfondita, è possibile eseguire una query sul database di serie temporali in PowerShell nel modo seguente:

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

In particolare, l'interfaccia di amministrazione di Windows usa errori di integrità per impostare lo stato e il colore dei nodi del cluster. Quindi, questo nuovo errore provocherà la transizione dei nodi del cluster dal rosso (in basso) al giallo (risincronizzazione) in verde (verso l'alto), anziché andare direttamente da rosso a verde, nel dashboard di HCI.

![Immagine della visualizzazione della risincronizzazione 2016 rispetto a 2019](media/understand-storage-resync/compare.png)

Mostrando lo stato di avanzamento della risincronizzazione dello spazio di archiviazione, è possibile sapere con precisione la quantità di dati non sincronizzati e se il sistema sta procedendo in avanti. Quando si apre l'interfaccia di amministrazione di Windows e si passa al *Dashboard*, il nuovo avviso viene visualizzato come segue:

![Immagine dell'avviso nell'interfaccia di amministrazione di Windows "](media/understand-storage-resync/alert.png)

L'avviso è utile per notificare quando è in corso la risincronizzazione, in modo che non vengano accidentalmente rilevati più server, il che potrebbe causare l'indisponibilità di più domini di errore, causando il rallentamento del cluster. 

Se si passa alla pagina *Server* nell'interfaccia di amministrazione di Windows, fare clic su *inventario*e quindi scegliere un server specifico, è possibile ottenere una visualizzazione più dettagliata del modo in cui viene eseguita la risincronizzazione dell'archiviazione in base al server. Se si passa al server e si osserva il grafico di *archiviazione* , verrà visualizzata la quantità di dati che deve essere ripristinata in una riga *viola* con il numero esatto sopra riportato. Questa quantità aumenterà quando il server è inattivo (è necessario risincronizzare più dati) e diminuisce gradualmente quando il server torna in linea (i dati vengono sincronizzati). Quando la quantità di dati che deve essere ripristinata è 0, lo spazio di archiviazione viene risincronizzato. è ora possibile ridurre il server, se necessario. Di seguito è riportata una schermata di questa esperienza nell'interfaccia di amministrazione di Windows:

![Immagine della visualizzazione server nell'interfaccia di amministrazione di Windows "](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>Come vedere risincronizzazione dell'archiviazione in Windows Server 2016

Come si può notare, questo avviso è particolarmente utile per ottenere una visualizzazione olistica degli elementi che si verificano a livello di archiviazione. Vengono riepilogate in modo efficace le informazioni che è possibile ottenere dal cmdlet Get-StorageJob, che restituisce informazioni sui processi del modulo di archiviazione con esecuzione prolungata, ad esempio un'operazione di ripristino in uno spazio di archiviazione. Un esempio è illustrato di seguito:

```PowerShell
Get-StorageJob
```

Di seguito è riportato un esempio di output:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Questa visualizzazione è molto più granulare, poiché i processi di archiviazione elencati sono per volume, è possibile visualizzare l'elenco dei processi in esecuzione ed è possibile tenere traccia dello stato di avanzamento individuale. Questo cmdlet funziona in Windows Server 2016 e 2019.

## <a name="see-also"></a>Vedere anche

- [Disconnessione del server a scopo di manutenzione](maintain-servers.md)
- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)