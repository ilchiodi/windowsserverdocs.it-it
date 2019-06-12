---
title: Set di cluster
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: Questo articolo viene descritto lo scenario di set di Cluster
ms.localizationpriority: medium
ms.openlocfilehash: 349b69835ae68c626e886cad30f4d5a89d358372
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719711"
---
# <a name="cluster-sets"></a>Set di cluster

> Si applica a: Windows Server 2019

Set di cluster è la nuova tecnologia di tipo scale-out cloud nella versione di Windows Server 2019 che aumenta il numero di nodi del cluster in un unico cloud Software definito Data Center (SDDC) di ordini di grandezza. Un set di cluster è un raggruppamento a regime di controllo di più cluster di Failover: calcolo, archiviazione convergente o iperconvergente. Cluster imposta tecnologia Abilita fluidità di macchina virtuale tra cluster di membro all'interno di un set di cluster e uno spazio dei nomi archiviazione unificata nel set per il supporto della fluidità di macchina virtuale.

Anche se si verifica nel mantenimento gestione Cluster di Failover esistenti nei cluster di membro, un'istanza del set di cluster sono inoltre disponibili principali casi d'uso per la gestione del ciclo di vita nell'aggregazione. Questa Guida alla valutazione di Windows Server 2019 Scenario fornisce le informazioni necessarie in background insieme alle istruzioni dettagliate per valutare la tecnologia di set di cluster tramite PowerShell.

## <a name="technology-introduction"></a>Introduzione di tecnologie

Tecnologia di set di cluster è stata sviluppata per soddisfare le richieste dei clienti specifici gestione di Software definito Datacenter (SDDC) cloud su larga scala. Proposta di valore di set di cluster può essere riepilogato come indicato di seguito:  

- Aumentare notevolmente la scalabilità cloud SDDC supportata per l'esecuzione di macchine virtuali a disponibilità elevata tramite la combinazione di più cluster più piccoli in un'unica infrastruttura di grandi dimensioni, pur mantenendo il limite di errore software in un singolo cluster
- Gestire l'intero ciclo di vita del Cluster di Failover tra cui onboarding e la rimozione dei cluster, senza influire sulla disponibilità delle macchine virtuali tenant, tramite agevolmente la migrazione delle macchine virtuali tra questa infrastruttura di grandi dimensioni
- Modificare con facilità il rapporto di calcolo-a-storage nell'iperconvergente ho
- Beneficiare [domini di errore simile a Azure e set di disponibilità](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) tra cluster nella selezione host per macchina virtuale iniziale e migrazione delle macchine virtuali successive
- Generazioni diverse e combinare di CPU hardware nello stesso cluster impostare fabric, persino, mantenendo i domini di errore singolo omogenei per garantire la massima efficienza.  Si noti che l'indicazione della stesso hardware sia ancora presente all'interno di ogni cluster, nonché il set intero cluster.

Da una vista di alto livello, si tratta quali cluster sono simili a set.

