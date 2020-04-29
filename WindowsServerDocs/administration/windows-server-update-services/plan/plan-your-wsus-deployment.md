---
title: Pianificare la distribuzione di WSUS
description: 'Argomento di Windows Server Update Services (WSUS): Panoramica del processo di pianificazione della distribuzione con collegamenti agli argomenti correlati'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 35865398-b011-447a-b781-1c52bc0c9e3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/24/2018
ms.openlocfilehash: 0208e23b94b5e7c5012bc99eabf71aa0c7ad944c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "82037140"
---
# <a name="plan-your-wsus-deployment"></a>Pianificare la distribuzione di WSUS

>Si applica a: Windows Server 2019, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il passaggio iniziale nella distribuzione di Windows Server Update Services (WSUS) prevede l'assunzione di decisioni importanti come, ad esempio, stabilire lo scenario di distribuzione WSUS, scegliere la topologia di rete e comprendere i requisiti di sistema. L'elenco di controllo seguente descrive tutti i passaggi correlati alla preparazione della distribuzione.

|Attività|Descrizione|
|----|--------|
|[1.1. Esaminare le considerazioni e i requisiti di sistema](#11-review-considerations-and-system-requirements)|Esaminare l'elenco di considerazioni e requisiti di sistema per verificare di disporre di tutto l'hardware e il software necessario per la distribuzione di Windows Server Update Services.|
|[1.2. Scegliere uno scenario di distribuzione WSUS](#12-choose-a-wsus-deployment-scenario)|Decidere lo scenario di distribuzione WSUS da utilizzare.|
|[1.3. Scegliere una strategia di archiviazione WSUS](#13-choose-a-wsus-storage-strategy)|Decidere la strategia di archiviazione WSUS più adatta alla distribuzione.|
|[1.4. Scegliere le lingue degli aggiornamenti WSUS](#14-choose-wsus-update-languages)|Decidere le lingue degli aggiornamenti WSUS da installare.|
|[1.5. Pianificare i gruppi di computer WSUS](#15-plan-wsus-computer-groups)|Pianificare l'approccio dei gruppi di computer WSUS utilizzati per la distribuzione.|
|[1.6. Pianificare le considerazioni sulle prestazioni WSUS: Servizio trasferimento intelligente in background](#16-plan-wsus-performance-considerations)|Pianificare una struttura WSUS per le prestazioni ottimizzate.|
|[1.7. Pianificare le impostazioni per gli aggiornamenti automatici](#17-plan-automatic-updates-settings)|Pianificare come configurare le impostazioni degli aggiornamenti automatici per lo scenario in uso.|

## <a name="11-review-considerations-and-system-requirements"></a>1.1. Rivedere le considerazioni e i requisiti di sistema

### <a name="system-requirements"></a>Requisiti di sistema

I requisiti hardware e software del database sono basati sul numero di computer client aggiornati nell'organizzazione.  Prima di abilitare il ruolo server WSUS, verificare che il server soddisfi i requisiti di sistema e confermare di disporre delle autorizzazioni necessarie per completare l'installazione aderendo alle linee guida riportate di seguito.

-   I requisiti hardware del server per abilitare il ruolo WSUS sono associati ai requisiti hardware. I requisiti hardware minimi per WSUS sono:

    -   **Processore:** processore x64 a 1,4 gigahertz (GHz) (consigliati almeno 2 Ghz)

    -   **Memoria:** WSUS richiede 2 GB di RAM aggiuntiva oltre a quanto richiesto dal server e da tutti gli altri servizi o software.

    -   **Spazio disponibile su disco:** consigliati almeno 40 GB

    -   **Scheda di rete:** almeno 100 megabit al secondo (Mbps) (consigliato 1 GB)

> [!NOTE] 
> Queste linee guida presuppongono che i client WSUS vengano sincronizzati con il server ogni otto ore per un aggiornamento cumulativo di 30.000 client. Se la sincronizzazione avviene con maggiore frequenza, si verificherà un aumento corrispondente nel carico del server.  

-   Requisiti software:

    -   Per visualizzare i report, WSUS richiede [Microsoft Report Viewer Redistributable 2008](https://www.microsoft.com/download/details.aspx?id=6576). In Windows Server 2016, WSUS richiede [Microsoft Report Viewer Runtime 2012](https://www.microsoft.com/download/details.aspx?id=35747)

-   Se si installano ruoli o aggiornamenti software che richiedono il riavvio del server al completamento dell'installazione, riavviare il server prima di abilitare il ruolo server WSUS.

-   Microsoft .NET Framework 4.0 deve essere installato nel server in cui verrà installato il ruolo server WSUS.

-   L'account NT Authority\Servizio di rete deve disporre dell'autorizzazione Controllo completo per le cartelle seguenti in modo che lo snap-in  Amministrazione di Windows Server Update Services venga visualizzato correttamente:

    -   %windir%\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files

        > [!NOTE]
        > Questo percorso potrebbe non essere disponibile prima dell'installazione del ruolo Server Web che contiene Internet Information Services (IIS).

    -   %windir%\Temp

-   Verificare che l'account che si intende utilizzare per l'installazione di Windows Server Update Services sia membro del gruppo Administrators locale.

### <a name="installation-considerations"></a>Considerazioni sull'installazione

Durante il processo di installazione, gli elementi seguenti verranno installati per impostazione predefinita:

-   .NET API e cmdlet di Windows PowerShell

-   Database interno di Windows, utilizzato da Windows Server Update Services

-   I servizi utilizzati da Windows Server Update Services, ovvero:

    -   Servizio aggiornamento

    -   Servizio Web gestione rapporti

    -   Servizio Web client

    -   Servizio Web di autenticazione Web semplice

    -   Servizio di sincronizzazione del server

    -   Servizio Web di autenticazione DSS

### <a name="features-on-demand-considerations"></a>Considerazioni su Funzionalità su richiesta

Tenere presente che la configurazione di computer client (inclusi i server) per l'aggiornamento tramite WSUS comporta le limitazioni seguenti:

1. I ruoli del server i cui payload sono stati rimossi usando Funzionalità su richiesta non possono essere installati su richiesta da Microsoft Update. È necessario fornire un'origine dell'installazione nel momento in cui si tenta di installare tali ruoli del server oppure configurare un'origine per Funzionalità su richiesta in Criteri di gruppo.

2. Le edizioni client di Windows non saranno in grado di installare .NET 3.5 su richiesta dal Web. Le stesse considerazioni relative ai ruoli del server si applicano a .NET 3.5.

   > [!NOTE]
   > La configurazione di un'origine dell'installazione di Funzionalità su richiesta non riguarda WSUS. Per informazioni su come configurare le funzionalità, vedere [Configurare Funzionalità su richiesta in Windows Server](https://technet.microsoft.com/library/jj127275.aspx).

3. I dispositivi aziendali che eseguono Windows 10, versione 1709 o versione 1803, non possono installare Funzionalità su richiesta direttamente da WSUS. Per installare Funzionalità su richiesta, [creare un file di funzionalità (archivio affiancato)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127275%28v=ws.11%29#create-a-feature-file-or-side-by-side-store) oppure ottenere il pacchetto di Funzionalità su richiesta da una delle origini seguenti:
   - [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter) (VLSC): è necessario l'accesso ai contratti multilicenza
   - Portale OEM: è necessario l'accesso a OEM
   - Download MSDN: è necessario un abbonamento MSDN

     I pacchetti di Funzionalità su richiesta ottenuti singolarmente possono essere installati mediante le [opzioni della riga di comando di Gestione e manutenzione immagini distribuzione](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options).

### <a name="wsus-database-requirements"></a>Requisiti del database di WSUS
Windows Server Update Services richiede uno dei database seguenti:

-   Database interno di Windows

-   Qualsiasi versione supportata di Microsoft SQL Server. Per altre informazioni, vedi [Criteri relativi al ciclo di vita Microsoft](https://aka.ms/sqllifecycle).

Le edizioni seguenti di SQL Server sono supportate da WSUS:

-   Standard

-   Funzionalità per le aziende

-   Express

> [!NOTE]
> In SQL Server Express 2008 R2 la dimensione del database è limitata a 10 GB. Tale dimensione è in genere sufficiente per WSUS, anche se l'uso di questo database al posto di Database interno di Windows non offre vantaggi apprezzabili. Database interno di Windows richiede almeno 2 GB di memoria RAM in aggiunta ai requisiti di sistema standard di Windows Server.

È possibile installare il ruolo WSUS in un computer diverso dal computer del server database. In tal caso, è necessario applicare i criteri aggiuntivi seguenti.

1.  Il server database non può essere configurato come controller di dominio.

2.  Il server WSUS non può eseguire Servizi Desktop remoto.

3.  Il server database deve far parte dello stesso dominio Active Directory del server WSUS o deve disporre di una relazione di trust con il dominio Active Directory del server WSUS.

4.  Il server WSUS e il server database devono trovarsi nello stesso fuso orario o essere sincronizzati alla stessa ora UTC (ora di Greenwich).

## <a name="12-choose-a-wsus-deployment-scenario"></a>1.2. Scegliere lo scenario di distribuzione WSUS
Questa sezione illustra le funzionalità di base di tutte le distribuzioni di Windows Server Update Services. Utilizzare questa sezione per acquisire familiarità con la distribuzione semplice di un singolo server WSUS, oltre che con scenari più complessi, quali una gerarchia di server WSUS o un server WSUS in un segmento di rete isolato.

### <a name="simple-wsus-deployment"></a>Distribuzione WSUS semplice
La distribuzione WSUS di base è costituita da un server all'interno del firewall aziendale che serve i computer client di una rete Intranet privata. Il server WSUS si connette a Microsoft Update per scaricare gli aggiornamenti. Questa operazione è denominata *sincronizzazione*. Durante la sincronizzazione, WSUS determina se sono stati resi disponibili aggiornamenti dall'ultima sincronizzazione effettuata. In caso di prima sincronizzazione, tutti gli aggiornamenti sono resi disponibili per il download.

> [!NOTE]
> La sincronizzazione iniziale può richiedere più di un'ora. Le sincronizzazioni successive dovrebbero essere notevolmente più rapide.

Per impostazione predefinita, per ottenere gli aggiornamenti da Microsoft il server WSUS usa la porta 80 per il protocollo HTTP e la porta 443 per il protocollo HTTPS. Se è presente un firewall aziendale tra la rete e Internet, è necessario aprire le porte sul server che comunica direttamente con Microsoft Update. Se si prevede di usare porte personalizzate per la comunicazione, aprire le porte desiderate. È possibile configurare più server WSUS da sincronizzare con un server WSUS padre. Per impostazione predefinita, per fornire gli aggiornamenti alle workstation client il server WSUS usa la porta 8530 per il protocollo HTTP e la porta 8531 per il protocollo HTTPS.

### <a name="multiple-wsus-servers"></a>Più server WSUS
Gli amministratori possono distribuire più server WSUS che sincronizzano tutti i contenuti all'interno della rete Intranet dell'organizzazione. È possibile esporre un solo server a Internet, ovvero l'unico server che esegue il download degli aggiornamenti da Microsoft Update. Il server è configurato come server upstream, ovvero l'origine a cui i server downstream eseguono la sincronizzazione. Quando applicabile, i server possono essere situati in una rete geograficamente dispersa per fornire la massima connettività a tutti i computer client.

### <a name="disconnected-wsus-server"></a>Server WSUS disconnesso
Se l'accesso dei computer a Internet è limitato da criteri aziendali o da altre condizioni, gli amministratori possono configurare un server interno per l'esecuzione di WSUS. Un esempio in tal senso è un server connesso alla rete Intranet, ma isolato da Internet. Dopo aver scaricato, testato e approvato gli aggiornamenti sul server, l'amministratore può esportare i metadati e il contenuto degli aggiornamenti in un DVD. I metadati e il contenuto degli aggiornamenti viene importato dal DVD nei server che eseguono WSUS all'interno della rete Intranet.

### <a name="wsus-server-hierarchies"></a>Gerarchie di server WSUS
È possibile creare complesse gerarchie di server WSUS. Poiché è possibile sincronizzare un server WSUS con un altro server WSUS anziché con Microsoft Update, è necessario che un solo server WSUS sia connesso a Microsoft Update. Quando i server WSUS vengono collegati, si creano un server WSUS upstream e un server WSUS downstream. Una gerarchia di server WSUS offre i vantaggi riportati di seguito.

-   È possibile scaricare gli aggiornamenti da Internet un'unica volta e poi distribuirli ai computer client tramite server downstream. Questo metodo consente di risparmiare larghezza di banda per la connessione Internet aziendale.

-   È possibile scaricare gli aggiornamenti in un server WSUS fisicamente più vicino ai computer client, ad esempio, in succursali.

-   È possibile configurare server WSUS separati per computer client che utilizzano lingue diverse di prodotti Microsoft.

-   È possibile adattare la soluzione WSUS nel caso di un'organizzazione di grandi dimensioni con un numero di computer client superiore a quello che un solo server WSUS è in grado di gestire in modo efficace.

> [!NOTE] 
> Si consiglia di non creare una gerarchia di server WSUS profonda più di tre livelli. La presenza dei singoli livelli comporta l'aggiunta di tempo per propagare gli aggiornamenti nei server connessi. Sebbene non esistano limiti teorici a una gerarchia, Microsoft ha testato solo distribuzioni con gerarchie profonde cinque livelli.
>
> Inoltre, la versione dei server downstream deve essere la stessa o una versione precedente di WSUS come origine di sincronizzazione del server upstream.

È possibile connettere i server WSUS in modalità autonoma, per l'amministrazione distribuita, o in modalità di replica, per l'amministrazione centralizzata. Non è necessario distribuire una gerarchia di server che utilizzi una sola modalità: è possibile distribuire una soluzione WSUS con server WSUS sia autonomi che di replica.

#### <a name="autonomous-mode"></a>Modalità autonoma
La modalità autonoma, denominata anche amministrazione distribuita, è l'opzione di installazione predefinita per WSUS. In tale modalità un server upstream di Windows Server Update Services condivide gli aggiornamenti con i server downstream durante la sincronizzazione. I server downstream di Windows Server Update Services vengono amministrati separatamente e non ricevono lo stato di approvazione degli aggiornamenti o le informazioni sul gruppo di computer dal server upstream. Il modello di gestione distribuita consente a ogni amministratore di server WSUS di selezionare le lingue degli aggiornamenti, creare gruppi di computer, assegnare i computer ai gruppi, eseguire i test e approvare gli aggiornamenti, infine verificare che vengano installati gli aggiornamenti corretti ai gruppi di computer appropriati. Nell'immagine seguente viene illustrato come distribuire server di Windows Server Update Services autonomi in un ambiente di succursali:

#### <a name="replica-mode"></a>Modalità di replica
La modalità di replica, denominata anche amministrazione centralizzata, prevede la presenza di un server upstream di Windows Server Update Services che condivide aggiornamenti, stato di approvazione e gruppi di computer con i server downstream. I server di replica ereditano le approvazioni degli aggiornamenti e non sono amministrati separatamente dal server WSUS upstream. Nell'immagine seguente viene illustrato come distribuire server WSUS di replica in un ambiente di succursali:

> [!NOTE]
> Se si configurano più server di replica per la connessione a un singolo server WSUS upstream, non pianificare di eseguire la sincronizzazione contemporaneamente su ogni server di replica. In questo modo, si eviteranno picchi di utilizzo improvvisi della larghezza di banda.

### <a name="branch-offices"></a>Succursali
La funzionalità Succursale di Windows può essere utilizzata per ottimizzare la distribuzione di Windows Server Update Services. Questo tipo di distribuzione offre i vantaggi seguenti.

1.  Aiuta a ridurre l'utilizzo di collegamenti WAN e migliora la velocità di risposta delle applicazioni. Per abilitare l'accelerazione di BranchCache per il contenuto fornito dal server WSUS, installare la funzionalità BranchCache nel server e nei client e verificare che il servizio BranchCache sia stato avviato. Non sono necessarie altre operazioni.

2.  È anche possibile utilizzare la funzionalità Succursale per succursali caratterizzate da connessione con larghezza di banda ridotta per l'ufficio centrale, ma elevata verso Internet. In questo caso, è preferibile configurare i server downstream di WSUS in modo da ottenere informazioni sugli aggiornamenti da installare dal server WSUS centrale, ma scaricare gli aggiornamenti da Microsoft Update.

### <a name="network-load-balancing"></a>Bilanciamento carico di rete
Bilanciamento carico di rete aumenta l'affidabilità e le prestazioni della rete WSUS utilizzata. È possibile configurare più server WSUS che condividono un unico cluster di failover che esegue SQL Server, ad esempio SQL Server 2008 R2 SP1. Anziché l'installazione di Database interno di Windows fornita da WSUS, in questa configurazione è necessario utilizzare un'installazione completa di SQL Server e installare il ruolo del database in tutti i server front-end di WSUS. È inoltre possibile impostare l'utilizzo di un file system distribuito (DFS) per l'archiviazione del contenuto di tutti i server WSUS.

**Configurazione di WSUS per Bilanciamento carico di rete:** rispetto alla configurazione di WSUS 3.2 per Bilanciamento carico di rete, non sono più necessari parametri e una chiamata di configurazione speciale per configurare WSUS per Bilanciamento carico di rete. È necessario solo configurare ogni server WSUS, tenendo presenti le considerazioni seguenti.

-   WSUS deve essere configurato usando l'opzione del database SQL anziché di Database interno di Windows.

-   Se gli aggiornamenti vengono archiviati localmente, la stessa cartella dei contenuti deve essere condivisa tra i server WSUS che condividono lo stesso database SQL.

-   La configurazione di WSUS deve essere effettuata in modo seriale. Le attività successive all'installazione non possono essere eseguite su più server contemporaneamente se viene condiviso lo stesso database SQL.

### <a name="wsus-deployment-with-roaming-client-computers"></a>Distribuzione WSUS con computer client in roaming
Se la rete include utenti mobili che effettuano l'accesso da percorsi diversi, è possibile configurare Windows Server Update Services per consentire agli utenti in roaming di aggiornare i propri computer client dal server WSUS geograficamente più vicino. Ad esempio, è possibile distribuire un server WSUS in ogni area e usare una subnet DNS diversa per ogni area. Tutti i computer client possono essere indirizzati allo stesso server WSUS che si risolve nel server WSUS fisico più vicino in ogni subnet.

## <a name="13-choose-a-wsus-storage-strategy"></a>1.3. Scegliere la strategia di archiviazione WSUS
Windows Server Update Services (WSUS) utilizza due tipi di sistemi di archiviazione: un database per archiviare la configurazione WSUS e aggiornare i metadati; un file system locale facoltativo per archiviare i file di aggiornamento. Prima dell'installazione di Windows Server Update Services, è necessario decidere come implementare l'archiviazione.

Gli aggiornamenti si compongono di due parti: i metadati che descrivono l'aggiornamento e i file necessari per installare l'aggiornamento. I metadati degli aggiornamenti sono generalmente molto più piccoli degli aggiornamenti effettivi e vengono archiviati nel database WSUS. I file degli aggiornamenti sono archiviati in un server WSUS locale o in un server Web di Microsoft Update.

### <a name="wsus-database"></a>Database WSUS
WSUS richiede un database per ogni server WSUS. WSUS supporta l'utilizzo di database residenti su computer diversi dal server WSUS con alcune restrizioni. Per un elenco dei database supportati e delle limitazioni per i database remoti, vedi la sezione 1.1 Rivedere le considerazioni e i requisiti di sistema in questa guida.

Nel database WSUS vengono archiviate le informazioni seguenti:

-   informazioni sulla configurazione dei server WSUS

-   i metadati che descrivono ogni aggiornamento

-   informazioni su computer client, aggiornamenti e interazioni

Se si installano più server WSUS, è necessario mantenere un database separato per ogni server WSUS, a prescindere se si tratti di un server autonomo o di replica. Non è possibile archiviare più database WSUS in una singola istanza di SQL Server, tranne che in cluster di Bilanciamento carico di rete che utilizzano il failover di SQL Server.

SQL Server, SQL Server Express e Database interno di Windows offrono le stesse caratteristiche di prestazioni per una configurazione con un solo server, in cui database e servizio WSUS risiedono sullo stesso computer. La configurazione con un solo server è in grado di supportare diverse migliaia di computer client di Windows Server Update Services.

> [!NOTE]
> Non tentare di gestire Windows Server Update Services accedendo direttamente al database. La gestione diretta può provocare il danneggiamento del database. Sebbene possa non essere immediatamente evidente, il danneggiamento può impedire l'aggiornamento alla versione successiva del prodotto. È possibile gestire WSUS tramite la console o le API (Application Programming Interface) di WSUS.

#### <a name="wsus-with-windows-internal-database"></a>WSUS con Database interno di Windows
L'installazione guidata crea e utilizza, per impostazione predefinita, un database interno di Windows denominato SUSDB.mdf. Il database si trova nella cartella %windir%\wid\data\, dove %windir% è l'unità locale in cui è installato il software del server WSUS.

> [!NOTE]
> Database interno di Windows è stato introdotto in Windows Server 2008.

WSUS supporta solo l'autenticazione di Windows per il database. Non è possibile utilizzare l'autorizzazione di SQL Server. Se si utilizza Database interno di Windows per il database WSUS, il programma di installazione di Windows Server Update Services crea un'istanza di SQL Server denominata server\Microsoft##WID, dove server è il nome del computer. Con entrambe le opzioni di database, il programma di installazione di Windows Server Update Services crea un database denominato SUSDB. Il nome del database non è configurabile.

È consigliabile utilizzare Database interno di Windows nei seguenti casi:

-   l'organizzazione non ha già acquistato SQL Server e non ne ha bisogno per altre applicazioni;

-   l'organizzazione non ha bisogno di una soluzione WSUS per il bilanciamento del carico di rete;

-   si desidera distribuire più server WSUS, ad esempio, in succursali. In questo caso, è opportuno prendere in considerazione l'utilizzo di Database interno di Windows nei server secondari, anche se si utilizza SQL Server per il server radice di Windows Server Update Services. Dal momento che ogni server WSUS richiede un'istanza distinta di SQL Server, se una sola istanza di SQL Server gestisce più server WSUS si verificheranno velocemente problemi di prestazioni del database.

Database interno di Windows non fornisce un'interfaccia utente o strumenti di gestione del database. Se si seleziona questo database per WSUS, è necessario utilizzare strumenti esterni per gestire il database. Per altre informazioni, vedi:

-   [Backup e ripristino di dati WSUS con backup del server](https://technet.microsoft.com/library/dd939904(WS.10).aspx)

-   [Reindicizzare il database WSUS](https://technet.microsoft.com/library/dd939795(WS.10).aspx)

#### <a name="wsus-with-sql-server"></a>WSUS con SQL Server
È consigliabile utilizzare Windows Server Update Services con SQL Server nei seguenti casi:

1.  è necessaria una soluzione WSUS per il bilanciamento del carico di rete;

2.  è già installata almeno un'istanza di SQL Server;

3.  non è possibile eseguire il servizio SQL Server in un account locale non di sistema oppure utilizzando l'autenticazione di SQL Server. WSUS supporta solo l'autenticazione di Windows.

### <a name="wsus-update-storage"></a>Archiviazione degli aggiornamenti WSUS
Quando gli aggiornamenti vengono sincronizzati con il server WSUS, i metadati e i file di aggiornamento vengono archiviati in due posizioni diverse. I metadati vengono archiviati nel database WSUS. I file di aggiornamento possono essere archiviati nel server WSUS o nei server Microsoft Update, in base alla configurazione delle opzioni di sincronizzazione. Se si sceglie di archiviare i file di aggiornamento nel server WSUS, i computer client scaricano gli aggiornamenti approvati dal server WSUS locale. In caso contrario, i computer client scaricano gli aggiornamenti approvati direttamente da Microsoft Update. L'opzione più indicata per la propria organizzazione varia in base alle larghezze di banda di rete per Internet e in Intranet nonché alla disponibilità di archiviazione locale.

È possibile selezionare una soluzione di archiviazione degli aggiornamenti diversa per ogni server WSUS distribuito.

#### <a name="local-wsus-server-storage"></a>Archiviazione sul server WSUS locale
Quando si installa e configura Windows Server Update Services, i file degli aggiornamenti vengono archiviati localmente per impostazione predefinita. Questa opzione consente di risparmiare larghezza di banda per la connessione aziendale a Internet in quanto che i computer client scaricano gli aggiornamenti direttamente dal server WSUS locale.

Per archiviare tutti gli aggiornamenti, è necessario che il server disponga di sufficiente spazio su disco. Per l'archiviazione locale degli aggiornamenti, WSUS richiede almeno 20 GB; in base alle variabili testate, tuttavia, si consiglia di prenotare 30 GB.

#### <a name="remote-storage-on-microsoft-update-servers"></a>Archiviazione remota nei server di Microsoft Update
È possibile archiviare gli aggiornamenti in remoto nei server di Microsoft Update. Questa opzione è utile quando la maggioranza dei computer client si collega al server WSUS tramite una connessione WAN lenta, ma per Internet utilizza una connessione con larghezza di banda elevata.

In questo caso, il server WSUS radice esegue la sincronizzazione con Microsoft Update e riceve i metadati degli aggiornamenti. Dopo aver approvato gli aggiornamenti, i computer client scaricano gli aggiornamenti approvati dai server di Microsoft Update.

## <a name="14-choose-wsus-update-languages"></a>1.4. Scegliere le lingue degli aggiornamenti WSUS
Quando si distribuisce una gerarchia di server di Windows Server Update Services, è necessario determinare in quali lingue fornire gli aggiornamenti nell'organizzazione. È consigliabile configurare il server WSUS radice in modo che scarichi gli aggiornamenti in tutte le lingue utilizzate nell'organizzazione.

La sede principale, ad esempio, potrebbe aver bisogno degli aggiornamenti in lingua inglese e francese, laddove una succursale ne ha bisogno in inglese, francese e tedesco e un'altra in inglese e spagnolo. In questo caso, si può configurare il server WSUS radice in modo che scarichi gli aggiornamenti in lingua inglese, francese, tedesco e spagnolo. Si può quindi configurare il server WSUS della prima succursale in modo che scarichi gli aggiornamenti solo in inglese, francese e tedesco e quello della seconda succursale in modo che scarichi gli aggiornamenti solo in inglese e spagnolo.

La pagina **Scelta lingue** della configurazione guidata WSUS consente di ricevere gli aggiornamenti in tutte le lingue o da un sottoinsieme di lingue. Se si seleziona un sottoinsieme di lingue sarà possibile risparmiare spazio su disco. È tuttavia IMPORTANTE scegliere tutte le lingue necessarie per tutti i server e i computer client downstream del server WSUS.

Di seguito sono riportate alcune note IMPORTANTI sulle lingue degli aggiornamenti che è opportuno ricordare prima di configurare questa opzione:

-   Includere sempre la lingua inglese, oltre alle altre lingue eventualmente necessarie nell'organizzazione. Tutti gli aggiornamenti si basano sui Language Pack inglesi.

-   I server downstream e i computer client non ricevono tutti gli aggiornamenti di cui hanno bisogno se non sono state selezionate tutte le lingue necessarie per il server upstream. Accertarsi di selezionare tutte le lingue richieste da tutti i computer client associati ai server downstream.

-   È in genere consigliabile scaricare gli aggiornamenti in tutte le lingue nel server WSUS radice che effettua la sincronizzazione a Microsoft Update. In questo modo si ha la certezza che tutti i server downstream e i computer client riceveranno gli aggiornamenti nelle lingue necessarie.

Se si archiviano gli aggiornamenti a livello locale ed è stato configurato un server WSUS per scaricare gli aggiornamenti in un numero limitato di lingue, è possibile notare la presenza di aggiornamenti in lingue diverse da quelle specificate. Molti file degli aggiornamenti sono raggruppamenti di diverse lingue, che comprendono almeno una delle lingue specificate nel server.

**Server upstream**

> [!NOTE]
> Configurare i server upstream per sincronizzare gli aggiornamenti in tutte le lingue richieste dai server di replica downstream. Non si riceveranno notifiche per gli aggiornamenti necessari nelle lingue non sincronizzate.

Gli aggiornamenti verranno visualizzati con l'indicazione **Non applicabile** nei computer client che richiedono la lingua. Per evitare che ciò accada, assicurarsi che tutte le lingue del sistema operativo siano incluse nelle opzioni di sincronizzazione del server WSUS. È possibile visualizzare tutte le lingue del sistema operativo passando alla visualizzazione **Computer** della console di amministrazione di WSUS e ordinando i computer in base alla lingua del sistema operativo. Tuttavia, può essere consigliabile includere più lingue, se sono presenti applicazioni Microsoft in più di una lingua (ad esempio, se è installata la versione italiana di Microsoft Word in computer che usano la versione inglese di Windows 8).

La scelta delle lingue per un server upstream non equivale alla scelta delle lingue per un server downstream. Le procedure seguenti illustrano le differenze.

#### <a name="to-choose-update-languages-for-a-server-synchronizing-from-microsoft-update"></a>Per scegliere le lingue degli aggiornamenti per un server che esegue la sincronizzazione da Microsoft Update

1.  Nella Configurazione guidata WSUS:

    -   Per ottenere gli aggiornamenti in tutte le lingue, fare clic su **Scarica gli aggiornamenti in tutte le lingue, incluse le nuove**.

    -   Per ottenere gli aggiornamenti solo per determinate lingue, fare clic su **Scarica aggiornamenti solo nelle lingue seguenti**e quindi selezionare le lingue per le quali si vogliono gli aggiornamenti.

#### <a name="to-choose-update-languages-for-a-downstream-server"></a>Per scegliere le lingue degli aggiornamenti per un server downstream

1.  Se il server upstream è stato configurato per scaricare i file di aggiornamento in un sottoinsieme di lingue: Nella Configurazione guidata WSUS fare clic su **Scarica aggiornamenti solo nelle lingue seguenti** (il server upstream supporta solo le lingue contrassegnate con un asterisco) e quindi selezionare le lingue per le quali si vogliono gli aggiornamenti.

> [!NOTE]
> Eseguire questa operazione anche se si vuole che il server downstream scarichi le stesse lingue del server upstream.

2. Se il server upstream è stato configurato per scaricare i file di aggiornamento in tutte le lingue: Nella Configurazione guidata WSUS fare clic su **Scarica aggiornamenti in tutte le lingue supportate dal server upstream**.

> [!NOTE]
> Eseguire questa operazione anche se si vuole che il server downstream scarichi le stesse lingue del server upstream. Questa impostazione fa in modo che il server upstream scarichi gli aggiornamenti in tutte le lingue, incluse quelle non originariamente configurate per il server upstream. Se si aggiungono lingue al server upstream, è necessario copiare i nuovi aggiornamenti nei relativi server di replica.
>
> Se si modificano le opzioni relative alle lingue solo nel server upstream, il numero di aggiornamenti approvati nel server centrale potrebbe non corrispondere al numero di aggiornamenti approvati nei server di replica.

## <a name="15-plan-wsus-computer-groups"></a>1.5. Pianificare gruppi di computer WSUS
Windows Server Update Services consente di applicare gli aggiornamenti a gruppi di computer client in modo da garantire che determinati computer ottengano sempre gli aggiornamenti corretti negli orari più appropriati. Se, ad esempio, tutti i computer del reparto Contabilità sono configurati in modo specifico, è possibile impostare un gruppo per il reparto, stabilire gli aggiornamenti necessari per i computer e l'orario in cui installarli, quindi utilizzare i rapporti WSUS per valutare gli aggiornamenti effettuati.

> [!NOTE]
> Se un server WSUS viene eseguito in modalità di replica, non è possibile creare gruppi di computer. Tutti i gruppi di computer necessari per i computer client del server di replica devono essere creati nel server WSUS radice della gerarchia. Per altre informazioni sulla modalità di replica, vedere [Manage WSUS Replica Servers](https://technet.microsoft.com/library/dd939893(WS.10).aspx) (Gestire i server di replica WSUS) nella Guida operativa di WSUS 3.0 SP2.

I computer sono sempre assegnati al gruppo **Tutti i computer** e rimangono assegnati al gruppo **Computer non assegnati** fino all'assegnazione a un altro gruppo. I computer possono appartenere a più di un gruppo.

È possibile impostare gruppi di computer in gerarchie, ad esempio i gruppi Retribuzioni e Contabilità fornitori possono essere contenuti nel gruppo Contabilità. Gli aggiornamenti approvati per un gruppo superiore vengono automaticamente distribuiti anche ai gruppi inferiori. Se, ad esempio, si approva l'aggiornamento Update1 per il gruppo Contabilità, l'aggiornamento viene distribuito a tutti i computer dei gruppi Contabilità, Retribuzioni e Contabilità fornitori.

Dal momento che i computer possono essere assegnati a più gruppi, è possibile che un singolo aggiornamento venga approvato più volte per lo stesso computer. L'aggiornamento sarà tuttavia distribuito una sola volta e gli eventuali conflitti saranno risolti dal server WSUS. Continuando con l'esempio precedente, se computerA è assegnato ai gruppi Retribuzioni e Contabilità fornitori e Update1 viene approvato per entrambi i gruppi, sarà distribuito una sola volta.

È possibile assegnare i computer a gruppi di computer utilizzando uno dei due metodi denominati rispettivamente destinazione lato server e destinazione lato client. Di seguito sono riportate le definizioni di ciascun metodo.

-   **Selezione della destinazione lato server**: uno o più computer client vengono assegnati manualmente a più gruppi contemporaneamente.

-   **Selezione della destinazione lato client**: si usa Criteri di gruppo o si modificano le impostazioni del Registro di sistema nei computer client per consentire l'aggiunta automatica di tali computer ai gruppi creati in precedenza.

### <a name="conflict-resolution"></a>Risoluzione conflitti
Per risolvere i conflitti e determinare l'azione da eseguire sui client, il server applica le regole seguenti:

1.  Priority

2.  Installazione/Disinstallazione

3.  Scadenza

#### <a name="priority"></a>Priority
Le azioni associate al gruppo con la priorità massima hanno la precedenza sulle azioni degli altri gruppi. Maggiore è la profondità di un gruppo nella gerarchia dei gruppi, maggiore sarà la sua priorità. La priorità viene assegnata solo in base alla profondità. Tutti i rami hanno la stessa priorità. Ad esempio, un gruppo che si trova due livelli sotto il ramo Desktop ha una priorità maggiore rispetto a un gruppo che si trova un livello sotto al ramo Server.

Nell'esempio di testo seguente del riquadro gerarchico della console Update Services, per un server WSUS denominato WSUS-01 al gruppo predefinito **Tutti i computer** sono stati aggiunti i gruppi Computer desktop e Server. I gruppi Computer desktop e Server sono allo stesso livello della gerarchia.

-   **Update Services**

    -   **WSUS-01**

        -   **Aggiornamenti**

        -   **Computer**

            -   **Tutti i computer**

                -   **Computer non assegnati**

                -   **Computer desktop**

                    -   **Desktop-L1**

                        -   **Desktop-L2**

                -   **Server**

                    -   **Server-L1**

        -   **Server downstream**

        -   **Sincronizzazioni**

        -   **Rapporti**

        -   **Opzioni**

In questo esempio, il gruppo situato due livelli sotto il ramo Computer desktop (Desktop L2) ha una priorità superiore a quella del gruppo situato un livello sotto il ramo Server (Server L1). Di conseguenza, per un computer appartenente sia al gruppo Desktop-L2 che al gruppo Server-L1, tutte le azioni per il gruppo Desktop-L2 hanno la priorità sulle azioni specificate per il gruppo Server-L1.

#### <a name="priority-of-install-and-uninstall"></a>Priorità di installazione e disinstallazione
Le azioni di installazione hanno la precedenza su quelle di disinstallazione. Le installazioni obbligatorie hanno la precedenza su quelle facoltative. Le installazioni facoltative sono disponibili solo tramite l'API e, se si utilizza la console di amministrazione di WSUS per modificare un'approvazione per un aggiornamento, tutte le approvazioni facoltative vengono cancellate.

#### <a name="priority-of-deadlines"></a>Priorità delle scadenze
Le azioni con una scadenza hanno la precedenza su quelle prive di scadenza.  Le azioni con scadenza anteriore hanno la precedenza su quelle con scadenza posteriore.

## <a name="16-plan-wsus-performance-considerations"></a>1.6. Pianificare le considerazioni sulle prestazioni WSUS
Prima della distribuzione di Windows Server Update Services, è opportuno pianificare attentamente alcune aree per ottimizzare le prestazioni. Le principali aree sono:

-   Configurazione della rete

-   Download differito

-   Filtri

-   Installazione

-   distribuzione di aggiornamenti di grandi dimensioni

-   Servizio trasferimento intelligente in background (BITS)

### <a name="network-setup"></a>Configurazione della rete
Per ottimizzare le prestazioni nelle reti WSUS, considerare i suggerimenti seguenti.

1.  Configurare le reti WSUS in una topologia hub e spoke invece che gerarchica.

2.  Utilizzare l'ordinamento netmask DNS per i computer client in roaming e configurarli per ottenere gli aggiornamenti dal server WSUS locale.

### <a name="deferred-download"></a>Download differito
È possibile approvare gli aggiornamenti e scaricare i metadati degli stessi prima di scaricare i file. Questo metodo viene definito *download differito*. Se i download sono differiti, gli aggiornamenti vengono scaricati solo dopo essere stati approvati. Si consiglia di differire i download in quanto che l'operazione ottimizza la larghezza di banda della rete e lo spazio su disco.

In una gerarchia di server, Windows Server Update Services imposta automaticamente l'utilizzo del download differito del server WSUS radice su tutti i server downstream. L'impostazione predefinita può essere modificata. È, ad esempio, possibile configurare un server upstream per effettuare sincronizzazioni immediate complete e un server downstream per differire i download.

Se si distribuisce una gerarchia di server WSUS connessi, è preferibile non annidarli in profondità. Se si abilitano i download differiti e un server downstream richiede un aggiornamento che non è stato ancora approvato nel server upstream, la richiesta forza un download nel server upstream. Il server downstream scarica l'aggiornamento nel corso di una sincronizzazione successiva. In una gerarchia di server WSUS profonda, possono verificarsi ritardi quando gli aggiornamenti vengono richiesti, scaricati e passati alla gerarchia. 'Per impostazione predefinita, i download differiti sono abilitati quando i file sono archiviati a livello locale. È possibile modificare manualmente questa opzione.

### <a name="filters"></a>Filtri
Windows Server Update Services consente di filtrare le sincronizzazioni degli aggiornamenti per lingua, prodotto e classificazione. In una gerarchia di server, WSUS imposta automaticamente su tutti i server downstream l'utilizzo delle opzioni di filtro degli aggiornamenti selezionate nel server WSUS radice. È possibile riconfigurare il download nei server in modo da ricevere solo un sottoinsieme di lingue.

Per impostazione predefinita, i prodotti da aggiornare sono Windows e Office e le classificazioni predefinite sono gli aggiornamenti critici, gli aggiornamenti della sicurezza e gli aggiornamenti delle definizioni. Per risparmiare larghezza di banda e spazio su disco, si consiglia di scaricare gli aggiornamenti solo nelle lingue effettivamente utilizzate.

### <a name="installation"></a>Installazione
Gli aggiornamenti sono generalmente costituiti da nuove versioni di file già presenti sul computer in fase di aggiornamento. A livello binario, i file esistenti possono non essere molto diversi dalle versioni aggiornate. La funzionalità di download dei file per l'installazione rapida identifica i byte esatti tra le versioni, crea e distribuisce gli aggiornamenti delle sole differenze e quindi unisce il file esistente ai byte aggiornati.

A volte questa funzionalità è denominata delta di recapito perché scarica solo il delta, ovvero la differenza, tra due versioni di un file. I file per l'installazione rapida sono più grandi degli aggiornamenti distribuiti ai computer client perché contengono tutte le versioni possibili dei file da aggiornare.

È possibile usare i file per l'installazione rapida per limitare la larghezza di banda utilizzata nella rete locale, perché WSUS trasmette solo i dati differenziali applicabili a una versione specifica di un componente aggiornato. Tuttavia, ciò comporta un maggiore utilizzo di larghezza di banda tra il server WSUS, i server WSUS upstream e Microsoft Update e richiede spazio su disco locale aggiuntivo. Per impostazione predefinita, Windows Server Update Services non utilizza i file per l'installazione rapida.

Non tutti gli aggiornamenti sono distribuibili tramite i file per l'installazione rapida. Selezionando questa opzione, si acquisiscono i file per l'installazione rapida di tutti gli aggiornamenti. Se non si archiviano gli aggiornamenti in locale, Agente di Windows Update stabilirà se scaricare i file per l'installazione rapida o le distribuzioni di aggiornamento con file completi.

### <a name="large-update-deployment"></a>Distribuzione di aggiornamenti di grandi dimensioni
Quando si distribuiscono aggiornamenti di grandi dimensioni, ad esempio i Service Pack, è possibile evitare di saturare la rete utilizzando le procedure seguenti.

1.  Utilizzare l'opzione di limitazione in BITS (Servizio trasferimento intelligente in background). Le limitazioni della larghezza di banda in BITS possono essere controllate in base all'orario, ma si applicano a tutte le applicazioni che utilizzano BITS. Per informazioni su come controllare la limitazione in BITS, vedere [Criteri di gruppo](https://msdn.microsoft.com/library/windows/desktop/aa362844(v=vs.85).aspx).

2.  Utilizzare l'opzione di limitazione in IIS (Internet Information Services (IIS) per applicarla a uno o più servizi Web.

3.  Utilizzare gruppi di computer per controllare l'implementazione. Un computer client si identifica come membro di un determinato gruppo di computer quando invia informazioni al server WSUS. Il server WSUS utilizza queste informazioni per determinare quali aggiornamenti distribuire al computer. È possibile configurare più gruppi di computer e approvare in modo sequenziale download di Service Pack di grandi dimensioni per un sottoinsieme dei gruppi configurati.

### <a name="background-intelligent-transfer-service"></a>Servizio trasferimento intelligente in background
Windows Server Update Services utilizza il protocollo BITS (Servizio trasferimento intelligente in background) per tutte le attività di trasferimento file riguardanti download a computer client e sincronizzazioni di server. BITS consente ai programmi di scaricare i file utilizzando la larghezza di banda disponibile. I trasferimenti dei file vengono conservati tramite disconnessioni di rete e riavvii dei computer. Per altre informazioni, vedi: [Servizio trasferimento intelligente in background](https://msdn.microsoft.com/library/bb968799.aspx).

## <a name="17-plan-automatic-updates-settings"></a>1.7. Pianificare le impostazioni per gli aggiornamenti automatici
È possibile specificare una scadenza per l'approvazione degli aggiornamenti nel server WSUS. La scadenza provoca l'installazione dell'aggiornamento nei computer client a un'ora specifica. Possono tuttavia esistere diverse situazioni a seconda che la scadenza sia stata superata, vi siano altri aggiornamenti da installare nella coda del computer oppure l'aggiornamento in questione o un altro nella coda richiedano un riavvio del sistema.

Per impostazione predefinita, Aggiornamenti automatici esegue il polling del server WSUS per la ricerca di aggiornamenti approvati ogni 22 ore meno un offset casuale. Se è necessario installare nuovi aggiornamenti, questi vengono scaricati. L'intervallo tra ogni ciclo di rilevamento può essere modificato da 1 a 22 ore.

È possibile modificare le opzioni di notifica come segue:

1.  se Aggiornamenti automatici è configurato per notificare all'utente la presenza di aggiornamenti da installare, la notifica viene inviata al registro di sistema e all'area di notifica del computer client;

2.  quando un utente con le credenziali corrette fa clic sull'icona dell'area di notifica, in Aggiornamenti automatici vengono visualizzati gli aggiornamenti disponibili per l'installazione. Per avviare l'installazione, è necessario fare clic su **Installa** . Se l'aggiornamento richiede il riavvio del computer al termine dell'installazione, viene visualizzato un messaggio. Se è necessario riavviare il computer, Aggiornamenti automatici non è in grado di rilevare la presenza di altri aggiornamenti fin quando non viene eseguito il riavvio.

Se Aggiornamenti automatici è configurato per l'installazione degli aggiornamenti in base a una pianificazione impostata, gli aggiornamenti applicabili vengono scaricati e contrassegnati come pronti per l'installazione. Aggiornamenti automatici avvisa gli utenti con le credenziali corrette visualizzando un'icona nell'area di notifica e registrando un evento nel registro di sistema.

Nel giorno e all'ora pianificati, Aggiornamenti automatici esegue l'installazione dell'aggiornamento e riavvia, se necessario, il computer, anche nel caso in cui nessun amministratore locale sia connesso. Se un amministratore locale è connesso ed è necessario riavviare il computer, Aggiornamenti automatici visualizza un avviso e un conteggio alla rovescia per il riavvio. Altrimenti, l'installazione avviene in background.

Se è necessario riavviare il computer e un utente è connesso, viene visualizzato un conteggio alla rovescia che avvisa l'utente che sta per essere eseguito un riavvio. I riavvii dei computer sono modificabili in Criteri di gruppo.

Al completamento del download dei nuovi aggiornamenti, Aggiornamenti automatici esegue il polling del server WSUS per cercare l'elenco dei pacchetti approvati e verificare che i pacchetti scaricati sono ancora validi e approvati. Ciò significa che, se un amministratore WSUS rimuove aggiornamenti dall'elenco degli aggiornamenti approvati mentre Aggiornamenti automatici sta eseguendo il download, saranno installati effettivamente solo gli aggiornamenti ancora approvati.

