---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: Quali sono le novità nel Clustering di Failover in Windows Server
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: b4fa59aa62acba5c89f20c191da2c3c1b776b1ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884752"
---
# <a name="whats-new-in-failover-clustering"></a>What's new in Failover Clustering (Novità del clustering di failover)

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

In questo argomento illustra le funzionalità nuove e modificate in Failover Clustering per Windows Server 2019, Windows Server 2016 e versioni canale semestrale di Windows Server.

## <a name="whats-new-in-windows-server-2019"></a>Novità di Windows Server 2019

- **Set di cluster**

    Set di cluster consentono di aumentare il numero di server in una soluzione singola definita da software datacenter (SDDC) oltre i limiti correnti di un cluster. Questa operazione viene eseguita mediante il raggruppamento di più cluster in un set di cluster: un raggruppamento a regime di controllo di più cluster di failover: calcolo, archiviazione e con iperconvergenza.
    Con i set di cluster, è possibile spostare le macchine virtuali online (migrazione in tempo reale) tra cluster all'interno del cluster impostato.

    Per altre informazioni, vedi [Cluster set](../storage/storage-spaces/cluster-sets.md).

- **Azure compatibile con cluster**

    I cluster di failover rileva ora automaticamente quando sono in esecuzione nelle macchine virtuali IaaS di Azure e ottimizzare la configurazione per garantire failover proattivo e la registrazione degli eventi di manutenzione pianificata di Azure per ottenere i massimi livelli di disponibilità. Distribuzione risulta semplificata, eliminando la necessità di configurare il bilanciamento del carico con nome di rete dinamici per il nome del cluster.

- **Migrazione del cluster tra domini**

    I cluster di failover possono ora spostare in modo dinamico da un dominio di Active Directory a un altro, semplificando il consolidamento dei domini e consentire i cluster creati da partner di hardware e aggiunti al dominio del cliente in un secondo momento.
- **Controllo del mirroring USB**

    È ora possibile usare un semplice dispositivo USB collegato a un commutatore di rete come server di controllo nella determinazione del quorum per un cluster. Estende il controllo di condivisione File per supportare qualsiasi dispositivo SMB2 conforme.

- **Miglioramenti all'infrastruttura di cluster**

    La cache CSV è ora abilitata per impostazione predefinita per migliorare le prestazioni delle macchine virtuali. MSDTC supporta ora Volumi condivisi cluster, per consentire la distribuzione di carichi di lavoro MSDTC in Spazi di archiviazione diretta, ad esempio con SQL Server. Logica avanzata per rilevare nodi partizionati con la riparazione automatica per restituire nodi ai membri del cluster. Rilevamento di route di rete di cluster e riparazione automatica avanzati.

- **Aggiornamento compatibile con cluster supporta la funzionalità spazi di archiviazione diretta**

    Aggiornamento compatibile con cluster è ora integrato e compatibile con Spazi di archiviazione diretta e convalida e garantisce il completamento della risincronizzazione dei dati su ciascun nodo. Aggiornamento compatibile con cluster, controlla se gli aggiornamenti solo se necessario, riavviare in modo intelligente. In questo modo orchestrando riavvii di tutti i server del cluster per la manutenzione pianificata.

- **Miglioramenti di controllo di condivisione di file** viene abilitato l'uso di un controllo di condivisione file negli scenari seguenti: 
    - Accesso a Internet assente o estremamente insufficiente a causa di una posizione remota, impedendo l'utilizzo di un cloud di controllo. 
    - Mancanza delle unità condivise per un disco di controllo. Potrebbe trattarsi di una configurazione iperconvergente di spazi di archiviazione diretta, un SQL Server Always in gruppi di disponibilità (AG), o un * Exchange disponibilità gruppo Database (DAG), nessuno dei quali Usa i dischi condivisi. 
    - Mancanza di una connessione al controller di dominio a causa di un cluster viene protetto da una rete Perimetrale. 
    - Un gruppo di lavoro o tra domini cluster per il quale non vi è alcun oggetto Active Directory del nome cluster (CNO). Altre informazioni su questi miglioramenti nel seguente post nel blog di gestione & Server: Controllo di condivisione File Cluster di failover e DFS.
    
    È ora in modo esplicito anche bloccare l'uso di una condivisione di spazi dei nomi DFS come un percorso. Aggiunta di un controllo di condivisione file a un DFS condivisione può causare problemi di stabilità per il cluster e questa configurazione non è mai stata supportata. È stata aggiunta logica per rilevare se una condivisione Usa spazi dei nomi DFS e presenza di spazi dei nomi DFS, gestione Cluster di Failover blocca la creazione del server di controllo e viene visualizzato un messaggio di errore relativo non è supportato.
