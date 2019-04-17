---
title: Problemi noti di Replica di archiviazione
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 10/13/2016
ms.assetid: ceddb0fa-e800-42b6-b4c6-c06eb1d4bc55
ms.openlocfilehash: 6f02ece1f327cf53667df09e6d13ace001259885
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="known-issues-with-storage-replica"></a>Problemi noti di Replica di archiviazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questa sezione illustra i problemi noti di Replica di archiviazione in Windows Server 2016.  

## <a name="after-removing-replication-disks-are-offline-and-you-cannot-configure-replication-again"></a>Dopo aver rimosso la replica, i dischi sono offline e non è possibile configurarla nuovamente  
In Windows Server 2016 può non essere possibile effettuare il provisioning della replica in un volume replicato in precedenza oppure si possono trovare volumi non montabili. Ciò può verificarsi quando una condizione di errore impedisce la rimozione della replica o quando si reinstalla il sistema operativo in un computer che è stato già usato per la replica dei dati.  

Per risolvere il problema, è necessario cancellare la partizione nascosta di Replica di archiviazione dai dischi e ripristinarne lo stato scrivibile usando il cmdlet `Clear-SRMetadata`.  

-   Per rimuovere tutti gli slot database orfani della partizione di Replica archiviazione e reinstallare tutte le partizioni, usare il parametro `-AllPartitions` come indicato di seguito:  

    ```  
    Clear-SRMetadata -AllPartitions  
    ```  

-   Per rimuovere tutti i dati del registro orfani di Replica archiviazione, usare il parametro `-AllLogs` come indicato di seguito:  

    ```  
    Clear-SRMetadata -AllLogs  
    ```  

-   Per rimuovere tutti i dati di configurazione orfani del cluster di failover, usare il parametro `-AllConfiguration` come indicato di seguito:  

    ```  
    Clear-SRMetadata -AllConfiguration  
    ```  

-   Per rimuovere i metadati singoli del gruppo di replica, usare il parametro `-Name` e specificare un gruppo di replica come indicato di seguito:  

    ```  
    Clear-SRMetadata -Name RG01 -Logs -Partition  
    ```  

Dopo la pulizia del database di partizione potrebbe essere necessario riavviare il server; tuttavia, è possibile saltare temporaneamente questa operazione con `-NoRestart` ma non è consigliabile ignorare il riavvio del server, se richiesto dal cmdlet. Questo cmdlet non rimuove i volumi di dati né i dati contenuti all'interno di tali volumi.  

## <a name="during-initial-sync-see-event-log-4004-warnings"></a>Durante la sincronizzazione iniziale, vedere gli avvisi 4004 del registro eventi  
In Windows Server 2016, quando si configura la replica, durante la sincronizzazione iniziale sia il server di origine sia quello di destinazione possono mostrare più avvisi 4004 del registro eventi **StorageReplica\Admin**, con stato "Risorse di sistema non sufficienti per completare l'API". È probabile che vengano visualizzati anche errori 5014. Queste informazioni indicano che i server non hanno sufficiente memoria disponibile (RAM) per eseguire la sincronizzazione iniziale o gestire carichi di lavoro. Aggiungere RAM oppure ridurre la quantità di RAM usata da funzionalità e applicazioni diverse da Replica archiviazione.  

## <a name="when-using-guest-clusters-with-shared-vhdx-and-a-host-without-a-csv-virtual-machines-stop-responding-after-configuring-replication"></a>Se si usano cluster guest con VHDX condiviso e un host senza un volume condiviso cluster, le macchine virtuali si bloccano dopo aver configurato la replica  
In Windows Server 2016, quando si usano cluster guest Hyper-V per scopi di test o dimostrazione di Replica di archiviazione, e quando si usa VHDX condiviso come archiviazione del cluster guest, le macchine virtuali si bloccano dopo la configurazione della replica. Se si riavvia l'host Hyper-V, le macchine virtuali iniziano a rispondere ma la configurazione della replica non sarà completata e non verrà eseguita alcuna replica.  

