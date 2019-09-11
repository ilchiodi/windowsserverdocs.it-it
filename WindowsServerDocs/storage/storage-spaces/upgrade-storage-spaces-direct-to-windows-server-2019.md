---
title: Aggiornare un cluster di Spazi di archiviazione diretta a Windows Server 2019
description: Come aggiornare un cluster di Spazi di archiviazione diretta a Windows Server 2019, mantenendo le macchine virtuali in esecuzione o mentre sono arrestate.
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 9966ee0fd3c0a2d1df0180bf177dec03343efc14
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867185"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>Aggiornare un cluster di Spazi di archiviazione diretta a Windows Server 2019

Questo argomento descrive come aggiornare un cluster di Spazi di archiviazione diretta a Windows Server 2019. Sono disponibili quattro approcci per l'aggiornamento di un cluster di Spazi di archiviazione diretta da Windows Server 2016 a Windows Server 2019, tramite il [processo di aggiornamento in sequenza del sistema operativo del cluster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) , due che comportano l'esecuzione delle macchine virtuali e due che comportano l'arresto di tutte le macchine virtuali. Ogni approccio presenta punti di forza e punti di debolezza diversi, quindi selezionarne uno più adatto alle esigenze dell'organizzazione:

- **Aggiornamento sul posto mentre le VM sono in esecuzione** in ogni server del cluster: questa opzione non comporta tempi di inattività delle VM, ma è necessario attendere il completamento dei processi di archiviazione (ripristino mirror) dopo l'aggiornamento di ogni server.

- **Installazione pulita del sistema operativo mentre le macchine virtuali sono in esecuzione** in ogni server del cluster. questa opzione non comporta tempi di inattività delle macchine virtuali, ma è necessario attendere il completamento dei processi di archiviazione (ripristino mirror) dopo l'aggiornamento di ogni server. sarà quindi necessario configurare tutti i server e tutti i relativi app e ruoli.

- **Aggiornamento sul posto mentre le macchine virtuali vengono interrotte** in ogni server del cluster: questa opzione comporta tempi di inattività delle macchine virtuali, ma non è necessario attendere i processi di archiviazione (ripristino mirror), quindi è più veloce.

- **Clean-OS Install mentre le macchine virtuali vengono interrotte** in ogni server del cluster: questa opzione comporta tempi di inattività delle macchine virtuali, ma non è necessario attendere i processi di archiviazione (ripristino mirror), in modo che sia più veloce.

## <a name="prerequisites-and-limitations"></a>Prerequisiti e limitazioni

Prima di procedere con un aggiornamento:

- Verificare di disporre di backup utilizzabili in caso di problemi durante il processo di aggiornamento.

- Verificare che il fornitore dell'hardware disponga di un BIOS, firmware e driver per i server che supporteranno in Windows Server 2019.

È necessario tenere presenti alcune limitazioni del processo di aggiornamento:

- Per abilitare Spazi di archiviazione diretta con Windows Server 2019 build precedenti a 176693,292, i clienti potrebbero dover contattare il supporto tecnico Microsoft per le chiavi del registro di sistema che consentono la funzionalità di Spazi di archiviazione diretta e Software Defined Networking. Per ulteriori informazioni, vedere l' [articolo 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking)della Microsoft Knowledge base.

- Quando si aggiorna un cluster con volumi ReFS, esistono alcune limitazioni:

- L'aggiornamento è completamente supportato nei volumi ReFS. Tuttavia, i volumi aggiornati non trarranno vantaggio dai miglioramenti apportati a ReFS in Windows Server 2019. Questi vantaggi, ad esempio un miglioramento delle prestazioni per la parità con accelerazione speculare, richiedono un volume Windows Server 2019 ReFS appena creato. In altre parole, è necessario creare nuovi volumi usando il `New-Volume` cmdlet o Server Manager. Ecco alcuni dei miglioramenti apportati da ReFS i nuovi volumi:

    - **Mapping log-bypass**: miglioramento delle prestazioni in refs che si applica solo ai sistemi cluster (spazi di archiviazione diretta) e non si applica ai pool di archiviazione autonomi.

    - Miglioramenti dell'efficienza della **compattazione** in Windows Server 2019 specifici per i volumi multiresilienza.

