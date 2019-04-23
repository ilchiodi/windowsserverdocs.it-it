---
title: Aggiornare un cluster di spazi di archiviazione diretta per Windows Server 2019
description: Come aggiornare un cluster di spazi di archiviazione diretta in Windows Server 2019 da entrambe, mantenendo le macchine virtuali in esecuzione o anche se è in esecuzione.
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 107ccfb5bf9ed3d71b076f3fa66a4f337380a607
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853782"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>Aggiornare un cluster di spazi di archiviazione diretta per Windows Server 2019

In questo argomento viene descritto come aggiornare un cluster di spazi di archiviazione diretta per Windows Server 2019. Esistono quattro possibili approcci all'aggiornamento di un cluster di spazi di archiviazione diretta da Windows Server 2016 in Server Windows 2019, usando il [processo di aggiornamento in sequenza del sistema operativo del cluster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md): due che coinvolgono mantenendo le macchine virtuali in esecuzione e due che coinvolgono l'arresto di tutte le macchine virtuali.
Ogni approccio presenta diversi vantaggi e svantaggi, se si desidera selezionare tale quella più adatta alle esigenze dell'organizzazione:

-   **L'aggiornamento sul posto mentre sono in esecuzione le macchine virtuali** in ogni server del cluster, questa opzione non comporta tempi di inattività della macchina virtuale, ma sarà necessario attendere il completamento dopo l'aggiornamento di ogni server dei processi di archiviazione (riparazione mirror).

-   **Installazione di pulizia del sistema operativo mentre sono in esecuzione le macchine virtuali** in ogni server del cluster, questa opzione non comporta tempi di inattività della macchina virtuale, ma sarà necessario attendere i processi di archiviazione (riparazione mirror) completare dopo che ogni server viene aggiornato e sarà necessario impostare backup di ogni server e tutti i le App e i ruoli nuovamente.

-   **L'aggiornamento sul posto mentre le macchine virtuali vengono arrestate** in ogni server del cluster, questa opzione comporta tempi di inattività della macchina virtuale, ma non è necessario attendere per la memoria processi (riparazione mirror), pertanto è più veloce.

-   **Pulizia-OS installa mentre le macchine virtuali vengono arrestate** in ogni server del cluster, questa opzione comporta tempi di inattività della macchina virtuale, ma non è necessario attendere l'archiviazione dei processi (riparazione mirror), pertanto è più veloce.

## <a name="prerequisites-and-limitations"></a>Prerequisiti e limitazioni

Prima di procedere con l'aggiornamento:

-   Verificare di avere i backup utilizzabili nel caso in cui si verificano problemi durante il processo di aggiornamento.

-   Verificare che il fornitore dell'hardware disponga di un BIOS, firmware e driver per i server che supportano Windows Server 2019.

Esistono alcune limitazioni con il processo di aggiornamento da tenere presenti:

-   Per abilitare spazi di archiviazione diretta con le build di Windows Server 2019 precedenti a 176693.292, i clienti potrebbe essere necessario contattare il supporto tecnico Microsoft per le chiavi del Registro di sistema che consentono la funzionalità spazi di archiviazione diretta e Software Defined Networking. Per altre informazioni, vedere l'articolo della Microsoft Knowledge Base [articolo 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking).

-   Quando si aggiorna un cluster con volumi ReFS, esistono alcune limitazioni:

-   L'aggiornamento è completamente supportato nei volumi ReFS, tuttavia, i volumi aggiornati non trarre vantaggio dalla ReFS miglioramenti in Windows Server 2019. Questi vantaggi, ad esempio un miglioramento delle prestazioni per la parità con accelerazione mirror, richiedono un volume ReFS 2019 di Windows Server appena creato. In altre parole, è necessario creare nuovi volumi che usano il `New-Volume` cmdlet o Server Manager. Ecco alcuni dei miglioramenti apportati a ReFS nuovi volumi potrebbe ottenere:

    -   **Bypass del log mappa**: un miglioramento delle prestazioni in ReFS solo valida per sistemi cluster (spazi di archiviazione diretta) che non si applica ai pool di archiviazione autonomo.

    -   **Compattazione** ai miglioramenti di efficienza in Windows Server 2019 specifiche per i volumi a più resilienti

