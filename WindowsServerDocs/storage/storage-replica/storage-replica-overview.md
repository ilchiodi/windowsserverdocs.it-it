---
title: Informazioni generali su Replica archiviazione
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 10/11/2016
ms.assetid: e9b18e14-e692-458a-a39f-d5b569ae76c5
ms.openlocfilehash: a1ed4627928cebd3aa1fa2b54ccd5f3f6a65a2d1
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="storage-replica-overview"></a>Panoramica di Replica di archiviazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Replica di archiviazione è la tecnologia di Windows Server che consente la replica dei volumi tra server o cluster per il ripristino di emergenza. Consente inoltre di creare cluster estesi di failover che si estendono su due siti, con tutti i nodi sincronizzati.

Replica di archiviazione supporta la replica sincrona e quella asincrona:

* La **replica sincrona** consente il mirroring dei dati all'interno di un sito di rete a bassa latenza con volumi coerenti per arresto anomalo del sistema al fine di impedire la perdita di dati a livello di file system in una condizione di errore.
* La **replica asincrona** consente il mirroring dei dati tra siti esterni agli intervalli di rete MAN su collegamenti di rete con latenze più elevate, ma senza la garanzia che entrambi i siti dispongano di copie identiche dei dati al momento dell'errore.

Questa guida descrive come le aziende possono trarre vantaggio da questa nuova funzionalità e indica i diversi scenari di replica supportati da Replica archiviazione. Si presuppone una conoscenza precedente di Windows Server, clustering di failover, file server e Hyper-V, inclusa l'amministrazione di base.  

## <a name="why-use-storage-replica"></a>Perché usare Replica archiviazione?  
Replica archiviazione offre nuove funzionalità di ripristino di emergenza e preparazione in Windows Server 2016 Datacenter Edition. Per la prima volta, Windows Server offre la tranquillità della perdita di dati pari a zero, con la possibilità di proteggere in modo sincrono i dati in diversi rack, piani, edifici, campus, province e città. Dopo un'emergenza, tutti i dati saranno trasferiti in un'altra posizione senza alcuna perdita. Lo stesso vale *prima* di un'emergenza; Replica archiviazione offre la possibilità di spostare i carichi di lavoro in una posizione sicura prima delle catastrofi quando vengono concessi alcuni secondi di avviso, ancora una volta senza perdita di dati.  

Replica archiviazione consente un uso più efficiente dei centri dati. Estendendo o replicando i cluster, i carichi di lavoro possono essere eseguiti in più centri dati perché gli utenti di prossimità locale e le applicazioni possano accedere ai dati più velocemente, senza contare una migliore distribuzione del carico e un utilizzo più efficiente delle risorse di calcolo. Se si verifica un'emergenza nei centri dati offline, è possibile spostare temporaneamente i carichi di lavoro tipici all'altro sito.  

Replica di archiviazione può consentire di ritirare i sistemi di replica dei file esistenti, come Replica DFS, che sono stati usati come soluzioni di ripristino di emergenza di fascia bassa. Anche se Replica DFS funziona correttamente sulle reti a larghezza di banda molto bassa, la sua latenza è molto elevata ed è spesso misurata in termini di ore o giorni. Ciò è dovuto alla necessità di chiudere i file e alle limitazioni artificiali che hanno lo scopo di impedire la congestione della rete. Con queste caratteristiche di progettazione, i file più recenti e più importanti in una replica gestita da Replica DFS sono quelli che più difficilmente verranno replicati. Replica archiviazione viene eseguita sotto il livello dei file e non presenta alcuna di queste restrizioni.  

