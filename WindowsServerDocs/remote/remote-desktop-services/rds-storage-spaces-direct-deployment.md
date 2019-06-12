---
title: Distribuire un SOFS diretto di spazi di archiviazione di due nodi per l'archiviazione UPD in Azure
description: Informazioni su come usare spazi di archiviazione diretta con Servizi Desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1099f21d-5f07-475a-92dd-ad08bc155da1
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
manager: scottman
ms.openlocfilehash: 792c9320f6976a4fc7f2ccd235f66daa0cb19b19
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805196"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>Distribuire un server di scalabilità orizzontale di spazi di archiviazione diretta due nodi per l'archiviazione UPD in Azure

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016

Servizi Desktop remoto (RDS) richiede un dominio file server per i dischi dei profili utente (UPD). Per distribuire un server a disponibilità elevata appartenenti a un dominio di file di scalabilità orizzontale (SOFS) in Azure, usare spazi di archiviazione diretta con Windows Server 2016. Se non si ha familiarità con gli UPD o Servizi Desktop remoto, estrarre [Benvenuti in Servizi Desktop remoto](welcome-to-rds.md).

> [!NOTE] 
> Microsoft appena pubblicato un [modello di Azure per distribuire un server di scalabilità orizzontale di spazi di archiviazione diretta](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/)! È possibile usare il modello per creare la distribuzione o usare i passaggi descritti in questo articolo. 

È consigliabile distribuire il SOFS con macchine virtuali serie DS e dischi di dati di archiviazione premium, in cui sono presenti lo stesso numero e dimensioni dei dischi dati in ogni macchina virtuale. È necessario un minimo di due account di archiviazione. 

Per distribuzioni di piccole dimensioni, si consiglia un cluster a 2 nodi con un cloud di controllo, in cui il volume è con mirroring con 2 copie. Aumento delle dimensioni piccole distribuzioni mediante l'aggiunta di dischi dati. Aumentare le distribuzioni più grandi mediante l'aggiunta di nodi (VM). 

Queste istruzioni sono per una distribuzione con 2 nodi. La tabella seguente mostra le dimensioni della macchina virtuale e disco, devi archiviare gli UPD per il numero di utenti nell'azienda. 

| Utenti | Totale (GB) | VM | # Dischi | Tipo di disco | Dimensioni disco (GB) | Configurazione   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2x(DS1 + 2 P20)  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2x(DS1 + 2 P30)  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2x(DS2 + 3 P30)  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2x(DS3 + 5 P30)  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2x(DS4 + 13 P30) |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2x(DS5 + 25 P30) | 

