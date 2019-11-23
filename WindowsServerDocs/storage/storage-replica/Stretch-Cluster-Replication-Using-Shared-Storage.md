---
title: Replica di un cluster esteso tramite l'archiviazione condivisa
ms.prod: windows-server
manager: eldenc
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 6c5b9431-ede3-4438-8cf5-a0091a8633b0
ms.openlocfilehash: 654b4aea135c360f5fc5f59fdf85627fe8dd4cc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402966"
---
# <a name="stretch-cluster-replication-using-shared-storage"></a>Replica di un cluster esteso tramite l'archiviazione condivisa

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

In questo esempio di valutazione i computer e la relativa archiviazione verranno configurati in un singolo cluster esteso, in cui due nodi condividono un unico set di archiviazione e due nodi condividono un altro set di archiviazione e la replica mantiene il mirroring di entrambi i gruppi di archiviazione nel cluster per consentire il failover immediato. Questi nodi e la relativa archiviazione devono trovarsi in siti fisici separati, anche se non è obbligatorio. Esistono due fasi distinte per la creazione di cluster Hyper-V e file server come scenari di esempio.  

> [!IMPORTANT]  
> In questa valutazione, i server in siti differenti devono essere in grado di comunicare con gli altri server tramite una rete, ma non dispongono di una connessione fisica all'archiviazione condivisa dell'altro sito. Questo scenario non si avvale di Spazi di archiviazione diretta.  

## <a name="terms"></a>Condizioni  
Questa procedura dettagliata usa l'ambiente seguente come esempio:  

-   Quattro server, denominati **SR-SRV01**, **SR-SRV02**, **SR-SRV03**, e **SR-SRV04** trasformati in un singolo cluster denominato **SR-SRVCLUS**.  

-   Una coppia di "siti" logici che rappresentano due centri dati diversi, uno chiamato **Redmond** e l'altro **Bellevue**.  

> [!NOTE]  
> È possibile usare solo due nodi in cui un nodo è presente in ogni sede. Tuttavia, non sarà in grado di eseguire il failover all'interno del sito con solo due server. È possibile usare un massimo di 64 nodi.

![Immagine che illustra due nodi della sede di Redmond di cui viene eseguita la replica con due nodi dello stesso cluster della sede di Bellevue](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_StretchClusterExample.png)  

**Figura 1: replica di archiviazione in un cluster esteso**  

## <a name="prerequisites"></a>Prerequisiti  
-   Foresta di Active Directory Domain Services (non è necessario eseguire Windows Server 2016).  
-   Server 2-64 che eseguono Windows Server 2019 o Windows Server 2016, Datacenter Edition. Se si esegue Windows Server 2019, è possibile usare l'edizione standard se si sta eseguendo la replica di un solo volume con dimensioni massime di 2 TB. 
-   Due set di risorse di archiviazione condivise che usano JBOD SAS (come con Spazi di archiviazione), SAN Fibre Channel, VHDX condiviso o destinazione iSCSI. Lo spazio di archiviazione deve contenere una combinazione di unità SSD e disco rigido e deve supportare la prenotazione permanente. Ogni risorsa di archiviazione verrà resa disponibile solo a due dei server (asimmetrica).  
-   Ogni set di risorse di archiviazione deve consentire la creazione di almeno due dischi virtuali, uno per i dati replicati e uno per i registri. L'archiviazione fisica deve avere le stesse dimensioni di settore su tutti i dischi di dati. L'archiviazione fisica deve avere le stesse dimensioni di settore su tutti i dischi dei registri.  
-   Almeno una connessione da 1 GbE su ciascun server per la replica sincrona, ma preferibilmente RDMA.   
-   Almeno 2 GB di RAM e due core per server. Se si dispone di più macchine virtuali saranno necessari più memoria e più core.  
-   Regole firewall e router appropriate per consentire ICMP, SMP (porta 445 più 5445 per SMB diretto) e traffico bidirezionale WS-MAN (porta 5985) tra tutti i nodi.  
-   Una rete tra i server con larghezza di banda sufficiente per contenere il carico di lavoro di scrittura delle operazioni di I/O e una latenza media di andata e ritorno di =5 ms, per la replica sincrona. La replica asincrona non dispone di un'indicazione di latenza.  
-   L'archiviazione replicata non può trovarsi nell'unità che contiene la cartella del sistema operativo Windows.

Molti di questi requisiti possono essere determinati usando il cmdlet `Test-SRTopology`. Se si installano le funzionalità Replica archiviazione o Strumenti di gestione di Replica archiviazione in almeno un server è possibile accedere a questo strumento. Non è necessario configurare Replica archiviazione perché usi questo strumento, ma è necessario installare il cmdlet. Altre informazioni sono incluse nei passaggi seguenti.  

## <a name="provision-operating-system-features-roles-storage-and-network"></a>Effettuare il provisioning di sistema operativo, funzionalità, ruoli, archiviazione e rete  

1.  Installare Windows Server in tutti i nodi del server usando le opzioni di installazione Server Core o server con esperienza desktop.  
    > [!IMPORTANT]
    > Da questo momento, accedere sempre a tutti i server come utente di dominio, ovvero un membro del gruppo amministratore incorporato. Ricordarsi di elevare i prompt di PowerShell e CMD futuro durante l'esecuzione in un'installazione server con interfaccia grafica o in un computer Windows 10.

2.  Aggiungere informazioni di rete e unire i nodi al dominio, quindi riavviarli.  
    > [!NOTE]
    > A partire da questo punto, la guida presuppone che si disponga di due associazioni di server da usare in un cluster esteso. Una rete LAN o WAN separa i server e i server appartengono a siti fisici o logici. La guida considera che **SR-SRV01** e **SR-SRV02** siano nel sito Redmond e che **SR-SRV03** e **SR-SRV04** siano nel sito **Bellevue**.  

3.  Connettere il primo set di enclosure di risorse di archiviazione condivise JBOD, VHDX condiviso, destinazione iSCSI o FC SAN ai server nel sito **Redmond**.  

4.  Collegare il secondo set di risorse di archiviazione ai server nel sito **Bellevue**.  

5.  Se necessario, installare il sistema di archiviazione del fornitore e il firmware dell'enclosure più recente, i driver HBA del fornitore più recenti, i firmware BIOS o UEFI del fornitore più recenti, i driver di rete del fornitore più recenti e driver dei chipset della scheda madre più recenti in tutti e quattro i nodi. Se necessario riavviare i nodi.  

    > [!NOTE]
    > Per la configurazione dell'archiviazione condivisa e dell'hardware di rete, consultare la documentazione del fornitore hardware.  

