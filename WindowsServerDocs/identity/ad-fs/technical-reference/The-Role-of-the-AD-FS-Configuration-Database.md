---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: Ruolo del database di configurazione di AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9ffdd1876e2dfbc044cebb65d7d6ef80880a64b8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860164"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>Ruolo del database di configurazione di AD FS
Nel database di configurazione AD FS vengono archiviati tutti i dati di configurazione che rappresentano una singola istanza di Active Directory Federation Services \(AD FS\) \(, ovvero servizio federativo\). Il database di configurazione di ADFS definisce il set di parametri richiesti dal Servizio federativo per identificare partner, certificati, archivi attributi, attestazioni e diversi dati relativi a queste entità associate. È possibile archiviare i dati di configurazione in un database di&reg; Microsoft SQL Server o nel database interno di Windows \(WID\) funzionalità inclusa in Windows Server&reg; 2008, Windows Server 2008 R2 e Windows Server&reg; 2012.  
  
> [!NOTE]  
> L'intero contenuto del database di configurazione di ADFS può essere archiviato in un'istanza di Database interno di Windows o in un'istanza del database SQL, ma non in entrambi. Ciò significa che non si possono avere alcuni server federativi che usano Database interno di Windows e altri che usano un database SQL Server per la stessa istanza del database di configurazione di ADFS.  
  
Per scoprire i vantaggi e gli svantaggi della scelta di Database interno di Windows o di SQL Server per archiviare il database di configurazione di ADFS, è possibile usare le informazioni in questo argomento insieme al contenuto disponibile in [Considerazioni sulla topologia di distribuzione di ADFS](https://technet.microsoft.com/library/gg982489.aspx) :  
  
Database interno di Windows utilizza un archivio dati relazionale e non dispone di un'interfaccia utente gestione \(dell'interfaccia Utente\). Gli amministratori possono invece modificare il contenuto del database di configurazione AD FS usando lo snap-in di gestione AD FS\-in, Fsconfig. exe o i cmdlet di&trade; di Windows PowerShell.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Uso di Database interno di Windows per archiviare il database di configurazione di ADFS  
È possibile creare il database di configurazione di AD FS usando WID come archivio usando lo strumento da riga\-comando Fsconfig. exe o la configurazione guidata del server AD FS Federazione. Quando si usa uno di questi strumenti, è possibile scegliere una qualsiasi delle opzioni seguenti per creare la topologia del server federativo. Ognuna di queste opzioni usa Database interno di Windows per archiviare il database di configurazione di ADFS:  
  
-   Creare un supporto\-server federativo da solo  
  
-   Creare il primo server federativo in una server farm federativa  
  
-   Aggiungere un server federativo a una server farm federativa  
  
