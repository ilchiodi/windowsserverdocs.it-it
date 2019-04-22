---
title: Comprendere e vedere risincronizzazione di archiviazione
description: Informazioni dettagliate su quando accade risincronizzazione di archiviazione e come visualizzarli in Windows Server 2019.
keywords: Spazi di archiviazione diretta, la risincronizzazione di archiviazione, risincronizzare, l'archiviazione S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813462"
---
# <a name="understand-and-monitor-storage-resync"></a>Comprendere e monitorare risincronizzazione di archiviazione

>Si applica a: Windows Server 2019

Gli avvisi di risincronizzazione di archiviazione sono una nuova funzionalità di [spazi di archiviazione diretta](storage-spaces-direct-overview.md) in Windows Server 2019 che consente al servizio di integrità generare un errore quando lo spazio di archiviazione è risincronizzazione. L'avviso è utile per informare l'utente quando è in corso la risincronizzazione, in modo che non portano accidentalmente più server verso il basso (che potrebbe causare più domini di errore possono essere interessate, causando l'indisponibilità del cluster). 

In questo argomento fornisce informazioni generali e procedure per comprendere e vedere risincronizzazione di archiviazione in un cluster di failover Windows Server con spazi di archiviazione diretta.

## <a name="understanding-resync"></a>Risincronizzazione conoscenza

Si inizierà con un esempio semplice per comprendere come archiviazione Ottiene sincronizzata. Tenere presente che qualsiasi "shared-nothing" (solo per l'unità locali) la soluzione di archiviazione distribuita presenta questo comportamento. Come si vedrà di seguito, se un nodo del server diventa inattivo, quindi relative unità non verrà aggiornato finché non torna in linea, questo vale per qualsiasi architettura iperconvergente. 

Si supponga che si desidera archiviare la stringa "HELLO". 

![ASCII di stringa "hello"](media/understand-storage-resync/hello.png)

Asssuming che abbiamo la resilienza mirror a tre vie, sono disponibili tre copie di questa stringa. A questo punto, se prendiamo server #1 verso il basso temporaneamente (per la manutenzione), è possibile accedere copia #1.

![Non è possibile accedere copia #1](media/understand-storage-resync/copy1.png)

Si supponga che si aggiorna la stringa da "HELLO" a "HELP". In questo momento.

![ASCII di stringa "help".](media/understand-storage-resync/help.png)

Una volta che viene aggiornato la stringa, copia #2 e #3 sarà aggiornata è stata completata. Tuttavia, copia #1 ancora non è accessibile perché server #1 è temporaneamente non disponibile (per la manutenzione). 

![GIF della scrittura copiare #2 e #2 "](media/understand-storage-resync/write.gif)

A questo punto, abbiamo copia #1 che contiene dati che non sono sincronizzati. Il sistema operativo Usa granulare area dirty di rilevamento per tenere traccia di bit che non sono sincronizzati. In questo modo quando ritorna in linea server #1, è possibile sincronizzare le modifiche leggendo i dati dalla copia #2 o 3 # e sovrascrivere i dati nella copia #1. I vantaggi di questo approccio sono che è sufficiente copiare i dati obsoleti, anziché tutti i dati dal server #2 o 3 # risincronizzazione.

![GIF di sovrascrittura per copiare #1 "](media/understand-storage-resync/overwrite.gif)

Pertanto, questo spiega come dati ottiene sincronizzati. Ma cosa come funziona a livello generale? Si presuppone per questo esempio che si dispone di un cluster iperconvergente di tre server. Quando è in manutenzione server #1, questa verrà visualizzata come inattivo. Quando verrà distribuito nuovamente server #1, avvia la risincronizzazione tutta l'archiviazione tramite il monitoraggio granulare area dirty (illustrato in precedenza). Quando i dati sono tutte sincronizzati, verranno visualizzati tutti i server come attiva.

![GIF della visualizzazione amministrazione della risincronizzazione"](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>Come monitorare la risincronizzazione di archiviazione in Windows Server 2019

A questo punto comprendere il funzionamento di risincronizzazione di archiviazione, è possibile esaminare come questo è evidente in Windows Server 2019. È stato aggiunto un nuovo errore per il [servizio integrità](../../failover-clustering/health-service-overview.md) che verrà visualizzato quando lo spazio di archiviazione è risincronizzazione.

Per visualizzare questo errore in PowerShell, eseguire:

``` PowerShell
Get-HealthFault
```

Questo è un nuovo errore in Windows Server 2019 e verrà visualizzato in PowerShell, nel report di convalida cluster e in qualsiasi altro che sfrutta gli errori di integrità. 

Per ottenere una visione più approfondita, è possibile eseguire una query di database di serie temporali in PowerShell come indicato di seguito:

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

In particolare, Windows Admin Center Usa gli errori di integrità per impostare lo stato e il colore dei nodi del cluster. Pertanto, questo nuovo errore causerà nodi del cluster per eseguire la transizione da rosso (in basso) (risincronizzazione) in verde (up), invece di passare direttamente da rosso a verde, giallo sul Dashboard uomo.

![Immagine della visualizzazione in Visual Studio 2016 2019 di risincronizzazione"](media/understand-storage-resync/compare.png)

Mostrando lo stato di risincronizzazione di archiviazione complessivo, è possibile conoscere con precisione la quantità di dati non è sincronizzato e indica se il sistema è progressi. Quando si apre Windows Admin Center e Vai al *Dashboard*, verrà visualizzato il nuovo avviso come indicato di seguito:

![Immagine dell'avviso nella Windows Admin Center "](media/understand-storage-resync/alert.png)

L'avviso è utile per informare l'utente quando è in corso la risincronizzazione, in modo che non portano accidentalmente più server verso il basso (che potrebbe causare più domini di errore possono essere interessate, causando l'indisponibilità del cluster). 

Se si passa al *server* pagina Windows Admin Center, fare clic su *inventario*, quindi scegliere un server specifico, è possibile ottenere una visualizzazione più dettagliata dell'aspetto di questa risincronizzazione di archiviazione in base al server. Se si passa al server e osservare il *Storage* grafico, si noterà la quantità di dati che devono essere ripristinato un *viola* riga con numero esatto immediatamente sopra. Questa quantità aumenterà quando il server è inattivo (ulteriori dati dovrà essere resynced) e diminuisce gradualmente man mano quando il server torna online (i dati da sincronizzare). Quando la quantità di dati che deve essere la correzione è 0, lo spazio di archiviazione viene eseguita una risincronizzazione - questo punto si è liberi di disconnettere un server verso il basso, se è necessario. Di seguito è riportato uno screenshot di questa esperienza in Windows Admin Center:

![Immagine della visualizzazione di server in Windows Admin Center "](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>Come visualizzare la risincronizzazione di archiviazione in Windows Server 2016

Come si può notare, questo avviso è particolarmente utile per ottenere una visione olistica delle operazioni eseguite a livello di archiviazione. In modo efficace che riepilogano le informazioni che è possibile ottenere dal cmdlet Get-StorageJob, che restituisce informazioni sui processi del modulo Storage a esecuzione prolungata, ad esempio un'operazione di ripristino in uno spazio di archiviazione. Di seguito è riportato un esempio:

```PowerShell
Get-StorageJob
```

Di seguito è riportato l'output:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

In questa vista è molto più granulare poiché sono elencati i processi di archiviazione per ogni volume, è possibile visualizzare l'elenco di processi in esecuzione ed è possibile monitorare lo stato di avanzamento singoli. Questo cmdlet funziona in Windows Server 2016 sia 2019.

## <a name="see-also"></a>Vedere anche

- [Portare un server in modalità offline per manutenzione](maintain-servers.md)
- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)