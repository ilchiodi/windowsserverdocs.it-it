---
title: Distribuzione di due nodi file server in cluster
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/01/2019
description: Questo articolo descrive la creazione di un cluster di due nodi file server
ms.openlocfilehash: 9f50470b379bd0ab05834eb3c5a35be0f5e9e93a
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453004"
---
# <a name="deploying-a-two-node-clustered-file-server"></a>Distribuzione di due nodi file server in cluster

> Si applica a: Windows Server 2019, Windows Server 2016

Un cluster di failover è costituito da un gruppo di computer indipendenti che interagiscono tra di loro per migliorare la disponibilità di servizi e applicazioni. I server inclusi nel cluster (detti nodi) sono connessi mediante cavi fisici e software. Se in uno dei nodi si verifica un errore, il servizio verrà garantito da un altro nodo tramite un processo denominato failover. Per gli utenti l'interruzione del servizio risulterà minima.

Questa guida descrive i passaggi per installare e configurare un cluster di failover server di file generico che ha due nodi. Creando la configurazione in questa Guida, è possibile conoscere i cluster di failover e acquisire familiarità con l'interfaccia di snap-in Gestione Cluster di Failover in Windows Server 2016 o Windows Server 2019.

## <a name="overview-for-a-two-node-file-server-cluster"></a>Panoramica di un cluster di file server a due nodi

Server in un cluster di failover possono funzionare in un'ampia gamma di ruoli, inclusi i ruoli del file server, Hyper-V server o server di database e possono garantire disponibilità elevata per un'ampia gamma di altri servizi e applicazioni. In questa guida viene illustrato come configurare un cluster di file server a due nodi.

Un cluster di failover include in genere un'unità di archiviazione connessa fisicamente a tutti i server del cluster, sebbene ogni volume dell'archiviazione sia accessibile solo da un server alla volta. Nel diagramma seguente viene mostrato un cluster di failover a due nodi connesso a un'unità di archiviazione.

![Cluster a due nodi](media/Cluster-File-Server/Cluster-FS-Overview.png)

I volumi di archiviazione o i numeri di unità logica (LUN) esposti ai nodi in un cluster non devono essere esposti ad altri server, inclusi i server in un altro cluster, come illustrato nel diagramma seguente.

![Unità logiche di archiviazione](media/Cluster-File-Server/Cluster-FS-LUNs.png)

Per ottenere la massima disponibilità di qualsiasi server, è importante seguire le procedure consigliate per la gestione dei server, che prevedono ad esempio di gestire in modo oculato l'ambiente fisico dei server, eseguire test sulle modifiche del software prima di implementarle e tenere traccia in modo efficace degli aggiornamenti software e delle modifiche alla configurazione in tutti i server del cluster.

Lo scenario seguente illustra il modo in cui può essere configurato un cluster di failover di file server. I file condivisi si trovano nell'archivio del cluster e qualsiasi server del cluster può fungere da file server che ne consente la condivisione.

## <a name="shared-folders-in-a-failover-cluster"></a>Cartelle condivise in un cluster di failover

L'elenco seguente descrive la funzionalità di configurazione cartella condivisa che è integrata nel clustering di failover:

- Visualizzazione limitata alle cartelle condivise in cluster (nessuna combinazione con le cartelle condivise non in cluster): Quando un utente visualizza le cartelle condivise specificando il percorso del file server in cluster, la visualizzazione includerà solo le cartelle condivise che fanno parte del ruolo del server di file specifico. Vengono escluse le cartelle condivise non in cluster e parte le condivisioni file separato dei ruoli del server che si trovano in un nodo del cluster.

- Enumerazione basata sull'accesso: l'enumerazione basata sull'accesso consente di nascondere una cartella specifica nella visualizzazione degli utenti. Anziché consentire agli utenti di visualizzare la cartella senza potervi accedere, è possibile evitare che la vedano. È possibile configurare l'enumerazione basata sull'accesso per una cartella condivisa in cluster nello stesso modo a quella di una cartella condivisa non in cluster.

