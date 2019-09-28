---
title: Domande frequenti su Replica archiviazione
ms.prod: windows-server
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: d369e7f2d3d725fe8b3871fea199da1cac044456
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393797"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Domande frequenti su Replica archiviazione

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

Questo argomento contiene le risposte alle domande frequenti su Replica archiviazione.

## <a name="FAQ1"></a>Replica archiviazione è supportata in Azure?
Sì. Con Azure è possibile usare gli scenari seguenti:

1. Replica da server a server all'interno di Azure (in modo sincrono o asincrono tra macchine virtuali IaaS in uno o due domini di errore del Data Center o in modo asincrono tra due aree separate)
2. Replica asincrona da server a server tra Azure e in locale (tramite VPN o Azure ExpressRoute)
3. Replica da cluster a cluster all'interno di Azure (in modo sincrono o asincrono tra macchine virtuali IaaS in uno o due domini di errore del Data Center o in modo asincrono tra due aree separate)
4. Replica asincrona da cluster a cluster tra Azure e l'ambiente locale (tramite VPN o Azure ExpressRoute)

Altre note sul clustering guest in Azure sono disponibili all'indirizzo: [Distribuzione di cluster guest di macchine virtuali IaaS in Microsoft Azure](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

Note importanti:

1. Azure non supporta il clustering guest VHDX condiviso, quindi le macchine virtuali del cluster di failover di Windows devono usare destinazioni iSCSI per il clustering di prenotazione disco permanente con archiviazione condivisa classica o Spazi di archiviazione diretta.
2. Sono disponibili modelli di Azure Resource Manager per il clustering di replica archiviazione basata su Spazi di archiviazione diretta in [creare un spazi di archiviazione diretta cluster SOFS con replica archiviazione per il ripristino di emergenza tra aree di Azure](https://aka.ms/azure-storage-replica-cluster).  
3. La comunicazione RPC da cluster a cluster in Azure (richiesta dalle API del cluster per la concessione dell'accesso tra cluster) richiede la configurazione dell'accesso alla rete per oggetto nome cluster. È necessario consentire la porta TCP 135 e l'intervallo dinamico sopra la porta TCP 49152. Informazioni [di riferimento sulla creazione di cluster di failover di Windows Server in Azure IaaS VM: parte 2 rete e creazione](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/).  
4. È possibile usare cluster guest a due nodi, in cui ogni nodo utilizza iSCSI loopback per un cluster asimmetrico replicato da replica archiviazione. Questa operazione, tuttavia, avrà probabilmente una riduzione delle prestazioni e deve essere usata solo per carichi di lavoro o test molto limitati.  

## <a name="FAQ2"></a>Ricerca per categorie visualizzare lo stato di avanzamento della replica durante la sincronizzazione iniziale?  
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

## <a name="FAQ3"></a>È possibile specificare interfacce di rete specifiche da usare per la replica?  

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

    Set-SRNetworkConstraint -SourceComputerName sr-cluster01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-cluster02 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"

## <a name="FAQ4"></a>È possibile configurare la replica uno-a-molti o transitiva (da a a B a C)?  
No, replica archiviazione supporta solo una replica di un nodo server, cluster o cluster esteso. Questo limite potrebbe venire superato nelle versioni successive. Naturalmente, è possibile configurare la replica tra i vari server di una coppia di volumi specifica, in entrambe le direzioni. Ad esempio, Server 1 può replicare il volume D su Server 2 e il volume E su Server 3.

## <a name="FAQ5"></a>È possibile aumentare o ridurre I volumi replicati replicati dalla replica di archiviazione?  
Puoi espandere (estendere) i volumi, ma non ridurli. Per impostazione predefinita, Replica di archiviazione impedisce agli amministratori di estendere i volumi replicati. Utilizza l'opzione `Set-SRGroup -AllowVolumeResize $TRUE` nel gruppo di origine, prima del ridimensionamento. Esempio:

1. Utilizzare nel computer di origine:`Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Espandere il volume utilizzando la tecnica preferita
3. Utilizzare nel computer di origine:`Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>È possibile portare online un volume di destinazione per l'accesso in sola lettura?  
Non in Windows Server 2016. Replica archiviazione smonta il volume di destinazione quando inizia la replica. 

Tuttavia, in Windows Server 2019 e Windows Server un canale semestrale che inizia con la versione 1709, è ora possibile montare l'archiviazione di destinazione. questa funzionalità è denominata "failover di test". A tale scopo, è necessario disporre di un volume NTFS o ReFS inutilizzato di cui non viene attualmente eseguita la replica nella destinazione. È quindi possibile montare temporaneamente uno snapshot dell'archiviazione replicata a scopo di test o backup. 

Ad esempio, per creare un failover di test quando si esegue la replica di un volume "D:" nel gruppo di replica "RG2" nel server di destinazione "SRV2" e si dispone di un'unità "T:" in SRV2 di cui non viene eseguita la replica:

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
Il volume replicato D: è ora accessibile in SRV2. È possibile leggere e scrivere in un file in modo normale, copiarne i file o eseguire un backup online salvato altrove per la sicurezza, nel percorso D:. Il volume T: conterrà solo i dati di log.

Per rimuovere lo snapshot del failover di test e annullare le modifiche:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Utilizzare la funzionalità per il failover di test solo per le operazioni temporanee a breve termine, in quanto non è destinata all'utilizzo a lungo termine. Quando è in uso, la replica continua per il volume di destinazione reale. 

## <a name="FAQ7"></a>È possibile configurare il file server di scalabilità orizzontale (SOFS) in un cluster esteso?  
Sebbene sia tecnicamente possibile, questa non è una configurazione consigliata a causa della mancanza di riconoscimento dei siti nei nodi di calcolo che contattano SOFS. Se si usa la rete a distanza campus, dove le latenze sono in genere inferiori al millisecondo, questa configurazione funziona in genere senza problemi.   

Se si configura una replica da cluster a cluster, Replica archiviazione supporta completamente i File server di scalabilità orizzontale, inclusi l'uso di Spazi di archiviazione diretta, durante la replica tra due cluster.  

## <a name="FAQ7.5"></a>CSV è necessario per la replica in un cluster esteso o tra cluster?  
No. È possibile eseguire la replica con il formato CSV o la prenotazione del disco permanente (PDR) di proprietà di una risorsa cluster, ad esempio un ruolo di file server. 

Se si configura una replica da cluster a cluster, Replica archiviazione supporta completamente i File server di scalabilità orizzontale, inclusi l'uso di Spazi di archiviazione diretta, durante la replica tra due cluster.  

## <a name="FAQ8"></a>È possibile configurare Spazi di archiviazione diretta in un cluster esteso con replica archiviazione?  
Questa configurazione non è supportata in Windows Server. Questo limite potrebbe venire superato nelle versioni successive. Se si configura una replica da cluster a cluster, Replica archiviazione supporta completamente i File server di scalabilità orizzontale e i server Hyper-V, inclusi l'uso di Spazi di archiviazione diretta.  

## <a name="FAQ9"></a>Ricerca per categorie configurare la replica asincrona?  

Specificare `New-SRPartnership -ReplicationMode` e fornire l'argomento **Asincrono**. Per impostazione predefinita, tutte le repliche in Replica archiviazione sono sincrone. È inoltre possibile modificare la modalità con `Set-SRPartnership -ReplicationMode`.  

## <a name="FAQ10"></a>Ricerca per categorie impedire il failover automatico di un cluster esteso?  
Per evitare il failover automatico, è possibile usare PowerShell per configurare `Get-ClusterNode -Name "NodeName").NodeWeight=0`. In questo modo si rimuove il voto in ogni nodo del sito di ripristino di emergenza. È quindi possibile usare `Start-ClusterNode -PreventQuorum` sui nodi nel sito primario e `Start-ClusterNode -ForceQuorum` sui nodi nel sito di emergenza per forzare il failover. Non esiste alcuna opzione grafica per evitare il failover automatico e in ogni caso non è consigliabile evitare il failover automatico.  

## <a name="FAQ11"></a>Ricerca per categorie disabilitare la resilienza della macchina virtuale?
Per impedire l'esecuzione della nuova funzionalità di resilienza delle macchine virtuali Hyper-V e quindi sospendere le macchine virtuali invece di eseguirne il failover nel sito di ripristino di emergenza, eseguire`(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a>Come è possibile ridurre il tempo per la sincronizzazione iniziale?

È possibile usare l'archiviazione con thin provisioning per accelerare i tempi di sincronizzazione iniziali. Replica archiviazione esegue una query e usa automaticamente l'archiviazione con thin provisioning, compresi gli Spazi di archiviazione non cluster, i dischi dinamici Hyper-V e i LUN SAN.  

È anche possibile usare i volumi di dati con seeding per ridurre l'utilizzo della larghezza di banda e talvolta il tempo, assicurando che il volume di destinazione disponga di un subset di dati dal `New-SRPartnership`database primario, quindi usando l'opzione seeding in Gestione cluster di failover o. Se il volume è prevalentemente vuoto, la sincronizzazione con seeding può ridurre la durata e l'uso della larghezza di banda. Esistono diversi modi per inizializzare i dati, con diversi gradi di efficacia:

1. Replica precedente: replicando con la sincronizzazione iniziale normale localmente tra i nodi contenenti dischi e volumi, rimuovendo la replica, spedendo i dischi di destinazione altrove e quindi aggiungendo la replica con l'opzione seeded. Questo è il metodo più efficace, perché replica di archiviazione ha garantito un mirror di copia a blocchi e l'unica cosa da replicare è il blocco Delta.
2. Ripristino dello snapshot o del backup basato su snapshot ripristinato: il ripristino di uno snapshot basato su volume nel volume di destinazione dovrebbe avere differenze minime nel layout dei blocchi. Questo è il metodo più efficace successivo, in quanto i blocchi probabilmente corrispondono grazie agli snapshot del volume che sono immagini speculari.
3. File copiati: creando un nuovo volume nella destinazione che non è mai stato usato prima ed eseguendo una copia della struttura ad albero di Robocopy/MIR completa dei dati, è probabile che si verifichino corrispondenze a blocchi. L'uso di Esplora file di Windows o la copia di una parte dell'albero non creerà molte corrispondenze dei blocchi. La copia manuale dei file è il metodo meno efficace per il seeding.

## <a name="FAQ13"></a>È possibile delegare gli utenti per amministrare la replica?  

È possibile utilizzare il `Grant-SRDelegation` cmdlet. Ciò consente di impostare utenti specifici in scenari di replica da server a server, da cluster a cluster e con cluster esteso che abbiano le autorizzazioni per creare, modificare o rimuovere la replica, pur non essendo un membro del gruppo di amministratori locale. Esempio:  

    Grant-SRDelegation -UserName contso\tonywang  

Il cmdlet ricorderà che l'utente deve disconnettersi e riconnettersi al server che intende amministrare affinché le modifiche abbiano effetto. È possibile usare `Get-SRDelegation` e `Revoke-SRDelegation` per controllare ulteriormente questa opzione.  

## <a name="FAQ13"></a>Quali sono le opzioni di backup e ripristino per i volumi replicati?
Replica archiviazione supporta il backup e il ripristino del volume di origine. Supporta inoltre la creazione e il ripristino degli snapshot del volume di origine. Non è possibile eseguire il backup o ripristinare il volume di destinazione quando è protetto da Replica archiviazione, in quanto non è montato né accessibile. Se si verifica una situazione di emergenza in cui il volume di origine viene perso, usando `Set-SRPartnership` per fare in modo che il volume di destinazione precedente venga promosso a origine di lettura/scrittura sarà possibile eseguire il backup o il ripristino di tale volume. È inoltre possibile rimuovere la replica con `Remove-SRPartnership` e `Remove-SRGroup` per rimontare tale volume come accessibile in lettura/scrittura.
Per creare snapshot periodici coerenti con l'applicazione, è possibile usare VSSADMIN.EXE sul server di origine per eseguire uno snapshot dei volumi di dati replicati. Ad esempio, se si sta replicando il volume F: con Replica archiviazione:

    vssadmin create shadow /for=F:
Dopo aver modificato la direzione di replica, rimosso la replica o quando ci si trova semplicemente nello stesso volume di origine, è possibile ripristinare eventuali snapshot al momento in cui sono stati eseguiti. Ad esempio, usando ancora F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
È inoltre possibile pianificare questo strumento in modo che venga eseguito periodicamente tramite un'attività pianificata. Per altre informazioni sull'uso del Servizio Copia Shadow del volume, vedere [Vssadmin](../../administration/windows-commands/vssadmin.md). Il backup di volumi di registro non richiede azioni o valori particolari. Il tentativo di eseguire questa operazione verrà ignorato da Servizio Copia Shadow del volume.
L'uso di Windows Server Backup, backup di Microsoft Azure, Microsoft DPM o altri snapshot, Servizio Copia Shadow del volume, macchina virtuale o tecnologie basate su file è supportato da Replica archiviazione purché questi servizi operino all'interno del livello del volume. Replica archiviazione non supporta il backup e il ripristino basati su blocchi.

## <a name="FAQ14"></a>È possibile configurare la replica per limitare l'utilizzo della larghezza di banda?
Sì, tramite il limitatore di larghezza di banda SMB. Si tratta di un'impostazione globale per tutto il traffico di Replica archiviazione e pertanto influisce su tutte le repliche dal server. In genere, è necessario solo con la configurazione della sincronizzazione iniziale di Replica archiviazione, in cui è necessario trasferire tutti i dati del volume. Se necessaria dopo la sincronizzazione iniziale, la larghezza di banda di rete è troppo bassa per il carico di lavoro delle operazioni di I/O. Ridurre le operazioni di I/O o aumentare la larghezza di banda.

Questa limitazione deve essere usata solo con la replica asincrona (nota: la sincronizzazione iniziale è sempre asincrona anche se è stata specificata l'opzione sincrona).
È anche possibile usare i criteri di rete QoS per dare forma al traffico di Replica archiviazione. L'uso di una replica di Replica archiviazione con seeding a corrispondenza elevata ridurrà notevolmente l'uso complessivo della larghezza di banda per la sincronizzazione iniziale.


Per impostare il limite della larghezza di banda, usare:

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Per visualizzare il limite della larghezza di banda, usare:

    Get-SmbBandwidthLimit -Category StorageReplication

Per rimuovere il limite della larghezza di banda, usare:

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>Quali porte di rete sono necessarie per la replica di archiviazione?
Replica di archiviazione si basa su SMB e WSMAN per la replica e la gestione. Questo significa che sono necessarie le seguenti porte:

 445 (protocollo di trasporto di replica SMB, protocollo di gestione RPC del cluster) 5445 (iWARP SMB-necessario solo quando si usa la rete RDMA iWARP) 5985 (WSManHTTP-Management Protocol per WMI/CIM/PowerShell)

Nota: Il cmdlet test-SRTopology richiede ICMPv4/ICMPv6, ma non per la replica o la gestione.

## <a name="FAQ15.5"></a>Quali sono le procedure consigliate per i volumi di log?
Le dimensioni ottimali del log variano notevolmente in base all'ambiente e al carico di lavoro e sono determinate dalla quantità di i/o di scrittura eseguita dal carico di lavoro. 

1.  Un log più grande o più piccolo non rende più veloce o più lento
2.  Un log più grande o più piccolo non ha alcun impatto su un volume di dati da 10 GB rispetto a un volume di dati 10 TB, ad esempio

Un log più grande può semplicemente raccogliere e conservare più operazioni di I/O di scrittura prima che vengano estratte. Questo consente il protrarsi di un'interruzione nel servizio tra il computer di origine e di destinazione, ad esempio un'interruzione di rete o una destinazione offline. Se il log può contenere 10 ore di scritture e la rete rimane inattiva per 2 ore, non appena quest'ultima tornerà attiva l'origine potrà semplicemente riprodurre molto rapidamente i delta delle modifiche non sincronizzate nella destinazione ripristinando altrettanto rapidamente la protezione. Se il log contiene 10 ore e il periodo di inattività è di 2 giorni, l'origine deve eseguire la riproduzione da un altro log denominato bitmap e la sincronizzazione sarà probabilmente più lenta. Una volta eseguita la sincronizzazione, tornerà a utilizzare il log.

Replica archiviazione si basa sul log per tutte le prestazioni di scrittura. Registrare le prestazioni critiche per le prestazioni di replica. È necessario assicurarsi che il volume di log offra prestazioni migliori rispetto al volume di dati, in quanto il log serializzerà e sequenzializzerà tutte le operazioni di I/O di scrittura. È necessario utilizzare sempre supporti flash come unità SSD nei volumi di log. Non consentire l'esecuzione di eventuali altri carichi di lavoro nel volume di log, né nei volumi di log del database SQL. 

Nuovo Microsoft consiglia vivamente di eseguire l'archiviazione dei log più velocemente rispetto all'archiviazione dei dati e che i volumi di log non devono mai essere usati per altri carichi di lavoro.

È possibile ottenere indicazioni sul dimensionamento del log eseguendo lo strumento test-SRTopology. In alternativa, è possibile usare i contatori delle prestazioni sui server esistenti per ottenere una valutazione delle dimensioni del log. La formula è semplice: monitora la velocità effettiva del disco dati (media scritture/sec) nel carico di lavoro e la usa per calcolare la quantità di tempo necessaria per riempire il log di dimensioni diverse. Ad esempio, la velocità effettiva del disco dati di 50 MB/s provocherà il wrapping del log di 120 GB in secondi di 120 GB/50 MB oppure 2400 secondi o 40 minuti. Quindi, l'intervallo di tempo durante il quale il server di destinazione potrebbe non essere raggiungibile prima che il log di cui è stato eseguito il wrapper sia di 40 minuti Se il log viene eseguito a capo ma la destinazione diventa nuovamente raggiungibile, l'origine ripeterà i blocchi tramite il log della mappa di bit anziché il log principale. Le dimensioni del log non hanno effetto sulle prestazioni.

È necessario eseguire il backup solo del disco dati del cluster di origine. NON è necessario eseguire il backup dei dischi del log della replica di archiviazione perché un backup può entrare in conflitto con le operazioni di replica di archiviazione.

## <a name="FAQ16"></a>Perché scegliere un cluster esteso rispetto a una topologia da cluster a cluster e da server a server?  
La replica di archiviazione è in tre configurazioni principali: il cluster esteso, il cluster a cluster e il server-server. Ognuna presenta vantaggi diversi.

La topologia del cluster esteso è ideale per i carichi di lavoro che richiedono il failover automatico con orchestrazione, ad esempio cluster di cloud privato Hyper-V e istanze del cluster di failover di SQL Server. Include anche un'interfaccia grafica incorporata che utilizza Gestione cluster di failover. Usa la classica architettura di archiviazione condivisa del cluster asimmetrico di Spazi di archiviazione, SAN, iSCSI e RAID tramite prenotazione permanente. Può essere eseguita con soli 2 nodi.

La topologia da cluster a cluster usa due cluster distinti ed è ideale per gli amministratori che desiderano un failover manuale, soprattutto quando viene effettuato il provisioning del secondo sito per il ripristino di emergenza e l'utilizzo non quotidiano. L'orchestrazione è manuale. A differenza del cluster esteso, Spazi di archiviazione diretta può essere usato in questa configurazione (con avvertenze. vedere le domande frequenti sulla replica di archiviazione e la documentazione da cluster a cluster). È possibile eseguirlo con soli quattro nodi. 

La topologia da server a server è ideale per i clienti che eseguono hardware che non può essere incluso nel cluster. Richiede l'orchestrazione e il failover manuali. Si tratta di una soluzione ideale per distribuzioni economiche tra filiali e Data Center centrali, soprattutto quando si usa la replica asincrona. Questa configurazione può sostituire spesso le istanze di file server protetti da DFSR utilizzati per scenari di ripristino di emergenza a master singolo.

In tutti i casi, le topologie supportano l'esecuzione sia su hardware fisico sia su macchine virtuali. In caso di macchine virtuali, l'hypervisor sottostante non richiede Hyper-V, ma VMware, KVM, Xen e così via.

Replica archiviazione include anche una modalità da server a sé stesso, dove la replica interesserà due volumi diversi nello stesso computer.

## <a name="FAQ18"></a>La deduplicazione dei dati è supportata con replica archiviazione?

Sì, i dati Deduplcation sono supportati con replica archiviazione. Abilitare la deduplicazione dei dati in un volume nel server di origine e durante la replica il server di destinazione riceve una copia deduplicata del volume.

Sebbene sia necessario *installare* la deduplicazione dei dati sia nel server di origine che in quello di destinazione (vedere [installazione e abilitazione della deduplicazione dei](../data-deduplication/install-enable.md)dati), è importante non *abilitare* la deduplicazione dei dati nel server di destinazione. Replica archiviazione consente solo scritture nel server di origine. Poiché la deduplicazione dei dati effettua scritture nel volume, deve essere eseguita solo nel server di origine. 

## <a name="FAQ19"></a>È possibile eseguire la replica tra Windows Server 2019 e Windows Server 2016?

Sfortunatamente, non è supportata la creazione di una *nuova* partnership tra windows server 2019 e windows server 2016. È possibile aggiornare in modo sicuro un server o un cluster che esegue Windows Server 2016 a Windows Server 2019 e qualsiasi relazione *esistente* continuerà a funzionare.

Tuttavia, per ottenere le prestazioni di replica migliorate di Windows Server 2019, tutti i membri della relazione devono eseguire Windows Server 2019 ed è necessario eliminare le relazioni esistenti e i gruppi di replica associati e quindi ricrearli con i dati Seed ( Quando si crea la partnership nell'interfaccia di amministrazione di Windows o con il cmdlet New-SRPartnership.

## <a name="FAQ17"></a>Ricerca per categorie segnalare un problema con replica archiviazione o questa guida?  
Per assistenza tecnica relativa a Replica di archiviazione, è possibile pubblicare un post nei [forum di Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). È inoltre possibile inviare un'e-mail all'indirizzo srfeed@microsoft.com per domande su Replica archiviazione o problemi relativi a questa documentazione. Il <https://windowsserver.uservoice.com> sito è preferibile per le richieste di modifica di progettazione, in quanto consente agli altri clienti di fornire supporto e commenti e suggerimenti per le proprie idee.



## <a name="related-topics"></a>Argomenti correlati  
- [Panoramica di replica archiviazione](storage-replica-overview.md) 
- [Replica del cluster esteso tramite l'archiviazione condivisa](stretch-cluster-replication-using-shared-storage.md)  
- [Replica archiviazione da server a server](server-to-server-storage-replication.md)  
- [Replica di archiviazione da cluster a cluster](cluster-to-cluster-storage-replication.md)  
- [Replica di archiviazione: Problemi noti](storage-replica-known-issues.md)  

## <a name="see-also"></a>Vedere anche  
- [Panoramica dell'archiviazione](../storage.md)  
- [Spazi di archiviazione diretta](../storage-spaces/storage-spaces-direct-overview.md)  
