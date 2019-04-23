---
title: Aggiornamento in sequenza del sistema operativo del cluster
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 03/27/2018
ms.openlocfilehash: 60dacf63f1a355b961f84169060dbd7122a6fd32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842732"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster

> Si applica a: Windows Server (canale semestrale), Windows Server 2016

Aggiornamento in sequenza del cluster del sistema operativo consente agli amministratori di aggiornare il sistema operativo dei nodi del cluster senza interrompere i carichi di lavoro di Scale-Out File Server o Hyper-V. Usando questa funzionalità, è possibile evitare le sanzioni per il tempo di inattività previste dai contratti di servizio.

Aggiornamento in sequenza del cluster del sistema operativo offre i vantaggi seguenti:

- I cluster di failover che esegue macchine virtuali Hyper-V e i carichi di lavoro di File Server di scalabilità orizzontale (SOFS) possono essere aggiornati da Windows Server 2012 R2 (in esecuzione in tutti i nodi del cluster) a Windows Server 2016 (in esecuzione in tutti i nodi del cluster del cluster) senza tempi di inattività. Altri carichi di lavoro del cluster, ad esempio SQL Server, sarà disponibile durante il tempo che necessario per il failover in Windows Server 2016 (in genere meno di cinque minuti).  
- Non richiede hardware aggiuntivo. Tuttavia, è possibile aggiungere altri nodi del cluster temporaneamente a cluster di piccole dimensioni per migliorare la disponibilità del cluster durante il Cluster di aggiornamento del sistema operativo in sequenza elaborano.  
- Il cluster non deve essere arrestato o riavviato.  
- Un nuovo cluster non è obbligatorio. Il cluster esistente viene aggiornato. Inoltre, vengono usati gli oggetti cluster esistenti archiviati in Active Directory.  
- Il processo di aggiornamento è reversibile fino a quando non sceglie il cliente di "punto-di-non di restituzione", quando tutti i nodi del cluster sono in esecuzione Windows Server 2016 e quando viene eseguito il cmdlet di PowerShell Update-ClusterFunctionalLevel.  
- Il cluster può supportare operazioni di applicazione di patch e manutenzione durante l'esecuzione in modalità mista del sistema operativo.  
- Supporta automazione tramite PowerShell e WMI.  
- La proprietà pubblica di cluster **ClusterFunctionalLevel** proprietà indica lo stato del cluster nei nodi del cluster Windows Server 2016. Questa proprietà può essere eseguita una query usando il cmdlet di PowerShell da un nodo cluster Windows Server 2016 che appartiene a un cluster di failover:  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    Un valore pari **8** indica che il cluster sia in esecuzione a livello funzionale di Windows Server 2012 R2. Un valore pari **9** indica che il cluster sia in esecuzione a livello funzionale di Windows Server 2016.  

Questa guida descrive le varie fasi del processo di aggiornamento in sequenza del Cluster del sistema operativo, passaggi di installazione, le limitazioni di funzionalità e domande frequenti (FAQ) ed è applicabile per i seguenti scenari di aggiornamento in sequenza del Cluster del sistema operativo in Windows Server 2016:  
- Cluster Hyper-V  
- Cluster di tipo Scale-Out File Server  

Lo scenario seguente non è supportato in Windows Server 2016:  
-  Aggiornamento in sequenza del sistema operativo del cluster guest usando il disco rigido virtuale (file con estensione vhdx) come archiviazione condivisa del cluster  

Aggiornamento in sequenza del cluster del sistema operativo è completamente supportato da System Center Virtual Machine Manager (SCVMM) 2016. Se si usa SCVMM 2016, vedere [eseguire un aggiornamento in sequenza di un cluster host Hyper-V a Windows Server 2016 in VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807) per indicazioni sull'aggiornamento dei cluster e automatizzare i passaggi descritti in questo documento.  

## <a name="requirements"></a>Requisiti  
Completare i requisiti seguenti prima di iniziare il processo di aggiornamento in sequenza del Cluster del sistema operativo:

- Iniziare con un Cluster di Failover che esegue Windows Server (canale semestrale), Windows Server 2016 o Windows Server 2012 R2.
- Aggiornamento di un cluster di spazi di archiviazione diretta in Windows Server, versione 1709 non è supportata.
- Se il carico di lavoro del cluster è macchine virtuali Hyper-V o File Server di scalabilità orizzontale, è possibile prevedere aggiornamento senza tempi di inattività.
- Verificare che i nodi Hyper-V dispongano di CPU che supportano Addressing tabella SLAT (Second Level) usando uno dei metodi seguenti:  
        : Consente di esaminare il [sei SLAT compatibile? WP8 SDK suggerimento 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) articolo che descrive due metodi per verificare se una CPU supporta SLATs  
        -Scaricare il [Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722) dello strumento per determinare se una CPU supporta SLAT.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Gli stati di transizione del cluster durante l'aggiornamento in sequenza del Cluster del sistema operativo

Questa sezione descrive i vari stati di transizione del cluster Windows Server 2012 R2 che viene aggiornato a Windows Server 2016 con aggiornamento in sequenza del Cluster del sistema operativo.  

Per mantenere i carichi di lavoro del cluster in esecuzione durante il processo di aggiornamento in sequenza del Cluster del sistema operativo, lo spostamento di un carico di lavoro del cluster da un nodo Windows Server 2012 R2 a Windows Server 2016 nodo funziona come se entrambi i nodi erano in esecuzione il sistema operativo Windows Server 2012 R2. Quando i nodi di Windows Server 2016 vengono aggiunti al cluster, operano in una modalità di compatibilità di Windows Server 2012 R2. Una nuova modalità cluster concettuale, denominata "Modalità mista-OS", consente ai nodi di versioni diverse di esistere nello stesso cluster (vedere la figura 1).  

