---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: Novità del clustering di failover in Windows Server
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: cdc9280c4c7129d2e76341bd97b0ae3b8209888f
ms.sourcegitcommit: 7200143aa787c7ac05ae0e012263b1c9a95b87ed
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721739"
---
# <a name="whats-new-in-failover-clustering"></a>What's new in Failover Clustering (Novità del clustering di failover)

> Si applica a: Windows Server 2019, Windows Server 2016

In questo argomento vengono illustrate le funzionalità nuove e modificate del clustering di failover per Windows Server 2019 e Windows Server 2016.

## <a name="whats-new-in-windows-server-2019"></a>Novità di Windows Server 2019

- **Set di cluster**

    I set di cluster consentono di aumentare il numero di server in una singola soluzione SDDC (software-defined datacenter) oltre i limiti correnti di un cluster. Questa operazione viene eseguita raggruppando più cluster in un set di cluster, ovvero un raggruppamento a regime di controllo libero di più cluster di failover: calcolo, archiviazione e iperconvergenza.
    Con i set di cluster è possibile spostare le macchine virtuali in linea (live migrate) tra i cluster all'interno del set di cluster.

    Per altre informazioni, vedere [set di cluster](../storage/storage-spaces/cluster-sets.md).

- **Cluster compatibili con Azure**
  
    I cluster di failover ora rilevano automaticamente quando sono in esecuzione in macchine virtuali IaaS di Azure e ottimizzano la configurazione per fornire il failover proattivo e la registrazione degli eventi di manutenzione pianificata di Azure per ottenere i massimi livelli di disponibilità. La distribuzione viene semplificata anche eliminando la necessità di configurare il servizio di bilanciamento del carico con il nome di rete dinamico per il nome del cluster.

- **Migrazione di cluster tra domini**

    I cluster di failover possono ora spostarsi in modo dinamico da un dominio Active Directory a un altro, semplificando il consolidamento del dominio e consentendo la creazione di cluster da parte dei partner hardware e l'aggiunta al dominio del cliente in un secondo momento.

- **Controllo USB**

    È ora possibile usare un'unità USB semplice collegata a un commutire di rete come server di controllo del mirroring per determinare il quorum per un cluster. Questo consente di estendere il controllo di condivisione file per supportare qualsiasi dispositivo conforme a SMB2.

- **Miglioramenti all'infrastruttura cluster**

    La cache CSV è ora abilitata per impostazione predefinita per migliorare le prestazioni della macchina virtuale. MSDTC supporta ora i volumi condivisi del cluster, per consentire la distribuzione di carichi di lavoro MSDTC in Spazi di archiviazione diretta, ad esempio con SQL Server. Logica migliorata per rilevare i nodi partizionati con riparazione automatica per restituire nodi all'appartenenza al cluster. Rilevamento della route di rete cluster migliorato e correzione automatica.

- **L'aggiornamento compatibile con i cluster supporta Spazi di archiviazione diretta**

    Aggiornamento compatibile con cluster è ora integrato e compatibile con Spazi di archiviazione diretta, convalidando e garantendo il completamento della risincronizzazione dei dati in ogni nodo. Aggiornamento compatibile con cluster controlla gli aggiornamenti per il riavvio intelligente solo se necessario. In questo modo è possibile eseguire il riavvio di tutti i server nel cluster per la manutenzione pianificata.

- **Miglioramenti al controllo della condivisione file**

    È stato abilitato l'uso di una condivisione file di controllo negli scenari seguenti:
  - Accesso a Internet assente o estremamente scadente a causa di una posizione remota che impedisce l'uso di un cloud di controllo.
  - Mancanza di unità condivise per un disco di controllo. Potrebbe trattarsi di un Spazi di archiviazione diretta configurazione iperconvergente, di un gruppo di disponibilità di SQL Server Always On (AG) o di un gruppo di disponibilità del database di Exchange (DAG), nessuno dei quali usa dischi condivisi.
  - Mancanza di una connessione del controller di dominio dovuta al fatto che il cluster si trova dietro una rete perimetrale.
  - Un cluster di gruppi di lavoro o tra domini per il quale non è presente Active Directory oggetto nome cluster (cluster Name Object). Ulteriori informazioni su questi miglioramenti sono disponibili nel post seguente nei Blog sulla gestione di server &: condivisione file di controllo del cluster di failover e DFS.

    È ora possibile bloccare in modo esplicito l'uso di una condivisione di spazi dei nomi DFS come percorso. L'aggiunta di un controllo di condivisione file a una condivisione DFS può causare problemi di stabilità per il cluster e questa configurazione non è mai stata supportata. È stata aggiunta la logica per rilevare se una condivisione utilizza spazi dei nomi DFS e se viene rilevato uno spazio dei nomi DFS, Gestione cluster di failover blocca la creazione del server di controllo del mirroring e visualizza un messaggio di errore in cui non è supportato.

