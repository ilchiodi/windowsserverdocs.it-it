---
title: Aggiornamento in sequenza del sistema operativo del cluster
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2017
ms.openlocfilehash: 8463c163294a4d2223a74b7cfeaea6ac5ae4fcfe
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster

> Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Aggiornamento in sequenza del cluster del sistema operativo consente all'amministratore di aggiornare il sistema operativo dei nodi del cluster senza interrompere i carichi di lavoro Scale-Out File Server o Hyper-V. Usando questa funzionalità, è possono evitare le sanzioni tempi di inattività contro contratti livello di servizio (SLA).

Aggiornamento in sequenza del cluster del sistema operativo offre i vantaggi seguenti:

- I cluster di failover che esegue macchine virtuali Hyper-V e i carichi di lavoro del File Server di scalabilità orizzontale (SOFS) possono essere aggiornati da Windows Server 2012 R2 (in esecuzione in tutti i nodi del cluster) per Windows Server 2016 (in esecuzione in tutti i nodi di cluster del cluster) senza tempi di inattività. Altri carichi di lavoro del cluster, ad esempio SQL Server, potrebbe non essere disponibile durante il tempo che necessario per eseguire il failover di Windows Server 2016 (in genere meno di cinque minuti).  
- Non richiede hardware aggiuntivo. Tuttavia, è possibile aggiungere altri nodi del cluster temporaneamente per piccoli cluster per migliorare la disponibilità del cluster durante il Cluster di aggiornamento del sistema operativo in sequenza processo.  
- Il cluster non deve necessariamente essere arrestato o riavviato.  
- Un nuovo cluster non è obbligatorio. Cluster esistente viene aggiornato. Inoltre, vengono utilizzati gli oggetti cluster esistenti archiviati in Active Directory.  
- Il processo di aggiornamento è reversibile fino a quando non possono il cliente di "punto di-no-ritorno", quando tutti i nodi cluster è in esecuzione Windows Server 2016 e quando viene eseguito il cmdlet PowerShell Update-ClusterFunctionalLevel.  
- Il cluster può supportare l'applicazione di patch e operazioni di manutenzione durante l'esecuzione in modalità mista del sistema operativo.  
- Supporta l'automazione tramite PowerShell e WMI.  
- La proprietà pubblica cluster **ClusterFunctionalLevel** proprietà indica lo stato del cluster nei nodi del cluster Windows Server 2016. Questa proprietà può essere richiesta utilizzando il cmdlet di PowerShell da un nodo cluster Windows Server 2016 che appartiene a un cluster di failover:  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    Il valore **8** indica che il cluster sia in esecuzione a livello funzionale di Windows Server 2012 R2. Il valore **9** indica che il cluster sia in esecuzione a livello funzionale di Windows Server 2016.  

Questa guida descrive le varie fasi del processo di aggiornamento in sequenza del Cluster del sistema operativo, i passaggi dell'installazione, limitazioni di funzionalità e domande più frequenti (FAQ) ed è applicabile per i seguenti scenari di aggiornamento in sequenza del Cluster del sistema operativo in Windows Server 2016:  
- Cluster Hyper-V  
- Cluster di File Server di scalabilità orizzontale  

Lo scenario seguente non è supportato in Windows Server 2016:  
-  Sistema operativo aggiornamento in sequenza cluster dei guest cluster con disco rigido virtuale (.vhdx file) come archiviazione condivisa  