6.  Assicurarsi che le impostazioni BIOS/UEFI dei server abilitati consentano alte prestazioni, ad esempio la disabilitazione dei C-State, l'impostazione della velocità QPI, l'abilitazione di NUMA e l'impostazione della frequenza di memoria massima. Assicurarsi che il risparmio energia in Windows Server sia impostato su prestazioni elevate. Riavviare se necessario.  

7.  Configurare i ruoli come indicato di seguito:  

    -   **Metodo grafico**  

        Eseguire **ServerManager.exe** e aggiungere tutti i nodi dei server facendo clic su **Gestisci** e **Aggiungi server**.  

        > [!IMPORTANT]
        > Installare i ruoli e le funzionalità **Clustering di failover** e **Replica archiviazione** in ciascuno dei nodi e riavviarli. Se si intende usare altri ruoli come Hyper-V o File server, è possibile installarli ora.  

    -   **Uso del metodo di Windows PowerShell**  

        In un computer di gestione remota o **SR-SRV04**, eseguire il comando seguente nella console di Windows PowerShell per installare i ruoli e le funzionalità necessarie per un cluster esteso sui quattro nodi e riavviarli:  

        ```PowerShell  
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'  

        $Servers | foreach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }  

        ```  

        Per altre informazioni su questi passaggi, vedere [Installazione o disinstallazione di ruoli, servizi ruolo o funzionalità](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md).  


8. Configurare lo spazio di archiviazione come indicato di seguito:  

    > [!IMPORTANT]  
    > -   È necessario creare due volumi in ciascuna enclosure: uno per i dati e uno per i registri.  
    > -   I dischi di dati e registro devono essere inizializzati come GPT, non come MBR.  
    > -   I due volumi di dati devono avere le stesse dimensioni.  
    > -   I due volumi di registro devono avere le stesse dimensioni.  
    > -   Tutti i dischi di dati replicati devono avere le stesse dimensioni del settore.  
    > -   Tutti i dischi di registro devono avere le stesse dimensioni del settore.  
    > -   I volumi di log devono usare risorse di archiviazione basate su flash e impostazioni di resilienza ad alte prestazioni. Microsoft consiglia una velocità di archiviazione dei log superiore a quella dell'archiviazione dei dati. I volumi di log non devono essere utilizzati per altri carichi di lavoro. 
    > -   I dischi di dati possono usare HDD, SSD o una combinazione a più livelli e spazi con mirroring o di parità o RAID 1 o 10, oppure RAID 5 o RAID 50.  
    > -  Il volume di log deve essere di almeno 9 GB per impostazione predefinita e può essere superiore o inferiore in base ai requisiti del log.  
    > - I volumi devono essere formattati con NTFS o ReFS.
    > - Il ruolo File Server è necessario solo per il funzionamento di Test-SRTopology, in quanto apre le porte del firewall necessarie per il test.  

    -   **Per enclosure JBOD:**  

        1.  Assicurarsi che ogni set di nodi del server associati possa visualizzare solo l'enclosure di archiviazione del sito (archiviazione asimmetrica) e che le connessioni SAS siano configurate correttamente.  

        2.  Effettuare il provisioning dell'archiviazione mediante Spazi di archiviazione seguendo i **passaggi 1-3** indicati in [Distribuire Spazi di archiviazione in un server autonomo](../storage-spaces/deploy-standalone-storage-spaces.md) usando Windows PowerShell o Server Manager.  

    -   **Per l'archiviazione iSCSI:**  

        1.  Assicurarsi che ogni set di nodi del server associati possa visualizzare solo l'enclosure di archiviazione del sito (archiviazione asimmetrica). Se si usa iSCSI, è necessario usare più di una singola scheda di rete.  

        2.  Effettua il provisioning dell'archiviazione usando la documentazione del fornitore. Se si usa la destinazione iSCSI basata su Windows, consultare [iSCSI Target Block Storage, How To](../iscsi/iscsi-target-server.md) (Procedura per l'archiviazione a blocchi nella destinazione iSCSI).  

    -   **Per l'archiviazione SAN FC:**  

        1.  Assicurati che ogni set di nodi del server associati possa visualizzare solo l'enclosure di archiviazione del sito (archiviazione asimmetrica) e che gli host siano stati suddivisi in zone correttamente.  

        2.  Effettua il provisioning dell'archiviazione usando la documentazione del fornitore.  

## <a name="configure-a-hyper-v-failover-cluster-or-a-file-server-for-a-general-use-cluster"></a>Configurare un cluster di failover Hyper-V o un file server per un cluster per uso generale