- Prima di aggiornare un server cluster Spazi di archiviazione diretta Windows Server 2016, è consigliabile inserire il server in modalità manutenzione archiviazione. Per ulteriori informazioni, vedere la sezione dell'evento 5120 di [Troubleshoot spazi di archiviazione diretta](troubleshooting-storage-spaces.md). Sebbene il problema sia stato risolto in Windows Server 2016, è consigliabile inserire ogni server Spazi di archiviazione diretta in modalità manutenzione archiviazione durante l'aggiornamento come procedura consigliata.

- Si è verificato un problema noto con gli ambienti software defined networking che utilizzano opzioni SET. Questo problema riguarda le migrazioni in tempo reale di macchine virtuali Hyper-V da Windows Server 2019 a Windows Server 2016 (migrazione in tempo reale a un sistema operativo precedente). Per garantire una corretta migrazione in tempo reale, si consiglia di modificare un'impostazione di rete VM in macchine virtuali di cui viene eseguita la migrazione in tempo reale da Windows Server 2019 a Windows Server 2016. Questo problema è stato risolto per Windows Server 2019 nel pacchetto di rollup degli hotfix 2019-01D, noto anche come build 17763,292. Per ulteriori informazioni, vedere l' [articolo 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976)della Microsoft Knowledge base.

A causa dei problemi noti precedenti, alcuni clienti possono prendere in considerazione la creazione di un nuovo cluster Windows Server 2019 e la copia dei dati dal cluster precedente, invece di aggiornare i cluster Windows Server 2016 usando uno dei quattro processi descritti di seguito.

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>Esecuzione di un aggiornamento sul posto mentre le macchine virtuali sono in esecuzione

Questa opzione non comporta tempi di inattività delle macchine virtuali, ma è necessario attendere il completamento dei processi di archiviazione (ripristino mirror) dopo l'aggiornamento di ogni server. Anche se i singoli server verranno riavviati in modo sequenziale durante il processo di aggiornamento, i server rimanenti nel cluster e tutte le VM rimarranno in esecuzione.

