---
title: Domande frequenti su Replica archiviazione
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 12/19/2018
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: 0e010f0319b46e04cf9aa15cde9552af1191ab22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824712"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Domande frequenti su Replica archiviazione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento contiene le risposte alle domande frequenti su Replica archiviazione.

## <a name="FAQ1"></a> Replica archiviazione è supportata in Azure?  
Sì. Con Azure, è possibile usare gli scenari seguenti:

1. Replica da server a server all'interno di Azure (in modo sincrono o in modo asincrono tra le macchine virtuali IaaS in uno o due domini di errore di Data Center o in modo asincrono tra due aree distinte)
2. Server-to-server replica asincrona tra Azure e in locale (tramite Azure ExpressRoute o VPN)
3. Replica da cluster a cluster all'interno di Azure (in modo sincrono o in modo asincrono tra le macchine virtuali IaaS in uno o due domini di errore di Data Center o in modo asincrono tra due aree distinte)
4. Cluster a cluster replica asincrona tra Azure e in locale (tramite Azure ExpressRoute o VPN)

Altre note sul clustering guest in Azure sono reperibile in: [Distribuzione di cluster Guest di VM IaaS in Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

Note importanti:

1. Azure non supporta clustering guest con VHDX condiviso in modo che le macchine virtuali del Cluster di Failover di Windows è necessario usare destinazioni iSCSI per il clustering di prenotazione classico dischi persistenti di archiviazione condivisa o spazi di archiviazione diretta.
2. Sono disponibili modelli di Azure Resource Manager per il clustering di spazi di archiviazione diretta basati su Replica archiviazione in [creare un cluster SOFS spazi di archiviazione è diretta (S2D) con Replica archiviazione per il ripristino di emergenza tra aree di Azure](https://aka.ms/azure-storage-replica-cluster).  
3. Cluster per la comunicazione RPC del cluster in Azure (obbligatorio per le API del cluster per la concessione dell'accesso tra cluster) richiede la configurazione di accesso alla rete per l'oggetto nome cluster. È necessario consentire la porta TCP 135 e l'intervallo dinamico sopra la porta TCP 49152. Riferimento [Building Windows Server Failover Clustering in Azure IAAS VM – Part 2 rete e la creazione](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/).  
4. È possibile usare i cluster guest a due nodi, dove ogni nodo Usa iSCSI di loopback per un cluster asimmetrico replicato tramite Replica archiviazione. Ma è probabile che siano molto una riduzione delle prestazioni e deve essere usato solo per i carichi di lavoro molto limitate o il test.  

## <a name="FAQ2"></a> Come è possibile visualizzare lo stato di avanzamento della replica durante la sincronizzazione iniziale?  
I messaggi di Evento 1237 visualizzati nel log degli eventi dell'amministratore di Replica di archiviazione nel server di destinazione mostrano il numero di byte copiati e i byte rimanenti ogni 10 secondi. È inoltre possibile usare il contatore delle prestazioni di Replica archiviazione sulla destinazione **\Statistiche Replica archiviazione\Byte totali ricevuti** per uno o più volumi replicati. È inoltre possibile effettuare una query sul gruppo di replica tramite Windows PowerShell. Ad esempio, questo comando di esempio ottiene il nome dei gruppi nella destinazione, quindi esegue una query di un gruppo denominato **Replica 2** ogni 10 secondi per mostrare lo stato di avanzamento:  

```  
Get-SRGroup

do{
    $r=(Get-SRGroup -Name "Replication 2").replicas
    [System.Console]::Write("Number of remaining bytes {0}`n", $r.NumOfBytesRemaining)
    Start-Sleep 10
}until($r.ReplicationStatus -eq 'ContinuouslyReplicating')
Write-Output "Replica Status: "$r.replicationstatus

```  

## <a name="FAQ3"></a>È possibile specificare le interfacce di rete specifico da utilizzare per la replica?  

Sì, usando `Set-SRNetworkConstraint`. Questo cmdlet opera a livello di interfaccia e viene usato in scenari con cluster e senza cluster.  
Ad esempio, con un server autonomo (in ogni nodo):  

```  
Get-SRPartnership  

Get-NetIPConfiguration  
```  
Prendere nota delle informazioni sul gateway e l'interfaccia (su entrambi i server) e le direzioni della relazione. Eseguire quindi:  

```  
Set-SRNetworkConstraint -SourceComputerName sr-srv06 -SourceRGName rg02 -  
SourceNWInterface 2 -DestinationComputerName sr-srv05 -DestinationNWInterface 3 -DestinationRGName rg01  

Get-SRNetworkConstraint  

Update-SmbMultichannelConnection  

```  

Per i vincoli della rete di configurazione in un cluster esteso:  

    Set-SRNetworkConstraint -SourceComputerName sr-srv01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-srv03 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"  

## <a name="FAQ4"></a> È possibile configurare la replica uno-a-molti o transitiva (da A B a C)?  
Non in Windows Server 2016. Questa versione supporta solo la replica uno a uno di un server, un cluster o un nodo del cluster esteso. Questo limite potrebbe venire superato nelle versioni successive. Naturalmente, è possibile configurare la replica tra i vari server di una coppia di volumi specifica, in entrambe le direzioni. Ad esempio, Server 1 può replicare il volume D su Server 2 e il volume E su Server 3.

## <a name="FAQ5"></a> È possibile aumentare o ridurre i volumi replicati tramite Replica archiviazione?  
Puoi espandere (estendere) i volumi, ma non ridurli. Per impostazione predefinita, Replica di archiviazione impedisce agli amministratori di estendere i volumi replicati. Utilizza l'opzione `Set-SRGroup -AllowVolumeResize $TRUE` nel gruppo di origine, prima del ridimensionamento. Ad esempio: 

1. Usare in base al computer di origine: `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Espandere il volume utilizzando la tecnica preferita
3. Usare in base al computer di origine: `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>È possibile trasferire un volume di destinazione per l'accesso di sola lettura?  
Non in Windows Server 2016 RTM, ovvero la cosiddetta versione "RS1". Replica archiviazione smonta il volume di destinazione quando inizia la replica. 

In Windows Server, versione 1709 è ora tuttavia possibile montare l'archiviazione di destinazione, grazie alla funzionalità denominata "Failover di test". A tale scopo, è necessario disporre di un volume NTFS o ReFS inutilizzato di cui non viene attualmente eseguita la replica nella destinazione. È quindi possibile montare temporaneamente uno snapshot dell'archiviazione replicata a scopo di test o backup. 

Ad esempio, per creare un failover di test quando si esegue la replica di un volume "D:" nel gruppo di replica "RG2" nel server di destinazione "SRV2" e si dispone di un'unità "T:" in SRV2 di cui non viene eseguita la replica:

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
Il volume replicato D: è ora accessibile in SRV2. È possibile leggere e scrivere in genere, copiare i file sposterà o eseguire un backup online per maggiore sicurezza, nel percorso di unità d: salvate in un' posizione. Oggetto t: volume conterrà solo i dati di log.

Per rimuovere lo snapshot del failover di test e annullare le modifiche:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Utilizzare la funzionalità per il failover di test solo per le operazioni temporanee a breve termine, in quanto non è destinata all'utilizzo a lungo termine. Quando è in uso, la replica continua per il volume di destinazione reale. 

## <a name="FAQ7"></a> È possibile configurare File Server di scalabilità orizzontale (SOFS) in un cluster esteso?  
Anche se tecnicamente possibile, non è una configurazione consigliata in Windows Server 2016 a causa della mancanza di riconoscimento dei siti nei nodi di calcolo che contattano SOFS. Se si usa rete distanza campus, dove le latenze sono in genere inferiori al millisecondo, questa configurazione è in genere funziona senza problemi.   

Se si configura una replica da cluster a cluster, Replica archiviazione supporta completamente i File server di scalabilità orizzontale, inclusi l'uso di Spazi di archiviazione diretta, durante la replica tra due cluster.  

## <a name="FAQ7.5"></a> CSV è necessario eseguire la replica in un cluster esteso o tra cluster?  
No. È possibile replicare con CSV o prenotazioni di disco persistente (democratica del Laos) appartenente a una risorsa cluster, ad esempio un ruolo File Server. 

Se si configura una replica da cluster a cluster, Replica archiviazione supporta completamente i File server di scalabilità orizzontale, inclusi l'uso di Spazi di archiviazione diretta, durante la replica tra due cluster.  

## <a name="FAQ8"></a>È possibile configurare spazi di archiviazione diretta in un cluster esteso con Replica archiviazione?  
Questa configurazione non è supportata in Windows Server 2016.  Questo limite potrebbe venire superato nelle versioni successive. Se si configura una replica da cluster a cluster, Replica archiviazione supporta completamente i File server di scalabilità orizzontale e i server Hyper-V, inclusi l'uso di Spazi di archiviazione diretta.  

## <a name="FAQ9"></a>Come si configura la replica asincrona?  

Specificare `New-SRPartnership -ReplicationMode` e fornire l'argomento **Asincrono**. Per impostazione predefinita, tutte le repliche in Replica archiviazione sono sincrone. È inoltre possibile modificare la modalità con `Set-SRPartnership -ReplicationMode`.  

## <a name="FAQ10"></a>Come si può evitare il failover automatico di un cluster esteso?  
Per evitare il failover automatico, è possibile usare PowerShell per configurare `Get-ClusterNode -Name "NodeName").NodeWeight=0`. In questo modo si rimuove il voto in ogni nodo del sito di ripristino di emergenza. È quindi possibile usare `Start-ClusterNode -PreventQuorum` sui nodi nel sito primario e `Start-ClusterNode -ForceQuorum` sui nodi nel sito di emergenza per forzare il failover. Non esiste alcuna opzione grafica per evitare il failover automatico e in ogni caso non è consigliabile evitare il failover automatico.  

## <a name="FAQ11"></a>Come si disabilita una resilienza della macchina virtuale?  
Per evitare che la nuova funzionalità di resilienza macchina virtuale di Hyper-V da in esecuzione e pertanto posizionando le macchine virtuali invece di provocarne il failover al sito di ripristino di emergenza, eseguire `(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a> Come si riduce la durata per la sincronizzazione iniziale?  

È possibile usare l'archiviazione con thin provisioning per accelerare i tempi di sincronizzazione iniziali. Replica archiviazione esegue una query e usa automaticamente l'archiviazione con thin provisioning, compresi gli Spazi di archiviazione non cluster, i dischi dinamici Hyper-V e i LUN SAN.  

È anche possibile usare i volumi dei dati di seeding per ridurre l'utilizzo della larghezza di banda e talvolta di tempo, garantendo che la destinazione volume dispone di un subset dei dati dal database primario quindi usando l'opzione con seeding in Gestione Cluster di Failover o `New-SRPartnership`. Se il volume è prevalentemente vuoto, la sincronizzazione con seeding può ridurre la durata e l'uso della larghezza di banda. Esistono diversi modi per dati di seeding, con diversi livelli di efficienza in termini di:

1. Precedente replica - tramite la replica iniziale normale sincronizzazione in locale tra i nodi che contiene i dischi e volumi, rimozione della replica, spedizione dei dischi di destinazione in un' posizione, quindi l'aggiunta di replica con l'opzione di seeding. Si tratta del metodo più efficace come Replica di archiviazione garantiti un mirror copia in blocco e blocchi delta non è l'unica cosa da replicare.
2. Ripristino dello snapshot o il ripristino basato su snapshot di backup - tramite il ripristino di uno snapshot in base al volume nel volume di destinazione, dovrebbe esserci delle differenze minime tra il layout di blocco. Si tratta del metodo più efficace successivo come i blocchi sono probabilmente in modo che corrispondano grazie alle istantanee di volume da immagini speculari.
3. I file copiati - per la creazione di un nuovo volume nella destinazione non è mai stata usata prima e l'esecuzione di una struttura ad albero /MIR robocopy completo copia dei dati, esistono probabilmente corrispondenze di blocco. Usando Esplora File di Windows o la copia di una parte dell'albero non creerà numero elevato di corrispondenze di blocco. La copia manuale dei file è il metodo meno efficace del seeding.



## <a name="FAQ13"></a> È possibile delegare agli utenti di amministrare la replica?  

È possibile usare il cmdlet `Grant-SRDelegation` in Windows Server 2016. Ciò consente di impostare utenti specifici in scenari di replica da server a server, da cluster a cluster e con cluster esteso che abbiano le autorizzazioni per creare, modificare o rimuovere la replica, pur non essendo un membro del gruppo di amministratori locale. Ad esempio:   

    Grant-SRDelegation -UserName contso\tonywang  

Il cmdlet ricorderà che l'utente deve disconnettersi e riconnettersi al server che intende amministrare affinché le modifiche abbiano effetto. È possibile usare `Get-SRDelegation` e `Revoke-SRDelegation` per controllare ulteriormente questa opzione.  

## <a name="FAQ13"></a> Quali sono il backup e ripristinare le opzioni per i volumi replicati?
Replica archiviazione supporta il backup e il ripristino del volume di origine. Supporta inoltre la creazione e il ripristino degli snapshot del volume di origine. Non è possibile eseguire il backup o ripristinare il volume di destinazione quando è protetto da Replica archiviazione, in quanto non è montato né accessibile. Se si verifica una situazione di emergenza in cui il volume di origine viene perso, usando `Set-SRPartnership` per fare in modo che il volume di destinazione precedente venga promosso a origine di lettura/scrittura sarà possibile eseguire il backup o il ripristino di tale volume. È inoltre possibile rimuovere la replica con `Remove-SRPartnership` e `Remove-SRGroup` per rimontare tale volume come accessibile in lettura/scrittura.
Per creare snapshot periodici coerenti con l'applicazione, è possibile usare VSSADMIN.EXE sul server di origine per eseguire uno snapshot dei volumi di dati replicati. Ad esempio, se si sta replicando il volume F: con Replica archiviazione:

    vssadmin create shadow /for=F:
Dopo aver modificato la direzione di replica, rimosso la replica o quando ci si trova semplicemente nello stesso volume di origine, è possibile ripristinare eventuali snapshot al momento in cui sono stati eseguiti. Ad esempio, usando ancora F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
È inoltre possibile pianificare questo strumento in modo che venga eseguito periodicamente tramite un'attività pianificata. Per altre informazioni sull'uso del Servizio Copia Shadow del volume, vedere [Vssadmin](https://technet.microsoft.com/library/cc754968.aspx). Il backup di volumi di registro non richiede azioni o valori particolari. Il tentativo di eseguire questa operazione verrà ignorato da Servizio Copia Shadow del volume.
L'uso di Windows Server Backup, backup di Microsoft Azure, Microsoft DPM o altri snapshot, Servizio Copia Shadow del volume, macchina virtuale o tecnologie basate su file è supportato da Replica archiviazione purché questi servizi operino all'interno del livello del volume. Replica archiviazione non supporta il backup e il ripristino basati su blocchi.

## <a name="FAQ14"></a> È possibile configurare la replica per limitare l'utilizzo della larghezza di banda?
Sì, tramite il limitatore di larghezza di banda SMB. Si tratta di un'impostazione globale per tutto il traffico di Replica archiviazione e pertanto influisce su tutte le repliche dal server. In genere, è necessario solo con la configurazione della sincronizzazione iniziale di Replica archiviazione, in cui è necessario trasferire tutti i dati del volume. Se necessaria dopo la sincronizzazione iniziale, la larghezza di banda di rete è troppo bassa per il carico di lavoro delle operazioni di I/O. Ridurre le operazioni di I/O o aumentare la larghezza di banda.

Questa limitazione deve essere usata solo con la replica asincrona (nota: la sincronizzazione iniziale è sempre asincrona anche se è stata specificata l'opzione sincrona).
È anche possibile usare i criteri di rete QoS per dare forma al traffico di Replica archiviazione. L'uso di una replica di Replica archiviazione con seeding a corrispondenza elevata ridurrà notevolmente l'uso complessivo della larghezza di banda per la sincronizzazione iniziale.


Per impostare il limite della larghezza di banda, usare:

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Per visualizzare il limite della larghezza di banda, usare:

    Get-SmbBandwidthLimit -Category StorageReplication

Per rimuovere il limite della larghezza di banda, usare:

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>Quali porte di rete richiede la Replica di archiviazione?
Replica di archiviazione si basa su SMB e WSMAN per la replica e la gestione. Questo significa che sono necessarie le seguenti porte:

 445 (SMB - protocollo di trasporto di replica, protocollo di gestione di cluster RPC) 5445 (SMB - necessario solo quando si usa la rete RDMA iWARP iWARP) 5985 (WSManHTTP - protocollo di gestione per WMI/CIM/PowerShell)

Nota: Il cmdlet Test-SRTopology richiede ICMPv4 o ICMPv6, ma non per la replica o gestione.

## <a name="FAQ15.5"></a>Quali sono le procedure ottimali di volume di log?
Le dimensioni ottimale del log variano notevolmente per ogni ambiente e del carico di lavoro e viene determinata dalla quantità di scrittura i/o il carico di lavoro esegue. 

1.  Un log più grande o più piccolo non determina una velocità superiore o inferiore
2.  Un log più grande o più piccolo non ha alcuna influenza su un volume di dati di 10 GB rispetto a un volume di dati di 10 TB

Un log più grande può semplicemente raccogliere e conservare più operazioni di I/O di scrittura prima che vengano estratte. Questo consente il protrarsi di un'interruzione nel servizio tra il computer di origine e di destinazione, ad esempio un'interruzione di rete o una destinazione offline. Se il log può contenere 10 ore di scritture e la rete rimane inattiva per 2 ore, non appena quest'ultima tornerà attiva l'origine potrà semplicemente riprodurre molto rapidamente i delta delle modifiche non sincronizzate nella destinazione ripristinando altrettanto rapidamente la protezione. Se il log contiene 10 ore e il periodo di inattività è di 2 giorni, l'origine deve eseguire la riproduzione da un altro log denominato bitmap e la sincronizzazione sarà probabilmente più lenta. Una volta eseguita la sincronizzazione, tornerà a utilizzare il log.

Replica archiviazione si basa sul log per tutte le prestazioni di scrittura. Registrare le prestazioni critiche per le prestazioni di replica. È necessario assicurarsi che il volume di log offra prestazioni migliori rispetto al volume di dati, in quanto il log serializzerà e sequenzializzerà tutte le operazioni di I/O di scrittura. È necessario utilizzare sempre supporti flash come unità SSD nei volumi di log. Non consentire l'esecuzione di eventuali altri carichi di lavoro nel volume di log, né nei volumi di log del database SQL. 

di nuovo: Microsoft consiglia che l'archiviazione dei log può risultare più efficienti l'archiviazione dei dati e che i volumi di log non devono essere utilizzati per altri carichi di lavoro.

È possibile ottenere consigli sul ridimensionamento del log eseguendo lo strumento di Test-SRTopology. In alternativa, è possibile usare i contatori delle prestazioni nei server esistenti per rendere un giudizio di dimensioni del log. La formula è semplice: monitorare la velocità effettiva del disco dati (Media byte scritti/Sec) sotto il carico di lavoro e usarla per calcolare la quantità di tempo necessario per riempire il log di dimensioni diverse. Ad esempio, velocità effettiva del disco dati di 50 MB/s determinerà il log di 120GB per eseguire il wrapping in secondi a 120GB o 50MB o 2400 secondi o minuti 40. Pertanto, la quantità di tempo che il server di destinazione potrebbe non essere raggiungibile prima del log eseguito il wrapping è 40 minuti. Se il log esegue il wrapping, ma la destinazione diventa nuovamente raggiungibile, l'origine sarebbe riprodurre blocchi tramite il log di mappa di bit anziché il log principale. Le dimensioni del log non hanno un effetto sulle prestazioni.

SOLO il disco dati dal cluster di origine deve essere eseguito il backup. I dischi di archiviazione Log della Replica devono non essere sottoposti a backup poiché una copia di backup può essere in conflitto con le operazioni di Replica di archiviazione.

## <a name="FAQ16"></a> Il motivo per cui è preferibile scegliere un cluster esteso rispetto a cluster a cluster rispetto al server topologia?  
Replica archiviazione è disponibile in tre configurazioni principali: cluster esteso, da cluster a cluster e da server a server. Ognuna presenta vantaggi diversi.

La topologia del cluster esteso è ideale per i carichi di lavoro che richiedono il failover automatico con orchestrazione, ad esempio cluster di cloud privato Hyper-V e istanze del cluster di failover di SQL Server. Include anche un'interfaccia grafica incorporata che utilizza Gestione cluster di failover. Usa la classica architettura di archiviazione condivisa del cluster asimmetrico di Spazi di archiviazione, SAN, iSCSI e RAID tramite prenotazione permanente. Può essere eseguita con soli 2 nodi.

La topologia da cluster a cluster usa due cluster distinti ed è ideale per gli amministratori che desiderano un failover manuale, soprattutto quando viene effettuato il provisioning del secondo sito per il ripristino di emergenza e l'utilizzo non quotidiano. L'orchestrazione è manuale. A differenza dei cluster esteso, spazi di archiviazione diretta può essere utilizzato in questa configurazione (con alcune avvertenze - vedere le domande frequenti sulla Replica di archiviazione e la documentazione del cluster a cluster). È possibile eseguirlo con soli quattro nodi. 

La topologia da server a server è ideale per i clienti che eseguono hardware che non può essere incluso nel cluster. Richiede l'orchestrazione e il failover manuali. È ideale per le distribuzioni economiche tra succursali e data center centrali, soprattutto quando si utilizza la replica asincrona. Questa configurazione può sostituire spesso le istanze di file server protetti da DFSR utilizzati per scenari di ripristino di emergenza a master singolo.

In tutti i casi, le topologie supportano l'esecuzione sia su hardware fisico sia su macchine virtuali. In caso di macchine virtuali, l'hypervisor sottostante non richiede Hyper-V, ma VMware, KVM, Xen e così via.

Replica archiviazione include anche una modalità da server a sé stesso, dove la replica interesserà due volumi diversi nello stesso computer.

## <a name="FAQ18"></a> La deduplicazione dei dati è supportata con Replica archiviazione?

Sì, Deduplcation dati è supportata con la Replica di archiviazione. Abilitare la deduplicazione dei dati in un volume nel server di origine e durante la replica il server di destinazione riceve una copia del volume deduplicata.

Mentre dovrebbero *installare* la deduplicazione dei dati nei server di origine e di destinazione (vedere [installazione e abilitare deduplicazione dati](../data-deduplication/install-enable.md)), è importante non a *abilitare*La deduplicazione dei dati nel server di destinazione. Replica archiviazione consente operazioni di scrittura solo nel server di origine. Poiché la deduplicazione dei dati semplifica operazioni di scrittura nel volume, deve essere eseguito solo sul server di origine. 

## <a name="FAQ17"></a> Come segnalare un problema con la Replica di archiviazione o questa Guida?  
Per assistenza tecnica relativa a Replica di archiviazione, è possibile pubblicare un post nei [forum di Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). È inoltre possibile inviare un'e-mail all'indirizzo srfeed@microsoft.com per domande su Replica archiviazione o problemi relativi a questa documentazione. Il https://windowsserver.uservoice.com sito diventi preferito per le richieste di modifica di progettazione, in quanto consente ai clienti fornire commenti e suggerimenti e supporto per le tue idee.



## <a name="related-topics"></a>Argomenti correlati  
- [Panoramica di Replica di archiviazione](storage-replica-overview.md) 
- [Replica di Cluster esteso tramite l'archiviazione condivisa](stretch-cluster-replication-using-shared-storage.md)  
- [Replica di archiviazione da server a Server](server-to-server-storage-replication.md)  
- [Replica di archiviazione da cluster a Cluster](cluster-to-cluster-storage-replication.md)  
- [Replica archiviazione: Problemi noti](storage-replica-known-issues.md)  

## <a name="see-also"></a>Vedere anche  
- [Panoramica dell'archiviazione](../storage.md)  
- [Spazi di archiviazione diretta in Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
