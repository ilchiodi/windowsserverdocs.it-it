---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: Panoramica di bilanciamento carico di macchina virtuale
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 0a106db25d476088898b914481e6041f20ce2e9e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-overview"></a>Panoramica di bilanciamento carico di macchina virtuale

> Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È importante considerare per le distribuzioni di cloud privato il spese di investimento (<abbr title="spese in conto capitale">Investimenti</abbr>) è necessario nell'ambiente di produzione. È molto comune per aggiungere la ridondanza per le distribuzioni di cloud privato per evitare di capacità insufficiente durante picco di traffico nell'ambiente di produzione, ma questa soluzione aumenta la <abbr title="spese in conto capitale">Investimenti</abbr>. La necessità per la ridondanza dipende da non bilanciato cloud privati in cui alcuni nodi ospitano più macchine virtuali (<abbr title="macchine virtuali">Le macchine virtuali</abbr>) e altri sono sottoutilizzati (ad esempio un server appena riavviato).

## <a id="what-is-vm-load-balancing"></a>Che cos'è il bilanciamento del carico macchina virtuale?
<abbr title="Macchina virtuale">VM</abbr> il bilanciamento del carico è una nuova funzionalità inclusa in Windows Server 2016 che consente di ottimizzare l'utilizzo di nodi in un Cluster di Failover. Identifica i nodi overcommii e distribuisce nuovamente <abbr title="macchine virtuali">Le macchine virtuali</abbr> da tali nodi ai nodi sotto riservata. Alcuni degli aspetti salienti di questa funzionalità sono i seguenti:

* *Si tratta di una soluzione zero-tempo di inattività*: <abbr title="macchine virtuali">Le macchine virtuali</abbr> vengono migrati in tempo reale per i nodi di inattività.
* *Integrazione con l'ambiente cluster esistente*: vengono rispettati i criteri di errore, ad esempio anti-affinità, i domini di errore e proprietari possibili.
* *L'euristica per bilanciamento del carico*: <abbr title="macchina virtuale">VM</abbr> utilizzo di memoria e l'utilizzo della CPU del nodo.
* *Un controllo granulare*: attivato per impostazione predefinita. Può essere attivato su richiesta o intervalli regolari.
* *Le soglie aggressività*: tre soglie disponibili basata sulle caratteristiche della distribuzione.

## <a id="feature-in-action"></a>La funzionalità in azione
### <a id="new-node-added"></a>Viene aggiunto un nuovo nodo al cluster di Failover
![Immagine di un nuovo nodo viene aggiunto al Cluster di Failover](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Quando si aggiungono nuove capacità per il Cluster di Failover, il <abbr title="macchina virtuale">VM</abbr> funzionalità di bilanciamento del carico bilancia automaticamente capacità dai nodi esistenti, nel nodo appena aggiunti nell'ordine seguente:

1. La pressione viene valutata sui nodi nel Cluster di Failover esistenti.
2. Tutti i nodi, il superamento della soglia vengono identificati.
3. Vengono identificati i nodi con la pressione più elevata per determinare la priorità di bilanciamento del carico.
4. <abbr title="Macchine virtuali">Le macchine virtuali</abbr> sono la migrazione in tempo reale (senza tempi di inattività) da un nodo, il superamento della soglia in un nodo nel Cluster di Failover appena aggiunto.

### <a id="recurring-load-balancing"></a>Bilanciamento del carico ricorrente
![Immagine di una macchina virtuale ricorrente il bilanciamento del carico](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Quando configurato per il bilanciamento del periodico, la pressione nei nodi del cluster viene valutata per bilanciamento del carico ogni 30 minuti. In alternativa, la pressione può essere valutato su richiesta. Ecco il flusso dei passaggi:

1. La pressione viene valutata in tutti i nodi nel cloud privato.
2. Tutti i nodi, il superamento della soglia e quelli soglia vengono identificati.
3. Vengono identificati i nodi con la pressione più elevata per determinare la priorità di bilanciamento del carico.
4. <abbr title="Macchine virtuali">Le macchine virtuali</abbr> sono la migrazione in tempo reale (senza tempi di inattività) da un nodo al superamento dei limiti al nodo inferiore alla soglia minima.

## <a name="see-also"></a>Vedere anche
* [Approfondimento sul bilanciamento del carico delle macchine virtuali](vm-load-balancing-deep-dive.md)
* [Clustering di failover](failover-clustering-overview.md)
* [Panoramica di Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