- **Protezione avanzata dei cluster**

    Le comunicazioni interne a un cluster tramite Server Message Block (SMB) per Volumi condivisi cluster e Spazi di archiviazione diretta ora sfruttano i certificati per fornire la piattaforma più sicura. In questo modo i cluster di failover possono funzionare senza dipendenze su NTLM e abilitare gli elementi di base della sicurezza.
- **Cluster di failover non usa l'autenticazione NTLM**

    I cluster di failover non utilizza più l'autenticazione NTLM. Autenticazione Kerberos e l'autenticazione basata su certificati viene invece usato in modo esclusivo. Non sono presenti modifiche necessarie per l'utente o gli strumenti di distribuzione possa sfruttare i vantaggi di questo miglioramento della sicurezza. Consente inoltre i cluster di failover per la distribuzione in ambienti in cui è stato disabilitato NTLM. 


## <a name="whats-new-in-windows-server-2016"></a>Novità in Windows Server 2016

### <a name="BKMK_RollingUpgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster

Aggiornamento in sequenza del cluster del sistema operativo consente agli amministratori di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a una versione più recente senza interrompere i carichi di lavoro di Scale-Out File Server o Hyper-V. Usando questa funzionalità, è possibile evitare le sanzioni per il tempo di inattività previste dai contratti di servizio. 

**Valore aggiunto da queste modifiche**  

Aggiornamento di un cluster Hyper-V o Scale-Out File Server da Windows Server 2012 R2 a Windows Server 2016 non è più richiede tempi di inattività. Il cluster continuerà a funzionare a un livello di Windows Server 2012 R2 fino a quando tutti i nodi del cluster sono in esecuzione Windows Server 2016. Livello funzionale del cluster viene aggiornato a Windows Server 2016 usando il cmdlet di Windows PowerShell `Update-ClusterFunctionalLevel`. 

> [!WARNING]  
> -   Dopo aver aggiornato il livello funzionale del cluster, sarà più possibile tornare a un livello funzionale del cluster di Windows Server 2012 R2. 
> -   Fino a quando non la `Update-ClusterFunctionalLevel` cmdlet viene eseguito, il processo è reversibile e possibile aggiungere nodi di Windows Server 2012 R2 e Windows Server 2016 nodi possono essere rimossi. 

**Differenze di funzionamento**  

Un cluster di failover Hyper-V o Scale-Out File Server a questo punto può facilmente essere aggiornato senza tempi di inattività o di dover creare un nuovo cluster con nodi che eseguono il sistema operativo Windows Server 2016. Migrazione dei cluster a Windows Server 2012 R2 coinvolti portare offline il cluster esistente e reinstallare il nuovo sistema operativo per ogni nodo e quindi portare online il cluster. Il processo precedente era complesso e necessari tempi di inattività. Tuttavia, in Windows Server 2016, il cluster non dovrà venga portato offline in qualsiasi momento. 