- Accesso offline: l'accesso non in linea, ovvero la memorizzazione nella cache, viene configurato nello stesso modo sia per una cartella condivisa in cluster che per una cartella condivisa non in cluster.

- I dischi del cluster vengono sempre riconosciuti come parte del cluster: Se si usa l'interfaccia del cluster di failover, Esplora Windows o lo snap-in Gestione condivisione e archiviazione, Windows rileva se un disco è stato designato come se fossero all'archiviazione cluster. Se è già stato configurato un disco in Gestione Cluster di Failover come parte di un file server in cluster, è possibile utilizzare quindi una delle interfacce citate in precedenza per creare una condivisione nel disco. Se il disco non è stato configurato come parte di un file server del cluster, non sarà possibile creare accidentalmente una condivisione nel disco. Verrà infatti visualizzato un messaggio di errore in cui viene indicato che prima di condividere il disco è necessario configurarlo come parte di un file server del cluster.

- Integrazione dei servizi per NFS: Il ruolo File Server in Windows Server include il servizio ruolo facoltativo denominato servizi per Network File System (NFS). Se si installa il servizio ruolo e si configurano cartelle condivise mediante Servizi per NFS, sarà possibile creare un file server del cluster che supporti i client basati su UNIX.

## <a name="requirements-for-a-two-node-failover-cluster"></a>Requisiti di un cluster di failover a due nodi

Per un cluster di failover in Windows Server 2016 o Windows Server 2019 per essere considerato una soluzione supportata ufficialmente da Microsoft, la soluzione deve soddisfare i criteri seguenti.