Questo comportamento si verifica quando si usa **fltmc.exe attach svhdxflt** per ignorare il requisito dell'host Hyper-V con un volume condiviso cluster in esecuzione. L'uso di questo comando non è supportato ed è consentito solo a fini di test e dimostrazione.  

La causa del rallentamento è un problema di interoperabilità da progettazione tra la funzionalità QoS di archiviazione in Windows Server 2016 e il filtro VHDX condiviso collegato manualmente. Per risolvere questo problema, disabilitare il driver del filtro QoS di archiviazione e riavviare l'host Hyper-V:  

```  
SC config storqosflt start= disabled  
```  

## <a name="cannot-configure-replication-when-using-new-volume-and-differing-storage"></a>Impossibile configurare la replica quando si usa un'archiviazione diversa o di nuovi volumi  
Quando si usa il cmdlet `New-Volume` insieme a diversi gruppi di archiviazione nel server di origine e destinazione, ad esempio due diversi SAN o due JBOD con dischi diversi, potrebbe non essere possibile configurare successivamente la replica usando `New-SRPartnership`. L'errore visualizzato può includere:  

    Data partition sizes are different in those two groups  

Usare il cmdlet `New-Partition**` per creare volumi e formattarli al contrario di `New-Volume`, poiché l'ultimo cmdlet può arrotondare le dimensioni del volume su array di archiviazione diversi. Se è già stato creato un volume NTFS, è possibile usare `Resize-Partition` per aumentare o ridurre uno dei volumi in modo che corrisponda all'altro (questa azione non si applica ai volumi ReFS). Se si usa **Diskmgmt** o **Server Manager**, non si verifica alcun arrotondamento.  

## <a name="running-test-srtopology-fails-with-name-related-errors"></a>L'esecuzione di Test-SRTopology ha esito negativo con errori correlati al nome

Quando si tenta di usare `Test-SRTopology` viene visualizzato uno degli errori seguenti:  

**ERRORE DI ESEMPIO 1:**

    WARNING: Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP address as  
    input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable inputs  
    WARNING: System.Exception  
    WARNING:    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()  
    Test-SRTopology : Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP  
    address as input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable  
    inputs  
    At line:1 char:1  
    + Test-SRTopology -SourceComputerName sr-srv01 -SourceVolumeName d: -So ...  
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  
        + CategoryInfo          : InvalidArgument: (:) [Test-SRTopology], Exception  
        + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand  

**ERRORE DI ESEMPIO 2:**

    WARNING: Invalid value entered for source computer name

**ERRORE DI ESEMPIO 3:**

    The specified volume cannot be found G: cannot be found on computer SRCLUSTERNODE1

Questo cmdlet prevede una segnalazione degli errori limitata in Windows Server 2016 e restituisce lo stesso output per molti problemi comuni. L'errore può essere visualizzato per i motivi seguenti:  

* Si è connessi al computer di origine come utente locale, non come utente di dominio.  
* Il computer di destinazione non è in esecuzione o non è accessibile nella rete.  
* È stato specificato un nome non corretto per il computer di destinazione.  
* È stato specificato un indirizzo IP per il server di destinazione.  
* Il firewall del computer di destinazione sta bloccando l'accesso alle chiamate di PowerShell e/o CIM.  
* Il computer di destinazione non sta eseguendo il servizio WMI.   
* CREDSSP non è stato usato durante l'esecuzione del cmdlet `Test-SRTopology` in modalità remota da un computer di gestione.
* Il volume di origine e quello di destinazione specificati sono dischi locali in un nodo del cluster e non dischi appartenenti al cluster.  

## <a name="configuring-new-storage-replica-partnership-returns-an-error---failed-to-provision-partition"></a>La configurazione di una nuova relazione di Replica archiviazione restituisce un errore: "Impossibile effettuare il provisioning della partizione"
Quando si tenta di creare una nuova relazione di replica con `New-SRPartnership`, viene visualizzato l'errore seguente:

    New-SRPartnership : Unable to create replication group test01, detailed reason: Failed to provision partition ed0dc93f-107c-4ab4-a785-afd687d3e734.
    At line: 1 char: 1
    + New-SRPartnership -SourceComputerName srv1 -SourceRGName test01
    + Categorylnfo : ObjectNotFound: (MSFT_WvrAdminTasks : root/ Microsoft/. . s) CNew-SRPartnership], CimException
    + FullyQua1ifiedErrorId : Windows System Error 1168 ,New-SRPartnership

