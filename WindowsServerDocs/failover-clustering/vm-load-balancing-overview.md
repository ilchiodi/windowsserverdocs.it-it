---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: Panoramica del bilanciamento del carico della macchina virtuale
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 1fea9e6297399a5081eb8fef1876f1b11d23c745
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360985"
---
# <a name="virtual-machine-load-balancing-overview"></a>Panoramica del bilanciamento del carico della macchina virtuale

> Si applica a: Windows Server 2019, Windows Server 2016

Una considerazione fondamentale per le distribuzioni di cloud privati è la spesa capitale (<abbr title="spesa capitale">Investimenti</abbr>) necessario per entrare nell'ambiente di produzione. È molto comune aggiungere ridondanza alle distribuzioni di cloud privati per evitare la capacità inferiore durante i picchi di traffico nell'ambiente di produzione, ma ciò aumenta <abbr title="spesa capitale">Investimenti</abbr>. La necessità di ridondanza è basata su cloud privati sbilanciati in cui alcuni nodi ospitano più macchine virtuali (<abbr title="macchine virtuali">Macchine virtuali</abbr>e altri sono sottoutilizzati, ad esempio un server appena riavviato.

<strong>Panoramica video veloce</strong><br>(6 minuti)<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a id="what-is-vm-load-balancing"></a>Che cos'è il bilanciamento del carico della macchina virtuale?
<abbr title="Macchina virtuale">VM</abbr> Il bilanciamento del carico è una funzionalità predefinita di Windows Server 2019 e Windows Server 2016 che consente di ottimizzare l'utilizzo dei nodi in un cluster di failover. Identifica i nodi sottoposte a commit e ridistribuisce <abbr title="macchine virtuali">Macchine virtuali</abbr> da tali nodi ai nodi sottoposti a commit. Di seguito sono riportati alcuni degli aspetti salienti di questa funzionalità:

* *Si tratta di una soluzione senza tempi di inattività*: <abbr title="Macchine virtuali">Macchine virtuali</abbr> viene eseguita la migrazione in tempo reale ai nodi inattivi.
* *Perfetta integrazione con l'ambiente cluster esistente*: Vengono rispettati i criteri di errore quali anti-affinità, domini di errore e proprietari possibili.
* *Euristica per il bilanciamento*: <abbr title="Macchina virtuale">VM</abbr> utilizzo elevato della memoria e della CPU del nodo.
* *Controllo granulare*: Abilitata per impostazione predefinita. Può essere attivato su richiesta o a intervalli regolari.
* *Soglie di aggressività*: Sono disponibili tre soglie basate sulle caratteristiche della distribuzione.

## <a id="feature-in-action"></a>Funzionalità in azione
### <a id="new-node-added"></a>Un nuovo nodo viene aggiunto al cluster di failover
![Rappresentazione grafica di un nuovo nodo aggiunto al cluster di failover](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Quando si aggiunge una nuova capacità al cluster di failover, il <abbr title="macchina virtuale">VM</abbr> La funzionalità di bilanciamento del carico bilancia automaticamente la capacità dai nodi esistenti al nodo appena aggiunto nell'ordine seguente:

1. La pressione viene valutata sui nodi esistenti nel cluster di failover.
2. Vengono identificati tutti i nodi che superano la soglia.
3. Vengono identificati i nodi con la massima pressione per determinare la priorità del bilanciamento.
4. <abbr title="Macchine virtuali">Macchine virtuali</abbr> viene eseguita la migrazione in tempo reale (senza tempi di inattività) da un nodo che supera la soglia di un nodo appena aggiunto nel cluster di failover.

### <a id="recurring-load-balancing"></a>Bilanciamento del carico ricorrente
![Rappresentazione grafica di un bilanciamento del carico della macchina virtuale ricorrente](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Quando è configurato per il bilanciamento periodico, il carico sui nodi del cluster viene valutato per il bilanciamento ogni 30 minuti. In alternativa, è possibile valutare la pressione su richiesta. Ecco il flusso dei passaggi:

1. La pressione viene valutata in tutti i nodi del cloud privato.
2. Vengono identificati tutti i nodi che superano la soglia e quelli sotto la soglia.
3. Vengono identificati i nodi con la massima pressione per determinare la priorità del bilanciamento.
4. <abbr title="Macchine virtuali">Macchine virtuali</abbr> viene eseguita la migrazione in tempo reale (senza tempi di inattività) da un nodo che supera la soglia al nodo sotto la soglia minima.

## <a name="see-also"></a>Vedere anche
* [Approfondimento sul bilanciamento del carico della macchina virtuale](vm-load-balancing-deep-dive.md)
* [Clustering di failover](failover-clustering-overview.md)
* [Panoramica di Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
