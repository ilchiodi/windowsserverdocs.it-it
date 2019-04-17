---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: "Novità di Clustering di Failover in Windows Server"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/11/2016
ms.openlocfilehash: a4330f62095e13f2f4736f15924ed31fb4893e7a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="whats-new-in-failover-clustering-in-windows-server-2016"></a>Novità di Clustering di Failover in Windows Server 2016
> Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento illustra le funzionalità nuove e modificate in Clustering di Failover in Windows Server 2016.  

## <a name="BKMK_RollingUpgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster  
Una nuova funzionalità di Clustering di Failover, Cluster del sistema operativo aggiornamento in sequenza, consente agli amministratori di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a Windows Server 2016 senza interrompere i carichi di lavoro Scale-Out File Server o Hyper-V. Usando questa funzionalità, è possono evitare le sanzioni tempi di inattività contro contratti livello di servizio (SLA).  

**Valore aggiunto da queste modifiche?**  

Aggiornamento di un cluster Hyper-V o File Server di scalabilità orizzontale da Windows Server 2012 R2 a Windows Server 2016 non è più richiesto tempo di inattività. Il cluster continuerà a funzionare a un livello di Windows Server 2012 R2 finché tutti i nodi del cluster sono in esecuzione Windows Server 2016. Livello funzionale del cluster viene aggiornato a Windows Server 2016 utilizzando il cmdlet di Windows PowerShell `Update-ClusterFunctionalLevel`.  

> [!WARNING]  
> -   Dopo aver aggiornato il livello di funzionalità del cluster, non puoi tornare a un livello di funzionalità cluster di Windows Server 2012 R2.  
> -   Fino a quando non la `Update-ClusterFunctionalLevel` cmdlet viene eseguito, il processo è reversibile, è possibile aggiungere nodi a Windows Server 2012 R2 e Windows Server 2016 nodi possono essere rimossi.  

**Differenze di funzionamento**  

Un cluster di failover Hyper-V o File Server di scalabilità orizzontale può ora sono essere facilmente aggiornato senza alcun tempo di inattività oppure è necessario creare un nuovo cluster con nodi che eseguono il sistema operativo Windows Server 2016. La migrazione di cluster a Windows Server 2012 R2 coinvolti portare offline il cluster esistente e reinstallare il nuovo sistema operativo per ogni nodi e quindi portare online il cluster. Il processo precedente è stato complesso e necessari tempi di inattività. Tuttavia, in Windows Server 2016, il cluster non necessario passare alla modalità offline in qualsiasi momento.  

I sistemi operativi di cluster per l'aggiornamento in più fasi sono i seguenti per ogni nodo del cluster:  
-   Il nodo è in pausa e separazione di tutte le macchine virtuali in esecuzione su di esso.  
-   Le macchine virtuali (o altri carichi di lavoro del cluster) vengono migrate in un altro nodo del cluster. Le macchine virtuali vengono migrate in un altro nodo del cluster.  
-   Il sistema operativo esistente viene rimosso e viene eseguita un'installazione pulita del sistema operativo Windows Server 2016 nel nodo.  
-   Il nodo che esegue il sistema operativo Windows Server 2016 viene aggiunto al cluster.  
-   A questo punto, il cluster viene definito sia in esecuzione in modalità mista, perché i nodi del cluster sono in esecuzione Windows Server 2012 R2 o Windows Server 2016.  
-   Livello funzionale del cluster rimane in Windows Server 2012 R2. Questo livello di funzionalità, nuove funzionalità in Windows Server 2016 che influiscono sulla compatibilità con le versioni precedenti del sistema operativo non sarà disponibile.  
-   Infine, tutti i nodi vengono aggiornati a Windows Server 2016.  
-   Livello di funzionalità del cluster viene quindi modificato in Windows Server 2016 tramite il cmdlet Windows PowerShell `Update-ClusterFunctionalLevel`. A questo punto, è possibile sfruttare le funzionalità di Windows Server 2016.  

Per ulteriori informazioni, vedere [aggiornamento in sequenza di Cluster del sistema operativo](cluster-operating-system-rolling-upgrade.md).  

## <a name="BKMK_SR"></a>Replica archiviazione  
Replica archiviazione è una nuova funzionalità che consente l'archiviazione indipendente, replica a livello di blocco e sincrona tra i server o cluster per il ripristino di emergenza, nonché l'adattamento di un cluster di failover tra siti. La replica sincrona consente il mirroring dei dati in siti fisici con volumi coerenti per arresto anomalo per garantire una perdita di dati a livello di file system. Replica asincrona consente l'estensione del sito oltre le aree metropolitane con la possibilità di perdita di dati.  

**Valore aggiunto da queste modifiche?**  

Replica archiviazione consente di eseguire le operazioni seguenti:  

-   Fornire un'unica soluzione di ripristino di emergenza fornitore per le interruzioni pianificate e non pianificate di carichi di lavoro di importanza critica.  