I sistemi operativi cluster per l'aggiornamento in fasi sono i seguenti per ogni nodo del cluster:  
-   Il nodo viene messo in pausa e svuotato di tutte le macchine virtuali in esecuzione su di esso. 
-   Le macchine virtuali (o altro carico di lavoro cluster) viene migrata a un altro nodo del cluster. 
-   Il sistema operativo esistente viene rimosso e viene eseguita un'installazione pulita del sistema operativo Windows Server 2016 nel nodo. 
-   Il nodo che esegue il sistema operativo Windows Server 2016 viene aggiunto al cluster. 
-   A questo punto, il cluster viene detto sia in esecuzione in modalità mista, perché i nodi del cluster sono in esecuzione in Windows Server 2012 R2 o Windows Server 2016. 
-   Livello funzionale del cluster rimanga in Windows Server 2012 R2. Questo livello di funzionalità, nuove funzionalità di Windows Server 2016 che influiscono sulla compatibilità con le versioni precedenti del sistema operativo sarà disponibile. 
-   Infine, tutti i nodi vengono aggiornati a Windows Server 2016. 
-   Livello funzionale del cluster viene quindi modificato in Windows Server 2016 usando il cmdlet di Windows PowerShell `Update-ClusterFunctionalLevel`. A questo punto, è possibile sfruttare le funzionalità di Windows Server 2016. 

Per altre informazioni, vedere [aggiornamento in sequenza del Cluster del sistema operativo](cluster-operating-system-rolling-upgrade.md). 

### <a name="BKMK_SR"></a>Replica di archiviazione  
Replica archiviazione è una nuova funzionalità che consente la risorsa di archiviazione, a livello di blocco e sincrona tra server o cluster per il ripristino di emergenza, nonché l'adattamento di un cluster di failover tra siti. La replica sincrona consente il mirroring dei dati in siti fisici con volumi coerenti per arresto anomalo del sistema senza perdere dati a livello di file system. La replica asincrona consente l'estensione del sito oltre le aree metropolitane con la possibilità di perdita di dati. 

**Valore aggiunto da queste modifiche**  

Replica archiviazione consente di eseguire le operazioni seguenti:  

-   Offrire una soluzione singola di ripristino di emergenza per le interruzioni pianificate e non pianificate di carichi di lavoro di importanza critica. 

-   Usare il trasporto SMB3 con prestazioni, scalabilità e affidabilità comprovati. 

-   Estendere i cluster di failover di Windows su distanze metropolitane. 

-   Utilizzo di software Microsoft to end per l'archiviazione e clustering, ad esempio Hyper-V, Replica archiviazione, spazi di archiviazione, Cluster, Scale-Out File Server, SMB3, deduplicazione e NTFS o ReFS. 

-   Aiuta a ridurre costi e complessità come indicato di seguito:  

    -   È indipendente dall'hardware e non necessita di una configurazione di archiviazione specifica come SAN o DAS. 

    -   Consente le tecnologie di rete e di archiviazione. 

    -   Presenta una gestione grafica semplice per nodi e cluster singoli tramite Gestione cluster di failover. 

    -   Include le opzioni di scripting complete e su larga scala tramite Windows PowerShell. 

-   Consente di ridurre i tempi di inattività e di aumentare l'affidabilità e la produttività di Windows. 

-   Offre possibilità di supporto, metriche delle prestazioni e funzionalità di diagnostica. 

Per altre informazioni, vedere [Replica archiviazione in Windows Server 2016](../storage/storage-replica/storage-replica-overview.md). 


### <a name="BKMK_CloudWitness"></a>Cloud di controllo  
Cloud di controllo è un nuovo tipo di quorum di controllo per un cluster di failover in Windows Server 2016 che si basa su Microsoft Azure come punto di arbitraggio. Cloud di controllo, come altri quorum di controllo, ottiene un voto e può partecipare ai calcoli del quorum. È possibile configurare questa funzionalità come quorum di controllo usando la Configurazione guidata quorum del cluster. 

**Valore aggiunto da queste modifiche**  

Uso di Cloud di controllo come un Cluster di Failover quorum di controllo offre i vantaggi seguenti:  

-   Si basa su Microsoft Azure ed elimina la necessità di una terzo separate del Data Center. 

