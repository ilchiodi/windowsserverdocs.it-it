---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: Il ruolo del Database di configurazione di ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3372e1f051ba7f900753a4961d948ddabdef6f4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-ad-fs-configuration-database"></a>Il ruolo del Database di configurazione di ADFS
Il database di configurazione di ADFS archivia tutti i dati di configurazione che rappresenta una singola istanza di Active Directory Federation Services \(AD FS\) \(that is, the Federation Service\). Il database di configurazione di ADFS definisce il set di parametri richiesti dal servizio federativo per identificare partner, certificati, archivi di attributi, attestazioni e diversi dati relativi a queste entità associate. È possibile archiviare questi dati di configurazione in un database Microsoft SQL Server® o la funzionalità Database interno di Windows \(WID\) incluso in Windows Server® 2008, Windows Server 2008 R2 e Windows Server® 2012.  
  
> [!NOTE]  
> L'intero contenuto del database di configurazione di ADFS può essere archiviato in un'istanza di database interno di Windows o in un'istanza del database SQL, ma non entrambi. Ciò significa che è non può avere alcuni server federativi che usano database interno di Windows e altri che usano un database SQL Server per la stessa istanza del database di configurazione di ADFS.  
  
È possibile utilizzare le seguenti informazioni in questo argomento insieme al contenuto disponibile [considerazioni sulla topologia di distribuzione ADFS](https://technet.microsoft.com/library/gg982489.aspx) per scoprire i vantaggi e svantaggi della scelta WID o SQL Server per archiviare il database di configurazione di ADFS:  
  
Database interno di Windows utilizza un dati relazionali, archiviare e non dispone di un proprio utente management interfaccia \(UI\). Al contrario, gli amministratori possono modificare il contenuto del database di configurazione di ADFS tramite Gestione snap-in ADFS, Fsconfig.exe o Windows PowerShell™ cmdlet.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Usa database interno di Windows per archiviare il database di configurazione di ADFS  
È possibile creare il database di configurazione di ADFS utilizza WID come archivio utilizzando lo strumento della riga di comando Fsconfig.exe o configurazione guidata Server federativo AD FS. Quando si usa uno di questi strumenti, è possibile scegliere una delle opzioni seguenti per creare la topologia di server federativo. Ognuna di queste opzioni Usa database interno di Windows per archiviare il database di configurazione di ADFS:  
  
-   Creare un server federativo autonoma  
  
-   Creare il primo server federativo in una server farm federativa  
  
-   Aggiungere un server federativo a una server farm federativa  
  
Se si seleziona l'opzione autonoma, database interno di Windows viene utilizzato per archiviare una singola istanza del database di configurazione di ADFS. Questa istanza non può essere condivisi tra più server federativi. È destinato agli ambienti di testing solo. Per ulteriori informazioni sull'opzione del server federativo autonoma o come configurarne uno, vedere [autonomo Federation Server tramite WID](https://technet.microsoft.com/library/gg982486.aspx) o [creare un Server federativo autonomo](https://technet.microsoft.com/library/ee913579.aspx).  
  
Se si seleziona il primo server federativo in un'opzione di farm di server federativo, database interno di Windows è configurato per la scalabilità che consenta di altri server federativi per essere aggiunto alla farm in un secondo momento. Per ulteriori informazioni sulla distribuzione di una farm database interno di Windows o come configurarne uno, vedere [Federation Server Farm utilizzando WID](https://technet.microsoft.com/library/gg982492.aspx) o [creare il primo Server federativo in una Server Farm federativa](https://technet.microsoft.com/library/dd807070.aspx)  
  
Se si seleziona l'opzione Aggiungi una federazione server, database interno di Windows è configurato per replicare le modifiche di database di configurazione per il nuovo server federativo a intervalli prestabiliti. Per ulteriori informazioni sull'aggiunta di un server federativo a una farm database interno di Windows, vedere [Federation Server Farm utilizzando WID](https://technet.microsoft.com/library/gg982492.aspx) o [aggiunge un Server federativo a una Server Farm federativa](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Quando si distribuisce una server farm federativa con database interno di Windows, alcune funzionalità di AD FS potrebbero non essere disponibili. Per accedere alla funzionalità completa impostato quando si configura la server farm, è consigliabile utilizzare Microsoft SQL Server per archiviare il database di configurazione di ADFS invece. Per ulteriori informazioni, vedere [considerazioni sulla topologia di distribuzione ADFS](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Funzionamento di una server farm federativa WID  
Questa sezione descrive i concetti importanti che descrivono come la server farm federativa WID replica i dati tra un server federativo primario e i server federativi secondari. .  
  
#### <a name="primary-federation-server"></a>Server federativo primario  
Un server federativo primario è un computer che esegue Windows Server 2008, Windows Server 2008 R2 o Windows Server® 2012 che è stato configurato nel ruolo del server federativo con configurazione guidata Server federativo AD FS e che include una copia in lettura/scrittura del database di configurazione di ADFS. Il server federativo primario viene sempre creato quando si utilizza configurazione guidata Server federativo AD FS e selezionare l'opzione per creare un nuovo servizio federativo e rendere il computer come primo server federativo nella farm. Tutti gli altri server federativi della farm, noto anche come server federativi secondari, è necessario sincronizzare le modifiche apportate nel server federativo primario in una copia del database di configurazione di ADFS archiviato localmente.  
  
#### <a name="secondary-federation-servers"></a>Server federativi secondari  
Server federativi secondari archiviano una copia del database di configurazione di ADFS dal server federativo primario, ma queste copie sono di sola lettura. I server federativi secondari connettono e i dati sincronizzano con il server federativo primario della farm eseguendo il polling a intervalli regolari per verificare se i dati sono stati modificati. I server federativi secondari disponibili per fornire la tolleranza di errore per il server federativo primario e per le richieste di accesso Load saldo eseguite in siti diversi in tutto l'ambiente di rete.  
  
> [!NOTE]  
> Se un server federativo primario si blocca e non è in linea, tutti i server federativi secondari continuano a elaborare le richieste normalmente. Tuttavia, non nuovo possono essere apportate modifiche al servizio federativo fino a quando il server federativo primario è stato portato in linea. È inoltre possibile designare un server federativo secondario diventi il server federativo primario utilizzando Windows PowerShell. Per ulteriori informazioni, vedere il [amministrazione di ADFS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-ad-fs-configuration-database-is-synchronized"></a>Modalità di sincronizzazione dei database di configurazione di ADFS  
A causa dell'importante ruolo svolto dal database di configurazione di ADFS, renderlo disponibile in tutti i server federativi nella rete per fornire tolleranza di errore e bilanciamento del carico Load funzionalità durante l'elaborazione delle richieste \ (quando network Load servizi di bilanciamento del sono used\). Tuttavia, per i server federativi secondari sfruttino questa funzionalità, è necessario sincronizzare il database di configurazione di ADFS archiviato nel server federativo primario.  
  
Quando si aggiunge un server federativo alla farm, il nuovo computer che diventerà un server federativo secondario si connette al server federativo primario per replicare la copia del database di configurazione di ADFS. Da questo punto in poi, il nuovo server federativo continua a scaricare gli aggiornamenti dal server federativo primario a intervalli regolari, come illustrato nella figura seguente.  
  
![Ruoli di AD FS](media/adfs2_WID.png)  
  
Ogni server federativo secondario esegue il polling del server federativo primario ogni cinque minuti per le modifiche. È possibile modificare questo valore five\ minuti predefinito o forzare una sincronizzazione immediata in qualsiasi momento utilizzando i cmdlet di Windows PowerShell. Per ulteriori informazioni su come eseguire questa operazione, vedere [amministrazione di ADFS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
Il processo di sincronizzazione di database interno di Windows supporta anche i trasferimenti incrementali per garantire trasferimenti più efficienti delle modifiche intermedie. Il processo di trasferimento incrementale richiede sostanza meno traffico sulla rete e i trasferimenti sono completati più rapidamente.  
  
> [!NOTE]  
> È supportata la migrazione di un database di configurazione di ADFS da database interno di Windows a un'istanza di SQL Server. Per ulteriori informazioni su come eseguire questa operazione, vedere [ADFS: migrazione di Database di configurazione di ADFS a SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) nel sito TechNet Wiki.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Utilizzando SQL Server per archiviare il database di configurazione di ADFS  
È possibile creare il database di configurazione di ADFS usando una singola istanza di database di SQL Server come archivio utilizzando lo strumento della riga di comando Fsconfig.exe. Utilizzo di un database di SQL Server come database di configurazione di ADFS offre i seguenti vantaggi tramite WID:  
  
-   Gli amministratori possono sfruttare le funzionalità di disponibilità elevata di SQL Server  
  
-   Fornisce ulteriore miglioramento delle prestazioni per il traffico elevato.  
  
-   Fornisce supporto per funzionalità di risoluzione artefatto SAML e SAML/WS-Federation riproduzione token rilevamento \(described below\).  
  
Il termine "server federativo primario" non si applica quando il database di configurazione di ADFS è archiviato in un'istanza di database SQL, perché tutti i server federativi possono leggere e scrivere nel database di configurazione di ADFS che utilizza la stessa istanza di SQL Server in cluster, come illustrato nella figura seguente.  
  
![Ruoli di AD FS](media/adfs2_SQL.png)  
  
È possibile utilizzare SQL Server per configurare due o più server di lavorare insieme come un cluster di server per garantire una disponibilità elevata per le richieste client in ingresso del servizio ADFS. La disponibilità elevata offre un'architettura scale\-out in cui è possibile aumentare la capacità del server aggiungendo altri server. Singoli punti di errore sono ridotti grazie al failover del cluster automatico.  
  
È possibile ottenere la disponibilità elevata tramite la rete Load-bilanciamento del carico e failover servizi offerti dalle tecnologie clustering SQL. Per ulteriori informazioni su come configurare SQL Server per la disponibilità elevata, vedere [panoramica delle soluzioni a disponibilità elevata](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Risoluzione artefatto SAML  
Risoluzione artefatto di Security Assertion Markup Language \(SAML\) è un endpoint basato sulla parte il protocollo SAML 2.0 che descrive come relying party può recuperare un token direttamente da un provider di attestazioni. Nella prima fase del processo di risoluzione, un browser client contatta un server federativo di risorsa e fornisce un artefatto. Nella seconda fase, i server federativi di risorsa inviano l'artefatto a un URL dell'endpoint artefatto SAML ospitato in un'organizzazione partner account per risolvere il messaggio dell'artefatto. Nella fase finale, il server federativo di account rilascia il token al server federativo per conto del browser client.  
  
> [!NOTE]  
> Se sei un amministratore in un'organizzazione partner account, assicurarsi di assegnare o associare un certificato SSL, che è concatenato a un certificato radice di un membro del programma Windows Root Certificate, per il sito Web federativo passivo in IIS \ (<ComputerName>\\Sites\\Default Web Site\\adfs\\ls\) in tutti i server federativi di account nella farm. Questo è importante per evitare i server federativi di risorsa di dover aggiungere manualmente il certificato SSL nell'archivio certificati persone attendibili i computer locali o che non sia possibile risolvere l'artefatto pubblicato nell'organizzazione.  
  
### <a name="samlws---federation-token-replay-detection"></a>SAML/WS - rilevamento riproduzione token federazione  
Il termine *riproduzione token* si riferisce all'atto con cui un client browser in un'organizzazione partner account prova ad inviare lo stesso token ricevuto da un server federativo di account più volte per l'autenticazione a un server federativo di risorsa.  Questo atto avviene quando un utente fa clic il **Indietro** pulsante del browser nel tentativo di inviare di nuovo la pagina di autenticazione.  
  
ADFS offre una funzionalità detta *il rilevamento riproduzione token* da quale più richieste di token utilizzando lo stesso token può essere rilevata e rimosse. Quando questa funzionalità è abilitata, il rilevamento riproduzione token protegge l'integrità delle richieste di autenticazione il profilo passivo WS-Federation sia il profilo WebSSO SAML, assicurati che lo stesso token non è mai usato più volte. Questa funzionalità deve essere abilitata in situazioni in cui sicurezza è di importanza cruciale, ad esempio quando si usano chioschi multimediali.  
  
Nell'esempio di chiosco multimediale, un utente può disconnettersi da tutti i siti Web e in seguito un utente malintenzionato può tentare di usare la cronologia del browser per inviare di nuovo la pagina di autenticazione federativa caricata dall'utente precedente. Questa funzionalità limita il rischio, archiviando informazioni aggiuntive su ogni autenticazione completata correttamente da un'organizzazione partner account per rilevare le successive riproduzioni del token e impedire l'esito positivo più tentativi di autenticazione.  
  