- **Protezione avanzata del cluster**

    Comunicazione intra-cluster su Server Message Block (SMB) per volumi condivisi cluster e Spazi di archiviazione diretta utilizza ora i certificati per fornire la piattaforma più sicura. Questo consente ai cluster di failover di funzionare senza dipendenze da NTLM e abilitare le basi di riferimento per la sicurezza.

- **Il cluster di failover non usa più l'autenticazione NTLM**

    I cluster di failover non utilizzano più l'autenticazione NTLM. L'autenticazione Kerberos e basata sui certificati viene invece utilizzata esclusivamente in modo esclusivo. Non sono richieste modifiche da parte dell'utente o degli strumenti di distribuzione per sfruttare i vantaggi di questo miglioramento della sicurezza. Consente inoltre di distribuire i cluster di failover in ambienti in cui NTLM è stato disabilitato.

## <a name="whats-new-in-windows-server-2016"></a>Novità di Windows Server 2016

### <a name="cluster-operating-system-rolling-upgrade"></a><a name="BKMK_RollingUpgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster

L'aggiornamento in sequenza del sistema operativo del cluster consente a un amministratore di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a una versione più recente senza arrestare i carichi di lavoro Hyper-V o File server di scalabilità orizzontale. Usando questa funzionalità, è possibile evitare le sanzioni per il tempo di inattività previste dai contratti di servizio.

**Valore aggiunto da queste modifiche**  

L'aggiornamento di un cluster Hyper-V o File server di scalabilità orizzontale da Windows Server 2012 R2 a Windows Server 2016 non richiede più tempo di inattività. Il cluster continuerà a funzionare a livello di Windows Server 2012 R2 fino a quando tutti i nodi del cluster non eseguono Windows Server 2016. Il livello di funzionalità del cluster viene aggiornato a Windows Server 2016 usando Windows PowerShell cmdlet `Update-ClusterFunctionalLevel` .

> [!WARNING]  
> - Dopo aver aggiornato il livello di funzionalità del cluster, non è possibile tornare al livello di funzionalità del cluster di Windows Server 2012 R2.
>
> - Fino a quando non `Update-ClusterFunctionalLevel` viene eseguito il cmdlet, il processo è reversibile e i nodi di Windows server 2012 R2 possono essere aggiunti e i nodi di Windows server 2016 possono essere rimossi.

**Differenze di funzionamento**  

Un cluster di failover Hyper-V o File server di scalabilità orizzontale ora può essere facilmente aggiornato senza tempi di inattività oppure è necessario compilare un nuovo cluster con nodi che eseguono il sistema operativo Windows Server 2016. La migrazione dei cluster a Windows Server 2012 R2 ha comportato la disconnessione del cluster esistente e la reinstallazione del nuovo sistema operativo per ogni nodo, quindi riportare il cluster di nuovo online. Il processo precedente è stato complesso e richiede tempi di inattività. Tuttavia, in Windows Server 2016, non è necessario che il cluster passi alla modalità offline in qualsiasi momento.

