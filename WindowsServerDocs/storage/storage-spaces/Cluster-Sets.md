---
title: Set di cluster
ms.prod: windows-server
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: Questo articolo descrive lo scenario dei set di cluster
ms.localizationpriority: medium
ms.openlocfilehash: db427e8fa4e5574c6eb7837cf0ab4a9fcc180410
ms.sourcegitcommit: 3c3dfee8ada0083f97a58997d22d218a5d73b9c4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639957"
---
# <a name="cluster-sets"></a>Set di cluster

> Si applica a: Windows Server 2019

Set di cluster è la nuova tecnologia per la scalabilità orizzontale del cloud nella versione di Windows Server 2019 che aumenta il numero di nodi del cluster in un singolo Cloud SDDC (software defined Data Center) per ordine di grandezza. Un set di cluster è un raggruppamento a regime di controllo libero di più cluster di failover: calcolo, archiviazione o iperconvergenza. Set di cluster la tecnologia consente la fluidità delle macchine virtuali tra i cluster di membri all'interno di un set di cluster e uno spazio dei nomi di archiviazione unificato nel set a supporto della fluidità delle macchine virtuali.

Mantenendo le esperienze di gestione del cluster di failover esistenti sui cluster di membri, un'istanza del set di cluster offre inoltre i casi di utilizzo chiave per la gestione del ciclo di vita nell'aggregazione. Questa guida alla valutazione dello scenario di Windows Server 2019 fornisce le informazioni di base necessarie insieme a istruzioni dettagliate per la valutazione della tecnologia dei set di cluster con PowerShell.

## <a name="technology-introduction"></a>Introduzione alla tecnologia

La tecnologia dei set di cluster è stata sviluppata per soddisfare specifiche richieste dei clienti che operano su cloud SDDC (software defined Data Center) su larga scala. La proposta del valore dei set di cluster può essere riepilogata come indicato di seguito:  

- Aumenta significativamente la scalabilità cloud SDDC supportata per l'esecuzione di macchine virtuali a disponibilità elevata combinando più cluster più piccoli in un'unica infrastruttura di grandi dimensioni, anche mantenendo il limite di errore software in un singolo cluster
- Gestire il ciclo di vita di un intero cluster di failover, inclusi il caricamento e il ritiro dei cluster, senza alcun effetto sulla disponibilità delle macchine virtuali tenant, tramite la migrazione fluida delle macchine virtuali in questa infrastruttura di grandi dimensioni
- Modificare con facilità il rapporto tra calcolo e archiviazione in iperconvergenza I
- Vantaggi offerti da [domini di errore e set di disponibilità di Azure](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) tra cluster nel posizionamento iniziale delle macchine virtuali e nella successiva migrazione della macchina virtuale
- Combina le diverse generazioni di hardware della CPU nella stessa infrastruttura del set di cluster, anche mantenendo i singoli domini di errore omogenei per la massima efficienza.  Si noti che la raccomandazione dello stesso hardware è ancora presente all'interno di ogni singolo cluster, nonché dell'intero set di cluster.

Da una visualizzazione di alto livello, questo è l'aspetto dei set di cluster.