-   Prima di aggiornare un cluster di server Windows Server 2016 spazi di archiviazione diretta, è consigliabile inserire il server in modalità manutenzione dell'archiviazione. Per altre informazioni, vedere la sezione 5120 eventi della [risolvere i problemi di spazi di archiviazione diretta](troubleshooting-storage-spaces.md#event-5120-with-statusiotimeout-c00000b5).
    Sebbene questo problema è stato risolto in Windows Server 2016, è consigliabile inserire ogni server di spazi di archiviazione diretta in modalità manutenzione dell'archiviazione durante l'aggiornamento come procedura consigliata.

-   Si verifica un problema noto con gli ambienti di rete definita dal Software che usano SET di opzioni. Questo problema riguarda migrazioni in tempo reale della macchina virtuale Hyper-V da Windows Server 2019 per Windows Server 2016 (migrazione in tempo reale a un sistema operativo precedente). Per garantire l'esito positivo migrazioni in tempo reale, è consigliabile modificare un'impostazione di rete della macchina virtuale nelle macchine virtuali che vengono trasmessi live-eseguire la migrazione da Windows Server 2019 per Windows Server 2016. Questo problema è risolto per Windows Server 2019 nel pacchetto di rollup di hotfix D 2019-01, noto anche come compilazione 17763.292. Per altre informazioni, vedere l'articolo della Microsoft Knowledge Base [articolo 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976).

A causa di problemi noti precedenti, alcuni clienti possono prendere in considerazione la creazione di un nuovo cluster di Windows Server 2019 e copia dei dati dal cluster precedente, anziché aggiornare i cluster di Windows Server 2016 usando uno dei quattro processi descritti di seguito.

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>Esegue un aggiornamento sul posto, mentre le macchine virtuali sono in esecuzione

Questa opzione non comporta tempi di inattività della macchina virtuale, ma sarà necessario attendere il completamento dopo l'aggiornamento di ogni server dei processi di archiviazione (riparazione mirror). Anche se i singoli server verrà riavviato in modo sequenziale durante il processo di aggiornamento, i server rimanenti nel cluster, nonché tutte le macchine virtuali, rimarranno in esecuzione.