![Cluster imposta soluzione visualizzazione](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

Di seguito viene fornita un breve riepilogo della ognuno degli elementi nell'immagine precedente:

**Cluster di gestione**

Cluster di gestione in un set di cluster è un Cluster di Failover che ospita il piano di gestione a disponibilità elevata del set intero cluster e il riferimento dello spazio dei nomi (Cluster Set Namespace) di archiviazione unificata del File Server di scalabilità orizzontale (SOFS). Un cluster di gestione è logicamente separato dal cluster di membri che eseguono i carichi di lavoro di macchina virtuale. In questo modo il cluster impostato resiliente piano di gestione su eventuali errori a livello di cluster localizzati, ad esempio, interruzione dell'alimentazione di un cluster di membro.   

**Cluster di membro**

Un cluster di membro in un set di cluster è in genere un cluster iperconvergente tradizionali che eseguono macchine virtuali e dei carichi di lavoro di spazi di archiviazione diretta. Partecipare a più cluster di membro in una distribuzione di set singolo cluster, che costituiscono l'infrastruttura cloud SDDC più grande. I cluster di membro sono diversi da un cluster di gestione in due aspetti principali: i cluster membro fanno parte di dominio di errore e costrutti di set di disponibilità e cluster membro vengono ridimensionate anche alla macchina virtuale host e i carichi di lavoro di spazi di archiviazione diretta. Macchine virtuali del set di cluster che si spostano attraverso i limiti di cluster in un set di cluster non deve essere ospitate nel cluster di gestione per questo motivo.

**Cluster impostato lo spazio dei nomi riferimento SOFS**

Un riferimento di spazio dei nomi di set di cluster (Cluster Set Namespace) SOFS è un File Server di scalabilità orizzontale in cui ogni condivisione SMB sul SOFS Namespace impostare Cluster è una condivisione di riferimento: di tipo 'SimpleReferral' appena introdotta in Windows Server 2019. Questo rinvio consente l'accesso alla destinazione di condivisione SMB ospitata nel cluster SOFS membro i client Server Message Block (SMB). Il cluster imposta lo spazio dei nomi riferimento SOFS è un meccanismo di riferimento leggeri e di conseguenza, non fa parte del percorso dei / o. Vengono memorizzati nella cache perennemente i riferimenti di SMB in ognuno dei nodi client e lo spazio dei nomi di set di cluster in modo dinamico viene aggiornato automaticamente questi riferimenti in base alle esigenze.

**Cluster di imposta il nodo master**

In un set di cluster, la comunicazione tra i cluster di membro è regime di controllo ed è coordinata da una nuova risorsa di cluster denominata "Cluster Set Master" (CS-Master). Come qualsiasi altra risorsa cluster, CS-Master è estremamente disponibile e resiliente agli errori di singoli membri del cluster e/o gli errori dei nodi del cluster di gestione. Tramite un nuovo provider WMI di Set di Cluster, CS-Master fornisce l'endpoint di gestione per tutte le interazioni gestibilità Cluster impostato.

**Cluster worker di set**

In una distribuzione Cluster impostato, CS-Master interagisce con una nuova risorsa di cluster nel membro del cluster denominato "Cluster impostare ruolo di lavoro" (CS-ruolo di lavoro). CS / ruolo di lavoro agisce come l'anello di congiunzione solo sul cluster per orchestrare le interazioni cluster locale, come richiesto da CS-Master. Sono esempi di tali interazioni posizionamento delle macchine virtuali e l'inventario delle risorse cluster locale. È presente una sola istanza di CS-ruolo di lavoro per ogni membro del cluster in un set di cluster. 

**dominio di errore**

Un dominio di errore è il raggruppamento di software e gli elementi hardware che determina l'amministratore potrebbe non riuscire insieme quando si verifica un errore.  Mentre un amministratore è stato possibile designare uno o più cluster insieme come un dominio di errore, ogni nodo può partecipare a un dominio di errore in un set di disponibilità. Cluster imposta da Progettazione lascia la decisione di determinazione di limite dominio di errore all'amministratore che è buona familiarità con considerazioni sulla topologia centro dati – ad esempio, PDU, rete, che condividono i cluster di membro. 

**Set di disponibilità**

Un set di disponibilità consente all'amministratore di configurare la ridondanza desiderata dei carichi di lavoro in cluster nei domini di errore, organizzando tali file in un set di disponibilità e la distribuzione di carichi di lavoro in tale set di disponibilità. Si supponga che se si sta distribuendo un'applicazione a due livelli, è consigliabile configurare almeno due macchine virtuali in un set di disponibilità per ogni livello che garantisce che quando un dominio di errore in tale set di disponibilità diventa inattiva, l'applicazione avrà almeno una macchina virtuale in ogni livello ospitato in un dominio di errore differenti di questo stesso set di disponibilità.

## <a name="why-use-cluster-sets"></a>Perché usare i set di cluster

Set di cluster offre i vantaggi di scalabilità senza sacrificare la resilienza.  

Set di cluster consente di clustering più cluster insieme per creare un'infrastruttura di grandi dimensioni, mentre ogni cluster rimane indipendente per garantire la resilienza.  Ad esempio, si dispone di un uomo di 4 nodi diversi cluster di macchine virtuali in esecuzione.  Ogni cluster offre la flessibilità necessaria per se stesso.  Se la memoria o archiviazione inizia a riempirsi, la scalabilità verticale è il passaggio successivo.  Con scalabilità verticale, esistono alcune opzioni e considerazioni.

1. Aggiungere ulteriore spazio di archiviazione per il cluster corrente.  Con spazi di archiviazione diretta, questa operazione può risultare difficile, come le stesse unità di modello/firmware esatta potrebbe non essere disponibile.  La considerazione di tempi di ricompilazione è inoltre necessario tenere conto.
2. Aggiungere ulteriore memoria.  Cosa accade se sono raggiunge il limite massimo della memoria che possono gestire i computer?  Cosa accade se tutti gli slot di memoria disponibile sono completi?
3. Aggiungere nodi di calcolo aggiuntive con le unità in cluster corrente.  Questo atteggiamento nuovamente all'opzione 1 che devono essere considerati.
4. Acquisto di un cluster completamente nuovo

Si tratta in cui il set di cluster offre i vantaggi della scalabilità.  Se si aggiungono i miei cluster in un set di cluster, è possibile sfruttare i vantaggi dell'archiviazione o memoria, che può essere disponibile in un altro cluster senza l'acquisto di altri componenti.  Dal punto di vista la resilienza, aggiunta di nodi aggiuntivi per un spazi di archiviazione diretta non verrà fornita aggiuntivi voti di quorum.  Come accennato [qui](drive-symmetry-considerations.md), un Cluster di spazi di archiviazione diretta può resistere alla perdita di 2 nodi prima di procedere verso il basso.  Se si dispone di un cluster di 4 nodi uomo, 3 nodi si disattivano richiederà l'intero cluster verso il basso.  Se si dispone di un cluster a 8 nodi, 3 nodi si disattivano richiederà l'intero cluster verso il basso.  Con set di Cluster con due cluster uomo di 4 nodi del set e 2 nodi in un uomo diventano inattivi e diventi non disponibile 1 nodo nell'altro uomo, entrambi i cluster di rimangano attive.  È preferibile per creare un cluster di spazi di archiviazione diretta 16 nodi grandi dimensioni o suddividerla in quattro cluster di 4 nodi e usare i set di cluster?  Con quattro cluster di 4 nodi con cluster set fornisce la stessa scala, ma una migliore resilienza in quanto più nodi di calcolo possono scendere (in modo imprevisto o per la manutenzione) e rimane di ambiente di produzione.

## <a name="considerations-for-deploying-cluster-sets"></a>Considerazioni per la distribuzione di set di cluster

Quando si valuta se il set di cluster è un elemento è necessario usare, considerare queste domande:

- È necessario andare oltre i corrente uomo calcolo e archiviazione di limiti di scalabilità?
- Tutti di calcolo e archiviazione non sono esattamente uguali?
- È in tempo reale la migrazione delle macchine virtuali tra i cluster?
- Si vuole il set di disponibilità di Azure simile a computer e domini di errore in più cluster?
- È necessario il tempo necessario per esaminare tutti i cluster per determinare dove devono essere inseriti eventuali nuove macchine virtuali?

Se la risposta è affermativa, set di cluster è ciò che è necessario.

Esistono alcuni altri elementi da considerare in cui una più grande SDDC potrebbe cambiare le strategie di centro dati complessivi.  SQL Server è un buon esempio.  Lo spostamento delle macchine virtuali di SQL Server tra i cluster richiede licenze di SQL per l'esecuzione nei nodi aggiuntivi?  

## <a name="scale-out-file-server-and-cluster-sets"></a>File server di scalabilità orizzontale e i set di cluster

In Windows Server 2019, vi è un nuovo ruolo di server di scalabilità orizzontale file chiamato infrastruttura Scale-Out File Server di scalabilità orizzontale (SOFS). 

Le considerazioni seguenti si applicano a un ruolo di infrastruttura SOFS:

1.  In un Cluster di Failover possono essere presenti al massimo un solo ruolo del cluster SOFS dell'infrastruttura. Ruolo SOFS infrastruttura viene creato specificando la " **-infrastruttura**" passare parametri per il **Add-ClusterScaleOutFileServerRole** cmdlet.  Ad esempio:

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Ogni volume CSV creato automaticamente il failover determina la creazione di una condivisione SMB con un nome generato automaticamente in base al nome del volume CSV. Un amministratore non può direttamente creare o modificare le condivisioni SMB in un ruolo SOFS, diverso da tramite operazioni di creazione/modifica volume CSV.

3.  Nelle configurazioni iperconvergenti, un SOFS infrastruttura consente a un client SMB (host Hyper-V) per comunicare con garantite disponibilità continua (CA) per il server di infrastruttura SOFS SMB. Questo codice iperconvergente SMB loopback autorità di certificazione viene realizzata attraverso le macchine virtuali l'accesso ai relativi file di disco virtuale (VHDx) in cui l'identità della macchina virtuale proprietaria viene inoltrato tra il client e server. Questa funzionalità di inoltro di identità consente i file VHDx di ACL ing come in configurazioni cluster iperconvergente standard come prima.

Dopo aver creato un set di cluster, lo spazio dei nomi di set di cluster si basa su un SOFS infrastruttura in ognuno dei cluster membro e inoltre un SOFS infrastruttura nel cluster di gestione.

Al momento un cluster del membro viene aggiunto a un set di cluster, se ne esiste già, l'amministratore specifica il nome di un SOFS infrastruttura in tale cluster. Se il SOFS infrastruttura non esiste, viene creato un nuovo ruolo di infrastruttura SOFS nel nuovo cluster membro da questa operazione. Se esiste già un ruolo SOFS infrastruttura nel cluster di membro, l'operazione di aggiunta in modo implicito viene rinominato con il nome specificato in base alle esigenze. Qualsiasi server SMB singleton esistenti, o i ruoli non - infrastruttura SOFS nel membro del cluster vengono lasciati inutilizzate per il set di cluster. 

In fase di creazione del set di cluster, l'amministratore ha la possibilità di usare un oggetto computer di Active Directory già esistente come radice dello spazio dei nomi nel cluster di gestione. Operazioni di creazione di set di cluster create il ruolo del cluster SOFS infrastruttura nel cluster di gestione o Rinomina il ruolo infrastruttura SOFS esistente semplicemente come descritto in precedenza per i cluster di membro. SOFS infrastruttura nel cluster di gestione viene utilizzato come cluster di set di riferimento dello spazio dei nomi (Cluster Set Namespace) SOFS. Significa semplicemente che ogni condivisione SMB nel cluster di impostare lo spazio dei nomi che SOFS è una condivisione di riferimento, di tipo 'SimpleReferral' - appena introdotta in Windows Server 2019.  Questo rinvio consente l'accesso ai client SMB per la destinazione di condivisione SMB ospitata nel cluster SOFS membro. Il cluster imposta lo spazio dei nomi riferimento SOFS è un meccanismo di riferimento leggeri e di conseguenza, non fa parte del percorso dei / o. Vengono memorizzati nella cache perennemente i riferimenti di SMB in ognuno dei nodi client e lo spazio dei nomi di set di cluster in modo dinamico viene aggiornato automaticamente questi riferimenti in base alle esigenze

## <a name="creating-a-cluster-set"></a>Creazione di un Set di Cluster

### <a name="prerequisites"></a>Prerequisiti

Durante la creazione di un cluster di set, è consigliabile si dei prerequisiti seguenti:

1. Configurare un client di gestione che esegue Windows Server 2019.
2. Installare gli strumenti di Cluster di Failover sul server di gestione.
3. Creare i membri del cluster (almeno due cluster con almeno due volumi condivisi del Cluster in ogni cluster)
4. Creare un cluster di gestione (fisico o guest) che attraversa i cluster di membro.  Questo approccio assicura che i set di Cluster piano di gestione continua a essere disponibile indipendentemente dagli errori di cluster possibili membro.

### <a name="steps"></a>Passaggi

1. Creare un nuovo cluster impostate da tre cluster come definito nei prerequisiti.  Il seguente grafico verrà fornito un esempio di cluster da creare.  Il nome del cluster impostato in questo esempio sarà **CSMASTER**.

   | Nome del cluster               | Nome SOFS infrastruttura da utilizzare in un secondo momento | 
   |----------------------------|-------------------------------------------|
   | SET-CLUSTER                | SOFS-CLUSTERSET                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. Dopo avere creato tutti i cluster, usare i comandi seguenti per creare il cluster set master.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Per aggiungere un Cluster di Server al gruppo di cluster, di seguito viene usato.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Se si usa una combinazione di indirizzo IP statica, è necessario includere *x.x.x. x - StaticAddress* nel **New-ClusterSet** comando.

4. Dopo aver creato il cluster all'esterno dei membri del cluster, è possibile elencare il set di nodi e le relative proprietà.  Per enumerare tutti i cluster di membro nel set di cluster:

        Get-ClusterSetMember -CimSession CSMASTER

5. Per enumerare tutti i cluster del membro del cluster impostare inclusi nodi del cluster di gestione:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Per elencare tutti i nodi dal cluster membro:

        Get-ClusterSetNode -CimSession CSMASTER

7. Per elencare tutti i gruppi di risorse nel set di cluster:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Per verificare il cluster impostare il processo di creazione creato una condivisione SMB (identificata come Volume1 o qualsiasi cartella CSV viene etichettata con la ScopeName è il nome del File Server dell'infrastruttura e il percorso sia) il server di scalabilità orizzontale dell'infrastruttura per ogni membro del cluster Volume condiviso cluster:

        Get-SmbShare -CimSession CSMASTER

8. Set di cluster include registri di debug che possono essere raccolti per la revisione.  Imposta il cluster e per tutti i membri e il cluster di gestione è possono raccogliere i log di debug di cluster.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configurare Kerberos [per la delega vincolata](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/) tra tutti i cluster di impostazione dei membri.

10. Configurare il tipo di autenticazione di migrazione in tempo reale di macchina virtuale tra cluster di Kerberos in ogni nodo in un Set di Cluster.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Aggiungere il cluster di gestione al gruppo administrators locale in ogni nodo nel set di cluster.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>Creazione di nuove macchine virtuali e aggiunta al set di cluster

Dopo la creazione del cluster è stato impostato, il passaggio successivo consiste nel creare nuove macchine virtuali.  In genere, quando è possibile creare macchine virtuali e aggiungerli a un cluster, è necessario eseguire alcuni controlli nei cluster per vedere quale potrebbe essere preferibile eseguire nell'ambito.  Questi controlli possono includere:

- Quantità di memoria è disponibile nei nodi del cluster?
- Spazio su disco è disponibile nei nodi del cluster?
- La macchina virtuale richiede requisiti di archiviazione specifico (ovvero voglio macchine virtuali SQL Server per passare a un cluster che eseguono le unità più veloce; in alternativa, la macchina virtuale dell'infrastruttura non è critica ed eseguibili in unità più lente).

Dopo questo domande, si crea la macchina virtuale nel cluster che è necessario che sia.  Uno dei vantaggi dei set di cluster è che i set di cluster eseguire tali controlli per l'utente e inserire la macchina virtuale nel nodo ottimale.

I comandi seguenti verranno entrambi identificare il cluster ottima e distribuire la macchina virtuale ad esso.  Nell'esempio seguente viene creata una nuova macchina virtuale specifica che almeno 4 GB di memoria è disponibile per la macchina virtuale e che dovrà utilizzare 1 processore virtuale.

- Assicurarsi che sia disponibile per la macchina virtuale di 4gb
- impostare il processore virtuale utilizzato in 1
- controllo per assicurarsi che ci sia almeno il 10% della CPU disponibile per la macchina virtuale

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

Al termine, sarà possibile le informazioni relative alla macchina virtuale e in cui è stato inserito.  Nell'esempio precedente, questo verrà visualizzato come:

        State         : Running
        ComputerName  : 1-S2D2

Se non dispone di sufficiente memoria, cpu o spazio su disco per aggiungere la macchina virtuale, si riceverà l'errore:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Dopo aver creata la macchina virtuale, verrà visualizzato nella console di gestione Hyper-V sul nodo specifico specificato.  Per aggiungerla come un cluster di set di macchine virtuali e al cluster, il comando sarà il seguente.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Al termine, l'output sarà:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Se è stato aggiunto un cluster con le macchine virtuali esistenti, le macchine virtuali anche dovrà essere registrato con il Cluster set quindi, registrare tutte le macchine virtuali in una sola volta, il comando da usare è:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

Tuttavia, il processo non è completo, come il percorso per la macchina virtuale deve essere aggiunto allo spazio dei nomi di set di cluster.

Ad esempio, viene aggiunto un cluster esistente e dispone di macchine virtuali preconfigurate si trovano nel locale Cluster Shared Volume (CSV), il percorso per il file VHDX sarà qualcosa di simile a "C:\ClusterStorage\Volume1\MYVM\Virtual Disks\MYVM.vhdx disco rigido.  Migrazione dell'archiviazione dovrà essere eseguita come previsto da Progettazione locale a un cluster singolo membro sono i percorsi CSV. Di conseguenza, non sarà accessibile alla macchina virtuale quando sono in tempo reale la migrazione tra cluster di membro. 

In questo esempio CLUSTER3 è stato aggiunto al cluster impostato utilizzando Add-ClusterSetMember con Scale-Out File Server dell'infrastruttura come SOFS CLUSTER3.  Per spostare la configurazione della macchina virtuale e l'archiviazione, il comando è:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Al termine, si riceverà un avviso:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Questo avviso può essere ignorato l'avviso è "senza modificare la configurazione archiviazione del ruolo macchina virtuale sono state rilevate".  Il motivo per l'avviso come percorso fisico effettivo non è modificabile; solo i percorsi di configurazione. 

Per altre informazioni su Move-VMStorage, consultare questa [collegamento](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

In tempo reale la migrazione di una macchina virtuale tra cluster diversi set di cluster è non uguale come in passato. In scenari set non cluster, la procedura sarebbe:

1. rimuovere il ruolo di macchina virtuale dal Cluster.
2. migrazione in tempo reale della macchina virtuale a un nodo membro di un cluster diverso.
3. aggiungere la macchina virtuale al cluster come un nuovo ruolo di macchina virtuale.

Con i set di Cluster non sono necessari questi passaggi e solo un comando è necessaria.  In primo luogo, è necessario impostare tutte le reti siano disponibili per la migrazione con il comando:

    Set-VMHost -UseAnyNetworkMigration $true

Ad esempio, desidera spostare una macchina virtuale di Cluster impostato dal CLUSTER1 in NODE2 CL3 su CLUSTER3.  Il singolo comando sarà:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Si noti che ciò non modifica i file di archiviazione o la configurazione di macchina virtuale.  Non è necessario il percorso per la macchina virtuale rimane come \\SOFS CLUSTER1\VOLUME1.  Dopo la registrazione di una macchina virtuale con i set di cluster ha il percorso della condivisione File Server di infrastruttura, l'unità e macchina virtuale non è necessario in corso nello stesso computer della macchina virtuale.

## <a name="creating-availability-sets-fault-domains"></a>Disponibilità di creazione di set di domini di errore

Come descritto nell'introduzione, domini di errore simile a Azure e i set di disponibilità possono essere configurati in un set di cluster.  Ciò è utile per le migrazioni tra cluster e selezioni host per macchina virtuale iniziale.  

Nell'esempio seguente, sono presenti quattro cluster che fanno parte del set di cluster.  All'interno del set, verrà creato un dominio di errore logico con due i cluster e un dominio di errore creati con altri due cluster.  Questi due domini di errore includerà il Set di disponibilità. 

Nell'esempio seguente, CLUSTER1 e CLUSTER2 saranno in un dominio di errore chiamato **FD1** mentre CLUSTER3 e CLUSTER4 saranno in un dominio di errore chiamato **FD2**.  Il set di disponibilità verrà chiamato **CSMASTER-AS** ed essere costituito di due domini di errore.

Per creare i domini di errore, i comandi sono:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Per garantire che sono state create correttamente, è possibile eseguire Get-ClusterSetFaultDomain con il proprio output illustrato.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Ora che sono stati creati i domini di errore, il set di disponibilità deve essere creata.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Per la convalida è stata creata, quindi usare:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Quando si creano nuove macchine virtuali, è necessario quindi usare il parametro - set di disponibilità come parte di determinare il nodo ottima.  Pertanto hanno quindi un aspetto simile al seguente:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Rimozione di un cluster dal cluster imposta a causa di diversi cicli di vita. Vi sono casi quando un cluster deve essere rimosso da un set di cluster. Come procedura consigliata, tutte le macchine virtuali del set di cluster deve essere spostate all'esterno del cluster. Ciò è possibile usare la **Move-ClusterSetVM** e **Move-VMStorage** comandi.

Tuttavia, se non verranno spostate anche le macchine virtuali, set di cluster esegue una serie di azioni per fornire un risultato intuitivo per l'amministratore.  Quando il cluster viene rimosso dal set, tutti i restanti set di macchine virtuali del cluster ospitate nel cluster da rimuovere semplicemente diventerà macchine virtuali a disponibilità elevata associate a tale cluster, purché che abbiano accesso al relativo spazio di archiviazione.  Set di cluster aggiornerà automaticamente anche l'inventario da:

- Non è più rilevamento dell'integrità del cluster rimosso ora e le macchine virtuali in esecuzione su di essa
- Rimuove dal cluster set dello spazio dei nomi e tutti i riferimenti a condivisioni ospitate nel cluster rimosso adesso

Ad esempio, il comando per rimuovere il cluster CLUSTER1 dai set di cluster sarà:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>Domande frequenti

**Domanda:** Nel set di cluster, è previsto un limite all'uso dei soli cluster iperconvergente? <br>
**Risposta:** No.  È possibile combinare gli spazi di archiviazione diretta con i cluster tradizionali.

**Domanda:** È possibile gestire il Cluster impostato tramite System Center Virtual Machine Manager? <br>
**Risposta:** System Center Virtual Machine Manager non supporta attualmente i set di Cluster <br><br> **Domanda:** Possono Windows Server 2012 R2 o 2016 cluster coesistere nello stesso set di cluster? <br>
**Domanda:** È possibile migrare i carichi di lavoro disattivare Windows Server 2012 R2 o 2016 cluster per la disponibilità di tali cluster partecipare allo stesso Set di Cluster? <br>
**Risposta:** Set di cluster è una nuova tecnologia introdotta in Windows Server 2019, pertanto, di conseguenza, non esiste nelle versioni precedenti. I cluster basati su sistemi operativi legacy non è possibile aggiungere un set di cluster. Tuttavia, la tecnologia degli aggiornamenti in sequenza del sistema operativo del Cluster deve fornire la funzionalità di migrazione che si sta cercando eseguendo l'aggiornamento di questi cluster a Windows Server 2019.

**Domanda:** Possibile set di Cluster consente la scalabilità di archiviazione o di calcolo (da sole)? <br>
**Risposta:** Sì, aggiungendo semplicemente un spazio di archiviazione diretta o cluster Hyper-V tradizionale. Con i set di cluster, è una semplice modifica del rapporto di calcolo-a-Storage anche in un set di cluster iperconvergente.

**Domanda:** What ' s gli strumenti di gestione per i set di cluster <br>
**Risposta:** PowerShell o WMI in questa versione.

**Domanda:** Come si integrano la migrazione in tempo reale tra cluster con processori di generazioni diversi?  <br>
**Risposta:** Set di cluster non ovviare alle differenze di processore e sostituiscono quelle attualmente supportate la tecnologia Hyper-V.  Pertanto, la modalità di compatibilità processore deve essere utilizzata con migrazioni rapide.  La raccomandazione per i set di Cluster consiste nell'usare lo stesso hardware del processore all'interno di ogni Cluster, nonché l'intero Cluster impostato per migrazioni in tempo reale tra cluster di cui si verifichi.

**Domanda:** È il set di cluster virtuale macchine automaticamente il failover su un errore del cluster?  <br>
**Risposta:** In questa versione, le macchine virtuali di set di cluster può essere solo manualmente live-eseguire la migrazione tra cluster; ma non automaticamente il failover. 

**Domanda:** Come è possibile garantire l'archiviazione è resiliente agli errori del cluster? <br>
**Risposta:** Usare soluzioni di Replica archiviazione (SR) tra cluster tra cluster di membro per realizzare la resilienza di archiviazione per gli errori del cluster.

**Domanda:** Usare Replica archiviazione (SR) per la replica tra i cluster di membro. Cluster archiviazione dello spazio dei nomi dei set di modifiche di percorsi UNC in SR failover nel cluster di spazi di archiviazione diretta di destinazione di replica? <br>
**Risposta:** In questa versione, tale cluster set dello spazio dei nomi riferimento modifica non viene eseguito con il failover di SR. Comunicarlo a Microsoft se questo scenario è essenziale è e come si prevede di usarlo.

**Domanda:** È possibile failover delle macchine virtuali nei domini di errore in una situazione di ripristino di emergenza (ad esempio il dominio di errore intera si è arrestato)? <br>
**Risposta:** No, si noti che il failover tra cluster all'interno di un errore logico dominio non è ancora supportato. 

**Domanda:** Il cluster impostabile span cluster in più siti (o i domini DNS)? <br> 
**Risposta:** Si tratta di uno scenario non testato e non immediatamente pianificato per il supporto di produzione. Comunicarlo a Microsoft se questo scenario è essenziale è e come si prevede di usarlo.

**Domanda:** Funziona con IPv6 è set da cluster? <br>
**Risposta:** IPv4 e IPv6 sono supportati con i set di cluster come cluster di Failover.

**Domanda:** Quali sono i requisiti di foresta di Active Directory per il cluster imposta <br>
**Risposta:** Tutti i cluster di membro devono essere nella stessa foresta Active Directory.

**Domanda:** Il numero di nodi o cluster può essere parte di un singolo cluster impostata? <br>
**Risposta:** In Windows Server 2019, cluster di set di stati testati e supportati al massimo 64 nodi del cluster complessivo. Tuttavia, cluster imposta architettura scale molto ampi e non è un elemento che è hardcoded per un limite. Comunicarlo a Microsoft se scala più ampia è essenziale è e come si prevede di usarlo.

**Domanda:** Tutti i cluster di spazi di archiviazione diretta in un set di cluster costituiranno un unico pool di archiviazione? <br>
**Risposta:** No. La tecnologia Direct degli spazi di archiviazione funziona comunque all'interno di un singolo cluster e non in cluster di membro in un set di cluster.

**Domanda:** Il cluster viene impostato lo spazio dei nomi a disponibilità elevata? <br>
**Risposta:** Sì, lo spazio dei nomi di set di cluster viene fornito tramite un server di spazio dei nomi SOFS riferimento continuamente disponibili (CA) in esecuzione nel cluster di gestione. Microsoft consiglia di includere un numero sufficiente di macchine virtuali dal cluster di membro per renderla resilienti agli errori a livello di cluster localizzati. Tuttavia, per conto di errori irreversibili imprevisti – ad esempio, tutte le macchine virtuali nel cluster di gestione verso la parte inferiore allo stesso tempo, le informazioni di riferimento viene inoltre in modo permanente memorizzato nella cache in ogni nodo del set di cluster, anche dopo i riavvii.
 
**Domanda:** Cluster imposta accesso di archiviazione basata su spazio dei nomi rallenterà le prestazioni di archiviazione in un set di cluster? <br>
**Risposta:** No. Lo spazio dei nomi di set di cluster offre uno spazio dei nomi di sovrapposizione dei riferimenti all'interno di un set di cluster, ovvero a livello concettuale, ad esempio Distributed File System gli spazi dei nomi (DFSN). A differenza DFSN, tutti i metadati di riferimento dello spazio dei nomi di set di cluster è non popolata automaticamente e aggiornate automaticamente in tutti i nodi senza alcun intervento dell'amministratore, pertanto non è quasi alcun overhead di prestazioni nel percorso di accesso di archiviazione. 

**Domanda:** Come è possibile il backup dei metadati di set di cluster? <br>
**Risposta:** Questo materiale sussidiario è analoga a quella del Cluster di Failover. Il Backup dello stato del sistema verrà backup nonché lo stato del cluster.  Tramite Windows Server Backup, è possibile eseguire ripristini di solo i database cluster del nodo (che non deve essere mai necessario a causa di una serie di funzionalità di riparazione automatica per la logica è disponibile) o eseguire un ripristino autorevole per ripristinare il database intero cluster in tutti i nodi. Nel caso di set di cluster, si consiglia di fare innanzitutto tali un ripristino autorevole nel cluster di membro e quindi il cluster di gestione, se necessario.