Dopo aver configurato i nodi del server, il passaggio successivo consiste nel creare uno dei tipi di cluster seguenti:  
*  [Cluster di failover Hyper-V](#BKMK_HyperV)  
*  [File server per il cluster di utilizzo generale](#BKMK_FileServer)  

### <a name="BKMK_HyperV"></a>Configurare un cluster di failover Hyper-V  

>[!NOTE]
> Se si desidera creare un cluster del file server e non un cluster Hyper-V, saltare questa sezione e andare alla sezione [Configurazione di un file server per un cluster per uso generale](#BKMK_FileServer).  

Ora si creerà un cluster di failover normale. Dopo la configurazione, la convalida e la fase di test, il cluster dovrà essere esteso usando Replica archiviazione. È possibile eseguire tutti i passaggi seguenti nei nodi del cluster direttamente o da un computer di gestione remota che contiene il Strumenti di amministrazione remota del server di Windows Server.  

#### <a name="graphical-method"></a>Metodo grafico  

1. Eseguire **cluadmin.msc**.  

2. Convalidare il cluster proposto e analizzare i risultati per assicurarsi di poter continuare.  

   > [!NOTE]  
   > Potrebbero verificarsi errori di archiviazione durante la convalida del cluster a causa dell'uso dell'archiviazione asimmetrica.  

3. Creare il cluster di calcolo Hyper-V. Assicurarsi che il nome del cluster non superi i 15 caratteri di lunghezza. Nell'esempio seguente il nome è SR-SRVCLUS. Se i nodi si trovano in subnet diverse, è necessario creare un indirizzo IP per il nome del cluster per ogni subnet e usare la dipendenza "OR".  Per altre informazioni [, vedere Configurazione degli indirizzi IP e delle dipendenze per i cluster su più subnet-parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).  

4. Configurare un controllo di condivisione file o un controllo del cloud per fornire quorum in caso di perdita del sito.  

   > [!NOTE]  
   > WIndows Server include ora un'opzione per il controllo basato su cloud (Azure). È possibile scegliere questa opzione di quorum anziché il controllo di condivisione file.  

   > [!WARNING]  
   > Per altre informazioni sulla configurazione quorum, vedere [Configurare e gestire il quorum in un cluster di failover di Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx). Per altre informazioni sul cmdlet `Set-ClusterQuorum`, vedere [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).  

5. Consultare [Network Recommendations for a Hyper-V Cluster in Windows Server 2012](https://technet.microsoft.com/library/dn550728.aspx) (Consigli sulla rete per un cluster Hyper-V in Windows Server 2012) e assicurarsi che la rete cluster sia stata configurata correttamente.  

6. Aggiungere un disco al cluster CSV nel sito Redmond. A tale scopo, fare clic con il pulsante destro del mouse su un disco di origine nel nodo **Dischi** della sezione **Archiviazione**, e quindi fare clic su **Aggiungi a volumi condivisi cluster**.  

7. Usando la guida [Distribuire un cluster Hyper-V](https://technet.microsoft.com/library/jj863389.aspx), segui i passaggi da 7 a 10 all'interno del sito **Redmond** per creare una macchina virtuale di test solo per assicurarti che il cluster stia funzionando normalmente all'interno dei due nodi che condividono l'archiviazione nel primo sito di test.  

8. Se crei un cluster esteso a due nodi, devi aggiungere tutta l'archiviazione prima di continuare. A tale scopo, aprire una sessione di PowerShell con autorizzazioni amministrative nei nodi del cluster ed eseguire il comando seguente: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Si tratta del comportamento predefinito in Windows Server 2016.

9. Avviare Windows PowerShell e usare il cmdlet `Test-SRTopology` per determinare se siano stati soddisfatti tutti i requisiti di Replica archiviazione.  

    Ad esempio, per convalidare dei nodi del cluster esteso proposti che presentano un volume **D:** ed **E:** ed eseguire il test per 30 minuti:
   1. Spostare tutta le risorse di archiviazione disponibili in **SR-SRV01**.
   2. Fare clic su **Crea ruolo vuoto** nella sezione **Ruoli** di Gestione cluster di failover.
   3. Aggiungere le risorse di archiviazione online a tale ruolo vuoto denominato **Nuovo ruolo**.
   4. Spostare tutte le risorse di archiviazione disponibili in **SR-SRV03**.
   5. Fare clic su **Crea ruolo vuoto** nella sezione **Ruoli** di Gestione cluster di failover.
   6. Spostare il **Nuovo ruolo (2)** vuoto in **SR-SRV03**.
   7. Aggiungere le risorse di archiviazione online a tale ruolo vuoto denominato **Nuovo ruolo (2)** .
   8. Tutte le risorse di archiviazione sono ora montate con lettere di unità e possono valutare il cluster con `Test-SRTopology`.

       Ad esempio:

           MD c:\temp  

           Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName D: -SourceLogVolumeName E: -DestinationComputerName SR-SRV03 -DestinationVolumeName D: -DestinationLogVolumeName E: -DurationInMinutes 30 -ResultPath c:\temp        

      > [!IMPORTANT]
      > Quando si usa un server di prova privo di carichi di operazioni I/O di scrittura nel volume di origine specificato durante il periodo di valutazione, considerare l'aggiunta di un carico di lavoro o Test-SRTopology non genererà un report utile. È necessario eseguire il test con carichi di lavoro simili alla produzione per poter visualizzare i numeri reali e le dimensioni consigliate del registro. In alternativa, è sufficiente copiare alcuni file nel volume di origine durante il test o scaricare ed eseguire DISKSPD per generare operazioni di I/O di scrittura. Ad esempio, un campione con un carico di lavoro di I/O di scrittura basso per dieci minuti per il volume D: come mostrato di seguito:   
       `Diskspd.exe -c1g -d600 -W5 -C5 -b4k -t2 -o2 -r -w5 -i100 d:\test.dat`  

10. Esaminare il report **TestSrTopologyReport-&lt; date &gt;.html** per assicurarsi di soddisfare i requisiti di Replica archiviazione e prendere nota delle stime iniziali sulla durata della sincronizzazione e delle raccomandazioni del registro.  

      ![Schermata che mostra il report di replica](./media/Stretch-Cluster-Replication-Using-Shared-Storage/SRTestSRTopologyReport.png)

11. Ripristina i dischi su Risorsa di archiviazione disponibile e rimuovi i ruoli vuoti temporanei.

12. Dopo aver verificato il funzionamento, rimuovi la macchina virtuale di test. Aggiungere tutte le macchine virtuali di test reali necessarie a un nodo di origine proposto per un'ulteriore valutazione.  

13. Configurare il riconoscimento dei siti del cluster esteso in modo che i server **SR-SRV01** e **SR-SRV02** si trovino nel sito **Redmond**, **SRV03 SR** e **SR-SRV04** si trovino nel sito **Bellevue** e **Redmond** diventi preferito per la proprietà del nodo dell'archiviazione di origine e delle macchine virtuali:  

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  
   
    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  
   
    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"
    ```

    > [!NOTE]
    > Non è possibile configurare il riconoscimento dei siti usando Gestione cluster di failover in Windows Server 2016.  

14. **(Facoltativo)** Configurare reti di cluster e Active Directory per accelerare il failover del sito DNS. È possibile usare le reti definite da software Hyper-V, VLAN estese, i dispositivi di astrazione rete, TTL DNS ridotti e altre tecniche comuni.

    Per altre informazioni, rivedere la sessione di Microsoft Ignite [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](http://channel9.msdn.com/Events/Ignite/2015/BRK3487) (Estensione dei cluster di failover e uso della replica di archiviazione in Windows Server vNext) e il post di blog [Enable Change Notifications between Sites - How and Why?](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx) (Abilitare le notifiche di modifica tra i siti - Come e perché?).  

15. **(Facoltativo)** Configurare la resilienza della macchina virtuale in modo che i guest non vengano sospesi a lungo durante gli errori dei nodi. In questo modo eseguiranno il failover nella nuova archiviazione di origine di replica entro 10 secondi.  

    ```PowerShell  
    (Get-Cluster).ResiliencyDefaultPeriod=10  
    ```  

    > [!NOTE]
    >  Non è possibile configurare la resilienza della macchina virtuale usando Gestione cluster di failover in Windows Server 2016.  

#### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  

1. Testare il cluster proposto e analizzare i risultati per assicurarsi di poter continuare:  

   ```PowerShell  
   Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04  
   ```  

   > [!NOTE]
   >  Potrebbero verificarsi errori di archiviazione durante la convalida del cluster a causa dell'uso dell'archiviazione asimmetrica.  

2. Creare il file server per il cluster di archiviazione di uso generale (è necessario specificare il proprio indirizzo IP statico che il cluster userà). Assicurarsi che il nome del cluster non superi i 15 caratteri di lunghezza.  Se i nodi si trovano in subnet diverse, è necessario creare un indirizzo IP per il sito aggiuntivo utilizzando la dipendenza "OR". Per altre informazioni [, vedere Configurazione degli indirizzi IP e delle dipendenze per i cluster su più subnet-parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).
   ```PowerShell  
   New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here>  
   Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”
   Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
   ```  

3. Configurare un controllo di condivisione file o un controllo cloud (Azure) nel cluster che punta a una condivisione ospitata nel controller di dominio o un altro server indipendente. Ad esempio:  

   ```PowerShell  
   Set-ClusterQuorum -FileShareWitness \\someserver\someshare  
   ```  

   > [!NOTE]
   > WIndows Server include ora un'opzione per il controllo basato su cloud (Azure). È possibile scegliere questa opzione di quorum anziché il controllo di condivisione file.  
    
   Per altre informazioni sulla configurazione quorum, vedere [Configurare e gestire il quorum in un cluster di failover di Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx). Per altre informazioni sul cmdlet `Set-ClusterQuorum`, vedere [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).  

4. Consultare [Network Recommendations for a Hyper-V Cluster in Windows Server 2012](https://technet.microsoft.com/library/dn550728.aspx) (Consigli sulla rete per un cluster Hyper-V in Windows Server 2012) e assicurarsi che la rete cluster sia stata configurata correttamente.  

5. Se crei un cluster esteso a due nodi, devi aggiungere tutta l'archiviazione prima di continuare. A tale scopo, aprire una sessione di PowerShell con autorizzazioni amministrative nei nodi del cluster ed eseguire il comando seguente: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Si tratta del comportamento predefinito in Windows Server 2016.

6. Usando la guida [Distribuire un cluster Hyper-V](https://technet.microsoft.com/library/jj863389.aspx), segui i passaggi da 7 a 10 all'interno del sito **Redmond** per creare una macchina virtuale di test solo per assicurarti che il cluster stia funzionando normalmente all'interno dei due nodi che condividono l'archiviazione nel primo sito di test.  

7. Dopo aver verificato il funzionamento, rimuovere la macchina virtuale di test. Aggiungere tutte le macchine virtuali di test reali necessarie a un nodo di origine proposto per un'ulteriore valutazione.  

8. Configurare il riconoscimento dei siti del cluster esteso in modo che i server **SR-SRV01** e **SR-SRV02** si trovino nel sito **Redmond**, **SRV03 SR** e **SR-SRV04** si trovino nel sito **Bellevue** e **Redmond** diventi preferito per la proprietà del nodo dell'archiviazione di origine e delle macchine virtuali:  

   ```PowerShell  
   New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

   New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

   Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
   Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
   Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
   Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

   (Get-Cluster).PreferredSite="Seattle"  
   ```  

9. **(Facoltativo)** Configurare reti di cluster e Active Directory per accelerare il failover del sito DNS. È possibile usare le reti definite da software Hyper-V, VLAN estese, i dispositivi di astrazione rete, TTL DNS ridotti e altre tecniche comuni.  

   Per altre informazioni, rivedere la sessione di Microsoft Ignite [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](http://channel9.msdn.com/Events/Ignite/2015/BRK3487) (Estensione dei cluster di failover e uso della replica di archiviazione in Windows Server vNext) e [Enable Change Notifications between Sites - How and Why?](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx) (Abilitare le notifiche di modifica tra i siti - Come e perché?).  

10. **(Facoltativo)** Configurare la resilienza della macchina virtuale in modo che i guest non vengano sospesi a lungo durante gli errori dei nodi. In questo modo eseguiranno il failover nella nuova archiviazione di origine di replica entro 10 secondi.  

    ```PowerShell  
    (Get-Cluster).ResiliencyDefaultPeriod=10  
    ```  

    > [!NOTE]  
    > Non è possibile configurare la resilienza della macchina virtuale usando Gestione cluster di failover in Windows Server 2016.  



### <a name="BKMK_FileServer"></a>Configurare un file server per un cluster di uso generale  

>[!NOTE]
> Se si è già configurato un cluster di failover Hyper-V come descritto in [Configurazione di un cluster di failover Hyper-V](#BKMK_HyperV), ignorare questa sezione.  

Ora si creerà un cluster di failover normale. Dopo la configurazione, la convalida e la fase di test, il cluster dovrà essere esteso usando Replica archiviazione. È possibile eseguire tutti i passaggi seguenti nei nodi del cluster direttamente o da un computer di gestione remota che contiene il Strumenti di amministrazione remota del server di Windows Server.  

#### <a name="graphical-method"></a>Metodo grafico  

1. Eseguire cluadmin.msc.  

2. Convalidare il cluster proposto e analizzare i risultati per assicurarsi di poter continuare.  
   >[!NOTE]
   >Potrebbero verificarsi errori di archiviazione durante la convalida del cluster a causa dell'uso dell'archiviazione asimmetrica.   
3. Creare il file server per il cluster di archiviazione per uso generale. Assicurarsi che il nome del cluster non superi i 15 caratteri di lunghezza. Nell'esempio seguente il nome è SR-SRVCLUS.  Se i nodi si trovano in subnet diverse, è necessario creare un indirizzo IP per il nome del cluster per ogni subnet e usare la dipendenza "OR".  Per altre informazioni [, vedere Configurazione degli indirizzi IP e delle dipendenze per i cluster su più subnet-parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).  

4. Configurare un controllo di condivisione file o un controllo del cloud per fornire quorum in caso di perdita del sito.  
   >[!NOTE]
   > WIndows Server include ora un'opzione per il controllo basato su cloud (Azure). È possibile scegliere questa opzione di quorum anziché il controllo di condivisione file.                                                                                                                                                                             
   >[!NOTE]
   >  Per altre informazioni sulla configurazione quorum, vedere [Configurare e gestire il quorum in un cluster di failover di Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx). Per altre informazioni sul cmdlet Set-ClusterQuorum, vedi [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum). 

5. Se crei un cluster esteso a due nodi, devi aggiungere tutta l'archiviazione prima di continuare. A tale scopo, aprire una sessione di PowerShell con autorizzazioni amministrative nei nodi del cluster ed eseguire il comando seguente: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Si tratta del comportamento predefinito in Windows Server 2016.

6. Assicurati che la rete di cluster sia stata configurata correttamente.  
    >[!NOTE]
    > Il ruolo File server deve essere installato in tutti i nodi prima di procedere al passaggio successivo.   |  

7. In **Ruoli**, fare clic su **Configura ruolo**. Rivedere **Prima di iniziare** e fare clic su **Avanti**.  

8. Selezionare **File server** e fare clic su **Avanti**.  

9. Lasciare selezionato **File server per uso generale** e fare clic su **Avanti**.  

10. Fornire un nome del **Punto di accesso client** (15 caratteri o meno) e fare clic su **Avanti**.  

11. Selezionare un disco che diventi il volume di dati e fare clic su **Avanti**.  

12. Rivedere le impostazioni e fare clic su **Avanti**. Fare clic su **Fine**.  

13. Fare clic con il pulsante destro del mouse sul nuovo ruolo file server e scegliere **Aggiungi condivisione file**. Continuare la procedura guidata per configurare le condivisioni.  

14. Facoltativo: aggiungere un altro ruolo file server che usi l'altra risorsa di archiviazione in questo sito.  

15. Configurare il riconoscimento dei siti del cluster esteso in modo che i server SR-SRV01 e SR-SRV02 si trovino nel sito Redmond, SRV03 SR e SR-SRV04 si trovino nel sito Bellevue e Redmond diventi preferito per la proprietà del nodo dell'archiviazione di origine e delle macchine virtuali:  

    ```PowerShell  
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"  
    ```  

      >[!NOTE]
      > Non è possibile configurare il riconoscimento dei siti usando Gestione cluster di failover in Windows Server 2016.  

16. (Facoltativo) Configurare reti di cluster e Active Directory per accelerare il failover del sito DNS. È possibile usare VLAN estese, i dispositivi di astrazione rete, TTL DNS ridotti e altre tecniche comuni.  

Per altre informazioni, rivedere la sessione di Microsoft Ignite [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](http://channel9.msdn.com/events/ignite/2015/brk3487) (Estensione dei cluster di failover e uso della replica di archiviazione in Windows Server vNext) e il post di blog [Enable Change Notifications between Sites - How and Why?](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx) (Abilitare le notifiche di modifica tra i siti - Come e perché?).    

#### <a name="powershell-method"></a>Metodo PowerShell

1. Testare il cluster proposto e analizzare i risultati per assicurarsi di poter continuare:    

    ```PowerShell
    Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04
    ```

    > [!NOTE]
    >  Potrebbero verificarsi errori di archiviazione durante la convalida del cluster a causa dell'uso dell'archiviazione asimmetrica.   

2.  Creare il cluster di calcolo Hyper-V (è necessario specificare l'indirizzo IP statico che verrà usato dal cluster). Assicurarsi che il nome del cluster non superi i 15 caratteri di lunghezza.  Se i nodi si trovano in subnet diverse, è necessario creare un indirizzo IP per il sito aggiuntivo utilizzando la dipendenza "OR". Per altre informazioni [, vedere Configurazione degli indirizzi IP e delle dipendenze per i cluster su più subnet-parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).  

    ```PowerShell
    New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here> 

    Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”

    Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
    ```


3. Configurare un controllo di condivisione file o un controllo cloud (Azure) nel cluster che punta a una condivisione ospitata nel controller di dominio o un altro server indipendente. Ad esempio:  

    ```PowerShell
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare
    ```

    >[!NOTE]
    > Windows Server include ora un'opzione per il cloud Witness con Azure. È possibile scegliere questa opzione di quorum anziché il controllo di condivisione file.  

   Per ulteriori informazioni sulla configurazione del quorum, vedere informazioni sul [cluster e sul quorum del pool](../storage-spaces/understand-quorum.md). Per altre informazioni sul cmdlet Set-ClusterQuorum, vedi [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).

4.  Se crei un cluster esteso a due nodi, devi aggiungere tutta l'archiviazione prima di continuare. A tale scopo, aprire una sessione di PowerShell con autorizzazioni amministrative nei nodi del cluster ed eseguire il comando seguente: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

    Si tratta del comportamento predefinito in Windows Server 2016.

5. Assicurati che la rete di cluster sia stata configurata correttamente.  

6.  Configurare un ruolo File server. Ad esempio:

    ```PowerShell  
    Get-ClusterResource  
    Add-ClusterFileServerRole -Name SR-CLU-FS2 -Storage "Cluster Disk 4"  

    MD e:\share01  

    New-SmbShare -Name Share01 -Path f:\share01 -ContinuouslyAvailable $false  
    ```

7. Configurare il riconoscimento dei siti del cluster esteso in modo che i server SR-SRV01 e SR-SRV02 si trovino nel sito Redmond, SRV03 SR e SR-SRV04 si trovino nel sito Bellevue e Redmond diventi preferito per la proprietà del nodo dell'archiviazione di origine e delle macchine virtuali:  

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"  
    ```

8.  (Facoltativo) Configurare reti di cluster e Active Directory per accelerare il failover del sito DNS. È possibile usare VLAN estese, i dispositivi di astrazione rete, TTL DNS ridotti e altre tecniche comuni.  
    
    Per altre informazioni, rivedere la sessione di Microsoft Ignite [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](http://channel9.msdn.com/events/ignite/2015/brk3487) (Estensione dei cluster di failover e uso della replica di archiviazione in Windows Server vNext) e il post di blog [Enable Change Notifications between Sites - How and Why?](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx) (Abilitare le notifiche di modifica tra i siti - Come e perché?).

### <a name="configure-a-stretch-cluster"></a>Configurare un cluster esteso  
A questo punto si configurerà il cluster esteso usando Gestione cluster di failover o Windows PowerShell. È possibile eseguire tutti i passaggi seguenti nei nodi del cluster direttamente o da un computer di gestione remota che contiene il Strumenti di amministrazione remota del server di Windows Server.  

#### <a name="failover-cluster-manager-method"></a>Metodo Gestione cluster di failover  

1.  Per carichi di lavoro Hyper-V, in un nodo in cui sono presenti i dati che si desidera replicare, aggiungere il disco di dati di origine dai dischi disponibili a volumi condivisi cluster se non è già configurato. Non aggiungere tutti i dischi; è sufficiente aggiungere il disco singolo. A questo punto, la metà dei dischi saranno offline poiché si tratta di archiviazione asimmetrica.  
Se si replica un carico di lavoro di una risorsa Disco fisico come file server per uso generale, si dispone già di un disco collegato al ruolo pronto all'uso.  

    ![Schermata che mostra Gestione cluster di failover](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_OnlineDisks2.png)  

2.  Fare clic con il pulsante destro del mouse sul disco CSV o sul disco collegato al ruolo, quindi su **Replica** e poi **Abilita**.  

3.  Selezionare il volume di dati di destinazione appropriato e fare clic su **Avanti**. Il volume dei dischi di destinazione selezionati sarà uguale a quello del disco di origine selezionato. Durante il passaggio tra queste finestre di dialogo della procedura guidata, anche l'archiviazione verrà spostata automaticamente e, se necessario, portata online in background.  

    ![Schermata che mostra la pagina Selezione disco di destinazione della procedura guidata Configurazione replica archiviazione](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_SelectDestinationDataDisk2.png)  

4.  Selezionare il disco del registro di origine appropriato e fare clic su **Avanti**. Il volume di log di origine deve trovarsi su un disco che usa unità SSD o supporti con la stessa velocità, non su dischi rotanti.  

5.  Selezionare il volume di log di destinazione appropriato e fare clic su Avanti. Il volume dei dischi del registro di destinazione selezionati sarà uguale a quello del disco di origine selezionato.  

6.  Lasciare il valore **Sovrascrivi volume** su **Sovrascrivi volume di destinazione** se il volume di destinazione non contiene una copia precedente dei dati dal server di origine. Se la destinazione contiene dati simili da un backup recente o una replica precedente, selezionare **Disco di destinazione con seeding**, quindi fare clic su **Avanti**.  

7.  Lasciare il valore **Modalità di replica** su **Replica sincrona** se si prevede di usare la replica RPO pari a zero. Modificarla in **Replica asincrona** se si prevede di estendere il cluster su reti a latenza superiore o se si ha bisogno di una latenza minore delle operazioni di I/O sui nodi del sito primario.  
8.  Lasciare il valore **Gruppo di coerenza** su **Massime prestazioni** se non si prevede di usare ordini di scrittura con coppie di dischi aggiuntive nel gruppo di replica. Se si prevede di aggiungere altri dischi a questo gruppo di replica e si richiedono ordini di scrittura garantiti, selezionare **Abilita ordine scrittura** e quindi fare clic su **Avanti**.  

9.  Fare clic su **Avanti** per configurare la replica e la formazione del cluster esteso.  

    ![Schermata che mostra la pagina Select Confirmation (Selezione conferma) della procedura guidata Configurazione replica archiviazione](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ConfigureSR2.png)  

10. Nella schermata di riepilogo, prendere nota dei risultati della finestra di dialogo di completamento. È possibile visualizzare il report in un browser Web.  

11. A questo punto è stata configurata una relazione di Replica archiviazione tra le due metà del cluster, ma la replica è in corso. Esistono diversi modi per visualizzare lo stato della replica tramite uno strumento grafico.  

    1.  Usare la colonna **Ruolo replica** e la scheda **Replica**. Al termine della sincronizzazione iniziale, lo stato della replica dei dischi di origine e di destinazione sarà **Replica continua in corso**.   

        ![Schermata che mostra la scheda Replica di un disco in Gestione cluster di failover](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ReplicationDetails2.png)  

    2.  Avviare **eventvwr.exe**.  

        1.  Nel server di origine andare su **Applicazioni e servizi \ Microsoft \ Windows \ StorageReplica \ Admin** ed esaminare gli eventi 5015, 5002, 5004, 1237, 5001 e 2200.  

        2.  Nel server di destinazione andare su **Applicazioni e servizi \ Microsoft \ Windows \ StorageReplica \ Operational** e attendere l'evento 1215. Questo evento indica il numero di byte copiati e il tempo impiegato. Esempio:  

            ```  
            Log Name:      Microsoft-Windows-StorageReplica/Operational  
            Source:        Microsoft-Windows-StorageReplica  
            Date:          4/6/2016 4:52:23 PM  
            Event ID:      1215  
            Task Category: (1)  
            Level:         Information  
            Keywords:      (1)  
            User:          SYSTEM  
            Computer:      SR-SRV03.Threshold.nttest.microsoft.com  
            Description:  
            Block copy completed for replica.  

            ReplicationGroupName: Replication 2  
            ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}  
            ReplicaName: \\?\Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}\  
            ReplicaId: {00000000-0000-0000-0000-000000000000}  
            End LSN in bitmap:   
            LogGeneration: {00000000-0000-0000-0000-000000000000}  
            LogFileId: 0  
            CLSFLsn: 0xFFFFFFFF  
            Number of Bytes Recovered: 68583161856  
            Elapsed Time (ms): 140  
            ```  

        3.  Nel server di destinazione andare su **Applicazioni e servizi \ Microsoft \ Windows \ StorageReplica \ Admin** ed esaminare gli eventi 5009, 1237, 5001, 5015, 5005 e 2200 per monitorare lo stato di avanzamento dell'elaborazione. Non dovrebbe essere presente alcun avviso di errori in questa sequenza. Ci saranno molti eventi 1237, perché questi indicano lo stato di avanzamento.  

            > [!WARNING]
            > L'utilizzo della CPU e della memoria saranno probabilmente superiori al normale fino al completamento della sincronizzazione iniziale.  

#### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  

1.  Assicurarsi che la console di Powershell sia in esecuzione con un account amministratore con privilegi elevati.  
2.  Aggiungere l'archiviazione di dati di origine solo al cluster come CSV. Per ottenere la dimensione, la partizione e il layout dei volumi dei dischi disponibili, usare i comandi seguenti:  

    ```PowerShell  
    Move-ClusterGroup -Name "available storage" -Node sr-srv01  

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }  
    $DiskResources | foreach {  
        $resource = $_  
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid  

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |  
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining  
    } | FT -AutoSize  

    Move-ClusterGroup -Name "available storage" -Node sr-srv03  

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }  
    $DiskResources | foreach {  
        $resource = $_  
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid  

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |  
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining  
    } | FT -AutoSize  
    ```  

4.  Impostare il disco corretto su CSV con:  

    ```PowerShell  
    Add-ClusterSharedVolume -Name "Cluster Disk 4"  
    Get-ClusterSharedVolume  
    Move-ClusterSharedVolume -Name "Cluster Disk 4" -Node sr-srv01  
    ```  

5.  Configurare il cluster esteso specificando quanto segue:  

    -   I nodi di origine e destinazione (in cui i dati di origine sono un disco CSV e non tutti gli altri dischi).  

    -   Nomi dei gruppi della replica di origine e di destinazione.  

    -   Dischi di origine e destinazione in cui le dimensioni delle partizioni corrispondano.  

    -   Volumi di log di origine e destinazione in cui è presente spazio libero sufficiente per contenere le dimensioni del log su entrambi i dischi e l'archiviazione è su unità SSD o su supporti con velocità simile.  

    -   Volumi di log di origine e destinazione in cui è presente spazio libero sufficiente per contenere le dimensioni del log su entrambi i dischi e l'archiviazione è su unità SSD o su supporti con velocità simile.  

    -   Dimensione log.  

    -   Il volume di log di origine deve trovarsi su un disco che usa unità SSD o supporti con la stessa velocità, non su dischi rotanti.  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv01 -SourceRGName rg01 -SourceVolumeName "C:\ClusterStorage\Volume1" -SourceLogVolumeName e: -DestinationComputerName sr-srv03 -DestinationRGName rg02 -DestinationVolumeName d: -DestinationLogVolumeName e:  
    ```  

    > [!NOTE]  
    > È inoltre possibile usare `New-SRGroup` su un nodo in ogni sito e `New-SRPartnership` per creare la replica in fasi, anziché tutti contemporaneamente.  

6.  Determinare lo stato di avanzamento della replica.  

    1.  Nel server di origine eseguire il comando seguente, quindi esaminare gli eventi 5015, 5002, 5004, 1237, 5001 e 2200:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  Nel server di destinazione, eseguire il comando seguente per visualizzare gli eventi di Replica archiviazione che mostrano la creazione della relazione. Questo evento indica il numero di byte copiati e il tempo impiegato. Esempio:  

            Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  

            TimeCreated  : 4/6/2016 4:52:23 PM  
            ProviderName : Microsoft-Windows-StorageReplica  
            Id           : 1215  
            Message      : Block copy completed for replica.  

               ReplicationGroupName: Replication 2  
               ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}  
               ReplicaName: ?Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}  
               ReplicaId: {00000000-0000-0000-0000-000000000000}  
               End LSN in bitmap:   
               LogGeneration: {00000000-0000-0000-0000-000000000000}  
               LogFileId: 0  
               CLSFLsn: 0xFFFFFFFF  
               Number of Bytes Recovered: 68583161856  
               Elapsed Time (ms): 140  

    3.  Nel server di destinazione, eseguire il comando seguente ed esaminare gli eventi 5009, 1237, 5001, 5015, 5005 e 2200 per comprendere lo stato di avanzamento dell'elaborazione. Non dovrebbe essere presente alcun avviso di errori in questa sequenza. Ci saranno molti eventi 1237, perché questi indicano lo stato di avanzamento.  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

    4.  In alternativa, il gruppo del server di destinazione per la replica indica in qualsiasi momento il numero di byte rimanenti da copiare, inoltre è possibile eseguire query tramite PowerShell. Ad esempio:  

        ```PowerShell  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        Come esempio dello stato di avanzamento (che non terminerà):  

        ```PowerShell  
        while($true) {  

         $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

7.  Per ottenere lo stato di origine e destinazione della replica all'interno del cluster esteso, usare `Get-SRGroup` e `Get-SRPartnership` per visualizzare lo stato configurato della replica nel cluster esteso.  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```  

### <a name="manage-stretched-cluster-replication"></a>Gestire la replica del cluster esteso  
Di seguito sono riportate le indicazioni su come gestire e utilizzare il cluster esteso. È possibile eseguire tutti i passaggi seguenti nei nodi del cluster direttamente o da un computer di gestione remota che contiene il Strumenti di amministrazione remota del server di Windows Server.  

#### <a name="graphical-tools-method"></a>Metodo degli strumenti grafici  

1.  Usare Gestione cluster di failover per determinare l'origine e la destinazione di replica attuale e il relativo stato.  

2.  Per misurare le prestazioni della replica, eseguire **Perfmon.exe** nei nodi di origine e di destinazione.  

    1.  Nel nodo di destinazione:  

        1.  Aggiungere gli oggetti **Statistiche Replica archiviazione** con tutti i relativi contatori delle prestazioni per il volume di dati.  

        2.  Esaminare i risultati.  

    2.  Nel nodo di origine:  

        1.  Aggiungere gli oggetti **Statistiche Replica archiviazione** e **	Statistiche I/O partizione Replica archiviazione** con tutti i relativi contatori delle prestazioni per il volume di dati (quest'ultimo è disponibile solo con i dati nel server di origine attuale).  

        2.  Esaminare i risultati.  

3.  Per modificare l'origine e la destinazione della replica all'interno del cluster esteso, usare i metodi seguenti:  

    1.  Per spostare la replica di origine tra i nodi nello stesso sito: fare clic con il pulsante destro del mouse sull'origine CSV, fare clic su **Sposta archiviazione**, **Seleziona nodo** e quindi selezionare un nodo nello stesso sito. Se si usa l'archiviazione non CSV per un disco a cui è stato assegnato un ruolo, spostare il ruolo.  

    2.  Per spostare la replica di origine da un sito a un altro: fare clic con il pulsante destro del mouse sull'origine CSV, fare clic su **Sposta archiviazione**, **Seleziona nodo** e quindi selezionare un nodo in un altro sito. Se è stato configurato un sito preferito, è possibile usare il miglior nodo possibile per spostare sempre l'archiviazione di origine in un nodo nel sito preferito. Se si usa l'archiviazione non CSV per un disco a cui è stato assegnato un ruolo, spostare il ruolo.  

    3.  Per eseguire il failover pianificato della direzione di replica da un sito a un altro: arrestare entrambi i nodi in un sito usando **ServerManager.exe** o **SConfig**.  

    4.  Per eseguire il failover non pianificato della direzione di replica da un sito a un altro: interrompere l'alimentazione a entrambi i nodi in un sito.  

        > [!NOTE]
        > In Windows Server 2016 potrebbe essere necessario usare Gestione cluster di failover o Move-ClusterGroup per riportare manualmente i dischi di destinazione nell'altro sito dopo che i nodi sono ritornati online.  

        > [!NOTE]
        > Replica archiviazione smonta i volumi di destinazione. Questo comportamento è da progettazione.  

4.  Per modificare le dimensioni del log da 8 GB predefiniti, fare clic con il pulsante destro del mouse sui dischi di log di origine e di destinazione, fare clic sulla scheda **log di replica** , quindi modificare le dimensioni di entrambi i dischi in modo che corrispondano.  

    > [!NOTE]  
    > La dimensione predefinita del registro è 8 GB. In base ai risultati del cmdlet `Test-SRTopology`, è possibile decidere di usare `-LogSizeInBytes` con un valore superiore o inferiore.  

5.  Per aggiungere un'altra coppia di dischi replicati al gruppo di replica esistente, è necessario assicurarsi che sia presente almeno un disco aggiuntivo nell'archiviazione disponibile. È quindi possibile fare clic con il pulsante destro del mouse sul disco di origine e selezionare **Aggiungi relazione di replica**.  
    > [!NOTE]  
    > Questa esigenza di un disco aggiuntivo 'fittizio' all'archiviazione disponibile è dovuta a una regressione e non è intenzionale. In precedenza, Gestione cluster di failover supportava l'aggiunta di più dischi e la supporterà nuovamente in una versione successiva.  


6.  Per rimuovere la replica esistente:  

    1.  Avviare **cluadmin.msc**.  

    2.  Fare clic con il pulsante destro del mouse sul disco CSV di origine e fare clic su **Replica**, quindi su **Rimuovi**. Accettare l'avviso.  

    3.  Facoltativamente, rimuovere la risorsa di archiviazione da CSV per restituirla all'archiviazione disponibile per altri test.  

        > [!NOTE]  
        > Potrebbe essere necessario usare **DiskMgmt.msc** o **ServerManager.exe** per aggiungere le lettere di unità ai volumi dopo essere ritornati all'archiviazione disponibile.  

#### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  

1.  Usare **Get-SRGroup** e **(Get-SRGroup).Replicas** per determinare l'origine e la destinazione di replica attuale e il relativo stato.  

2.  Per misurare le prestazioni della replica, usare il cmdlet `Get-Counter` nei nodi di origine e di destinazione. I nomi dei contatori sono:  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Numero di sospensioni scaricamento  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Numero di I/O in attesa di scaricamento  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Numero richieste ultima scrittura log  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Lunghezza media coda scaricamento  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Lunghezza coda scaricamento corrente  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Numero richieste scrittura applicazione  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Numero medio di richieste per scrittura di log  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Latenza media scrittura app  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Latenza media lettura app  

    -   \Statistiche Replica archiviazione(*)\RPO destinazione  

    -   \Statistiche Replica archiviazione(*)\RPO corrente  

    -   \Statistiche Replica archiviazione(*)\Lunghezza media coda log  

    -   \Statistiche Replica archiviazione(*)\Lunghezza coda log corrente  

    -   \Statistiche Replica archiviazione(*)\Totale byte ricevuti  

    -   \Statistiche Replica archiviazione(*)\Totale byte inviati  

    -   \Statistiche Replica archiviazione(*)\Latenza media invio rete  

    -   \Statistiche Replica archiviazione(*)\Stato replica  

    -   \Statistiche Replica archiviazione(*)\Latenza media round trip messaggio  

    -   \Statistiche Replica archiviazione\Tempo trascorso ultimo ripristino  

    -   \Statistiche Replica archiviazione(*)\Numero di transazioni di ripristino scaricate  

    -   \Statistiche Replica archiviazione(*)\Numero di transazioni di ripristino  

    -   \Statistiche Replica archiviazione(*)\Numero di transazioni di replica scaricate  

    -   \Statistiche Replica archiviazione(*)\Numero di transazioni di replica  

    -   \Statistiche Replica archiviazione(*)\Quantità massima di numeri di sequenza del file di log  

    -   \Statistiche Replica archiviazione(*)\Numero di messaggi ricevuti  

    -   \Statistiche Replica archiviazione(*)\Numero di messaggi inviati  

    Per altre informazioni sui contatori delle prestazioni in Windows PowerShell, vedere [Get-Counter](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Diagnostics/Get-Counter).  

3.  Per modificare l'origine e la destinazione della replica all'interno del cluster esteso, usare i metodi seguenti:  

    1.  Per spostare l'origine della replica da un nodo a un altro nel sito **Redmond**, spostare la risorsa CSV usando il cmdlet Move-ClusterSharedVolume.  

        ```PowerShell  
        Get-ClusterSharedVolume | fl *  
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv02  
        ```  

    2.  Per spostare la direzione della replica da un sito a un altro "pianificato", spostare la risorsa CSV usando il cmdlet **Move-ClusterSharedVolume**.  

        ```PowerShell 
        Get-ClusterSharedVolume | fl *  
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv04  
        ```  

        Questa azione sposterà anche i registri e i dati per gli altri siti e nodi.  

    3.  Per eseguire il failover non pianificato della direzione di replica da un sito a un altro: interrompere l'alimentazione a entrambi i nodi in un sito.  

        > [!NOTE]  
        > Replica archiviazione smonta i volumi di destinazione. Questo comportamento è da progettazione.  

4.  Per modificare le dimensioni del log da 8 GB predefiniti, usare **set-SRGroup** sia nei gruppi di replica di archiviazione di origine che in quello di destinazione.   Ad esempio, per impostare tutti i registri a 2 GB:  

    ```PowerShell  
    Get-SRGroup | Set-SRGroup -LogSizeInBytes 2GB  
    Get-SRGroup  
    ```  

5.  Per aggiungere un'altra coppia di dischi replicati al gruppo di replica esistente, è necessario assicurarsi che sia presente almeno un disco aggiuntivo nell'archiviazione disponibile. È quindi possibile fare clic con il pulsante destro del mouse sul disco di origine e selezionare Aggiungi relazione di replica.  
       >[!NOTE]
       >Questa esigenza di un disco aggiuntivo 'fittizio' all'archiviazione disponibile è dovuta a una regressione e non è intenzionale. In precedenza, Gestione cluster di failover supportava l'aggiunta di più dischi e la supporterà nuovamente in una versione successiva.  

    Usare il cmdlet **Set-SRPartnership** con i parametri **-SourceAddVolumePartnership** e **-DestinationAddVolumePartnership**.  
6.  Per rimuovere la replica, usare `Get-SRGroup`, Get-`SRPartnership`, `Remove-SRGroup` e `Remove-SRPartnership` su ciascun nodo.  

    ```PowerShell  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

    > [!NOTE]
    > Se si usa un computer di gestione remota sarà necessario specificare il nome del cluster per questi cmdlet e fornire i due nomi RG.  

### <a name="related-topics"></a>Argomenti correlati  
- [Panoramica di replica archiviazione](storage-replica-overview.md)  
- [Replica archiviazione da server a server](server-to-server-storage-replication.md)  
- [Replica di archiviazione da cluster a cluster](cluster-to-cluster-storage-replication.md)  
- [Replica archiviazione: problemi noti](storage-replica-known-issues.md) 
- [Replica archiviazione: domande frequenti](storage-replica-frequently-asked-questions.md)  

## <a name="see-also"></a>Vedi anche  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Spazi di archiviazione diretta in Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)