Usare la procedura seguente per creare un controller di dominio (noi l'abbiamo denominata "my-dc" di seguito) e le macchine virtuali ("my fsn1" e "my fsn2") a due nodi e configurare le macchine virtuali da un SOFS diretto di spazi di 2 nodi di archiviazione.

1. Creare un [sottoscrizione di Microsoft Azure](https://azure.microsoft.com).
2. Accedi il [portale di Azure](https://ms.portal.azure.com).
3. Creare un [account di archiviazione Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account) in Azure Resource Manager. Crearlo in un nuovo gruppo di risorse e usare le configurazioni seguenti:
   - Modello di distribuzione: Resource Manager
   - Tipo di account di archiviazione: Utilizzo generico
   - Livello di prestazioni: Premium
   - Opzione di replica: LRS
4. Configurare una foresta di Active Directory utilizzando un modello di avvio rapido o distribuzione manuali dell'insieme di strutture. 
   - Distribuire usando un modello di avvio rapido di Azure:
      - [Creare una VM di Azure con una nuova foresta di Active Directory](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [Creare un nuovo dominio di Active Directory con 2 controller di dominio](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) (per la disponibilità elevata)
   - Manualmente [distribuire foresta](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/) con le configurazioni seguenti:
      - Creare la rete virtuale nello stesso gruppo di risorse dell'account di archiviazione.
      - Dimensioni consigliate: DS2 (aumentare le dimensioni, se il controller di dominio ospita più oggetti di dominio)
      - Usare una rete virtuale generata automaticamente.
      - Seguire i passaggi per installare Active Directory Domain Services.
5. Configurare i nodi del cluster file server. È possibile farlo tramite la distribuzione di [cluster SOFS diretta di Windows Server 2016 archiviazione spazi modello di Azure](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/) oppure seguendo i passaggi da 6 a 11 per distribuire manualmente.
6. Per configurare manualmente i nodi del cluster file server:
   1. Creare il primo nodo: 
      1. Creare una nuova macchina virtuale usando l'immagine di Windows Server 2016. (Fare clic su **nuovo > macchine virtuali > Windows Server 2016.** Selezionare **Resource Manager**, quindi fare clic su **crea**.)
      2. Impostare la configurazione di base come segue:
         - Nome: my-fsn1
         - Tipo di disco della macchina virtuale SSD
         - Usare un gruppo di risorse esistente, quello creato nel passaggio 3. 
      3. Dimensioni: DS1, DS2, DS3, DS4 o e DS5 a seconda dell'utente deve (vedere la tabella all'inizio delle istruzioni è disponibile). Verificare che sia selezionato supporto disco premium.
      4. Impostazioni: 
         - Account di archiviazione: Scegliere l'account di archiviazione creato nel passaggio 3.
         - Disponibilità elevata - creare un nuovo set di disponibilità. (Fare clic su **disponibilità elevata > Crea nuovo**e quindi immettere un nome (ad esempio, s2d-cluster). Usare i valori predefiniti per **domini di aggiornamento** e **domini di errore**.)
   2. Creare il secondo nodo. Ripetere il passaggio precedente con le modifiche seguenti:
      - Nome: my-fsn2
      - Disponibilità elevata - selezionare che il set di disponibilità creato in precedenza.  
7. [Collegare dischi dati](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/) per macchine virtuali del nodo cluster in base a utente alle esigenze (come illustrato nella tabella precedente). Dopo che i dati vengono creati e collegati alla VM, impostare i dischi **ospitare la memorizzazione nella cache** al **None**.
8. Impostare gli indirizzi IP per tutte le macchine virtuali **statici**. 
   1. Nel gruppo di risorse, selezionare una macchina virtuale e quindi fare clic su **interfacce di rete** (sotto **impostazioni**). Selezionare l'interfaccia di rete elencata e quindi fare clic su **configurazioni IP**. Selezionare la configurazione IP elencata, selezionare **statici**, quindi fare clic su **salvare**.
   2. Si noti il dominio controller (my-controller di dominio per questo esempio) indirizzo IP privato (10.x.x.x).
9. Impostare indirizzo server DNS primario su schede di rete di macchine virtuali del nodo cluster per il server di risorse del controller di dominio. Selezionare la macchina virtuale e quindi fare clic su **interfacce di rete > server DNS > DNS personalizzato**. Immettere l'indirizzo IP privato è indicato in precedenza e quindi fare clic su **salvare**.
10. Creare un [account di archiviazione di Azure per il cloud di controllo](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness). (Se si usano istruzioni collegate, arrestata quando si arriva alla "Configurazione di Cloud di controllo con Failover Cluster Manager GUI" - lo faremo tale passaggio riportato di seguito).
11. Configurare il file server di spazi di archiviazione diretta. Connettersi a un nodo della macchina virtuale e quindi eseguire i seguenti cmdlet di Windows PowerShell.
    1. Installare la funzionalità Clustering di Failover e la funzionalità File Server in due file server cluster macchine virtuali del nodo:

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. Convalidare macchine virtuali del nodo cluster e creare i cluster SOFS a 2 nodi:

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. Configurare il controllo del cloud. Utilizzare la cloud witness accesso e il nome chiave dell'account.

       ```powershell
       Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> 
       ```
    4. Abilitare spazi di archiviazione diretta.

       ```powershell
       Enable-ClusterS2D 
       ```
      
    5. Creare un volume del disco virtuale.

       ```powershell
       New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 120GB 
       ```
       Per visualizzare informazioni sul volume condiviso cluster nel cluster SOFS, eseguire il cmdlet seguente:

       ```powershell
       Get-ClusterSharedVolume
       ```
   
    6. Creare il server di scalabilità orizzontale (SOFS):

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. Creare una nuova condivisione file SMB nel cluster SOFS.

       ```powershell
       New-Item -Path C:\ClusterStorage\Volume1\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\Volume1\Data
       ```

È ora disponibile una condivisione nella `\\my-sofs1\UpdStorage`, che è possibile usare per l'archiviazione UPD quando si [abilitare UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx) per gli utenti. 