![Illustrazione che mostra le tre fasi di un aggiornamento in sequenza del sistema operativo del cluster: tutti i nodi Windows Server 2012 R2, in modalità mista del sistema operativo e tutti i nodi Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figura 1: Transizioni di stato del sistema operativo cluster**  

Un cluster di Windows Server 2012 R2 entra in modalità mista del sistema operativo quando viene aggiunto un nodo Windows Server 2016 per il cluster. Il processo è completamente reversibile: Windows Server 2016 nodi possono essere rimossi dal cluster e nodi di Windows Server 2012 R2 possono essere aggiunti al cluster in questa modalità. Il "punto di non ritorno" si verifica quando viene eseguito il cmdlet di PowerShell Update-ClusterFunctionalLevel nel cluster. Affinché questo cmdlet abbia esito positivo, tutti i nodi devono essere Windows Server 2016 e tutti i nodi devono essere online.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>Stati di transizione di un cluster a quattro nodi durante l'esecuzione dell'aggiornamento in sequenza del sistema operativo

In questa sezione vengono illustrate e descritte le quattro diverse fasi di un cluster con archiviazione condivisa di cui i nodi vengono aggiornati da Windows Server 2012 R2 a Windows Server 2016.  

"Fase 1" è lo stato iniziale, si inizia con un cluster di Windows Server 2012 R2.  

![Illustrazione che mostra lo stato iniziale: tutti i nodi Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figura 2: Stato iniziale: Cluster di Failover di Windows Server 2012 R2 (fase 1)**  

Nella "fase 2", due nodi sono stati in pausa, svuotati, rimossi, riformattati e installati con Windows Server 2016.  

![Illustrazione che mostra il cluster in modalità mista del sistema operativo: all'esterno del cluster di 4 nodi di esempio, due nodi sono in esecuzione Windows Server 2016 e due nodi sono in esecuzione Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figura 3: Lo stato intermedio: Modalità mista del sistema operativo: Windows Server 2012 R2 e Windows Server 2016 Failover cluster (fase 2)**  

Nella "fase 3", tutti i nodi del cluster sono stati aggiornati a Windows Server 2016 e il cluster è pronto per l'aggiornamento con cmdlet di PowerShell Update-ClusterFunctionalLevel.  

> [!NOTE]  
> In questa fase, il processo può essere invertito completamente e i nodi di Windows Server 2012 R2 possono essere aggiunti a questo cluster.  

![Illustrazione che mostra che il cluster è stato completamente aggiornato a Windows Server 2016 e sia pronto per il cmdlet Update-ClusterFunctionalLevel per portare il livello funzionale del cluster fino a Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figura 4: Lo stato intermedio: Tutti i nodi aggiornati a Windows Server 2016, pronto per Update-ClusterFunctionalLevel (fase 3)**  

Dopo l'esecuzione di Update-ClusterFunctionalLevelcmdlet, il cluster entra "Fase 4", in cui è possibile usare nuove funzionalità di cluster di Windows Server 2016.  

![Illustrazione che mostra che il cluster di aggiornamento del sistema operativo in sequenza è stato completato; tutti i nodi sono stati aggiornati a Windows Server 2016 e il cluster è in esecuzione a livello funzionale del cluster Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figura 5: Stato finale: Cluster di Failover di Windows Server 2016 (fase 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Processo di aggiornamento in sequenza del sistema operativo cluster

In questa sezione viene descritto il flusso di lavoro per l'esecuzione di aggiornamento in sequenza del Cluster del sistema operativo.  

![Illustrazione che mostra il flusso di lavoro per l'aggiornamento di un cluster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figura 6: In sequenza del flusso di lavoro di processo di aggiornamento del sistema operativo cluster**  

Aggiornamento in sequenza del sistema operativo del cluster include i passaggi seguenti:  

1. Preparare il cluster per l'aggiornamento del sistema operativo, come indicato di seguito:  
    1. Aggiornamento in sequenza del cluster del sistema operativo, è necessario rimuovere un nodo alla volta dal cluster. Verificare la presenza di una capacità sufficiente nel cluster per mantenere la disponibilità elevata di contratti di servizio quando uno dei nodi del cluster viene rimosso dal cluster per un aggiornamento del sistema operativo. In altre parole, si richiedono la possibilità di failover dei carichi di lavoro in un altro nodo quando un nodo viene rimosso dal cluster durante il processo di aggiornamento in sequenza del Cluster del sistema operativo? Il cluster ha la capacità di eseguire i carichi di lavoro necessarie quando un nodo viene rimosso dal cluster per l'aggiornamento in sequenza del Cluster del sistema operativo?  
    2. Per i carichi di lavoro di Hyper-V, verificare che tutti gli host Windows Server 2016 Hyper-V abbiano CPU supportano indirizzo tabella SLAT (Second Level). Solo le macchine in grado di supportare SLAT possono usare il ruolo Hyper-V in Windows Server 2016.  
    3. Verificare che eventuali backup del carico di lavoro completate e provare a effettuare un backup del cluster. Arrestare le operazioni di backup durante l'aggiunta di nodi al cluster.  
    4. Verificare che tutti i nodi del cluster sono online/esecuzione/backup utilizzando il [ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet (vedere la figura 7).  

        ![Schermata che mostra i risultati dell'esecuzione del cmdlet Get-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figura 7: Determinare lo stato del nodo usando il cmdlet Get-ClusterNode**  

    5. Se si eseguono aggiornamenti (compatibile con Cluster), verificare se aggiornamento compatibile con cluster è attualmente in esecuzione usando il **l'aggiornamento del Cluster-Aware** dell'interfaccia utente, o il [ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet (vedere la figura 8). Interrompere l'uso di aggiornamento compatibile con il [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet (vedere Figura 9) per impedire tutti i nodi sospensione e spossato dall'aggiornamento compatibile con cluster durante il processo di aggiornamento in sequenza del Cluster del sistema operativo.  

        ![Schermata che mostra l'output del cmdlet Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figura 8: Usando il [ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet per determinare se gli aggiornamenti compatibili con Cluster è in esecuzione nel cluster**  

        ![Schermata che mostra l'output del cmdlet Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figura 9: La disabilitazione di ruolo di aggiornamento compatibile con Cluster con il [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet**  

2. Per ogni nodo del cluster, completare le operazioni seguenti:  
    1. Usando Gestione Cluster di UI, selezionare un nodo e usare il **Pause | Svuotare** opzione di menu per svuotare il nodo (vedere la figura 10) o usare il [ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet (vedere la figura 11).  

        ![Schermata che illustra come svuotare i ruoli con la UI di Cluster Manager](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figura 10: Svuotamento dei ruoli da un nodo tramite Gestione Cluster di Failover**  

        ![Schermata che mostra l'output del cmdlet Suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figura 11: Svuotamento dei ruoli da un nodo tramite il [ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet**  

    2.  Usando Gestione Cluster di UI, **rimozione** il nodo in pausa dal cluster oppure utilizzare il [ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet.  

        ![Schermata che mostra l'output del cmdlet Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figura 12: Rimuovere un nodo dal cluster usando [ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet**  

    3.  Riformattare le unità di sistema ed eseguire una "installazione pulita del sistema operativo" di Windows Server 2016 nel nodo usando il **personalizzato: Installa solo Windows (avanzate)** opzione di installazione (vedere Figura 13) setup.exe. Evitare di selezionare il **aggiornare: Installare Windows e mantenere i file, impostazioni e applicazioni** opzione dall'aggiornamento in sequenza del Cluster del sistema operativo non incoraggiare l'aggiornamento sul posto.  

        ![Schermata dell'installazione guidata di Windows Server 2016 con l'opzione di installazione personalizzata selezionata](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figura 13: Opzioni di installazione disponibili per Windows Server 2016**  

    4.  Aggiungere il nodo al dominio Active Directory appropriato.  
    5.  Aggiungere gli utenti appropriati al gruppo Administrators.  
    6.  Usare il cmdlet di PowerShell Install-WindowsFeature o Server Manager UI, installare i ruoli del server necessari, ad esempio Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  Usare il cmdlet di PowerShell Install-WindowsFeature o Server Manager UI, installare la funzionalità Clustering di Failover.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Installare le ulteriori funzionalità necessarie per i carichi di lavoro del cluster.  
    9. Controllare impostazioni di connettività di rete e archiviazione utilizzando la UI gestione Cluster di Failover.  
    10. Se si utilizza Windows Firewall, verificare che le impostazioni del Firewall siano corrette per il cluster. Ad esempio, i cluster di aggiornamento (compatibile con Cluster) abilitato richieda la configurazione del Firewall.  
    11. Per i carichi di lavoro di Hyper-V, usare l'interfaccia utente gestione di Hyper-V per avviare la finestra di dialogo Gestione commutatori virtuali (vedere la figura 14).  

        Verificare che il nome del Switch(s) virtuale usati siano identici per tutti i nodi host di Hyper-V nel cluster.  

        ![Schermata che mostra la posizione della finestra di dialogo Gestione commutatori virtuali Hyper-V](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figura 14: Gestione commutatori virtuali**  

    12. In un nodo Windows Server 2016 (non utilizzare un nodo Windows Server 2012 R2), utilizzare Gestione Cluster di Failover (vedere Figura 15) per connettersi al cluster.  

        ![Schermata che mostra la finestra di dialogo selezionare cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figura 15: Aggiunta di un nodo al cluster usando Gestione Cluster di Failover**  

    13. Usare entrambi la UI gestione Cluster di Failover o il [ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet (vedere Figura 16) per aggiungere il nodo al cluster.  

        ![Schermata che mostra l'output del cmdlet Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Figura 16: Aggiunta di un nodo nel cluster usando [ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet**  

        > [!NOTE]  
        > Quando il primo nodo Windows Server 2016 viene aggiunto al cluster, il cluster entra in modalità "Mixed-OS", e le risorse principali del cluster vengono spostate nel nodo Windows Server 2016. Un cluster in modalità "Mixed-OS" è un cluster completamente funzionale in cui i nuovi nodi eseguito in una modalità di compatibilità con i nodi precedenti. Modalità di "Operativi misti" è una modalità transitoria per il cluster. Non deve essere permanente e i clienti dovrebbero aggiornare tutti i nodi del cluster relativi all'interno di quattro settimane.  

    14. Dopo Windows Server 2016 nodo viene aggiunto al cluster, è possibile (facoltativamente) spostare parte del carico di lavoro del cluster per il nodo appena aggiunto per ribilanciare il carico di lavoro nel cluster come indicato di seguito:

        ![Schermata che mostra l'output del cmdlet Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figura 17: Lo spostamento di un cluster del carico di lavoro (ruolo di macchina virtuale del cluster) usando [ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet**  

        1. Uso **Live Migration** da Gestione Cluster di Failover per le macchine virtuali o nella [ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet (vedere Figura 17) per eseguire una migrazione in tempo reale delle macchine virtuali.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Uso **spostare** da Gestione Cluster di Failover o il [ `Move-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) cmdlet per altri carichi di lavoro del cluster.  

3. Quando ogni nodo è stato aggiornato a Windows Server 2016 e aggiunto al cluster o quando sono stati eliminati eventuali nodi rimanenti di Windows Server 2012 R2, eseguire le operazioni seguenti:  

    > [!IMPORTANT]  
    > -   Dopo aver aggiornato il livello funzionale del cluster, sarà più possibile tornare a livello di funzionalità di Windows Server 2012 R2 e Windows Server 2012 R2 nodi non possono essere aggiunti al cluster.
    > -   Fino a quando la [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet viene eseguito, il processo è completamente reversibile e nodi di Windows Server 2012 R2 possono essere aggiunti al cluster e nodi di Windows Server 2016 possono essere rimosso.  
    > -   Dopo il [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet viene eseguito, le nuove funzionalità sarà disponibile.  

    1.  Utilizzando la UI gestione Cluster di Failover o il [ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet, verificare che tutti i ruoli del cluster sono in esecuzione nel cluster, come previsto. Nell'esempio seguente, spazio di archiviazione disponibile non utilizzato, viene invece usato CSV, di conseguenza, Visualizza risorse di archiviazione disponibili un' **Offline** stato (vedere Figura 18).  

        ![Schermata che mostra l'output del cmdlet Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figura 18: Verifica che tutti i cluster di gruppi (i ruoli del cluster) siano in esecuzione usando il [ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet**  

    2.  Verificare che tutti i nodi del cluster sono online e in esecuzione usando il [ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet.  
    3.  Eseguire la [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet - non devono essere restituiti errori (vedere la figura 19).  

        ![Schermata che mostra l'output del cmdlet Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **Figura 19: Aggiornare il livello funzionale di un cluster mediante PowerShell**  

    4.  Dopo il [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet viene eseguito, sono disponibili nuove funzionalità.  

4. Windows Server 2016 - riprendere gli aggiornamenti del cluster normale e i backup:  

    1. Se in precedenza si eseguiva l'aggiornamento compatibile con cluster, riavviarlo, usare la UI CAU o usare il [ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet (vedere Figura 20).  

        ![Schermata che mostra l'output di Enable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **Figura 20: Abilita gli aggiornamenti compatibili con Cluster ruolo usando il [ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet**  

    2. Riprendere le operazioni di backup.  

5. Abilitare e usare le funzionalità di Windows Server 2016 in macchine virtuali Hyper-V.  

    1. Dopo che il cluster è stato aggiornato a livello di funzionalità di Windows Server 2016, molti carichi di lavoro, ad esempio le macchine virtuali Hyper-V saranno disponibili nuove funzionalità. Per un elenco delle nuove funzionalità di Hyper-V. vedere [eseguire la migrazione e aggiornamento delle macchine virtuali](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. In ogni nodo di host Hyper-V nel cluster, usare il [ `Get-VMHostSupportedVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) cmdlet per visualizzare le versioni di configurazione della macchina virtuale Hyper-V supportate dall'host.  

        ![Schermata che mostra l'output del cmdlet Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figura 21: Visualizzare le versioni di configurazione della macchina virtuale Hyper-V supportate dall'host**  

   3.  In ogni nodo di host Hyper-V nel cluster, è possibile aggiornare versioni della configurazione della macchina virtuale Hyper-V, la pianificazione di una finestra di manutenzione breve con gli utenti, backup, la disattivazione di macchine virtuali e in esecuzione la [ `Update-VMVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) cmdlet (vedere Figura 22). Questo verrà aggiornare la versione di macchina virtuale e abilitare le nuove funzionalità di Hyper-V, eliminando la necessità per gli aggiornamenti futuri di componenti di integrazione Hyper-V (IC). Questo cmdlet può essere eseguito dal nodo Hyper-V che ospita la macchina virtuale, o `-ComputerName` parametro può essere usato per aggiornare la versione della macchina virtuale in modalità remota. In questo esempio, qui abbiamo aggiornamento la versione di configurazione di VM1 da 5.0 a 7.0 per sfruttare i vantaggi di molte nuove funzionalità di Hyper-V associata a questa versione di configurazione della macchina virtuale, ad esempio i checkpoint di produzione (backup coerenti con l'applicazione) e binario della macchina virtuale file di configurazione.  

        ![Schermata che mostra il cmdlet Update-VMVersion in azione](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
        **Figura 22: Aggiornamento della versione della macchina virtuale usando il cmdlet PowerShell Update-VMVersion**  

4.  I pool di archiviazione possono essere aggiornati usando il [Update-StoragePool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) cmdlet di PowerShell - si tratta di un'operazione online.  

Anche se vengono esaminate scenari di Cloud privato, in particolare Hyper-V e cluster di tipo Scale-out File Server, che può essere aggiornato senza tempi di inattività, il processo di aggiornamento in sequenza del Cluster del sistema operativo possono essere utilizzati per tutti i ruoli del cluster.  

## <a name="restrictions--limitations"></a>Restrizioni o limitazioni  
- Questa funzionalità funziona solo per Windows Server 2012 R2 per solo le versioni di Windows Server 2016. Questa funzionalità non è possibile aggiornare le versioni precedenti di Windows Server, ad esempio Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2016.  
- Ogni nodo di Windows Server 2016 deve essere solo installazione nuove o riformattate. "Posto" o "aggiornare" tipo di installazione è sconsigliato.  
- Un nodo Windows Server 2016 deve essere utilizzato per aggiungere Windows Server 2016 nodi al cluster.  
- Quando si gestisce un cluster in modalità mista del sistema operativo, eseguire sempre le attività di gestione da un nodo di livello superiore che esegue Windows Server 2016. I nodi di livello inferiore Windows Server 2012 R2 non è possibile usare gli strumenti dell'interfaccia utente o la gestione con Windows Server 2016.  
- Microsoft invita i clienti per spostarsi rapidamente tra il processo di aggiornamento del cluster poiché alcune funzionalità di cluster non sono ottimizzate per la modalità mista del sistema operativo.  
- Evitare la creazione o il ridimensionamento di archiviazione in Windows Server 2016 nodi mentre il cluster è in esecuzione in modalità mista del sistema operativo a causa di possibili problemi di incompatibilità in caso di failover da un nodo Windows Server 2016 per i nodi di livello inferiore Windows Server 2012 R2.  

## <a name="frequently-asked-questions"></a>Domande frequenti  
**Il tempo il cluster di failover di esecuzione in modalità mista del sistema operativo?**  
    Microsoft invita i clienti a completare l'aggiornamento all'interno di quattro settimane. Esistono numerose ottimizzazioni in Windows Server 2016. È stata aggiornata correttamente Hyper-V e Scale-out File Server di cluster senza tempi di inattività totale inferiore a quattro ore.  

**Si verrà porta indietro per Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 questa funzionalità?**  
    Si prevede di porta indietro per le versioni precedenti questa funzionalità non è disponibile. Aggiornamento in sequenza del cluster del sistema operativo è l'obiettivo per l'aggiornamento dei cluster di Windows Server 2012 R2 a Windows Server 2016 e altro ancora.  

**Il cluster di Windows Server 2012 R2 è necessario disporre di tutti gli aggiornamenti software installati prima di avviare il processo di aggiornamento in sequenza del Cluster del sistema operativo?**  
    Sì, prima di iniziare il processo di aggiornamento in sequenza del Cluster del sistema operativo, verificare che tutti i nodi del cluster vengono aggiornati con gli aggiornamenti software più recenti.  

**È possibile eseguire la [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet mentre i nodi sono disattivati o messo in pausa?**  
    No. Tutti i nodi del cluster devono essere su e in abbonamento attivo per il [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) funzionamento del cmdlet.  

**Aggiornamento in sequenza del Cluster del sistema operativo funziona per qualsiasi carico di lavoro del cluster? Funziona per SQL Server?**  
    Sì, l'aggiornamento in sequenza del Cluster del sistema operativo funziona per qualsiasi carico di lavoro del cluster. Tuttavia, è solo senza tempi di inattività per Hyper-V e cluster di tipo Scale-out File Server. La maggior parte degli altri carichi di lavoro dei tempi di inattività (in genere un paio di minuti) quando vengono il failover e il failover è necessaria almeno una volta durante il processo di aggiornamento in sequenza del Cluster del sistema operativo.  

**È possibile automatizzare questo processo usando PowerShell?**  
    Sì, abbiamo progettato Cluster OS Rolling eseguire l'aggiornamento a essere automatizzate tramite PowerShell.  

**Per un cluster di grandi dimensioni con il carico di lavoro aggiuntivo e la capacità di failover, è possibile eseguire l'aggiornamento più nodi contemporaneamente?**  
    Sì. Quando un nodo viene rimosso dal cluster per eseguire l'aggiornamento del sistema operativo, il cluster avrà un nodo in meno per il failover, quindi avrà una capacità di failover ridotto. Per i cluster di grandi dimensioni con sufficiente del carico di lavoro e la capacità di failover, più nodi possono essere aggiornati contemporaneamente. È possibile aggiungere temporaneamente i nodi del cluster al cluster per fornire il carico di lavoro migliorata e la capacità di failover durante il processo di aggiornamento in sequenza del Cluster del sistema operativo.  

**Cosa accade se rileva un problema nel cluster dopo aver [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) è stato eseguito correttamente?**  
    Se si have eseguito il backup il database del cluster con un backup dello stato del sistema prima dell'esecuzione [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps), dovrebbe essere possibile eseguire un autorevole ripristinare in un nodo del cluster Windows Server 2012 R2 e il cluster originale configurazione e un database.  

**È possibile utilizzare l'aggiornamento sul posto per ogni nodo invece di usare OS clean install riformattando le unità di sistema?**  
    Non si consiglia l'uso di aggiornamento sul posto di Windows Server, ma siamo consapevoli del fatto che funzioni in alcuni casi in cui vengono usati driver predefiniti. Leggere con attenzione tutti i messaggi di avviso visualizzato durante l'aggiornamento sul posto di un nodo del cluster.  

**Se si usa la replica Hyper-V per una macchina virtuale Hyper-V nel cluster Hyper-V, replica rimarranno invariata durante e dopo il processo di aggiornamento in sequenza del Cluster del sistema operativo?**  
    Sì, la replica Hyper-V rimanga intatta durante e dopo il processo di aggiornamento in sequenza del Cluster del sistema operativo.  

**È possibile usare System Center 2016 Virtual Machine Manager (SCVMM) per automatizzare il processo di aggiornamento in sequenza del Cluster del sistema operativo?**  
    Sì, è possibile automatizzare il processo di aggiornamento in sequenza del Cluster del sistema operativo con VMM in System Center 2016.  

## <a name="see-also"></a>Vedere anche  
-   [Note sulla versione: Problemi importanti in Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Quali sono le novità in Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [What ' s New in Failover Clustering in Windows Server](whats-new-in-failover-clustering.md)  