-   Usa standard disponibili pubblicamente Microsoft archiviazione Blob di Azure che elimina il sovraccarico di manutenzione aggiuntive di macchine virtuali ospitate in un cloud pubblico. 

-   Stesso Account di archiviazione di Microsoft Azure può essere utilizzato per più cluster (file di un blob per ogni cluster; cluster id univoco usato come nome del file blob). 

-   Fornisce un costo in corso molto basso per l'Account di archiviazione (dati di dimensioni molto ridotte scritti per ogni file di blob, file di blob aggiornato solo una volta quando viene modificato lo stato dei nodi del cluster). 

Per altre informazioni, vedere [distribuire un Cloud di controllo per un Cluster di Failover](deploy-cloud-witness.md). 

**Differenze di funzionamento**  

Questa funzionalità è stata introdotta in Windows Server 2016. 

### <a name="BKMK_VMs"></a>Resilienza della macchina virtuale  
**Resilienza di calcolo** Windows Server 2016 include la resilienza di calcolo maggiore delle macchine virtuali per consentire di ridurre i problemi di comunicazione all'interno del cluster nel cluster di elaborazione come indicato di seguito: 

-   **Opzioni di resilienza disponibili per le macchine virtuali:**  È ora possibile configurare le opzioni di resilienza di macchina virtuale che definiscono il comportamento delle macchine virtuali durante gli errori temporanei:  

    -   **Livello di resilienza:** Consente di definire come vengono gestiti gli errori temporanei. 

    -   **Periodo di resilienza:**  Consente di definire quanti tutte le macchine virtuali possono essere eseguiti isolati. 

-   **Quarantena di nodi non integri:** Nodi non integri vengono messi in quarantena e non saranno più autorizzati ad aggiungere il cluster. In questo modo si impedisce che i nodi instabile negativamente compromettono altri nodi e cluster nel suo complesso. 

Per altre informazioni macchina virtuale calcolo resilienza del flusso di lavoro e nodo impostazioni di quarantena che controllano il modo in cui il nodo viene inserito in isolamento o la quarantena, vedere [resilienza della macchina virtuale di calcolo in Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx). 

**Resilienza di archiviazione** In Windows Server 2016, le macchine virtuali sono più resiliente agli errori di archiviazione temporanea. La resilienza della macchina virtuale migliorata aiuta a mantenere gli stati di sessione di macchina virtuale tenant in caso di interruzione di archiviazione. Questo risultato viene ottenuto dalla macchina virtuale intelligente e veloce risposta ai problemi di infrastruttura di archiviazione. 

Quando una macchina virtuale si disconnette dal relativo spazio di archiviazione sottostante, consente di sospendere e in attesa di archiviazione per il ripristino. Durante la pausa, la macchina virtuale mantenga il contesto di applicazioni in esecuzione in esso. Quando viene ripristinata la connessione della macchina virtuale per lo spazio di archiviazione, la macchina virtuale torna allo stato in esecuzione. Di conseguenza, lo stato della sessione del computer tenant verrà conservato nel ripristino. 

In Windows Server 2016, la resilienza di archiviazione macchina virtuale è ottimizzato per i cluster guest e non specifici, troppo. 

### <a name="BKMK_Diagnostics"></a>Miglioramenti alla diagnostiche nel Clustering di Failover  
Per facilitare la diagnosi di problemi con i cluster di failover, Windows Server 2016 include quanto segue:  

-   Numerosi miglioramenti al file di log di cluster (ad esempio informazioni sul fuso orario e DiagnosticVerbose del log) che rende è più semplice risolvere i problemi del clustering di failover. Per altre informazioni, vedere [Windows Server 2016 Failover Cluster Troubleshooting Enhancements - Cluster Log](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx). 

-   Un nuovo tipo di un dump del **dump di memoria attiva**, che esclude la maggior parte delle pagine di memoria allocate alle macchine virtuali e di conseguenza rende le dmp molto più piccolo e semplice salvare o copiare. Per altre informazioni, vedere [Windows Server 2016 Failover Cluster Troubleshooting Enhancements - Dump Active](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx). 