I sistemi operativi del cluster per l'aggiornamento in fasi sono i seguenti per ogni nodo in un cluster:  
-   Il nodo viene sospeso e svuotato di tutte le macchine virtuali in esecuzione su di esso. 
-   Viene eseguita la migrazione delle macchine virtuali (o di un altro carico di lavoro del cluster) a un altro nodo del cluster. 
-   Il sistema operativo esistente viene rimosso e viene eseguita un'installazione pulita del sistema operativo Windows Server 2016 nel nodo. 
-   Il nodo che esegue il sistema operativo Windows Server 2016 viene nuovamente aggiunto al cluster. 
-   A questo punto, si dice che il cluster è in esecuzione in modalità mista, perché i nodi del cluster eseguono Windows Server 2012 R2 o Windows Server 2016. 
-   Il livello di funzionalità del cluster rimane in Windows Server 2012 R2. A questo livello di funzionalità, le nuove funzionalità di Windows Server 2016 che influiscono sulla compatibilità con le versioni precedenti del sistema operativo non saranno disponibili. 
-   Infine, tutti i nodi vengono aggiornati a Windows Server 2016. 
-   Il livello di funzionalità del cluster viene quindi modificato in Windows Server 2016 usando il cmdlet di Windows PowerShell `Update-ClusterFunctionalLevel` . A questo punto, è possibile sfruttare le funzionalità di Windows Server 2016. 

Per ulteriori informazioni, vedere [aggiornamento in sequenza del sistema operativo del cluster](cluster-operating-system-rolling-upgrade.md). 

### <a name="storage-replica"></a><a name="BKMK_SR"></a>Replica archiviazione  
Replica archiviazione è una nuova funzionalità che consente la replica sincrona, a livello di blocco e indipendente dall'archiviazione tra server o cluster per il ripristino di emergenza, nonché l'estensione di un cluster di failover tra siti. La replica sincrona consente il mirroring dei dati in siti fisici con volumi coerenti per arresto anomalo del sistema senza perdere dati a livello di file system. La replica asincrona consente l'estensione del sito oltre le aree metropolitane con la possibilità di perdita di dati. 

**Valore aggiunto da queste modifiche**  

Replica archiviazione consente di eseguire le operazioni seguenti:  

-   Offrire una soluzione singola di ripristino di emergenza per le interruzioni pianificate e non pianificate di carichi di lavoro di importanza critica. 

-   Usare il trasporto SMB3 con prestazioni, scalabilità e affidabilità comprovati. 

-   Estendere i cluster di failover di Windows su distanze metropolitane. 

-   Usare il software Microsoft end-to-end per l'archiviazione e il clustering, ad esempio Hyper-V, replica di archiviazione, spazi di archiviazione, cluster, File server di scalabilità orizzontale, SMB3, deduplicazione dati e ReFS/NTFS. 

-   Aiuta a ridurre costi e complessità come indicato di seguito:  

    -   È indipendente dall'hardware e non necessita di una configurazione di archiviazione specifica come SAN o DAS. 

    -   Consente le tecnologie di rete e di archiviazione. 

    -   Presenta una gestione grafica semplice per nodi e cluster singoli tramite Gestione cluster di failover. 

    -   Include le opzioni di scripting complete e su larga scala tramite Windows PowerShell. 

-   Consente di ridurre i tempi di inattività e di aumentare l'affidabilità e la produttività di Windows. 

-   Offre possibilità di supporto, metriche delle prestazioni e funzionalità di diagnostica. 

Per altre informazioni, vedere [Replica archiviazione in Windows Server 2016](../storage/storage-replica/storage-replica-overview.md). 

### <a name="cloud-witness"></a><a name="BKMK_CloudWitness"></a>Cloud di controllo

Cloud di controllo è un nuovo tipo di quorum di controllo per un cluster di failover in Windows Server 2016 che si basa su Microsoft Azure come punto di arbitraggio. Cloud di controllo, come altri quorum di controllo, ottiene un voto e può partecipare ai calcoli del quorum. È possibile configurare questa funzionalità come quorum di controllo usando la Configurazione guidata quorum del cluster. 

**Valore aggiunto da queste modifiche**  

L'uso del cloud di controllo come quorum di controllo del cluster di failover offre i vantaggi seguenti:  

-   Sfrutta Microsoft Azure ed elimina la necessità di un terzo data center separato. 

-   USA lo standard disponibile pubblicamente Microsoft Azure archiviazione BLOB, che elimina l'overhead di manutenzione aggiuntivo delle macchine virtuali ospitate in un cloud pubblico. 

-   Lo stesso account di Archiviazione di Microsoft Azure può essere usato per più cluster (un file BLOB per cluster; ID univoco del cluster usato come nome del file BLOB). 

-   Fornisce un costo molto basso per l'account di archiviazione (dati molto piccoli scritti per ogni file BLOB, il file BLOB viene aggiornato solo una volta quando viene modificato lo stato dei nodi del cluster). 

