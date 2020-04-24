---
title: Distribuire un SOFS di Spazi di archiviazione diretta a due nodi per l'archiviazione di dischi profili utente in Azure
description: Informazioni su come usare Spazi di archiviazione diretta con Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 1099f21d-5f07-475a-92dd-ad08bc155da1
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
manager: scottman
ms.openlocfilehash: 2386a231edf80fa611daf71c171bc0de3a7b497e
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855544"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>Distribuire un file server di scalabilità orizzontale di Spazi di archiviazione diretta a due nodi per l'archiviazione di dischi profili utente in Azure

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Servizi Desktop remoto (RDS) richiede un file server appartenente a un dominio per i dischi dei profili utente (UPD). Per distribuire un file server di scalabilità orizzontale appartenente a un dominio a disponibilità elevata (SOFS) in Azure, usare spazi di archiviazione diretta con Windows Server 2016. Se non hai familiarità con gli UPD o con Servizi Desktop remoto, consulta [Introduzione a Servizi Desktop remoto](welcome-to-rds.md).

> [!NOTE] 
> Microsoft ha appena pubblicato un [modello di Azure per distribuire un file server di scalabilità orizzontale di spazi di archiviazione diretta](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/). È possibile usare il modello per creare la distribuzione o usare i passaggi descritti in questo articolo. 

È consigliabile distribuire il SOFS con macchine virtuali serie DS e dischi di dati di archiviazione premium, in cui sono presenti lo stesso numero e dimensioni dei dischi dati in ogni macchina virtuale. È necessario un minimo di due account di archiviazione. 

Per distribuzioni di piccole dimensioni, si consiglia un cluster a 2 nodi con un cloud di controllo, in cui il volume è con mirroring con 2 copie. Aumentare le dimensioni delle piccole distribuzioni mediante l'aggiunta di dischi dati. Aumentare le distribuzioni più grandi mediante l'aggiunta di nodi (VM). 

Queste istruzioni sono per una distribuzione a 2 nodi. La tabella seguente mostra le dimensioni della macchina virtuale e del disco dove archiviare gli UPD per il numero di utenti nell'azienda. 

| Utenti | Totale (GB) | VM | Numero di dischi | Tipo di disco | Dimensione disco (GB) | Configurazione   |
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

