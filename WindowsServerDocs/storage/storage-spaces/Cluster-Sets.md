---
title: Set di cluster
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: Questo articolo descrive lo scenario di set di Cluster
ms.localizationpriority: medium
ms.openlocfilehash: 2deeb6968f910e80bacb2354ad2e575060a7797a
ms.sourcegitcommit: 07aefbdbb0eedb42aaed3d195c2429310c761da0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2019
ms.locfileid: "9041007"
---
# Set di cluster

> Si applica a: Build di Windows Server Insider Preview 17650 e versioni successive

Set di cluster è la nuova tecnologia di scalabilità orizzontale cloud in questa versione di anteprima che aumenta il numero di nodo del cluster in un singolo cloud Software Defined Data Center (SDDC) di ordini di grandezza. Un set di cluster è un raggruppamento ad accoppiamento ridotto di più cluster di Failover: elaborazione, archiviazione o iperconvergente. Cluster imposta tecnologia consente fluidità macchina virtuale a cluster di membro all'interno di un set di cluster e uno spazio dei nomi di archiviazione unificato tra il set in seguito a fluidità macchina virtuale. 

Anche se si verificano conservazione esistente di gestione dei Cluster di Failover in cluster di membro, un'istanza di set di cluster offre inoltre casi di utilizzo chiavi attorno all'aggregato della gestione del ciclo di vita. Questa Guida alla valutazione Scenario di anteprima di Windows Server offre le informazioni necessarie in background insieme a istruzioni dettagliate per valutare la tecnologia di set di cluster con PowerShell. 

## Introduzione tecnologia

Tecnologia di set di cluster è sviluppata per soddisfare le richieste di clienti specifici operativo nuvole Software Defined Datacenter (SDDC) con una scala. Proposta di valore ai set di cluster in questa versione di anteprima potrà essere riassunta come indicato di seguito:  

- Migliorare notevolmente la scala di cloud SDDC supportata per l'esecuzione di macchine virtuali a disponibilità elevata grazie alla combinazione di più cluster più piccoli in un'unica infrastruttura di grandi dimensioni, anche mantenendo il limite di errore di software di un singolo cluster
- Gestire l'intero ciclo di vita del Cluster di Failover tra cui onboarding e il ritiro dei cluster, senza alcun impatto sulla disponibilità di macchina virtuale tenant, tramite fluido la migrazione di macchine virtuali tra questa infrastruttura di grandi dimensioni
- Modificare facilmente il rapporto di calcolo-per-archiviazione nella tua iperconvergente I
- Trarre vantaggio da [domini di errore Azure simili e disponibilità imposta](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) in cluster nella macchina virtuale iniziale posizionamento e la migrazione di macchina virtuale successivi
- Combinazione-and-match appartengono a generazioni diverse di hardware CPU nello stesso cluster imposta dell'infrastruttura, anche mantenendo domini di errore singoli omogeneo per la massima efficienza.  Tieni presente che la raccomandazione del stesso hardware è ancora presente all'interno di ogni cluster, nonché il set di intero cluster.

Da una visualizzazione di alto livello, questo è il cluster set possono essere simile.

![Cluster imposta soluzione visualizzazione](media\Cluster-sets-Overview\Cluster-sets-solution-View.png)

Di seguito fornisce un breve riepilogo di tutti gli elementi nell'immagine precedente:

**Cluster di gestione**

Gestione cluster in un set di cluster è un Cluster di Failover che ospita il piano di gestione disponibilità elevata del set intero cluster e la segnalazione dello spazio dei nomi (Cluster impostare Namespace) di archiviazione unificato del File Server di scalabilità orizzontale (SOFS). Un cluster di gestione è logicamente separato dal cluster membro che eseguono carichi di lavoro macchina virtuale. In questo modo il cluster impostare resiliente piano di gestione di eventuali errori a livello di cluster localizzate, ad esempio, interruzione dell'alimentazione di un cluster di membro.   

**Cluster di membro**

