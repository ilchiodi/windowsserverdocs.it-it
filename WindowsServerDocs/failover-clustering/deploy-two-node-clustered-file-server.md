---
title: Distribuzione di un file server cluster a due nodi
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/01/2019
description: Questo articolo descrive la creazione di un cluster file server a due nodi
ms.openlocfilehash: 03e78495b3fc85449d3d383706fb82541dd10372
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361301"
---
# <a name="deploying-a-two-node-clustered-file-server"></a>Distribuzione di un file server cluster a due nodi

> Si applica a: Windows Server 2019, Windows Server 2016

Un cluster di failover è costituito da un gruppo di computer indipendenti che interagiscono tra di loro per migliorare la disponibilità di servizi e applicazioni. I server inclusi nel cluster (detti nodi) sono connessi mediante cavi fisici e software. Se in uno dei nodi si verifica un errore, il servizio verrà garantito da un altro nodo tramite un processo denominato failover. Per gli utenti l'interruzione del servizio risulterà minima.

Questa guida descrive i passaggi per l'installazione e la configurazione di un cluster di failover di file server per utilizzo generico con due nodi. Creando la configurazione in questa guida, è possibile ottenere informazioni sui cluster di failover e acquisire familiarità con l'interfaccia dello snap-in Gestione cluster di failover in Windows Server 2019 o Windows Server 2016.

## <a name="overview-for-a-two-node-file-server-cluster"></a>Panoramica di un cluster di file server a due nodi

I server in un cluster di failover possono funzionare in diversi ruoli, inclusi i ruoli di file server, server Hyper-V o server di database e possono garantire disponibilità elevata per un'ampia gamma di altri servizi e applicazioni. In questa guida viene illustrato come configurare un cluster di file server a due nodi.

Un cluster di failover include in genere un'unità di archiviazione connessa fisicamente a tutti i server del cluster, sebbene ogni volume dell'archiviazione sia accessibile solo da un server alla volta. Nel diagramma seguente viene mostrato un cluster di failover a due nodi connesso a un'unità di archiviazione.

![Cluster a due nodi](media/Cluster-File-Server/Cluster-FS-Overview.png)

I volumi di archiviazione o i numeri di unità logica (LUN) esposti ai nodi in un cluster non devono essere esposti ad altri server, inclusi i server in un altro cluster, come illustrato nel diagramma seguente.