-   Utilizzare il trasporto SMB3 con prestazioni, scalabilità e affidabilità comprovati.  

-   Estendere i cluster di failover di Windows su distanze metropolitane.  

-   Utilizzare il software Microsoft to end per l'archiviazione e clustering, come Hyper-V, Replica archiviazione, spazi di archiviazione, Cluster, File Server di scalabilità orizzontale, SMB3, deduplicazione e NTFS o ReFS.  

-   Ridurre i costi e complessità come indicato di seguito:  

    -   È indipendente dall'hardware e non necessita di una configurazione di archiviazione specifica come SAN o DAS.  

    -   Consente le tecnologie di archiviazione e rete.  

    -   Facilità di funzionalità di gestione grafica per i singoli nodi e cluster tramite Gestione Cluster di Failover.  

    -   Include opzioni di scripting complete e su larga scala tramite Windows PowerShell.  

-   Consente di ridurre i tempi di inattività e aumentare l'affidabilità e la produttività di Windows.  

-   Fornire supporto, metriche delle prestazioni e funzionalità di diagnostica.  

Per ulteriori informazioni, vedere il [Replica archiviazione in Windows Server 2016](../storage/storage-replica/storage-replica-overview.md).  


## <a name="BKMK_CloudWitness"></a>Cloud di controllo  
Cloud di controllo è un nuovo tipo di quorum di controllo del Cluster di Failover in Windows Server 2016 che si basa su Microsoft Azure come punto di arbitraggio. Cloud di controllo, come altri quorum di controllo, ottiene un voto e può partecipare ai calcoli del quorum. È possibile configurare controllo cloud come un quorum di controllo usando la configurazione guidata quorum del Cluster.  

**Valore aggiunto da queste modifiche?**  

L'uso di Cloud di controllo come un Cluster di Failover quorum di controllo offre i vantaggi seguenti:  

-   Si basa su Microsoft Azure ed elimina la necessità di un terzo datacenter separato.  

-   Usa il standard disponibile pubblicamente Microsoft Azure archiviazione Blob che elimina l'overhead aggiuntivo manutenzione delle macchine virtuali ospitate in un cloud pubblico.  

-   Stesso Account di archiviazione di Microsoft Azure può essere utilizzato per più cluster (file di un blob per ogni cluster; id univoco del cluster utilizzato come nome del file blob).  

-   Fornisce un costo in corso insufficiente per l'Account di archiviazione (dati di dimensioni molto ridotte scritti per ogni file blob blob file aggiornato solo una volta quando viene modificato lo stato dei nodi del cluster).  

Per ulteriori informazioni, vedere [distribuire un Cloud di controllo per un Cluster di Failover](deploy-cloud-witness.md).  

**Differenze di funzionamento**  

Questa funzionalità è stata introdotta in Windows Server 2016.  

## <a name="BKMK_VMs"></a>Resilienza della macchina virtuale  
**Calcolo resilienza** Windows Server 2016 include macchine virtuali aumento calcolo resilienza per ridurre i problemi di comunicazione all'interno del cluster nel cluster di elaborazione nel modo seguente: 

-   **Opzioni di resilienza disponibili per le macchine virtuali:** è ora possibile configurare le opzioni di resilienza macchina virtuale che definiscono il comportamento delle macchine virtuali durante gli errori temporanei:  

    -   **Livello di resilienza:** consente di definire come vengono gestiti gli errori temporanei.  

    -   **Periodo di resilienza:** consente di definire quanto tempo tutte le macchine virtuali possono essere eseguite isolato.  

-   **Quarantena dei nodi di tipo non integri:** nodi di tipo non integri vengono messi in quarantena e non sono più consentiti da aggiungere al cluster. Ciò impedisce che i nodi ali influiscono negativamente altri nodi e il cluster generale.  

Per ulteriori informazioni calcolo resilienza del flusso di lavoro e nodo quarantena impostazioni della macchina virtuale che controllano la modalità di inserimento in quarantena o di isolamento del nodo, vedere [resilienza della macchina virtuale di calcolo in Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx).  

**Archiviazione resilienza** In Windows Server 2016, le macchine virtuali sono più resilienti agli errori di archiviazione temporanea. La resilienza migliorata macchina virtuale è possibile mantenere gli stati di sessione di macchina virtuale tenant in caso di un'interruzione di archiviazione. Questo risultato viene ottenuto dalla macchina virtuale intelligente e veloce risposta ai problemi dell'infrastruttura di archiviazione.  

Quando una macchina virtuale si disconnette dal relativo spazio di archiviazione sottostante, sospende e attende per l'archiviazione di ripristino. Durante la pausa, la macchina virtuale mantenga il contesto di applicazioni che sono in esecuzione. Quando la connessione della macchina virtuale per l'archiviazione viene ripristinata, la macchina virtuale torna allo stato in esecuzione. Di conseguenza, lo stato della sessione del computer tenant verrà conservato nel ripristino.  

In Windows Server 2016, resilienza archiviazione della macchina virtuale è presente e ottimizzato per i cluster guest.  