Un cluster di membro in un set di cluster è in genere un cluster iper-convergenti tradizionale in esecuzione di macchine virtuali e i carichi di lavoro spazi di archiviazione diretta. Più cluster di membro partecipare a una distribuzione set singolo cluster, che formano l'infrastruttura cloud SDDC più grande. Cluster di membro diversa da un cluster di gestione in due aspetti fondamentali: cluster membro partecipare al dominio di errore e disponibilità imposta costrutti cluster membro anche vengono ridimensionati di macchina virtuale dell'host e i carichi di lavoro spazi di archiviazione diretta. Macchine virtuali di set di cluster che spostano i limiti del cluster in un set di cluster non devono essere ospitate nel cluster di gestione per questo motivo.

**Riferimento dello spazio dei nomi SOFS set di cluster**

Un riferimento di spazio dei nomi di set di cluster (Cluster impostare Namespace) SOFS è un File Server di scalabilità orizzontale in cui ogni condivisione SMB in Cluster impostare Namespace SOFS è una condivisione di riferimento, di tipo 'SimpleReferral' appena introdotte in questa versione di anteprima.  Questa segnalazione consente l'accesso alla destinazione di condivisione SMB ospitata nel cluster di membro SOFS client Server Message Block (SMB). Il cluster Imposta riferimento dello spazio dei nomi SOFS è un meccanismo di segnalazione leggero e di conseguenza, non partecipa nel percorso dei / o. Vengono memorizzate nella cache definitiva le segnalazioni di SMB su ognuno dei nodi del client e lo spazio dei nomi set di cluster in modo dinamico gli aggiornamenti automaticamente questi riferimenti in base alle esigenze.

**Set di cluster master**

In un set di cluster, la comunicazione tra i cluster di membro viene ad accoppiamento e viene coordinata da una nuova risorsa cluster denominata "Cluster impostare Master" (CS-Master). Come qualsiasi altra risorsa di cluster, CS-Master è altamente disponibile e di errori di singoli membri del cluster e/o gli errori dei nodi del cluster la gestione. Tramite un nuovo provider WMI impostare Cluster, CS-Master fornisce l'endpoint di gestione per tutte le interazioni gestibilità Set di Cluster.

**Ruolo di lavoro di set di cluster**

In una distribuzione Set di Cluster, CS-Master interagisce con una nuova risorsa di cluster nel membro cluster denominato "Cluster impostare Worker" (CS-Worker). CS-Worker opera come il collegamento solo sul cluster per progettare le interazioni di cluster locale come richiesto del CS-Master. Sono esempi di tali interazioni posizionamento di macchina virtuale e inventario delle risorse locali cluster. Esiste una sola istanza CS-Worker per ogni membro del cluster in un set di cluster. 

**Dominio di errore**

Un dominio di errore è il raggruppamento di software e artefatti hardware che determina l'amministratore potrebbero non riuscire insieme quando si verifica un errore.  Anche se un amministratore potrebbe definire uno o più cluster insieme come un dominio di errore, ogni nodo può partecipare a un dominio di errore in un set di disponibilità. Imposta cluster con progettazione foglie la decisione di determinazione limite dominio di errore per l'amministratore che è ben versed con considerazioni sulla topologia di data center-PDU, ad esempio, di rete-che condividono cluster membro. 

**Set di disponibilità**

Un set di disponibilità consente all'amministratore di configurare ridondanza desiderata di carichi di lavoro cluster tra domini di errore, organizzando quelle in un set di disponibilità e distribuzione di carichi di lavoro in tale set di disponibilità. Supponiamo che se stai distribuendo un'applicazione a due livelli, ti consigliamo di configurare almeno due macchine virtuali in una disponibilità impostare per ogni livello che garantisce che quando un dominio di errore in questo set di disponibilità diventa non disponibile, l'applicazione avrà almeno una macchina virtuale in ogni livello ospitato in un dominio di errore diverse di tale stesso set di disponibilità.

## Perché usare set di cluster

Set di cluster offre il vantaggio di scala senza compromettere la resilienza.  