Aggiornamento in sequenza del cluster del sistema operativo è completamente supportata da System Center Virtual Machine Manager (SCVMM) 2016. Se si utilizza SCVMM 2016, vedere [l'aggiornamento a Windows Server 2012 R2 cluster a Windows Server 2016 in VMM](https://technet.microsoft.com/library/mt445417.aspx) per indicazioni sull'aggiornamento dei cluster e l'automazione i passaggi descritti in questo documento.  

## <a name="requirements"></a>Requisiti  
Prima di iniziare il processo di aggiornamento in sequenza del Cluster del sistema operativo, completare i seguenti requisiti:

- Iniziare con un Cluster di Failover che esegue Windows Server (canale annuale e virgola), Windows Server 2016 o Windows Server 2012 R2.
- Versione 1709 aggiornamento di un cluster di spazi di archiviazione diretta per Windows Server, non è supportata.
- Se il carico di lavoro del cluster, le macchine virtuali Hyper-V o File Server di scalabilità orizzontale, è possibile prevedere aggiornamento zero-tempo di inattività.
- Verificare che i nodi Hyper-V dispongano di CPU che supportano indirizzamento tabella SLAT (Second Level) utilizzando uno dei metodi seguenti.  
        -Consultare il [sei SLAT compatibile? WP8 SDK suggerimento 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) articolo che descrive due metodi per verificare se una CPU supporta SLATs  
        -Scarica il [Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722) strumento per determinare se una CPU supporta SLAT.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Gli stati di transizione durante l'aggiornamento in sequenza del Cluster del sistema operativo del cluster

Questa sezione descrive i diversi stati di transizione del cluster di Windows Server 2012 R2 che viene aggiornato a Windows Server 2016 tramite l'aggiornamento in sequenza del Cluster del sistema operativo.  

Per mantenere i carichi di lavoro del cluster in esecuzione durante il processo di aggiornamento in sequenza del Cluster del sistema operativo, lo spostamento di un carico di lavoro del cluster da un nodo di Windows Server 2012 R2 a Windows Server 2016 nodo funziona come se entrambi i nodi erano in esecuzione il sistema operativo Windows Server 2012 R2. Quando Windows Server 2016 nodi vengono aggiunti al cluster, operano in una modalità di compatibilità di Windows Server 2012 R2. Una nuova modalità concettuale cluster denominata "Modalità mista del sistema operativo", consente ai nodi di versioni diverse di esistere nello stesso cluster (vedere la figura 1).  

![Immagine che mostra le tre fasi di un aggiornamento in sequenza del sistema operativo del cluster: tutti i nodi di Windows Server 2012 R2, la modalità mista del sistema operativo e tutti i nodi di Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figura 1: Le transizioni di stato del sistema operativo del Cluster**  

Un cluster di Windows Server 2012 R2 entra in modalità mista del sistema operativo quando un nodo di Windows Server 2016 viene aggiunto al cluster. Il processo è completamente reversibile: Windows Server 2016 nodi possono essere rimossi dal cluster e Windows Server 2012 R2 nodi possono essere aggiunti al cluster in questa modalità. "Punto di non ritorno" si verifica quando viene eseguito il cmdlet PowerShell Update-ClusterFunctionalLevel nel cluster. Affinché questo cmdlet abbia esito positivo, tutti i nodi devono essere Windows Server 2016, e tutti i nodi devono essere online.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>Stati di transizione di un cluster a quattro nodi durante l'esecuzione di aggiornamento del sistema operativo in sequenza

In questa sezione illustra e descrive le quattro diverse fasi di un cluster con archiviazione condivisa cui nodi vengono aggiornati da Windows Server 2012 R2 a Windows Server 2016.  

"Fase 1" è lo stato iniziale, iniziamo con un cluster di Windows Server 2012 R2.  

![Immagine che mostra lo stato iniziale: tutti i nodi di Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figura 2: Stato iniziale: Cluster di Failover Windows Server 2012 R2 (la fase 1)**  

In "fase 2", due nodi sono stato sospeso, completamente scarica, rimosso, riformattati e installati con Windows Server 2016.  

![Immagine che mostra il cluster in modalità mista del sistema operativo: dal cluster a 4 nodi di esempio, due nodi in esecuzione Windows Server 2016 e due nodi eseguono Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figura 3: Intermedia stato: modalità mista del sistema operativo: Windows Server 2012 R2 e Windows Server 2016 Failover cluster (fase 2)**  

In "fase 3", tutti i nodi del cluster sono stati aggiornati a Windows Server 2016 e il cluster è pronto per essere aggiornato con i cmdlet di PowerShell Update-ClusterFunctionalLevel.  

> [!NOTE]  
> In questa fase, il processo può essere completamente annullato e i nodi di Windows Server 2012 R2 è possibile aggiungere a questo cluster.  

![Immagine che mostra il cluster è stato aggiornato completamente a Windows Server 2016 che è pronto per il cmdlet Update-ClusterFunctionalLevel portare il livello funzionale del cluster fino a Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figura 4: Intermedia stato: tutti i nodi aggiornati a Windows Server 2016, pronto per Update-ClusterFunctionalLevel (fase 3)**  

Dopo l'esecuzione di Update-ClusterFunctionalLevelcmdlet, il cluster immette "Fase 4", in cui può essere utilizzata nuove funzionalità di cluster Windows Server 2016.  

![Immagine che mostra che il sequenza di aggiornamento del sistema operativo del cluster è stato completato; tutti i nodi sono stati aggiornati a Windows Server 2016 e il cluster è in esecuzione a livello funzionale di cluster Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figura 5: Finale stato: Windows Server 2016 Failover Cluster (fase 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Sequenza di processo di aggiornamento del sistema operativo cluster

In questa sezione viene descritto il flusso di lavoro per l'esecuzione di aggiornamento in sequenza del Cluster del sistema operativo.  

![Immagine che mostra il flusso di lavoro per l'aggiornamento di un cluster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figura 6: Processo di aggiornamento del flusso di lavoro di sequenza del sistema operativo del Cluster di**  

Aggiornamento in sequenza del sistema operativo del cluster include i passaggi seguenti:  

1. Preparare il cluster per l'aggiornamento del sistema operativo, come indicato di seguito:  
    1. Aggiornamento in sequenza del cluster del sistema operativo richiede la rimozione di un nodo alla volta dal cluster. Verificare la presenza di una capacità sufficiente nel cluster per gestire i contratti a disponibilità elevata quando uno dei nodi del cluster viene rimosso dal cluster per un aggiornamento del sistema operativo. In altre parole, è necessaria la funzionalità per i carichi di lavoro di failover in un altro nodo quando viene rimosso un nodo dal cluster durante il processo di aggiornamento in sequenza del Cluster del sistema operativo? Il cluster dispone della capacità di eseguire i carichi di lavoro necessarie quando viene rimosso un nodo dal cluster per l'aggiornamento in sequenza del Cluster del sistema operativo?  
    2. Per carichi di lavoro Hyper-V, controllare che tutti gli host Windows Server 2016 Hyper-V CPU supportano tabella indirizzo di secondo livello (SLAT). Solo i computer in grado di supportare SLAT è possono utilizzare il ruolo Hyper-V in Windows Server 2016.  
    3. Verifica che hanno completato il backup di qualsiasi carico di lavoro ed è consigliabile eseguire un backup dei cluster. Interrompere le operazioni di backup durante l'aggiunta di nodi al cluster.  
    4. Verificare che tutti i nodi del cluster siano in linea/esecuzione/backup utilizzando il [`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx)cmdlet (vedere la figura 7).  

        ![ScreenCap che mostra i risultati dell'esecuzione del cmdlet Get-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figura 7: Determinare lo stato del nodo utilizzando cmdlet Get-ClusterNode**  

    5. Se si eseguono Cluster aggiornamenti compatibile, verificare se aggiornamento compatibile con cluster è attualmente in esecuzione usando il **l'aggiornamento del Cluster-Aware** dell'interfaccia utente, o [`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx)cmdlet (vedi figura 8). Interrompi l'uso di aggiornamento compatibile con cluster il [`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx)cmdlet (vedi figura 9) per impedire eventuali nodi sospensione e esaurirsi da aggiornamento compatibile con cluster durante il processo di aggiornamento in sequenza del Cluster del sistema operativo.  

        ![ScreenCap che mostra l'output del cmdlet Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figura 8: Utilizzo di [`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx)cmdlet per determinare se gli aggiornamenti compatibile con Cluster è in esecuzione nel cluster**  

        ![ScreenCap che mostra l'output del cmdlet Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figura 9: Disabilitando il ruolo di aggiornamento compatibile con Cluster usando il [`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx)cmdlet**  

2. Per ogni nodo del cluster, completare le operazioni seguenti:  
    1. Utilizzando Gestione Cluster di UI, selezionare un nodo e utilizzare il **pausa | Svuotare** opzione di menu per svuotare il nodo (vedere la figura 10) o usare il [`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx)cmdlet (vedi figura 11).  

        ![ScreenCap che illustra come svuotare ruoli con la UI gestione Cluster di](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figura 10: Svuotamento dei ruoli da un nodo utilizzando Gestione Cluster di Failover**  

        ![ScreenCap che mostra l'output del cmdlet Suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figura 11: Consuma ruoli da un nodo tramite il [`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx)cmdlet**  

    2.  Utilizzando Gestione Cluster di UI, **Rimuovi** il nodo in pausa dal cluster oppure utilizzare il [`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx)cmdlet.  

        ![ScreenCap che mostra l'output del cmdlet Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figura 12: Rimuovere un nodo dal cluster utilizzando [`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx)cmdlet**  

    3.  Riformattare l'unità del sistema e di eseguire una "installazione pulita del sistema operativo" di Windows Server 2016 nel nodo tramite il **personalizzata: installa solo Windows (utenti esperti)** opzione di installazione (vedi figura 13) setup.exe. Evita di selezionare il **aggiornamento: installazione di Windows e mantenere i file, impostazioni e applicazioni** opzione dall'aggiornamento in sequenza del Cluster del sistema operativo non incoraggiare l'aggiornamento sul posto.  

        ![ScreenCap dell'installazione guidata di Windows Server 2016 che mostra l'opzione di installazione personalizzato selezionato](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figura 13: Opzioni di installazione disponibili per Windows Server 2016**  

    4.  Aggiungere il nodo al dominio di Active Directory appropriato.  
    5.  Aggiungere gli utenti appropriati al gruppo Administrators.  
    6.  Utilizzando il cmdlet di Server Manager UI o PowerShell Install-WindowsFeature, installare i ruoli del server che è necessario, ad esempio Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  Utilizzando il cmdlet di Server Manager UI o PowerShell Install-WindowsFeature, installare la funzionalità Clustering di Failover.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Installare le funzionalità aggiuntive necessarie per i carichi di lavoro del cluster.  
    9. Controllare impostazioni di connettività di rete e di archiviazione usando la UI gestione Cluster di Failover.  
    10. Se viene utilizzato Windows Firewall, verificare che le impostazioni del Firewall siano corrette per il cluster. I cluster del Cluster aggiornamento compatibile con abilitato, ad esempio, potrebbe essere necessario alla configurazione del Firewall.  
    11. Per carichi di lavoro Hyper-V, usare l'interfaccia utente di Hyper-V Manager per avviare la finestra di dialogo Gestione commutatori virtuali (vedi figura 14).  

        Verificare che il nome dello switch virtuale utilizzato sono identici per tutti i nodi host Hyper-V nel cluster.  

        ![ScreenCap che mostra la posizione della finestra di dialogo Gestione commutatori virtuali Hyper-V](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figura 14: Gestione commutatori virtuali**  

    12. In un nodo di Windows Server 2016 (non si utilizza un nodo di Windows Server 2012 R2), utilizzare Gestione Cluster di Failover (vedi figura 15) per connettersi al cluster.  

        ![Visualizzare la finestra di dialogo Seleziona cluster ScreenCap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figura 15: Aggiunta di un nodo al cluster usando Gestione Cluster di Failover**  

    13. Utilizzare uno dei due l'UI gestione Cluster di Failover o [`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx)cmdlet (vedi figura 16) per aggiungere il nodo al cluster.  

        ![ScreenCap che mostra l'output del cmdlet Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Nella figura 16: Aggiunta di un nodo al cluster utilizzando [`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx)cmdlet**  

        > [!NOTE]  
        > Quando il primo nodo di Windows Server 2016 viene aggiunto al cluster, il cluster viene attivata la modalità "Misto del sistema operativo" e le risorse principali del cluster vengono spostate nel nodo di Windows Server 2016. Un cluster in modalità "Misto so" è un cluster completamente funzionale in cui i nuovi nodi eseguito in una modalità di compatibilità con i nodi precedente. Modalità di "Operativi misti" è una modalità transitoria per il cluster. Non deve essere permanente e i clienti sono previsti per aggiornare tutti i nodi del cluster relativi all'interno di quattro settimane.  

    14. Dopo Windows Server 2016 nodo viene aggiunto al cluster, è possibile, facoltativamente, spostare alcuni del carico di lavoro del cluster nel nodo appena aggiunta per bilanciare il carico di lavoro all'interno del cluster, come indicato di seguito:

        ![ScreenCap che mostra l'output del cmdlet Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figura 17: Lo spostamento di un carico di lavoro (ruolo macchina virtuale del cluster) cluster mediante [`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx)cmdlet**  

        1. Utilizzare **Live Migration** da Gestione Cluster di Failover per le macchine virtuali o [`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx)cmdlet (vedi figura 17) per eseguire una migrazione in tempo reale delle macchine virtuali.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Utilizzare **spostare** da Gestione Cluster di Failover o [`Move-ClusterGroup`](https://technet.microsoft.com/library/ee461002.aspx)cmdlet per altri carichi di lavoro del cluster.  

3. Quando viene eseguito l'aggiornamento a Windows Server 2016 e aggiunto nuovamente al cluster di ogni nodo o quando sono stati eliminati eventuali nodi rimanenti di Windows Server 2012 R2, eseguire le operazioni seguenti:  

    > [!IMPORTANT]  
    > -   Dopo aver aggiornato il livello di funzionalità del cluster, non puoi tornare a livello di funzionalità di Windows Server 2012 R2 e Windows Server 2012 R2 nodi non possono essere aggiunti al cluster.
    > -   Fino a quando non la [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet viene eseguito, il processo è completamente reversibile e i nodi di Windows Server 2012 R2 possono essere aggiunti a questo cluster e Windows Server 2016 nodi possono essere rimossi.  
    > -   Dopo il [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet viene eseguito, saranno disponibili nuove funzionalità.  

    1.  Utilizzando la UI gestione Cluster di Failover o [`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx)cmdlet, verificare che tutti i ruoli del cluster sono in esecuzione nel cluster, come previsto. Nell'esempio seguente, non è in uso di archiviazione disponibile, ma viene utilizzato CSV, di conseguenza, spazio di archiviazione disponibile Visualizza un **Offline** stato (vedi figura 18).  

        ![ScreenCap che mostra l'output del cmdlet Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Nella figura 18: Verifica che tutti i cluster gruppi (ruoli del cluster) siano in esecuzione utilizzando il [`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx)cmdlet**  

    2.  Verificare che tutti i nodi del cluster siano online e in esecuzione usando il [`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx)cmdlet.  
    3.  Eseguire il [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet - non devono essere restituiti errori (vedere la figura 19).  

        ![ScreenCap che mostra l'output del cmdlet Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **La figura 19: Aggiornare il livello di funzionalità di un cluster con PowerShell**  

    4.  Dopo il [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet viene eseguito, sono disponibili nuove funzionalità.  

4. Windows Server 2016 - recuperare gli aggiornamenti del cluster normale e backup:  

    1. Se aggiornamento compatibile con cluster precedentemente in esecuzione, riavviarlo tramite l'UI CAU oppure utilizzare il [`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx)cmdlet (vedi figura 20).  

        ![ScreenCap che mostra l'output del Enable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **Figura 20: Abilitare gli aggiornamenti del compatibile con Cluster ruolo utilizzando il [`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx)cmdlet**  

    2. Riprendere le operazioni di backup.  

5. Abilitare e usare le funzionalità di Windows Server 2016 in macchine virtuali Hyper-V.  

    1. Dopo il cluster è stato aggiornato a livello di funzionalità di Windows Server 2016, molti carichi di lavoro come macchine virtuali Hyper-V disporrà nuove funzionalità. Per un elenco delle nuove funzionalità di Hyper-V. Vedere [migrazione e aggiornamento delle macchine virtuali](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. In ogni nodo host Hyper-V nel cluster, utilizzare il [`Get-VMHostSupportedVersion`](https://technet.microsoft.com/library/mt653838.aspx)cmdlet per visualizzare le versioni di configurazione macchina virtuale Hyper-V sono supportate dall'host.  

        ![ScreenCap che mostra l'output del cmdlet Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figura 21: Visualizzando le versioni di configurazione macchina virtuale Hyper-V supportate dall'host**  

   3.  In ogni nodo host Hyper-V nel cluster, è possibile aggiornare versioni di configurazione macchina virtuale Hyper-V, la pianificazione di una finestra di manutenzione breve con gli utenti, eseguire il backup, la disattivazione di macchine virtuali e l'esecuzione di [`Update-VMVersion`](https://technet.microsoft.com/library/mt484146.aspx)cmdlet (vedi figura 22). Verrà aggiornato la versione di macchina virtuale e abilitare le nuove funzionalità di Hyper-V, eliminando la necessità di aggiornamenti futuri di componenti di integrazione Hyper-V (IC). Questo cmdlet può essere eseguito dal nodo Hyper-V che ospita la macchina virtuale, o `-ComputerName`parametro può essere utilizzato per aggiornare la versione di macchina virtuale in modalità remota. In questo esempio, qui abbiamo aggiornare la versione di configurazione di VM1 da 5.0 a 7.0 per sfruttare i vantaggi di numerose nuove funzionalità di Hyper-V associata a questa versione di configurazione macchina virtuale, ad esempio i checkpoint di produzione (backup coerenti con l'applicazione) e file di configurazione macchina virtuale binario.  

        ![ScreenCap che mostra il cmdlet Update-VMVersion in azione](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
        **Figura 22: L'aggiornamento a una versione di macchina virtuale utilizzando il cmdlet PowerShell Update-VMVersion**  

4.  Pool di archiviazione possono essere aggiornati tramite il [Update-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/update-storagepool) cmdlet PowerShell - operazione non in linea.  

Anche se destinazione scenari di Cloud privato, in particolare Hyper-V e cluster di File Server di scalabilità orizzontale, che possono essere aggiornati senza tempi di inattività, il processo di aggiornamento in sequenza del Cluster del sistema operativo può essere utilizzato per qualsiasi ruolo del cluster.  

## <a name="restrictions--limitations"></a>Restrizioni / limitazioni  
- Questa caratteristica funziona solo per Windows Server 2012 R2 per solo le versioni di Windows Server 2016. Questa funzionalità è possibile aggiornare le versioni precedenti di Windows Server, ad esempio Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2016.  
- Ogni nodo di Windows Server 2016 deve essere riformattati/nuova installazione solo. "Sul posto" o "aggiornamento" tipo di installazione è sconsigliato.  
- Un nodo di Windows Server 2016 deve essere utilizzato per aggiungere Windows Server 2016 nodi al cluster.  
- La gestione di un cluster in modalità mista del sistema operativo, è sempre eseguire le attività di gestione da un nodo di livello superiore che esegue Windows Server 2016. I nodi di livello inferiore di Windows Server 2012 R2 è possono utilizzare strumenti dell'interfaccia utente o di gestione per Windows Server 2016.  
- Microsoft consiglia di spostarsi rapidamente il processo di aggiornamento del cluster perché alcune funzionalità di cluster non sono ottimizzati per la modalità mista del sistema operativo.  
- Evitare la creazione o il ridimensionamento di archiviazione in Windows Server 2016 nodi mentre il cluster è in esecuzione in modalità mista del sistema operativo a causa di possibili problemi di incompatibilità in caso di failover da un nodo di Windows Server 2016 ai nodi di Windows Server 2012 R2 di livello inferiore.  

## <a name="frequently-asked-questions"></a>Domande frequenti  
**Quanto tempo il cluster di failover sono eseguibili in modalità mista del sistema operativo?**  
    Microsoft consiglia di completare l'aggiornamento nelle quattro settimane. Esistono numerose ottimizzazioni in Windows Server 2016. È stata aggiornata correttamente Hyper-V e cluster di File Server di scalabilità orizzontale con tempi di inattività totale meno di quattro ore.  

**Si verrà porta questa funzionalità a Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008?**  
    Non abbiamo i piani per convertire questa caratteristica a versioni precedenti. Aggiornamento in sequenza del cluster del sistema operativo è la nostra visione per l'aggiornamento di Windows Server 2012 R2 cluster a Windows Server 2016 e versioni successive.  

**Il cluster di Windows Server 2012 R2 è necessario per tutti gli aggiornamenti software installati prima di avviare il processo di aggiornamento in sequenza del Cluster del sistema operativo?**  
    Sì, prima di iniziare il processo di aggiornamento in sequenza del Cluster del sistema operativo, verificare che tutti i nodi del cluster vengono aggiornati con gli aggiornamenti software più recenti.  

**È possibile eseguire il [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet mentre i nodi sono disattivati o in pausa?**  
    No. Tutti i nodi del cluster devono essere su e in appartenenza attiva per il [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)funzionamento del cmdlet.  

**Aggiornamento in sequenza del Cluster del sistema operativo funziona per qualsiasi carico di lavoro del cluster? Funziona per SQL Server?**  
    Sì, l'aggiornamento in sequenza del Cluster del sistema operativo funziona per qualsiasi carico di lavoro del cluster. Tuttavia, è solo zero-tempo di inattività per Hyper-V e cluster di File Server di scalabilità orizzontale. La maggior parte degli altri carichi di lavoro dei tempi di inattività (in genere un paio di minuti) quando sono failover e il failover, è necessaria almeno una volta durante il processo di aggiornamento in sequenza del Cluster del sistema operativo.  

**È possibile automatizzare questo processo con PowerShell?**  
    Sì, abbiamo progettato Cluster del sistema operativo aggiornamento in sequenza per essere automatizzate tramite PowerShell.  

**Per un cluster di grandi dimensione che con carico di lavoro aggiuntivo e la capacità di failover, è possibile aggiornare più nodi contemporaneamente?**  
    Sì. Quando viene rimosso un nodo dal cluster per eseguire l'aggiornamento del sistema operativo, il cluster avranno un nodo in meno per il failover, pertanto avrà una capacità di failover ridotta. Per i cluster di grandi dimensioni con sufficiente carico di lavoro e la capacità di failover, più nodi possono essere aggiornati contemporaneamente. È possibile aggiungere temporaneamente i nodi del cluster nel cluster per fornire maggiore carico di lavoro e capacità di failover durante il processo di aggiornamento in sequenza del Cluster del sistema operativo.  

**Cosa succede se si scopre di un problema nel cluster dopo [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)è stato eseguito correttamente?**  
    Se si have backup database del cluster con un backup dello stato del sistema prima dell'esecuzione [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx), deve essere in grado di eseguire un autorevoli ripristinare in un nodo cluster Windows Server 2012 R2 e ripristinare il database del cluster originale e la configurazione.  

**È possibile utilizzare l'aggiornamento sul posto per ogni nodo anziché eseguire la pulizia del sistema operativo installazione riformattando le unità del sistema?**  
    Non abbiamo favorire l'uso di aggiornamento sul posto di Windows Server, ma siamo consapevoli che funziona in alcuni casi in cui vengono utilizzati i driver predefiniti. Leggere attentamente tutti i messaggi di avviso visualizzato durante l'aggiornamento sul posto di un nodo del cluster.  

**Se si utilizza la replica Hyper-V per una macchina virtuale Hyper-V nel cluster Hyper-V, replica rimarranno invariata durante e dopo il processo di aggiornamento in sequenza del Cluster del sistema operativo?**  
    Sì, la replica Hyper-V rimane invariata durante e dopo il processo di aggiornamento in sequenza del Cluster del sistema operativo.  

**È utilizzare System Center 2016 Virtual Machine Manager (SCVMM) per automatizzare il processo di aggiornamento in sequenza del Cluster del sistema operativo?**  
    Sì, è possibile automatizzare il processo di aggiornamento in sequenza del Cluster del sistema operativo tramite VMM in System Center 2016.  

## <a name="see-also"></a>Vedere anche  
-   [Note sulla versione: Problemi importanti in Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [What's New in Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [What's New in Failover Clustering in Windows Server](whats-new-in-failover-clustering.md)  