1.  Controllare che tutti i server nel cluster sono installati gli ultimi aggiornamenti di Windows.  
    Per altre informazioni, vedi [Windows 10 e Windows Server 2016 aggiornamento cronologia](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
    Come minimo, installare Microsoft Knowledge Base [articolo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 febbraio 2019). Il numero di build (vedere `ver` comando) deve essere 14393.2828 o versione successiva.

2.  Se si utilizza Software Defined Networking con SET di opzioni, aprire una sessione di PowerShell con privilegi elevata ed eseguire il comando seguente per disabilitare i controlli di verifica di migrazione in tempo reale della macchina virtuale in tutte le VM nel cluster:

    ```PowerShell
    Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
    Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
    ```

1.  Seguire i passaggi seguenti nel server del cluster alla volta:

    1.  Utilizzare la migrazione in tempo reale di VM Hyper-V per spostare macchine virtuali in esecuzione fuori del server che si sta per eseguire l'aggiornamento.

    2.  Sospendere il server del cluster usando il comando PowerShell seguente, si noti che alcuni gruppi interni sono nascosti.  
    È consigliabile prestare attenzione in questo passaggio, ovvero se già non è stato in tempo reale la migrazione di VM fuori del server di questo cmdlet eseguirà che, pertanto si può ignorare il passaggio precedente se si preferisce.

         ```PowerShell
        Suspend-ClusterNode -Drain
        ```

    3.  Collocare il server in modalità manutenzione dell'archiviazione eseguendo i comandi PowerShell seguenti:

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Enable-StorageMaintenanceMode
        ```

    2.  Eseguire il cmdlet seguente per verificare che il **OperationalStatus** valore è **In modalità manutenzione**:

        ```PowerShell
        Get-PhysicalDisk
        ```

    3.  Eseguire un'installazione dell'aggiornamento di Windows Server 2019 sul server eseguendo **setup.exe** e utilizzando l'opzione "Mantieni l'App e i file personali".  
    Al termine dell'installazione, il server rimane nel cluster e il cluster del servizio viene avviato automaticamente.

    4.  Verificare che il server appena aggiornato con gli ultimi aggiornamenti di Windows Server 2019.  
    Per altre informazioni, vedi [Windows 10 e Windows Server 2019 aggiornare la cronologia](https://support.microsoft.com/help/4464619/windows-10-update-history).
    Il numero di build (vedere `ver` comando) deve essere 17763.292 o versione successiva.

    5.  Rimuovere il server dalla modalità manutenzione dell'archiviazione tramite il comando PowerShell seguente:

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Disable-StorageMaintenanceMode
        ```

    1.  Riprendere il server usando il comando PowerShell seguente:

        ```PowerShell
        Resume-ClusterNode
        ```

    1.  Attendere che i processi di ripristino di archiviazione completare e per tutti i dischi tornare a uno stato integro.  
    Ciò potrebbe richiedere molto tempo a seconda del numero di macchine virtuali in esecuzione durante l'aggiornamento del server. Ecco i comandi da eseguire:

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

2.  Aggiornare il server successivo nel cluster.

3.  Dopo aver aggiornati tutti i server per Windows Server 2019, usare il cmdlet di PowerShell seguente per aggiornare il livello funzionale del cluster.
    
    ```PowerShell
    Update-ClusterFunctionalLevel
    ```

    >   [!NOTE]
    >   È consigliabile aggiornare il livello funzionale del cluster appena possibile, anche se tecnicamente è necessario eseguire questa operazione fino a quattro settimane.

1.  Dopo aver aggiornato il livello funzionale del cluster, usare il cmdlet seguente per aggiornare il pool di archiviazione.  
    A questo punto, i nuovi cmdlet, ad esempio `Get-ClusterPerf` sarà completamente operativa in qualsiasi server nel cluster.

    ```PowerShell
    Update-StoragePool
    ```

2.  Aggiornare facoltativamente i livelli di configurazione della macchina virtuale, arrestare ogni macchina virtuale, usando il `Update-VMVersion` cmdlet e quindi avviarlo di nuovo le macchine virtuali.

3.  Se si utilizza Software Defined Networking con SET di opzioni e disabilitato i controlli di migrazione in tempo reale della macchina virtuale come descritto al precedente, usano il cmdlet seguente per abilitare nuovamente i controlli di verifica in tempo reale della macchina virtuale:

    ```PowerShell
    Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
    Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
    ```

1.  Verificare che il cluster aggiornato funziona come previsto.  
    Ruoli devono effettuare il failover in modo corretto e se la migrazione in tempo reale della macchina virtuale viene usata nel cluster, le macchine virtuali correttamente in tempo reale devono eseguire la migrazione.

2.  Convalidare il cluster tramite l'esecuzione di convalida del Cluster (`Test-Cluster`) ed esaminando i report di convalida cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Eseguire un'installazione pulita del sistema operativo, mentre le macchine virtuali sono in esecuzione

Questa opzione non comporta tempi di inattività della macchina virtuale, ma sarà necessario attendere il completamento dopo l'aggiornamento di ogni server dei processi di archiviazione (riparazione mirror). Anche se i singoli server verrà riavviato in modo sequenziale durante il processo di aggiornamento, i server rimanenti nel cluster, nonché tutte le macchine virtuali, rimarranno in esecuzione.

1.  Verificare che tutti i server del cluster eseguano gli aggiornamenti più recenti.  
    Per altre informazioni, vedi [Windows 10 e Windows Server 2016 aggiornamento cronologia](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
    Come minimo, installare Microsoft Knowledge Base [articolo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 febbraio 2019). Il numero di build (vedere `ver` comando) deve essere 14393.2828 o versione successiva.

2.  Se si utilizza Software Defined Networking con SET di opzioni, aprire una sessione di PowerShell con privilegi elevata ed eseguire il comando seguente per disabilitare i controlli di verifica di migrazione in tempo reale della macchina virtuale in tutte le VM nel cluster:

    ```PowerShell
    Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
    Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
    ```

3.  Seguire i passaggi seguenti nel server del cluster alla volta:

    1.  Utilizzare la migrazione in tempo reale di VM Hyper-V per spostare macchine virtuali in esecuzione fuori del server che si sta per eseguire l'aggiornamento.

    2.  Sospendere il server del cluster usando il comando PowerShell seguente, si noti che alcuni gruppi interni sono nascosti.  
    È consigliabile prestare attenzione in questo passaggio, ovvero se già non è stato in tempo reale la migrazione di VM fuori del server di questo cmdlet eseguirà che, pertanto si può ignorare il passaggio precedente se si preferisce.

         ```PowerShell
        Suspend-ClusterNode -Drain
        ```

    3.  Collocare il server in modalità manutenzione dell'archiviazione eseguendo i comandi PowerShell seguenti:

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Enable-StorageMaintenanceMode
        ```

    4.  Eseguire il cmdlet seguente per verificare che il **OperationalStatus** valore è **In modalità manutenzione**:

        ```PowerShell
        Get-PhysicalDisk
        ```

    3.  Rimuovere il server dal cluster eseguendo il comando PowerShell seguente:  

        ```PowerShell
        Remove-ClusterNode <ServerName>
        ```

    4.  Eseguire un'installazione pulita di Windows Server 2019 sul server: formattazione dell'unità di sistema, eseguire **setup.exe** e usare la "Nothing" opzione.  
    È possibile configurare l'identità del server, ruoli, funzionalità e le applicazioni al termine dell'installazione e il riavvio del server.

    5.  Installare il ruolo Hyper-V e la funzionalità Clustering di Failover nel server (è possibile usare il `Install-WindowsFeature` cmdlet).

    6.  Installare i driver più recenti di archiviazione e rete per l'hardware che sono approvati dal produttore del server per l'uso con spazi di archiviazione diretta.

    7.  Verificare che il server appena aggiornato con gli ultimi aggiornamenti di Windows Server 2019.  
    Per altre informazioni, vedi [Windows 10 e Windows Server 2019 aggiornare la cronologia](https://support.microsoft.com/help/4464619/windows-10-update-history).
    Il numero di build (vedere `ver` comando) deve essere 17763.292 o versione successiva.

    8.  Aggiungere nuovamente il server al cluster usando il comando PowerShell seguente:

        ```PowerShell
        Add-ClusterNode
        ```

    9.  Rimuovere il server dalla modalità manutenzione dell'archiviazione con i comandi di PowerShell seguenti:

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Disable-StorageMaintenanceMode
        ```

    10.  Attendere che i processi di ripristino di archiviazione completare e per tutti i dischi tornare a uno stato integro.  
    Ciò potrebbe richiedere molto tempo a seconda del numero di macchine virtuali in esecuzione durante l'aggiornamento del server. Ecco i comandi da eseguire:

         ```PowerShell
         Get-StorageJob
         Get-VirtualDisk
         ```

4.  Aggiornare il server successivo nel cluster.

5.  Dopo aver aggiornati tutti i server per Windows Server 2019, usare il cmdlet di PowerShell seguente per aggiornare il livello funzionale del cluster.
    
    ```PowerShell
    Update-ClusterFunctionalLevel
    ```

    >   [!NOTE]
    >   È consigliabile aggiornare il livello funzionale del cluster appena possibile, anche se tecnicamente è necessario eseguire questa operazione fino a quattro settimane.

6.  Dopo aver aggiornato il livello funzionale del cluster, usare il cmdlet seguente per aggiornare il pool di archiviazione.  
    A questo punto, i nuovi cmdlet, ad esempio `Get-ClusterPerf` sarà completamente operativa in qualsiasi server nel cluster.

    ```PowerShell
    Update-StoragePool
    ```

7.  Facoltativamente l'aggiornamento di livelli di configurazione della macchina virtuale da arrestare ogni macchina virtuale e tramite la `Update-VMVersion` cmdlet e quindi avviarlo di nuovo le macchine virtuali.

8.  Se si utilizza Software Defined Networking con SET di opzioni e disabilitato i controlli di migrazione in tempo reale della macchina virtuale come descritto al precedente, usano il cmdlet seguente per abilitare nuovamente i controlli di verifica in tempo reale della macchina virtuale:

    ```PowerShell
    Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
    Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
    ```

9.  Verificare che il cluster aggiornato funziona come previsto.  
    Ruoli devono effettuare il failover in modo corretto e se la migrazione in tempo reale della macchina virtuale viene usata nel cluster, le macchine virtuali correttamente in tempo reale devono eseguire la migrazione.

10.  Convalidare il cluster tramite l'esecuzione di convalida del Cluster (`Test-Cluster`) ed esaminando i report di convalida cluster.

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>Esegue un aggiornamento sul posto, mentre le macchine virtuali vengono arrestate

Questa opzione comporta tempi di inattività della macchina virtuale, ma può richiedere meno tempo rispetto a quando è mantenuto le macchine virtuali in esecuzione durante l'aggiornamento perché non è necessario attendere il completamento dopo l'aggiornamento di ogni server dei processi di archiviazione (riparazione mirror). Anche se i singoli server verrà riavviato in modo sequenziale durante il processo di aggiornamento, i server rimanenti nel cluster rimangano in esecuzione.

1.  Verificare che tutti i server del cluster eseguano gli aggiornamenti più recenti.  
    Per altre informazioni, vedi [Windows 10 e Windows Server 2016 aggiornamento cronologia](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
    Come minimo, installare Microsoft Knowledge Base [articolo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 febbraio 2019). Il numero di build (vedere `ver` comando) deve essere 14393.2828 o versione successiva.

2.  Arrestare le macchine virtuali in esecuzione nel cluster.

3.  Seguire i passaggi seguenti nel server del cluster alla volta:

    1.  Sospendere il server del cluster usando il comando PowerShell seguente, si noti che alcuni gruppi interni sono nascosti.  
    È consigliabile prestare attenzione in questo passaggio.

         ```PowerShell
        Suspend-ClusterNode -Drain
        ```

    2.  Collocare il server in modalità manutenzione dell'archiviazione eseguendo i comandi PowerShell seguenti:

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Enable-StorageMaintenanceMode
        ```

    3.  Eseguire il cmdlet seguente per verificare che il **OperationalStatus** valore è **In modalità manutenzione**:

        ```PowerShell
        Get-PhysicalDisk
        ```

    4.  Eseguire un'installazione dell'aggiornamento di Windows Server 2019 sul server eseguendo **setup.exe** e utilizzando l'opzione "Mantieni l'App e i file personali".  
    Al termine dell'installazione, il server rimane nel cluster e il cluster del servizio viene avviato automaticamente.

    5.  Verificare che il server appena aggiornato con gli ultimi aggiornamenti di Windows Server 2019.  
    Per altre informazioni, vedi [Windows 10 e Windows Server 2019 aggiornare la cronologia](https://support.microsoft.com/help/4464619/windows-10-update-history).
    Il numero di build (vedere `ver` comando) deve essere 17763.292 o versione successiva.

    6.  Rimuovere il server dalla modalità manutenzione dell'archiviazione con i comandi di PowerShell seguenti:

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Disable-StorageMaintenanceMode
        ```

    7.  Riprendere il server usando il comando PowerShell seguente:

        ```PowerShell
        Resume-ClusterNode
        ```

    8.  Attendere che i processi di ripristino di archiviazione completare e per tutti i dischi tornare a uno stato integro.  
    Questo dovrebbe essere relativamente veloce, poiché le macchine virtuali non sono in esecuzione. Ecco i comandi da eseguire:

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4.  Aggiornare il server successivo nel cluster.
5.  Dopo aver aggiornati tutti i server per Windows Server 2019, usare il cmdlet di PowerShell seguente per aggiornare il livello funzionale del cluster.
    
    ```PowerShell
    Update-ClusterFunctionalLevel
    ```

    >   [!NOTE]
    >   È consigliabile aggiornare il livello funzionale del cluster appena possibile, anche se tecnicamente è necessario eseguire questa operazione fino a quattro settimane.

6.  Dopo aver aggiornato il livello funzionale del cluster, usare il cmdlet seguente per aggiornare il pool di archiviazione.  
    A questo punto, i nuovi cmdlet, ad esempio `Get-ClusterPerf` sarà completamente operativa in qualsiasi server nel cluster.

    ```PowerShell
    Update-StoragePool
    ```

7.  Avviare le macchine virtuali nel cluster e verificare che il corretto funzionamento.

8.  Facoltativamente l'aggiornamento di livelli di configurazione della macchina virtuale da arrestare ogni macchina virtuale e tramite la `Update-VMVersion` cmdlet e quindi avviarlo di nuovo le macchine virtuali.

9.  Verificare che il cluster aggiornato funziona come previsto.  
    Ruoli devono effettuare il failover in modo corretto e se la migrazione in tempo reale della macchina virtuale viene usata nel cluster, le macchine virtuali correttamente in tempo reale devono eseguire la migrazione.

10.  Convalidare il cluster tramite l'esecuzione di convalida del Cluster (`Test-Cluster`) ed esaminando i report di convalida cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>L'esecuzione di un'installazione pulita del sistema operativo durante le macchine virtuali vengono arrestate

Questa opzione comporta tempi di inattività della macchina virtuale, ma può richiedere meno tempo rispetto a quando è mantenuto le macchine virtuali in esecuzione durante l'aggiornamento perché non è necessario attendere il completamento dopo l'aggiornamento di ogni server dei processi di archiviazione (riparazione mirror). Anche se i singoli server verrà riavviato in modo sequenziale durante il processo di aggiornamento, i server rimanenti nel cluster rimangano in esecuzione.

1.  Verificare che tutti i server del cluster eseguano gli aggiornamenti più recenti.  
    Per altre informazioni, vedi [Windows 10 e Windows Server 2016 aggiornamento cronologia](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
    Come minimo, installare Microsoft Knowledge Base [articolo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 febbraio 2019). Il numero di build (vedere `ver` comando) deve essere 14393.2828 o versione successiva.

2.  Arrestare le macchine virtuali in esecuzione nel cluster.

3.  Seguire i passaggi seguenti nel server del cluster alla volta:

    2.  Sospendere il server del cluster usando il comando PowerShell seguente, si noti che alcuni gruppi interni sono nascosti.  
    È consigliabile prestare attenzione in questo passaggio.

         ```PowerShell
        Suspend-ClusterNode -Drain
        ```

    3.  Collocare il server in modalità manutenzione dell'archiviazione eseguendo i comandi PowerShell seguenti:

        ```PowerShell
        Get-StorageFaultDomain -type StorageScaleUnit | 
        Where FriendlyName -Eq <ServerName> | 
        Enable-StorageMaintenanceMode
        ```

    4.  Eseguire il cmdlet seguente per verificare che il **OperationalStatus** valore è **In modalità manutenzione**:

        ```PowerShell
        Get-PhysicalDisk
        ```

    5.  Rimuovere il server dal cluster eseguendo il comando PowerShell seguente:  
    
        ```PowerShell
        Remove-ClusterNode <ServerName>
        ```

    6.  Eseguire un'installazione pulita di Windows Server 2019 sul server: formattazione dell'unità di sistema, eseguire **setup.exe** e usare la "Nothing" opzione.  
    È possibile configurare l'identità del server, ruoli, funzionalità e le applicazioni al termine dell'installazione e il riavvio del server.

    7.  Installare il ruolo Hyper-V e la funzionalità Clustering di Failover nel server (è possibile usare il `Install-WindowsFeature` cmdlet).

    8.  Installare i driver più recenti di archiviazione e rete per l'hardware che sono approvati dal produttore del server per l'uso con spazi di archiviazione diretta.

    9.  Verificare che il server appena aggiornato con gli ultimi aggiornamenti di Windows Server 2019.  
    Per altre informazioni, vedi [Windows 10 e Windows Server 2019 aggiornare la cronologia](https://support.microsoft.com/help/4464619/windows-10-update-history).
    Il numero di build (vedere `ver` comando) deve essere 17763.292 o versione successiva.

    10.  Aggiungere nuovamente il server al cluster usando il comando PowerShell seguente:

         ```PowerShell
         Add-ClusterNode
         ```

    11.  Rimuovere il server dalla modalità manutenzione dell'archiviazione tramite il comando PowerShell seguente:

         ```PowerShell
         Get-StorageFaultDomain -type StorageScaleUnit | 
         Where FriendlyName -Eq <ServerName> | 
         Disable-StorageMaintenanceMode
         ```

    10.  Attendere che i processi di ripristino di archiviazione completare e per tutti i dischi tornare a uno stato integro.  
    Ciò potrebbe richiedere molto tempo a seconda del numero di macchine virtuali in esecuzione durante l'aggiornamento del server. Ecco i comandi da eseguire:

         ```PowerShell
         Get-StorageJob
         Get-VirtualDisk
         ```

4.  Aggiornare il server successivo nel cluster.

5.  Dopo aver aggiornati tutti i server per Windows Server 2019, usare il cmdlet di PowerShell seguente per aggiornare il livello funzionale del cluster.
    
    ```PowerShell
    Update-ClusterFunctionalLevel
    ```

    >   [!NOTE]
    >   È consigliabile aggiornare il livello funzionale del cluster appena possibile, anche se tecnicamente è necessario eseguire questa operazione fino a quattro settimane.

6.  Dopo aver aggiornato il livello funzionale del cluster, usare il cmdlet seguente per aggiornare il pool di archiviazione.  
    A questo punto, i nuovi cmdlet, ad esempio `Get-ClusterPerf` sarà completamente operativa in qualsiasi server nel cluster.

    ```PowerShell
    Update-StoragePool
    ```

7.  Avviare le macchine virtuali nel cluster e verificare che il corretto funzionamento.

8.  Facoltativamente l'aggiornamento di livelli di configurazione della macchina virtuale da arrestare ogni macchina virtuale e tramite la `Update-VMVersion` cmdlet e quindi avviarlo di nuovo le macchine virtuali.

9.  Verificare che il cluster aggiornato funziona come previsto.  
    Ruoli devono effettuare il failover in modo corretto e se la migrazione in tempo reale della macchina virtuale viene usata nel cluster, le macchine virtuali correttamente in tempo reale devono eseguire la migrazione.

10.  Convalidare il cluster tramite l'esecuzione di convalida del Cluster (`Test-Cluster`) ed esaminando i report di convalida cluster.