Set di cluster consente per il clustering di più cluster insieme per creare un'infrastruttura di grandi dimensioni, mentre ogni cluster resta indipendente per la resilienza.  Ad esempio, avere un diversi HCI 4 nodi cluster di macchine virtuali in esecuzione.  Ogni cluster offre la flessibilità necessaria per la stessa.  Se la memoria o archiviazione viene avviato riempimento, scalabilità verticale è il passaggio successivo.  Con la scalabilità verticale, esistono alcune considerazioni e le opzioni.

1. Aggiungere uno spazio di archiviazione al cluster corrente.  Con spazi di archiviazione diretta questo può essere difficile, come le unità di modello/firmware stesso esatto potrebbero non essere disponibili.  Prendere in considerazione la ricompilazione volte necessario anche essere presi in considerazione.
2. Aggiungere più memoria.  Cosa succede se sono raggiunge il limite massimo nella memoria in grado di gestire i computer?  Cosa succede se tutti gli slot di memoria disponibile vengono completi?
3. Aggiungere nodi di calcolo aggiuntive con unità nel cluster corrente.  Questa operazione richiede ci torna all'opzione 1 dover essere considerato.
4. Acquisto di un cluster completamente nuovo

Si tratta in cui il set di cluster offre il vantaggio di ridimensionamento.  Se aggiunge il mio cluster in un set di cluster, è possibile sfruttare di archiviazione o di memoria che potrebbe essere disponibile in un altro cluster senza eventuali altri acquisti.  Dal punto di vista resilienza, aggiunta di nodi aggiuntivi a un spazi di archiviazione diretta non sta per fornire ulteriori voti per quorum.  Come indicato [di seguito](drive-symmetry-considerations.md), un Cluster direttamente spazi di archiviazione può sostenere la perdita di 2 nodi prima di procedere verso il basso.  Se disponi di un cluster HCI a 4 nodi, 3 nodi Vai verso il basso richiederà l'intero cluster verso il basso.  Se disponi di un cluster di 8 nodi, 3 nodi Vai verso il basso richiederà l'intero cluster verso il basso.  Con set di Cluster con due cluster HCI 4 nodi nel set di 2 nodi in una HCI Vai verso il basso e 1 nodo nell'altro HCI Vai verso il basso, entrambi i cluster rimangano alto.  È preferibile creare un cluster di spazi di archiviazione diretta 16 nodi grandi o suddividerlo in quattro 4 nodi cluster e usare set di cluster?  Con quattro 4 nodi cluster con cluster set fornisce la stessa scala, ma la resilienza migliore in quanto più nodi di calcolo possono andare verso il basso (in modo imprevisto o di manutenzione) e rimane di produzione.

## Considerazioni per la distribuzione di set di cluster

Quando valuti se set di cluster è un elemento è necessario utilizzare, Prendi in considerazione queste domande:

- È necessario superando corrente HCI calcolo e archiviazione scala?
- Tutti i calcolo e archiviazione non sono in modo identico lo stesso?
- Se attivi migrare le macchine virtuali tra i cluster?
- Che vuoi set di disponibilità del computer di Azure simili e domini di errore a più cluster?
- È necessario eseguire il momento di esaminare tutti i cluster per determinare dove devono essere posizionati eventuali nuove macchine virtuali?

Se la tua risposta è Sì, i set di cluster è ciò che è necessario.

Esistono alcuni altri aspetti da considerare in cui un grande SDDC potrebbero cambiare le strategie di centro dati complessiva.  SQL Server è un buon esempio.  Macchine virtuali di SQL Server spostare tra i cluster richiede licenze SQL per l'esecuzione nei nodi aggiuntivi?  

## Server di scalabilità orizzontale file e i set di cluster

In Windows Server 2019, esiste un nuovo ruolo server di scalabilità orizzontale file denominato infrastruttura Scale-Out File Server (SOFS). 

Le considerazioni seguenti si applicano a un ruolo SOFS dell'infrastruttura:

1.  In un Cluster di Failover possono essere presenti al massimo solo un ruolo del cluster SOFS dell'infrastruttura. Ruolo di infrastruttura SOFS viene creato, specificando il "**-infrastruttura**" passare parametro al cmdlet **Add-ClusterScaleOutFileServerRole** .  Ad esempio:

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Ogni volume CSV creato automaticamente il failover attiva la creazione di una condivisione SMB con un nome generato automaticamente in base al nome di volume CSV. Un amministratore non può direttamente creare o modificare le condivisioni SMB in un ruolo SOFS, diverso da tramite operazioni di creare/modificare volume CSV.