Ciò è dovuto alla selezione di un volume di dati che si trova nella stessa partizione come unità di sistema (ad esempio l'unità **C:** con la sua cartella di Windows). Ad esempio, in un'unità che contiene i volumi **C:** e **D:** creati dalla stessa partizione. Questo non è supportato in Replica archiviazione. È necessario selezionare un volume diverso per eseguire la replica.

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-update"></a>Il tentativo di espandere un volume replicato non riesce a causa di un aggiornamento mancante
Quando provi a estendere o espandere un volume replicato, ricevi l'errore seguente:

    PS C:\> Resize-Partition -DriveLetter d -Size 44GB
    Resize-Partition : The operation failed with return code 8
    At line:1 char:1
    + Resize-Partition -DriveLetter d -Size 44GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition
    [Resize-Partition], CimException
    + FullyQualifiedErrorId : StorageWMI 8,Resize-Partition

Se usi lo snap-in MMC di Gestione disco, ricevi l'errore seguente: 

    Element not found

Ciò si verifica anche se si abiliti correttamente il ridimensionamento del volume sul server di origine mediante `Set-SRGroup -Name rg01 -AllowVolumeResize $TRUE`. 

Questo problema è stato risolto nell'aggiornamento cumulativo per Windows 10 versione 1607 e Windows Server 2016: 9 dicembre 2016 (KB3201845). 

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-step"></a>Il tentativo di espandere un volume replicato non riesce a causa di un passaggio mancante
Se tenti di ridimensionare un volume replicato nel server di origine senza prima impostare `-AllowResizeVolume $TRUE`, ricevi il seguente errore:

    PS C:\> Resize-Partition -DriveLetter I -Size 8GB
    Resize-Partition : Failed
    Activity ID: {87aebbd6-4f47-4621-8aa4-5328dfa6c3be}
    At line:1 char:1
    + Resize-Partition -DriveLetter I -Size 8GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition) [Resize-Partition], CimException
        + FullyQualifiedErrorId : StorageWMI 4,Resize-Partition

Errore del registro eventi di Replica di archiviazione 10307:

    Attempted to resize a partition that is protected by Storage Replica .

    DeviceName: \Device\Harddisk1\DR1
    PartitionNumber: 7
    PartitionId: {b71a79ca-0efe-4f9a-a2b9-3ed4084a1822}

    Guidance: To grow a source data partition, set the policy on the replication group containing the data partition.

    Set-SRGroup -ComputerName [ComputerName] -Name [ReplicationGroupName] -AllowVolumeResize $true

    Before you grow the source data partition, ensure that the destination data partition has enough space to grow to an equal size. Shrinking of data partition protected by Storage Replica is blocked.

Errore snap-in di MMC Gestione disco: 

    An unexpected error has occurred 

Dopo il ridimensionamento del volume, ricorda di disabilitare il ridimensionamento con `Set-SRGroup -Name rg01 -AllowVolumeResize $FALSE`. Questo parametro impedisce gli amministratori di tentare di ridimensionare i volumi prima di verificare che vi sia spazio sufficiente nel volume di destinazione, in genere perché non a conoscenza della presenza di Replica di archiviazione. 

## <a name="attempting-to-move-a-pdr-resource-between-sites-on-an-asynchronous-stretch-cluster-fails"></a>Il tentativo di spostare una risorsa PDR tra siti su un cluster esteso asincrono non riesce
Quando si tenta di spostare un ruolo del disco fisico collegato alla risorsa, ad esempio un file server per uso generale, per spostare lo spazio di archiviazione associato in un cluster esteso asincrono, viene visualizzato un errore.

Se si usa snap-in Gestione cluster di failover:

    Error
    The operation has failed.
    The action 'Move' did not complete.
    Error Code: 0x80071398
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a possible owner of the group
    