### <a name="BKMK_SiteAware"></a>Cluster di Failover con riconoscimento del sito  
Windows Server 2016 include site - compatibile con i cluster di failover che abilitano i nodi di gruppo nei cluster estesi in base alla località fisica (sito). Riconoscimento dei siti di cluster migliora le operazioni principali durante il ciclo di vita del cluster, ad esempio il comportamento di failover, i criteri di posizionamento, heartbeat tra i nodi e comportamento di quorum. Per altre informazioni, vedere [con i cluster di Failover in Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx). 

### <a name="BKMK_multidomainclusters"></a>Cluster del gruppo di lavoro e con più domini  
In Windows Server 2012 R2 e versioni precedenti, un cluster può essere creato solo tra i nodi del membro aggiunti allo stesso dominio. Windows Server 2016 supera questo limite e introduce la possibilità di creare un cluster di failover indipendente da Active Directory. È ora possibile creare i cluster di failover nelle configurazioni seguenti:  

-   **Cluster con dominio singolo.** Cluster con tutti i nodi aggiunti al dominio stesso. 

-   **Cluster con più domini.** Cluster con nodi che sono membri di domini diversi. 

-   **Cluster del gruppo di lavoro.** I cluster con nodi che sono server membri o gruppi di lavoro (non di dominio). 

Per altre informazioni, vedere [multi-dominio o gruppo di cluster in Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
### <a name="BKMK_VMLoadBalancing"></a>Bilanciamento del carico delle macchine virtuali  
Il bilanciamento del carico macchina virtuale è una nuova funzionalità di Clustering di Failover che facilita la facile il bilanciamento del carico delle macchine virtuali tra i nodi in un cluster. Nodi overcommii vengono identificati basato su macchina virtuale di memoria e utilizzo della CPU nel nodo. Le macchine virtuali vengono spostate (migrazione in tempo reale) da un nodo di overcommit ai nodi con larghezza di banda disponibile (se applicabile). L'aggressività del bilanciamento del carico può essere ottimizzata per assicurare prestazioni ottimali del cluster e utilizzo. Bilanciamento del carico è abilitato per impostazione predefinita in Windows Server 2016 Technical Preview. Tuttavia, il bilanciamento del carico è disabilitata quando è abilitata l'ottimizzazione dinamica di SCVMM. 

### <a name="BKMK_VMStartOrder"></a>Ordine di avvio di macchine virtuali  
Ordine avvia macchina virtuale è una nuova funzionalità di Clustering di Failover che introduce l'orchestrazione dell'ordine di avvio per le macchine virtuali (e tutti i gruppi) in un cluster. Le macchine virtuali possono ora essere raggruppate in livelli e dipendenze di ordine di avvio possono essere create tra i diversi livelli. Ciò garantisce che le macchine virtuali più importanti (ad esempio le macchine virtuali controller di dominio o l'utilità) vengano avviate prima. Le macchine virtuali non siano state avviate fino a quando le macchine virtuali che hanno una dipendenza nel inoltre vengono avviate. 

### <a name="BKMK_SMBMultiChannel"></a> SMB multicanale semplificato e reti di Cluster Multi-NIC  
Le reti del Cluster di failover non sono più limitate a una singola scheda di rete per ogni subnet / di rete. Con SMB multicanale semplificato e reti di Cluster Multi-NIC, configurazione di rete è automatica e ogni interfaccia di rete nella subnet può essere usato per il traffico di cluster e del carico di lavoro. Questa funzionalità avanzata consente ai clienti di ottimizzare la velocità effettiva di rete per Hyper-V, Cluster di failover di SQL Server e altri carichi di lavoro SMB. 

Per altre informazioni, vedere [SMB multicanale semplificato e reti di Cluster Multi-NIC](smb-multichannel.md).

## <a name="see-also"></a>Vedere anche  
* [Archiviazione](../storage/storage.md)  
* [Nuove funzionalità di archiviazione in Windows Server 2016](../storage/whats-new-in-storage.md)  