Per altre informazioni, vedere [distribuire un cloud di controllo per un cluster di failover](deploy-cloud-witness.md). 

**Differenze di funzionamento**  

Questa funzionalità è stata introdotta in Windows Server 2016. 

### <a name="virtual-machine-resiliency"></a><a name="BKMK_VMs"></a>Resilienza macchina virtuale

**Resilienza di calcolo** Windows Server 2016 include un aumento della resilienza delle macchine virtuali per ridurre i problemi di comunicazione all'interno del cluster nel cluster di calcolo, come indicato di seguito: 

-   **Opzioni di resilienza disponibili per le macchine virtuali:**  È ora possibile configurare le opzioni di resilienza delle macchine virtuali che definiscono il comportamento delle macchine virtuali durante gli errori temporanei:  

    -   **Livello di resilienza:** Consente di definire il modo in cui vengono gestiti gli errori temporanei. 

    -   **Periodo di resilienza:**  Consente di definire per quanto tempo tutte le macchine virtuali possono essere eseguite in modalità isolata. 

-   **Quarantena dei nodi non integri:** I nodi non integri sono messi in quarantena e non possono più essere aggiunti al cluster. Ciò impedisce che i nodi influiscano negativamente sugli altri nodi e sul cluster complessivo. 

Per altre informazioni sul flusso di lavoro di resilienza delle macchine virtuali e sulle impostazioni di quarantena del nodo che controllano il modo in cui il nodo viene inserito in isolamento o quarantena, vedere [resilienza di calcolo delle macchine virtuali in Windows Server 2016](https://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx). 

**Resilienza di archiviazione** In Windows Server 2016, le macchine virtuali sono più resilienti agli errori di archiviazione temporanei. La resilienza migliorata della macchina virtuale aiuta a mantenere gli Stati di sessione della macchina virtuale tenant in caso di un'ininterrottità di archiviazione. Questa operazione viene ottenuta tramite risposta di macchina virtuale intelligente e rapida ai problemi dell'infrastruttura di archiviazione. 

Quando una macchina virtuale si disconnette dalla relativa archiviazione sottostante, viene sospesa e resta in attesa del ripristino dell'archiviazione. Durante la pausa, la macchina virtuale conserva il contesto di applicazioni in esecuzione. Quando viene ripristinata la connessione della macchina virtuale alla relativa archiviazione, la macchina virtuale torna allo stato di esecuzione. Di conseguenza, lo stato della sessione del computer tenant viene mantenuto al momento del ripristino. 

In Windows Server 2016, la resilienza dell'archiviazione delle macchine virtuali è compatibile e ottimizzata anche per i cluster guest. 

### <a name="diagnostic-improvements-in-failover-clustering"></a><a name="BKMK_Diagnostics"></a>Miglioramenti diagnostici nel clustering di failover

Per semplificare la diagnosi dei problemi relativi ai cluster di failover, Windows Server 2016 include quanto segue:  

- Diversi miglioramenti apportati ai file di log del cluster, ad esempio informazioni sul fuso orario e log DiagnosticVerbose, che semplificano la risoluzione dei problemi relativi al clustering di failover. Per ulteriori informazioni, vedere [miglioramenti della risoluzione dei problemi del cluster di failover di Windows Server 2016-log del cluster](https://techcommunity.microsoft.com/t5/failover-clustering/windows-server-2016-failover-cluster-troubleshooting/ba-p/372005).

- Un nuovo tipo di dump del **dump di memoria attivo**, che filtra la maggior parte delle pagine di memoria allocate alle macchine virtuali e quindi rende la memoria. dmp molto più piccola e più semplice da salvare o copiare. Per ulteriori informazioni, vedere [miglioramenti della risoluzione dei problemi del cluster di failover di Windows Server 2016-dump attivo](https://techcommunity.microsoft.com/t5/failover-clustering/windows-server-2016-failover-cluster-troubleshooting/ba-p/372008).

### <a name="site-aware-failover-clusters"></a><a name="BKMK_SiteAware"></a>Cluster di failover con informazioni sulla presenza nel sito

Windows Server 2016 include cluster di failover compatibili con il sito che consentono di raggruppare i nodi nei cluster estesi, in base alla posizione fisica (sito). Il riconoscimento del sito del cluster migliora le operazioni principali durante il ciclo di vita del cluster, ad esempio il comportamento di failover, i criteri di posizionamento, l'heartbeat tra i nodi e il comportamento del quorum. Per ulteriori informazioni, vedere [cluster di failover in grado di riconoscere il sito in Windows Server 2016](https://techcommunity.microsoft.com/t5/failover-clustering/site-aware-failover-clusters-in-windows-server-2016/ba-p/372060).

### <a name="workgroup-and-multi-domain-clusters"></a><a name="BKMK_multidomainclusters"></a>Workgroup and Multi-domain clusters (Gruppo di lavoro e cluster a più domini)

In Windows Server 2012 R2 e versioni precedenti è possibile creare un cluster solo tra nodi membro aggiunti allo stesso dominio. Windows Server 2016 supera questo limite e introduce la possibilità di creare un cluster di failover indipendente da Active Directory. È ora possibile creare cluster di failover nelle configurazioni seguenti:  

-   **Cluster a dominio singolo.** Cluster con tutti i nodi aggiunti allo stesso dominio. 

-   **Cluster multidominio.** Cluster con nodi che sono membri di domini diversi. 

-   **Cluster del gruppo di lavoro.** Cluster con nodi che sono server membro/gruppo di lavoro (non aggiunto a un dominio). 

Per altre informazioni, vedere [gruppi di lavoro e cluster multidominio in Windows Server 2016](https://techcommunity.microsoft.com/t5/failover-clustering/workgroup-and-multi-domain-clusters-in-windows-server-2016/ba-p/372059)

### <a name="virtual-machine-load-balancing"></a><a name="BKMK_VMLoadBalancing"></a>Bilanciamento del carico della macchina virtuale  

Il bilanciamento del carico della macchina virtuale è una nuova funzionalità del clustering di failover che semplifica il bilanciamento del carico delle macchine virtuali tra i nodi di un cluster. I nodi overcommit sono identificati in base alla memoria della macchina virtuale e all'utilizzo della CPU nel nodo. Le macchine virtuali vengono quindi spostate (migrate in tempo reale) da un nodo overcommit ai nodi con larghezza di banda disponibile (se applicabile). L'aggressività del bilanciamento può essere ottimizzata per garantire prestazioni e utilizzo ottimali del cluster. Il bilanciamento del carico è abilitato per impostazione predefinita in Windows Server 2016 Technical Preview. Tuttavia, il bilanciamento del carico è disabilitato quando è abilitata l'ottimizzazione dinamica SCVMM. 

### <a name="virtual-machine-start-order"></a><a name="BKMK_VMStartOrder"></a>Ordine di avvio della macchina virtuale

L'ordine di avvio della macchina virtuale è una nuova funzionalità del clustering di failover che introduce l'orchestrazione dell'ordine di avvio per le macchine virtuali e tutti i gruppi in un cluster. Ora le macchine virtuali possono essere raggruppate in livelli e le dipendenze degli ordini di avvio possono essere create tra livelli diversi. In questo modo si garantisce che le macchine virtuali più importanti, ad esempio controller di dominio o macchine virtuali di utilità, vengano avviate per prime. Le macchine virtuali non vengono avviate fino a quando non vengono avviate anche le macchine virtuali di cui hanno una dipendenza. 

### <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a><a name="BKMK_SMBMultiChannel"></a>Reti SMB multicanale e multicanale semplificate

Le reti cluster di failover non sono più limitate a una singola scheda di interfaccia di rete per subnet/rete. Con le reti SMB multicanale e multicanale semplificate, la configurazione di rete è automatica e ogni scheda di interfaccia di rete nella subnet può essere usata per il traffico del cluster e del carico di lavoro. Questa funzionalità avanzata consente ai clienti di ottimizzare la velocità effettiva della rete per Hyper-V, SQL Server istanza del cluster di failover e altri carichi di lavoro SMB. 

Per ulteriori informazioni, vedere [reti SMB multicanale e cluster multicanale semplificate](smb-multichannel.md).

## <a name="see-also"></a>Vedere anche

* [Archiviazione](../storage/storage.md)  
* [Novità di archiviazione in Windows Server 2016](../storage/whats-new-in-storage.md)  