Se si usa il cmdlet di PowerShell per il cluster:

    PS C:\> Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    Move-ClusterGroup : An error occurred while moving the clustered role 'sr-fs-006'.
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a
    possible owner of the group
    At line:1 char:1
    + Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Move-ClusterGroup], ClusterCmdletException
    + FullyQualifiedErrorId : Move-ClusterGroup,Microsoft.FailoverClusters.PowerShell.MoveClusterGroupCommand

Questo è dovuto a un comportamento predefinito in Windows Server 2016. Per risolvere il problema, è necessario usare `Set-SRPartnership` per spostare questi dischi PDR in un cluster esteso asincrono.  

## <a name="attempting-to-add-disks-to-a-two-node-asymmetric-cluster-returns-no-disks-suitable-for-cluster-disks-found"></a>Il tentativo di aggiungere dischi a un cluster asimmetrico a due nodi restituisce un errore relativo all'assenza di dischi adatti ai dischi del cluster 
Quando si tenta di effettuare il provisioning di un cluster con solo due nodi, prima di aggiungere la replica estesa di Replica di archiviazione, tentare di aggiungere i dischi nel secondo sito ai dischi disponibili. Vien visualizzato l'errore seguente:

    "No disks suitable for cluster disks found. For diagnostic information about disks available to the cluster, use the Validate a Configuration Wizard to run Storage tests." 

Questa situazione non si verifica se nel cluster sono disponibili almeno tre nodi. Questo problema si verifica a causa di una modifica del codice in Windows Server 2016 da progettazione, dovuta al clustering dell'archiviazione asimmetrica. 

Per aggiungere lo spazio di archiviazione, è possibile eseguire il comando seguente sul nodo nel secondo sito:

`Get-ClusterAvailableDisk -All | Add-ClusterDisk`

Tale comando non funzionerà con l'archiviazione locale nel nodo. Puoi usare Replica archiviazione per replicare un cluster esteso tra due nodi totali, **ciascuno dei quali usa il proprio set di archiviazione condivisa.** 

## <a name="the-smb-bandwidth-limiter-fails-to-throttle-storage-replica-bandwidth"></a>Il limitatore di larghezza di banda SMB non riesce a limitare la larghezza di banda di Replica di archiviazione
Quando si specifica un limite di larghezza di banda di Replica di archiviazione, il limite viene ignorato e si utilizza la larghezza di banda piena. Ad esempio:

`Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond 32MB`

Questo problema si verifica a causa di un problema di interoperabilità tra Replica di archiviazione e SMB. Questo problema è stato risolto per la prima volta nell'aggiornamento cumulativo di Windows Server 2016 di luglio 2017 e in Windows Server, 1709 versione.

## <a name="event-1241-warning-repeated-during-initial-sync"></a>Avviso evento 1241 ripetuto durante la sincronizzazione iniziale
Quando si specifica che una relazione di replica è asincrona, il computer di origine registra ripetutamente l'evento di avviso 1241 nel canale dell'amministratore di Replica di archiviazione. Ad esempio:

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 3:10:41 PM
    Event ID:      1241
    Task Category: (1)
    Level:         Warning
    Keywords:      (1)
    User:          SYSTEM
    Computer:      sr-srv05.corp.contoso.com
    Description:
    The Recovery Point Objective (RPO) of the asynchronous destination is unavailable.

    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {e20b6c68-1758-4ce4-bd3b-84a5b5ef2a87}
    LocalReplicaName: f:\
    LocalPartitionId: {27484a49-0f62-4515-8588-3755a292657f}
    ReplicaSetId: {1f6446b5-d5fd-4776-b29b-f235d97d8c63}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {7f18e5ea-53ca-4b50-989c-9ac6afb3cc81}
    TargetRPO: 30

    Guidance: This is typically due to one of the following reasons: 

La destinazione asincrona è attualmente disconnessa. L'RPO può diventare disponibile al ripristino della connessione.

    The asynchronous destination is unable to keep pace with the source such that the most recent destination log record is no longer present in the source log. The destination will start block copying. The RPO should become available after block copying completes.