Se si seleziona l'opzione\-autonoma, viene utilizzato WID per archiviare una singola istanza del database di configurazione AD FS. Questa istanza non può essere condivisa tra più server federativi. È destinato solo agli ambienti di testing. Per ulteriori informazioni al supporto\-da solo l'opzione server federativo o come configurarne uno, vedere [autonomo Federation Server tramite WID](https://technet.microsoft.com/library/gg982486.aspx) o [creare un Server federativo autonomo](https://technet.microsoft.com/library/ee913579.aspx).  
  
Se si seleziona il primo server federativo nell'opzione per una server farm federativa, Database interno di Windows viene configurato ai fini della scalabilità per poter aggiungere altri server federativi alla farm in un secondo momento. Per altre informazioni sulla distribuzione di una farm con Database interno di Windows o su come configurarne una, vedere [Server farm federativa che usa Database interno di Windows](https://technet.microsoft.com/library/gg982492.aspx) o [Creare il primo server federativo in una server farm federativa.](https://technet.microsoft.com/library/dd807070.aspx)  
  
Se si sceglie l'opzione per aggiungere un server federativo, Database interno di Windows viene configurato per replicare al nuovo server federativo le modifiche fatte al database di configurazione a intervalli prestabiliti. Per altre informazioni sull'aggiunta di un server federativo a una farm con Database interno di Windows, vedere [Server farm federativa che usa Database interno di Windows](https://technet.microsoft.com/library/gg982492.aspx) o [Aggiungere un server federativo a una server farm federativa](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Quando si distribuisce una server farm federativa tramite WID, alcune funzionalità di ADFS non siano disponibili. Per avere accesso all'intero set di funzionalità quando si configura la server farm, considerare la possibilità di usare invece Microsoft SQL Server per archiviare il database di configurazione di ADFS. Per altre informazioni, vedere [Considerazioni sulla topologia di distribuzione di ADFS](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Funzionamento di una server farm federativa con Database interno di Windows  
In questa sezione sono disponibili concetti importanti che descrivono in che modo la server farm federativa che usa Database interno di Windows replica i dati tra un server federativo primario e i server federativi secondari. .  
  
#### <a name="primary-federation-server"></a>Server federativo primario  
Un server federativo primario è un computer che esegue Windows Server 2008, Windows Server 2008 R2 o Windows Server&reg; 2012 che è stato configurato nel ruolo server federativo con la configurazione guidata server federativo di AD FS e che dispone di una copia di lettura/scrittura del database di configurazione AD FS. Il server federativo primario viene sempre creato quando si utilizza configurazione guidata Server federativo AD ADFS e selezionare l'opzione per creare un nuovo servizio federativo e rendere il computer del primo server federativo nella farm. Tutti gli altri server federativi della farm, detti anche server federativi secondari, devono sincronizzare le modifiche fatte sul server federativo primario in una copia del database di configurazione di ADFS archiviato localmente.  
  
#### <a name="secondary-federation-servers"></a>Server federativi secondari  
I server federativi secondari archiviano una copia del database di configurazione AD FS dal server federativo primario, ma queste copie sono di sola lettura\-. I server federativi secondari si connettono al server federativo primario nella farm e sincronizzano i dati, eseguendo il polling a intervalli regolari per verificare se i dati sono stati modificati. I server federativi secondari disponibili per fornire tolleranza di errore per il server federativo primario quando agisce per caricare\-bilanciare le richieste di accesso che vengono inviate ai diversi siti in tutto l'ambiente di rete.  
  
> [!NOTE]  
> Se un server federativo primario si arresta in modo anomalo ed è offline, tutti i server federativi secondari continuano a elaborare le richieste normalmente. Tuttavia, nessuna modifica può essere apportata al servizio federativo finché il server federativo primario non ritorna online. È anche possibile usare Windows PowerShell per nominare un server federativo secondario come server federativo primario. Per ulteriori informazioni, vedere l' [ad FS amministrazione con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>Sincronizzazione del database di configurazione di ADFS  
A causa del ruolo importante svolto dal database di configurazione AD FS, viene reso disponibile in tutti i server federativi della rete per fornire funzionalità di tolleranza di errore e bilanciamento del carico\-durante l'elaborazione delle richieste \(quando si utilizzano i bilanciamento del carico di rete\-\). Tuttavia, per fare in modo che i server federativi secondari sfruttino questa funzionalità, è necessario che il database di configurazione di ADFS archiviato nel server federativo primario sia sincronizzato.  
  
Quando si aggiunge un server federativo alla farm, il nuovo computer che diventerà un server federativo secondario si connette al server federativo primario per replicare la copia del database di configurazione di ADFS. Da questo momento, il nuovo server federativo continua a eseguire il pull degli aggiornamenti dal server federativo primario a intervalli regolari, come illustrato nella figura seguente.  
  
![Ruoli di AD FS](media/adfs2_WID.png)  
  
Ogni server federativo secondario esegue il polling al server federativo primario ogni cinque minuti per rilevare le modifiche. È possibile modificare questo valore predefinito di cinque\-minuti o forzare una sincronizzazione immediata in qualsiasi momento usando un cmdlet di Windows PowerShell. Per ulteriori informazioni su come eseguire questa operazione, vedere [amministrazione di ADFS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
Anche il processo di sincronizzazione di Database interno di Windows supporta i trasferimenti incrementali per garantire trasferimenti più efficienti delle modifiche intermedie. Il processo di trasferimento incrementale richiede in sostanza meno traffico sulla rete e i trasferimenti sono completati più rapidamente.  
  
> [!NOTE]  
> È supportata la migrazione da un database di configurazione di ADFS da Database interno di Windows a un'istanza di SQL Server. Per ulteriori informazioni su come eseguire questa operazione, vedere [ADFS: migrazione del Database di configurazione di ADFS a SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) sul sito TechNet Wiki.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Uso di SQL Server per archiviare il database di configurazione di ADFS  
È possibile creare il database di configurazione di AD FS usando una singola istanza di SQL Server database come archivio usando il comando Fsconfig. exe\-strumento da riga. L'uso di un database SQL Server come database di configurazione di ADFS offre i seguenti vantaggi rispetto a Database interno di Windows:  
  
-   Consente agli amministratori di sfruttare le funzionalità a disponibilità elevata di SQL Server.  
  
-   Offre un ulteriore miglioramento delle prestazioni in presenza di traffico elevato.  
  
-   Fornisce supporto delle funzionalità per la risoluzione degli artefatti SAML e SAML/WS\-il rilevamento della riproduzione del token federativo \(descritto di seguito\).  
  
Il termine "server federativo primario" non si applica quando il database di configurazione AD FS viene archiviato in un'istanza del database SQL, perché tutti i server federativi possono leggere e scrivere in modo analogo nel database di configurazione AD FS che utilizza la stessa istanza del SQL Server cluster, come illustrato nella figura seguente.  
  
![Ruoli di AD FS](media/adfs2_SQL.png)  
  
È possibile utilizzare SQL Server per configurare due o più server in modo che funzionino insieme come cluster di server per garantire che AD FS sia reso a disponibilità elevata per soddisfare le richieste client in ingresso. La disponibilità elevata offre una scala\-architettura in cui è possibile aumentare la capacità del server aggiungendo altri server. I singoli punti di errore sono ridotti grazie al failover del cluster automatico.  
  
È possibile ottenere la disponibilità elevata utilizzando il carico di rete\-servizi di bilanciamento del carico e failover che forniscono le tecnologie di clustering di SQL. Per altre informazioni su come configurare SQL Server per la disponibilità elevata, vedere [Panoramica delle soluzioni a disponibilità elevata](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Risoluzione artefatto SAML  
Security Assertion Markup Language \(SAML\) risoluzione artefatto è un endpoint basato sulla parte il protocollo SAML 2.0 che descrive come relying party può recuperare un token direttamente da un provider di attestazioni. Nella prima fase del processo di risoluzione, un browser client contatta un server federativo di risorsa e fornisce un artefatto. Nella seconda fase i server federativi di risorsa inviano l'artefatto all'URL di un endpoint artefatto SAML ospitato nell'organizzazione di un partner account per risolvere il messaggio dell'artefatto. Nella fase finale il server federativo di account rilascia il token al server federativo per conto del browser client.  
  
> [!NOTE]  
> Se si è un amministratore di un'organizzazione partner account, assicurarsi di assegnare o associare un certificato SSL, che si concatena a un certificato radice di un membro del programma Windows root certificate, al sito Web federativo passivo in IIS \(<ComputerName>\\siti\\sito Web predefinito\\ADFS\\LS\) in tutti i server federativi di account nella farm. Questa configurazione è importante per evitare che sui server federativi di risorsa sia necessario aggiungere manualmente il certificato SSL all'archivio certificati Persone attendibili dei computer locali o che non sia possibile risolvere l'artefatto pubblicato nell'organizzazione.  
  
### <a name="samlws---federation-token-replay-detection"></a>Rilevamento riproduzione token SAML/WS-Federation  
Il termine *riproduzione token* si riferisce all'atto con cui un client browser nell'organizzazione di un partner account prova ad inviare più volte lo stesso token ricevuto da un server federativo di account per autenticarsi con un server federativo di risorsa.  Questo atto si verifica quando un utente fa clic sul pulsante indietro del browser nel tentativo di inviare di **nuovo** la pagina di autenticazione.  
  
ADFS include una funzionalità detta *rilevamento riproduzione token* con la quale più richieste di token che usano lo stesso token possono essere rilevate e rimosse. Quando questa funzionalità è abilitata, il rilevamento riproduzione token protegge l'integrità delle richieste di autenticazione sia WS\-Federation passivo e il profilo WebSSO SAML, verificare che lo stesso token non è mai usato più volte. È consigliabile abilitare questa funzionalità nelle situazioni in cui la sicurezza è di importanza cruciale, ad esempio quando si usano chioschi multimediali.  
  
Nell'esempio di chiosco multimediale un utente può disconnettersi da tutti i siti Web e in seguito un utente malintenzionato può provare a usare la cronologia del browser per inviare di nuovo la pagina di autenticazione federativa caricata dall'utente precedente. Questa funzionalità mitiga questa preoccupazione archiviando informazioni aggiuntive su ogni autenticazione riuscita effettuata da un'organizzazione partner account per rilevare le successive riesecuzione del token e impedire la riuscita di più tentativi di autenticazione.  
  