Replica archiviazione supporta anche la replica asincrona per gli intervalli più lunghi e le reti a latenza superiore. Poiché non è basata sul punto di arresto ma replica continuamente, i delta delle modifiche tenderanno a essere molto inferiori rispetto ai prodotti basati su snapshot. Inoltre, Replica archiviazione opera a livello di partizione e, pertanto, replica tutti gli snapshot di Servizio Copia Shadow del volume creati da Windows Server o dal software di backup; in questo modo consente l'uso di snapshot dei dati coerenti con l'applicazione per il ripristino temporizzato, in particolar modo i dati utente non strutturati replicati in modo asincrono.  

## <a name="BKMK_SRSupportedScenarios"></a>Configurazioni supportate

Usando questa guida e Windows Server 2016 Datacenter Edition è possibile distribuire la replica di archiviazione nelle configurazioni con cluster esteso, da cluster a cluster e da server a server (vedere le figure 1-3).

Lo scenario con **cluster esteso** consente la configurazione di computer e dell'archiviazione in un singolo cluster, in cui alcuni nodi condividono un unico set di archiviazione asimmetrica e alcuni nodi ne condividono un altro, quindi esegue la replica in modo sincrono o asincrono con riconoscimento dei siti. Questo scenario può usare Spazi di archiviazione con LUN collegati tramite dispositivi di archiviazione SAS condivisi, SAN e iSCSI. Viene gestito con PowerShell e lo strumento grafico Gestione cluster di failover e consente il failover automatico del carico di lavoro.  