Si tratta di un comportamento normale durante la sincronizzazione iniziale e può essere ignorato. È possibile che questo comportamento venga superato in una versione successiva. Se questo comportamento si verifica durante la replica asincrona, analizzare la relazione per determinare il motivo per cui la replica presenta un ritardo superiore all'RPO configurato (30 secondi, per impostazione predefinita).

## <a name="event-4004-warning-repeated-after-rebooting-a-replicated-node"></a>Avviso evento 4004 ripetuto in seguito al riavvio di un nodo replicato
In circostanze rare e di solito non riproducibili, il riavvio di un server che si trova in una relazione, impedisce la riuscita di una replica e comporta un evento di avviso 4004 relativo alla registrazione del nodo riavviato con un errore di accesso negato.

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 11:43:25 AM
    Event ID:      4004
    Task Category: (7)
    Level:         Warning
    Keywords:      (256)
    User:          SYSTEM
    Computer:      server.contoso.com
    Description:
    Failed to establish a connection to a remote computer.

    RemoteComputerName: server
    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    ReplicaSetId: {00000000-0000-0000-0000-000000000000}
    RemoteShareName:{a386f747-bcae-40ac-9f4b-1942eb4498a0}.{00000000-0000-0000-0000-000000000000}
    Status: {Access Denied}
    A process has requested access to an object, but has not been granted those access rights.

    Guidance: Possible causes include network failures, share creation failures for the remote replication group, or firewall settings. Make sure SMB traffic is allowed and there are no connectivity issues between the local computer and the remote computer. You should expect this event when suspending replication or removing a replication partnership.

Notare `Status: "{Access Denied}"` e il messaggio `A process has requested access to an object, but has not been granted those access rights.` Si tratta di un problema noto all'interno di Replica di archiviazione che è stato risolto nell'aggiornamento qualitativo del 12 settembre 2017: KB4038782 (build del sistema operativo 14393.1715) https://support.microsoft.com/it-it/help/4038782/windows-10-update-kb4038782 

## <a name="error-failed-to-bring-the-resource-cluster-disk-x-online-with-a-stretch-cluster"></a>Errore "Impossibile connettere la risorsa "Cluster Disk x". con un cluster esteso
Nel tentativo di connettere un disco del cluster in seguito a un failover dall'esito positivo, in cui si sta tentando di rendere nuovamente primario al sito di origine, viene visualizzato un errore in Gestione cluster di failover. Ad esempio:

    Error
    The operation has failed.
    Failed to bring the resource 'Cluster Disk 2' online.
    
    Error Code: 0x80071397
    The operation failed because either the specified cluster node is not the owner of the resource, or the node is not a possible owner of the resource.
    
Se si tenta di spostare manualmente disco o CSV, viene visualizzato un errore aggiuntivo. Ad esempio:

    Error
    The operation has failed.
    The action 'Move' did not complete.
    
    Error Code: 0x8007138d
    A cluster node is not available for this operation

Questo problema è causato da uno o più dischi non inizializzati collegati a uno o più nodi del cluster. Per risolvere il problema, inizializzare tutta l'archiviazione collegata tramite DiskMgmt.msc, DISKPART.EXE o cmdlet PowerShell Initialize-Disk.

Stiamo lavorando per fornire un aggiornamento che consenta di risolvere definitivamente il problema. Se si è interessati a fornirci assistenza e si dispone di un contratto di supporto tecnico Microsoft Premier, inviare un'e-mail a SRFEED@microsoft.com in modo che possiamo collaborare sulla presentazione di una richiesta di backporting.

## <a name="gpt-error-when-attempting-to-create-a-new-sr-partnership"></a>Errore GPT nel tentativo di creare una nuova relazione SR

Quando si esegue Nuova relazione SR, l'esito è negativo con l'errore: 

    Disk layout type for volume \\?\Volume{GUID}\ is not a valid GPT style layout.
    New-SRPartnership : Unable to create replication group SRG01, detailed reason: Disk layout type for volume
    \\?\Volume{GUID}\ is not a valid GPT style layout.
    At line:1 char:1
    + New-SRPartnership -SourceComputerName nodesrc01 -SourceRGName SRG01 ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (MSFT_WvrAdminTasks:root/Microsoft/...T_WvrAdminTasks) [New-SRPartnership]
    , CimException
    + FullyQualifiedErrorId : Windows System Error 5078,New-SRPartnership