- Tutti i componenti hardware e software devono soddisfare i requisiti di qualifica per il logo appropriato. Per Windows Server 2016, questo è il logo "Certified for Windows Server 2016". Per Windows Server 2019, questo è il logo "Certified for Windows Server 2019". Per ulteriori informazioni su quali sistemi hardware e software hanno ottenuto la certificazione, visita il Microsoft [catalogo di Windows Server](https://www.windowsservercatalog.com/default.aspx) sito.

- La soluzione configurazione completa (server, rete e archiviazione) deve superare tutti i test della procedura guidata convalida, che fa parte dello snap-in cluster di failover.

Sarà necessario quanto segue per un cluster di failover a due nodi.

- **Server:** È consigliabile usare i computer corrispondenti con i componenti uguali o simili.  I server per un cluster di failover a due nodi devono eseguire la stessa versione di Windows Server. Dispongono anche gli stessi aggiornamenti software (patch).

- **Cavo e adattatori di rete:** Hardware di rete, come gli altri componenti nella soluzione cluster di failover, deve essere compatibile con Windows Server 2016 o Windows Server 2019. Se si usa iSCSI, le schede di rete devono essere dedicate alle comunicazioni di rete o a iSCSI, non entrambi. Evitare singoli punti di errore nell'infrastruttura di rete utilizzata per la connessione dei nodi del cluster. A tale scopo, è possibile eseguire diverse procedure. È possibile connettere i nodi cluster con più reti distinte. In alternativa, è possibile connettere i nodi cluster con una rete costruita con schede di rete in gruppo, commutatori ridondanti, router ridondanti o hardware simile che rimuova singoli punti di errore.

   > [!NOTE]
   > Se i nodi del cluster sono connesse con una singola rete, la rete supererà il requisito di ridondanza nella convalida guidata configurazione.  Tuttavia, il report includerà un avviso che la rete deve essere presente un singolo punto di guasto.

- **I controller dei dispositivi o adattatori appropriati per l'archiviazione:**
    - **Serial Attached SCSI o Fibre Channel:** in caso di utilizzo di SAS o Fibre Channel, in tutti i server del cluster, è preferibile che tutti i componenti dello stack di archiviazione siano identici. È necessario che il software multipath i/o (MPIO) e i componenti software di modulo specifico dispositivo (DSM) essere identici.  È consigliabile che i controller dei dispositivi di archiviazione di massa, ossia la scheda bus host (HBA) e i relativi driver e firmware, collegati all'archiviazione cluster siano identici. Se si utilizzano schede HBA diverse, è opportuno verificare con il fornitore dell'archiviazione che vengano eseguite le configurazioni supportate o consigliate.
    - **iSCSI:** Se si usa iSCSI, ogni server del cluster deve avere uno o più schede di rete o HBA dedicate all'archiviazione ISCSI. La rete utilizzata per iSCSI non può essere utilizzata per la comunicazione di rete. In tutti i server del cluster, le schede di rete utilizzate per eseguire la connessione alla destinazione di archiviazione iSCSI devono essere identiche ed è consigliabile utilizzare Gigabit Ethernet o versione successiva.  

- **Spazio di archiviazione:** È necessario usare l'archiviazione condivisa è certificata per Windows Server 2016 o Windows Server 2019.
  
    Per un cluster di failover a due nodi, lo spazio di archiviazione deve contenere almeno due volumi separati (LUN) se si usa un disco di controllo per il quorum. Il disco di controllo è un disco dell'archiviazione cluster progettato per contenere una copia del database di configurazione del cluster. Per questo esempio di cluster a due nodi, la configurazione quorum sarà maggioranza dei nodi e dischi. Disco maggioranza dei nodi e significa che i nodi e il disco di controllo contengono copie della configurazione del cluster e il cluster dispone del quorum, purché la maggioranza (due di tre) di queste copie sono disponibili. L'altro volume (LUN) conterrà i file che vengono condivisi tra gli utenti.

    Di seguito sono elencati i requisiti di archiviazione:

    - Per utilizzare il supporto del disco nativo incluso nel clustering di failover, utilizzare dischi di base, non dinamici.
    - È consigliabile formattare le partizioni con NTFS. Per le partizioni del disco di controllo il formato NTFS è obbligatorio.
    - È possibile utilizzare MBR (Record di avvio principale, Master Boot Record) o GPT (Tabella di partizione GUID, GUID Partition Table) come stile di partizione del disco.
    - Lo spazio di archiviazione deve rispondere correttamente a comandi SCSI specifici, lo spazio di archiviazione deve rispettare lo standard denominato 3 SCSI Primary Commands-(SPC-3). In particolare, l'archiviazione deve supportare le prenotazioni permanenti come specificato nello standard SPC-3. 
    -  Il driver miniport utilizzato per l'archiviazione deve essere compatibile con il driver di archiviazione Microsoft Storport.

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Distribuzione di reti SAN con cluster di failover

Quando si distribuisce una rete di archiviazione (SAN) con un cluster di failover, le linee guida seguenti devono essere osservate.

- **Conferma certificazione dello spazio di archiviazione:** Usando il [catalogo di Windows Server](https://www.windowsservercatalog.com/default.aspx) del sito, verificare l'archiviazione del fornitore, compresi software, firmware e driver è certificato per Windows Server 2016 o Windows Server 2019.

- **Isolare i dispositivi di archiviazione, un cluster per dispositivo:** i server di cluster diversi non devono essere in grado di accedere agli stessi dispositivi di archiviazione. Nella maggior parte dei casi, un LUN utilizzato per un insieme di server del cluster deve essere isolato da tutti gli altri server tramite mascheramento o suddivisione in zone dei LUN.

- **È consigliabile utilizzare software multipath i/o:** in un'infrastruttura di archiviazione a elevata disponibilità è possibile distribuire cluster di failover con più schede HBA mediante software Multipath I/O. In questo modo viene fornito il livello più elevato di ridondanza e disponibilità. La soluzione a percorsi multipli deve essere basata su Microsoft Multipath i/o (MPIO). Il fornitore dell'hardware di archiviazione può fornire un modulo MPIO specifiche del dispositivo (DSM) per l'hardware, sebbene Windows Server 2016 e Windows Server 2019 includere uno o più moduli DSM al come parte del sistema operativo.

## <a name="network-infrastructure-and-domain-account-requirements"></a>Requisiti degli account di dominio e dell'infrastruttura di rete

Per un cluster di failover a due nodi, saranno necessari l'infrastruttura di rete seguente e un account amministrativo con le autorizzazioni di dominio indicate di seguito.

- **Le impostazioni di rete e indirizzi IP:** se si utilizzano schede di rete identiche per una rete, utilizzare impostazioni di comunicazioni identiche (ad esempio, Velocità, Mod. Duplex, Controllo di flusso e Tipo di supporto). Confrontare inoltre le impostazioni della scheda di rete e del commutatore a cui è connessa, quindi verificare che non esistano conflitti.

    In caso di reti private non indirizzate al resto dell'infrastruttura di rete, verificare che ogni rete privata utilizzi una subnet univoca. Questa condizione è necessaria anche nel caso in cui a ogni scheda di rete sia stato associato un indirizzo IP univoco. Se ad esempio si dispone di un nodo cluster in un ufficio centrale che impiega una rete fisica e di un altro nodo in una succursale che utilizza una rete fisica separata, non specificare 10.0.0.0/24 per entrambe le reti, anche nel caso in cui a ogni scheda sia stato associato un indirizzo IP univoco.

    Per altre informazioni sulle schede di rete, vedere i requisiti Hardware per un cluster di failover a due nodi, più indietro in questa Guida.

- **DNS:** i server nel cluster devono utilizzare DNS per la risoluzione dei nomi. È possibile utilizzare il protocollo di aggiornamento dinamico del DNS.

- **Ruolo di dominio:** tutti i server nel cluster devono trovarsi all'interno dello stesso dominio Active Directory. È consigliabile che tutti i server del cluster dispongano dello stesso ruolo di dominio (server membro o controller di dominio). Il ruolo consigliato è server membro.

- **Controller di dominio:** è consigliabile che i server del cluster siano server membri. In tal caso, sarà necessario disporre di un server aggiuntivo che funga da controller di dominio nel dominio che contiene il cluster di failover.

- **Client:** come per il test, è possibile connettere uno o più client della rete al cluster di failover creato e osservare gli effetti sul client quando si sposta il file server del cluster o se ne esegue il failover da un nodo del cluster all'altro.

- **Account per l'amministrazione del cluster:** la prima volta che si crea un cluster o vi si aggiungono server, è necessario eseguire l'accesso al dominio con un account provvisto di autorizzazioni e diritti di amministratore su tutti i server del cluster. Non è necessario che si tratti di un account Domain Admins, ma può trattarsi di un account Domain Users appartenente al gruppo Administrators in entrambi i server del cluster. Inoltre, se l'account non è un account Domain Admins, l'account (o il gruppo che l'account è membro di) è necessario assegnare il **crea oggetti Computer** e **Leggi tutte le proprietà** autorizzazioni nel dominio unità organizzativa (OU) che si troverà nella.

## <a name="steps-for-installing-a-two-node-file-server-cluster"></a>Procedura per l'installazione di un cluster di file server a due nodi

Per installare un cluster di failover di file server a due nodi è necessario completare i passaggi indicati di seguito.

Passaggio 1: connettere i server del cluster alle reti e all'archiviazione

Passaggio 2: installare la funzionalità di cluster di failover

Passaggio 3: convalidare la configurazione del cluster

Passaggio 4: creare il cluster

Se già stato installato di nodi del cluster e si desidera configurare un cluster di failover di file server, vedere i passaggi per configurare un cluster di server di file di due nodi, più avanti in questa Guida.

### <a name="step-1-connect-the-cluster-servers-to-the-networks-and-storage"></a>Passaggio 1: connettere i server del cluster alle reti e all'archiviazione

Per una rete del cluster di failover, evitare singoli punti di errore. A tale scopo, è possibile eseguire diverse procedure. È possibile connettere i nodi cluster con più reti distinte. In alternativa, è possibile connettere i nodi del cluster con un'unica rete creata con schede di rete in team, commutatori ridondanti, router ridondanti o componenti hardware simili che consentano di eliminare i singoli punti di errore. Se si utilizza una rete per iSCSI, è necessario creare questa rete in aggiunta alle altre reti.

Per un cluster del file server a due nodi, quando si connettono i server all'archivio cluster, è necessario esporre almeno due volumi (LUN). Se necessario per un test più approfondito della configurazione, è possibile esporre ulteriori volumi. Non esporre i volumi del cluster ai server che non fanno parte del cluster.

#### <a name="to-connect-the-cluster-servers-to-the-networks-and-storage"></a>Per connettere i server del cluster alle reti e all'archiviazione

1. Esaminare i dettagli sulle reti in requisiti Hardware per un cluster di failover a due nodi e i requisiti di account dominio e dell'infrastruttura di rete per un cluster di failover a due nodi, più indietro in questa Guida.

2. Connettere e configurare le reti che verranno utilizzate dai server nel cluster.

3. Se la configurazione dei test include client o un controller di dominio non cluster, verificare che questi computer possano connettersi ai server del cluster mediante almeno una rete.

4. Seguire le istruzioni del produttore per connettere fisicamente i server all'archiviazione.

5. Verificare che i dischi (LUN) che si desidera utilizzare nel cluster siano esposti ai server che verranno configurati in cluster e solo a tali server. Per esporre dischi o LUN, è possibile utilizzare qualsiasi interfaccia tra quelle indicate di seguito.

    - L'interfaccia fornita dal produttore dell'archiviazione.

    - Se si utilizza iSCSI, un'interfaccia iSCSI appropriata.

6. Se è stato acquistato software che controlla il formato o la funzione del disco, seguire le istruzioni dal fornitore sull'utilizzo del software con Windows Server.

7. In uno dei server che si desidera del cluster, fare clic su Start, scegliere Strumenti di amministrazione, fare clic su Gestione Computer e quindi fare clic su Gestione disco. (Se viene visualizzata la finestra di dialogo controllo dell'Account utente, verificare che l'azione visualizzata sia quello desiderato e quindi fare clic su Continua.) In Gestione disco, verificare che i dischi del cluster siano visibili.

8. Se si desidera un nuovo volume di archiviazione superiore a 2 terabyte e si utilizza l'interfaccia di Windows per controllare il formato del disco, convertire il disco nello stile di partizione denominato GPT (Tabella di partizione GUID, GUID Partition Table). A tale scopo, eseguire il backup di tutti i dati sul disco, eliminare tutti i volumi sul disco e quindi, in Gestione disco, fare clic sul disco (non una partizione) e fare clic su Converti in disco GPT.  Per i volumi inferiori a 2 terabyte, anziché utilizzare GPT, è possibile utilizzare lo stile di partizione denominato MBR (Record di avvio principale, Master Boot Record).

9. Controllare il formato di tutti i LUN o i volumi esposti. È consigliabile utilizzare NTFS per il formato. Per il disco di controllo l'utilizzo di NTFS è obbligatorio.

### <a name="step-2-install-the-file-server-role-and-failover-cluster-feature"></a>Passaggio 2: Installare la funzionalità cluster ruolo e il failover del server di file

In questo passaggio verrà installata la funzionalità cluster ruolo e il failover del server di file. Entrambi i server devono essere in esecuzione in Windows Server 2016 o Windows Server 2019.

#### <a name="using-server-manager"></a>Con Server Manager

1. Aprire **Server Manager** e sotto il **Gestisci** elenco a discesa, selezionare **Aggiungi ruoli e funzionalità**.

   ![Aggiungere funzionalità](media/Cluster-File-Server/Cluster-FS-Add-Feature.png)

2. Se il **prima di iniziare** verrà visualizzata la finestra, scegliere **successivo**.

3. Per il **tipo di installazione**, selezionare **installazione basata su ruoli o basata su funzionalità** e **Next**.

4. Assicurarsi **selezionare un server dal pool di server** è selezionata, viene evidenziato il nome del computer, e **successivo**.

5. Per il ruolo di Server, nell'elenco dei ruoli, aprire **servizi File**, selezionare **File Server**, e **Next**.

   ![Aggiungere il ruolo](media/Cluster-File-Server/Cluster-FS-Add-FS-Role-1.png)

6. Per le funzionalità, nell'elenco delle funzionalità, selezionare **Clustering di Failover**.  Verrà visualizzata una finestra di dialogo popup che elenca gli strumenti di amministrazione installati anche.  Mantieni tutto selezionato, scegliere **Aggiungi funzionalità** e **successivo**.

   ![Aggiungere funzionalità](media/Cluster-File-Server/Cluster-FS-Add-WSFC-1.png)

7. Nella pagina di conferma, scegliere Installa.

8. Al termine dell'installazione, riavviare il computer.

9. Ripetere i passaggi nella seconda macchina.

#### <a name="using-powershell"></a>Mediante PowerShell

1. Aprire una sessione amministrativa di PowerShell facendo clic sul pulsante Start e quindi selezionando **Windows PowerShell (Admin)** .
2. Per installare il ruolo File Server, eseguire il comando:

    ```PowerShell
    Install-WindowsFeature -Name FS-FileServer
    ```

3. Per installare la funzionalità Clustering di Failover e relativi strumenti di gestione, eseguire il comando:

    ```PowerShell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

4. Dopo che sono stati completati, è possibile verificare che vengano installati con i comandi:

    ```PowerShell
    Get-WindowsFeature -Name FS-FileServer
    Get-WindowsFeature -Name Failover-Clustering
    ```

5. Al termine della verifica che sono installati, riavviare il computer con il comando:

    ```PowerShell
    Restart-Computer
    ```

6. Ripetere i passaggi nel secondo server.

### <a name="step-3-validate-the-cluster-configuration"></a>Passaggio 3: convalidare la configurazione del cluster

Prima di creare un cluster, è consigliabile convalidare la configurazione. La convalida consente di confermare che la configurazione dei server, della rete e dell'archiviazione soddisfa un insieme di requisiti specifici per i cluster di failover.

#### <a name="using-failover-cluster-manager"></a>Usando Gestione Cluster di Failover

1. Dal **Server Manager**, scegliere il **Tools** elenco a discesa e selezionare **gestione Cluster di Failover**.

2. Nella **gestione Cluster di Failover**, passare alla colonna intermedia sotto **Management** e scegliere **convalida configurazione**.

3. Se il **prima di iniziare** verrà visualizzata la finestra, scegliere **successivo**.

4. Nel **selezione dei server o un Cluster** finestra, aggiungere i nomi dei due computer che rappresenteranno i nodi del cluster.  Ad esempio, se i nomi sono NODE1 e NODE2, immettere il nome e selezionare **Add**.  È anche possibile scegliere il **esplorare** pulsante per cercare i nomi di Active Directory.  Quando entrambi sono elencati sotto **i server selezionati**, scegliere **successivo**.

5. Nel **opzioni di Testing** finestra, seleziona **Esegui tutti i test (scelta consigliati)** , e **Avanti**.

6. Nel **conferma** pagina, verrà visualizzato l'elenco di tutti i test verrà verificata la validità.  Scegli **successivo** iniziare i test.

7. Una volta completata, il **riepilogo** verrà visualizzata la pagina dopo l'esecuzione dei test. Per visualizzare gli argomenti della Guida che consentiranno di interpretare i risultati, fare clic su **Ulteriori informazioni sui test di convalida dei cluster**.

8. Mentre sempre nella pagina di riepilogo, fare clic su Visualizza Report e leggere i risultati del test. Apportare le modifiche necessarie alla configurazione e rieseguire i test. <br>Per visualizzare i risultati dei test dopo aver chiuso la procedura guidata, vedere *SystemRoot\Cluster\Reports\Validation Report Data e ora. HTML*.

9. Per visualizzare gli argomenti della Guida sulla convalida del cluster dopo aver chiuso la procedura guidata, in Gestione Cluster di Failover, fare clic su fare clic sugli argomenti della Guida, selezionare la scheda Sommario, espandere il contenuto per il cluster di failover della Guida in linea e fare clic su convalida una configurazione di Cluster di Failover .

#### <a name="using-powershell"></a>Mediante PowerShell

1. Aprire una sessione amministrativa di PowerShell facendo clic sul pulsante Start e quindi selezionando **Windows PowerShell (Admin)** .

2. Per convalidare le macchine (ad esempio, i nomi di computer da NODE1 e NODE2) per il Clustering di Failover, eseguire il comando:

    ```PowerShell
    Test-Cluster -Node "NODE1","NODE2"
    ```
4. Per visualizzare i risultati dei test dopo aver chiuso la procedura guidata, vedere il file specificato (in SystemRoot\Cluster\Reports\), quindi apportare le modifiche necessarie alla configurazione e rieseguire i test.

Per altre informazioni, vedi [convalida una configurazione di Cluster di Failover](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v=ws.11)).

### <a name="step-4-create-the-cluster"></a>Passaggio 4: Creare il Cluster

Di seguito verrà creato un cluster all'esterno le macchine e della configurazione che è impostata.

#### <a name="using-failover-cluster-manager"></a>Usando Gestione Cluster di Failover

1. Dal **Server Manager**, scegliere il **Tools** elenco a discesa e selezionare **gestione Cluster di Failover**.

2. In **gestione Cluster di Failover**, passare alla colonna intermedia sotto **Management** e scegliere **crea Cluster**.

3. Se il **prima di iniziare** verrà visualizzata la finestra, scegliere **successivo**.

4. Nel **selezione dei server** finestra, aggiungere i nomi dei due computer che rappresenteranno i nodi del cluster.  Ad esempio, se i nomi sono NODE1 e NODE2, immettere il nome e selezionare **Add**.  È anche possibile scegliere il **esplorare** pulsante per cercare i nomi di Active Directory.  Quando entrambi sono elencati sotto **i server selezionati**, scegliere **successivo**.

5. Nel **punto di accesso per l'amministrazione del Cluster** finestra, immettere il nome del cluster si intende utilizzare.  Si noti che non si userà per connettersi alle condivisioni file con nome.  Si tratta di semplice amministrazione del cluster.

   > [!NOTE]
   > Se si usano indirizzi IP statici, è necessario selezionare la rete da usare e immettere l'indirizzo IP verrà utilizzato per il nome del cluster.  Se si usa DHCP per gli indirizzi IP, l'indirizzo IP verranno configurato automaticamente per l'utente.

6. Scegli **successivo**.

7. Nel **conferma** pagina, verificare che cosa è stato configurato e selezionare **successivo** per creare il Cluster.

8. Nel **riepilogo** pagina, in questo caso sarà la configurazione creata.  È possibile selezionare Visualizza Report per visualizzare il report della creazione.

#### <a name="using-powershell"></a>Mediante PowerShell

1. Aprire una sessione amministrativa di PowerShell facendo clic sul pulsante Start e quindi selezionando **Windows PowerShell (Admin)** .

2. Eseguire il comando seguente per creare il cluster, se si usa indirizzi IP statici.  Ad esempio, i nomi computer sono NODE1 e NODE2, il nome del cluster è CLUSTER e l'indirizzo IP sarà 1.1.1.1.

   ```PowerShell
    New-Cluster -Name CLUSTER -Node "NODE1","NODE2" -StaticAddress 1.1.1.1
   ```

3. Eseguire il comando seguente per creare il cluster, se si usa DHCP per gli indirizzi IP.  Ad esempio, i nomi computer sono NODE1 e NODE2, e il nome del cluster è CLUSTER.

   ```PowerShell    
   New-Cluster -Name CLUSTER -Node "NODE1","NODE2"
   ```

### <a name="steps-for-configuring-a-file-server-failover-cluster"></a>Passaggi per configurare un cluster di failover di file server

Per configurare un cluster di failover di file server, seguire i passaggi riportati di seguito.

1. Dal **Server Manager**, scegliere il **Tools** elenco a discesa e selezionare **gestione Cluster di Failover**.

2. Quando si apre Gestione Cluster di Failover, dovrebbe visualizzare automaticamente il nome del cluster che è stato creato.  Se non esiste, andare nella colonna centrale in **Management** e scegliere **Connetti al Cluster**.  Immettere il nome del cluster è stato creato e **OK**.

3. Nell'albero della console, fare clic sui ">" segno più accanto al cluster creato per espanderne gli elementi sottostanti.

4. Fare clic destro del mouse su **ruoli** e selezionare **Configura ruolo**.

5. Se il **prima di iniziare** verrà visualizzata la finestra, scegliere **successivo**.

6. Nell'elenco dei ruoli, scegliere **File Server** e **successivo**.

7. Il tipo di File Server, selezionare **File Server per utilizzo generico** e **successivo**.<br>Per informazioni relative a Scale-Out File Server, vedere [Panoramica di Scale-Out File Server](sofs-overview.md).

   ![Tipo di file Server](media/Cluster-File-Server/Cluster-FS-File-Server-Type.png)

8. Nel **punto di accesso Client** finestra, immettere il nome del file server verrà utilizzato.  Si noti che questo non è il nome del cluster.  Questo vale per la connettività di condivisione file.  Ad esempio, se vuole connettere \\SERVER, il nome immesso è SERVER.

   > [!NOTE]
   > Se si usano indirizzi IP statici, è necessario selezionare la rete da usare e immettere l'indirizzo IP verrà utilizzato per il nome del cluster.  Se si usa DHCP per gli indirizzi IP, l'indirizzo IP verranno configurato automaticamente per l'utente.

6. Scegli **successivo**.

7. Nel **seleziona archivi** finestra, selezionare l'unità aggiuntiva (non il controllo del mirroring) che conterrà le condivisioni e **successivo**.

8. Nel **conferma** pagina, verificare la configurazione e selezionare **successivo**.

9. Nel **riepilogo** pagina, in questo caso sarà la configurazione creata.  È possibile selezionare Visualizza Report per visualizzare il report di creazione del ruolo file server.

10. Sotto **ruoli** nell'albero della console, verrà visualizzato il nuovo ruolo creato elencato come il nome è stato creato.  Con questo evidenziato, sotto il **azioni** riquadro a destra, scegliere **aggiungere una condivisione**.

11. Eseguire la procedura guidata condivisione inserendo il seguente:

    - Tipo di condivisione che sarà
    - Percorso o il percorso della cartella condivisa sarà
    - Il nome degli utenti condivisione si connetterà al
    - Impostazioni aggiuntive, ad esempio l'enumerazione basata sull'accesso, la memorizzazione nella cache, crittografia e così via
    - Le autorizzazioni a livello di file se saranno diversi dai valori predefiniti

12. Nel **conferma** pagina, verificare che cosa è stato configurato e selezionare **crea** per creare la condivisione di file server.

13. Nel **risultati** pagina, selezionare Chiudi se è stata creata la condivisione.  Se non è possibile creare la condivisione, verrà visualizzato gli errori rilevati.

14. Scegli **Chiudi**.

15. Ripetere questo processo per tutte le condivisioni aggiuntive.