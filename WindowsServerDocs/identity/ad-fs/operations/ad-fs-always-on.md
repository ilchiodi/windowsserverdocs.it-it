---
title: Configurazione di una distribuzione di AD FS con Gruppi di disponibilità AlwaysOn
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/20/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2ea32943f8b46718b90c30024da883c1a35f3888
ms.sourcegitcommit: b649047f161cb605df084f18b573f796a584753b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76162745"
---
# <a name="setting-up-an-ad-fs-deployment-with-alwayson-availability-groups"></a>Configurazione di una distribuzione di AD FS con Gruppi di disponibilità AlwaysOn
Una topologia con distribuzione geografica a disponibilità elevata offre:
* Eliminazione di un singolo punto di errore: con le funzionalità di failover, è possibile ottenere un'infrastruttura ad FS a disponibilità elevata anche se uno dei data center in una parte di un globo diventa inattivo.
* Miglioramento delle prestazioni: è possibile usare la distribuzione consigliata per fornire un'infrastruttura ad alte prestazioni di ADFS

È possibile configurare AD FS per uno scenario a disponibilità elevata con distribuzione geografica.
La guida seguente illustra una panoramica delle AD FS con i gruppi di disponibilità always on di SQL e fornisce indicazioni e considerazioni sulla distribuzione.

## <a name="overview---alwayson-availability-groups"></a>Panoramica-Gruppi di disponibilità AlwaysOn

