---
title: Replica di archiviazione da server a server
description: Come configurare e usare la Replica di archiviazione per la replica da server a server in Windows Server, tra cui Windows Admin Center e PowerShell.
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 61881b52-ee6a-4c8e-85d3-702ab8a2bd8c
ms.openlocfilehash: dd0a160213e69e59194e1f775040c12769f1eb5e
ms.sourcegitcommit: 4ff3d00df3148e4bea08056cea9f1c3b52086e5d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64772485"
---
# <a name="server-to-server-storage-replication-with-storage-replica"></a>Replica di archiviazione da server a server con Replica archiviazione

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

È possibile usare Replica di archiviazione per configurare due server per la sincronizzazione dei dati, in modo che ciascuno disponga di una copia identica dello stesso volume. Questo argomento offre informazioni generali sulla replica di archiviazione da server a server, oltre a indicazioni per la configurazione e la gestione dell'ambiente.

Per gestire la Replica di archiviazione è possibile usare [Windows Admin Center](../../manage/windows-admin-center/overview.md) o PowerShell.

Ecco un video di panoramica dell'uso di Replica di archiviazione in Windows Admin Center.
> [!video https://www.microsoft.com/videoplayer/embed/3aa09fd4-867b-45e9-953e-064008468c4b?autoplay=false]


## <a name="prerequisites"></a>Prerequisiti  

* Foresta di Active Directory Domain Services (non necessario per eseguire Windows Server 2016).  
* Due server che eseguono Windows Server 2016, Datacenter Edition o Windows Server 2019. Se si esegue Windows Server 2019, è possibile invece usare Standard Edition se si replicano bene solo un singolo volume fino a 2 TB di dimensioni.  
* Due set di risorse di archiviazione che usano JBOD SAS, SAN Fibre Channel, destinazione iSCSI o archiviazione SATA o SCSI locale. L'archivio deve contenere una combinazione di supporti SSD e HDD. Ogni set di risorse di archiviazione verrà resa disponibile a un solo server senza accesso condiviso.  
* Ogni set di risorse di archiviazione deve consentire la creazione di almeno due dischi virtuali, uno per i dati replicati e uno per i registri. L'archiviazione fisica deve avere le stesse dimensioni di settore su tutti i dischi di dati. L'archiviazione fisica deve avere le stesse dimensioni di settore su tutti i dischi dei registri.  
* Almeno una connessione Ethernet/TCP su ogni server per la replica sincrona, ma preferibilmente RDMA.   
* Regole firewall e router appropriate per consentire ICMP, SMP (porta 445 più 5445 per SMB diretto) e traffico bidirezionale WS-MAN (porta 5985) tra tutti i nodi.  
* Una rete tra i server con larghezza di banda sufficiente per contenere il carico di lavoro di scrittura delle operazioni di I/O e una latenza media di andata e ritorno di =5 ms, per la replica sincrona. La replica asincrona non dispone di un'indicazione di latenza.<br>
Se si esegue la replica tra server locali e macchine virtuali di Azure, è necessario creare un collegamento di rete tra i server locali e macchine virtuali di Azure. A tale scopo, usare [Express Route](#add-azure-vm-expressroute), un [connessione gateway VPN Site-to-Site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal), o installare il software VPN in macchine virtuali di Azure per connetterli con la rete locale.
* L'archiviazione replicata non può trovarsi nell'unità che contiene la cartella del sistema operativo Windows.

> [!IMPORTANT]
>  In questo scenario ogni server deve trovarsi in un'ubicazione fisica o logica differente. Ogni server deve essere in grado di comunicare con l'altro tramite una rete.  


Molti di questi requisiti possono essere determinati mediante il `Test-SRTopology cmdlet`. Se si installano le funzionalità Replica archiviazione o Strumenti di gestione di Replica archiviazione in almeno un server è possibile accedere a questo strumento. Non è necessario configurare Replica archiviazione perché usi questo strumento, ma è necessario installare il cmdlet. Altre informazioni sono incluse nei passaggi seguenti.  

## <a name="windows-admin-center-requirements"></a>Requisiti Windows Admin Center

Per utilizzare contemporaneamente la Replica di archiviazione e di Windows Admin Center, è necessario quanto segue:

| Sistema                        | Sistema operativo                                            | Obbligatorio per     |
|-------------------------------|-------------------------------------------------------------|------------------|
| Due server <br>(qualsiasi combinazione di hardware locale, le macchine virtuali e cloud VM tra cui macchine virtuali di Azure)| Windows Server 2019, Windows Server 2016 o Windows Server (canale semestrale) | Replica archiviazione  |
| Un PC                     | Windows 10                                                  | Windows Admin Center |

> [!NOTE]
> A questo punto è possibile usare Windows Admin Center in un server per gestire la Replica di archiviazione.

## <a name="terms"></a>Condizioni  
Questa procedura dettagliata usa l'ambiente seguente come esempio:  

-   Due server, denominati **SR-SRV05** e **SR SRV06**.  

-   Una coppia di "siti" logici che rappresentano due centri dati diversi, uno chiamato **Redmond** e l'altro **Bellevue**.  

![Diagramma che mostra la replica di un server in Building 5 con un server in Building 9](media/Server-to-Server-Storage-Replication/Storage_SR_ServertoServer.png)  

**Figura 1: Server di replica di server**  

## <a name="step-1-install-and-configure-windows-admin-center-on-your-pc"></a>Passaggio 1: Installare e configurare Windows Admin Center sul PC

Se si usa Windows Admin Center per gestire la Replica di archiviazione, usare la procedura seguente per preparare il PC per gestire la Replica di archiviazione.
1. Scaricare e installare [Windows Admin Center](../../manage/windows-admin-center/overview.md).
2. Scaricare e installare il [strumenti di amministrazione remota del Server](https://www.microsoft.com/download/details.aspx?id=45520).
    - Se si usa Windows 10, versione 1809 o versioni successive, installare la "amministrazione remota del server: Replica modulo di archiviazione per Windows PowerShell"dalla funzionalità su richiesta.
3. Aprire una sessione di PowerShell come amministratore, selezionare la **avviare** button, digitando **PowerShell**, facendo **Windows PowerShell,** e quindi selezionando  **Esegui come amministratore**.
4. Immettere il comando seguente per abilitare il protocollo WS-Management nel computer locale e impostare la configurazione predefinita per la gestione remota sul client.

    ```PowerShell
    winrm quickconfig
    ```

5. Tipo di **Y** per abilitare i servizi di gestione remota Windows e abilitare l'eccezione del Firewall per WinRM.

## <a name="provision-os"></a>Passaggio 2: Effettuare il provisioning di sistema operativo, funzionalità, ruoli, archiviazione e rete

1.  Installare Windows Server su entrambi i nodi del server con un tipo di installazione di Windows Server **(esperienza Desktop)** . 
 
    Per usare una macchina virtuale di Azure connessa alla rete tramite un circuito ExpressRoute, vedere [aggiunta di una VM di Azure connessa alla rete tramite ExpressRoute](#add-azure-vm-expressroute).

3.  Aggiungere le informazioni di rete, aggiungere i server allo stesso dominio come la gestione di Windows 10 PC (se si usa uno) e quindi riavviare i server.  

    > [!NOTE]
    > Da questo momento, accedere sempre a tutti i server come utente di dominio, ovvero un membro del gruppo amministratore incorporato. Ricordarsi di elevare i prompt di PowerShell e CMD futuro durante l'esecuzione in un'installazione server con interfaccia grafica o in un computer Windows 10.  

3.  Il primo set di enclosure di archiviazione JBOD, destinazione iSCSI, SAN FC o archiviazione disco a dimensione fissa locale (DAS) di connettersi al server nel sito **Redmond**.  

4.  Connettere il secondo set di archiviazione per il server nel sito **Bellevue**.  

5.  Se necessario, installare il sistema di archiviazione del fornitore e il firmware dell'enclosure più recente, i driver HBA del fornitore più recenti, i firmware BIOS o UEFI del fornitore più recenti, i driver di rete del fornitore più recenti e driver dei chipset della scheda madre più recenti in entrambi i nodi. Se necessario riavviare i nodi.  

    > [!NOTE]
    > Per la configurazione dell'archiviazione condivisa e dell'hardware di rete, consultare la documentazione del fornitore hardware.  

6.  Assicurarsi che le impostazioni BIOS/UEFI dei server abilitati consentano alte prestazioni, ad esempio la disabilitazione dei C-State, l'impostazione della velocità QPI, l'abilitazione di NUMA e l'impostazione della frequenza di memoria massima. Verificare che il risparmio di energia in Windows Server è impostato su prestazioni elevate. Riavviare se necessario.  

7.  Configurare i ruoli come indicato di seguito:  

    -   **Metodo Windows Admin Center**
        1. In Windows Admin Center, passare a Server Manager e quindi selezionare uno dei server.
        2. Passare a **ruoli e funzionalità**.
        3. Selezionare **caratteristiche** > **Replica di archiviazione**, quindi fare clic su **installare**.
        4. Ripetere su altro server.
    -   **Metodo di Server Manager**  

        1.  Eseguire **ServerManager.exe** e creare un gruppo di server aggiungendo tutti i nodi del server.  

        2.  Installare i ruoli e le funzionalità **File Server** e **Replica archiviazione** in ciascuno dei nodi e riavviarli.  

    -   **Metodo Windows PowerShell**  

        In un computer di gestione remota o SR-SRV06, eseguire il comando seguente nella console di Windows PowerShell per installare i ruoli e funzionalità necessarie e riavviarli:  

        ```powershell  
        $Servers = 'SR-SRV05','SR-SRV06'  

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,FS-FileServer -IncludeManagementTools -restart }  
        ```  

        Per altre informazioni su questi passaggi, vedere [Installazione o disinstallazione di ruoli, servizi ruolo o funzionalità](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)  

8.  Configurare lo spazio di archiviazione come indicato di seguito:  

    > [!IMPORTANT]  
    > -   È necessario creare due volumi in ciascuna enclosure: uno per i dati e uno per i registri.  
    > -   I dischi di dati e registro devono essere inizializzati come GPT, non come MBR.  
    > -   I due volumi di dati devono avere le stesse dimensioni.  
    > -   I due volumi di registro devono avere le stesse dimensioni.  
    > -   Tutti i dischi di dati replicati devono avere le stesse dimensioni del settore.  
    > -   Tutti i dischi di registro devono avere le stesse dimensioni del settore.  
    > -   I volumi di registro devono usare un'archiviazione basata su flash, ad esempio le unità SSD. Microsoft consiglia una velocità di archiviazione dei log superiore a quella dell'archiviazione dei dati. I volumi di log non devono essere utilizzati per altri carichi di lavoro.
    > -   I dischi di dati possono usare HDD, SSD o una combinazione a più livelli e spazi con mirroring o di parità o RAID 1 o 10, oppure RAID 5 o RAID 50.  
    > -   Il volume di log deve essere di almeno 9 GB per impostazione predefinita e può essere superiore o inferiore in base ai requisiti del log.  
    > -   Il ruolo File Server è necessario solo per il funzionamento di Test-SRTopology, in quanto apre le porte del firewall necessarie per il test.
    
    - **Per le enclosure JBOD:**  

        1.  Assicurarsi che ogni server possa visualizzare solo gli alloggiamenti di archiviazione del sito e che le connessioni SAS siano configurate correttamente.  

        2.  Effettuare il provisioning dell'archiviazione mediante Spazi di archiviazione seguendo i **passaggi 1-3** indicati in [Distribuire Spazi di archiviazione in un server autonomo](../storage-spaces/deploy-standalone-storage-spaces.md) usando Windows PowerShell o Server Manager.  

    - **Per l'archiviazione iSCSI:**  

        1.  Assicurarsi che ogni cluster possa visualizzare solo gli alloggiamenti di archiviazione di tale sito. Se si usa iSCSI, è necessario usare più di una singola scheda di rete.    

        2.  Effettua il provisioning dell'archiviazione usando la documentazione del fornitore. Se si usa la destinazione iSCSI basata su Windows, consultare [iSCSI Target Block Storage, How To](../iscsi/iscsi-target-server.md) (Procedura per l'archiviazione a blocchi nella destinazione iSCSI).  

    - **Per l'archiviazione FC SAN:**  

        1.  Assicurarsi che ogni cluster sia in grado di vedere solo gli alloggiamenti di archiviazione di tale sito e che gli host siano stati suddivisi correttamente in zone.   

        2.  Effettua il provisioning dell'archiviazione usando la documentazione del fornitore.  

    - **Per l'archiviazione disco a dimensione fissa locale:**  

        -   Verificare che lo spazio di archiviazione non contenga un volume di sistema, file di paging, o file dump.  

        -   Effettua il provisioning dell'archiviazione usando la documentazione del fornitore.  

9. Avviare Windows PowerShell e usare il cmdlet **Test-SRTopology** per determinare se siano stati soddisfatti tutti i requisiti di Replica archiviazione. È possibile usare il cmdlet in modalità dedicata ai requisiti per un rapido test, oppure in modalità di valutazione delle prestazioni a esecuzione prolungata.  

    Ad esempio, per convalidare i nodi proposti che presentano un volume **F:** e **G:** ed eseguire il test per 30 minuti:  
        
    ```PowerShell
    MD c:\temp  

    Test-SRTopology -SourceComputerName SR-SRV05 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV06 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp  
    ```

    > [!IMPORTANT]
      > Quando si usa un server di prova privo di carichi di operazioni I/O di scrittura nel volume di origine specificato durante il periodo di valutazione, considerare l'aggiunta di un carico di lavoro o non verrà generato un report utile. È necessario eseguire il test con carichi di lavoro simili alla produzione per poter visualizzare i numeri reali e le dimensioni consigliate del registro. In alternativa, è sufficiente copiare alcuni file nel volume di origine durante il test o scaricare ed eseguire  [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) per generare operazioni di I/O di scrittura. Ad esempio, un campione con un carico di lavoro di I/O di scrittura basso per dieci minuti per il volume D: come mostrato di seguito:  
      >
      > `Diskspd.exe -c1g -d600 -W5 -C5 -b8k -t2 -o2 -r -w5 -i100 d:\test` 

10. Esaminare i **Testsrtopologyreport** report illustrato nella figura 2 per garantire che siano soddisfatti i requisiti di Replica di archiviazione.  

    ![Schermata che mostra il report della topologia](media/Server-to-Server-Storage-Replication/SRTestSRTopologyReport.png)

    **Figura 2: Report della topologia di replica archiviazione**

## <a name="step-3-set-up-server-to-server-replication"></a>Passaggio 3: Configurare la replica da server a server
### <a name="using-windows-admin-center"></a>Utilizzando Windows Admin Center

1. Aggiungere il server di origine.
    1. Selezionare il **Add** pulsante.
    2. Selezionare **Aggiungi connessione server**.
    3. Digitare il nome del server e quindi selezionare **Submit**.
2. Nel **tutte le connessioni** pagina, selezionare il server di origine.
3. Selezionare **Replica di archiviazione** dal Pannello di strumenti.
4. Selezionare **New** per creare una nuova relazione.
5. Fornire i dettagli della relazione e quindi selezionare **Create**. <br>
![La schermata nuova Partnership che mostra i dettagli di collaborazione, ad esempio una dimensione del Registro di 8 GB.](media\Storage-Replica-UI\Honolulu_SR_Create_Partnership.png)

    **Figura 3: Creazione di una nuova relazione**

> [!NOTE]
> Rimuovendo la partnership di Replica di archiviazione in Windows Admin Center non rimuove il nome del gruppo di replica.

### <a name="using-windows-powershell"></a>Tramite Windows PowerShell

Ora è necessario configurare la replica da server a server con PowerShell. È necessario eseguire tutti i passaggi seguenti sui nodi direttamente o da un computer di gestione remota che contiene strumenti di amministrazione di Windows Server Remote Server.  

1. Assicurarsi di utilizzare una console di Powershell con privilegi elevati in qualità di amministratore.  
2. Configurare la replica da server a server, specificando i dischi di origine e di destinazione, i registri di origine e di destinazione, i nodi di origine e di destinazione e le dimensioni del registro.  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv05 -SourceRGName rg01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName sr-srv06 -DestinationRGName rg02 -DestinationVolumeName f: -DestinationLogVolumeName g:  
    ```  

   Output:
   ```PowerShell
   DestinationComputerName : SR-SRV06
   DestinationRGName       : rg02
   SourceComputerName      : SR-SRV05
   PSComputerName          :
   ```

    > [!IMPORTANT]
    > La dimensione predefinita del registro è 8 GB. In base ai risultati del cmdlet `Test-SRTopology`, è possibile decidere di usare - LogSizeInBytes con un valore superiore o inferiore.  

2.  Per ottenere lo stato di origine e destinazione della replica, usare `Get-SRGroup` e `Get-SRPartnership` come indicato di seguito:  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```
    Output:

    ```PowerShell
    CurrentLsn             : 0
    DataVolume             : F:\
    LastInSyncTime         :
    LastKnownPrimaryLsn    : 1
    LastOutOfSyncTime      :
    NumOfBytesRecovered    : 37731958784
    NumOfBytesRemaining    : 30851203072
    PartitionId            : c3999f10-dbc9-4a8e-8f9c-dd2ee6ef3e9f
    PartitionSize          : 68583161856
    ReplicationMode        : synchronous
    ReplicationStatus      : InitialBlockCopy
    PSComputerName         :
    ```

3.  Determinare lo stato della replica come indicato di seguito:  

    1.  Nel server di origine eseguire il comando seguente, quindi esaminare gli eventi 5015, 5002, 5004, 1237, 5001 e 2200:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  Nel server di destinazione, eseguire il comando seguente per visualizzare gli eventi di Replica archiviazione che mostrano la creazione della relazione. Questo evento indica il numero di byte copiati e il tempo impiegato. Esempio:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  
        ```
        Ecco un output di esempio:
        ```
        TimeCreated  : 4/8/2016 4:12:37 PM  
        ProviderName : Microsoft-Windows-StorageReplica  
        Id           : 1215  
        Message      : Block copy completed for replica.  

        ReplicationGroupName: rg02  
        ReplicationGroupId: {616F1E00-5A68-4447-830F-B0B0EFBD359C}  
        ReplicaName: f:\  
        ReplicaId: {00000000-0000-0000-0000-000000000000}  
        End LSN in bitmap:   
        LogGeneration: {00000000-0000-0000-0000-000000000000}  
        LogFileId: 0  
        CLSFLsn: 0xFFFFFFFF  
        Number of Bytes Recovered: 68583161856  
        Elapsed Time (ms): 117  
        ```  

        > [!NOTE]
        > Replica archiviazione smonta i volumi di destinazione e i relativi punti di montaggio o lettere dell'unità. Questo comportamento è da progettazione.  

    3.  In alternativa, il gruppo del server di destinazione per la replica indica in qualsiasi momento il numero di byte rimanenti da copiare, inoltre è possibile eseguire query tramite PowerShell. Ad esempio:  

        ```PowerShell  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        Come esempio dello stato di avanzamento (che non terminerà):  

        ```PowerShell  
        while($true) {  

         $v = (Get-SRGroup -Name "RG02").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

    4.  Nel server di destinazione, eseguire il comando seguente ed esaminare gli eventi 5009, 1237, 5001, 5015, 5005 e 2200 per comprendere lo stato di avanzamento dell'elaborazione. Non dovrebbe essere presente alcun avviso di errori in questa sequenza. Ci saranno molti eventi 1237, perché questi indicano lo stato di avanzamento.  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

## <a name="step-4-manage-replication"></a>Passaggio 4: Gestire la replica

Ora, proseguire con la gestione e l'uso dell'infrastruttura replicata da server a server. È possibile eseguire tutti i passaggi seguenti sui nodi direttamente o da un computer di gestione remota che contiene strumenti di amministrazione di Windows Server Remote Server.  

1.  Usare `Get-SRPartnership` e `Get-SRGroup` per determinare l'origine e la destinazione di replica attuale e il relativo stato.  

2.  Per misurare le prestazioni della replica, usare il cmdlet `Get-Counter` nei nodi di origine e di destinazione. I nomi dei contatori sono:  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Numero di sospensioni scaricamento  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Numero di I/O in attesa di scaricamento  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Numero richieste ultima scrittura log  

    -   \Statistiche I/O partizione Replica archiviazione(*)\ Lunghezza media coda scaricamento  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Lunghezza coda scaricamento corrente  

    -   \Statistiche I/O partizione Replica archiviazione(*)\Numero richieste scrittura applicazione  

    -   \Statistiche I/O partizione Replica archiviazione(*)\ Numero medio di richieste per scrittura di log  

    -   \Statistiche I/O partizione Replica archiviazione(*)\ Latenza media scrittura app  

    -   \Statistiche I/O partizione Replica archiviazione(*)\ Latenza media lettura app  

    -   \Statistiche Replica archiviazione(*)\RPO destinazione  

    -   \Statistiche Replica archiviazione(*)\RPO corrente  

    -   \Statistiche Replica archiviazione(*)\ Lunghezza media coda log  

    -   \Statistiche Replica archiviazione(*)\Lunghezza coda log corrente  

    -   \Statistiche Replica archiviazione(*)\Totale byte ricevuti  

    -   \Statistiche Replica archiviazione(*)\Totale byte inviati  

    -   \Statistiche Replica archiviazione(*)\ Latenza media invio rete  

    -   \Statistiche Replica archiviazione(*)\Stato replica  

    -   \Statistiche Replica archiviazione(*)\ Latenza media round trip messaggio  

    -   \Statistiche Replica archiviazione\Tempo trascorso ultimo ripristino  

    -   \Statistiche Replica archiviazione(*)\Numero di transazioni di ripristino scaricate  

    -   \Statistiche Replica archiviazione(*)\Numero di transazioni di ripristino  

    -   \Statistiche Replica archiviazione(*)\Numero di transazioni di replica scaricate  

    -   \Statistiche Replica archiviazione(*)\Numero di transazioni di replica  

    -   \Statistiche Replica archiviazione(*)\Quantità massima di numeri di sequenza del file di log  

    -   \Statistiche Replica archiviazione(*)\Numero di messaggi ricevuti  

    -   \Statistiche Replica archiviazione(*)\Numero di messaggi inviati  

    Per altre informazioni sui contatori delle prestazioni in Windows PowerShell, vedere [Get-Counter](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Diagnostics/Get-Counter).  

3.  Per spostare la direzione di replica da un sito, usare il cmdlet `Set-SRPartnership`.  

    ```PowerShell  
    Set-SRPartnership -NewSourceComputerName sr-srv06 -SourceRGName rg02 -DestinationComputerName sr-srv05 -DestinationRGName rg01  
    ```  

    > [!WARNING]  
    > Windows Server impedisce il cambio di ruolo durante la sincronizzazione iniziale, perché può causare la perdita di dati se si tenta di cambio prima del completamento della replica iniziale. Non forzare il cambio delle direzioni fino al completamento della sincronizzazione iniziale.  

    Controllare i registri eventi per visualizzare la direzione della modalità di modifica e ripristino della replica e quindi risolvere le differenze. Le operazioni di I/O di scrittura possono essere effettuate nell'archiviazione di proprietà nel nuovo server di origine. La modifica la direzione di replica blocca le operazioni di I/O di scrittura nel computer di origine precedente.  

4.  Per rimuovere la replica, usare `Get-SRGroup`, `Get-SRPartnership`, `Remove-SRGroup` e `Remove-SRPartnership` su ciascun nodo. Assicurarsi di eseguire il cmdlet `Remove-SRPartnership` solo sull'origine della replica corrente, non nel server di destinazione. Eseguire `Remove-Group` su entrambi i server. Ad esempio, per rimuovere completamente la replica dai due server:  

    ```powershell
    Get-SRPartnership  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

## <a name="replacing-dfs-replication-with-storage-replica"></a>Sostituzione di Replica DFS con Replica archiviazione  
Molti clienti Microsoft distribuiscono Replica DFS come soluzione di ripristino di emergenza per i dati utente non strutturati, come home directory e condivisioni di reparto. Replica DFS è disponibile in Windows Server 2003 R2 e in tutti i sistemi operativi successivi e opera su reti a larghezza di banda ridotta. Risulta quindi interessante per gli ambienti di modifica a bassa e alta latenza con più nodi. Come soluzione di replica dei dati, tuttavia, Replica DFS presenta limitazioni significative:  
* File in uso o aperti non vengono replicate.  
* Non vengono replicate in modo sincrono.  
* La latenza di replica asincrona può durare molti minuti, ore o persino giorni.  
* Si basa su un database che può richiedere verifiche di coerenza di lunga durata dopo un'interruzione dell'alimentazione.  
* È in genere configurato come multi-master, che consente di apportare modifiche al flusso in entrambe le direzioni, eventualmente sovrascrivendo i dati più recenti.  

Replica archiviazione non presenta alcuna di queste limitazioni. Tuttavia, presenta diversi problemi che potrebbero renderla meno interessante in alcuni ambienti:  

* Consente solo la replica uno a uno tra i volumi. È possibile replicare volumi diversi tra più server.  
* Anche se supporta la replica asincrona, non è progettato per la larghezza di banda ridotta, reti a latenza elevata.  
* Questa funzionalità non consente l'accesso utente ai dati protetti nella destinazione mentre è in corso la replica  

Se questi fattori non rappresentano un limite, Replica di archiviazione consente di sostituire i server di Replica DFS con questa tecnologia più recente.   
Il processo è così suddiviso a un livello elevato:  

1.  Installare Windows Server in due server e configurare l'archiviazione. Questo potrebbe comportare l'aggiornamento di un set esistente di server o la relativa installazione.  
2.  Verificare l'esistenza di tutti i dati che si desidera replicare in uno o più volumi di dati e non sull'unità C:.   
a.  È inoltre possibile effettuare il seeding dei dati sull'altro server per risparmiare tempo usando un backup o copie del file, oppure sfruttare l'archiviazione con thin provisioning. A differenza di Replica DFS, non è necessario che la sicurezza analoga a quella dei metadati corrisponda perfettamente.  
3.  Condividere i dati nel server di origine e renderli accessibili tramite uno spazio dei nomi DFS. Questa azione è importante per garantire che gli utenti possano comunque accedervi se il nome del server viene modificato in un sito di emergenza.  
a.  È possibile creare condivisioni corrispondenti nel server di destinazione, che non sarà disponibile durante le normali operazioni.   
b.  Non aggiungere il server di destinazione allo spazio dei nomi di spazi dei nomi DFS, o tal caso, assicurarsi che tutte le destinazioni cartella siano disabilitate.  
4.  Abilitare la replica di Replica archiviazione e completare la sincronizzazione iniziale. La replica può essere sincrona o asincrona.   
a.  Tuttavia, è consigliabile la replica sincrona per garantire la coerenza dei dati di I/O nel server di destinazione.   
b.  È consigliabile abilitare il servizio Copia Shadow del volume ed eseguire periodicamente gli snapshot con VSSADMIN o altri strumenti. Ciò garantisce che le applicazioni scarichino i propri file di dati su disco in modo coerente. In caso di emergenza, è possibile ripristinare i file dagli snapshot nel server di destinazione che potrebbe essere stato parzialmente replicato in modo asincrono. Gli snapshot vengono replicati insieme ai file.  
5.  Usare normalmente fino a quando non si verifica una situazione di emergenza.  
6.  Fare in modo che il server di destinazione diventi la nuova origine, la quale riflette i volumi replicati agli utenti.  
7.  Se si usa la replica sincrona, il ripristino dei dati non sarà necessario a meno che l'utente non stia usando un'applicazione che sta scrivendo i dati senza la protezione delle transazioni, indipendentemente dalla replica, durante la perdita del server di origine. Se si usa la replica asincrona la necessità di un montaggio snapshot VSS sarà maggiore, ma è consigliabile usare VSS in tutte le circostanze per gli snapshot coerenti con l'applicazione.  
8.  Aggiungere il server e le sue azioni come destinazione cartella spazi dei nomi DFS.   
9.  Gli utenti possono quindi accedere ai dati.  

 > [!NOTE]
 > La pianificazione del ripristino di emergenza è un argomento complesso e che richiede particolare attenzione ai dettagli. È consigliabile creare runbook ed eseguire annualmente le esercitazioni di failover in tempo reale. Quando si verifica un'emergenza effettiva, regnerà il caos e il personale esperto potrebbe non essere disponibile.  

## <a name="add-azure-vm-expressroute"></a>Aggiunta di una VM di Azure connessa alla rete tramite ExpressRoute

1. [Creare un circuito ExpressRoute nel portale di Azure](https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager).<br>Dopo l'approvazione di ExpressRoute, un gruppo di risorse viene aggiunto alla sottoscrizione, passare a **gruppi di risorse** per visualizzare questo nuovo gruppo. Prendere nota del nome di rete virtuale.
![Portale di Azure con il gruppo di risorse aggiunto con ExpressRoute](media/Server-to-Server-Storage-Replication/express-route-resource-group.png)
    
    **Figura 4: Le risorse associate a un circuito ExpressRoute: prendere nota del nome di rete virtuale**
1. [Creare un nuovo gruppo di risorse](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).
1. [Aggiungere un gruppo di sicurezza di rete](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal). Momento della creazione, selezionare l'ID sottoscrizione associato di ExpressRoute è stato creato e selezionare il gruppo di risorse che appena creato anche.
<br><br>Aggiungere eventuali regole di sicurezza in ingresso e in uscita che è necessario per il gruppo di sicurezza di rete. Ad esempio, è possibile consentire l'accesso Desktop remoto alla macchina virtuale.
1. [Creare una VM di Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) con le impostazioni seguenti (illustrate nella figura 5):
    - **Indirizzo IP pubblico**: Nessuno
    - **Rete virtuale**: Selezionare la rete virtuale che è stata eseguita nota del gruppo di risorse aggiunte con ExpressRoute.
    - **Gruppo di sicurezza di rete (firewall)** : Selezionare il gruppo di sicurezza di rete creato in precedenza.
    ![Crea macchina virtuale che mostra le impostazioni di rete di ExpressRoute](media/Server-to-Server-Storage-Replication/azure-vm-express-route.png)
    **figura 5: Creazione di una macchina virtuale durante la selezione delle impostazioni di rete di ExpressRoute**
1. Dopo aver creata la macchina virtuale, vedere [passaggio 2: Effettuare il provisioning del sistema operativo, funzionalità, ruoli, archiviazione e rete](#provision-os).


## <a name="related-topics"></a>Argomenti correlati  
- [Panoramica di Replica di archiviazione](storage-replica-overview.md)  
- [Replica di Cluster esteso tramite l'archiviazione condivisa](stretch-cluster-replication-using-shared-storage.md)  
- [Replica di archiviazione da cluster a Cluster](cluster-to-cluster-storage-replication.md)
- [Replica di archiviazione: Problemi noti](storage-replica-known-issues.md)  
- [Replica di archiviazione: domande frequenti](storage-replica-frequently-asked-questions.md)
- [Spazi di archiviazione diretta in Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
