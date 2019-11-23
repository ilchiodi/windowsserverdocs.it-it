---
title: Aggiornamento in sequenza del sistema operativo del cluster
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 03/27/2018
ms.openlocfilehash: f7d20a099f287d2ee05ae6e908c173e1eb3cfc66
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361838"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster

> Si applica a: Windows Server 2019, Windows Server 2016

Aggiornamento in sequenza del sistema operativo del cluster consente a un amministratore di aggiornare il sistema operativo dei nodi del cluster senza arrestare i carichi di lavoro Hyper-V o File server di scalabilità orizzontale. Usando questa funzionalità, è possibile evitare le sanzioni per il tempo di inattività previste dai contratti di servizio.

L'aggiornamento in sequenza del sistema operativo cluster offre i vantaggi seguenti:

- I cluster di failover che eseguono i carichi di lavoro della macchina virtuale Hyper-V e del file server di scalabilità orizzontale (SOFS) possono essere aggiornati da Windows Server 2012 R2 (in esecuzione su tutti i nodi del cluster) a Windows Server 2016 (in esecuzione su tutti i nodi del cluster) senza tempi di inattività. Gli altri carichi di lavoro del cluster, ad esempio SQL Server, non saranno disponibili durante il periodo di tempo (in genere meno di cinque minuti) necessari per eseguire il failover a Windows Server 2016.  
- Non richiede hardware aggiuntivo. Sebbene sia possibile aggiungere altri nodi del cluster temporaneamente a cluster di piccole dimensioni per migliorare la disponibilità del cluster durante il processo di aggiornamento in sequenza del sistema operativo del cluster.  
- Non è necessario arrestare o riavviare il cluster.  
- Non è necessario un nuovo cluster. Il cluster esistente viene aggiornato. Vengono inoltre utilizzati gli oggetti cluster esistenti archiviati in Active Directory.  
- Il processo di aggiornamento è reversibile fino a quando il cliente non sceglie il "punto di non ritorno", quando tutti i nodi del cluster eseguono Windows Server 2016 e quando viene eseguito il cmdlet di PowerShell Update-ClusterFunctionalLevel.  
- Il cluster può supportare le operazioni di applicazione di patch e manutenzione durante l'esecuzione in modalità a sistema misto.  
- Supporta l'automazione tramite PowerShell e WMI.  
- La proprietà del cluster Public Property **ClusterFunctionalLevel** indica lo stato del cluster nei nodi del cluster Windows Server 2016. È possibile eseguire query su questa proprietà usando il cmdlet di PowerShell da un nodo del cluster Windows Server 2016 che appartiene a un cluster di failover:  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    Il valore **8** indica che il cluster è in esecuzione al livello di funzionalità di Windows Server 2012 R2. Il valore **9** indica che il cluster è in esecuzione al livello di funzionalità di Windows Server 2016.  

Questa guida descrive le varie fasi del processo di aggiornamento in sequenza del sistema operativo del cluster, i passaggi di installazione, le limitazioni delle funzionalità e le domande frequenti (FAQ) ed è applicabile ai seguenti scenari di aggiornamento in sequenza del sistema operativo del cluster in Windows Server 2016:  
- Cluster Hyper-V  
- Cluster File server di scalabilità orizzontale  

Lo scenario seguente non è supportato in Windows Server 2016:  
-  Aggiornamento in sequenza del sistema operativo del cluster di cluster guest tramite disco rigido virtuale (file con estensione VHDX) come spazio di archiviazione condiviso  

L'aggiornamento in sequenza del sistema operativo del cluster è completamente supportato da System Center Virtual Machine Manager (SCVMM) 2016. Se si usa SCVMM 2016, vedere [eseguire un aggiornamento in sequenza di un cluster host Hyper-V a Windows Server 2016 in VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807) per istruzioni sull'aggiornamento dei cluster e l'automazione dei passaggi descritti in questo documento.  

## <a name="requirements"></a>Requisiti  
Prima di iniziare il processo di aggiornamento in sequenza del sistema operativo del cluster, completare i requisiti seguenti:

- Inizia con un cluster di failover che esegue Windows Server (canale semestrale), Windows Server 2016 o Windows Server 2012 R2.
- L'aggiornamento di un cluster di Spazi di archiviazione diretta a Windows Server, versione 1709 non è supportato.
- Se il carico di lavoro del cluster è macchine virtuali Hyper-V o File server di scalabilità orizzontale, è possibile prevedere un aggiornamento senza tempi di inattività.
- Verificare che i nodi Hyper-V dispongano di CPU che supportano la tabella di indirizzamento di secondo livello (stecca) utilizzando uno dei metodi seguenti:  
        -Verificare [che sia compatibile con le doghe? Articolo WP8 SDK Tip 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) che descrive due metodi per verificare se una CPU supporta le stecche  
        -Scaricare lo strumento [Coreinfo v 3.31](https://technet.microsoft.com/sysinternals/cc835722) per determinare se una CPU supporta la stecca.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Stati di transizione del cluster durante l'aggiornamento in sequenza del sistema operativo cluster

In questa sezione vengono descritti i vari Stati di transizione del cluster di Windows Server 2012 R2 aggiornato a Windows Server 2016 mediante l'aggiornamento in sequenza del sistema operativo del cluster.  

Per garantire che i carichi di lavoro del cluster siano in esecuzione durante il processo di aggiornamento in sequenza del sistema operativo del cluster, lo scorrimento di un carico di lavoro del cluster da un nodo Windows Server 2012 R2 al nodo Windows Server 2016 funziona come se entrambi i nodi eseguissero il sistema operativo Windows Server 2012 R2. Quando i nodi di Windows Server 2016 vengono aggiunti al cluster, funzionano in modalità di compatibilità con Windows Server 2012 R2. Una nuova modalità concettuale del cluster, denominata "modalità mista del sistema operativo", consente l'esistenza di nodi di versioni diverse nello stesso cluster (vedere la figura 1).  

![illustrazione che mostra le tre fasi di un aggiornamento in sequenza del sistema operativo del cluster: tutti i nodi Windows Server 2012 R2, modalità sistema operativo misto e tutti i nodi Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figura 1: transizioni dello stato del sistema operativo del cluster**  

Un cluster Windows Server 2012 R2 entra in modalità sistema operativo misto quando un nodo Windows Server 2016 viene aggiunto al cluster. Il processo è completamente reversibile: i nodi di Windows Server 2016 possono essere rimossi dal cluster e i nodi di Windows Server 2012 R2 possono essere aggiunti al cluster in questa modalità. Il "punto di nessun ritorno" si verifica quando il cmdlet di PowerShell Update-ClusterFunctionalLevel viene eseguito nel cluster. Affinché il cmdlet abbia esito positivo, tutti i nodi devono essere Windows Server 2016 e tutti i nodi devono essere online.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>Stati di transizione di un cluster a quattro nodi durante l'aggiornamento del sistema operativo in sequenza

In questa sezione vengono illustrate e descritte le quattro diverse fasi di un cluster con archiviazione condivisa i cui nodi vengono aggiornati da Windows Server 2012 R2 a Windows Server 2016.  

"Stage 1" è lo stato iniziale. si inizia con un cluster di Windows Server 2012 R2.  

![illustrazione che mostra lo stato iniziale: tutti i nodi Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figura 2: stato iniziale: cluster di failover di Windows Server 2012 R2 (fase 1)**  

In "fase 2", due nodi sono stati sospesi, svuotati, rimossi, riformattati e installati con Windows Server 2016.  

![illustrazione che mostra il cluster in modalità sistema operativo misto: dal cluster a 4 nodi di esempio, due nodi eseguono Windows Server 2016 e due nodi eseguono Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figura 3: stato intermedio: modalità mista del sistema operativo: Windows Server 2012 R2 e Windows Server 2016 failover cluster (fase 2)**  

In "fase 3" tutti i nodi del cluster sono stati aggiornati a Windows Server 2016 e il cluster è pronto per essere aggiornato con il cmdlet Update-ClusterFunctionalLevel di PowerShell.  

> [!NOTE]  
> In questa fase il processo può essere completamente invertito e i nodi di Windows Server 2012 R2 possono essere aggiunti a questo cluster.  

![illustrazione che mostra che il cluster è stato completamente aggiornato a Windows Server 2016 ed è pronto per il cmdlet Update-ClusterFunctionalLevel per portare il livello di funzionalità del cluster fino a Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figura 4: stato intermedio: tutti i nodi aggiornati a Windows Server 2016, pronti per Update-ClusterFunctionalLevel (fase 3)**  

Dopo l'esecuzione di Update-ClusterFunctionalLevelcmdlet, il cluster entra in "fase 4", in cui è possibile usare nuove funzionalità del cluster di Windows Server 2016.  

![illustrazione che mostra che l'aggiornamento del sistema operativo in sequenza del cluster è stato completato correttamente. tutti i nodi sono stati aggiornati a Windows Server 2016 e il cluster è in esecuzione al livello di funzionalità del cluster Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figura 5: stato finale: cluster di failover di Windows Server 2016 (fase 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Processo di aggiornamento in sequenza del sistema operativo del cluster

Questa sezione descrive il flusso di lavoro per l'esecuzione dell'aggiornamento in sequenza del sistema operativo cluster.  

![illustrazione che illustra il flusso di lavoro per l'aggiornamento di un cluster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figura 6: flusso di lavoro del processo di aggiornamento in sequenza del sistema operativo cluster**  

L'aggiornamento in sequenza del sistema operativo del cluster include i passaggi seguenti:  

1. Preparare il cluster per l'aggiornamento del sistema operativo come indicato di seguito:  
    1. Per l'aggiornamento in sequenza del sistema operativo del cluster è necessario rimuovere un nodo alla volta dal cluster. Verificare che il cluster disponga di capacità sufficiente per mantenere i contratti di servizio a disponibilità elevata quando uno dei nodi del cluster viene rimosso dal cluster per un aggiornamento del sistema operativo. In altre parole, è necessario avere la possibilità di eseguire il failover dei carichi di lavoro in un altro nodo quando un nodo viene rimosso dal cluster durante il processo di aggiornamento in sequenza del sistema operativo del cluster? Il cluster è in grado di eseguire i carichi di lavoro necessari quando un nodo viene rimosso dal cluster per l'aggiornamento in sequenza del sistema operativo del cluster?  
    2. Per i carichi di lavoro Hyper-V, verificare che tutti gli host Hyper-V di Windows Server 2016 siano in grado di supportare la CPU per la tabella degli indirizzi di secondo livello. Solo i computer che supportano le doghe possono usare il ruolo Hyper-V in Windows Server 2016.  
    3. Verificare che tutti i backup del carico di lavoro siano stati completati e prendere in considerazione il backup del cluster. Arrestare le operazioni di backup durante l'aggiunta di nodi al cluster.  
    4. Verificare che tutti i nodi del cluster siano online/running/up usando il cmdlet [`Get-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) (vedere la figura 7).  

        ![screencap che mostra i risultati dell'esecuzione del cmdlet Get-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figura 7: determinazione dello stato del nodo tramite il cmdlet Get-ClusterNode**  

    5. Se si eseguono aggiornamenti compatibile con cluster, verificare se aggiornamento compatibile con cluster è attualmente in esecuzione usando l'interfaccia utente di **aggiornamento compatibile con cluster** o il cmdlet [`Get-CauRun`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) (vedere la figura 8). Arrestare aggiornamento compatibile con cluster usando il cmdlet [`Disable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) (vedere la figura 9) per impedire che i nodi vengano sospesi e svuotati da aggiornamento compatibile con cluster durante il processo di aggiornamento in sequenza del sistema operativo del cluster.  

        ![screencap che mostra l'output del cmdlet Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figura 8: uso del cmdlet [`Get-CauRun`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) per determinare se gli aggiornamenti compatibili con cluster sono in esecuzione nel cluster**  

        ![screencap che mostra l'output del cmdlet Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figura 9: disabilitazione del ruolo aggiornamenti in grado di riconoscere cluster con il cmdlet [`Disable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps)**  

2. Per ogni nodo del cluster, completare le operazioni seguenti:  
    1. Utilizzando l'interfaccia utente di gestione cluster, selezionare un nodo e utilizzare la **pausa |** Opzione di menu Svuota per svuotare il nodo (vedere la figura 10) o usare il cmdlet [`Suspend-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) (vedere la figura 11).  

        ![screencap che illustra come svuotare i ruoli con l'interfaccia utente di gestione cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figura 10: svuotamento dei ruoli da un nodo con Gestione cluster di failover**  

        ![screencap che mostra l'output del cmdlet Suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figura 11: svuotamento dei ruoli da un nodo con il cmdlet [`Suspend-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps)**  

    2.  Utilizzando l'interfaccia utente di gestione cluster, **rimuovere** il nodo sospeso dal cluster oppure utilizzare il cmdlet [`Remove-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) .  

        ![screencap che mostra l'output del cmdlet Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figura 12: rimuovere un nodo dal cluster usando [`Remove-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet**  

    3.  Riformattare l'unità di sistema ed eseguire un'installazione "Pulisci sistema operativo" di Windows Server 2016 sul nodo usando l'installazione **personalizzata: installa solo Windows (opzione avanzata)** (vedere la figura 13) in Setup. exe. Evitare di selezionare l'opzione **installa Windows e Mantieni file, impostazioni e applicazioni** perché l'aggiornamento in sequenza del sistema operativo del cluster non favorisce l'aggiornamento sul posto.  

        ![screencap dell'installazione guidata di Windows Server 2016 che mostra l'opzione di installazione personalizzata selezionata](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figura 13: opzioni di installazione disponibili per Windows Server 2016**  

    4.  Aggiungere il nodo al dominio di Active Directory appropriato.  
    5.  Aggiungere gli utenti appropriati al gruppo Administrators.  
    6.  Usando l'interfaccia utente di Server Manager o il cmdlet di PowerShell Install-WindowsFeature, installare tutti i ruoli del server necessari, ad esempio Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  Installare la funzionalità clustering di failover usando l'interfaccia utente Server Manager o il cmdlet di PowerShell Install-WindowsFeature.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Installare eventuali funzionalità aggiuntive necessarie per i carichi di lavoro del cluster.  
    9. Controllare le impostazioni di connettività di rete e di archiviazione usando l'interfaccia utente di Gestione cluster di failover.  
    10. Se si usa Windows Firewall, verificare che le impostazioni del firewall siano corrette per il cluster. Ad esempio, i cluster abilitati per l'aggiornamento compatibile con cluster possono richiedere la configurazione del firewall.  
    11. Per i carichi di lavoro Hyper-V, usare l'interfaccia utente di gestione Hyper-V per avviare la finestra di dialogo Virtual Switch Manager (vedere la figura 14).  

        Verificare che il nome dei commutiri virtuali usati sia identico per tutti i nodi host Hyper-V nel cluster.  

        ![screencap che mostra il percorso della finestra di dialogo Gestione commutiri virtuali Hyper-V](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figura 14: gestione commutiri virtuali**  

    12. In un nodo Windows Server 2016 (non usare un nodo Windows Server 2012 R2) usare il Gestione cluster di failover (vedere la figura 15) per connettersi al cluster.  

        ![screencap che mostra la finestra di dialogo Seleziona cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figura 15: aggiunta di un nodo al cluster con Gestione cluster di failover**  

    13. Usare l'interfaccia utente di Gestione cluster di failover o il cmdlet [`Add-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) (vedere la figura 16) per aggiungere il nodo al cluster.  

        ![screencap che mostra l'output del cmdlet Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Figura 16: aggiunta di un nodo al cluster con [`Add-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet**  

        > [!NOTE]  
        > Quando il primo nodo Windows Server 2016 viene aggiunto al cluster, il cluster entra in modalità "mixed-OS" e le risorse principali del cluster vengono spostate nel nodo Windows Server 2016. Un cluster in modalità "mixed-OS" è un cluster completamente funzionante in cui i nuovi nodi vengono eseguiti in modalità di compatibilità con i nodi precedenti. La modalità "mixed-OS" è una modalità transitoria per il cluster. Non è destinato a essere permanente e i clienti dovrebbero aggiornare tutti i nodi del cluster entro quattro settimane.  

    14. Dopo che il nodo Windows Server 2016 è stato aggiunto al cluster, è possibile (facoltativamente) spostare parte del carico di lavoro del cluster nel nodo appena aggiunto per poter ribilanciare il carico di lavoro nel cluster, come indicato di seguito:

        ![screencap che mostra l'output del cmdlet Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figura 17: trasferimento di un carico di lavoro del cluster (ruolo VM del cluster) tramite il cmdlet [`Move-ClusterVirtualMachineRole`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps)**  

        1. Usare **Live Migration** dal gestione cluster di failover per le macchine virtuali o il cmdlet [`Move-ClusterVirtualMachineRole`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) (vedere la figura 17) per eseguire una migrazione in tempo reale delle macchine virtuali.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Usare **Sposta** dalla gestione cluster di failover o dal cmdlet [`Move-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) per altri carichi di lavoro del cluster.  

3. Quando ogni nodo è stato aggiornato a Windows Server 2016 e aggiunto di nuovo al cluster o quando sono stati rimossi tutti i nodi di Windows Server 2012 R2 rimanenti, procedere come segue:  

    > [!IMPORTANT]  
    > -   Dopo aver aggiornato il livello di funzionalità del cluster, non è possibile tornare al livello di funzionalità di Windows Server 2012 R2 e i nodi di Windows Server 2012 R2 non possono essere aggiunti al cluster.
    > -   Fino a quando non viene eseguito il cmdlet [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) , il processo è completamente reversibile e i nodi di windows Server 2012 R2 possono essere aggiunti a questo cluster e i nodi di windows server 2016 possono essere rimossi.  
    > -   Dopo l'esecuzione del cmdlet [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) , saranno disponibili nuove funzionalità.  

    1.  Usando l'interfaccia utente di Gestione cluster di failover o il cmdlet [`Get-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) , verificare che tutti i ruoli del cluster siano in esecuzione nel cluster come previsto. Nell'esempio seguente non viene usata l'archiviazione disponibile, ma viene usato il formato CSV, di conseguenza, l'archiviazione disponibile Visualizza uno stato **offline** (vedere la figura 18).  

        ![screencap che mostra l'output del cmdlet Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figura 18: verifica per determinare se tutti i gruppi di cluster (ruoli del cluster) sono in esecuzione usando il cmdlet [`Get-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps)**  

    2.  Verificare che tutti i nodi del cluster siano online e in esecuzione usando il cmdlet [`Get-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) .  
    3.  Eseguire il cmdlet [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) . non deve essere restituito alcun errore (vedere la figura 19).  

        ![screencap che mostra l'output del cmdlet Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **Figura 19: aggiornamento del livello di funzionalità di un cluster con PowerShell**  

    4.  Dopo l'esecuzione del cmdlet [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) , sono disponibili nuove funzionalità.  

4. Windows Server 2016-riprendere gli aggiornamenti e i backup normali del cluster:  

    1. Se in precedenza è stato eseguito aggiornamento compatibile con cluster, riavviarlo usando l'interfaccia utente di aggiornamento compatibile con cluster o usare il cmdlet [`Enable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) (vedere la figura 20).  

        ![screencap che mostra l'output del](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png) Enable-CauClusterRole  
        **Figura 20: abilitare il ruolo degli aggiornamenti compatibili con il cluster usando il cmdlet [`Enable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps)**  

    2. Riprendere le operazioni di backup.  

5. Abilitare e usare le funzionalità di Windows Server 2016 nelle macchine virtuali Hyper-V.  

    1. Dopo che il cluster è stato aggiornato al livello di funzionalità di Windows Server 2016, molti carichi di lavoro come le VM Hyper-V avranno nuove funzionalità. Per un elenco delle nuove funzionalità di Hyper-V. vedere [eseguire la migrazione e l'aggiornamento di macchine virtuali](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. In ogni nodo host Hyper-V nel cluster usare il cmdlet [`Get-VMHostSupportedVersion`](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) per visualizzare le versioni di configurazione della macchina virtuale Hyper-v supportate dall'host.  

        ![screencap che mostra l'output del cmdlet Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figura 21: visualizzazione delle versioni di configurazione della macchina virtuale Hyper-V supportate dall'host**  

   3. In ogni nodo host Hyper-V nel cluster è possibile aggiornare le versioni di configurazione della macchina virtuale Hyper-V pianificando una breve finestra di manutenzione con gli utenti, eseguendo il backup, disattivando le macchine virtuali ed eseguendo il cmdlet [`Update-VMVersion`](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) (vedere la figura 22). Questa operazione aggiornerà la versione della macchina virtuale e attiverà nuove funzionalità di Hyper-V, eliminando la necessità di aggiornamenti futuri dei componenti di integrazione Hyper-V (IC). Questo cmdlet può essere eseguito dal nodo Hyper-V che ospita la macchina virtuale oppure è possibile usare il parametro `-ComputerName` per aggiornare la versione della macchina virtuale in modalità remota. In questo esempio viene aggiornata la versione di configurazione di VM1 da 5,0 a 7,0 per sfruttare le numerose nuove funzionalità di Hyper-V associate a questa versione di configurazione della macchina virtuale, ad esempio i checkpoint di produzione (backup coerenti con l'applicazione) e la macchina virtuale binaria file di configurazione.  

       ![screencap che mostra il cmdlet Update-VMVersion in azione](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
       **Figura 22: aggiornamento di una versione di macchina virtuale con il cmdlet di PowerShell Update-VMVersion**  

6. È possibile aggiornare i pool di archiviazione usando il cmdlet di PowerShell [Update-StoragePool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) . si tratta di un'operazione online.  

Sebbene siano destinati a scenari basati su cloud privato, in particolare Hyper-V e i cluster di file server di scalabilità orizzontale, che possono essere aggiornati senza tempi di inattività, è possibile usare il processo di aggiornamento in sequenza del sistema operativo del cluster per qualsiasi ruolo del cluster.  

## <a name="restrictions--limitations"></a>Limitazioni/limitazioni  
- Questa funzionalità funziona solo per le versioni di Windows Server 2012 R2 solo per Windows Server 2016. Questa funzionalità non può aggiornare le versioni precedenti di Windows Server, ad esempio Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2016.  
- Ogni nodo di Windows Server 2016 deve essere riformattato/nuovo solo installazione. Il tipo di installazione "sul posto" o "aggiornamento" è sconsigliato.  
- Per aggiungere nodi Windows Server 2016 al cluster, è necessario usare un nodo Windows Server 2016.  
- Quando si gestisce un cluster in modalità con sistema operativo misto, eseguire sempre le attività di gestione da un nodo di livello superiore che esegue Windows Server 2016. I nodi di livello inferiore di Windows Server 2012 R2 non possono usare l'interfaccia utente o gli strumenti di gestione in Windows Server 2016.  
- Si consiglia ai clienti di eseguire rapidamente il processo di aggiornamento del cluster perché alcune funzionalità del cluster non sono ottimizzate per la modalità con sistema operativo misto.  
- Evitare di creare o ridimensionare l'archiviazione nei nodi di Windows Server 2016 mentre il cluster è in esecuzione in modalità a sistema operativo misto a causa di possibili incompatibilità sul failover da un nodo Windows Server 2016 ai nodi di Windows Server 2012 R2 di livello inferiore.  

## <a name="frequently-asked-questions"></a>Domande frequenti  
**Per quanto tempo il cluster di failover può essere eseguito in modalità sistema operativo misto?**  
    Invitiamo i clienti a completare l'aggiornamento entro quattro settimane. Sono disponibili numerose ottimizzazioni in Windows Server 2016. I cluster di file server di scalabilità orizzontale e Hyper-V sono stati aggiornati senza tempi di inattività in meno di quattro ore in totale.  

**Questa funzionalità verrà nuovamente riportata a Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008?**  
    Non sono previsti piani per trasferire questa funzionalità alle versioni precedenti. L'aggiornamento in sequenza del sistema operativo del cluster è la visione dell'aggiornamento dei cluster Windows Server 2012 R2 a Windows Server 2016 e versioni successive.  

**Per il cluster Windows Server 2012 R2 è necessario installare tutti gli aggiornamenti software prima di avviare il processo di aggiornamento in sequenza del sistema operativo del cluster?**  
    Sì, prima di avviare il processo di aggiornamento in sequenza del sistema operativo del cluster, verificare che tutti i nodi del cluster siano aggiornati con gli aggiornamenti software più recenti.  

**È possibile eseguire il cmdlet [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) mentre i nodi sono spenti o sospesi?**  
    No. Per il corretto funzionamento del cmdlet di [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) , è necessario che tutti i nodi del cluster siano attivati e in appartenenza.  

**L'aggiornamento in sequenza del sistema operativo del cluster funziona per qualsiasi carico di lavoro del cluster? Funziona per SQL Server?**  
    Sì, l'aggiornamento in sequenza del sistema operativo cluster funziona per qualsiasi carico di lavoro del cluster. Tuttavia, si tratta solo di tempi di inattività per Hyper-V e cluster di file server di scalabilità orizzontale. La maggior parte degli altri carichi di lavoro comporta tempi di inattività (in genere un paio di minuti) quando eseguono il failover e il failover è necessario almeno una volta durante il processo di aggiornamento in sequenza del sistema operativo del cluster.  

**È possibile automatizzare questo processo usando PowerShell?**  
    Sì, l'aggiornamento in sequenza del sistema operativo del cluster è stato progettato per essere automatizzato tramite PowerShell.  

**Per un cluster di grandi dimensioni con un carico di lavoro e una capacità di failover aggiuntivi, è possibile aggiornare più nodi contemporaneamente?**  
    Sì. Quando un nodo viene rimosso dal cluster per aggiornare il sistema operativo, il cluster disporrà di un nodo inferiore per il failover, di conseguenza avrà una capacità di failover ridotta. Per i cluster di grandi dimensioni con una capacità di carico di lavoro e failover sufficiente, è possibile aggiornare contemporaneamente più nodi. È possibile aggiungere temporaneamente nodi del cluster al cluster per garantire una maggiore capacità di carico di lavoro e failover durante il processo di aggiornamento in sequenza del sistema operativo del cluster.  

**Cosa accade se si individua un problema nel cluster dopo che [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) è stato eseguito correttamente?**  
    Se è stato eseguito il backup del database cluster con un backup dello stato del sistema prima di eseguire [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps), dovrebbe essere possibile eseguire un ripristino autorevole in un nodo del cluster Windows Server 2012 R2 e ripristinare il database e la configurazione originali del cluster.  

**È possibile usare l'aggiornamento sul posto per ogni nodo anziché usare clean-OS Install riformattando l'unità di sistema?**  
    Non è consigliabile usare l'aggiornamento sul posto di Windows Server, ma si è consapevoli che funziona in alcuni casi in cui vengono usati i driver predefiniti. Leggere attentamente tutti i messaggi di avviso visualizzati durante l'aggiornamento sul posto di un nodo del cluster.  

**Se si usa la replica Hyper-V per una macchina virtuale Hyper-V nel cluster Hyper-V, la replica rimarrà intatta durante e dopo il processo di aggiornamento in sequenza del sistema operativo del cluster?**  
    Sì, la replica Hyper-V rimane intatta durante e dopo il processo di aggiornamento in sequenza del sistema operativo del cluster.  

**È possibile usare System Center 2016 Virtual Machine Manager (SCVMM) per automatizzare il processo di aggiornamento in sequenza del sistema operativo del cluster?**  
    Sì, è possibile automatizzare il processo di aggiornamento in sequenza del sistema operativo del cluster usando VMM in System Center 2016.  

## <a name="see-also"></a>Vedi anche  
-   [Note sulla versione: problemi importanti in Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Novità di Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [Novità del clustering di failover in Windows Server](whats-new-in-failover-clustering.md)  