Usare la procedura seguente per creare un controller di dominio (noi l'abbiamo denominata "my-dc" di seguito) e le macchine virtuali ("my fsn1" e "my fsn2") a due nodi e configurare le macchine virtuali come SOFS diretto di spazi di archiviazione a 2 nodi.

1. Creare una [Sottoscrizione Microsoft Azure](https://azure.microsoft.com).
2. Accedi al [portale di Azure](https://ms.portal.azure.com).
3. Creare un [account di archiviazione Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account) in Azure Resource Manager. Creare l'account in un nuovo gruppo di risorse e usare le configurazioni seguenti:
   - Modello di distribuzione: Resource Manager
   - Tipo di account di archiviazione: Utilizzo generico
   - Livello prestazioni: Premium
   - Opzione Replica: LRS
4. Configurare una foresta di Active Directory utilizzando un modello di avvio rapido o distribuendo la foresta manualmente. 
   - Distribuire usando un modello di avvio rapido di Azure:
      - [Creare una macchina virtuale di Azure con una nuova foresta di Active Directory](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [Creare un nuovo dominio di Active Directory con 2 controller di dominio](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) (per la disponibilità elevata)
   - [Distribuire manualmente la foresta](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/) con le configurazioni seguenti:
      - Creare la rete virtuale nello stesso gruppo di risorse dell'account di archiviazione.
      - Dimensioni consigliate: DS2 (aumentare le dimensioni, se il controller di dominio ospita più oggetti di dominio)
      - Usare una rete virtuale generata automaticamente.
      - Seguire i passaggi per installare Active Directory Domain Services.
5. Configurare i nodi del cluster di file server. È possibile farlo tramite la distribuzione del [modello di Azure di cluster SOFS diretto di archiviazione spazi di Windows Server 2016](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/) oppure seguendo i passaggi da 6 a 11 per eseguire la distribuzione manualmente.
6. Per configurare i nodi del cluster di file server manualmente:
   1. Creare il primo nodo: 
      1. Creare una nuova macchina virtuale usando l'immagine di Windows Server 2016. (Fare clic su **Nuovo > Macchine virtuali > Windows Server 2016.** Selezionare **Resource Manager**, quindi fare clic su **Crea**.)
      2. Impostare la configurazione di base come segue:
         - Nome: my-fsn1
         - Tipo di disco della macchina virtuale unità SSD
         - Usare un gruppo di risorse esistente, quello creato nel passaggio 3. 
      3. Dimensioni: DS1, DS2, DS3, DS4 o DS5 a seconda delle necessità dell'utente (vedere la tabella all'inizio delle istruzioni). Verificare che sia selezionato il supporto disco premium.
      4. Impostazioni: 
         - Account di archiviazione: Scegliere l'account di archiviazione creato nel passaggio 3.
         - Disponibilità elevata: creare un nuovo set di disponibilità. (Fare clic su **Disponibilità elevata > Crea nuovo**e quindi immettere un nome (ad esempio, s2d-cluster). Usare i valori predefiniti per **domini di aggiornamento** e **domini di errore**.)
   2. Creare il secondo nodo. Ripetere il passaggio precedente con le modifiche seguenti:
      - Nome: my-fsn2
      - Disponibilità elevata: selezionare il set di disponibilità creato in precedenza.  
7. [Collegare i dischi dati](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/) alle macchine virtuali del nodo cluster in base alle necessità dell'utente (come illustrato nella tabella precedente). Dopo la creazione dei dischi dati e il collegamento alla VM, impostare **la memorizzazione nella cache per gli host** su **Nessuna**.
8. Impostare gli indirizzi IP per tutte le macchine virtuali su **Statico**. 
   1. Nel gruppo di risorse, selezionare una macchina virtuale e quindi fare clic su **Interfacce di rete** (sotto **impostazioni**). Selezionare l'interfaccia di rete elencata e quindi fare clic su **Configurazioni IP**. Selezionare la configurazione IP elencata, selezionare **Statico**, quindi fare clic su **Salva**.
   2. Si noti l'indirizzo IP privato (10.x.x.x) del controller di dominio (my-dc per questo esempio).
9. Impostare l'indirizzo server DNS primario sulle schede di interfaccia di rete delle macchine virtuali del nodo del cluster per il server my-dc. Selezionare la macchina virtuale e quindi fare clic su **Interfacce di rete > Server DNS > DNS personalizzato**. Immettere l'indirizzo IP privato annotato in precedenza e quindi fare clic su **Salva**.
10. Creare un [account di archiviazione di Azure per il cloud di controllo](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness). (Se si usano le istruzioni del link, fermarsi quando si arriva a "Configurazione deli cloud di controllo con Failover Cluster Manager GUI". Eseguiremo tale passaggio di seguito.)
11. Configurare il file server di spazi di archiviazione diretta. Connettersi a un nodo della macchina virtuale e quindi eseguire i seguenti cmdlet di Windows PowerShell.
    1. Installare la funzionalità Clustering di failover e la funzionalità File server nelle due macchine virtuali del nodo del cluster di file server:

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. Convalidare le macchine virtuali del nodo del cluster e creare i cluster SOFS a 2 nodi:

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. Configurare il cloud di controllo. Utilizzare il nome e la chiave di accesso dell'account di archiviazione del cloud di controllo.

       ```powershell
       Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> 
       ```
    4. Abilitare Spazi di archiviazione diretta.

       ```powershell
       Enable-ClusterS2D 
       ```
      
    5. Creare un volume del disco virtuale.

       ```powershell
       New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 120GB 
       ```
       Per visualizzare informazioni sul volume condiviso del cluster nel cluster SOFS, eseguire il cmdlet seguente:

       ```powershell
       Get-ClusterSharedVolume
       ```
   
    6. Creare un file server di scalabilità orizzontale (SOFS):

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. Creare una nuova condivisione file SMB nel cluster SOFS.

       ```powershell
       New-Item -Path C:\ClusterStorage\VDisk01\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\VDisk01\Data
       ```

È ora disponibile una condivisione in `\\my-sofs1\UpdStorage`, che è possibile usare per l'archiviazione UPD quando si [abilita UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx) per gli utenti. 