## <a name="BKMK_Diagnostics"></a>Miglioramenti della diagnostica nel Clustering di Failover  
Per facilitare la diagnosi di problemi con i cluster di failover, Windows Server 2016 include quanto segue:  

-   Vari miglioramenti per i file di registro cluster (ad esempio, informazioni sul fuso orario e DiagnosticVerbose log) che rende è più semplice risolvere i problemi del clustering di failover. Per ulteriori informazioni, vedere [Windows Server 2016 Failover Cluster Troubleshooting Enhancements - Cluster Log](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx).  

-   Un nuovo tipo di un dump di **dump della memoria Active**, che esclude la maggior parte delle pagine di memoria allocate alle macchine virtuali e pertanto il dmp molto più piccoli e facile da salvare o copiare.  Per ulteriori informazioni, vedere [Windows Server 2016 Failover Cluster Troubleshooting Enhancements - Dump Active](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx).  

## <a name="BKMK_SiteAware"></a>Cluster di Failover in grado di riconoscere del sito  
Windows Server 2016 include del sito - compatibile con i cluster di failover che consentono di raggruppare i nodi nei cluster estesi in base alla relativa posizione fisica (sito). Riconoscimento dei siti del cluster migliora le operazioni principali durante il ciclo di vita del cluster, ad esempio il comportamento di failover, i criteri di selezione host, heartbeat tra i nodi e il comportamento di quorum. Per ulteriori informazioni, vedere [sito compatibile con cluster di Failover in Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx).  

## <a name="BKMK_multidomainclusters"></a>Gruppo di lavoro e più domini cluster  
Solo in Windows Server 2012 R2 e versioni precedenti, è possibile creare un cluster tra i nodi del membro aggiunti allo stesso dominio.   Windows Server 2016 supera questo limite e introduce la possibilità di creare un Cluster di Failover senza le dipendenze di Active Directory. È ora possibile creare cluster di failover nelle configurazioni seguenti:  

-   **Cluster con dominio singolo.** Cluster con tutti i nodi aggiunti al dominio stesso.  

-   **Cluster a più domini.** Cluster con nodi che sono membri di domini diversi.  

-   **Gruppo di lavoro cluster.** I cluster con nodi che sono server membri al gruppo di lavoro (non di dominio).  

Per ulteriori informazioni, vedere [cluster del gruppo di lavoro e più domini in Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
## <a name="BKMK_VMLoadBalancing"></a>Bilanciamento del carico delle macchine virtuali  
Il bilanciamento del carico macchina virtuale è una nuova funzionalità di Clustering di Failover che facilita il trasparente il bilanciamento del carico delle macchine virtuali tra i nodi in un cluster. Nodi overcommii vengono identificati in base alle macchine virtuali della memoria e l'utilizzo della CPU sul nodo. Le macchine virtuali vengono spostate (migrazione in tempo reale) da un nodo overcommit ai nodi con larghezza di banda disponibile (se applicabile). L'aggressività al bilanciamento del può essere ottimizzata per garantire l'utilizzo e prestazioni ottimali.  Il bilanciamento del carico è abilitata per impostazione predefinita in Windows Server 2016 Technical Preview. Tuttavia, il bilanciamento del carico viene disabilitato quando è abilitata l'ottimizzazione dinamica SCVMM.  

## <a name="BKMK_VMStartOrder"></a>Ordine di avvio di macchina virtuale  
Ordine di avviare la macchina virtuale è una nuova funzionalità di Clustering di Failover che introduce orchestrazione dell'ordine di avvio per le macchine virtuali (e tutti i gruppi) in un cluster. Le macchine virtuali possono ora essere raggruppate in livelli e le dipendenze di ordine di avvio possono essere create tra diversi livelli. Ciò garantisce che siano state avviate prima delle macchine virtuali più importanti (ad esempio, i controller di dominio o utilità macchine virtuali). Le macchine virtuali non vengono avviate fino a quando le macchine virtuali presentano una dipendenza anche vengono avviate.  

## <a name="BKMK_SMBMultiChannel"></a>SMB multicanale semplificato e reti di Cluster Multi-NIC  
Le reti del Cluster di failover non sono più limitate a una singola scheda per ogni subnet / di rete. Con SMB multicanale semplificato e reti di Cluster Multi-NIC, configurazione di rete è automatica e tutte le schede NIC della subnet può essere utilizzata per il traffico del cluster e il carico di lavoro. Questo miglioramento consente ai clienti di ottimizzare la velocità effettiva di rete per altri carichi di lavoro SMB, istanza del Cluster di Failover di SQL Server e Hyper-V.  

Per ulteriori informazioni, vedere [SMB multicanale semplificato e reti di Cluster Multi-NIC](smb-multichannel.md).

## <a name="see-also"></a>Vedere anche  
* [Archiviazione](../storage/storage.md)  
* [Novità di archiviazione in Windows Server 2016](../storage/whats-new-in-storage.md)  