Per ulteriori informazioni sui gruppi di disponibilità AlwaysOn, vedere [Panoramica di gruppi di disponibilità AlwaysOn (SQL Server)](https://technet.microsoft.com/library/ff877884.aspx)

Dal punto di vista dei nodi di una farm di AD FS SQL Server, il gruppo di disponibilità AlwaysOn sostituisce la singola istanza di SQL Server come database Policy/artefatto.  Il listener del gruppo di disponibilità è quello usato dal client (il servizio token di sicurezza AD FS) per connettersi a SQL.
Il diagramma seguente mostra un SQL Server Farm ADFS con gruppo di disponibilità AlwaysOn.

![server farm con SQL](media/ad-fs-always-on/SQLoverview.png)

Un gruppo di disponibilità Always On è uno o più database utente il cui failover viene eseguito contemporaneamente. Un gruppo di disponibilità è costituito da una replica di disponibilità primaria e da una a quattro repliche secondarie gestite tramite SQL Server spostamento dei dati basato su log per la protezione dei dati senza la necessità di archiviazione condivisa. Ogni replica è ospitata da un'istanza di SQL Server in un nodo diverso del cluster WSFC. Il gruppo di disponibilità e un nome di rete virtuale corrispondente sono registrati come risorse nel cluster WSFC.

Un listener del gruppo di disponibilità sul nodo della replica primaria risponde alle richieste client in ingresso per connettersi al nome della rete virtuale e, in base agli attributi nella stringa di connessione, reindirizza ogni richiesta all'istanza di SQL Server appropriata.
In caso di failover, anziché trasferire la proprietà delle risorse fisiche condivise a un altro nodo, viene sfruttato WSFC per riconfigurare una replica secondaria in un'altra istanza di SQL Server per diventare la replica primaria del gruppo di disponibilità. La risorsa del nome di rete virtuale del gruppo di disponibilità viene quindi trasferita a tale istanza.
In qualsiasi momento, solo una singola istanza di SQL Server può ospitare la replica primaria dei database di un gruppo di disponibilità, tutte le repliche secondarie associate devono trovarsi in un'istanza separata e ogni istanza deve risiedere in nodi fisici distinti.

> [!NOTE] 
> Se i computer sono in esecuzione in Azure, configurare le macchine virtuali di Azure per consentire alla configurazione del listener di comunicare con i gruppi di disponibilità AlwaysOn. Per ulteriori informazioni, [macchine virtuali: listener SQL always on](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener).

Per ulteriori informazioni sulla Gruppi di disponibilità AlwaysOn, vedere [Panoramica dei gruppi di disponibilità always on (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-ver15).

> [!NOTE] 
> Se l'organizzazione richiede il failover tra più data center, è consigliabile creare un database di artefatti in ogni data center e abilitare una cache in background che riduce la latenza durante l'elaborazione della richiesta. Seguire le istruzioni riportate in [ottimizzazione di SQL e riduzione della latenza](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/adfs-sql-latency).

## <a name="deployment-guidance"></a>Guida alla distribuzione

1. <b>Prendere in considerazione il database corretto per gli obiettivi della distribuzione di ad FS.</b> AD FS utilizza un database per archiviare la configurazione e, in alcuni casi, i dati transazionali correlati al Servizio federativo. Per archiviare i dati nel servizio federativo, è possibile usare AD FS software per selezionare il database interno di Windows (WID) o Microsoft SQL Server 2008 o una versione successiva.
La tabella seguente descrive le differenze tra le funzionalità supportate tra un database WID e un database SQL.


| Categoria      | Funzionalità       | Supportato da WID  | Supportato da SQL |
| ------------------ |:-------------:| :---:|:---: |
| Funzionalità di AD FS     | Distribuzione in una server farm federativa | Sì  | Sì |
| Funzionalità di AD FS     | Risoluzione artefatto SAML. Nota: questa operazione non è comune per le applicazioni SAML     |   No | No  |
| Funzionalità di AD FS | Rilevamento riproduzione token SAML/WS-Federation. Nota: è necessario solo quando AD FS riceve token da IDP esterni. Questa operazione non è necessaria se AD FS non funge da IDP.      |    No  | Sì |
| Caratteristiche del database     |   Ridondanza di base del database tramite la replica pull, in cui uno o più server che ospitano una copia di sola lettura del database richiedono modifiche apportate in un server di origine che ospita una copia di lettura/scrittura del database    |   No | No  |
| Caratteristiche del database | Ridondanza dei database con soluzioni a disponibilità elevata, ad esempio il clustering o il mirroring (a livello di database)      |    No  | Sì |

Se si è un'organizzazione di grandi dimensioni con più di 100 relazioni di trust che devono fornire agli utenti interni e agli utenti esterni l'accesso Single Sign-on alle applicazioni o ai servizi federativi, SQL è l'opzione consigliata.

Se si è un'organizzazione con 100 o meno relazioni di trust configurate, WID fornisce dati e ridondanza del servizio federativo (dove ogni server federativo replica le modifiche in altri server federativi nella stessa farm). WID non supporta il rilevamento della riproduzione dei token o la risoluzione degli artefatti e ha un limite di 30 server federativi.
Per ulteriori informazioni sulla pianificazione della distribuzione, visitare [qui](https://docs.microsoft.com/windows-server/identity/ad-fs/design/planning-your-deployment).

## <a name="sql-server-high-availability-solutions"></a>SQL Server soluzioni a disponibilità elevata
Se si usa SQL Server come database di configurazione AD FS, è possibile configurare la ridondanza geografica per la farm di AD FS usando SQL Server replica. La ridondanza geografica replica i dati tra due siti geograficamente distanti, in modo che le applicazioni possano passare da un sito a un altro. In questo modo, in caso di errore di un sito, è comunque possibile disporre di tutti i dati di configurazione disponibili nel secondo sito. 
Se SQL è il database appropriato per gli obiettivi di distribuzione, procedere con questa guida alla distribuzione.

Questa guida illustra le procedure seguenti:
* Distribuisci AD FS
* Configurare AD FS per l'uso di un Gruppi di disponibilità AlwaysOn
* Installare il ruolo clustering di failover
* Eseguire i test di convalida del cluster
* Abilitare i gruppi di disponibilità Always On
* Eseguire il backup di AD FS database
* Creazione di gruppi di disponibilità AlwaysOn
* Aggiungere database nel secondo nodo
* Aggiungere una replica di disponibilità a un gruppo di disponibilità
* Aggiornare la stringa di connessione SQL

## <a name="deploy-ad-fs"></a>Distribuisci AD FS

> [!NOTE] 
> Se i computer sono in esecuzione in Azure, le macchine virtuali devono essere configurate in modo specifico per consentire al listener di comunicare con il gruppo di Always On disponibilità. Per informazioni sulla configurazione, vedere [configurare un servizio di bilanciamento del carico per un gruppo di disponibilità in Azure SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener)


In questa guida alla distribuzione verrà mostrata una farm a due nodi con due server SQL come esempio.
Per distribuire AD FS seguire i collegamenti iniziali seguenti per installare il servizio ruolo AD FS. Per configurare per un gruppo AoA, saranno necessari ulteriori passaggi per il ruolo.
-   [Aggiungere un computer a un dominio](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain)
-   [Registrare un certificato SSL per AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/enroll-an-ssl-certificate-for-ad-fs)
-   [Installare il servizio ruolo di AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/install-the-ad-fs-role-service)


## <a name="configuring-ad-fs-to-use-an-alwayson-availability-group"></a>Configurazione di ADFS per utilizzare un gruppo di disponibilità AlwaysOn

La configurazione di una farm AD FS con i gruppi di disponibilità AlwaysOn richiede una lieve modifica alla procedura di distribuzione del AD FS. Verificare che in ogni istanza del server sia in esecuzione la stessa versione di SQL. Per visualizzare l'elenco completo di prerequisiti, restrizioni e consigli per Gruppi di disponibilità Always On, vedere [qui](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability?view=sql-server-2017#PrerequisitesForDbs).

1.  Prima di configurare i gruppi di disponibilità AlwaysOn, è necessario creare i database che si desidera eseguire il backup.  ADFS crea i relativi database come parte del programma di installazione e configurazione iniziale del primo nodo del servizio federativo di una nuova farm di Server SQL di ADFS.  Specificare il nome host del database per la farm esistente mediante SQL Server. Come parte della configurazione di AD FS, è necessario specificare una stringa di connessione SQL, pertanto sarà necessario configurare la prima farm AD FS per la connessione diretta a un'istanza di SQL (solo temporanea). Per indicazioni specifiche sulla configurazione di una farm di ADFS, inclusa la configurazione di un nodo della farm ADFS con una stringa di connessione SQL server, vedere [configurare un Server federativo](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/configure-a-federation-server).

![Specifica farm](media/ad-fs-always-on/deploymentSpecifyFarm.png)

2.  Verificare la connettività al database usando SSMS, quindi connettersi al nome host del database di destinazione. Se si aggiunge un altro nodo alla farm federativa, connettersi al database di destinazione.
3.  Specificare il certificato SSL per la farm AD FS.

![specificare il certificato SSL](media/ad-fs-always-on/deploymentSpecifySSL.png)

4.  Connettere la farm a un account del servizio o a gMSA.

![Specifica account del servizio](media/ad-fs-always-on/deploymentSpecifyServiceAccount.png)

5.  Completare la configurazione e l'installazione di AD FS farm.

> [!NOTE] 
> SQL Server deve essere eseguito con un account di dominio per l'installazione dei gruppi di disponibilità Always On. Per impostazione predefinita, viene eseguito come sistema locale.

## <a name="install-the-failover-clustering-role"></a>Installare il ruolo clustering di failover
Il ruolo cluster di failover di Windows Server fornisce per ulteriori informazioni sui cluster di failover di Windows Server,
1.  Avviare Server Manager.
2.  Scegliere Aggiungi ruoli e funzionalità dal menu Gestisci.
3.  Nella pagina prima di iniziare selezionare Avanti.
4.  Nella pagina Selezione tipo di installazione selezionare installazione basata su ruoli o basata su funzionalità e quindi fare clic su Avanti.
5.  Nella pagina Selezione server di destinazione selezionare il server SQL in cui si desidera installare la funzionalità e quindi fare clic su Avanti.

![server di destinazione](media/ad-fs-always-on/clusteringDestinationServer.png)

6.  Nella pagina Selezione ruoli server selezionare Avanti.
7.  Nella pagina Selezione funzionalità selezionare la casella di controllo Clustering di failover.

![Selezione funzionalità clustering](media/ad-fs-always-on/clusteringFeature.png)

8.  Nella pagina Conferma selezioni per l'installazione selezionare Installa.
L'installazione della funzionalità Clustering di failover non richiede alcun riavvio del server.
9.  Al termine dell'installazione, fare clic su Chiudi.
10. Ripetere la procedura su tutti i server da aggiungere come nodi del cluster.

## <a name="run-cluster-validation-tests"></a>Eseguire i test di convalida del cluster
1.  In un computer in cui sono installati gli strumenti di gestione del cluster di failover da Strumenti di amministrazione remota del server, oppure in un server in cui è installata la funzionalità Clustering di failover, avviare Gestione cluster di failover. Per eseguire questa operazione in un server, avviare Server Manager, quindi scegliere Gestione cluster di failover dal menu strumenti.
2.  Nel riquadro Gestione cluster di failover fare clic su Convalida configurazione in gestione.
3.  Nella pagina prima di iniziare selezionare Avanti.
4.  Nella pagina Selezione di server o di un cluster, nella casella Immettere il nome, immettere il nome NetBIOS o il nome di dominio completo di un server che si prevede di aggiungere come nodo del cluster di failover e quindi selezionare Aggiungi. Ripetere questo passaggio per ogni server da aggiungere. Per aggiungere più server contemporaneamente, separare i nomi con una virgola o un punto e virgola. Immettere ad esempio i nomi nel formato server1.contoso.com, server2.contoso.com. Al termine, fare clic su Avanti.

![selezione immagine server](media/ad-fs-always-on/clusterValidationServers.png)

5. Nella pagina Opzioni di testing selezionare Esegui tutti i test (scelta consigliata) e quindi fare clic su Avanti.
6. Nella pagina conferma selezionare Avanti.
Nella pagina Convalida in corso verrà visualizzato lo stato dei test in esecuzione.
7. Nella pagina Riepilogo eseguire una delle operazioni seguenti:
- Se i risultati indicano che i test sono stati completati correttamente e la configurazione è adatta per il clustering e si vuole creare il cluster immediatamente, assicurarsi che la casella di controllo Crea il cluster ora utilizzando i nodi convalidati sia selezionata, quindi selezionare Fine. Quindi, continuare con il passaggio 4 della [Procedura creare il cluster di failover](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#create-the-failover-cluster).

![convalidare l'immagine di configurazione](media/ad-fs-always-on/clusterValidationResults.png)

-   Se i risultati indicano la presenza di avvisi o errori, selezionare Visualizza report per visualizzare i dettagli e determinare quali problemi devono essere corretti. Tenere presente che un avviso per un test di convalida specifico indica che quell'aspetto del cluster di failover può essere supportato, ma che potrebbe non rispettare le procedure consigliate.

> [!NOTE]
> Se viene visualizzato un avviso per il test Convalida prenotazione permanente degli spazi di archiviazione, vedere il post di blog sulla [visualizzazione di un avviso relativo alla convalida del cluster di failover di Windows che indica che i dischi in uso non supportano le prenotazioni permanenti per gli spazi di archiviazione](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) per ottenere altre informazioni.
> Per altre informazioni sui test di convalida dell'hardware, vedere [Validate Hardware for a Failover Cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)).

## <a name="create-the-failover-cluster"></a>Creare il cluster di failover

Per completare questo passaggio, verificare che l'account utente con cui si esegue l'accesso soddisfa i requisiti indicati nella sezione [Verificare i prerequisiti](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#verify-the-prerequisites) di questo argomento.
1.  Avviare Server Manager.
2.  Scegliere Gestione cluster di failover dal menu strumenti.
3.  Nel riquadro Gestione cluster di failover fare clic su Crea cluster in gestione.
Verrà visualizzata la Creazione guidata cluster.
4.  Nella pagina prima di iniziare selezionare Avanti.
5.  Se viene visualizzata la pagina Selezione server, nella casella Immettere il nome immettere il nome NetBIOS o il nome di dominio completo di un server che si prevede di aggiungere come nodo del cluster di failover e quindi selezionare Aggiungi. Ripetere questo passaggio per ogni server da aggiungere. Per aggiungere più server contemporaneamente, separare i nomi con una virgola o un punto e virgola. Immettere ad esempio i nomi nel formato server1.contoso.com; server2.contoso.com. Al termine, fare clic su Avanti.

![Crea cluster e seleziona Server](media/ad-fs-always-on/createClusterServers.png)

> [!NOTE]
> Se si sceglie di creare il cluster immediatamente dopo aver eseguito la convalida nella [procedura di convalida della configurazione](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#validate-the-configuration), la pagina Selezione server non sarà visualizzata. I nodi che hanno superato la convalida vengono aggiunti automaticamente alla Creazione guidata cluster e non sarà più necessario immetterli successivamente.

6.  Se si è precedentemente ignorata la convalida, verrà visualizzata la pagina Avviso di convalida. È consigliabile che la convalida del cluster venga eseguita. Solo i cluster che superano tutti i test di convalida sono supportati da Microsoft. Per eseguire i test di convalida, selezionare Sì e quindi fare clic su Avanti. Completare la convalida guidata configurazione come descritto in [convalidare la configurazione](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#validate-the-configuration).
7.  Nella pagina Punto di accesso per l'amministrazione del cluster eseguire le operazioni seguenti:
-   Nella casella Nome cluster immettere il nome che si vuole usare per amministrare il cluster. Prima di procedere, tuttavia, prendere nota delle informazioni seguenti:
 -  Durante la creazione del cluster, questo nome viene registrato come oggetto computer del cluster (anche noto come oggetto nome cluster o oggetto nome cluster) in servizi di dominio Active Directory. Se si specifica un nome NetBIOS per il cluster, l'oggetto CNO viene creato nello stesso percorso in cui risiedono gli altri oggetti computer per i nodi del cluster. Questo può essere il contenitore Computer predefinito o un'unità organizzativa.
 -  Per specificare un percorso diverso per l'oggetto CNO, è possibile immettere il nome distinto di un'unità organizzativa nella casella Nome cluster. Ad esempio: CN = clustername, OU = clusters, DC = contoso, DC = com.
 -  Se un amministratore di dominio ha pre-installato l'oggetto CNO in un'unità organizzativa differente rispetto a quella in cui risiedono i nodi del cluster, specificare il nome distinto fornito dall'amministratore.
- Se il server non dispone di un adattatore di rete configurato per usare DHCP, è necessario configurare uno o più indirizzi IP statici per il cluster di failover. Selezionare la casella di controllo accanto a ogni rete da usare per la gestione del cluster. Selezionare il campo indirizzo accanto a una rete selezionata, quindi immettere l'indirizzo IP che si desidera assegnare al cluster. Questo indirizzo (o questi indirizzi) IP verranno associati al nome del cluster in ambito DNS (Domain Name System).
- Al termine, fare clic su Avanti.

8.  Verificare le impostazioni nella finestra di dialogo Conferma. Per impostazione predefinita, la casella di controllo Aggiungi tutte le risorse di archiviazione idonee al cluster è selezionata. Deselezionarla se si intende procedere in uno dei modi seguenti:
-   Si vuole configurare l'archiviazione in un secondo tempo.
-   Si prevede di creare spazi di archiviazione in cluster mediante Gestione cluster di failover o mediante i cmdlet di Windows PowerShell per il clustering di failover e non si sono ancora creati spazi di archiviazione in Servizi file e archiviazione. Per altre informazioni, vedere [Deploy Clustered Storage Spaces](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)).
9.  Selezionare Avanti per creare il cluster di failover.
10. Nella pagina Riepilogo confermare che il cluster di failover è stato creato correttamente. Se sono presenti avvisi o errori, visualizzare l'output di riepilogo o selezionare Visualizza report per visualizzare il report completo. Fare clic su Fine.
11. Per confermare che il cluster sia stato creato, verificare che il nome sia elencato nell'albero di spostamento di Gestione cluster di failover. È possibile espandere il nome del cluster e quindi selezionare gli elementi in nodi, archiviazione o reti per visualizzare le risorse associate.
Tenere presente che il completamento della replica del nome del cluster nel DNS potrebbe richiedere del tempo. Dopo la corretta registrazione e replica DNS, se si selezionano tutti i server in Server Manager, il nome del cluster dovrebbe essere elencato come server con lo stato di gestibilità online.

![creazione del cluster completata](media/ad-fs-always-on/createClusterComplete.png)

## <a name="enable-always-on-availability-groups-with-sql-server-configuration-manager"></a>Abilitare i gruppi di disponibilità always on con Gestione configurazione SQL Server

1.  Connettersi al nodo WSFC (Windows Server failover cluster) che ospita l'istanza di SQL Server in cui si desidera abilitare Always On gruppi di disponibilità.
2.  Dal menu Start scegliere tutti i programmi, Microsoft SQL Server, Strumenti di configurazione e fare clic su Gestione configurazione SQL Server.
3.  In Gestione configurazione SQL Server fare clic su servizi SQL Server, fare clic con il pulsante destro del mouse su SQL Server (<instance name>), dove <instance name> è il nome di un'istanza del server locale per cui si desidera abilitare Always On gruppi di disponibilità, quindi fare clic su Proprietà.
4.  Selezionare la scheda disponibilità elevata Always On.
5.  Verificare che nel campo Nome cluster di failover Windows sia incluso il nome del cluster di failover locale. Se questo campo è vuoto, questa istanza del server non supporta attualmente Gruppi di disponibilità Always On. Il computer locale non è un nodo del cluster, il cluster WSFC è stato arrestato o questa edizione di SQL Server che non supporta Gruppi di disponibilità Always On.
6.  Selezionare la casella di controllo Abilita gruppi di disponibilità Always On e fare clic su OK.
La modifica viene salvata da Gestione configurazione SQL Server. Successivamente, è necessario riavviare manualmente il servizio SQL Server. In questo modo è possibile scegliere un'ora per il riavvio che meglio soddisfa le esigenze aziendali. Quando il servizio SQL Server viene riavviato, Always On verrà abilitato e la proprietà del server IsHadrEnabled sarà impostata su 1.

![Abilita AoA](media/ad-fs-always-on/enableAoAGroup.png)

## <a name="back-up-ad-fs-databases"></a>Eseguire il backup di AD FS database
Eseguire il backup dei database di configurazione ed artefatto di AD FS con i log delle transazioni completi. Posizionare il backup nella destinazione scelta.
Eseguire il backup dell'artefatto e dei database di configurazione di ADFS.
- Le attività > backup > completa > aggiungere a un file di backup > OK per la creazione

![backup del server](media/ad-fs-always-on/backUpADFS.png)

## <a name="create-new-availability-group"></a>Crea nuovo gruppo di disponibilità

1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica primaria.
2.  Espandere il nodo Always On disponibilità elevata e il nodo gruppi di disponibilità.
3.  Per avviare la Creazione guidata Gruppo di disponibilità, selezionare il comando Creazione guidata Gruppo di disponibilità.
4.  Quando si esegue la procedura guidata per la prima volta, viene visualizzata una pagina Introduzione. Per ignorare questa pagina in futuro, è possibile fare clic su Non visualizzare più questa pagina. Dopo avere letto questa pagina, fare clic su Avanti.
5.  Nella pagina specifica opzioni del gruppo di disponibilità immettere il nome del nuovo gruppo di disponibilità nel campo nome gruppo di disponibilità. Questo nome deve essere un identificatore di SQL Server valido che sia univoco nel cluster e nell'intero dominio. La lunghezza massima consentita per il nome del gruppo di disponibilità è 128 caratteri. e
6.  Successivamente, specificare il tipo di cluster. I tipi di cluster possibili dipendono dalla versione SQL Server e dal sistema operativo. Scegliere WSFC, EXTERNAL o NONE. Per informazioni dettagliate, vedere la pagina [specifica il nome del gruppo di disponibilità](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/specify-availability-group-name-page?view=sql-server-ver15)

![Nome gruppo e cluster AoA](media/ad-fs-always-on/createAoAName.png)

7.  Nella griglia della pagina Seleziona database sono elencati i database utente sull'istanza del server connessa idonei per diventare i database di disponibilità. Selezionare uno o più dei database elencati per usarli nel nuovo gruppo di disponibilità. Questi database diventeranno inizialmente i database primari iniziali.
Per ogni database elencato, nella colonna Dimensioni viene visualizzata la dimensione del database, se nota. Nella colonna stato viene indicato se un determinato database soddisfa i [prerequisiti](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability?view=sql-server-ver15) per i database di disponibilità. Se i prerequisiti non vengono soddisfatti, una breve descrizione dello stato indica il motivo per il database non è idoneo; ad esempio, se non usano il modello di recupero con registrazione completa. Per altre informazioni, fare clic sulla descrizione relativa allo stato.
Se si modifica un database per renderlo idoneo, fare clic su Aggiorna per aggiornare la griglia dei database.
Se il database contiene una chiave master di database, immettere la password per la chiave master di database nella colonna Password.

![Selezionare i database per AoA](media/ad-fs-always-on/createAoASelectDb.png)

8. nella pagina Specifica repliche specificare e configurare una o più repliche per il nuovo gruppo di disponibilità. In questa pagina sono incluse quattro schede. Queste schede vengono presentate nella tabella seguente. Per ulteriori informazioni, vedere l'argomento [pagina Specifica repliche (creazione guidata gruppo di disponibilità: Aggiungi replica)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard?view=sql-server-ver15) .

| TAB      | Breve descrizione       |
| ------------------ |:-------------:|
| Repliche     | Utilizzare questa scheda per specificare ogni istanza di SQL Server in cui verrà ospitata una replica secondaria. Si noti che la replica primaria sarà ospitata nell'istanza del server a cui si è attualmente connessi. |
| Endpoint     | Usare questa scheda per verificare eventuali endpoint del mirroring di database esistenti e, inoltre, se tale endpoint risulta mancante in un'istanza del server i cui account del servizio usano l'autenticazione di Windows, per creare l'endpoint automaticamente.|
| Preferenze di backup | Usare questa scheda per specificare le preferenze di backup per il gruppo di disponibilità nel suo complesso e le priorità di backup per le singole repliche di disponibilità.      |
| Listener     | Usare questa scheda per creare un listener del gruppo di disponibilità. Per impostazione predefinita, la procedura guidata non crea un listener.      |

![specificare i dettagli della replica](media/ad-fs-always-on/createAoAchooseReplica.png)

9. Nella pagina Seleziona sincronizzazione dei dati iniziale specificare come creare e aggiungere i nuovi database secondari al gruppo di disponibilità. Scegliere una delle seguenti opzioni:
-   Seeding automatico
 - SQL Server crea automaticamente le repliche secondarie per ogni database nel gruppo. Il seeding automatico richiede che il percorso dei dati e del file di log sia lo stesso in ogni istanza di SQL Server inclusa nel gruppo. Disponibile in SQL Server 2016 (13. x) e versioni successive. Vedere [inizializzare automaticamente i gruppi di disponibilità always on](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group?view=sql-server-ver15).
- Backup completo del database e del log
 - Selezionare questa opzione se l'ambiente soddisfa i requisiti per l'avvio automatico della sincronizzazione dei dati iniziale. per ulteriori informazioni, vedere [prerequisiti, restrizioni e raccomandazioni, più indietro in questo argomento](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-ver15#Prerequisites).
Se si seleziona Completo, dopo avere creato il gruppo di disponibilità verrà eseguito il backup di ogni database primario e del relativo log delle transazioni su una condivisione di rete e verranno ripristinati i backup su ogni istanza del server in cui è ospitata una replica secondaria. Ogni database secondario verrà quindi aggiunto al gruppo di disponibilità.
Nel campo Specificare un percorso di rete condiviso accessibile da tutte le repliche: specificare una condivisione di backup a cui abbiano accesso in lettura e scrittura tutte le istanze del server che ospitano repliche. Per altre informazioni, vedere la sessione Prerequisiti più indietro in questo argomento. Nel passaggio di convalida la procedura guidata esegue un test per verificare che la posizione di rete sia valida e il test crea nella replica primaria un database "BackupLocDb_" seguito da un GUID, quindi esegue il backup nella posizione di rete specificata e quindi il ripristino nelle repliche secondarie. È possibile eliminare questo database insieme alla cronologia di backup e al file di backup se la procedura guidata non li ha eliminati.
- Solo join
 - Se sono stati preparati manualmente database secondari sulle istanze del server che ospiteranno le repliche secondarie, è possibile selezionare questa opzione. I database secondari esistenti verranno uniti al gruppo di disponibilità tramite la procedura guidata.
- Ignora sincronizzazione dati iniziale
 - Selezionare questa opzione se si desidera usare i backup dei database e del log dei database primari. Per ulteriori informazioni, vedere [avviare lo spostamento dati su un database secondario always on (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server?view=sql-server-ver15).

![scegliere l'opzione di sincronizzazione dati](media/ad-fs-always-on/createAoADataSync.png)

9.  Nella pagina Convalida viene verificato se i valori specificati in questa procedura guidata soddisfano i requisiti della Creazione guidata Gruppo di disponibilità. Per apportare una modifica, fare clic su Precedente per tornare a una pagina precedente della procedura guidata per modificare uno o più valori. Scegliere Avanti per tornare alla pagina Convalida, quindi fare clic su Ripeti convalida.

10. Nella pagina Riepilogo rivedere le scelte effettuate per il nuovo gruppo di disponibilità. Per apportare una modifica, fare clic su Indietro per tornare alla pagina pertinente. Dopo avere apportato la modifica, fare clic su Avanti per tornare alla pagina Riepilogo.

> [!NOTE] 
> Quando l'account del servizio SQL Server di un'istanza del server che ospiterà una nuova replica di disponibilità non esiste già come account di accesso, è necessario creare l'account di accesso tramite la procedura guidata nuovo gruppo di disponibilità. Nella pagina Riepilogo vengono visualizzate le informazioni relative all'account di accesso da creare. Se si fa clic su Fine, viene creato questo account di accesso per l'account del servizio SQL Server e viene concessa l'autorizzazione CONNECT.
> Se si è soddisfatti delle selezioni, è possibile fare clic su Script per creare uno script dei passaggi eseguiti nel corso della procedura guidata. Per creare e configurare il nuovo gruppo di disponibilità, fare quindi clic su Fine.

11. Nella pagina Stato viene visualizzato lo stato di avanzamento dei passaggi per la creazione del gruppo di disponibilità (configurazione di endpoint, creazione del gruppo di disponibilità e creazione di un join della replica secondaria al gruppo).
12. Al termine di questi passaggi, nella pagina Risultati vengono visualizzati i relativi risultati. Se tutti questi passaggi sono stati eseguiti correttamente, il nuovo gruppo di disponibilità è completamente configurato. Se si verifica un errore durante uno dei passaggi, potrebbe essere necessario completare manualmente la configurazione o usare una procedura dettagliata per il passaggio errato. Per informazioni sulla causa di un determinato errore, fare clic sul collegamento "Errore" nella colonna Risultato.
Al termine della procedura guidata, fare clic su Chiudi per uscire.

![Convalida completata](media/ad-fs-always-on/createAoAValidation.png)

## <a name="add-databases-on-secondary-node"></a>Aggiungere database nel nodo secondario

1.  Ripristinare il database degli artefatti tramite l'interfaccia utente nel nodo secondario usando i file di backup creati.
ripristino ![tramite interfaccia utente](media/ad-fs-always-on/restoreDB.png)

2. Ripristinare il database in uno stato NON di ripristino.
ripristino ![con](media/ad-fs-always-on/restoreNonRecovery.png) non di ripristino

3. Ripetere il processo per ripristinare il database di configurazione.

## <a name="join-availability-replica-to-an-availability-group"></a>Aggiungere una replica di disponibilità a un gruppo di disponibilità

1.  In Esplora oggetti connettersi all'istanza del server in cui viene ospitata la replica secondaria e fare clic sul nome del server per espandere il relativo albero.
2.  Espandere il nodo Always On disponibilità elevata e il nodo gruppi di disponibilità.
3.  Selezionare il gruppo di disponibilità della replica secondaria a cui si è connessi.
4.  Fare clic con il pulsante destro del mouse sulla replica secondaria e scegliere Crea un join del gruppo di disponibilità.
5.  In questo modo verrà aperta la finestra di dialogo Creare un join della replica al gruppo di disponibilità.
6.  Per creare un join della replica secondaria al gruppo di disponibilità, fare clic su OK.

![Join replica secondaria](media/ad-fs-always-on/jointoAoA.png)

## <a name="update-the-sql-connection-string"></a>Aggiornare la stringa di connessione SQL
Infine, utilizzare PowerShell per modificare le proprietà di AD FS per aggiornare la stringa di connessione SQL per utilizzare l'indirizzo DNS del listener del gruppo di disponibilità AlwaysOn.
Eseguire la modifica del database di configurazione in ogni nodo e riavviare il servizio ADFS in tutti i nodi ADFS. Il valore iniziale del catalogo viene modificato in base alla versione della farm.

```
PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService
PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”
PS:\>$temp.put()
PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”
```