![Visualizzazione soluzione set di cluster](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

Di seguito viene fornito un riepilogo rapido di ognuno degli elementi nell'immagine precedente:

**Cluster di gestione**

Il cluster di gestione in un set di cluster è un cluster di failover che ospita il piano di gestione a disponibilità elevata dell'intero set di cluster e il riferimento File server di scalabilità orizzontale (SOFS) per lo spazio dei nomi di archiviazione unificato (cluster set Namespace). Un cluster di gestione è disaccoppiato logicamente dai cluster membri che eseguono i carichi di lavoro delle macchine virtuali. In questo modo il piano di gestione del set di cluster è resiliente agli errori localizzati a livello di cluster, ad esempio la perdita di potenza di un cluster membro.   

**Cluster membro**

Un cluster membro in un set di cluster è in genere un cluster iperconvergente tradizionale che esegue carichi di lavoro di macchine virtuali e Spazi di archiviazione diretta. I cluster di più membri partecipano a una singola distribuzione di set di cluster, che costituisce l'infrastruttura cloud SDDC più ampia. I cluster membro differiscono da un cluster di gestione in due aspetti principali: i cluster membro partecipano a costrutti di dominio di errore e di set di disponibilità e i cluster di membri vengono anche dimensionati per ospitare macchine virtuali e carichi di lavoro di Spazi di archiviazione diretta. Per questo motivo, le macchine virtuali del set di cluster che si spostano tra i limiti del cluster in un set di cluster non devono essere ospitate nel cluster di gestione.

**Riferimento spazio dei nomi set cluster SOFS**

Un riferimento allo spazio dei nomi del set di cluster (cluster set Namespace) SOFS è un File server di scalabilità orizzontale in cui ogni condivisione SMB nello spazio dei nomi del set di cluster SOFS è una condivisione di riferimento, di tipo ' SimpleReferral ' introdotta in Windows Server 2019. Questo riferimento consente ai client SMB (Server Message Block) di accedere alla condivisione SMB di destinazione ospitata nel cluster membro SOFS. Il riferimento dello spazio dei nomi del set di cluster SOFS è un meccanismo di riferimento leggero e, di conseguenza, non partecipa al percorso di I/O. I riferimenti SMB vengono memorizzati nella cache perpetuamente su ognuno dei nodi client e il cluster imposta lo spazio dei nomi in modo dinamico aggiorna automaticamente questi riferimenti in base alle esigenze.

**Master del set di cluster**

In un set di cluster, la comunicazione tra i cluster membro è a regime di controllo libero ed è coordinata da una nuova risorsa cluster denominata "cluster set master" (CS-Master). Analogamente a qualsiasi altra risorsa cluster, CS-Master è altamente disponibile e resiliente per i singoli errori del cluster membro e/o per gli errori dei nodi del cluster di gestione. Tramite un nuovo provider WMI per set di cluster, CS-Master fornisce l'endpoint di gestione per tutte le interazioni di gestione dei set di cluster.

**Ruolo di lavoro del set di cluster**

In una distribuzione di set di cluster, il CS-Master interagisce con una nuova risorsa cluster nei cluster membri denominati "cluster set worker" (CS-Worker). CS-Worker funge da unico collegamento sul cluster per orchestrare le interazioni del cluster locale come richiesto dal CS-Master. Esempi di tali interazioni includono la selezione host per macchina virtuale e l'inventario delle risorse locali del cluster. Esiste una sola istanza del ruolo di lavoro CS per ogni cluster membro in un set di cluster. 

**Dominio di errore**

Un dominio di errore è il raggruppamento di elementi software e hardware che l'amministratore determina potrebbe non riuscire insieme quando si verifica un errore.  Sebbene un amministratore possa designare uno o più cluster come dominio di errore, ogni nodo potrebbe partecipare a un dominio di errore in un set di disponibilità. I set di cluster in base alla progettazione lasciano la decisione della determinazione dei limiti del dominio di errore all'amministratore, che è ben versato con data center considerazioni sulla topologia, ad esempio PDU, rete, che i cluster membro condividono. 

**Set di disponibilità**

Un set di disponibilità consente all'amministratore di configurare la ridondanza desiderata dei carichi di lavoro in cluster tra domini di errore, organizzando questi in un set di disponibilità e distribuendo i carichi di lavoro in tale set di disponibilità. Supponiamo che se si distribuisce un'applicazione a due livelli, si consiglia di configurare almeno due macchine virtuali in un set di disponibilità per ogni livello, in modo da garantire che quando un dominio di errore nel set di disponibilità diventa inattivo, l'applicazione avrà almeno una macchina virtuale in ogni livello ospitata in un dominio di errore diverso dello stesso set di disponibilità.

## <a name="why-use-cluster-sets"></a>Perché usare i set di cluster

I set di cluster offrono il vantaggio della scalabilità senza sacrificare la resilienza.  

I set di cluster consentono di raggruppare più cluster per creare un'infrastruttura di grandi dimensioni, mentre ogni cluster rimane indipendente per la resilienza.  Ad esempio, sono disponibili diversi cluster HCI a 4 nodi che eseguono macchine virtuali.  Ogni cluster fornisce la resilienza necessaria per se stessa.  Se lo spazio di archiviazione o la memoria inizia a riempirsi, la scalabilità verticale è il passaggio successivo.  Con la scalabilità verticale sono disponibili alcune opzioni e considerazioni.

1. Aggiungere altro spazio di archiviazione al cluster corrente.  Con Spazi di archiviazione diretta, questo potrebbe essere difficile perché le stesse unità del modello/firmware potrebbero non essere disponibili.  È necessario tenere in considerazione anche i tempi di ricompilazione.
2. Aggiungere ulteriore memoria.  Che cosa accade se si è raggiunto il limite massimo della memoria che i computer sono in grado di gestire?  Cosa accade se tutti gli slot di memoria disponibili sono pieni?
3. Aggiungere nodi di calcolo aggiuntivi con le unità nel cluster corrente.  Questa operazione ci riporta all'opzione 1 che deve essere considerata.
4. Acquistare un intero nuovo cluster

Questo è il punto in cui i set di cluster forniscono i vantaggi della scalabilità.  Se aggiungo i cluster in un set di cluster, posso sfruttare i vantaggi dell'archiviazione o della memoria che potrebbero essere disponibili in un altro cluster senza ulteriori acquisti.  Dal punto di vista della resilienza, l'aggiunta di nodi aggiuntivi a una Spazi di archiviazione diretta non fornirà ulteriori voti per il quorum.  Come indicato in [questo](drive-symmetry-considerations.md)articolo, un cluster spazi di archiviazione diretta può sopravvivere alla perdita di 2 nodi prima di procedere.  Se si dispone di un cluster HCI a 4 nodi, 3 nodi si arrestano per difetto l'intero cluster.  Se si dispone di un cluster a 8 nodi, la disattivazione di 3 nodi comporterà l'intero cluster.  Con i set di cluster con due cluster HCI a 4 nodi nel set, 2 nodi in un HCI si arrestano e 1 nodo nell'altro HCI si arrestano, entrambi i cluster rimangono attivi.  È preferibile creare un cluster di Spazi di archiviazione diretta a 16 nodi di grandi dimensioni o suddividerlo in quattro cluster a 4 nodi e usare i set di cluster?  La presenza di quattro cluster a 4 nodi con set di cluster offre la stessa scalabilità, ma una resilienza migliore in quanto più nodi di calcolo possono rallentare (in modo imprevisto o per la manutenzione) e la produzione rimane.

## <a name="considerations-for-deploying-cluster-sets"></a>Considerazioni per la distribuzione di set di cluster

Quando si considerano i set di cluster che è necessario usare, prendere in considerazione le domande seguenti:

- È necessario superare i limiti di scalabilità di calcolo e archiviazione di HCI correnti?
- Tutte le risorse di calcolo e di archiviazione non sono identiche?
- Si esegue la migrazione in tempo reale di macchine virtuali tra cluster?
- Si desidera che i set di disponibilità di computer e i domini di errore di Azure siano in più cluster?
- È necessario esaminare tutti i cluster per determinare la posizione in cui devono essere inserite le nuove macchine virtuali?

Se la risposta è sì, i set di cluster sono gli elementi necessari.

Ci sono alcuni altri elementi da considerare dove una SDDC più ampia potrebbe modificare le strategie di data center globale.  SQL Server è un esempio valido.  Lo stato di trasferimento di SQL Server macchine virtuali tra cluster richiede l'esecuzione di SQL licenze per i nodi aggiuntivi?  

## <a name="scale-out-file-server-and-cluster-sets"></a>Scalabilità orizzontale file server e set di cluster

In Windows Server 2019 è disponibile un nuovo ruolo di file server con scalabilità orizzontale denominato Infrastructure File server di scalabilità orizzontale (SOFS). 

Le considerazioni seguenti si applicano a un ruolo SOFS dell'infrastruttura:

1.  In un cluster di failover può essere presente al massimo un solo ruolo cluster SOFS di infrastruttura. Il ruolo SOFS dell'infrastruttura viene creato specificando il parametro switch " **-Infrastructure**" per il cmdlet **Add-ClusterScaleOutFileServerRole** .  Ad esempio,

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Ogni volume CSV creato nel failover attiva automaticamente la creazione di una condivisione SMB con un nome generato automaticamente in base al nome del volume CSV. Un amministratore non può creare o modificare direttamente le condivisioni SMB in un ruolo SOFS, oltre che tramite operazioni di creazione/modifica del volume CSV.

3.  Nelle configurazioni iperconvergenti, un SOFS di infrastruttura consente a un client SMB (host Hyper-V) di comunicare con la disponibilità continua garantita (CA) al server SMB SOFS di infrastruttura. Questa CA di loopback SMB iperconvergente viene eseguita tramite macchine virtuali che accedono ai file del disco virtuale (VHDx) in cui l'identità della macchina virtuale proprietaria viene trasmessa tra il client e il server. Questo tipo di inoltri consente ai file VHDx ACL come nelle configurazioni di cluster iperconvergenti standard come prima.

Una volta creato un set di cluster, lo spazio dei nomi del set di cluster si basa su un SOFS di infrastruttura in ognuno dei cluster membri, oltre a un'infrastruttura SOFS nel cluster di gestione.

Quando un cluster membro viene aggiunto a un set di cluster, l'amministratore specifica il nome di un'infrastruttura SOFS nel cluster, se ne esiste già una. Se il SOFS dell'infrastruttura non esiste, viene creato un nuovo ruolo SOFS infrastruttura nel nuovo cluster membro per questa operazione. Se nel cluster membro è già presente un ruolo SOFS dell'infrastruttura, l'operazione di aggiunta la rinomina in modo implicito nel nome specificato, in base alle esigenze. Eventuali server SMB singleton esistenti o ruoli SOFS non di infrastruttura nei cluster membro non vengono utilizzati dal set di cluster. 

Al momento della creazione del set di cluster, l'amministratore ha la possibilità di usare un oggetto computer AD già esistente come radice dello spazio dei nomi nel cluster di gestione. Le operazioni di creazione di set di cluster creano il ruolo del cluster SOFS dell'infrastruttura nel cluster di gestione o rinominano il ruolo SOFS dell'infrastruttura esistente come descritto in precedenza per i cluster di membri. Il SOFS dell'infrastruttura nel cluster di gestione viene usato come riferimento allo spazio dei nomi del set di cluster (spazio dei nomi del set di cluster) SOFS. Significa semplicemente che ogni condivisione SMB nello spazio dei nomi del set di cluster SOFS è una condivisione di riferimento, di tipo ' SimpleReferral ', appena introdotta in Windows Server 2019.  Questo riferimento consente ai client SMB di accedere alla condivisione SMB di destinazione ospitata nel cluster membro SOFS. Il riferimento dello spazio dei nomi del set di cluster SOFS è un meccanismo di riferimento leggero e, di conseguenza, non partecipa al percorso di I/O. I riferimenti SMB vengono memorizzati nella cache perpetuamente su ognuno dei nodi client e il cluster imposta lo spazio dei nomi in modo dinamico aggiorna automaticamente questi riferimenti in base alle esigenze

## <a name="creating-a-cluster-set"></a>Creazione di un set di cluster

### <a name="prerequisites"></a>Prerequisiti

Quando si crea un set di cluster, sono consigliati i prerequisiti seguenti:

1. Configurare un client di gestione che esegue Windows Server 2019.
2. Installare gli strumenti del cluster di failover in questo server di gestione.
3. Creare membri del cluster (almeno due cluster con almeno due volumi condivisi del cluster in ogni cluster)
4. Creare un cluster di gestione (fisico o Guest) che risieda nei cluster di membri.  Questo approccio assicura che il piano di gestione dei set di cluster continui a essere disponibile nonostante i possibili errori del cluster di membri.

### <a name="steps"></a>Passaggi

1. Creare un nuovo set di cluster da tre cluster come definito nei prerequisiti.  Il grafico seguente fornisce un esempio di cluster da creare.  Il nome del cluster impostato in questo esempio sarà **csmaster**.

   | Nome del cluster               | Nome SOFS dell'infrastruttura da usare in un secondo momento | 
   |----------------------------|-------------------------------------------|
   | SET-CLUSTER                | SOFS-CLUSTERSET                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. Una volta creato tutto il cluster, usare i comandi seguenti per creare il master del set di cluster.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Per aggiungere un server del cluster al set di cluster, viene usato il seguente.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Se si usa uno schema di indirizzi IP statici, è necessario includere *-StaticAddress x* . x.x. x nel comando **New-ClusterSet** .

4. Una volta creato il cluster impostato sui membri del cluster, è possibile elencare il set di nodi e le relative proprietà.  Per enumerare tutti i cluster membro nel set di cluster:

        Get-ClusterSetMember -CimSession CSMASTER

5. Per enumerare tutti i cluster di membri nel set di cluster, inclusi i nodi del cluster di gestione:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Per elencare tutti i nodi dei cluster membro:

        Get-ClusterSetNode -CimSession CSMASTER

7. Per elencare tutti i gruppi di risorse nel set di cluster:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Per verificare che il processo di creazione del set di cluster abbia creato una condivisione SMB (identificata come volume1 o la cartella CSV con l'etichetta ScopeName come nome del file server dell'infrastruttura e il percorso come entrambi) nell'infrastruttura SOFS per ogni volume CSV del membro del cluster:

        Get-SmbShare -CimSession CSMASTER

8. I set di cluster dispongono di log di debug che possono essere raccolti per la revisione.  Sia il set di cluster che i log di debug del cluster possono essere raccolti per tutti i membri e il cluster di gestione.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configurare la [delega vincolata](https://techcommunity.microsoft.com/t5/virtualization/live-migration-via-constrained-delegation-with-kerberos-in/ba-p/382334) Kerberos tra tutti i membri del set di cluster.

10. Configurare il tipo di autenticazione della migrazione in tempo reale di una macchina virtuale tra cluster in Kerberos in ogni nodo del set di cluster.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Aggiungere il cluster di gestione al gruppo Administrators locale in ogni nodo del set di cluster.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>Creazione di nuove macchine virtuali e aggiunta a set di cluster

Dopo aver creato il set di cluster, il passaggio successivo consiste nel creare nuove macchine virtuali.  In genere, quando è il momento di creare macchine virtuali e aggiungerle a un cluster, è necessario eseguire alcuni controlli sui cluster per vedere quale potrebbe essere l'esecuzione migliore.  Questi controlli possono includere:

- La quantità di memoria disponibile nei nodi del cluster
- Quanto spazio su disco è disponibile nei nodi del cluster?
- La macchina virtuale richiede requisiti di archiviazione specifici, ad esempio se si desidera che le macchine virtuali SQL Server accedano a un cluster che esegue unità più veloci. in caso contrario, la macchina virtuale dell'infrastruttura non è cruciale e può essere eseguita in unità più lente).

Una volta soddisfatte queste domande, è necessario creare la macchina virtuale nel cluster.  Uno dei vantaggi dei set di cluster è che i set di cluster eseguono tali verifiche e posizionano la macchina virtuale nel nodo più ottimale.

I comandi seguenti consentono di identificare il cluster ottimale e di distribuire la macchina virtuale.  Nell'esempio seguente viene creata una nuova macchina virtuale che specifica che sono disponibili almeno 4 gigabyte di memoria per la macchina virtuale e che sarà necessario utilizzare 1 processore virtuale.

- Assicurarsi che 4GB sia disponibile per la macchina virtuale
- imposta il processore virtuale usato su 1
- Verificare che sia disponibile almeno il 10% di CPU per la macchina virtuale

   ```PowerShell
   # Identify the optimal node to create a new virtual machine
   $memoryinMB=4096
   $vpcount = 1
   $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10
   $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
   $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

   # Deploy the virtual machine on the optimal node
   Invoke-Command -ComputerName $targetnode.name -scriptblock { param([String]$storagepath); New-VM CSVM1 -MemoryStartupBytes 3072MB -path $storagepath -NewVHDPath CSVM.vhdx -NewVHDSizeBytes 4194304 } -ArgumentList @("\\SOFS-CLUSTER1\VOLUME1") -Credential $cred | Out-Null
   
   Start-VM CSVM1 -ComputerName $targetnode.name | Out-Null
   Get-VM CSVM1 -ComputerName $targetnode.name | fl State, ComputerName
   ```

Al termine, verranno fornite le informazioni relative alla macchina virtuale e alla posizione in cui sono state inserite.  Nell'esempio precedente, verrebbe visualizzato come:

        State         : Running
        ComputerName  : 1-S2D2

Se non si dispone di memoria sufficiente, CPU o spazio su disco per aggiungere la macchina virtuale, verrà visualizzato l'errore seguente:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Una volta creata, la macchina virtuale verrà visualizzata nella console di gestione di Hyper-V nel nodo specifico specificato.  Per aggiungerlo come macchina virtuale del set di cluster e nel cluster, il comando è riportato di seguito.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Al termine dell'operazione, l'output sarà:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Se è stato aggiunto un cluster con macchine virtuali esistenti, sarà necessario registrare anche le macchine virtuali con i set di cluster, in modo da registrare tutte le macchine virtuali contemporaneamente. il comando da usare è:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

Tuttavia, il processo non è completo perché il percorso della macchina virtuale deve essere aggiunto allo spazio dei nomi del set di cluster.

Quindi, ad esempio, viene aggiunto un cluster esistente e le macchine virtuali preconfigurate si trovano nel Volume condiviso cluster locale (CSV), il percorso per il VHDX è simile a "C:\ClusterStorage\Volume1\MYVM\Virtual hard Disks\MYVM.vhdx.  È necessario eseguire una migrazione dell'archiviazione perché i percorsi CSV sono progettati in locale in un singolo cluster membro. Quindi, non sarà accessibile alla macchina virtuale dopo la migrazione in tempo reale tra i cluster membri. 

In questo esempio, CLUSTER3 è stato aggiunto al set di cluster usando Add-ClusterSetMember con l'infrastruttura File server di scalabilità orizzontale come SOFS-CLUSTER3.  Per spostare la configurazione e l'archiviazione della macchina virtuale, il comando è:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Al termine, verrà visualizzato un avviso:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Questo avviso può essere ignorato perché l'avviso è "non sono state rilevate modifiche nella configurazione dell'archiviazione del ruolo macchina virtuale".  Il motivo dell'avviso perché la posizione fisica effettiva non cambia; solo i percorsi di configurazione. 

Per ulteriori informazioni su Move-VMStorage, consultare questo [collegamento](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

La migrazione in tempo reale di una macchina virtuale tra cluster di set di cluster diversi non è uguale a quella del passato. Negli scenari di set non cluster, i passaggi sono i seguenti:

1. rimuovere il ruolo di macchina virtuale dal cluster.
2. eseguire la migrazione in tempo reale della macchina virtuale in un nodo membro di un cluster diverso.
3. aggiungere la macchina virtuale al cluster come nuovo ruolo macchina virtuale.

Con cluster imposta questi passaggi non sono necessari ed è necessario un solo comando.  Per prima cosa, è necessario impostare tutte le reti in modo che siano disponibili per la migrazione con il comando:

    Set-VMHost -UseAnyNetworkForMigration $true

Ad esempio, voglio spostare una macchina virtuale del set di cluster da CLUSTER1 a NODE2-CL3 in CLUSTER3.  Il singolo comando è:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Si noti che questa operazione non consente di spostare i file di configurazione o di archiviazione della macchina virtuale.  Questa operazione non è necessaria perché il percorso della macchina virtuale rimane come \\SOFS-CLUSTER1\VOLUME1.  Quando una macchina virtuale è stata registrata con i set di cluster ha il percorso di condivisione del file server dell'infrastruttura, le unità e la macchina virtuale non devono essere nello stesso computer della macchina virtuale.

## <a name="creating-availability-sets-fault-domains"></a>Creazione di gruppi di disponibilità domini di errore

Come descritto nell'introduzione, i domini di errore e i set di disponibilità di tipo Azure possono essere configurati in un set di cluster.  Questa operazione è utile per le migrazioni e le migrazioni iniziali delle macchine virtuali tra i cluster.  

Nell'esempio seguente sono presenti quattro cluster che partecipano al set di cluster.  All'interno del set verrà creato un dominio di errore logico con due cluster e un dominio di errore creato con gli altri due cluster.  Questi due domini di errore costituiranno il set di disponibilità. 

Nell'esempio seguente CLUSTER1 e CLUSTER2 si troveranno in un dominio di errore denominato **FD1** mentre CLUSTER3 e CLUSTER4 si troveranno in un dominio di errore denominato **FD2**.  Il set di disponibilità sarà denominato **csmaster-As** e sarà costituito dai due domini di errore.

Per creare i domini di errore, i comandi sono:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Per assicurarsi che siano stati creati correttamente, è possibile eseguire Get-ClusterSetFaultDomain con l'output visualizzato.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Ora che sono stati creati i domini di errore, è necessario creare il set di disponibilità.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Per convalidare che è stato creato, usare:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Quando si creano nuove macchine virtuali, è necessario usare il parametro-abilitate per come parte della determinazione del nodo ottimale.  Quindi, avrà un aspetto simile al seguente:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Rimozione di un cluster da set di cluster a causa di diversi cicli di vita. In alcuni casi è necessario rimuovere un cluster da un set di cluster. Come procedura consigliata, tutte le macchine virtuali del set di cluster devono essere spostate all'esterno del cluster. Questa operazione può essere eseguita usando i comandi **Move-ClusterSetVM** e **Move-VMStorage** .

Tuttavia, se anche le macchine virtuali non vengono spostate, i set di cluster eseguono una serie di azioni per fornire un risultato intuitivo all'amministratore.  Quando il cluster viene rimosso dal set, tutte le macchine virtuali del set di cluster rimanenti ospitate nel cluster da rimuovere diventeranno semplicemente macchine virtuali a disponibilità elevata associate a tale cluster, presumendo che abbiano accesso alla propria archiviazione.  I set di cluster aggiornano inoltre automaticamente l'inventario per:

- Non è più possibile tenere traccia dello stato del cluster ora rimosso e delle macchine virtuali in esecuzione
- Rimuove dallo spazio dei nomi del set di cluster e tutti i riferimenti alle condivisioni ospitate nel cluster ora rimosso

Ad esempio, il comando per rimuovere il cluster CLUSTER1 dai set di cluster sarà:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>Domande frequenti

**Domanda:** Nel set di cluster sono limitati a usare solo cluster iperconvergenti? <br>
**Risposta:** No.  È possibile combinare Spazi di archiviazione diretta con i cluster tradizionali.

**Domanda:** È possibile gestire il set di cluster tramite System Center Virtual Machine Manager? <br>
**Risposta:** System Center Virtual Machine Manager attualmente non supporta i set di cluster <br><br> **Domanda:** I cluster Windows Server 2012 R2 o 2016 possono coesistere nello stesso set di cluster? <br>
**Domanda:** È possibile eseguire la migrazione dei carichi di lavoro dai cluster Windows Server 2012 R2 o 2016 semplicemente se tali cluster si aggiungono allo stesso set di cluster? <br>
**Risposta:** I set di cluster sono una nuova tecnologia introdotta in Windows Server 2019, pertanto non esiste nelle versioni precedenti. I cluster basati su sistema operativo legacy non possono essere aggiunti a un set di cluster. Tuttavia, la tecnologia di aggiornamento in sequenza del sistema operativo del cluster deve fornire la funzionalità di migrazione che si sta cercando aggiornando questi cluster a Windows Server 2019.

**Domanda:** I set di cluster possono ridimensionare l'archiviazione o il calcolo (da solo)? <br>
**Risposta:** Sì, aggiungendo semplicemente un cluster Hyper-V con spazio di archiviazione diretto o tradizionale. Con i set di cluster, si tratta di una semplice modifica del rapporto tra calcolo e archiviazione anche in un set di cluster iperconvergente.

**Domanda:** Quali sono gli strumenti di gestione per i set di cluster <br>
**Risposta:** PowerShell o WMI in questa versione.

**Domanda:** In che modo è possibile usare la migrazione in tempo reale tra cluster con processori di generazioni diverse?  <br>
**Risposta:** I set di cluster non sono compatibili con le differenze del processore e sostituiscono quelle attualmente supportate da Hyper-V.  Pertanto, è necessario utilizzare la modalità di compatibilità del processore con migrazioni rapide.  Il Consiglio per i set di cluster consiste nell'utilizzare lo stesso hardware del processore all'interno di ogni singolo cluster, nonché l'intero set di cluster per le migrazioni in tempo reale tra i cluster.

**Domanda:** Il cluster può impostare automaticamente il failover delle macchine virtuali in caso di errore del cluster?  <br>
**Risposta:** In questa versione, le macchine virtuali del set di cluster possono essere migrate solo manualmente tra i cluster. ma non è possibile eseguire il failover automatico. 

**Domanda:** Come è possibile garantire che l'archiviazione sia resiliente agli errori del cluster? <br>
**Risposta:** Usare la soluzione di replica di archiviazione tra cluster (SR) tra i cluster di membri per realizzare la resilienza dell'archiviazione in caso di errori del cluster.

**Domanda:** Si usa la replica di archiviazione (SR) per eseguire la replica tra I cluster di membri. I percorsi UNC dello spazio dei nomi del set di cluster cambiano nel failover SR alla destinazione di replica Spazi di archiviazione diretta cluster? <br>
**Risposta:** In questa versione, la modifica del riferimento dello spazio dei nomi del set di cluster non si verifica con il failover SR. Microsoft sa se questo scenario è fondamentale per l'utente e come si prevede di usarlo.

**Domanda:** È possibile eseguire il failover delle macchine virtuali tra domini di errore in una situazione di ripristino di emergenza (ad eccezione del fatto che l'intero dominio di errore è stato arrestato)? <br>
**Risposta:** No. si noti che il failover tra cluster all'interno di un dominio di errore logico non è ancora supportato. 

**Domanda:** Il cluster può estendersi sui cluster in più siti o domini DNS? <br>**risposta  
:** si tratta di uno scenario non testato e non pianificato immediatamente per il supporto di produzione. Microsoft sa se questo scenario è fondamentale per l'utente e come si prevede di usarlo.

**Domanda:** Il set di cluster funziona con IPv6? <br>
**Risposta:** Sia IPv4 che IPv6 sono supportati con i set di cluster come per i cluster di failover.

**Domanda:** Quali sono i requisiti della foresta Active Directory per i set di cluster <br>
**Risposta:** Tutti i cluster membro devono trovarsi nella stessa foresta di Active Directory.

**Domanda:** Quanti cluster o nodi possono far parte di un singolo set di cluster? <br>
**Risposta:** In Windows Server 2019 i set di cluster sono stati testati e sono supportati fino a 64 nodi totali del cluster. Tuttavia, il cluster imposta la scalabilità dell'architettura a limiti molto più grandi e non è un elemento hardcoded per un limite. Microsoft sa se la scalabilità più ampia è fondamentale per l'utente e il modo in cui si prevede di usarla.

**Domanda:** Tutti i cluster Spazi di archiviazione diretta in un set di cluster formano un singolo pool di archiviazione? <br>
**Risposta:** No. Spazi di archiviazione diretta tecnologia opera comunque all'interno di un singolo cluster e non tra i cluster membro in un set di cluster.

**Domanda:** Lo spazio dei nomi del set di cluster è a disponibilità elevata? <br>
**Risposta:** Sì, lo spazio dei nomi del set di cluster viene fornito tramite un server dello spazio dei nomi SOFS a disponibilità continua (CA) in esecuzione nel cluster di gestione. Microsoft consiglia di disporre di un numero sufficiente di macchine virtuali dai cluster membro per renderlo resiliente agli errori localizzati a livello di cluster. Tuttavia, per tenere conto di errori irreversibili non previsti, ad esempio tutte le macchine virtuali nel cluster di gestione si arrestano contemporaneamente, le informazioni di riferimento vengono memorizzate nella cache in modo permanente in ogni nodo del set di cluster, anche in caso di riavvio.
 
**Domanda:** L'accesso di archiviazione basato su spazio dei nomi rallenta le prestazioni di archiviazione in un set di cluster? <br>
**Risposta:** No. Lo spazio dei nomi del set di cluster offre uno spazio dei nomi di riferimento sovrapposto all'interno di un cluster, concettualmente come file system distribuito spazi dei nomi (DFSN) A differenza di DFSN, tutti i metadati del riferimento dello spazio dei nomi del set di cluster vengono compilati automaticamente e aggiornati automaticamente in tutti i nodi senza alcun intervento da parte dell'amministratore, quindi non si verifica alcun sovraccarico delle prestazioni nel percorso di accesso di archiviazione. 

**Domanda:** Come è possibile eseguire il backup dei metadati del set di cluster? <br>
**Risposta:** Questa guida è identica a quella del cluster di failover. Il backup dello stato del sistema eseguirà il backup anche dello stato del cluster.  Tramite Windows Server Backup, è possibile eseguire il ripristino di un solo database cluster del nodo, che non dovrebbe mai essere necessario a causa di una serie di logica di correzione automatica, oppure eseguire un ripristino autorevole per eseguire il rollback dell'intero database del cluster in tutti i nodi. Nel caso dei set di cluster, Microsoft consiglia di eseguire un ripristino autorevole per primo sul cluster membro e quindi il cluster di gestione, se necessario.
