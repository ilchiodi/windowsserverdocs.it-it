---
title: Domande frequenti su Replica archiviazione
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 07/20/2017
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: f29ca24a39e00f5142fe700e6abb09d1673acf7b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Domande frequenti su Replica archiviazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento contiene le risposte alle domande frequenti su Replica archiviazione.

## <a name="FAQ1"></a> Replica archiviazione è supportata in Nano Server?  
Sì.  

> [!NOTE]
> È necessario usare il pacchetto Nano Server **Archiviazione** durante l'installazione. Per altre informazioni sulla distribuzione di Nano Server, vedere [Getting Started with Nano Server](https://technet.microsoft.com/library/mt126167.aspx) (Introduzione a Nano Server).  

Installare Replica archiviazione su Nano Server usando la comunicazione remota di PowerShell nel modo seguente:  

1. Aggiungere Nano Server all'elenco di attendibilità del client.  
   > [!NOTE]
   > Questo passaggio è necessario solo se il computer non è un membro di una foresta di Active Directory Domain Services o di una foresta non attendibile. Aggiunge il supporto NTLM alla comunicazione remota PSSession, che è disabilitata per impostazione predefinita per motivi di sicurezza. Per altre informazioni, vedere [PowerShell Remoting Security Considerations](https://msdn.microsoft.com/powershell/scriptiwinrmsecurity) (Considerazioni sulla sicurezza della comunicazione remota di PowerShell).  

   ```  
       Set-Item WSMan:\localhost\Client\TrustedHosts "<computer name of Nano Server>"  
   ```  
2.  Per installare la funzionalità Replica archiviazione, eseguire il cmdlet seguente da un computer di gestione:  

    ```  
    Install-windowsfeature -Name storage-replica,RSAT-Storage-Replica -ComputerName <nano server> -Restart -IncludeManagementTools  
    ```  

    L'uso del cmdlet `Test-SRTopology` con Nano Server in Windows Server 2016 richiede una chiamata allo script remoto con CredSSP. A differenza di altri cmdlet di Replica archiviazione, `Test-SRTopology` deve essere eseguito localmente nel server di origine.   
In Nano Server (tramite una PSSession remota):  

    >[!NOTE]
    >CREDSSP è necessario per il supporto della connessione a doppio hop di Kerberos nel cmdlet `Test-SRTopology`, mentre non è richiesto da altri cmdlet di Replica di archiviazione, che gestiscono automaticamente le credenziali di sistema distribuite. L'uso di CREDSSP non è consigliabile in circostanze tipiche. Per un'alternativa a CREDSSP, vedere il post di blog Microsoft "PowerShell Remoting Kerberos Double Hop Solved Securely" (Risoluzione sicura del problema del doppio hop Kerberos per la comunicazione remota di PowerShell) disponibile all'indirizzo https://blogs.technet.microsoft.com/ashleymcglone/2016/08/30/powershell-remoting-kerberos-double-hop-solved-securely/ 

        Enable-WSManCredSSP -role server       

    Nel computer di gestione:  

         Enable-WSManCredSSP Client -DelegateComputer <remote server name>  

         $CustomCred = Get-Credential  

         Invoke-Command -ComputerName sr-srv01 -ScriptBlock { Test-SRTopology <commands> } -Authentication Credssp -Credential $CustomCred  

       Quindi copiare i risultati sul computer di gestione o condividere il percorso. Poiché Nano Server non richiede librerie grafiche, è possibile usare Test-SRTopology per elaborare i risultati e ottenere un file di report con grafici. Ad esempio:  

        Test-SRTopology -GenerateReport -DataPath \\sr-srv05\c$\temp  


## <a name="FAQ2"></a> Come si visualizza lo stato di avanzamento della replica durante la sincronizzazione iniziale?  
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

## <a name="FAQ3"></a>È possibile specificare le interfacce di rete specifiche da usare per la replica?  

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

## <a name="FAQ4"></a> È possibile configurare la replica uno-a-molti o transitiva (da A a B a C)?  
Non in Windows Server 2016. Questa versione supporta solo la replica uno a uno di un server, un cluster o un nodo del cluster esteso. Questo limite potrebbe venire superato nelle versioni successive. Naturalmente, è possibile configurare la replica tra i vari server di una coppia di volumi specifica, in entrambe le direzioni. Ad esempio, Server 1 può replicare il volume D su Server 2 e il volume E su Server 3.

## <a name="FAQ5"></a> È possibile espandere o ridurre i volumi replicati tramite Replica archiviazione?  
Puoi espandere (estendere) i volumi, ma non ridurli. Per impostazione predefinita, Replica di archiviazione impedisce agli amministratori di estendere i volumi replicati. Utilizza l'opzione `Set-SRGroup -AllowVolumeResize $TRUE` nel gruppo di origine, prima del ridimensionamento. Ad esempio:

1. Utilizza rispetto al computer di origine: `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Espandere il volume utilizzando la tecnica preferita
3. Utilizza rispetto al computer di origine: `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>È possibile connettere un volume di destinazione online per l'accesso di sola lettura?  
Non in Windows Server 2016 RTM, ovvero la cosiddetta versione "RS1". Replica archiviazione smonta il volume di destinazione quando inizia la replica. 

In Windows Server, versione 1709 è ora tuttavia possibile montare l'archiviazione di destinazione, grazie alla funzionalità denominata "Failover di test". A tale scopo, è necessario disporre di un volume NTFS o ReFS inutilizzato di cui non viene attualmente eseguita la replica nella destinazione. È quindi possibile montare temporaneamente uno snapshot dell'archiviazione replicata a scopo di test o backup. 

Ad esempio, per creare un failover di test quando si esegue la replica di un volume "D:" nel gruppo di replica "RG2" nel server di destinazione "SRV2" e si dispone di un'unità "T:" in SRV2 di cui non viene eseguita la replica:

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
Il volume replicato D: è ora accessibile in SRV2. È possibile eseguire in modo normale la lettura e la scrittura, copiare i file al di fuori di esso o eseguirne un backup online e salvarlo in un'altra posizione per maggiore sicurezza.

Per rimuovere lo snapshot del failover di test e annullare le modifiche:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Utilizzare la funzionalità per il failover di test solo per le operazioni temporanee a breve termine, in quanto non è destinata all'utilizzo a lungo termine. Quando è in uso, la replica continua per il volume di destinazione reale. 

## <a name="FAQ7"></a> È possibile configurare File server di scalabilità orizzontale in un cluster esteso?  
Anche se tecnicamente possibile, non è una configurazione consigliata in Windows Server 2016 a causa della mancanza di riconoscimento dei siti nei nodi di calcolo che contattano SOFS. Se si usa una rete a distanza campus, dove le latenze sono in genere inferiori al millisecondo, questa configurazione funziona normalmente senza problemi.   

Se si configura una replica da cluster a cluster, Replica archiviazione supporta completamente i File server di scalabilità orizzontale, inclusi l'uso di Spazi di archiviazione diretta, durante la replica tra due cluster.  

## <a name="FAQ8"></a>È possibile configurare Spazi di archiviazione diretta in un cluster esteso con Replica archiviazione?  
Questa configurazione non è supportata in Windows Server 2016.  Questo limite potrebbe venire superato nelle versioni successive. Se si configura una replica da cluster a cluster, Replica archiviazione supporta completamente i File server di scalabilità orizzontale e i server Hyper-V, inclusi l'uso di Spazi di archiviazione diretta.  

## <a name="FAQ9"></a>Come si configura la replica asincrona?  

Specificare `New-SRPartnership -ReplicationMode` e fornire l'argomento **Asincrono**. Per impostazione predefinita, tutte le repliche in Replica archiviazione sono sincrone. È inoltre possibile modificare la modalità con `Set-SRPartnership -ReplicationMode`.  

## <a name="FAQ10"></a>Come si può evitare il failover automatico di un cluster esteso?  
Per evitare il failover automatico, è possibile usare PowerShell per configurare `Get-ClusterNode -Name "NodeName").NodeWeight=0`. In questo modo si rimuove il voto in ogni nodo del sito di ripristino di emergenza. È quindi possibile usare `Start-ClusterNode -PreventQuorum` sui nodi nel sito primario e `Start-ClusterNode -ForceQuorum` sui nodi nel sito di emergenza per forzare il failover. Non esiste alcuna opzione grafica per evitare il failover automatico e in ogni caso non è consigliabile evitare il failover automatico.  

## <a name="FAQ11"></a>Come si può disabilitare la resilienza della macchina virtuale?  
Per evitare che la nuova funzionalità di resilienza della macchina virtuale Hyper-V venga eseguita, mettendo quindi in pausa le macchine virtuali invece di provocarne il failover al sito di ripristino di emergenza, eseguire `(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a> Come si riduce la durata della sincronizzazione iniziale?  

È possibile usare l'archiviazione con thin provisioning per accelerare i tempi di sincronizzazione iniziali. Replica archiviazione esegue una query e usa automaticamente l'archiviazione con thin provisioning, compresi gli Spazi di archiviazione non cluster, i dischi dinamici Hyper-V e i LUN SAN.  

È inoltre possibile usare i volumi dei dati di seeding, per limitare l'uso della larghezza di banda e talvolta anche la durata, verificando innanzitutto che il volume di destinazione possieda un subset dei dati del database primario (tramite un backup ripristinato, uno snapshot precedente, una replica precedente, file copiati e così via) e quindi usando l'opzione Con seeding in Gestione cluster di failover o `New-SRPartnership`. Se il volume è prevalentemente vuoto, la sincronizzazione con seeding può ridurre la durata e l'uso della larghezza di banda.

## <a name="FAQ13"></a> È possibile delegare agli utenti la gestione della replica?  

È possibile usare il cmdlet `Grant-SRDelegation` in Windows Server 2016. Ciò consente di impostare utenti specifici in scenari di replica da server a server, da cluster a cluster e con cluster esteso che abbiano le autorizzazioni per creare, modificare o rimuovere la replica, pur non essendo un membro del gruppo di amministratori locale. Ad esempio:  

    Grant-SRDelegation -UserName contso\tonywang  

Il cmdlet ricorderà che l'utente deve disconnettersi e riconnettersi al server che intende amministrare affinché le modifiche abbiano effetto. È possibile usare `Get-SRDelegation` e `Revoke-SRDelegation` per controllare ulteriormente questa opzione.  

## <a name="FAQ13"></a> Quali sono le opzioni di backup e di ripristino per i volumi replicati?
Replica archiviazione supporta il backup e il ripristino del volume di origine. Supporta inoltre la creazione e il ripristino degli snapshot del volume di origine. Non è possibile eseguire il backup o ripristinare il volume di destinazione quando è protetto da Replica archiviazione, in quanto non è montato né accessibile. Se si verifica una situazione di emergenza in cui il volume di origine viene perso, usando `Set-SRPartnership` per fare in modo che il volume di destinazione precedente venga promosso a origine di lettura/scrittura sarà possibile eseguire il backup o il ripristino di tale volume. È inoltre possibile rimuovere la replica con `Remove-SRPartnership` e `Remove-SRGroup` per rimontare tale volume come accessibile in lettura/scrittura.
Per creare snapshot periodici coerenti con l'applicazione, è possibile usare VSSADMIN.EXE sul server di origine per eseguire uno snapshot dei volumi di dati replicati. Ad esempio, se si sta replicando il volume F: con Replica archiviazione:

    vssadmin create shadow /for=F:
Dopo aver modificato la direzione di replica, rimosso la replica o quando ci si trova semplicemente nello stesso volume di origine, è possibile ripristinare eventuali snapshot al momento in cui sono stati eseguiti. Ad esempio, usando ancora F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
È inoltre possibile pianificare questo strumento in modo che venga eseguito periodicamente tramite un'attività pianificata. Per altre informazioni sull'uso del Servizio Copia Shadow del volume, vedere [Vssadmin](https://technet.microsoft.com/library/cc754968.aspx). Il backup di volumi di registro non richiede azioni o valori particolari. Il tentativo di eseguire questa operazione verrà ignorato da Servizio Copia Shadow del volume.
L'uso di Windows Server Backup, backup di Microsoft Azure, Microsoft DPM o altri snapshot, Servizio Copia Shadow del volume, macchina virtuale o tecnologie basate su file è supportato da Replica archiviazione purché questi servizi operino all'interno del livello del volume. Replica archiviazione non supporta il backup e il ripristino basati su blocchi.

## <a name="FAQ14"></a> È possibile configurare la replica in modo che limiti l'uso della larghezza di banda?
Sì, tramite il limitatore di larghezza di banda SMB. Si tratta di un'impostazione globale per tutto il traffico di Replica archiviazione e pertanto influisce su tutte le repliche dal server. In genere, è necessario solo con la configurazione della sincronizzazione iniziale di Replica archiviazione, in cui è necessario trasferire tutti i dati del volume. Se necessaria dopo la sincronizzazione iniziale, la larghezza di banda di rete è troppo bassa per il carico di lavoro delle operazioni di I/O. Ridurre le operazioni di I/O o aumentare la larghezza di banda.

Questa limitazione deve essere usata solo con la replica asincrona (nota: la sincronizzazione iniziale è sempre asincrona anche se è stata specificata l'opzione sincrona).
È anche possibile usare i criteri di rete QoS per dare forma al traffico di Replica archiviazione. L'uso di una replica di Replica archiviazione con seeding a corrispondenza elevata ridurrà notevolmente l'uso complessivo della larghezza di banda per la sincronizzazione iniziale.


Per impostare il limite della larghezza di banda, usare:

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Per visualizzare il limite della larghezza di banda, usa:

    Get-SmbBandwidthLimit -Category StorageReplication

Per rimuovere il limite della larghezza di banda, usa:

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>Quali porte di rete richiede Replica di archiviazione?
Replica di archiviazione si basa su SMB e WSMAN per la replica e la gestione. Questo significa che sono necessarie le seguenti porte:

   445 (SMB - protocollo di trasporto di replica) 5445 (iWARP SMB - necessaria solo quando si utilizzano le reti RDMA iWARP) 5895 (WSManHTTP - protocollo di gestione per WMI/CIM/PowerShell)

Nota: il cmdlet Test-SRTopology richiede ICMPv4/ICMPv6, ma non per la replica o la gestione.

## <a name="FAQ15"></a>Quali sono le procedure consigliate per il volume di log?
La dimensione ottimale del log varia notevolmente per ogni ambiente e carico di lavoro ed è determinata dal numero delle operazioni di I/O di scrittura eseguite dal carico di lavoro. 

1.  Un log più grande o più piccolo non determina una velocità superiore o inferiore
2.  Un log più grande o più piccolo non ha alcuna influenza su un volume di dati di 10 GB rispetto a un volume di dati di 10 TB

Un log di dimensioni maggiori può semplicemente raccogliere e conservare più operazioni di I/O di scrittura prima che vengano estratte. Questo consente il protrarsi di un'interruzione nel servizio tra il computer di origine e di destinazione, ad esempio un'interruzione di rete o una destinazione offline. Se il log può contenere 10 ore di scritture e la rete rimane inattiva per 2 ore, non appena quest'ultima tornerà attiva l'origine potrà semplicemente riprodurre molto rapidamente i delta delle modifiche non sincronizzate nella destinazione ripristinando altrettanto rapidamente la protezione. Se il log contiene 10 ore e il periodo di inattività è di 2 giorni, l'origine deve eseguire la riproduzione da un altro log denominato bitmap e la sincronizzazione sarà probabilmente più lenta. Una volta eseguita la sincronizzazione, tornerà a usare il log.

Sono disponibili contatori delle prestazioni di Replica archiviazione che indicano la velocità di varianza, consentendo all'utente di effettuare alcune valutazioni. Anche il cmdlet Test-SRTopology esegue questa operazione. Fondamentalmente si stanno semplicemente esaminando I/O di scrittura in un carico di lavoro esistente per decidere quante operazioni di I/O potrebbero essere aggiunte al log ogni minuto, ora o giorno.

Replica archiviazione si basa sul log per tutte le prestazioni di scrittura. Registrare le prestazioni critiche per le prestazioni di replica. È necessario assicurarsi che il volume di log offra prestazioni migliori rispetto al volume di dati, in quanto il log serializzerà e sequenzializzerà tutte le operazioni di I/O di scrittura. È necessario utilizzare sempre supporti flash come unità SSD nei volumi di log. Non consentire l'esecuzione di eventuali altri carichi di lavoro nel volume di log, né nei volumi di log del database SQL. 

Microsoft consiglia vivamente che la velocità di archiviazione dei log sia maggiore rispetto all'archiviazione dei dati e che i volumi di log non vengano mai utilizzati per altri carichi di lavoro.

## <a name="FAQ16"></a> Perché scegliere una topologia cluster esteso rispetto a una da cluster a cluster o da server a server?  
Replica archiviazione è disponibile in tre configurazioni principali: cluster esteso, da cluster a cluster e da server a server. Ognuna presenta vantaggi diversi.

La topologia del cluster esteso è ideale per i carichi di lavoro che richiedono il failover automatico con orchestrazione, ad esempio cluster di cloud privato Hyper-V e istanze del cluster di failover di SQL Server. Include anche un'interfaccia grafica incorporata che utilizza Gestione cluster di failover. Usa la classica architettura di archiviazione condivisa del cluster asimmetrico di Spazi di archiviazione, SAN, iSCSI e RAID tramite prenotazione permanente. Può essere eseguita con soli 2 nodi.

La topologia da cluster a cluster usa due cluster distinti ed è ideale per gli amministratori che desiderano un failover manuale, soprattutto quando viene effettuato il provisioning del secondo sito per il ripristino di emergenza e l'utilizzo non quotidiano. L'orchestrazione è manuale. A differenza di un cluster esteso, Spazi di archiviazione diretta può essere utilizzato in questa configurazione. È possibile eseguirlo con soli quattro nodi. 

La topologia da server a server è ideale per i clienti che eseguono hardware che non può essere incluso nel cluster. Richiede l'orchestrazione e il failover manuali. È ideale per le distribuzioni economiche tra succursali e data center centrali, soprattutto quando si utilizza la replica asincrona. Questa configurazione può sostituire spesso le istanze di file server protetti da DFSR utilizzati per scenari di ripristino di emergenza a master singolo.

In tutti i casi, le topologie supportano l'esecuzione sia su hardware fisico sia su macchine virtuali. In caso di macchine virtuali, l'hypervisor sottostante non richiede Hyper-V, ma VMware, KVM, Xen e così via.

Replica archiviazione include anche una modalità da server a sé stesso, dove la replica interesserà due volumi diversi nello stesso computer.

## <a name="FAQ17"></a> Come si può segnalare un problema di Replica archiviazione o di questa guida?  
Per assistenza tecnica relativa a Replica archiviazione, puoi pubblicare un post nei [forum di Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). È inoltre possibile inviare un'e-mail all'indirizzo srfeed@microsoft.com per domande su Replica archiviazione o problemi relativi a questa documentazione. Il sito https://windowsserver.uservoice.com è consigliato per le richieste di modifica di progettazione, in quanto consente ai clienti di offrire supporto e feedback per lo sviluppo di nuove idee.



## <a name="related-topics"></a>Argomenti correlati  
- [Informazioni generali su Replica archiviazione](storage-replica-overview.md) 
- [Replica di un cluster esteso tramite l'archiviazione condivisa](stretch-cluster-replication-using-shared-storage.md)  
- [Replica di archiviazione da server a server](server-to-server-storage-replication.md)  
- [Cluster to Cluster Storage Replication (Replica archiviazione da cluster a cluster)](cluster-to-cluster-storage-replication.md)  
- [Replica archiviazione: Problemi noti](storage-replica-known-issues.md)  

## <a name="see-also"></a>Vedere anche  
- [Panoramica dell'archiviazione](../storage.md)  
- [Spazi di archiviazione diretta in Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