3.  In configurazioni iperconvergente, SOFS un'infrastruttura consente a un client SMB (host Hyper-V) per comunicare con garantito continuo disponibilità (CA) al server di infrastruttura SOFS SMB. Questo iperconvergente SMB loopback della CA viene ottenuto tramite macchine virtuali di accesso ai loro file di disco virtuale (. VHDx) in cui viene inoltrato il proprietario di macchina virtuale identità tra il client e server. Questo invio identità consente ai file VHDx ACL-uso proprio come in configurazioni con cluster iper-convergenti standard come prima.

Dopo aver creato un set di cluster, lo spazio dei nomi set di cluster si basa su un'infrastruttura SOFS su ognuno dei cluster membro e inoltre SOFS un'infrastruttura del cluster di gestione.

Al momento un cluster di membro viene aggiunto a un set di cluster, l'amministratore specifica il nome del SOFS infrastruttura su tale cluster, se ne esiste già. Se SOFS infrastruttura non è presente, viene creato un nuovo ruolo infrastruttura SOFS nel nuovo cluster membro da questa operazione. Se esiste già un ruolo di infrastruttura SOFS nel cluster di membro, l'operazione di aggiunta in modo implicito viene rinominato per il nome specificato in base alle esigenze. Qualsiasi server SMB singleton esistenti o i ruoli non - infrastruttura SOFS il membro cluster rimangono unutilized dal set di cluster. 

Al momento viene creato il set di cluster, l'amministratore ha la possibilità di usare un oggetto computer di Active Directory esistente come radice dello spazio dei nomi nel cluster di gestione. Operazioni di creazione di set di cluster creare il ruolo cluster infrastruttura SOFS nel cluster di gestione o Rinomina il ruolo SOFS dell'infrastruttura esistente semplicemente come descritto in precedenza per i cluster di membro. SOFS nel cluster di gestione dell'infrastruttura viene usato come riferimento dello spazio dei nomi (Cluster impostare Namespace) SOFS set di cluster. Significa semplicemente che ogni condivisione SMB nel cluster di impostare dello spazio dei nomi che SOFS è una condivisione di riferimento, di tipo 'SimpleReferral' - appena introdotta in questa versione di anteprima.  Questa segnalazione consente l'accesso client SMB per la destinazione di condivisione SMB ospitato nel cluster di membro SOFS. Il cluster Imposta riferimento dello spazio dei nomi SOFS è un meccanismo di segnalazione leggero e di conseguenza, non partecipa nel percorso dei / o. Vengono memorizzate nella cache definitiva le segnalazioni di SMB su ognuno dei nodi del client e lo spazio dei nomi set di cluster in modo dinamico gli aggiornamenti automaticamente questi riferimenti in base alle esigenze

## Creazione di un Set di Cluster

### Prerequisiti

Durante la creazione di un cluster set, attenersi alla seguente prerequisiti sono consigliati:

1. Configurare un client di gestione che esegue la versione più recente di Windows Server Insider.
2. Installare gli strumenti di Cluster di Failover in questo server di gestione.
3. Creare i membri del cluster (almeno due cluster con volumi condivisi Cluster almeno due in ogni cluster)
4. Creare un cluster di gestione (fisico o guest) che supera i cluster di membro.  Questo approccio assicura che i set di Cluster piano gestione continua a essere disponibile indipendentemente dagli errori di cluster membro possibili.

### Passaggi

1. Creare un nuovo cluster impostato dal cluster di tre come definito nei prerequisiti.  Di seguito grafico offre un esempio di cluster per creare.  Il nome del cluster in questo esempio impostato sarà **CSMASTER**.

   | Nome del cluster               | Infrastruttura SOFS nome per l'uso in un secondo momento | 
   |----------------------------|-------------------------------------------|
   | SET-CLUSTER                | SOFS-CLUSTERSET                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. Dopo aver creato tutti i cluster, usare i comandi seguenti per creare il cluster set master.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Per aggiungere un Server di Cluster per il set di cluster, di seguito può essere usata.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Se usi uno schema di indirizzo IP statico, è necessario includere *x.x.x. x - StaticAddress* sul comando **New-ClusterSet** .

