---
title: Manuale dell'utente di Server Performance Advisor
description: Manuale dell'utente di Server Performance Advisor
ms.assetid: be99a5a9-b9c0-436b-912c-e5c5839a533d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server
ms.technology: manage
ms.openlocfilehash: 3b29a7e10cc6a862873516b9adc16182d64dd926
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465465"
---
# <a name="server-performance-advisor-users-guide"></a>Manuale dell'utente di Server Performance Advisor

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8

Questo manuale dell'utente per Microsoft Server Performance Advisor (SPA) fornisce linee guida sull'utilizzo di SPA per individuare i colli di bottiglia delle prestazioni nei sistemi distribuiti in diversi ruoli del server.

SPA consentono con quanto segue:

* Gestire le prestazioni del server e risolvere i problemi relativi alle prestazioni del server.

* Fornire report sui dati e raccomandazioni sui problemi di configurazione e di prestazioni comuni.

* Fornire consigli sulle procedure consigliate in base ai dati raccolti.

> [!NOTE]
> La Console di SPA non viene apportata alcuna modifica ai server.

Per ulteriori informazioni sullo sviluppo di SPA Pack di Advisor, vedere [Guida di sviluppo Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

## <a name="server-performance-advisor-overview"></a>Panoramica di Server Performance Advisor


In questa sezione illustra la cronologia di SPA, descrive il gruppo di destinatari e gli scenari e viene presentata una panoramica dell'architettura dello strumento.

### <a name="history-of-spa"></a>Cronologia di SPA

La versione originale di Microsoft Server Performance Advisor (SPA) è stata rilasciata nel 2006 2005. Ha principalmente come destinazione Windows Server 2003, inclusi il sistema operativo principale e i ruoli del server, ad esempio Internet Information Services (IIS) e Active Directory. L'obiettivo dello strumento è che consentono di valutare le prestazioni del server e risolvere i problemi di prestazioni di server.

SPA fornisce report sui dati e indicazioni per gli amministratori di sistema sulla configurazione comune e problemi di prestazioni. SPA raccoglie i dati relativi alle prestazioni da diverse origini su server, ad esempio, i contatori delle prestazioni, le chiavi del Registro di sistema, WMI query, i file di configurazione e traccia eventi per Windows (ETW). In base ai dati sulle prestazioni di server da esso raccolti, SPA può fornire una descrizione approfondita la situazione attuale delle prestazioni di server e delle raccomandazioni problema sui quali possono essere migliorate.

Sono state più di un milione di download dello strumento SPA originale e che ha contribuito a molti amministratori di sistema le prestazioni dei server di gestione. È stato uno strumento di risoluzione dei problemi delle prestazioni di successo.

Dopo il rilascio di SPA originale, esistono diverse importanti modifiche nel mondo delle prestazioni server. In particolare, la versione di Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008, con le nuove versioni dei ruoli del server corrispondente, ad esempio Internet Information Services (IIS) 8.5. Le versioni più recenti di Server Performance Advisor estendono le capacità di SPA originale per i sistemi operativi server recenti e i ruoli del server.

Sebbene SPA 3.1 condivide la stessa obiettivi di progettazione e un paradigma di analisi delle prestazioni tramite lo strumento originale, è stato riscritto con la tecnologia più recente. Offre i miglioramenti principali seguenti:

* Eseguire l'analisi di server che eseguono Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008.

* Supporta funzionalità di analisi remota, che consente di SPA 3.1 raccogliere e analizzare i dati da una console centrale, senza la necessità di installare codice sui server da analizzare.

* Utilizza Microsoft SQL Server per l'analisi di dati e archiviazione, che consente l'elaborazione e archiviazione di grandi quantità di dati.

* Separa lo strumento dai pacchetti di Advisor. Logica di analisi delle prestazioni per ogni ruolo del server viene rilasciata sotto forma di un pacchetto di advisor, che può essere rilasciato o aggiornato separatamente dallo strumento.

* Offre pacchetti di Advisor sviluppate negli script SQL con un'architettura aperta. Parti non Microsoft possono sviluppare Pack di advisor o estendere il Pack advisor esistenti da Microsoft per soddisfare particolari esigenze.

* Supporta le nuove funzionalità quali i report di confronto side-by-side e tendenze e cronologici grafici che consentono di individuare eventuali anomalie.

* Fornisce funzionalità come la modifica, importazione ed esportazione soglie che consentono di fine ottimizzare i report e notifiche e condividono tali se con altri utenti SPA.

* Supporta più progetti, possono essere utilizzati per raggruppare i server di destinazione.

* Fornisce ricorrente raccolta dati all'interno della console SPA.

* Consente la generazione del report e query personalizzate utilizzando Microsoft SQL Server (per utenti esperti).

* Cmdlet di Windows PowerShell personalizzati sono disponibili per l'utilizzo con SPA

* Compatibile con le versioni precedenti per l'importazione e la visualizzazione dei report da un database di SPA 3.0

### <a name="target-audience"></a>Utenti interessati

Lo strumento SPA è stato progettato principalmente per gli amministratori di sistema che gestiscono meno di 100 server in vari ruoli server. Può inoltre essere utilizzato dagli addetti al supporto per raccogliere dati sulle prestazioni e risoluzione dei problemi di prestazioni per i clienti.

SPA offre indicazioni che consentono di risolvere i problemi di prestazioni. Tuttavia, è basato su presupposti che potrebbero non essere applicabili all'ambiente server specifico. I consigli offerti tramite SPA è necessario considerare i suggerimenti. È probabile che gli utenti SPA avere una buona conoscenza della loro configurazione di sistema, casi di utilizzo e l'impatto di ottimizzazione di sistema per il comportamento del sistema complessivo. Gli amministratori di sistema devono decidere se le indicazioni di SPA devono essere applicate all'ambiente in uso.

### <a href="" id="what-s-new-in-spa-3-1-"></a>Cosa è nuovo in SPA 3,1?

In SPA 3.1 sono state aggiunte le funzionalità e i miglioramenti seguenti:

* Active Directory Advisor Pack consente di analizzare le prestazioni generali del ruolo server Servizi di dominio Active Directory del server.

* Supporto per sviluppatori di aggiungere perso avviso di notifiche di eventi ETW nei Pack di advisor e rendere visibile l'avviso quando è stato generato un report e gli eventi sono stati persi a causa di alta frequenza registrazione lenta o frequentemente conflitti disco

### <a name="target-scenarios"></a>Scenari di destinazione

Di seguito sono gli scenari di destinazione per l'autenticazione SPA:

* **Ambiente server**

    SPA è progettato per configurare e gestire facilmente. Si applica agli ambienti server che utilizzano server 1 e 100. SPA non è scalabile correttamente se si sta provando a gestire le prestazioni per più di 100 server. Per ambienti più grandi o più sofisticati, è opportuno utilizzare System Center Operations Manager.

* **Risoluzione dei problemi relativi alle prestazioni**

    È possibile utilizzare SPA come strumento di risoluzione dei problemi delle prestazioni. Fornisce la possibilità di raccogliere dati sulle prestazioni di alto livello, esegue un post completo l'elaborazione dei dati per fornire una migliore comprensione del loro funzionamento complessivo del sistema e il flag eventuali anomalie. Quando si sospetta un problema di prestazioni da un cliente, è possibile utilizzare SPA per raccogliere e analizzare i dati sulle prestazioni dal server.

    Il SISTEMA genera un report che è possibile visualizzare. SPA forniscono un elenco di notifica che evidenzia i potenziali problemi e una sezione di dati che include vari indice delle prestazioni, le configurazioni e impostazioni per il server. È possibile utilizzare questo report per identificare il problema di prestazioni specifici e utilizzare le raccomandazioni per trovare soluzioni per il problema. I report possono essere confrontati con gli altri report generati in un momento diverso o da un server diverso. Usando questo confronto affiancato, è possibile determinare le differenze tra il comportamento normale e quello anomalo della linea di base.

    **Nota** SPA non è progettato per essere uno strumento di debug o di misurazione. Inoltre, dal punto di vista dei server, può essere considerato uno strumento di sola lettura e non modifica le configurazioni dei server.

     

* **Monitoraggio degli indici delle prestazioni**

    È possibile utilizzare il SISTEMA per monitorare l'indice delle prestazioni dei server. È possibile scegliere di eseguire regolarmente SPA su server per raccogliere dati sulle prestazioni e quindi eseguire un grafico di tendenza o un grafico di andamento storico per individuare le eventuali anomalie. È possibile visualizzare il report per una determinata analisi, reperire ulteriori informazioni sul problema di prestazioni e quindi utilizzare le indicazioni o altri dati del report per risolvere il problema.

### <a name="spa-architecture"></a>Architettura SPA

La logica di raccolta dati SPA si basa su un protocollo in Windows, denominato AVVISI e registri di prestazioni. Erano messe a Disposizione consente di raccogliere dati sulle prestazioni dai server locali o remoti, ad esempio i contatori delle prestazioni, WMI query, ETW tracce, le chiavi del Registro di sistema e i file di configurazione. Quando SPA viene eseguita l'analisi delle prestazioni in un server di destinazione, viene creato un set di agenti di raccolta dati erano messe a Disposizione basato su Service Pack Advisor SPA specifico selezionato. Il pacchetto di advisor contiene l'origine dati da raccogliere e la condivisione di file in cui sono memorizzati i registri. Raccolta dei dati SPA memorizza solo un singolo account utente; lo stesso account utente utilizzato da PIA viene anche utilizzato per scrivere i log.

Il SISTEMA utilizza un database di SQL Server per archiviare i log delle prestazioni raccolti dai server di destinazione. SPA Importa tutti i registri di prestazioni dalla condivisione file al database e quindi viene utilizzata la logica di analisi dei dati all'interno di ogni pack advisor per elaborare i dati e generare i report. Un pacchetto di advisor analizza i dati sulle prestazioni raccolti dai server di destinazione e genera i report SPA. Per ulteriori informazioni sulla creazione di un pacchetto di advisor, vedere [Guida di sviluppo Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

Le interfacce utente console SPA e interazioni vengono compilate come parte di SPAConsole.exe. È possibile utilizzare la console per creare un database, aggiungere o rimuovere Pack di advisor, gestire server di destinazione, l'analisi delle prestazioni e visualizzare i report di prestazioni.

La console SPA può essere eseguita nei sistemi operativi seguenti:

* Windows 8.1

* Windows 8

* Windows 7

* Windows Server 2012 R2

* Windows Server 2012

* Windows Server 2008 R2

* Windows Server 2008

In una tipica applicazione aziendale, esistono tre livelli: il livello di presentazione, il livello di logica di business e il livello di archiviazione. SPA è progettato come prodotto a due livelli la console e il database. La console viene utilizzato come un livello di presentazione con determinata logica correlati al processo inclusi e viene utilizzato il database come il livello di archiviazione e il livello di logica di business. La console acquisisce l'input dell'utente e controlla i passaggi per la raccolta dei dati, l'elaborazione dati e generazione di report. SPA non dipende da servizi di sistema di Windows.

Nel diagramma seguente viene illustrata l'architettura di alto livello del sistema SPA. Il processo è:

1.  Dalla console di SPA, eseguire analisi delle prestazioni nel server specificato.

2.  Quando vengono raccolti i dati sulle prestazioni, erano messe a Disposizione nei server di destinazione i log vengono scritte nella condivisione file specificata dal set di agenti di raccolta dati.

3.  Al termine la raccolta dei dati in un computer di destinazione, la console SPA Importa i registri per il database di SQL Server.

4.  La console richiama la logica di elaborazione dati di Service pack di advisor specifico.

5.  Il pacchetto di advisor elabora i dati e chiama le API di SPA per inserire i record nelle tabelle di sistema di report che sono definite dal framework di applicazione a singola PAGINA.

6.  È possibile utilizzare il Visualizzatore di report nella console per visualizzare i report che ha generato. Per risolvere i problemi di prestazioni, SPA fornisce tre tipi di report: singolo report, report, side-by-side e tendenza o grafici cronologici. Per ulteriori informazioni sui report, vedere [la visualizzazione dei report](#bkmk-viewingreports).

![architettura di SPA](../media/server-performance-advisor/spa-user-manual-architecture.png)

## <a name="getting-started-with-spa"></a>Introduzione a SPA


### <a name="requirements"></a>Requisiti

La console SPA può essere installata in Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008. Esecuzione SPA nelle versioni precedenti del sistema operativo Windows Server non è supportata. SPA viene eseguito su x86 o x64, ma non supporta le architetture ARM o IA64.

SPA è basato su Windows Presentation Foundation (WPF) 2.0, che fa parte di Microsoft .NET Framework 4, pertanto è richiesto .NET Framework 4.

È inoltre necessario installare SQL Server 2008 R2 Express nello stesso computer in cui è installato il SISTEMA. SQL Server 2008 R2 Express è un motore di database gratuito che viene rilasciato da Microsoft. È possibile scaricare Microsoft SQL Server 2008 R2 Express dal Microsoft Download Center. Mentre si consiglia di SQL Server 2008 R2 Express, le versioni più recenti di SQL Server possono inoltre essere compatibile con SPA.

**Nota** SPA non include SQL Server o l'.NET Framework come parte del pacchetto di installazione di SPA. Dopo l'installazione di Microsoft SQL Server 2008 R2 e .NET Framework 4.0, è consigliabile eseguire Windows Update prima di installare l'applicazione a singola PAGINA.

 

Poiché gli utenti possono creare e gestire i database con SPA, l'account utente utilizzato per eseguire l'applicazione a singola PAGINA deve avere gli stessi privilegi di amministratore di SQL Server.

### <a href="" id="bkmk-setupspa"></a>Configurazione di SPA

SPA è incluso in un pacchetto con estensione cab che include tutti i file binari per il Framework SPA, i cmdlet di Windows PowerShell usati in scenari avanzati e i pacchetti di Advisor seguenti: sistema operativo principale, Hyper-V, Active Directory e IIS. Dopo aver estratto il file CAB in una cartella, è necessaria alcuna installazione aggiuntiva. Tuttavia, per eseguire l'applicazione a singola PAGINA, è necessario abilitare la raccolta dati dal server di destinazione come segue:

* Per eseguire la raccolta dei dati erano messe a Disposizione, l'account utente utilizzato per eseguire la console SPA deve essere parte del gruppo di sicurezza Administrators sul server di destinazione. Se il server di destinazione e la console nello stesso dominio, account utente di dominio deve essere parte del gruppo di sicurezza Administrators sul server di destinazione. Se il server di destinazione e la console non sono nello stesso dominio, creare un account utente con privilegi amministrativi nel server di destinazione con gli stessi nome utente e password dell'account utente utilizzato per eseguire la console SPA.

* Creare una cartella condivisa per i risultati nel server.

* Assicurarsi che l'account utente utilizzato per eseguire la console SPA dispone di autorizzazioni lettura e scrittura alla cartella condivisa. PLA usa questo account per scrivere i log nella cartella.
La console SPA usa lo stesso account per leggere i log e importarli nel database.

    **Nota** ETW implementa un buffer circolare per archiviare la traccia e spostarli nella cartella condivisa, quando possibile. Se il server è occupato o l'operazione di scrittura è lenta, ETW Elimina le tracce quando il buffer è pieno. È importante che la cartella condivisa si trovi in un server con accesso I/O veloce. È consigliabile che ogni server di destinazione disponga di una cartella condivisa per ridurre al minimo la perdita di dati dovuta ai / o file lenta.

     

* Per i PIA accedere ai server di destinazione, impostare Windows Firewall per consentire l'accesso gli avvisi e registri di prestazioni remoto nei server di destinazione. Erano messe a Disposizione utilizza la porta TCP 139.

* Assicurarsi che il **gli avvisi e registri di prestazioni** servizio è in esecuzione.

* Se la console e il server di destinazione si trovano in subnet diverse, è necessario impostare anche il campo indirizzo IP remoto nelle regole del firewall in ingresso nelle impostazioni dell' **ambito** nella pagina **avvisi e registri di prestazioni** , come illustrato di seguito.

    ![proprietà erano messe a disposizione](../media/server-performance-advisor/spa-user-manual-pla-firewall.png)

* Attivare l'individuazione di rete nella console e in ognuno dei server di destinazione.

* Se il server di destinazione non è aggiunto a un dominio, abilitare la seguente impostazione del registro di **sistema: HKLM\\SOFTWARE\\Microsoft\\Windows\\Currentversion\\Policies\\system\\LocalAccountTokenFilterPolicy**.

**Nota** Per impostazione predefinita, SPA scrive i log di diagnostica nella cartella in cui si trova SpaConsole. exe. Se SPA è installato nella cartella programmi, SPA è in grado di scrivere il log solo quando SpaConsole. exe viene eseguito come amministratore.

Se si vuole eseguire l'analisi dei dati sul computer della console, è necessario eseguire SPA come amministratore. PIA passa attraverso un percorso di codice diverso durante l'esecuzione per un computer locale, che richiede privilegi di amministratore.

Se si desidera eseguire il pacchetto di Advisor di IIS SPA sui server, è necessario abilitare la query WMI e la traccia ETW per il server IIS. Tale scopo, è possibile abilitare **traccia** sotto il **integrità e diagnostica** servizio ruolo e **Strumenti e script di gestione IIS** in **gli strumenti di gestione** del **Web Server (IIS)** ruolo del server.

 

### <a name="creating-your-first-project"></a>Creazione del primo progetto

Dopo tutto è configurato, è possibile creare il primo progetto di applicazione a singola PAGINA. Come descritto nella sezione precedente, il SISTEMA memorizza tutto ciò che è correlato all'analisi dei dati delle prestazioni all'interno di un database. Ogni progetto di applicazione a singola PAGINA corrisponde a un singolo database. La **prima volta che si usa la procedura guidata** è possibile eseguire i passaggi seguenti.

**Per creare un progetto SPA**

1.  Avviare SpaConsole.exe. La console di una modalità disconnessa, in cui SPA non è connesso a qualsiasi database e la finestra principale è vuota.

2.  Per creare un nuovo progetto, fare clic su **File**, quindi fare clic su **Nuovo progetto**. Viene avviata la procedura guidata per la prima volta. Nella prima pagina vengono illustrati i passaggi da eseguire durante l'utilizzo della procedura guidata:

    * Creare un database

    * Effettuare il provisioning Pack di advisor

    * Aggiungere server all'elenco di server di destinazione

3.  Fare clic su **Avanti**. Nella pagina **Crea database progetto** viene chiesto di specificare il nome dell'istanza di Microsoft SQL Server in cui si desidera creare il database. Se, ad esempio, si trova nello stesso computer della console di, è possibile utilizzare **localhost\\&lt;il nome del server SQL&gt;** .

    **Nota** Il nome dell'istanza predefinita per un'installazione di SQL Server 2008 R2 Express è SQLEXPRESS. Predefinito in genere per un'istanza di SQL Server 2008 R2 Express è installato nel computer locale, il database verrà impostato su **localhost\\SQLExpress**. Tuttavia, si sia stato modificato durante l'installazione di SQL Server, pertanto è necessario utilizzare il nome dell'istanza SQL Server corretto.

     

4.  Fornire il nome del database. Solo lettere, cifre e caratteri di sottolineatura (\_) sono consentiti come caratteri validi per un nome di database. Suggerimenti per il nome del database SPA ragionevoli sarebbe **SPA**. Se si immette un nome non valido, viene visualizzata un'icona rossa di errore. La descrizione comando associata indica il motivo dell'errore di convalida.

    **Nota** È importante ricordare il nome del database e il nome dell'istanza del server, perché si tratta degli unici identificatori per il progetto. È necessario fornire queste informazioni se si desidera passare a questo database.

     

5.  Dopo aver fornito il nome dell'istanza del server e il nome del database, la prima volta che si utilizza la procedura guidata viene generato il percorso per il file di database.

6.  Nella pagina **Crea database di progetto** fare clic su **Avanti**. La prima volta che si usa la procedura guidata crea un database e genera tutte le stored procedure, le funzioni e lo schema del database correlati a SPA nel database. Questo passaggio potrebbe richiedere alcuni secondi a seconda della velocità di hardware e di rete.

    **Nota** se questo passaggio ha esito negativo, viene visualizzato un messaggio di errore. Alcuni dei problemi più comuni sono: Console non può connettersi all'istanza di SQL Server, dispone di privilegi sufficienti per creare database, o il nome del database esiste già.

     

7.  Quando il passaggio precedente ha esito positivo, viene visualizzata la pagina **provision Advisor Pack** . Elenca tutti i Pack advisor sono disponibili nel computer in uso. SPA analizza automaticamente la cartella denominata **APs** nella directory radice SPA. Elenca il nome completo, versione e l'autore per ogni Service pack di advisor.

    **Nota** per altre informazioni sul modo in cui il nome completo e la versione vengono usati in Spa, vedere [gestione dei pacchetti di Advisor](#bkmk-manageadvisorpacks)

     

8.  Scegliere quali Pack advisor che si desidera eseguire il provisioning nel database del progetto e quindi fare clic su **Avanti**. È possibile fare clic su **Skip** per passare al passaggio successivo senza provisioning qualsiasi Pack di advisor.

    **Nota** È possibile eseguire il provisioning di Pack di Advisor ogni volta che si utilizza lo strumento. Per ulteriori informazioni, vedere [Gestione Pack di advisor](#bkmk-manageadvisorpacks).

     

9.  Nella pagina **Aggiungi server** , per ogni server da aggiungere all'elenco dei server di destinazione, è necessario compilare due campi obbligatori: **il nome del server** e il **percorso della condivisione file**.

    **Nota** Esiste anche un campo **remark** , che viene usato principalmente per classificare o trovare il server. In casi in cui si dispone di molti server, è possibile importare un file di elenco delimitato da virgole (CSV) che contiene il nome del server, la cartella dei risultati e il campo è facoltativo. Il campo del **contrassegno** viene usato per descrivere il server e il termine può essere usato per filtrare i server per la raccolta dei dati. Se si inizializzano i server tramite il file con estensione CSV, un errore di analisi all'interno del file non caricherà i server.

     

10. È necessario impostare per abilitare la raccolta di dati erano messe a Disposizione, come descritto in diverse configurazioni [impostazione SPA](#bkmk-setupspa). La pagina **Aggiungi server** fornisce una funzionalità di configurazione di test che consente di risolvere i problemi di configurazione. Selezionare la casella di controllo associata al computer, quindi fare clic su **Verifica connettività**. SPA tenta di generare un agente di raccolta dati sui server di destinazione e si tenta di importare di che nuovo i risultati nel database. Se tutto è corretto, il **stato** Mostra **passare**. In caso contrario, viene visualizzato un suggerimento che descrive il motivo dell'errore.

11. Ogni server viene aggiunto automaticamente al database anche se non supera il test di configurazione. Per rimuovere i server dall'elenco, selezionare il nome del server e fare clic su **Rimuovi**.

12. Al termine, fare clic su **fine** per chiudere la procedura guidata per la prima volta. Se si chiude la procedura guidata per la prima volta prima di terminarla, tutti i passaggi precedenti vengono mantenuti e nessuno esegue il rollback automatico. È necessario apportare manualmente modifiche future.

## <a name="running-analysis"></a>Esecuzione dell'analisi


Dopo aver configurato il database, è possibile eseguire l'analisi delle prestazioni sui server.

Ogni volta che viene avviata la console SPA, l'ultimo progetto che è stato utilizzato dall'utente corrente viene aperto automaticamente. La finestra principale contiene un elenco di server. Ogni server dispone di quattro proprietà: il nome del server, il risultato dell'analisi, lo stato corrente e il contrassegno.

* **Nome del server** il nome del server, che è l'identificatore per il server. Non esistono nomi duplicati sono consentiti.

* **Risultato dell'analisi** per impostazione predefinita, viene illustrato il risultato dell'analisi delle prestazioni più recente esecuzione sul server. Se non è stata qualsiasi analisi delle prestazioni eseguire nel server, verrà visualizzato **alcun report**. Se viene generato un avviso generato dal report, verrà visualizzato **avviso** e il timestamp quando è stato generato il report più recente. Se durante l'analisi più recente sul server è stato trovato alcun problema, verrà visualizzato **OK** e il timestamp.

    **Nota** se di recente è stata modificata un'impostazione di sistema, è consigliabile eseguire di nuovo l'analisi per valutare l'effetto complessivo della modifica e ottenere un report aggiornato dello stato del sistema. SPA non tiene traccia delle modifiche di configurazione per il sistema sottoposto a test.

     

* **Stato corrente** Mostra lo stato delle prestazioni di attività di analisi attualmente in esecuzione sul server. È possibile annullare un'attività in esecuzione facendo la **Annulla** icona, viene indicato da una X rossa.

* **Contrassegna** Descrive il server di destinazione corrente. Ad esempio, è possibile descrivere il server usando il ruolo del server (ad esempio, SQL Server) o un percorso (ad esempio, Kent). SPA usa il **nome del server** e il **contrassegno** per cercare e trovare il server appropriato. È possibile digitare nella casella di testo di ricerca. Se il **nome del server** o il **contrassegno** delle colonne contiene la stringa esatta immessa nella casella di ricerca, il server verrà visualizzato nell'elenco Server.

I seguenti controlli sono anche disponibili nella console:

* **Ripetere** una casella di controllo che descrive la possibilità di ripetere regolarmente una raccolta, basato su un intervallo di tempo. Per la maggior parte delle installazioni di server, è preferibile disporre di una raccolta di SPA ripetere ogni ora per cronologia sufficiente per l'analisi. Se si desidera eseguire una sola volta la raccolta, non è necessario selezionare il **ripetere** casella di controllo.

* **Rimuovi ricorrenza** Pulsante che consente di annullare un processo di ripetizione della raccolta in corso. Cancella la raccolta di ripetizione, ma non la raccolta corrente (se presente) è in corso. Questa opzione consente di ripristinare un nuovo intervallo di ripetizione insieme o eseguire manualmente la raccolta.

Prima di iniziare l'analisi delle prestazioni, selezionare la durata di raccolta dati. Anche se la raccolta di ulteriori dati consente di fornire un'immagine più accurata della situazione delle prestazioni server, viene inoltre generato un numero maggiore di log e può avere un impatto potenziale maggiore sul server. Scegliere la durata di raccolta dati appropriati in base alle proprie esigenze specifiche. Ogni pack advisor consente di definire una durata minima valida. La durata di raccolta dati che si sceglie deve superare la durata minima dei pacchetti di advisor selezionato.

Per eseguire l'analisi delle prestazioni nei server di destinazione, selezionare i server su cui si desidera eseguire l'analisi delle prestazioni, quindi fare clic su **Esegui analisi** . verrà visualizzata una finestra di dialogo in cui viene chiesto di selezionare i pacchetti di Advisor da eseguire sui server. I pacchetti selezionati advisor eseguire simultaneamente. I report generati sono basati sui dati sulle prestazioni raccolti nello stesso periodo di tempo.

**Nota** se si seleziona un server in cui è in esecuzione un'analisi periodica delle prestazioni, il pulsante **Rimuovi ricorrenza** consente di annullare la raccolta dei dati ricorrente. SPA non consente più sessioni di raccolta dati contemporaneamente nello stesso computer.

 

## <a href="" id="bkmk-viewingreports"></a>Visualizzazione di report


In SPA, esistono tre tipi di report di analisi delle prestazioni: singolo report, report, side-by-side e tendenza e grafici cronologici.

Dopo l'esecuzione dell'analisi delle prestazioni, viene generato un report per ogni Pack advisor eseguire sul computer di destinazione. Dall'elenco di server nella finestra principale, è possibile espandere **risultato dell'analisi** per visualizzare tutti i pacchetti di advisor che sono stati eseguiti nel server specifico. È possibile scegliere un nome di report per visualizzare un singolo report.

Sono disponibili tre icone accanto al nome di Service pack di advisor che mostrano lo stato dell'esecuzione nel server di analisi più recenti:

* Il **più recente** icona Mostra il report generato dall'analisi delle prestazioni più recente su questo server per il pacchetto di advisor.

* Nell'icona **trova** viene visualizzato l'elenco dei report di analisi delle prestazioni, che consente di selezionare il report corretto. Il **pack Advisor** e **server di destinazione** campi sono precompilati con la preparazione pack e destinazione server le informazioni correnti. L'intervallo di tempo predefinito è impostato su una settimana e la data di fine viene impostata su oggi. Se si sceglie il **ricerca** pulsante nell'angolo superiore destro, è possibile ottenere un elenco di tutti i report di analisi delle prestazioni per il pacchetto di server e advisor selezionato all'interno dell'intervallo di tempo.

* Il **visualizzazione grafici** icona consente di visualizzare la tendenza e la vista grafico di andamento storico.

Nella figura seguente sono illustrate le icone **più recenti**, **trova**e **Visualizza grafici** dopo ogni pacchetto di Advisor:

![icone di report](../media/server-performance-advisor/spa-user-manual-report-icons.png)

### <a name="searching-for-and-within-reports"></a>Ricerca di e all'interno dei report

Ricerca di report viene eseguita tramite **Esplora Report**. Ciò consente di cercare report pack di advisor, nome del server e intervallo di date. Questo è il modo consigliato per trovare un report di interesse diverso l'ultima esecuzione del report. L'ultima esecuzione del report è disponibile tramite **Visualizza Report** per tale server.

Quando si visualizza un report specifico, è facilmente possibile passare al report successivo e precedente in base all'ora o esaminare un report correlato, ad esempio un punto di accesso diverso in esecuzione nello stesso momento. Queste opzioni sono disponibili in **azioni**.

È inoltre possibile cercare all'interno di un report. Per una serie di report è disponibile una casella di ricerca **trova** stringa per la ricerca di stringhe di testo rapido all'interno del report. Deselezionare la casella di testo, è possibile chiuderla. Per attivare una casella di ricerca (in windows con il testo di ricerca), è possibile utilizzare la combinazione CTRL + F scelta rapida. La casella **trova** consente all'utente di specificare una ricerca con distinzione tra maiuscole e minuscole appropriata con l'opzione **maiuscole/minuscole** .

Nella figura seguente viene illustrata la casella **trova** ricerca con la stringa **Power** nella scheda **report** .

![la casella di ricerca di ricerca](../media/server-performance-advisor/spa-user-manual-find-search-box.png)

### <a name="single-report"></a>Singolo report

Un singolo report mostra i risultati dell'analisi delle prestazioni da una singola esecuzione di un pacchetto di preparazione di un singolo computer. Il report visualizza il nome del pacchetto di preparazione, il nome del server di destinazione, l'ora di generazione di report e la durata per la raccolta dati.

Un singolo report contiene una sezione di notifica e le sezioni di dati.

### <a name="notification-section"></a>Sezione di notifica

La sezione notifica è costituito da un set di regole di analisi delle prestazioni. Ogni notifica contiene alcune origini dati, alcuni valori di soglia e la logica di business. Quando si esegue l'analisi delle prestazioni, la logica di business valuta le origini dati con le soglie per determinare se la regola positivo o negativo. In caso contrario, viene visualizzato un avviso per informare l'utente su un potenziale problema di prestazioni. Fornisce inoltre indicazioni che consentono di risolvere il problema. La sezione notifica è sempre la prima tabulazione nella vista del singolo report.

La sezione notifica è suddivisa in due parti: **avviso** e **altre notifiche**.

Se l'origine dati per una regola soddisfa determinate condizioni in base alle impostazioni della logica e della soglia, viene visualizzato un avviso nell'area di **avviso** . Un avviso include le parti seguenti:

* Un'icona di avviso indica l'esistenza di un potenziale problema.

* Nome della regola. Ad esempio, **rete ricezione Elimina pacchetto** è un collegamento che punta alla pagina dei dettagli regola, come descritto in [Gestione Pack di advisor](#bkmk-manageadvisorpacks).

* Una breve descrizione sul problema potenziale.

* Suggerimento per una possibile soluzione al problema di prestazioni potenziali.

Server diversi possono avere modelli di utilizzo e configurazione notevolmente differente, e non è possibile impostare le soglie e le regole sono applicabili a tutti i server in tutte le condizioni. SPA offre la possibilità di modificare le soglie. È possibile anche scegliere di disattivare una regola se la regola non applicabile al proprio scenario. Per impostazione predefinita, tutte le regole sono attivate. Una regola disabilitata non viene visualizzata nell'area di notifica. Per ulteriori informazioni, vedere [Gestione Pack di advisor](#bkmk-manageadvisorpacks).

Il **altre notifiche** area contiene tutte le altre regole, in cui non viene generato alcun avviso o la regola non è applicabile. Contiene le parti simile tra quelli presenti nella **avviso** area. La differenza principale è che se non viene generato alcun avviso o non è applicabile la regola, in genere alcuna indicazione non viene fornita.

### <a name="data-sections"></a>Sezioni di dati

Sezioni di dati contengono i dati sulle prestazioni che genera il pacchetto di advisor in base ai dati non elaborati raccolti dai server di destinazione. Sezioni di dati includono una serie di sezioni di primo livello e più livelli di sottosezioni. Le sezioni di primo livello vengono presentate sotto forma di schede. Tutte le sottosezioni sotto le sezioni di primo livello vengono presentate nelle aree espandibile. È possibile comprimere o espandere sezioni di area di interesse, come illustrato nella figura seguente.

![sezioni di dati](../media/server-performance-advisor/spa-user-manual-data-sections.png)

La preparazione Pack di Core del sistema operativo SPA e IIS SPA Advisor contengono un **System overview** sezione. In questa sezione include le informazioni sulla configurazione e utilizzo delle risorse di livello superiore. Altre sezioni di primo livello rappresentano le aree dei dati sulle prestazioni. SPA presenta dati del report nei modi seguenti:

* **Singolo valore** una coppia chiave/valore. La chiave è una stringa che rappresenta il significato del valore. Il valore può essere una stringa, un valore numerico o un valore booleano. Viene spesso usato per visualizzare informazioni statiche, ad esempio la configurazione, l'architettura della CPU, le dimensioni totali della memoria e la versione del BIOS, che non cambiano nel tempo.

* **valore elenco** Questa è talvolta una coppia chiave/valore, ma il valore dell'elenco può contenere più campi. Ad esempio, l'attributo della CPU può essere visualizzata in una tabella con più colonne e più righe. Ogni riga rappresenta una CPU e ogni colonna rappresenta un attributo della CPU.

* **Statistiche** può essere considerato un tipo speciale di singolo valore. Può contenere solo dati numerici. Durante il periodo di raccolta dati, molti dei punti dati numerici fluttuazioni anziché rimanere costante. Ad esempio, l'utilizzo della CPU cambia ogni volta che i PIA raccoglie il contatore delle prestazioni. Con un solo valore non può riflettere accuratamente la situazione di prestazioni. Anziché la visualizzazione di un solo valore, il valore medio, massimo, minimo e il 90% vengono utilizzati per tali punti di dati numerici dinamica. Il valore del 90% rappresenta attività uguale o superiore di 90 ° percentile tra tutti gli eventi per quel contatore in tale intervallo di raccolta specificato.

* **Elenco superiore** contiene in genere i principali consumatori di una risorsa specifica o le prime entità che ha riscontrato determinati eventi. Ad esempio, **Elabora i primi 10 in termini di utilizzo medio della CPU** include i processi di dieci con più alto utilizzo medio della CPU durante il periodo di raccolta dati. Poiché l'utilizzo della CPU è anche un punto dati numerici dinamica, altre statistiche quali massimo, minimo e valore 90% sono inoltre inclusi nell'elenco per consentire all'utente un quadro più completo di utilizzo della CPU di.

Come indicato nelle sezioni precedenti, il SISTEMA si basa su erano messe a Disposizione per raccogliere una traccia ETW, query WMI, i contatori delle prestazioni, le chiavi del Registro di sistema e i file di configurazione per generare il report. È importante comprendere l'origine dati dietro ogni punto dati del report. SPA fornisce tali informazioni tramite le descrizioni comandi. È possibile posizionare sulle righe per visualizzare una descrizione dell'origine dati o colonne chiave. Ad esempio, **WMI:Win32\_DisDrive: didascalia** significa che l'origine dati deriva da una query WMI, il nome della classe WMI è Win32\_DiskDrive e la proprietà è **didascalia**.

### <a href="" id="side-by-side-report-"></a>Report affiancato

Solo i report forniscono le notifiche e una sezione di dati per l'utente individuare potenziali problemi di prestazioni, ma spesso è difficile identificare potenziali problemi di prestazioni direttamente esaminando un singolo report. Un singolo report potrebbe contenere Troppi punti dati, che rende difficile individuare i potenziali problemi.

Per risolvere questo problema, SPA consente di confrontare due report. È possibile confrontare un report con un potenziale problema a un report di base per consentire di individuare le differenze.

Report side-by-side può essere avvio da un visualizzatore di report singola. Gli utenti possono fare clic su **azioni**e quindi fare clic su **Confronta report** per selezionare i report. È rilevante solo per confrontare i report dello stesso pacchetto advisor. È possibile confrontare il report con un rapporto precedente nel tempo, il successivo report nel tempo o un report di arbitrario selezionato attraverso la funzionalità di ricerca. Ad esempio, per isolare un comportamento anomalo, è possibile confrontare un report del server di base per un report che è stato generato in un momento diverso nello stesso computer o per un report che è stato generato in un computer diverso che include un ruolo di server simili e di carico.

Un report side-by-side è molto simile singolo report. Contiene una sezione di notifica e sezioni di dati. Contiene lo stesso numero di notifiche e sezioni di dati come visualizzatore di report singola. L'unica differenza è che i report vengono visualizzati in modalità side-by-side. Ogni sezione contiene i dati di report di origine (rapporto 1) e il report di destinazione (report 2). Il report side-by-side consente di visualizzare il nome del pacchetto di preparazione, il nome del server di destinazione (rapporto 1 a sinistra) e report 2 a destra, l'ora in cui è stato generato il report e la durata della raccolta di dati per ogni report.

Se si ignora la finestra di dialogo **trova** , è possibile riattivarla digitando CTRL + F. Questa finestra di dialogo consente di trovare e evidenziare le stringhe di testo all'interno della sezione corrente.

Nella sezione notifica, se uno dei risultati dei due report confrontati è un avviso, viene elencato nell'area di **avviso** . In caso contrario, i risultati vengono elencati nell'area **altre notifiche** . Poiché la chiave per un report side-by-side consiste nell'identificare le differenze tra i report, non viene visualizzate alcun informazioni dettagliate su una regola. Gli utenti potranno selezionare il nome della regola per visualizzare il modulo dettagli regola per ulteriori informazioni sulla regola.

Nelle sezioni di dati, i dati sono presentati in modalità side-by-side con dati da 1 a sinistra di report e i dati da report 2 a destra. SPA Mostra valori singoli nella stessa tabella, ma anziché etichette delle colonne **valore**, sono denominati **Report 1** e **Report 2** rispettivamente. Il report side-by-side Mostra tutte le altre forme di dati nelle tabelle side-by-side.

Il Visualizzatore di report side-by-side offre anche le descrizioni comandi sull'origine dati.

### <a name="historical-and-trend-charts"></a>Grafici di tendenza e cronologico

È consigliabile solo per mostrare tendenze e i grafici cronologia per un server specifico e Service pack di advisor specifico. È necessario scegliere l'intervallo di tempo (che è impostata sul valore predefinito della settimana) e quindi fare clic su **OK** per visualizzare la tendenza e Visualizzatore grafico di andamento storico.

La tendenza e un grafico di andamento storico Visualizzatore contiene tre schede: grafico di andamento storico, grafico di tendenza di 24 ore e grafico di tendenza di 7 giorni.

### <a name="historical-chart"></a>Grafico di andamento storico

Il grafico di andamento storico Mostra una serie di valori per un punto dati numerici tramite l'intervallo di tempo specificato. Ad esempio, il **latenza media richieste** per IIS in un singolo server negli ultimi 15 giorni. Ciascun punto dati in un grafico di andamento storico rappresenta il valore di un'origine dati specifica eseguita in una sessione di analisi delle prestazioni.

Esistono un paio di modi per utilizzare un grafico di andamento storico:

1.  Per individuare le anomalie in un determinato momento per un punto dati, ad esempio alle 2:00 AM ogni giorno, la **latenza media delle richieste** di IIS passa da circa 200 ms a 500 ms.

2.  Per correlare più punti dati. Ad esempio, che mostra **latenza media richieste** e **numero medio di richiesta** insieme negli ultimi 15 giorni. Il report potrebbe mostrare che la latenza di richiesta e l'aumento di numero di richiesta allo stesso tempo campione, che potrebbe indicare che l'aumento di latenza richiesta sia causato da un aumento del numero di richieste.

In un grafico di andamento storico, gli utenti possono eseguire le operazioni seguenti:

* Mostrare più serie di dati nell'area del grafico. Ogni serie di dati viene visualizzato come un grafico a linee nel Visualizzatore di report. Ogni grafico a linee viene ridimensionato automaticamente per adattarsi nel Visualizzatore di report.

* Aggiungere o rimuovere una serie di dati dall'elenco di serie dei dati nella parte inferiore del Visualizzatore grafico di andamento storico.

* Mostrare o nascondere una serie di dati nell'elenco di serie di dati. Gli utenti possono fare clic su una serie di dati specifico nell'elenco per evidenziare il grafico a linee corrispondente nell'area del grafico.

* Lo zoom avanti per un determinato periodo di tempo, selezionare il periodo di tempo all'interno dell'area del grafico. Per eseguire lo zoom indietro, fare clic sul pulsante che si trova nell'angolo inferiore sinistro del grafico.

* Provare a utilizzare un singolo report facendo doppio clic su un particolare punto dati.

* Copiare i dati e renderli disponibili per altri programmi, ad esempio Microsoft Excel. Ciò consente di utilizzare Microsoft Excel per grafici di capacità, quando appropriato.

### <a name="trend-charts"></a>Grafici di tendenza

Molti problemi di prestazioni ricorrenti sono causati da attività periodiche in esecuzione sul o sui server di destinazione. Ad esempio, un sito Web di intrattenimento-oriented potrebbe ottenere risultati più durante il fine settimana, o un'attività di backup disco pianificazione potrebbe interferire con le prestazioni di un server di ogni giorno alle 2:00.

Un grafico di tendenza è progettato per individuare questi problemi di prestazioni. Problemi di prestazioni possono verificarsi ripetutamente in vari modelli. I modelli più diffusi sono modelli giornaliere e settimanale modelli in cui si verificano problemi di prestazioni durante la stessa ora di un giorno o giorno stesso della settimana. Pertanto, SPA fornisce un grafico di tendenza di 24 ore e un grafico di tendenza di 7 giorni.

Il grafico di analisi di tendenza fornisce un livello più profondo di analisi in un set di dati e la ricerca di tendenze in base all'ora del giorno. L'asse x è impostata su un periodo di 24 ore, a partire da 0:00 (mezzanotte) e termina in 23:59. SPA non mostra le tendenze per più serie di dati nello stesso momento. È possibile fare clic su **scegliere serie di dati** per selezionare una serie di dati per visualizzare.

Per elaborare i dati, il SISTEMA cerca tutti gli snapshot creati compreso tra 0:00 e 0:59 per ogni ora. SPA determina il minimo, massimo, medio e i valori per il set di snapshot in quell'ora sigma e li grafici come grafici a candela. SPA ripete il processo per gli snapshot creati tra 1:00 per 1:59, quindi 2:00 e 2:59 e così via. Se nessuno snapshot esiste per l'ora specificata, SPA lascia quell'ora vuota nel grafico e procede all'ora in avanti.

Un grafico di tendenza di 7 giorni è molto simile al grafico di tendenza di 24 ore. L'unica differenza è che raggruppa una serie di dati basata sul giorno della settimana anziché ora del giorno.

La serie di dati selezionato nella tendenza e grafici cronologici vengono archiviata come una preferenza dell'utente. Alla successiva apertura della tendenza e del visualizzatore grafico cronologico per lo stesso pacchetto di Advisor, viene elencato come predefinito lo stesso set di serie di dati.

## <a name="managing-reports"></a>Gestione di rapporti


### <a name="deleting-reports"></a>eliminazione di report

I report possono essere rimossi per ridurre al minimo il numero di report che devono essere gestiti tramite SPA. In base alla frequenza di report e il numero di server, si consiglia di eliminazione di report non necessari. Mentre SPA non dispone di un limite nei report che consentono di gestire, nel database sottostante siano presenti un limite di dimensione.

**Nota** non è possibile annullare l'eliminazione dei report eliminati.

 

### <a name="exporting-and-importing-reports"></a>Esportazione e importazione di report

I report possono essere esportati in un file XML al trasporto per un'altra console SPA o per posta elettronica a un altro utente. Esportazione del report non comporta l'eliminazione del report. Per esportare il report visualizzato al momento, da **Visualizzatore Report**, fare clic su **azioni**, quindi fare clic su **esportare**. Per esportare più report, in **Esplora report**fare clic su **Abilita selezione multipla**, selezionare più report nella casella di selezione e quindi fare clic su **Esporta**. I report vengono esportati in formato XML nella directory di destinazione selezionata.

È possibile visualizzare un report esportato in SPA. i report importati non vengono aggiunti al database SPA. Sono utilizzati principalmente per essere utilizzato come un visualizzatore XML per il report esportato. Non è necessario che il server per il report importato gli stessi Pack advisor installato come la console SPA originale del report esportato.

## <a href="" id="bkmk-manageadvisorpacks"></a>Gestione dei pacchetti di Advisor


SPA include pacchetti di Advisor per il sistema operativo principale, Hyper-V, Active Directory e IIS. SPA fornisce un'architettura aperta per lo sviluppo Pack di advisor tramite SQL, è anche possibile per gli sviluppatori non Microsoft compilare le versioni dei pacchetti di advisor. Sono disponibili quattro opzioni per la gestione di un pacchetto di preparazione: effettuare il provisioning, personalizzare, reimpostare o rimuovere.

### <a name="provision-new-advisor-packs"></a>Effettuare il provisioning di nuovi pacchetti di advisor

Nuovi pacchetti di advisor possono essere rilasciati da Microsoft o da sviluppatori non Microsoft. Un pacchetto di preparazione include un provisionMetaData.xml e un set di script SQL che descrivono la logica.

**Per eseguire il provisioning di un nuovo pacchetto di Advisor**

1.  copiare tutto il contenuto del pacchetto di Advisor nella directory *% SpaRoot%* \\APS.

2.  Nella finestra principale, fare clic su **configurazione**, quindi fare clic su **configurare Pack di Advisor**. Il **configurare Pack di Advisor** verrà visualizzata la finestra di dialogo.

    **Nota** Questa finestra di dialogo è simile alla pagina **Pack Advisor di provisioning** nella procedura guidata per la prima volta. Viene illustrato un elenco di pacchetti di advisor che sono disponibili per la gestione. Ogni pacchetto di advisor nell'elenco contiene le proprietà, quali nome, versione, versione e autore installati. Nome è il nome completo del pacchetto di preparazione e versione installata è la versione di questo pacchetto di advisor è già stato eseguito il provisioning del progetto. Se il pacchetto di Advisor non è stato sottoposti a provisioning nel database corrente, la casella di testo versione installata **non viene installata**. Il campo versione indica la versione Pack advisor, che si trova sotto la cartella Pack di advisor.

     

3.  Selezionare il pacchetto di Advisor nell'elenco. Se il pacchetto di Advisor non è stato sottoposto a provisioning o se è presente una versione più recente nella cartella Pack di Advisor rispetto a quella nel database, il pulsante **provisioning** è abilitato. Fare clic sui **provisioning** pulsante.

4.  Al termine del provisioning, il campo **versione installata** per il pacchetto di Advisor selezionato contiene le nuove informazioni sulla versione.

### <a name="customize-advisor-packs"></a>Personalizzare Pack di advisor

SPA definisce un'architettura aperta che consente agli utenti di modificare i pacchetti di advisor. Modificare le soglie, condivisione, soglie e abilitando o disabilitando le regole, gli utenti possono modificare i file di Service pack di advisor.

Per ulteriori informazioni su come modificare e compilare pacchetti di Advisor, vedere la [Guida di sviluppo Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="changing-thresholds"></a>Modificare le soglie

Le soglie vengono utilizzate in SPA per determinare se viene soddisfatta la condizione di attivazione di una regola. Le soglie effettive di scenari possono variare notevolmente a causa il carico di lavoro, l'ambiente hardware e business è necessario. Le soglie predefinite potrebbero non essere appropriate per il case utente corrente, pertanto SPA offre la possibilità di modificare la soglia.

È possibile modificare i valori di soglia facendo clic sul nome della regola in un report singolo o side-by-side. O per gestire tutte le regole di un pacchetto particolare advisor, è possibile modificare i valori soglia di **configurazione** menu.

**Per modificare un valore di soglia**

1.  Nel **configurazione** menu, fare clic su **configurare Pack di Advisor**, scegliere il nome del pacchetto di preparazione da modificare e quindi fare clic su **Configura**.

    **Nota** Viene visualizzato un elenco di tutte le regole incluse nel pacchetto di Advisor. La casella di controllo a sinistra del nome del Service pack di advisor indica se la regola è attivata. Se una regola è disabilitata, viene nascosta da tutti i report.

     

2.  Fare clic sulla regola specifica che si desidera modificare. Il **Dettagli regola** modulo per la regola selezionata verrà visualizzata.

Il modulo dettagli regola contiene informazioni dettagliate su una regola specifica. Include il nome, descrizione, stato, possibili risultati e le soglie. SPA supporta due tipi di risultati della regola, **avviso** e **OK**. Per ogni tipo è testo consigli e un'indicazione.

Alcune delle regole non è necessario soglie definite. Ad esempio, il **Keep-Alive HTTP** regola controlla il valore booleano per IIS. Pertanto, l'elenco di valori di soglia può essere vuota. In caso contrario, verranno elencate tutte le soglie utilizzate dalla regola corrente. Una descrizione dettagliata sull'utilizzo di una soglia della regola è inclusa come parte della descrizione.

Se il modulo dei **Dettagli della regola** viene avviato dal menu di **configurazione** , l'elenco di soglie include tre colonne: nome, impostazione originale e impostazione di modifica. Se viene avviato da un report singolo o side-by-Side, vengono inclusi anche i valori soglia utilizzati dal report. Gli utenti possono modificare i valori di soglia correnti modificando il valore nella colonna **impostazione modifiche** e quindi facendo clic su **Salva** per salvare le modifiche apportate al database.

Tutte le modifiche apportate alle soglie vengono applicate solo ai report generati dopo le modifiche. Queste modifiche non influiscono sui report esistenti.

### <a name="sharing-thresholds"></a>Condivisione delle soglie

Se si gestiscono i server in situazioni simili, è possibile scegliere di usare lo stesso set di soglie. È possibile esportare e importare le soglie per un pacchetto di advisor specifico utilizzando il **configurazione** menu. È possibile selezionare il pacchetto di advisor specifico e quindi fare clic su **Configura**. Il file esportato soglia è in formato XML.

Quando si importa una soglia, SPA convalida il formato di file XML e verifica che il file corrisponda al pack advisor selezionato. Se l'operazione viene completata, SPA Importa tutti i valori dal file di soglia nel database di progetto corrente. Analogamente allo scenario precedente per le soglie di modifica, tutte le modifiche ai valori di soglia diventano effettive solo per i report generati in futuro. I report esistenti non sono interessati.

### <a name="enable-or-disable-rules"></a>Abilitare o disabilitare le regole

Può essere attivata o disattivata da una regola di **Dettagli regola** form. È necessario fare clic su **salvare** per rendere persistenti le modifiche apportate. Se una regola è disabilitata, non verrà visualizzata in nessuno dei report. Tuttavia, la logica di business sottostante viene attivata durante la generazione del report. Pertanto, quando si sceglie di riabilitare la regola, viene visualizzata di nuovo nei report.

### <a name="reset-advisor-packs-to-original-state"></a>Reimposta i Pack di Advisor sullo stato originale

È possibile decidere di modificare un pack advisor provisioning nel database. Oltre alla modifica delle soglie, è inoltre possibile modificare gli script SQL. SPA non supporta la modifica dei metadati di report, o aggiungendo o rimuovendo regole per un pacchetto di provisioning advisor. Modificare manualmente le aree potrebbe causare un comportamento imprevisto.

Per ulteriori informazioni sulla modifica di un pacchetto di Advisor di cui è stato effettuato il provisioning, vedere la [Guida di sviluppo Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

Se si desidera eseguire il rollback delle modifiche apportate a un pacchetto di Advisor di cui è stato effettuato il provisioning, è possibile scegliere di reimpostare il pacchetto di Advisor. Vengono sovrascritti tutti gli script SQL correlati al pacchetto di Advisor e vengono reimpostati tutti i valori delle soglie predefinite. Questo consente di mantenere tutti i report esistenti.

la reimpostazione di Advisor Pack può essere eseguita tramite il modulo **Configure Advisor Packs** . È necessario selezionare il pacchetto di Advisor da ripristinare e quindi fare clic su **Reimposta**.

### <a name="remove-advisor-packs"></a>Rimuovi Pack di Advisor

Quando un pacchetto di advisor non è più necessario, gli utenti possono rimuoverlo dal database. la rimozione del pacchetto di Advisor consente di rimuovere tutti gli elementi relativi al pacchetto di Advisor dal database, incluse le regole e le soglie, tutti gli script SQL e tutti i report. Nessuna delle azioni può essere eseguito il rollback.

la rimozione del pacchetto di Advisor può essere eseguita tramite la configurazione del modulo **Pack di Advisor** . È necessario selezionare il pacchetto di preparazione da rimuovere e quindi fare clic su **Deprovision**.

### <a name="update-existing-advisor-packs"></a>Pack esistenti di preparazione aggiornamento

Pack esistenti di preparazione aggiornamento è molto simile alla reimpostazione pack di advisor allo stato originale. Per aggiornare il pacchetto di preparazione di una versione più recente, copiare il nuovo pacchetto di advisor nella cartella di Service pack di advisor. Un pacchetto di advisor è considerato un aggiornamento a un pacchetto di advisor esistente solo se è disponibile alcuna modifica dei metadati di report. Il report e le regole gli ID devono essere identici. In caso contrario, devono essere considerate come due pacchetti diversi advisor.

Se sono presenti solo modifiche della logica di business e non vengono apportate modifiche ai metadati del report per un pacchetto di Advisor, è necessario assegnare un nuovo numero di versione, ad esempio da 1,0 a 2,0. Se è presente una modifica dei metadati di report, il Service pack di advisor devono avere un nome diverso. Ad esempio, Microsoft.ServerPerformanceAdvisor.IIS.V1 potrebbe essere modificato in Microsoft.ServerPerformanceAdvisor.IIS.V2.

Se è presente una versione più recente del pacchetto di Advisor, l'elenco nel modulo **Configura Pack di Advisor** compila automaticamente la colonna **versione** con la versione più recente del pacchetto di Advisor. È possibile selezionare il pacchetto di Advisor e quindi fare clic sulla **reimpostazione**. Gli aggiornamenti di Service pack di advisor con le nuove regole business e le soglie. Vengono mantenuti tutti i report per questo Service pack di advisor.

## <a name="managing-servers"></a>Gestione dei server


SPA offre funzionalità di base per la gestione dei server di destinazione. È possibile scegliere di aggiungere nuovi server all'elenco di server di destinazione, rimuovere server dall'elenco o modificare la sezione Osservazioni per i server.

**Per aggiungere o rimuovere server o modificare le informazioni del server**

1.  Fare clic su di **configurazione** menu, fare clic su **Configura server**.

2.  Nell'elenco dei server presenti nel database del progetto, l'ultima riga è vuoto. Fare clic sulla riga e compilare i campi. Il **nome Server** e **percorso di condivisione File** cartella campi sono obbligatori e il nome del server deve essere univoco.

3.  Immettere le altre informazioni sul server nella colonna **contrassegno** per ogni server.

    **Nota** Questo campo usa un formato di testo libero, quindi è possibile usarlo come campo di descrizione. Utilizzare questo campo per contrassegnare i server in modo sono reperibili facilmente nella finestra principale o del gruppo di server, ad esempio, dal ruolo del server o posizione.

     

4.  Se si vuole usare SPA con un numero elevato di server, SPA supporta un formato con valori delimitati da virgole (CSV) per l'importazione. Il file deve contenere almeno due campi: **Server** e **percorso di condivisione File**. Il terzo campo, **remark** è facoltativo, ma è consigliabile organizzare i server. È inoltre possibile esportare l'elenco dei server in un file CSV per determinare il formato appropriato o eseguire il backup della configurazione del server.

### <a name="searching-and-filtering"></a>Ricerca e filtro

Se si gestiscono più server, SPA fornisce il supporto di base per individuare rapidamente i server nella finestra principale. È possibile fare clic sulla colonna intestazione per ordinare in base al nome del server, risultati dell'analisi, lo stato corrente delle attività o la sezione Osservazioni. È inoltre possibile scegliere di utilizzare la funzionalità di ricerca. Nell'angolo superiore destro della finestra principale, è possibile digitare una stringa da cercare. L'elenco dei **server di destinazione** nella finestra principale usa la stringa per filtrare i server e visualizzare solo i server con i campi nome o contrassegno che contengono la stringa di ricerca.

La figura seguente mostra in che modo la stringa **dell** corrisponde ai server con la stringa **dell** o Servers con il campo **remark** che contiene **dell**.

![esempio di filtro e ricerca](../media/server-performance-advisor/spa-user-manual-find-search-example.png)

## <a name="advanced-functionality"></a>Funzionalità avanzate


### <a name="working-with-spa-windows-powershell-cmdlets"></a>Utilizzare i cmdlet di Windows PowerShell SPA

La console SPA dispone di supporto tramite l'interfaccia Utente per la raccolta dati ricorrente. Se tale funzionalità non sono sufficienti per l'ambiente, sono disponibili i cmdlet di Windows PowerShell che un amministratore avanzato consente di personalizzare la raccolta dei dati. Questi cmdlet Windows PowerShell consentono agli amministratori di sistema per l'analisi delle prestazioni nei server di destinazione e utilizza il SISTEMA di supporto tecnico remoto automaticamente. Ad esempio, gli amministratori di sistema è possono scrivere script per richiamare i cmdlet SPA entro determinati intervalli di tempo per cui campionare periodicamente la condizione delle prestazioni dei server di destinazione.

È consigliabile chiudere l'applicazione SPAConsole.exe prima di utilizzare questi cmdlet.

Prima di eseguire qualsiasi cmdlet di Windows PowerShell, è necessario registrare i cmdlet sul computer della console.

**Per registrare i cmdlet di Windows PowerShell**

1.  Da un prompt dei comandi di Windows PowerShell con privilegi elevati, digitare **registerSpaCmdlets. cmd**. Viene visualizzato il messaggio **Register Spa cmdlets** .

2.  Eseguire **PowerShell.cmd SPA**. Se si passa il percorso a un file di script di Windows PowerShell, gli script vengono eseguiti automaticamente. In caso contrario, viene aperto un prompt dei comandi di Windows PowerShell, che è pronto per l'esecuzione dei cmdlet di Windows PowerShell per SPA.

Nella tabella seguente vengono descritti i cmdlet di SPA Windows PowerShell:

| Nome del cmdlet | Parametri | Descrizione |
| ------ | ------- | ------ |
| Inizio SpaAnalysis | **-Nomeserver** Nome del server di destinazione.<br>**-AdvisorPackName** Nome completo del pacchetto di Advisor da accodare nel server. Quando è pianificata l'esecuzione di più di un pacchetto allo stesso tempo, il valore del parametro deve essere formattato come AP1name, AP2name.<br>**-Durata** Durata della raccolta dei dati.<br>**-Credential** Credenziali utente per l'account che esegue la raccolta di dati nel server di destinazione.<br>**-SqlInstanceName** Nome dell'istanza di SQL Server.<br>**-SQLDatabaseName** Nome del database del progetto SPA. | Avvia una sessione di raccolta dati SPA nel server specificato. |
| Stop-SpaAnalysis | **-SqlInstanceName** Nome dell'istanza di SQL Server.<br>**-SQLDatabaseName** Nome del database del progetto SPA.<br>**-Nomeserver** Nome del server di destinazione. | Tenta di arrestare una sessione SPA in esecuzione. Se una sessione è già completa, viene restituito senza eseguire alcuna operazione. |
| Get-SpaServer | **-SqlInstanceName** Nome dell'istanza di SQL Server.<br>**-SQLDatabaseName** Nome del database del progetto SPA. | Ottiene l'elenco di server nel database. Restituisce un elenco di oggetti, tra cui queste proprietà: nome, stato, condivisione file e osservazioni. |
| Get-SpaAdvisorPacks | **-SqlInstanceName** Nome dell'istanza di SQL Server<br>**-SQLDatabaseName** Nome del database del progetto SPA | Ottiene l'elenco di Service pack di advisor nel database. Restituisce un elenco di oggetti, tra cui queste proprietà: Name, DisplayName, autore e versione. |

Windows PowerShell offre la possibilità di passare le credenziali tramite i file crittografati per abilitare gli scenari di automazione. Per altre informazioni sull'uso di file crittografati per passare le credenziali a un cmdlet, vedere [creare script di Windows PowerShell che accettano le credenziali](https://technet.microsoft.com/magazine/ff714574.aspx).

### <a name="automating-spa-report-collection-by-using-windows-powershell"></a>Automazione di raccolta report SPA utilizzando Windows PowerShell

Le procedure seguenti rappresentano un esempio di come automatizzare un insieme di report SPA utilizzando i cmdlet di Windows PowerShell SPA.

Le credenziali dell'utente possono essere crittografate e memorizzato nella cache tramite Windows PowerShell. Queste credenziali vengono utilizzate per accedere al server remoto. Sebbene non specifico di SPA, nell'esempio seguente viene illustrata questa tecnica:

``` syntax
$fileName = 'D:\temp\operator.txt'
$userName = 'domainname\operator'
 
# save credential to file
$(Get-Credential).Password | convertFrom-SecureString | Set-Content $fileName
 
# load credential from file
$credential = New-Object System.Management.Automation.PsCredential $userName, $(Get-Content $fileName | convertTo-SecureString)
 
# run command
.\start-SpaAnalysis  ServerName: Server1  Credential: $credential  AdvisorPackName:Microsoft.ServerPerformanceAdvisor.CoreOS.V1 10  Duration:10  SqlInstanceName: .\SQLExpress  SqlDatabaseName:SPA8294
```

Prima di poter eseguire questo file, è necessario eseguire **Set-ExecutionPolicy RemoteSigned**

È possibile eseguire da un file batch utilizzando il comando seguente:

``` syntax
PowerShell -command "& '.\RunSpa.ps1' "
```

### <a name="use-t-sql-to-generate-reports"></a>Utilizzare T-SQL per generare report

Utilizzando SQL per creare report personalizzati non vengono forniti all'interno di SPAConsole.exe, è possono estrarre i dati del report SPA.

ad esempio, il comando T-SQL seguente fornisce un elenco dei primi 10 server per CPU per l'intervallo di tempo che copre gli ultimi tre giorni:

``` syntax
select TOP 10
   MachineName,
   AverageCpu
FROM (
   select
      __MachineName MachineName,
      AVG(AverageValue) AverageCpu
   FROM [SPA].[Microsoft.ServerPerformanceAdvisor.CoreOS.V1].vwCpuPerformance
   WHERE __Creationtime > dateadd(DAY, -3, GETUTcdate())
      AND Name = N'% Processor time' AND CpuId = N'_Total'
   GROUP BY __MachineName
) t
OrdER BY t.AverageCpu DESC 
```

### <a name="working-with-multiple-projects"></a>Utilizzo di più progetti

In alcuni casi, si può decidere di partizionare i database di SQL Server utilizzati da SPA. Ciò consente di ridurre le dimensioni di dati del database di SQL Server o per suddividere logicamente il server. In questi casi, la console SPA supporta più progetti. È possibile creare un nuovo database di progetto, fare clic su **File**, e quindi fare clic su **Nuovo progetto**. Viene visualizzata la prima procedura guidata usa per creare il nuovo database del progetto.

Dopo aver creati i progetti, è possibile fare clic su **File**, quindi fare clic su **Apri progetto** per passare tra i progetti. La console SPA non consente il cambio di database durante l'esecuzione di analisi delle prestazioni in sospeso all'interno del progetto attualmente aperto. Si tratta di proteggere l'integrità dei dati sulle prestazioni.

Quando si sceglie di creare un nuovo progetto database o aprire un altro progetto database, il progetto corrente viene chiuso. Tutti i report aperti vengono chiusi quando viene chiuso il progetto corrente.

**Nota** SQL Server 2008 R2 Express ha un limite del database di 10 GB. Utilizzando più progetti, è possibile utilizzare uno o più database di SQL Server e restare sotto il limite di 10 GB SQL Server 2008 R2 Express.

 

### <a name="logging-and-debugging"></a>Registrazione e debug

SPA fornisce funzionalità di registrazione di base. Consente solo i registri in cui scrivere un file di registro che si trova nella stessa cartella SPAConsole.exe. L'account utente che esegue la console SPA deve disporre dell'autorizzazione di scrittura per la cartella in cui è stato installato il SISTEMA per assicurarsi che il file di log possono essere scritti i log.

SPA contiene i seguenti livelli di log valido:

* **Informativo** esegue il dump dei log per ogni azione che accetta la console SPA, ed è progettato principalmente a scopo di debug.

* **Avviso** Registra tutti i problemi e le eccezioni che si verificano all'interno della console di SPA. Alcuni degli errori sono semplicemente gli errori di convalida che possono essere gestiti dalla console di SPA.

* **Critico** Registra solo errori ed eccezioni che non possono essere gestite dalla console di SPA. Questi errori comportano l'arresto anomalo della console SPA. I registri di forniscono le informazioni di contesto per tali errori.

Per impostazione predefinita, il livello di registrazione è Warning, il che significa che SPA registra solo gli errori e le eccezioni che si verificano in SPA. Il livello di registrazione può essere modificato modificando il **SpaConsole.exe.config** file SpaConsole.exe si trova nella stessa cartella. Tutti i log vengono scritti i file di log. txt nella stessa cartella.

SPA fornisce inoltre alcune funzionalità di base per il debug. Per attivare il debug per l'autenticazione SPA, gli utenti devono modificare manualmente il database di progetto di applicazione a singola PAGINA. L'impostazione è archiviata in una tabella di configurazioni. È necessario eseguire lo script SQL seguente per modificare il progetto di applicazione a singola PAGINA per eseguire il debug in modalità utente:

``` syntax
UPdate [Configurations] SET Value = N'true' WHERE Name = N'Debugmode'.
```

In modalità debug, il framework SPA mantiene tutti i dati temporanei che viene generati durante l'analisi delle prestazioni, pertanto è possibile risolvere i problemi nei Pack di advisor. Questo script è progettato principalmente per essere utilizzata dagli sviluppatori di Service pack di advisor.

Per ulteriori informazioni sulle funzionalità di debug di SPA, vedere la [Guida di sviluppo Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="managing-the-database"></a>Gestione del database

### <a name="backup-and-restore-the-database"></a>Backup e ripristino del database

La console SPA non fornisce funzionalità per il backup e ripristinare il database SPA. Utilizzare gli strumenti di Microsoft SQL Server standard e gli script per eseguire il backup e ripristino dei database.

### <a name="clean-up-the-database"></a>pulire il database

I database di progetto di applicazione a singola PAGINA possono raggiungere dimensioni durante l'esecuzione di ulteriori analisi delle prestazioni. Per impostazione predefinita, il SISTEMA mantiene tutti i report. Rimozione di vecchi report per migliorare le prestazioni. Per eliminare un report tramite il **Esplora Report** interfaccia.

## <a name="privacy-and-security"></a>Privacy e sicurezza


SPA supporta solo la raccolta dati dal server di destinazione nella console di SPA. Non è progettata per inviare le informazioni a Microsoft o gli sviluppatori non Microsoft. Per ulteriori informazioni sulla privacy SPA, fare riferimento alle condizioni di licenza Software Microsoft per Server Performance Advisor.

Tutti i dati raccolti da SPA vengono archiviati nei database del progetto. A causa della natura di alcune delle tracce ETW, SPA potrebbe raccogliere informazioni riservate in grado di importanza elevata business. È necessario essere consapevoli del rischio potenziale associato condividono l'accesso ai database di progetto di applicazione a singola PAGINA. File di log temporanei vengono salvati in cartelle condivise che vengono specificate in ogni computer di destinazione. Anche se SPA tenta di eliminare i file di log temporanei al termine dell'importazione dei dati, non vi è alcuna garanzia che i file di log vengano sempre eliminati. È necessario assicurarsi che le cartelle condivise si trovano in percorsi sicuri.

Pack di advisor SPA contiene script SQL per analizzare e analizzare i registri di prestazioni per generare report sulle prestazioni. Il SISTEMA tenta di limitare gli script eseguiti con il privilegio. Tuttavia, è comunque possibile che gli script possono raccogliere informazioni riservate tramite SPA dal server di destinazione, oppure ottenere o modificare le informazioni riservate archiviate nel database del progetto stesso SPA. È necessario assicurarsi che tutti i Pack advisor vengono eseguito il provisioning di database del progetto SPA provengono da fonti attendibili.

## <a name="errors-and-troubleshooting"></a>Errori e risoluzione dei problemi


### <a name="spaconsoleexe-does-not-start-or-write-log-file"></a>SPAConsole.exe non avviare o scrivere file di log

Quando si tenta di eseguire SPAConsole. exe per la prima volta, se il .NET Framework non è installato, l'applicazione non avvia o scrive un file di log. Verificare che un compatible.NET che Framework sia installato e funzionante in modo corretto prima di avviare il SISTEMA.

### <a name="locating-log-information"></a>Individuazione delle informazioni di log

SPA archivia informazioni sull'errore nel file di log. txt nella cartella SPA. i messaggi di errore dettagliati e le informazioni sullo stack di chiamate vengono scritti in questa cartella. Se si verificano errori che richiedono ulteriori informazioni su come interpretare, è possibile aprire il file di log. txt per visualizzare i dettagli dell'errore.

### <a name="database-size-limitations-for-sql-server-express"></a>Limiti delle dimensioni del database di SQL Server Express

SQL Server Express dispone di una dimensione massima di 10 GB per un database utente. Questo database di dimensioni ha la capacità di archiviazione di report da 20.000-30.000. Per ridurre la possibilità di raggiungere il limite di database, è consigliabile eliminare regolarmente i report e/o un'altra versione di SQL Server che non dispone il limite del database utente 10 GB.

### <a name="sql-server-express-log-size-and-disk-capacity"></a>SQL Server Express dimensioni e il disco capacità registro

Se si utilizza SQL Server Express, il database utente è limitato a 10 GB, ma il file di log corrispondente può superare 70 GB. Per questi motivi, è consigliabile più di 100 GB di spazio libero su disco per SQL Server Express. Questo spazio su disco dovrebbe essere sufficiente per archiviare circa i report da 20.000 a 30.000. Questo file di log è denominato SPADB\_log. ldf e si trova in **% Program Files%\\Microsoft SQL Server\\MSSQL10. SQLEXPRESS\\MSSQL\\DatA**.

### <a name="failure-to-connect-to-target-server"></a>Impossibile connettersi al server di destinazione

Quando si esegue l'analisi delle prestazioni nei server di destinazione, l'account utente che esegue la console SPA deve disporre di determinati privilegi per l'agente di raccolta dati impostata su PIA nei server di destinazione della coda. Impostazioni di Windows Firewall è inoltre necessario essere modificata per consentire il passaggio delle comunicazioni erano messe a Disposizione. Per istruzioni di configurazione dettagliati, vedere [impostazione SPA](#bkmk-setupspa).

### <a name="failure-to-create-pla-data-collection-set-on-the-target-server"></a>Errore durante la creazione di Set di raccolta dati erano messe a Disposizione sul server di destinazione

Se non è possibile creare un set di raccolta dati di PLA nel messaggio del server di destinazione, assicurarsi di eseguire le operazioni seguenti:

* Assicurarsi che il **gli avvisi e registri di prestazioni** servizio è in esecuzione

* L'impostazione di protezione **accesso alla rete: non consentire l'archiviazione delle password e credenziali per l'autenticazione di rete** è disabilitato. L'impostazione di sicurezza deve essere disabilitato perché SPA deve utilizzare le credenziali utente per creare il Set di raccolta dati nel server di destinazione.

### <a href="" id="running-spa-against-the-console-"></a>Esecuzione di SPA sulla console

Se il server di destinazione è quello utilizzato per la console SPA, erano messe a Disposizione passa attraverso un canale diverso. Anche se l'account utente esegue la console SPA con privilegi di amministratore, PLA non riesce. Se in un server di destinazione è installata la console SPA, gli utenti devono avviare SPA come amministratore per assicurarsi che l'attività di analisi delle prestazioni è possibile eseguire nella console.

### <a name="running-multiple-consoles-at-the-same-time"></a>Esecuzione di più console nello stesso momento

SPA non supporta più console in esecuzione nello stesso database di progetto di applicazione a singola PAGINA nello stesso momento. SPA non fornisce inoltre il meccanismo di blocco e la sincronizzazione per impedire che si verifichi. Se due console SPA sono in esecuzione nello stesso momento, la console si comporterà in modo incoerente a seconda la sequenza temporale che eseguono tali console SPA. Per evitare questo problema, prima che venga elaborato dalla console che avvia l'analisi di tutte le sessioni di analisi delle prestazioni in coda devono essere rimosso dall'elenco.

SPA protegge l'integrità di ogni report che è stato generato dall'applicazione a singola PAGINA. allo stesso tempo, SPA non garantisce che tutte le attività di analisi in coda siano completate. Se si notano incoerente stato viene modificato per le sessioni di analisi delle prestazioni o gli errori che il sistema di attestazione non è possibile trovare il set di registri di prestazioni generati dall'agente di raccolta dati, è potrebbero essere causati da più istanze di console SPA in esecuzione nello stesso database di progetto di applicazione a singola PAGINA.

Esecuzione del cmdlet di Windows PowerShell SPA potrebbero verificarsi problemi anche da una console SPA che è in esecuzione nello stesso database SPA. Si consiglia di chiudere la console SPA prima di eseguire i cmdlet Windows PowerShell SPA.

### <a name="spaconsoleexe-recurring-collection-is-disrupted"></a>Interruzione raccolta ricorrente SPAConsole.exe

Quando si esegue il SPAConsole.exe e si utilizza una raccolta di dati ricorrenti (ad esempio, una raccolta oraria), il server che esegue il SPAConsole.exe non deve essere in modalità di risparmio energia in modo che è possibile sospendere. SPA non controlla la presenza di un criterio di risparmio energia. Questa attività sospesa potrebbe interferire con la raccolta dei dati regolare ricorrente.

### <a name="lost-etw-events"></a>Eventi ETW persi

Per creare impatto minimo sulle prestazioni nei server di destinazione, erano messe a Disposizione è progettata per funzionare con priorità bassa, mentre è la raccolta di informazioni sulle prestazioni. Se il server di destinazione è occupato, PLA può eliminare alcune delle attività di raccolta dati per ottenere le attività ad alta priorità in esecuzione nei server di destinazione. È consigliabile configurare la cartella condivisa in cui gli eventi vengono scritti su un disco che non è in conflitto con l'I/O del carico di lavoro o un'unità più veloce, ad esempio un'unità SSD. Quando gli eventi vengono eliminati, poiché gli eventi persi possono influire sull'affidabilità delle metriche delle prestazioni generati vengono segnalati nella visualizzazione report.

Alcuni problemi di autorizzazione potrebbero causare anche del Registro di sistema o le query WMI vengono ignorate. Tuttavia, ciò è molto meno probabile che risultino più perdita di eventi ETW. Di conseguenza, i risultati di set di agenti di raccolta dati non contengano tutti i valori richiesti. È necessario assicurarsi che la situazione viene gestita dagli script T-SQL per tutti i pacchetti di advisor. Se i dati non esistono nel risultato della raccolta dei dati, vengono contrassegnati come nessun dato nei report.

Poiché la perdita di eventi ETW è comune che erano messe a Disposizione, i punti dati che vengono generati in base una traccia ETW potrebbe non essere coerenti con i punti dati che vengono generati basato su, ad esempio, i contatori delle prestazioni. Ad esempio, è possibile vedere che l'utilizzo totale della CPU da IIS è 80% (che deriva da contatori delle prestazioni) e che nella parte superiore gli URL di soli 10% di tutto il tempo di CPU (che è un punto dati da cui deriva la traccia ETW). In genere, è possibile visualizzare l'origine dati per un punto dati tramite la descrizione comando del punto dati. È necessario essere consapevoli dell'impatto di tali perdite di dati.

Per evitare di perdere i dati evento, la cartella di risultato deve essere chiuso al server di destinazione.

Se i risultati dell'agente di raccolta dati contengono dati incompleti diversi dalla perdita di tracce ETW e lo sviluppatore di Advisor Pack ha aggiunto il supporto per la notifica della perdita di eventi ETW, nella parte superiore del singolo report viene visualizzata una barra informazioni per notificare all'utente la presenza di potenziali report incoerente causato dalla perdita di dati. le informazioni dettagliate sulla perdita di dati sono reperibili nel file log. txt.

## <a name="glossary"></a>Glossario


Ecco alcuni dei termini usati con SPA:

* **Pack Advisor** una raccolta di metadati e script T-SQL che elaborano i registri di prestazioni vengono raccolti dal server di destinazione. Il pacchetto di advisor genera quindi report dai dati di log delle prestazioni. I metadati nel pacchetto di advisor definiscono i dati da raccogliere da server di destinazione per le misurazioni delle prestazioni. I metadati definiscono inoltre il set di regole, le soglie e il formato del report. In genere, un pacchetto di advisor è scritto specificamente per un unico ruolo del server, ad esempio, Internet Information Services (IIS).

* **Console SPA** SpaConsole.exe, che rappresenta la parte centrale di SPA. SPA non è necessario eseguire nel server di destinazione che si sta verificando. La console SPA contiene tutte le interfacce utente per l'autenticazione SPA, dall'impostazione del progetto per l'esecuzione dell'analisi e visualizzazione di report. Per impostazione predefinita, il SISTEMA è un'applicazione a due livelli. La console SPA contiene il livello dell'interfaccia Utente e una parte del livello di logica di business. La console SPA pianifica ed elabora le richieste di analisi delle prestazioni.

* **Framework SPA** fornisce tutte le interfacce utente, elaborazione dei registri di prestazioni, configurazione, la gestione degli errori e procedure di gestione e API di database.

* **Progetto SPA** un database che contiene tutte le informazioni sui server di destinazione, Pack di advisor e analisi delle prestazioni indica che vengono generati nei server di destinazione per i pacchetti di advisor. È possibile confrontare e visualizzare i grafici cronologia e la tendenza nello stesso progetto di applicazione a singola PAGINA. È possibile creare più di un progetto. I progetti SPA sono indipendenti una da altra e non sono presenti dati condivisi tra progetti.

* **Server di destinazione** il computer fisico o macchina virtuale che esegue Windows Server con determinati ruoli del server, ad esempio IIS.

* **Sessione di analisi dei dati** un'analisi delle prestazioni in un server di destinazione specifico. Una sessione di analisi di dati può includere più Pack di advisor. I set di agenti di raccolta dati da tali Pack advisor vengono uniti in un insieme agenti di raccolta dati. Tutti i registri di prestazioni per una sessione di analisi dati vengono raccolti nello stesso periodo di tempo. Analisi dei report generati da Pack di advisor in esecuzione nella stessa sessione di analisi di dati consente agli utenti di comprendere la situazione complessiva delle prestazioni e identificare le cause principali problemi di prestazioni.

* **Traccia eventi per Windows** un sistema di traccia ad alte prestazioni sovraccarica scalabile, che viene fornito in Windows. Fornisce funzionalità, che può essere utilizzata per risolvere una varietà di scenari di debug e profilatura. SPA utilizza gli eventi ETW come origine dati per la generazione di report di prestazioni. Per informazioni generali su ETW, vedere [migliora il debug e ottimizzazione delle prestazioni con ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

* **Strumentazione gestione Windows (WMI)** dell'infrastruttura per i dati di gestione e operazioni in Windows. È possibile scrivere script WMI o applicazioni per automatizzare le attività amministrative sui computer remoti. WMI fornisce anche i dati di gestione ad altre parti del sistema operativo e ai prodotti. SPA utilizza i punti dati e informazioni sulle classi WMI come origini per la generazione di rapporti di prestazioni.

* **I contatori delle prestazioni** per fornire informazioni sul funzionamento del sistema operativo o un'applicazione, servizio o driver viene utilizzato. I dati del contatore delle prestazioni consentono di determinare i colli di bottiglia e ottimizzare le prestazioni di sistema e dell'applicazione. Il sistema operativo, rete e dispositivi forniscono dati contatore che un'applicazione possono essere utilizzati per fornire agli utenti di visualizzare graficamente le prestazioni del sistema. SPA utilizza punti dati e informazioni sui contatori delle prestazioni come origini per generare report di prestazioni.

* **AVVISI e registri di prestazioni** raccoglie e registri di prestazioni e le tracce genera avvisi di prestazioni quando vengono soddisfatte determinate trigger. Erano messe a Disposizione consente di raccogliere i contatori delle prestazioni, eventi di traccia per Windows (ETW), query WMI, chiavi del Registro di sistema e configurazione di file. PIA supporta inoltre la raccolta dati remoti tramite chiamate di procedura remota (RPC). L'utente definisce un insieme agenti di raccolta dati, che include le informazioni sul dati da raccogliere, frequenza di raccolta dati, la durata di raccolta dati, filtri e un percorso per salvare il file dei risultati. SPA utilizza erano messe a Disposizione per raccogliere tutti i dati sulle prestazioni dal server di destinazione.

* **Singolo report** SPA un rapporto generato basato su sessione di analisi di dati per la preparazione di un Service pack in un singolo server di destinazione. Può contenere diverse sezioni di dati e notifiche.

* **Report side-by-side** report A SPA che confronta due report singolo per il pacchetto stesso di advisor. I due report possono essere generati dal server di destinazione diversa o da esecuzioni dell'analisi delle prestazioni separato sullo stesso server di destinazione. Il report side-by-side consente di creare la funzionalità per confrontare due report per consentire agli utenti di identificare i comportamenti anomali o le impostazioni in uno dei report. Un report side-by-side contiene diverse sezioni di dati e notifiche. In ogni sezione dati da entrambi i report vengono elencati side-by-side.

* **Grafico di tendenza** report SPA oggetto utilizzato per analizzare modelli ricorrenti di problemi di prestazioni. Molti problemi di prestazioni ricorrenti sono causati da modifiche del carico server pianificato dal server o dal computer client, ciò può verificarsi giornaliera o settimanale. SPA fornisce un grafico di tendenza di 24 ore e un grafico di tendenza di 7 giorni di identificare tali problemi.

    L'utente può scegliere una o più serie di dati alla volta, ovvero un valore numerico all'interno del singolo report, ad esempio **totale utilizzo medio della CPU**. in particolare, un valore numerico è un valore scalare da un singolo server generato da un singolo punto di accesso in un'istanza di tempo specificata. SPA tali valori vengono raggruppati in 24 gruppi, uno per ogni ora del giorno (7 per un report di 7 giorni, uno per ogni giorno della settimana). SPA calcola una media, minimo, massimo e deviazioni standard per ogni gruppo.

* **Grafico di andamento storico** report SPA oggetto utilizzato per visualizzare le modifiche in alcuni numerico valori all'interno di singolo report per un determinato server e advisor coppia pack nel tempo. L'utente può scegliere di più serie di dati e visualizzarli insieme nel grafico cronologia per comprendere la correlazione tra le serie di dati diversi.

* **Serie di dati** dati numerici che vengono raccolti dalla stessa origine dati in un periodo di tempo. La stessa origine significa che i dati deve provenire dallo stesso server di destinazione, ad esempio la lunghezza della coda Media delle richieste per IIS in un unico server.

* **Regole** combinazioni di logica, le soglie e descrizioni. Rappresentano un potenziale problema di prestazioni. Ogni pacchetto di advisor contiene più regole. Ogni regola viene attivata da un processo di generazione di report. Una regola si applica la logica e le soglie per i dati nel report singola. Se vengono soddisfatti i criteri, viene generata una notifica di avviso. Se non è impostata la notifica di **OK** dello stato. Se non si applica la regola, la notifica viene impostata su non applicabile (**NA**) dello stato.

* **Notifiche** informazioni che consente di visualizzare una regola per gli utenti. Include lo stato della regola (**OK**, **NA**, o un **avviso**), il nome della regola e indicazioni possibili per risolvere i problemi di prestazioni.