![Diagramma che mostra due nodi del cluster di New York che usa Replica di archiviazione per replicare l'archiviazione con due nodi in New Jersey](./media/Storage-Replica-Overview/Storage_SR_StretchCluster.png)  

**FIGURA 1: Replica archiviazione in un cluster esteso tramite Replica archiviazione**  

Lo scenario **da cluster a cluster** consente la replica tra due cluster distinti, in cui un cluster replica in modo sincrono o asincrono con un altro cluster. Questo scenario può usare Spazi di archiviazione diretta e Spazi di archiviazione con LUN collegati tramite dispositivi di archiviazione SAS condivisi, SAN e iSCSI. Viene gestito con PowerShell e richiede l'intervento manuale per il failover. Il supporto per Azure Site Recovery di questo scenario è pianificato.

![Diagramma che mostra un cluster di Los Angeles che usa Replica di archiviazione per replicare l'archiviazione con un cluster diverso a Las Vegas](./media/Storage-Replica-Overview/Storage_SR_ClustertoCluster.png)  

**FIGURA 2: Replica archiviazione da cluster a cluster tramite Replica archiviazione**  

Lo scenario **da server a server** consente la replica sincrona e asincrona tra due server autonomi, usando Spazi di archiviazione con LUN collegati tramite dispositivi di archiviazione SAS condivisi, SAN e iSCSI e unità locali. Viene gestito con PowerShell e con lo strumento Server Manager e richiede l'intervento manuale per il failover.  

![Diagramma che mostra la replica di un server in Building 5 con un server in Building 9](./media/Storage-Replica-Overview/Storage_SR_ServertoServer.png)  

**FIGURA 3: Replica archiviazione da server a server tramite Replica archiviazione**  

> [!NOTE]
> È inoltre possibile configurare la replica da server a sé stesso usando quattro volumi separati in un computer. Tuttavia, questa guida non illustra questo scenario.  

## <a name="BKMK_SR2"> </a> Funzionalità di Replica archiviazione  

* **Perdita di dati pari a zero, replica a livello di blocco**. Con la replica sincrona non vi è alcuna possibilità di perdere i dati. Con la replica a livello di blocco non vi è alcuna possibilità di bloccare i file.  

* **Distribuzione e gestione semplici**. Replica archiviazione dispone di un mandato di progettazione per facilitarne l'uso. La creazione di una relazione di replica tra due server richiede solo un unico comando di PowerShell. La distribuzione di cluster estesi usa la procedura guidata intuitiva nello strumento Gestione cluster di failover già noto.   

* **Guest e host**. Tutte le funzionalità di Replica archiviazione sono esposte in distribuzioni virtualizzate basate su host e guest. Ciò significa che i guest possono replicare i volumi di dati anche se sono in esecuzione su piattaforme di virtualizzazione non Windows o in cloud pubblici, purché usino Windows Server 2016 Datacenter Edition nel guest.  

* **Basata su SMB3**. Replica archiviazione usa la tecnologia comprovata e collaudata SMB 3, rilasciata per la prima volta in Windows Server 2012. Ciò significa che tutte le caratteristiche avanzate di SMB, come il supporto di SMB multicanale e diretto sulle schede di rete RoCE, iWARP e InfiniBand RDMS, sono disponibili per Replica archiviazione.   

* **Sicurezza**. A differenza di molti prodotti di fornitore, Replica archiviazione integra una tecnologia di protezione leader nel settore. Ciò include la firma dei pacchetti, la crittografia completa dei dati AES-128-GCM, il supporto per l'accelerazione della crittografia Intel AES-NI e la prevenzione dagli attacchi man-in-the-middle all'integrità di pre-autenticazione. Replica archiviazione usa Kerberos AES256 per tutte le autenticazioni tra i nodi.  

* **Sincronizzazione iniziale ad alte prestazioni**. Replica archiviazione supporta la sincronizzazione iniziale con seeding, in cui un subset di dati esiste già in una destinazione proveniente da copie, backup o unità spedite meno recenti. La replica iniziale copierà solo i blocchi diversi, riducendo potenzialmente il tempo di sincronizzazione iniziale e impedendo che i dati sfruttino la larghezza di banda limitata. Il calcolo e l'aggregazione del checksum del blocco delle repliche di archiviazione fa sì che le prestazioni di sincronizzazione iniziale siano limitate solo dalla velocità dell'archiviazione e della rete.  

* **Gruppi di coerenza**. L'ordine della scrittura garantisce che le applicazioni come Microsoft SQL Server possano scrivere su più volumi replicati e siano a conoscenza che i dati verranno scritti in sequenza sul server di destinazione.  

* **Delega utente**. Gli utenti possono disporre di autorizzazioni delegate per gestire la replica senza essere membri del gruppo di amministratori nei nodi delegati, limitando quindi le loro possibilità di accesso ad aree non correlate.  

* **Vincolo di rete**. Replica archiviazione può essere limitata a singole reti in base al server e ai volumi replicati, in modo da offrire la larghezza di banda per il software di gestione, del backup e dell'applicazione.  

* **Thin provisioning**. Supporto per thin provisioning in dispositivi SAN e Spazi di archiviazione, per poter offrire una replica iniziale quasi istantanea in molte circostanze.  

Windows Server 2016 implementa le funzionalità seguenti in Replica di archiviazione:  

|Funzionalità|Dettagli|  
|-----------|-----------|  
|Type|Basata su host|  
|Sincrono|Sì|  
|Asincrona|Sì|  
|Indipendente dall'hardware di archiviazione|Sì|  
|Unità di replica|Volume (partizione)|  
|Creazione di cluster estesi di Windows Server|Sì|  
|Replica da server a server|Sì|  
|Replica da cluster a cluster|Sì|  
|Transport|SMB3|  
|Rete|TCP/IP o RDMA|
|Supporto per vincoli di rete|Sì|  
|RDMA*|iWARP, InfiniBand, RoCE v2|  
|Requisiti firewall per la porta di rete della replica|Porta IANA singola (TCP 445 o 5445)|  
|Multipath/multicanale|Sì (SMB3)|  
|Supporto per Kerberos|Sì (SMB3)|  
|Crittografia e firma tramite rete|Sì (SMB3)|  
|Failover in base al volume consentiti|Sì|
|Supporto di archiviazione con thin provisioning|Sì|
|Interfaccia utente di gestione inclusa|PowerShell, Gestione cluster di failover|  

*Potrebbe richiedere apparecchiature e cablaggi prolungati aggiuntivi.  

## <a name="BKMK_SR3"></a> Prerequisiti di Replica archiviazione  

* Foresta di Active Directory Domain Services.  
* Spazi di archiviazione con JBOD SAS, Spazi di archiviazione diretta, SAN fibre channel, VHDX condiviso, destinazione iSCSI o archiviazione SCSI/SAS o SATA locale. Unità SSD o unità più veloci consigliate per le unità di log della replica. Microsoft consiglia una velocità di archiviazione dei log superiore a quella dell'archiviazione dei dati. I volumi di log non devono essere utilizzati per altri carichi di lavoro. 
* Almeno una connessione Ethernet/TCP su ogni server per la replica sincrona, ma preferibilmente RDMA.   
* Almeno 2 GB di RAM e due core per server.  
* Una rete tra i server con larghezza di banda sufficiente per contenere il carico di lavoro di scrittura delle operazioni di I/O e una latenza media di andata e ritorno di 5 ms, o inferiore, per la replica sincrona. La replica asincrona non dispone di un'indicazione di latenza.  

##  <a name="BKMK_SR4"> </a> Background  
Questa sezione include informazioni sui termini di settore di alto livello, la replica sincrona e asincrona e i comportamenti chiave.
### <a name="high-level-industry-terms"></a>Termini di settore di alto livello  
Il termine ripristino di emergenza (DR) fa riferimento a un piano di emergenza per il ripristino da catastrofi del sito in modo che l'azienda continui a funzionare. Il ripristino di emergenza dei dati crea più copie dei dati di produzione in un percorso fisico separato. Ad esempio, un cluster esteso in cui metà dei nodi sono presenti in un sito e metà sono presenti in un altro. Il termine Preparazione alle emergenze (DP) fa riferimento a un piano di emergenza per lo spostamento preventivo dei carichi di lavoro in un altro percorso prima di un'emergenza incombente, ad esempio un uragano.  

Il termine Contratti di servizio (SLA) definisce la disponibilità delle applicazioni di un'azienda e la relativa tolleranza all'inattività e alla perdita di dati durante le interruzioni pianificate e non pianificate. Il termine Obiettivo del tempo di ripristino (RTO) definisce quanto tempo l'azienda è in grado di tollerare l'inaccessibilità totale ai dati. Il termine Obiettivo del punto di ripristino (RPO) definisce la quantità di dati che l'azienda può permettersi di perdere.  

### <a name="synchronous-replication"></a>Replica sincrona  
La replica sincrona garantisce che l'applicazione scriva i dati in due posizioni alla volta prima del completamento delle operazioni di I/O. Questa replica risulta più appropriata per i dati importanti, poiché richiede investimenti di archiviazione e di rete nonché un rischio di prestazioni ridotte delle applicazioni.  

Quando si verificano operazioni di scrittura dell'applicazione nella copia dei dati di origine, l'archiviazione di origine non riconosce immediatamente le operazioni di I/O. Al contrario, le modifiche ai dati replicano la copia di destinazione remota e restituiscono un riconoscimento. Solo a questo punto l'applicazione riceverà il riconoscimento delle operazioni di I/O. Ciò garantisce la costante sincronizzazione del sito remoto con il sito di origine, estendendo effettivamente l'archiviazione delle operazioni di I/O nella rete. In caso di errore del sito di origine, le applicazioni possono eseguire il failover al sito remoto e riprendere le operazioni con la garanzia di poter conservare tutti i dati.  

|Modalità|Diagramma|Passaggi|  
|--------|-----------|---------|  
|**Sincrono**<br /><br />Perdita di dati pari a zero<br /><br />RPO|![Diagramma che mostra come Replica di archiviazione scrive dati in modalità di replica sincrona](./media/Storage-Replica-Overview/Storage_SR_SynchronousV2.png)|1.  L'applicazione scrive i dati<br />2.  I dati del log vengono scritti e replicati nel sito remoto<br />3.  I dati del log vengono scritti nel sito remoto<br />4.  Riconoscimento dal sito remoto<br />5.  Scrittura dell'applicazione riconosciuta<br /><br />t & t1: dati scaricati nel volume, log scrivono sempre|  

### <a name="asynchronous-replication"></a>Replica asincrona  
Al contrario, la replica asincrona comporta che, quando l'applicazione scrive i dati, tali dati vengano replicati nel sito remoto senza garanzie di riconoscimento immediate. Questa modalità consente tempi di risposta più rapidi per l'applicazione, nonché una soluzione di ripristino di emergenza che funziona geograficamente.  

Quando l'applicazione scrive i dati, il motore di replica acquisisce la scrittura e invia immediatamente il riconoscimento all'applicazione. I dati acquisiti vengono quindi replicati al percorso remoto. Il nodo remoto elabora la copia dei dati e invia il riconoscimento in modo differito alla copia di origine. Poiché le prestazioni di replica non sono più nel percorso delle operazioni di I/O dell'applicazione, la velocità di risposta del sito remoto e la distanza sono fattori meno importanti. Vi è rischio di perdita di dati se i dati di origine vanno persi e la copia di destinazione dei dati è ancora in buffer senza aver lasciato l'origine.  

Con il suo RPO maggiore di zero, la replica asincrona è meno adatta per le soluzioni a disponibilità elevata come cluster di failover, perché sono progettate per operazioni continue con ridondanza e nessuna perdita dei dati.  

|Modalità|Diagramma|Passaggi|  
|--------|-----------|---------|  
|**Asincrona**<br /><br />Perdita di dati quasi pari a zero<br /><br />(dipende da vari fattori)<br /><br />RPO|![Diagramma che mostra come Replica di archiviazione scrive dati in modalità di replica asincrona](./media/Storage-Replica-Overview/Storage_SR_AsynchronousV2.png)|1.  L'applicazione scrive i dati<br />2.  Dati del log scritti<br />3.  Scrittura dell'applicazione riconosciuta<br />4.  Dati replicati al sito remoto<br />5.  Dati del log scritti nel sito remoto<br />6.  Riconoscimento dal sito remoto<br /><br />t & t1: dati scaricati nel volume, log scrivono sempre|  

### <a name="key-evaluation-points-and-behaviors"></a>Punti di valutazione e comportamenti principali  

-   Larghezza di banda e latenza della rete con archiviazione più rapida. Esistono limiti fisici per la replica sincrona. Poiché Replica di archiviazione implementa un meccanismo di filtro delle operazioni di I/O che usa i log e richiede round trip della rete, è probabile che la replica sincrona comporti una riduzione della velocità di scrittura dell'applicazione. Usando una latenza bassa, reti con una larghezza di banda elevata e sottosistemi di dischi ad alta velocità per i log, gli overhead delle prestazioni verranno ridotti al minimo.  

-   Il volume di destinazione non è accessibile durante la replica in Windows Server 2016. Quando si configura la replica, il volume di destinazione si smonta, rendendolo inaccessibile a letture o scritture da parte degli utenti. La lettera dell'unità potrebbe essere visibile in interfacce tipiche, ad esempio Esplora File, ma un'applicazione non può accedere al volume stesso. Le tecnologie di replica a livello di blocco non sono compatibili con l'accesso al file system montato della destinazione in un volume. NTFS e ReFS non supportano la scrittura dei dati nel volume mentre i blocchi cambiano al di sotto di essi. 

In Windows Server, versione 1709 è stata aggiunta la nuova opzione Failover di test. È ora supportato il montaggio temporaneo di uno snapshot di lettura/scrittura del volume di destinazione a scopo di backup, test e altro. Per altre informazioni, vedi https://aka.ms/srfaq.

-   L'implementazione Microsoft della replica asincrona è diversa rispetto alla maggior parte delle implementazioni. La maggior parte delle implementazioni della replica asincrona nel settore si basano sulla replica basata su snapshot, in cui trasferimenti differenziali periodici si muovono sull'altro nodo e si uniscono. La replica di Replica archiviazione asincrona funziona come la replica sincrona, ad eccezione del fatto che elimina la necessità di un riconoscimento sincrono serializzato dalla destinazione. Ciò significa che Replica di archiviazione possiede teoricamente un RPO inferiore, poiché esegue continuamente la replica. Tuttavia, ciò significa che si basa su garanzie di coerenza interne dell'applicazione invece di usare gli snapshot per forzare la coerenza nei file dell'applicazione. Replica di archiviazione assicura una coerenza per arresto anomalo del sistema in tutte le modalità di replica  

-   Molti clienti usano Replica DFS come soluzione di ripristino di emergenza anche se è spesso poco pratica per tale scenario. Replica DFS non può infatti replicare i file aperti ed è progettata per ridurre al minimo l'uso della larghezza di banda a scapito delle prestazioni, causando ampi delta del punto di ripristino. Replica di archiviazione può consentire di ritirare Replica DFS da alcune di queste attività di ripristino di emergenza.  

-   Replica archiviazione non è un backup. Alcuni ambienti IT distribuiscono i sistemi di replica come soluzioni di backup, a causa delle loro perdita di dati pari a zero rispetto ai backup giornalieri. Replica di archiviazione replica tutte le modifiche a tutti i blocchi di dati nel volume, indipendentemente dal tipo di modifica. Se un utente elimina tutti i dati da un volume, Replica archiviazione replicherà immediatamente l'eliminazione nell'altro volume, rimuovendo definitivamente i dati da entrambi i server. Non usare Replica di archiviazione come sostituzione per una soluzione di backup temporizzata.  

-   Replica archiviazione non è la replica Hyper-V e non costituisce i gruppi di disponibilità AlwaysOn di Microsoft SQL. Replica archiviazione è un motore indipendente dall'archiviazione per scopi generali. Per definizione, non è possibile personalizzarne il comportamento come per la replica a livello di applicazione. Ciò potrebbe causare gap di funzionalità specifici che invitano a distribuire o a rimanere sulle tecnologie di replica specifiche dell'applicazione.  

> [!NOTE]
> Questo documento contiene un elenco di [problemi noti](storage-replica-known-issues.md) e comportamenti previsti nonché la sezione [Domande frequenti](storage-replica-frequently-asked-questions.md).
 
### <a name="storage-replica-terminology"></a>Terminologia relativa a Replica archiviazione  
Questa guida usa frequentemente i termini seguenti:  

-   L'origine è il volume del computer che consente scritture locali e che esegue la replica in uscita. Nota anche come "primaria".  

-   La destinazione è il volume del computer che non consente scritture locali e che esegue in ingresso. Nota anche come "secondaria".   

-   Una relazione di replica è la relazione di sincronizzazione tra un computer di origine e di destinazione per uno o più volumi e usa un singolo registro.  

-   Un gruppo di replica è l'organizzazione dei volumi e la relativa configurazione di replica all'interno di una relazione, in base al server. Un gruppo può contenere uno o più volumi.  

## <a name="see-also"></a>Vedere anche
- [Replica di un cluster esteso tramite l'archiviazione condivisa](stretch-cluster-replication-using-shared-storage.md)  
- [Replica di archiviazione da server a server](server-to-server-storage-replication.md)  
- [Cluster to Cluster Storage Replication (Replica archiviazione da cluster a cluster)](cluster-to-cluster-storage-replication.md)  
- [Replica archiviazione: Problemi noti](storage-replica-known-issues.md)  
- [Replica archiviazione: Domande frequenti](storage-replica-frequently-asked-questions.md)  
- [Spazi di archiviazione diretta in Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