![Lun nell'archiviazione](media/Cluster-File-Server/Cluster-FS-LUNs.png)

Per ottenere la massima disponibilità di qualsiasi server, è importante seguire le procedure consigliate per la gestione dei server, che prevedono ad esempio di gestire in modo oculato l'ambiente fisico dei server, eseguire test sulle modifiche del software prima di implementarle e tenere traccia in modo efficace degli aggiornamenti software e delle modifiche alla configurazione in tutti i server del cluster.

Lo scenario seguente illustra il modo in cui può essere configurato un cluster di failover di file server. I file condivisi si trovano nell'archivio del cluster e qualsiasi server del cluster può fungere da file server che ne consente la condivisione.

## <a name="shared-folders-in-a-failover-cluster"></a>Cartelle condivise in un cluster di failover

Nell'elenco seguente vengono descritte le funzionalità di configurazione di cartelle condivise integrate nel clustering di failover di:

- La visualizzazione ha come ambito solo le cartelle condivise in cluster (nessuna combinazione con cartelle condivise non in cluster): Quando un utente Visualizza cartelle condivise specificando il percorso di un file server cluster, la visualizzazione includerà solo le cartelle condivise che fanno parte del ruolo di file server specifico. Esclude le cartelle condivise non in cluster e condivide parte di ruoli di file server distinti che si trovano in un nodo del cluster.

- Enumerazione basata sull'accesso: l'enumerazione basata sull'accesso consente di nascondere una cartella specifica nella visualizzazione degli utenti. Anziché consentire agli utenti di visualizzare la cartella senza potervi accedere, è possibile evitare che la vedano. È possibile configurare l'enumerazione basata sull'accesso per una cartella condivisa in cluster in modo analogo a una cartella condivisa non in cluster.

- Accesso offline: l'accesso non in linea, ovvero la memorizzazione nella cache, viene configurato nello stesso modo sia per una cartella condivisa in cluster che per una cartella condivisa non in cluster.

- I dischi del cluster vengono sempre riconosciuti come parte del cluster: Se si utilizza l'interfaccia del cluster di failover, Esplora risorse o lo snap-in Gestione condivisione e archiviazione, Windows riconosce se un disco è stato designato come nell'archivio cluster. Se un disco di questo tipo è già stato configurato in Gestione cluster di failover come parte di un file server cluster, è possibile usare una delle interfacce citate in precedenza per creare una condivisione sul disco. Se il disco non è stato configurato come parte di un file server del cluster, non sarà possibile creare accidentalmente una condivisione nel disco. Verrà infatti visualizzato un messaggio di errore in cui viene indicato che prima di condividere il disco è necessario configurarlo come parte di un file server del cluster.

- Integrazione dei servizi per il file System di rete: Il ruolo file server in Windows Server include il servizio ruolo facoltativo denominato servizi per NFS (Network File System). Se si installa il servizio ruolo e si configurano cartelle condivise mediante Servizi per NFS, sarà possibile creare un file server del cluster che supporti i client basati su UNIX.

## <a name="requirements-for-a-two-node-failover-cluster"></a>Requisiti di un cluster di failover a due nodi

Affinché un cluster di failover in Windows Server 2016 o Windows Server 2019 venga considerato una soluzione ufficialmente supportata da Microsoft, la soluzione deve soddisfare i criteri seguenti.

- Tutti i componenti hardware e software devono soddisfare i requisiti di qualifica per il logo appropriato. Per Windows Server 2016, si tratta del logo "Certified for Windows Server 2016". Per Windows Server 2019, si tratta del logo "Certified for Windows Server 2019". Per ulteriori informazioni su quali sistemi hardware e software sono stati certificati, visitare il sito del [Catalogo di Microsoft Windows Server](https://www.windowsservercatalog.com/default.aspx) .

- La soluzione completamente configurata (server, rete e archiviazione) deve superare tutti i test nella procedura guidata di convalida, che fa parte dello snap-in cluster di failover.

Per un cluster di failover a due nodi, saranno necessari gli elementi seguenti.

- **Server** Si consiglia di utilizzare i computer corrispondenti con componenti uguali o simili.  I server per un cluster di failover a due nodi devono eseguire la stessa versione di Windows Server. Devono anche avere gli stessi aggiornamenti software (patch).

- **Cavi e schede di rete:** L'hardware di rete, come gli altri componenti nella soluzione cluster di failover, deve essere compatibile con Windows Server 2016 o Windows Server 2019. Se si utilizza iSCSI, le schede di rete devono essere dedicate alla comunicazione di rete o a iSCSI, non a entrambe. Evitare singoli punti di errore nell'infrastruttura di rete utilizzata per la connessione dei nodi del cluster. A tale scopo, è possibile eseguire diverse procedure. È possibile connettere i nodi cluster con più reti distinte. In alternativa, è possibile connettere i nodi cluster con una rete costruita con schede di rete in gruppo, commutatori ridondanti, router ridondanti o hardware simile che rimuova singoli punti di errore.

   > [!NOTE]
   > Se i nodi del cluster sono connessi a una singola rete, la rete supererà il requisito di ridondanza nella convalida guidata configurazione.  Tuttavia, il report includerà un avviso indicante che la rete non deve avere un singolo punto di errore.

- **Controller del dispositivo o adattatori appropriati per l'archiviazione:**
    - **SCSI o Fibre Channel collegato seriale:** in caso di utilizzo di SAS o Fibre Channel, in tutti i server del cluster, è preferibile che tutti i componenti dello stack di archiviazione siano identici. È necessario che i componenti software MPIO (Multipath I/O) e DSM (Device Specific Module) siano identici.  È consigliabile che i controller dei dispositivi di archiviazione di massa, ossia la scheda bus host (HBA) e i relativi driver e firmware, collegati all'archiviazione cluster siano identici. Se si utilizzano schede HBA diverse, è opportuno verificare con il fornitore dell'archiviazione che vengano eseguite le configurazioni supportate o consigliate.
    - **iSCSI** Se si utilizza iSCSI, ogni server del cluster deve disporre di una o più schede di rete o schede bus host dedicate all'archiviazione ISCSI. La rete utilizzata per iSCSI non può essere utilizzata per la comunicazione di rete. In tutti i server del cluster, le schede di rete utilizzate per eseguire la connessione alla destinazione di archiviazione iSCSI devono essere identiche ed è consigliabile utilizzare Gigabit Ethernet o versione successiva.  

- **Archiviazione** È necessario usare l'archiviazione condivisa certificata per Windows Server 2016 o Windows Server 2019.
  
    Per un cluster di failover a due nodi, l'archiviazione deve contenere almeno due volumi separati (lun) se si utilizza un disco di controllo per il quorum. Il disco di controllo è un disco dell'archiviazione cluster progettato per contenere una copia del database di configurazione del cluster. Per questo esempio di cluster a due nodi, la configurazione del quorum sarà maggioranza dei nodi e dei dischi. Maggioranza dei nodi e dei dischi significa che i nodi e il disco di controllo contengono ciascuno copie della configurazione del cluster e il cluster dispone del quorum, purché sia disponibile una maggioranza (due di tre) di queste copie. L'altro volume (LUN) conterrà i file condivisi dagli utenti.

    Di seguito sono elencati i requisiti di archiviazione:

    - Per utilizzare il supporto del disco nativo incluso nel clustering di failover, utilizzare dischi di base, non dinamici.
    - È consigliabile formattare le partizioni con NTFS. Per le partizioni del disco di controllo il formato NTFS è obbligatorio.
    - È possibile utilizzare MBR (Record di avvio principale, Master Boot Record) o GPT (Tabella di partizione GUID, GUID Partition Table) come stile di partizione del disco.
    - Lo spazio di archiviazione deve rispondere correttamente a comandi SCSI specifici, lo spazio di archiviazione deve seguire lo standard denominato SCSI Primary Commands-3 (SPC-3). In particolare, l'archiviazione deve supportare le prenotazioni permanenti come specificato nello standard SPC-3. 
    -  Il driver miniport utilizzato per l'archiviazione deve essere compatibile con il driver di archiviazione Microsoft Storport.

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Distribuzione di reti SAN con cluster di failover

Quando si distribuisce una rete di archiviazione (SAN) con un cluster di failover, è opportuno osservare le linee guida seguenti.

- **Confermare la certificazione dello spazio di archiviazione:** Utilizzando il sito del [Catalogo di Windows Server](https://www.windowsservercatalog.com/default.aspx) , verificare che la risorsa di archiviazione del fornitore, inclusi driver, firmware e software, sia certificata per windows server 2016 o windows server 2019.

- **Isolare i dispositivi di archiviazione, un cluster per dispositivo:** i server di cluster diversi non devono essere in grado di accedere agli stessi dispositivi di archiviazione. Nella maggior parte dei casi, un LUN utilizzato per un insieme di server del cluster deve essere isolato da tutti gli altri server tramite mascheramento o suddivisione in zone dei LUN.

- **Prendere in considerazione l'uso del software Multipath I/O:** in un'infrastruttura di archiviazione a elevata disponibilità è possibile distribuire cluster di failover con più schede HBA mediante software Multipath I/O. In questo modo viene fornito il livello più elevato di ridondanza e disponibilità. La soluzione a percorsi multipli deve essere basata su Microsoft Multipath I/O (MPIO). Il fornitore dell'hardware di archiviazione può fornire un modulo DSM (Device Specific Module) MPIO per l'hardware, sebbene Windows Server 2016 e Windows Server 2019 includano uno o più DSM come parte del sistema operativo.

## <a name="network-infrastructure-and-domain-account-requirements"></a>Requisiti degli account di dominio e dell'infrastruttura di rete

Per un cluster di failover a due nodi, saranno necessari l'infrastruttura di rete seguente e un account amministrativo con le autorizzazioni di dominio indicate di seguito.

- **Impostazioni di rete e indirizzi IP:** se si utilizzano schede di rete identiche per una rete, utilizzare impostazioni di comunicazioni identiche (ad esempio, Velocità, Mod. Duplex, Controllo di flusso e Tipo di supporto). Confrontare inoltre le impostazioni della scheda di rete e del commutatore a cui è connessa, quindi verificare che non esistano conflitti.

    In caso di reti private non indirizzate al resto dell'infrastruttura di rete, verificare che ogni rete privata utilizzi una subnet univoca. Questa condizione è necessaria anche nel caso in cui a ogni scheda di rete sia stato associato un indirizzo IP univoco. Se ad esempio si dispone di un nodo cluster in un ufficio centrale che impiega una rete fisica e di un altro nodo in una succursale che utilizza una rete fisica separata, non specificare 10.0.0.0/24 per entrambe le reti, anche nel caso in cui a ogni scheda sia stato associato un indirizzo IP univoco.

    Per ulteriori informazioni sulle schede di rete, vedere Requisiti hardware per un cluster di failover a due nodi, più indietro in questa guida.

- **DNS** i server nel cluster devono utilizzare DNS per la risoluzione dei nomi. È possibile utilizzare il protocollo di aggiornamento dinamico del DNS.

- **Ruolo di dominio:** tutti i server nel cluster devono trovarsi all'interno dello stesso dominio Active Directory. È consigliabile che tutti i server del cluster dispongano dello stesso ruolo di dominio (server membro o controller di dominio). Il ruolo consigliato è server membro.

- **Controller di dominio:** è consigliabile che i server del cluster siano server membri. In tal caso, sarà necessario disporre di un server aggiuntivo che funga da controller di dominio nel dominio che contiene il cluster di failover.

- **Client** come per il test, è possibile connettere uno o più client della rete al cluster di failover creato e osservare gli effetti sul client quando si sposta il file server del cluster o se ne esegue il failover da un nodo del cluster all'altro.

- **Account per l'amministrazione del cluster:** la prima volta che si crea un cluster o vi si aggiungono server, è necessario eseguire l'accesso al dominio con un account provvisto di autorizzazioni e diritti di amministratore su tutti i server del cluster. Non è necessario che si tratti di un account Domain Admins, ma può trattarsi di un account Domain Users appartenente al gruppo Administrators in entrambi i server del cluster. Inoltre, se l'account non è un account Domain Admins, all'account o al gruppo di cui l'account è membro è necessario assegnare le autorizzazioni **Crea oggetti computer** e **Leggi tutte le proprietà** nell'unità organizzativa del dominio (OU) che verrà si trova in.

## <a name="steps-for-installing-a-two-node-file-server-cluster"></a>Procedura per l'installazione di un cluster di file server a due nodi

Per installare un cluster di failover di file server a due nodi è necessario completare i passaggi indicati di seguito.

Passaggio 1: connettere i server del cluster alle reti e all'archiviazione

Passaggio 2: installare la funzionalità di cluster di failover

Passaggio 3: convalidare la configurazione del cluster

Passaggio 4: creare il cluster

Se i nodi del cluster sono già stati installati e si desidera configurare un cluster di failover di file server, vedere la procedura per la configurazione di un cluster file server a due nodi, più avanti in questa guida.

### <a name="step-1-connect-the-cluster-servers-to-the-networks-and-storage"></a>Passaggio 1: connettere i server del cluster alle reti e all'archiviazione

Per una rete del cluster di failover, evitare singoli punti di errore. A tale scopo, è possibile eseguire diverse procedure. È possibile connettere i nodi cluster con più reti distinte. In alternativa, è possibile connettere i nodi del cluster con un'unica rete creata con schede di rete in team, commutatori ridondanti, router ridondanti o componenti hardware simili che consentano di eliminare i singoli punti di errore. Se si utilizza una rete per iSCSI, è necessario creare questa rete in aggiunta alle altre reti.

Per un cluster del file server a due nodi, quando si connettono i server all'archivio cluster, è necessario esporre almeno due volumi (LUN). Se necessario per un test più approfondito della configurazione, è possibile esporre ulteriori volumi. Non esporre i volumi del cluster ai server che non fanno parte del cluster.

#### <a name="to-connect-the-cluster-servers-to-the-networks-and-storage"></a>Per connettere i server del cluster alle reti e all'archiviazione

1. Esaminare i dettagli sulle reti in requisiti hardware per un cluster di failover a due nodi e requisiti dell'account di dominio e dell'infrastruttura di rete per un cluster di failover a due nodi, più indietro in questa guida.

2. Connettere e configurare le reti che verranno utilizzate dai server nel cluster.

3. Se la configurazione dei test include client o un controller di dominio non cluster, verificare che questi computer possano connettersi ai server del cluster mediante almeno una rete.

4. Seguire le istruzioni del produttore per connettere fisicamente i server all'archiviazione.

5. Verificare che i dischi (LUN) che si desidera utilizzare nel cluster siano esposti ai server che verranno configurati in cluster e solo a tali server. Per esporre dischi o LUN, è possibile utilizzare qualsiasi interfaccia tra quelle indicate di seguito.

    - L'interfaccia fornita dal produttore dell'archiviazione.

    - Se si utilizza iSCSI, un'interfaccia iSCSI appropriata.

6. Se è stato acquistato un software che controlla il formato o la funzione del disco, seguire le istruzioni fornite dal fornitore per informazioni sull'utilizzo del software con Windows Server.

7. In uno dei server che si desidera inserire nel cluster, fare clic sul pulsante Start, scegliere Strumenti di amministrazione, Gestione computer, quindi fare clic su Gestione disco. Se viene visualizzata la finestra di dialogo controllo account utente, verificare che l'azione visualizzata sia quella desiderata, quindi fare clic su continua. In Gestione disco verificare che i dischi del cluster siano visibili.

8. Se si desidera un nuovo volume di archiviazione superiore a 2 terabyte e si utilizza l'interfaccia di Windows per controllare il formato del disco, convertire il disco nello stile di partizione denominato GPT (Tabella di partizione GUID, GUID Partition Table). A tale scopo, eseguire il backup di tutti i dati sul disco, eliminare tutti i volumi sul disco e quindi, in Gestione disco, fare clic con il pulsante destro del mouse sul disco (non una partizione) e scegliere Converti in disco GPT.  Per i volumi inferiori a 2 terabyte, anziché utilizzare GPT, è possibile utilizzare lo stile di partizione denominato MBR (Record di avvio principale, Master Boot Record).

9. Controllare il formato di tutti i LUN o i volumi esposti. È consigliabile utilizzare NTFS per il formato. Per il disco di controllo l'utilizzo di NTFS è obbligatorio.

### <a name="step-2-install-the-file-server-role-and-failover-cluster-feature"></a>Passaggio 2: Installare il ruolo di file server e la funzionalità del cluster di failover

In questo passaggio verrà installato il ruolo file server e la funzionalità cluster di failover. In entrambi i server deve essere in esecuzione Windows Server 2016 o Windows Server 2019.

#### <a name="using-server-manager"></a>Utilizzo di Server Manager

1. Aprire **Server Manager** e nell'elenco a discesa **Gestisci** Selezionare **Aggiungi ruoli e funzionalità**.

   ![Aggiungi funzionalità](media/Cluster-File-Server/Cluster-FS-Add-Feature.png)

2. Se viene visualizzata la finestra **prima di iniziare** , scegliere **Avanti**.

3. Per il **tipo di installazione**selezionare **installazione basata su ruoli o basata su funzionalità** e **quindi su Avanti**.

4. Verificare che **selezionare un server dal pool di server** sia selezionato, che il nome del computer sia evidenziato e **quindi Avanti**.

5. Per il ruolo del server, nell'elenco dei ruoli aprire **Servizi file**, selezionare **file server**e **quindi Avanti**.

   ![Aggiungi ruolo](media/Cluster-File-Server/Cluster-FS-Add-FS-Role-1.png)

6. Per le funzionalità, nell'elenco delle funzionalità selezionare **clustering di failover**.  Verrà visualizzata una finestra di dialogo popup in cui sono elencati gli strumenti di amministrazione da installare.  Mantieni tutti gli elementi selezionati, scegliere **Aggiungi funzionalità** e **quindi Avanti**.

   ![Aggiungi funzionalità](media/Cluster-File-Server/Cluster-FS-Add-WSFC-1.png)

7. Nella pagina conferma selezionare Installa.

8. Al termine dell'installazione, riavviare il computer.

9. Ripetere i passaggi nel secondo computer.

#### <a name="using-powershell"></a>Mediante PowerShell

1. Aprire una sessione di PowerShell amministrativa facendo clic con il pulsante destro del mouse sul pulsante Start e quindi scegliendo **Windows PowerShell (amministratore)** .
2. Per installare il ruolo file server, eseguire il comando:

    ```PowerShell
    Install-WindowsFeature -Name FS-FileServer
    ```

3. Per installare la funzionalità clustering di failover e i relativi strumenti di gestione, eseguire il comando:

    ```PowerShell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

4. Una volta completate le operazioni, è possibile verificare che siano installate con i comandi:

    ```PowerShell
    Get-WindowsFeature -Name FS-FileServer
    Get-WindowsFeature -Name Failover-Clustering
    ```

5. Dopo aver verificato l'installazione, riavviare il computer con il comando:

    ```PowerShell
    Restart-Computer
    ```

6. Ripetere i passaggi nel secondo server.

### <a name="step-3-validate-the-cluster-configuration"></a>Passaggio 3: convalidare la configurazione del cluster

Prima di creare un cluster, è consigliabile convalidare la configurazione. La convalida consente di confermare che la configurazione dei server, della rete e dell'archiviazione soddisfa un insieme di requisiti specifici per i cluster di failover.

#### <a name="using-failover-cluster-manager"></a>Utilizzo di Gestione cluster di failover

1. Da **Server Manager**, scegliere l'elenco a discesa **strumenti** e selezionare **Gestione cluster di failover**.

2. In **Gestione cluster di failover**passare alla colonna centrale sotto **gestione** e scegliere **Convalida configurazione**.

3. Se viene visualizzata la finestra **prima di iniziare** , scegliere **Avanti**.

4. Nella finestra **Selezione server o cluster** aggiungere i nomi dei due computer che saranno i nodi del cluster.  Ad esempio, se i nomi sono NODE1 e NODE2, immettere il nome e selezionare **Aggiungi**.  È anche possibile scegliere il pulsante **Sfoglia** per cercare i nomi in Active Directory.  Una volta elencate in **server selezionati**, fare clic su **Avanti**.

5. Nella finestra **Opzioni di testing** selezionare **Esegui tutti i test (scelta consigliata)** e **quindi Avanti**.

6. Nella pagina di **conferma** verrà fornito un elenco di tutti i test che verificherà.  Fare clic su **Avanti** per avviare i test.

7. Al termine, la pagina **Riepilogo** viene visualizzata dopo l'esecuzione dei test. Per visualizzare gli argomenti della Guida che consentiranno di interpretare i risultati, fare clic su **Ulteriori informazioni sui test di convalida dei cluster**.

8. Sempre nella pagina Riepilogo, fare clic su Visualizza rapporto e leggere i risultati del test. Apportare le modifiche necessarie alla configurazione ed eseguire di nuovo i test. <br>Per visualizzare i risultati dei test dopo aver chiuso la procedura guidata, vedere *SystemRoot\Cluster\Reports\Validation Report data e ora. html*.

9. Per visualizzare gli argomenti della guida sulla convalida del cluster dopo aver chiuso la procedura guidata, in Gestione cluster di failover fare clic su?, argomenti della guida, fare clic sulla scheda Sommario, espandere il contenuto della guida del cluster di failover e fare clic su convalida di una configurazione del cluster di failover. .

#### <a name="using-powershell"></a>Mediante PowerShell

1. Aprire una sessione di PowerShell amministrativa facendo clic con il pulsante destro del mouse sul pulsante Start e quindi scegliendo **Windows PowerShell (amministratore)** .

2. Per convalidare le macchine virtuali, ad esempio i nomi dei computer NODE1 e NODE2, per il clustering di failover, eseguire il comando:

    ```PowerShell
    Test-Cluster -Node "NODE1","NODE2"
    ```
4. Per visualizzare i risultati dei test dopo aver chiuso la procedura guidata, vedere il file specificato (in SystemRoot\Cluster\Reports @ no__t-0, quindi apportare le modifiche necessarie alla configurazione ed eseguire di nuovo i test.

Per ulteriori informazioni, vedere [convalida di una configurazione del cluster di failover](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v=ws.11)).

### <a name="step-4-create-the-cluster"></a>Passaggio 4: Creare il Cluster

Di seguito viene creato un cluster da computer e configurazione.

#### <a name="using-failover-cluster-manager"></a>Utilizzo di Gestione cluster di failover

1. Da **Server Manager**, scegliere l'elenco a discesa **strumenti** e selezionare **Gestione cluster di failover**.

2. In **Gestione cluster di failover**passare alla colonna centrale sotto **gestione** e scegliere **Crea cluster**.

3. Se viene visualizzata la finestra **prima di iniziare** , scegliere **Avanti**.

4. Nella finestra **Selezione server** aggiungere i nomi dei due computer che saranno i nodi del cluster.  Ad esempio, se i nomi sono NODE1 e NODE2, immettere il nome e selezionare **Aggiungi**.  È anche possibile scegliere il pulsante **Sfoglia** per cercare i nomi in Active Directory.  Una volta elencate in **server selezionati**, fare clic su **Avanti**.

5. Nella finestra **punto di accesso per l'amministrazione del cluster** immettere il nome del cluster che verrà usato.  Si noti che questo non è il nome che verrà usato per connettersi alle condivisioni file con.  Questa operazione è sufficiente per amministrare il cluster.

   > [!NOTE]
   > Se si usano indirizzi IP statici, sarà necessario selezionare la rete da usare e immettere l'indirizzo IP che verrà usato per il nome del cluster.  Se si usa DHCP per gli indirizzi IP, l'indirizzo IP verrà configurato automaticamente.

6. Scegliere **Avanti**.

7. Nella pagina **conferma** verificare gli elementi configurati e selezionare **Avanti** per creare il cluster.

8. Nella pagina **Riepilogo** verrà restituita la configurazione creata.  È possibile selezionare Visualizza report per visualizzare il report della creazione.

#### <a name="using-powershell"></a>Mediante PowerShell

1. Aprire una sessione di PowerShell amministrativa facendo clic con il pulsante destro del mouse sul pulsante Start e quindi scegliendo **Windows PowerShell (amministratore)** .

2. Eseguire il comando seguente per creare il cluster se si usano indirizzi IP statici.  Ad esempio, i nomi dei computer sono NODE1 e NODE2, il nome del cluster sarà CLUSTER e l'indirizzo IP sarà 1.1.1.1.

   ```PowerShell
    New-Cluster -Name CLUSTER -Node "NODE1","NODE2" -StaticAddress 1.1.1.1
   ```

3. Eseguire il comando seguente per creare il cluster se si utilizza DHCP per gli indirizzi IP.  Ad esempio, i nomi dei computer sono NODE1 e NODE2 e il nome del cluster sarà CLUSTER.

   ```PowerShell    
   New-Cluster -Name CLUSTER -Node "NODE1","NODE2"
   ```

### <a name="steps-for-configuring-a-file-server-failover-cluster"></a>Procedura per la configurazione di un cluster di failover di file server

Per configurare un cluster di failover di file server, attenersi alla procedura riportata di seguito.

1. Da **Server Manager**, scegliere l'elenco a discesa **strumenti** e selezionare **Gestione cluster di failover**.

2. Quando Gestione cluster di failover si apre, deve inserire automaticamente il nome del cluster creato.  In caso contrario, passare alla colonna centrale sotto **gestione** e scegliere **Connetti al cluster**.  Immettere il nome del cluster creato e **OK**.

3. Nell'albero della console fare clic sul segno ">" accanto al cluster creato per espandere gli elementi sottostanti.

4. Fare clic con il pulsante destro del mouse su **ruoli** e scegliere **Configura ruolo**.

5. Se viene visualizzata la finestra **prima di iniziare** , scegliere **Avanti**.

6. Nell'elenco dei ruoli scegliere **file server** e **Avanti**.

7. Per il tipo di file server, selezionare **file server per uso generale** e **Avanti**.<br>Per informazioni su File server di scalabilità orizzontale, vedere [Panoramica di file server di scalabilità orizzontale](sofs-overview.md).

   ![Tipo di file server](media/Cluster-File-Server/Cluster-FS-File-Server-Type.png)

8. Nella finestra **punto di accesso client** immettere il nome del file server che verrà utilizzato.  Si noti che questo non è il nome del cluster.  Per la connettività alla condivisione file.  Se, ad esempio, si desidera connettersi a \\SERVER, il nome inputted sarà SERVER.

   > [!NOTE]
   > Se si usano indirizzi IP statici, sarà necessario selezionare la rete da usare e immettere l'indirizzo IP che verrà usato per il nome del cluster.  Se si usa DHCP per gli indirizzi IP, l'indirizzo IP verrà configurato automaticamente.

6. Scegliere **Avanti**.

7. Nella finestra **Seleziona archiviazione** selezionare l'unità aggiuntiva (non il server di controllo del mirroring) che conterrà le condivisioni e **Avanti**.

8. Nella pagina **conferma** verificare la configurazione e fare clic su **Avanti**.

9. Nella pagina **Riepilogo** verrà restituita la configurazione creata.  È possibile selezionare Visualizza report per visualizzare il report della creazione del ruolo file server.

10. In **ruoli** nell'albero della console viene visualizzato il nuovo ruolo creato nell'elenco come nome creato.  Evidenziato, nel riquadro **azioni** a destra scegliere **Aggiungi una condivisione**.

11. Eseguire la procedura guidata di condivisione per inserire quanto segue:

    - Tipo di condivisione che sarà
    - Percorso/percorso in cui sarà condivisa la cartella
    - Nome della condivisione a cui gli utenti si connetteranno
    - Impostazioni aggiuntive, ad esempio l'enumerazione basata sull'accesso, la memorizzazione nella cache, la crittografia e così via
    - Autorizzazioni a livello di file se saranno diverse da quelle predefinite

12. Nella pagina **conferma** verificare gli elementi configurati e selezionare **Crea** per creare la condivisione file server.

13. Nella pagina **risultati** selezionare Chiudi se è stata creata la condivisione.  Se non è stato possibile creare la condivisione, vengono restituiti gli errori generati.

14. Scegliere **Chiudi**.

15. Ripetere questo processo per tutte le condivisioni aggiuntive.