1. Verificare che tutti i server nel cluster abbiano installato gli aggiornamenti di Windows più recenti. Per ulteriori informazioni, vedere la [cronologia degli aggiornamenti di Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Installare come minimo l' [articolo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) della Microsoft Knowledge base (19 febbraio 2019). Il numero di build ( `ver` vedere il comando) deve essere 14393,2828 o superiore.

2. Se si usa software defined networking con i commutatori impostati, aprire una sessione di PowerShell con privilegi elevati ed eseguire il comando seguente per disabilitare i controlli di verifica della migrazione in tempo reale della VM in tutte le macchine virtuali del cluster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Eseguire i passaggi seguenti in un server cluster alla volta:

   1. Usare la migrazione in tempo reale della macchina virtuale Hyper-V per spostare le macchine virtuali in esecuzione dal server che si sta per aggiornare.

   2. Sospendere il server del cluster usando il comando di PowerShell seguente: si noti che alcuni gruppi interni sono nascosti. Si consiglia di eseguire questa operazione con cautela: se non è già stata eseguita la migrazione in tempo reale delle macchine virtuali dal server, questo cmdlet eseguirà questa operazione per l'utente, pertanto è possibile ignorare il passaggio precedente, se si preferisce.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Posizionare il server in modalità manutenzione archiviazione eseguendo i comandi di PowerShell seguenti:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Eseguire il cmdlet seguente per verificare che il valore di **OperationalStatus** sia **in modalità di manutenzione**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. Eseguire un'installazione di aggiornamento di Windows Server 2019 sul server eseguendo **Setup. exe** e usando l'opzione "Mantieni i file e le app personali". Al termine dell'installazione, il server rimane nel cluster e il servizio cluster viene avviato automaticamente.

   6. Verificare che il server appena aggiornato disponga degli aggiornamenti più recenti di Windows Server 2019. Per ulteriori informazioni, vedere la [cronologia degli aggiornamenti di Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). Il numero di build ( `ver` vedere il comando) deve essere 17763,292 o superiore.

   7. Rimuovere il server dalla modalità di manutenzione dell'archiviazione usando il comando PowerShell seguente:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. Riprendere il server usando il comando PowerShell seguente:

       ```PowerShell
       Resume-ClusterNode
       ```

   9. Attendere che i processi di ripristino dell'archiviazione vengano completati e che tutti i dischi tornino a uno stato integro. Questa operazione potrebbe richiedere molto tempo a seconda del numero di macchine virtuali in esecuzione durante l'aggiornamento del server. Ecco i comandi da eseguire:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Aggiornare il server successivo nel cluster.

5. Dopo che tutti i server sono stati aggiornati a Windows Server 2019, usare il cmdlet di PowerShell seguente per aggiornare il livello di funzionalità del cluster.

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > È consigliabile aggiornare il livello di funzionalità del cluster il prima possibile, sebbene tecnicamente siano disponibili fino a quattro settimane.

6. Al termine dell'aggiornamento del livello di funzionalità del cluster, usare il cmdlet seguente per aggiornare il pool di archiviazione. A questo punto, i nuovi cmdlet, ad `Get-ClusterPerf` esempio, saranno completamente operativi in tutti i server del cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Facoltativamente, aggiornare i livelli di configurazione della macchina virtuale arrestando `Update-VMVersion` ogni macchina virtuale, usando il cmdlet e quindi riavviando le macchine virtuali.

8. Se si usa software defined networking con le opzioni SET e i controlli di migrazione in tempo reale della macchina virtuale disabilitati come indicato in precedenza, usare il cmdlet seguente per riabilitare i controlli di verifica Live della macchina virtuale:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. Verificare che il cluster aggiornato funzioni come previsto. Il failover dei ruoli deve essere eseguito correttamente e se nel cluster viene usata la migrazione in tempo reale della macchina virtuale, le VM devono eseguire la migrazione in tempo reale.

10. Convalidare il cluster eseguendo la convalida`Test-Cluster`del cluster () ed esaminando il report di convalida del cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Esecuzione di un'installazione pulita del sistema operativo mentre le macchine virtuali sono in esecuzione

Questa opzione non comporta tempi di inattività delle macchine virtuali, ma è necessario attendere il completamento dei processi di archiviazione (ripristino mirror) dopo l'aggiornamento di ogni server. Anche se i singoli server verranno riavviati in modo sequenziale durante il processo di aggiornamento, i server rimanenti nel cluster e tutte le VM rimarranno in esecuzione.

1. Verificare che tutti i server del cluster eseguano gli aggiornamenti più recenti. Per ulteriori informazioni, vedere la [cronologia degli aggiornamenti di Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Installare come minimo l' [articolo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) della Microsoft Knowledge base (19 febbraio 2019). Il numero di build ( `ver` vedere il comando) deve essere 14393,2828 o superiore.

2. Se si usa software defined networking con i commutatori impostati, aprire una sessione di PowerShell con privilegi elevati ed eseguire il comando seguente per disabilitare i controlli di verifica della migrazione in tempo reale della VM in tutte le macchine virtuali del cluster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Eseguire i passaggi seguenti in un server cluster alla volta:

   1. Usare la migrazione in tempo reale della macchina virtuale Hyper-V per spostare le macchine virtuali in esecuzione dal server che si sta per aggiornare.

   2. Sospendere il server del cluster usando il comando di PowerShell seguente: si noti che alcuni gruppi interni sono nascosti. Si consiglia di eseguire questa operazione con cautela: se non è già stata eseguita la migrazione in tempo reale delle macchine virtuali dal server, questo cmdlet eseguirà questa operazione per l'utente, pertanto è possibile ignorare il passaggio precedente, se si preferisce.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Posizionare il server in modalità manutenzione archiviazione eseguendo i comandi di PowerShell seguenti:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Eseguire il cmdlet seguente per verificare che il valore di **OperationalStatus** sia **in modalità di manutenzione**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  Rimuovere il server dal cluster eseguendo il comando PowerShell seguente:  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. Eseguire un'installazione pulita di Windows Server 2019 nel server: formattare l'unità di sistema, eseguire **Setup. exe** e usare l'opzione "Nothing". È necessario configurare l'identità del server, i ruoli, le funzionalità e le applicazioni dopo il completamento dell'installazione e il riavvio del server.

   5. Installare il ruolo Hyper-V e la funzionalità clustering di failover nel server (è possibile usare il `Install-WindowsFeature` cmdlet).

   6. Installare i driver di archiviazione e di rete più recenti per l'hardware approvati dal produttore del server per l'uso con Spazi di archiviazione diretta.

   7. Verificare che il server appena aggiornato disponga degli aggiornamenti più recenti di Windows Server 2019. Per ulteriori informazioni, vedere la [cronologia degli aggiornamenti di Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). Il numero di build ( `ver` vedere il comando) deve essere 17763,292 o superiore.

   8. Aggiungere nuovamente il server al cluster usando il comando PowerShell seguente:

       ```PowerShell
       Add-ClusterNode
       ```

   9. Rimuovere il server dalla modalità di manutenzione dell'archiviazione usando i comandi di PowerShell seguenti:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. Attendere che i processi di ripristino dell'archiviazione vengano completati e che tutti i dischi tornino a uno stato integro. Questa operazione potrebbe richiedere molto tempo a seconda del numero di macchine virtuali in esecuzione durante l'aggiornamento del server. Ecco i comandi da eseguire:

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. Aggiornare il server successivo nel cluster.

5. Dopo che tutti i server sono stati aggiornati a Windows Server 2019, usare il cmdlet di PowerShell seguente per aggiornare il livello di funzionalità del cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > È consigliabile aggiornare il livello di funzionalità del cluster il prima possibile, sebbene tecnicamente siano disponibili fino a quattro settimane.

6. Al termine dell'aggiornamento del livello di funzionalità del cluster, usare il cmdlet seguente per aggiornare il pool di archiviazione. A questo punto, i nuovi cmdlet, ad `Get-ClusterPerf` esempio, saranno completamente operativi in tutti i server del cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Facoltativamente, aggiornare i livelli di configurazione della macchina virtuale arrestando `Update-VMVersion` ogni macchina virtuale e usando il cmdlet, quindi riavviando le macchine virtuali.

8. Se si usa software defined networking con le opzioni SET e i controlli di migrazione in tempo reale della macchina virtuale disabilitati come indicato in precedenza, usare il cmdlet seguente per riabilitare i controlli di verifica Live della macchina virtuale:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. Verificare che il cluster aggiornato funzioni come previsto. Il failover dei ruoli deve essere eseguito correttamente e se nel cluster viene usata la migrazione in tempo reale della macchina virtuale, le VM devono eseguire la migrazione in tempo reale.

10. Convalidare il cluster eseguendo la convalida`Test-Cluster`del cluster () ed esaminando il report di convalida del cluster.

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>Esecuzione di un aggiornamento sul posto mentre le macchine virtuali vengono arrestate

Questa opzione comporta tempi di inattività delle VM, ma può richiedere meno tempo rispetto a quando le VM sono in esecuzione durante l'aggiornamento perché non è necessario attendere il completamento dei processi di archiviazione (ripristino mirror) dopo l'aggiornamento di ogni server. Sebbene i singoli server vengano riavviati in modo sequenziale durante il processo di aggiornamento, i restanti server del cluster rimarranno in esecuzione.

1. Verificare che tutti i server del cluster eseguano gli aggiornamenti più recenti. Per ulteriori informazioni, vedere la [cronologia degli aggiornamenti di Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Installare come minimo l' [articolo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) della Microsoft Knowledge base (19 febbraio 2019). Il numero di build ( `ver` vedere il comando) deve essere 14393,2828 o superiore.

2. Arrestare le macchine virtuali in esecuzione nel cluster.

3. Eseguire i passaggi seguenti in un server cluster alla volta:

   1. Sospendere il server del cluster usando il comando di PowerShell seguente: si noti che alcuni gruppi interni sono nascosti. Si consiglia di eseguire questa operazione con cautela.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. Posizionare il server in modalità manutenzione archiviazione eseguendo i comandi di PowerShell seguenti:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. Eseguire il cmdlet seguente per verificare che il valore di **OperationalStatus** sia **in modalità di manutenzione**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. Eseguire un'installazione di aggiornamento di Windows Server 2019 sul server eseguendo **Setup. exe** e usando l'opzione "Mantieni i file e le app personali".  
   Al termine dell'installazione, il server rimane nel cluster e il servizio cluster viene avviato automaticamente.

   5.  Verificare che il server appena aggiornato disponga degli aggiornamenti più recenti di Windows Server 2019.  
   Per ulteriori informazioni, vedere la [cronologia degli aggiornamenti di Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
   Il numero di build ( `ver` vedere il comando) deve essere 17763,292 o superiore.

   6.  Rimuovere il server dalla modalità di manutenzione dell'archiviazione usando i comandi di PowerShell seguenti:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  Riprendere il server usando il comando PowerShell seguente:

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  Attendere che i processi di ripristino dell'archiviazione vengano completati e che tutti i dischi tornino a uno stato integro.  
   Questa operazione dovrebbe essere relativamente veloce, perché le macchine virtuali non sono in esecuzione. Ecco i comandi da eseguire:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Aggiornare il server successivo nel cluster.
5. Dopo che tutti i server sono stati aggiornati a Windows Server 2019, usare il cmdlet di PowerShell seguente per aggiornare il livello di funzionalità del cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   È consigliabile aggiornare il livello di funzionalità del cluster il prima possibile, sebbene tecnicamente siano disponibili fino a quattro settimane.

6. Al termine dell'aggiornamento del livello di funzionalità del cluster, usare il cmdlet seguente per aggiornare il pool di archiviazione.  
   A questo punto, i nuovi cmdlet, ad `Get-ClusterPerf` esempio, saranno completamente operativi in tutti i server del cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Avviare le macchine virtuali nel cluster e verificare che funzionino correttamente.

8. Facoltativamente, aggiornare i livelli di configurazione della macchina virtuale arrestando `Update-VMVersion` ogni macchina virtuale e usando il cmdlet, quindi riavviando le macchine virtuali.

9. Verificare che il cluster aggiornato funzioni come previsto.  
   Il failover dei ruoli deve essere eseguito correttamente e se nel cluster viene usata la migrazione in tempo reale della macchina virtuale, le VM devono eseguire la migrazione in tempo reale.

10. Convalidare il cluster eseguendo la convalida`Test-Cluster`del cluster () ed esaminando il report di convalida del cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>Esecuzione di un'installazione pulita del sistema operativo mentre le macchine virtuali vengono arrestate

Questa opzione comporta tempi di inattività delle VM, ma può richiedere meno tempo rispetto a quando le VM sono in esecuzione durante l'aggiornamento perché non è necessario attendere il completamento dei processi di archiviazione (ripristino mirror) dopo l'aggiornamento di ogni server. Sebbene i singoli server vengano riavviati in modo sequenziale durante il processo di aggiornamento, i restanti server del cluster rimarranno in esecuzione.

1. Verificare che tutti i server del cluster eseguano gli aggiornamenti più recenti.  
   Per ulteriori informazioni, vedere la [cronologia degli aggiornamenti di Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
   Installare come minimo l' [articolo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) della Microsoft Knowledge base (19 febbraio 2019). Il numero di build ( `ver` vedere il comando) deve essere 14393,2828 o superiore.

2. Arrestare le macchine virtuali in esecuzione nel cluster.

3. Eseguire i passaggi seguenti in un server cluster alla volta:

   2. Sospendere il server del cluster usando il comando di PowerShell seguente: si noti che alcuni gruppi interni sono nascosti.  
      Si consiglia di eseguire questa operazione con cautela.

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. Posizionare il server in modalità manutenzione archiviazione eseguendo i comandi di PowerShell seguenti:

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. Eseguire il cmdlet seguente per verificare che il valore di **OperationalStatus** sia **in modalità di manutenzione**:

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. Rimuovere il server dal cluster eseguendo il comando PowerShell seguente:  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. Eseguire un'installazione pulita di Windows Server 2019 nel server: formattare l'unità di sistema, eseguire **Setup. exe** e usare l'opzione "Nothing".  
      È necessario configurare l'identità del server, i ruoli, le funzionalità e le applicazioni dopo il completamento dell'installazione e il riavvio del server.

   7. Installare il ruolo Hyper-V e la funzionalità clustering di failover nel server (è possibile usare il `Install-WindowsFeature` cmdlet).

   8. Installare i driver di archiviazione e di rete più recenti per l'hardware approvati dal produttore del server per l'uso con Spazi di archiviazione diretta.

   9. Verificare che il server appena aggiornato disponga degli aggiornamenti più recenti di Windows Server 2019.  
      Per ulteriori informazioni, vedere la [cronologia degli aggiornamenti di Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
      Il numero di build ( `ver` vedere il comando) deve essere 17763,292 o superiore.

   10. Aggiungere nuovamente il server al cluster usando il comando PowerShell seguente:

      ```PowerShell
      Add-ClusterNode
      ```

   11. Rimuovere il server dalla modalità di manutenzione dell'archiviazione usando il comando PowerShell seguente:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. Attendere che i processi di ripristino dell'archiviazione vengano completati e che tutti i dischi tornino a uno stato integro.  
       Questa operazione potrebbe richiedere molto tempo a seconda del numero di macchine virtuali in esecuzione durante l'aggiornamento del server. Ecco i comandi da eseguire:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Aggiornare il server successivo nel cluster.

5. Dopo che tutti i server sono stati aggiornati a Windows Server 2019, usare il cmdlet di PowerShell seguente per aggiornare il livello di funzionalità del cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   È consigliabile aggiornare il livello di funzionalità del cluster il prima possibile, sebbene tecnicamente siano disponibili fino a quattro settimane.

6. Al termine dell'aggiornamento del livello di funzionalità del cluster, usare il cmdlet seguente per aggiornare il pool di archiviazione.  
   A questo punto, i nuovi cmdlet, ad `Get-ClusterPerf` esempio, saranno completamente operativi in tutti i server del cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Avviare le macchine virtuali nel cluster e verificare che funzionino correttamente.

8. Facoltativamente, aggiornare i livelli di configurazione della macchina virtuale arrestando `Update-VMVersion` ogni macchina virtuale e usando il cmdlet, quindi riavviando le macchine virtuali.

9. Verificare che il cluster aggiornato funzioni come previsto.  
   Il failover dei ruoli deve essere eseguito correttamente e se nel cluster viene usata la migrazione in tempo reale della macchina virtuale, le VM devono eseguire la migrazione in tempo reale.

10. Convalidare il cluster eseguendo la convalida`Test-Cluster`del cluster () ed esaminando il report di convalida del cluster.