4. Dopo aver creato il cluster impostare esplicitamente i membri del cluster, è possibile elencare il set di nodi e le relative proprietà.  Per enumerare tutti i cluster di membro nel set di cluster:

        Get-ClusterSetMember -CimSession CSMASTER

5. Per enumerare tutti i cluster di membro del cluster impostare tra i nodi del cluster gestione:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Per visualizzare l'elenco di tutti i nodi dai cluster di membro:

        Get-ClusterSetNode -CimSession CSMASTER

7. Per visualizzare l'elenco tutti i gruppi di risorse tra il set di cluster:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Per verificare il cluster impostare il processo di creazione creato una condivisione SMB (identificata come Volume1 o qualsiasi cartella CSV con etichetta con il ScopeName essere il nome del File Server di infrastruttura e il percorso sia come) sul SOFS infrastruttura per ogni membro cluster Volume CSV:

        Get-SmbShare -CimSession CSMASTER

8. Set di cluster ha i registri di debug che possono essere raccolti per la revisione.  Imposta il cluster e i registri di debug cluster possono essere raccolti per tutti i membri e il cluster di gestione.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configurare Kerberos [delega vincolata](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/) tra tutti i membri di set di cluster.

10. Configurare il tipo di autenticazione macchina virtuale di cross-cluster migrazione in tempo reale per Kerberos in ogni nodo nel Set di Cluster.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Aggiungere il cluster di gestione al gruppo administrators locale su ciascun nodo nel set di cluster.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## Creazione di nuove macchine virtuali e l'aggiunta a set di cluster

Dopo la creazione del cluster, il passaggio successivo consiste nel creare nuove macchine virtuali.  In genere, quando è il momento di creare macchine virtuali e aggiungerli a un cluster, è necessario eseguire alcuni controlli sui cluster per verificare che potrebbe essere preferibile eseguire.  Questi controlli potrebbero includere:

- Quantità di memoria è disponibile nei nodi del cluster?
- Spazio su disco è disponibile nei nodi del cluster?
- La macchina virtuale richiede i requisiti di spazio di archiviazione specifico (ad esempio, è possibile mie macchine virtuali di SQL Server per passare a un cluster che esegue le unità più veloci; o, la macchina virtuale dell'infrastruttura non è essenziale e può essere eseguita su unità più lente).

Una volta questo domande, creare la macchina virtuale nel cluster di cui è che necessario essere.  Uno dei vantaggi dei set di cluster è che il set di cluster fare i controlli per te e posiziona la macchina virtuale nel nodo ottimale.

Di seguito comandi verranno di identificare il cluster ottimale e distribuire la macchina virtuale.  Nel seguente esempio viene creata una nuova macchina virtuale che specifica che almeno 4 GB di memoria è disponibile per la macchina virtuale e che dovrà usare 1 del processore virtuale.

- Assicurati che sia disponibile per la macchina virtuale 4gb
- impostare il processore virtuale usato in fase di 1
- Verificare che c'è almeno 10% della CPU disponibile per la macchina virtuale

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

Al termine, sarà possibile le informazioni relative alla macchina virtuale e in cui è stata inserita.  Nell'esempio precedente verrebbe visualizzate come:

        State         : Running
        ComputerName  : 1-S2D2

Se fosse necessario non hanno sufficiente memoria, cpu o spazio su disco per aggiungere la macchina virtuale, Ricevi l'errore:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Dopo aver creata la macchina virtuale, verrà visualizzato nella console di gestione Hyper-V sul nodo specifico specificato.  Per aggiungerlo come set di un cluster di macchina virtuale e nel cluster, il comando è di sotto.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Al termine, l'output sarà:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Se hai aggiunto un cluster con le macchine virtuali esistenti, le macchine virtuali dovranno anche deve essere registrata con Cluster imposta quindi Registra tutte le macchine virtuali in una sola volta, il comando da utilizzare è:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

Tuttavia, il processo non è completo, come il percorso alla macchina virtuale deve essere aggiunto allo spazio dei nomi set di cluster.

Ad esempio, viene aggiunto un cluster esistente e include le macchine virtuali preconfigurate si trovano nel locale Cluster Volume condiviso, il percorso per il VHDX sarebbe qualcosa di simile a "C:\ClusterStorage\Volume1\MYVM\Virtual Disks\MYVM.vhdx disco rigido.  Una migrazione di archiviazione dovrà essere eseguito come CSV percorsi sono in base alla progettazione locale a un cluster singolo membro. Di conseguenza, non saranno accessibili alla macchina virtuale dopo che sono attivi la migrazione a cluster di membro. 

In questo esempio, CLUSTER3 è stato aggiunto al cluster impostata tramite Add-ClusterSetMember con il File Server di scalabilità orizzontale infrastruttura come SOFS-CLUSTER3.  Per spostare la configurazione della macchina virtuale e l'archiviazione, il comando è:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Termine, riceverai un avviso:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

È possibile ignorare questo avviso come l'avviso è "sono stati rilevati apportare alcuna modifica la configurazione di archiviazione dei ruoli di macchina virtuale".  Il motivo per l'avviso come la posizione fisica effettiva non cambia; solo i percorsi di configurazione. 

Per altre informazioni su Move-VMStorage, esamina il [collegamento](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

Live la migrazione di una macchina virtuale tra i cluster diverso set di cluster è non lo stesso come in passato. In scenari set senza cluster, sarebbe i passaggi:

1. rimuovere il ruolo di macchina virtuale dal Cluster.
2. animati eseguire la migrazione della macchina virtuale a un nodo del membro di un cluster diverso.
3. aggiungere la macchina virtuale nel cluster come un nuovo ruolo di macchina virtuale.

Con il set di Cluster questi passaggi non sono necessari e un solo comando è necessario.  Ad esempio, è possibile spostare una macchina virtuale Set di Cluster da CLUSTER1 a nodo 2-CL3 su CLUSTER3.  Il comando singolo sarebbe:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Tieni presente che questo non si sposta i file di archiviazione o configurazione macchina virtuale.  Questo non è necessario il percorso alla macchina virtuale rimane come \\SOFS-CLUSTER1\VOLUME1.  Una volta una macchina virtuale è stata registrata con il set di cluster è il percorso di condivisione File Server di infrastruttura, le unità e la macchina virtuale non richiedono viene nello stesso computer come macchina virtuale.

## Disponibilità di creazione di set di domini di errore

Come descritto nell'introduzione, domini di errore di Azure simili e disponibilità set possono essere configurati in un set di cluster.  Ciò è utile per la macchina virtuale iniziale posizionamenti e le migrazioni tra i cluster.  

Nell'esempio seguente, ci sono quattro cluster che fanno parte del set di cluster.  All'interno del set, verrà creato un dominio di errore logico con due dei cluster e un dominio di errore creato con gli altri due cluster.  Questi domini di due errore includerà il Set di Availabiilty. 

Nell'esempio seguente, CLUSTER1 e CLUSTER2 è in un dominio di errore chiamato **FD1** mentre CLUSTER3 e CLUSTER4 sarà in un dominio di errore chiamato **FD2**.  Il set di disponibilità verrà chiamato **CSMASTER-AS** ed essere costituito dai domini di due errore.

Per creare i domini di errore, i comandi sono:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Per garantire che sono stati creati correttamente, Get-ClusterSetFaultDomain può essere eseguito con il relativo output mostrato.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Dopo aver creati i domini di errore, la disponibilità imposta deve essere creato.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Per eseguire la convalida è stato creato, quindi utilizzare:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Durante la creazione di nuove macchine virtuali, quindi dovresti usare il parametro - AvailabilitySet come parte di determinazione del nodo ottimale.  Pertanto quindi dovrebbe essere simile al seguente:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Rimozione di un cluster dal cluster imposta a causa di diversi cicli di vita. In alcuni casi, quando un cluster deve essere rimosso da un set di cluster. Come procedura consigliata, tutte le macchine virtuali set di cluster deve essere spostate all'esterno del cluster. Questa operazione può essere eseguita tramite i comandi di **Spostamento-ClusterSetVM** e **Move-VMStorage** .

Tuttavia, se non verrà spostate anche le macchine virtuali, i set di cluster esegue una serie di azioni per fornire un risultato intuitivo per l'amministratore.  Quando il cluster viene rimossa dal set, tutti i rimanenti cluster set di macchine virtuali ospitate nel cluster viene rimosso diventerà semplicemente le macchine virtuali disponibile elevata associate al cluster, supponendo che hanno accesso nello spazio di archiviazione.  Set di cluster aggiornerà automaticamente anche l'inventario da:

- Non è più monitoraggio dell'integrità del cluster rimosso ora e le macchine virtuali in esecuzione su di esso
- Rimuove da spazio dei nomi set di cluster e tutti i riferimenti alle condivisioni ospitate nel cluster di rimosso ora

Ad esempio, il comando per rimuovere il cluster CLUSTER1 dal set di cluster sarebbe:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## Domande frequenti

**Domanda:** Il set di cluster, limitato seleziona l'opzione solo tramite cluster iper-convergenti? <br>
**Risposte:** No.  È possibile combinare spazi di archiviazione diretta con i cluster tradizionali.

**Domanda:** È possibile gestire il Set di Cluster tramite System Center Virtual Machine Manager? <br>
**Risposte:** System Center Virtual Machine Manager non supporta attualmente il set di Cluster <br><br> **Domanda:** Possono Windows Server 2012 R2 o 2016 cluster coesistere nello stesso set di cluster? <br>
**Domanda:** È possibile migrare i carichi di lavoro disattivato Windows Server 2012 R2 oppure 2016 cluster richiedendo semplicemente a tali cluster aggiungere stesso Set di Cluster? <br>
**Risposte:** Set di cluster è una nuova tecnologia introdotta in Windows Server Preview build, pertanto, di conseguenza, non esiste nelle versioni precedenti. Cluster di livello inferiore in base del sistema operativo non può essere aggiunto un set di cluster. Tuttavia, la tecnologia gli aggiornamenti in sequenza del sistema operativo del Cluster deve fornire le funzionalità di migrazione che stai cercando eseguendo l'aggiornamento di questi cluster a Windows Server 2019.

**Domanda:** Può set di Cluster consente la scala di archiviazione o di calcolo (da solo)? <br>
**Risposte:** Sì, aggiungendo semplicemente un spazio di archiviazione diretta o un cluster Hyper-V tradizionali. Con il set di cluster, è una semplice modifica di rapporto Compute-per-archiviazione anche in un set di cluster iper-convergenti.

**Domanda:** Che cos'è gli strumenti di gestione per i set di cluster <br>
**Risposte:** PowerShell o WMI in questa versione.

**Domanda:** La migrazione in tempo reale cross-cluster funzionamento con processori di appartengono a generazioni diverse?  <br>
**Risposte:** Set di cluster non risolvere le differenze del processore e la priorità sulle chiavi cosa attualmente supporta Hyper-V.  Di conseguenza, la modalità di compatibilità processore deve essere utilizzata con le migrazioni rapide.  Il suggerimento per i set di Cluster consiste nell'usare lo stesso hardware del processore all'interno di ogni Cluster, nonché l'intero Set di Cluster per live migrazioni tra i cluster di essere eseguita.

**Domanda:** Può il set di cluster virtuale machines automaticamente un errore di cluster di failover?  <br>
**Risposte:** In questa versione, le macchine virtuali di set di cluster può essere manualmente live-la migrazione tra i cluster; ma non può automaticamente il failover. 

**Domanda:** Come abbiamo garantire è resiliente errori di cluster di archiviazione? <br>
**Risposte:** Usa soluzione di Replica archiviazione (SR) cross-cluster a cluster di membro per realizzare la resilienza di archiviazione per gli errori del cluster.

**Domanda:** Usare Replica archiviazione (SR) per la replica tra cluster membro. Set dello spazio dei nomi archiviazione modifica percorsi UNC del cluster in failover SR al cluster di spazi di archiviazione diretta di destinazione di replica? <br>
**Risposte:** In questa versione, tale cluster set dello spazio dei nomi dei riferimenti modifica non si verifica con il failover SR. Tieni informare Microsoft se questo scenario è fondamentale e come prevedi di usarlo.

**Domanda:** È possibile alle macchine virtuali di failover tra domini di errore in una situazione di ripristino di emergenza (ad esempio il dominio di errore intero si è arrestato)? <br>
**Risposte:** No, tieni presente che cross-cluster di failover all'interno di un errore logico dominio non è ancora supportato. 

**Domanda:** Il mio cluster impostare span cluster in più siti (o domini DNS)? <br> 
**Risposte:** Si tratta di uno scenario non testato e non immediatamente pianificati per il supporto di produzione. Tieni informare Microsoft se questo scenario è fondamentale e come prevedi di usarlo.

**Domanda:** Cluster di imposta interagiscono con IPv6? <br>
**Risposte:** IPv4 e IPv6 sono supportati con il set di cluster in cluster di Failover.

**Domanda:** Quali sono i requisiti di foresta di Active Directory per cluster imposta <br>
**Risposte:** Tutti i cluster di membro devono essere nella stessa foresta di Active Directory.

**Domanda:** Il numero di cluster o nodi può essere parte di un singolo cluster impostato? <br>
**Risposte:** Nell'anteprima, i set di cluster state testate e supportate fino a 64 nodi del cluster totale. Tuttavia, cluster imposta scale architettura molto più grandi limiti e non è un elemento che è hardcoded per un limite. Tieni informare Microsoft se vasta scala è fondamentale e come prevedi di usarlo.

**Domanda:** Tutti i cluster di spazi di archiviazione diretta in un set di cluster andrà a formare un pool di archiviazione singola? <br>
**Risposte:** No. Tecnologia di spazi di archiviazione funziona comunque all'interno di un singolo cluster e non tra i cluster membro in un set di cluster.

**Domanda:** Il cluster è impostato dello spazio dei nomi disponibilità elevata? <br>
**Risposte:** Sì, lo spazio dei nomi set di cluster è disponibile tramite un server dello spazio dei nomi di continuamente disponibili (CA) dei riferimenti SOFS in esecuzione nel cluster di gestione. Si consiglia di avere un numero sufficiente di macchine virtuali dal cluster di membro per renderlo resiliente localizzati errori a livello di cluster di. Tuttavia, per tenere conto di imprevisti errori irreversibili, ad esempio, tutte le macchine virtuali nel cluster gestione passando verso il basso nello stesso momento-le informazioni di riferimento sono inoltre in modo permanente memorizzate nella cache in ogni nodo del cluster set, anche su riavvii.
 
**Domanda:** Cluster imposta l'accesso di archiviazione basata su spazio dei nomi rallentamento le prestazioni di archiviazione in un set di cluster? <br>
**Risposte:** No. Spazio dei nomi set di cluster offre uno spazio dei nomi dei riferimenti di sovrapposizione all'interno di un set di cluster – concettualmente simile Distributed File System spazi dei nomi (DFSN). E, a differenza di DFSN, tutti i metadati di segnalazione cluster set dello spazio dei nomi vengono popolato automaticamente e aggiornato automaticamente su tutti i nodi senza intervento dell'amministratore, dunque non quasi Nessuna riduzione delle prestazioni nel percorso di accesso di archiviazione. 

**Domanda:** Come posso il backup dei metadati di set di cluster? <br>
**Risposte:** Questa guida è uguale a quello di Cluster di Failover. Il Backup dello stato del sistema verrà backup anche lo stato del cluster.  Tramite Windows Server Backup, è possibile eseguire il ripristino delle solo i database del cluster un nodo (che non dovrebbe essere necessario mai a causa di una serie di logica automatica abbiamo agli) o eseguire un ripristino autorevole per eseguire il rollback di database del cluster intero in tutti i nodi. Nel caso di set di cluster, si consiglia di eseguire prima di tutto tali autorevole nel cluster di membro e quindi il cluster di gestione se necessario. 