Nella GUI di Gestione cluster di failover, non è possibile configurare la replica per il disco.

Quando si esegue Test-SRTopology, l'esito è negativo con: 

    WARNING: Object reference not set to an instance of an object.
    WARNING: System.NullReferenceException
    WARNING:    at Microsoft.FileServices.SR.Powershell.MSFTPartition.GetPartitionInStorageNodeByAccessPath(String AccessPath, String ComputerName, MIObject StorageNode)
       at Microsoft.FileServices.SR.Powershell.Volume.GetVolume(String Path, String ComputerName)
       at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()
    Test-SRTopology : Object reference not set to an instance of an object.
    At line:1 char:1
    + Test-SRTopology -SourceComputerName nodesrc01 -SourceVolumeName U: - ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidArgument: (:) [Test-SRTopology], NullReferenceException
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand 

La causa è il livello funzionale del cluster che viene ancora impostato su Windows Server 2012 R2 (ovvero FL 8). Replica di archiviazione deve restituire un errore specifico qui, ma al contrario, restituisce una mappatura degli errori non corretta.

Esegui Get-Cluster | fl * su ciascun nodo.

Se ClusterFunctionalLevel = 9, ovvero la versione ClusterFunctionalLevel di Windows 2016 necessaria per implementare Replica di archiviazione su questo nodo.
Se ClusterFunctionalLevel non è 9, sarà necessario aggiornare ClusterFunctionalLevel al fine di implementare Replica di archiviazione su questo nodo.

Per risolvere il problema, aumentare il livello di funzionalità del cluster eseguendo cmdlet PowerShell: ClusterFunctionalLevel aggiornamento https://technet.microsoft.com/itpro/powershell/windows/failoverclusters/update-clusterfunctionallevel

## <a name="small-unknown-partition-listed-in-diskmgmt-for-each-replicated-volume"></a>Piccola partizione sconosciuta elencata in DISKMGMT per ciascun volume replicato

Quando esegui lo snap-in di gestione disco (DISKMGMT.MSC), noterai uno o più volumi elencati senza etichetta o lettera di unità e delle dimensioni di 1 MB. Potresti essere in grado di eliminare il volume sconosciuto o visualizzare uno dei seguenti messaggi:

    "An Unexpected Error has Occurred"  

Questo comportamento è da progettazione. Non si tratta di un volume, ma di una partizione. Replica archiviazione crea una partizione di 512 KB come slot di database (lo strumento DiskMgmt.msc legacy esegue l'arrotondamento al MB più vicino). Disporre di una partizione simile per ciascun volume replicato è normale e auspicabile. Quando non è più in uso, sei libero di eliminare questa partizione di 512 KB. Le partizioni in uso non possono essere eliminate. La partizione non verrà estesa né ridotta. Se si sta ricreando la replica, si consiglia di lasciare la partizione in quanto Replica archiviazione richiederà quelle non usate.

Per visualizzare i dettagli, usa lo strumento DISKPART o il cmdlet Get-Partition. Tali partizioni avranno un tipo GPT di `558d43c5-a1ac-43c0-aac8-d1472b2923d1`. 

## <a name="see-also"></a>Vedere anche  
- [Replica di archiviazione](storage-replica-overview.md)  
- [Replica di un cluster esteso tramite l'archiviazione condivisa](stretch-cluster-replication-using-shared-storage.md)  
- [Replica di archiviazione da server a server](server-to-server-storage-replication.md)  
- [Cluster to Cluster Storage Replication (Replica archiviazione da cluster a cluster)](cluster-to-cluster-storage-replication.md)  
- [Replica archiviazione: Domande frequenti](storage-replica-frequently-asked-questions.md)  
- [Spazi di archiviazione diretti](../storage-spaces/storage-spaces-direct-overview.md)  
