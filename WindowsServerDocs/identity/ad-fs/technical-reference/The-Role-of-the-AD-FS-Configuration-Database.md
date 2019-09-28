---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: Ruolo del database di configurazione di AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 22047a93ab67d3f21b3e2318fcce497feab8f996
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385578"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>Ruolo del database di configurazione di AD FS
Il database di configurazione AD FS archivia tutti i dati di configurazione che rappresentano una singola istanza di Active Directory Federation Services \(AD FS @ no__t-1 \(that è, il Servizio federativo @ no__t-3. Il database di configurazione di ADFS definisce il set di parametri richiesti dal Servizio federativo per identificare partner, certificati, archivi attributi, attestazioni e diversi dati relativi a queste entità associate. È possibile archiviare i dati di configurazione in un database di® Microsoft SQL Server o nella funzionalità database interno di Windows \(WID @ no__t-1 inclusa in Windows Server® 2008, Windows Server 2008 R2 e Windows Server® 2012.  
  
> [!NOTE]  
> L'intero contenuto del database di configurazione di ADFS può essere archiviato in un'istanza di Database interno di Windows o in un'istanza del database SQL, ma non in entrambi. Ciò significa che non si possono avere alcuni server federativi che usano Database interno di Windows e altri che usano un database SQL Server per la stessa istanza del database di configurazione di ADFS.  
  
Per scoprire i vantaggi e gli svantaggi della scelta di Database interno di Windows o di SQL Server per archiviare il database di configurazione di ADFS, è possibile usare le informazioni in questo argomento insieme al contenuto disponibile in [Considerazioni sulla topologia di distribuzione di ADFS](https://technet.microsoft.com/library/gg982489.aspx) :  
  
Database interno di Windows utilizza un archivio dati relazionale e non dispone di un'interfaccia utente gestione \(dell'interfaccia Utente\). Gli amministratori possono invece modificare il contenuto del database di configurazione AD FS usando i cmdlet di gestione AD FS snap @ no__t-0cm, Fsconfig. exe o Windows PowerShell™.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Uso di Database interno di Windows per archiviare il database di configurazione di ADFS  
È possibile creare il database di configurazione AD FS usando WID come archivio usando lo strumento Fsconfig. exe Command @ no__t-0line o la configurazione guidata server federativo di AD FS. Quando si usa uno di questi strumenti, è possibile scegliere una qualsiasi delle opzioni seguenti per creare la topologia del server federativo. Ognuna di queste opzioni usa Database interno di Windows per archiviare il database di configurazione di ADFS:  
  
-   Creare un supporto\-server federativo da solo  
  
-   Creare il primo server federativo in una server farm federativa  
  
-   Aggiungere un server federativo a una server farm federativa  
  
Se si seleziona l'opzione stand @ no__t-0alone, viene utilizzato WID per archiviare una singola istanza del database di configurazione AD FS. Questa istanza non può essere condivisa tra più server federativi. È destinato solo agli ambienti di testing. Per ulteriori informazioni al supporto\-da solo l'opzione server federativo o come configurarne uno, vedere [autonomo Federation Server tramite WID](https://technet.microsoft.com/library/gg982486.aspx) o [creare un Server federativo autonomo](https://technet.microsoft.com/library/ee913579.aspx).  
  
Se si seleziona il primo server federativo nell'opzione per una server farm federativa, Database interno di Windows viene configurato ai fini della scalabilità per poter aggiungere altri server federativi alla farm in un secondo momento. Per altre informazioni sulla distribuzione di una farm con Database interno di Windows o su come configurarne una, vedere [Server farm federativa che usa Database interno di Windows](https://technet.microsoft.com/library/gg982492.aspx) o [Creare il primo server federativo in una server farm federativa.](https://technet.microsoft.com/library/dd807070.aspx)  
  
Se si sceglie l'opzione per aggiungere un server federativo, Database interno di Windows viene configurato per replicare al nuovo server federativo le modifiche fatte al database di configurazione a intervalli prestabiliti. Per ulteriori informazioni sull'aggiunta di un server federativo a una farm database interno di Windows, vedere [Federation Server Farm utilizzando WID](https://technet.microsoft.com/library/gg982492.aspx) o [aggiungere un Server federativo a una Server Farm federativa](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Quando si distribuisce una server farm federativa tramite WID, alcune funzionalità di ADFS non siano disponibili. Per avere accesso all'intero set di funzionalità quando si configura la server farm, considerare la possibilità di usare invece Microsoft SQL Server per archiviare il database di configurazione di ADFS. Per altre informazioni, vedere [Considerazioni sulla topologia di distribuzione di ADFS](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Funzionamento di una server farm federativa con Database interno di Windows  
In questa sezione sono disponibili concetti importanti che descrivono in che modo la server farm federativa che usa Database interno di Windows replica i dati tra un server federativo primario e i server federativi secondari. .  
  
#### <a name="primary-federation-server"></a>Server federativo primario  
Un server federativo primario è un computer che esegue Windows Server 2008, Windows Server 2008 R2 o Windows Server® 2012 che è stato configurato nel ruolo server federativo con la configurazione guidata server federativo di AD FS e che dispone di una copia di lettura/scrittura del AD FS database di configurazione. Il server federativo primario viene sempre creato quando si utilizza configurazione guidata Server federativo AD ADFS e selezionare l'opzione per creare un nuovo servizio federativo e rendere il computer del primo server federativo nella farm. Tutti gli altri server federativi della farm, detti anche server federativi secondari, devono sincronizzare le modifiche fatte sul server federativo primario in una copia del database di configurazione di ADFS archiviato localmente.  
  
#### <a name="secondary-federation-servers"></a>Server federativi secondari  
I server federativi secondari archiviano una copia del database di configurazione AD FS dal server federativo primario, ma queste copie sono lette @ no__t-0only. I server federativi secondari si connettono al server federativo primario nella farm e sincronizzano i dati, eseguendo il polling a intervalli regolari per verificare se i dati sono stati modificati. I server federativi secondari disponibili per fornire tolleranza di errore per il server federativo primario quando agisce per caricare\-bilanciare le richieste di accesso che vengono inviate ai diversi siti in tutto l'ambiente di rete.  
  
> [!NOTE]  
> Se un server federativo primario si arresta in modo anomalo ed è offline, tutti i server federativi secondari continuano a elaborare le richieste normalmente. Tuttavia, nessuna modifica può essere apportata al servizio federativo finché il server federativo primario non ritorna online. È anche possibile usare Windows PowerShell per nominare un server federativo secondario come server federativo primario. Per ulteriori informazioni, vedere l' [ad FS amministrazione con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>Sincronizzazione del database di configurazione di ADFS  
A causa del ruolo importante svolto dal database di configurazione AD FS, viene reso disponibile in tutti i server federativi della rete per fornire la tolleranza di errore e caricare le funzionalità @ no__t-0balancing quando si elaborano le richieste @no__t-carico di rete 1when @ No_ _T-2balancers vengono usati @ no__t-3. Tuttavia, per fare in modo che i server federativi secondari sfruttino questa funzionalità, è necessario che il database di configurazione di ADFS archiviato nel server federativo primario sia sincronizzato.  
  
Quando si aggiunge un server federativo alla farm, il nuovo computer che diventerà un server federativo secondario si connette al server federativo primario per replicare la copia del database di configurazione di ADFS. Da questo momento, il nuovo server federativo continua a eseguire il pull degli aggiornamenti dal server federativo primario a intervalli regolari, come illustrato nella figura seguente.  
  
![Ruoli di AD FS](media/adfs2_WID.png)  
  
Ogni server federativo secondario esegue il polling al server federativo primario ogni cinque minuti per rilevare le modifiche. È possibile modificare questo valore predefinito di cinque @ no__t-0minute o forzare una sincronizzazione immediata in qualsiasi momento usando un cmdlet di Windows PowerShell. Per ulteriori informazioni su come eseguire questa operazione, vedere [amministrazione di ADFS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
Anche il processo di sincronizzazione di Database interno di Windows supporta i trasferimenti incrementali per garantire trasferimenti più efficienti delle modifiche intermedie. Il processo di trasferimento incrementale richiede in sostanza meno traffico sulla rete e i trasferimenti sono completati più rapidamente.  
  
> [!NOTE]  
> È supportata la migrazione da un database di configurazione di ADFS da Database interno di Windows a un'istanza di SQL Server. Per ulteriori informazioni su come eseguire questa operazione, vedere [AD FS: Eseguire la migrazione del database di configurazione di AD FS a SQL Server @ no__t-0 nel sito wiki di TechNet.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Uso di SQL Server per archiviare il database di configurazione di ADFS  
È possibile creare il database di configurazione AD FS usando una singola istanza di SQL Server database come archivio usando lo strumento Fsconfig. exe Command @ no__t-0line. L'uso di un database SQL Server come database di configurazione di ADFS offre i seguenti vantaggi rispetto a Database interno di Windows:  
  
-   Consente agli amministratori di sfruttare le funzionalità a disponibilità elevata di SQL Server.  
  
-   Offre un ulteriore miglioramento delle prestazioni in presenza di traffico elevato.  
  
-   Fornisce supporto della funzionalità di risoluzione degli artefatti SAML e SAML/WS @ no__t-0Federation per il rilevamento della riproduzione dei token \(described sotto @ no__t-2.  
  
Il termine "server federativo primario" non si applica quando il database di configurazione di ADFS è archiviato in un'istanza del database SQL, perché tutti i server federativi possono leggere e scrivere nel database di configurazione di ADFS che usa la stessa istanza in cluster di SQL Server, come illustrato nella figura seguente.  
  
![Ruoli di AD FS](media/adfs2_SQL.png)  
  
È possibile utilizzare SQL Server per configurare due o più server in modo che funzionino insieme come cluster di server per garantire che AD FS sia reso a disponibilità elevata per soddisfare le richieste client in ingresso. La disponibilità elevata offre una scala\-architettura in cui è possibile aumentare la capacità del server aggiungendo altri server. I singoli punti di errore sono ridotti grazie al failover del cluster automatico.  
  
È possibile ottenere la disponibilità elevata utilizzando il carico di rete\-servizi di bilanciamento del carico e failover che forniscono le tecnologie di clustering di SQL. Per altre informazioni su come configurare SQL Server per la disponibilità elevata, vedere [Panoramica delle soluzioni a disponibilità elevata](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Risoluzione artefatto SAML  
Security Assertion Markup Language \(SAML\) risoluzione artefatto è un endpoint basato sulla parte il protocollo SAML 2.0 che descrive come relying party può recuperare un token direttamente da un provider di attestazioni. Nella prima fase del processo di risoluzione, un browser client contatta un server federativo di risorsa e fornisce un artefatto. Nella seconda fase i server federativi di risorsa inviano l'artefatto all'URL di un endpoint artefatto SAML ospitato nell'organizzazione di un partner account per risolvere il messaggio dell'artefatto. Nella fase finale il server federativo di account rilascia il token al server federativo per conto del browser client.  
  
> [!NOTE]  
> Se si è un amministratore di un'organizzazione partner account, assicurarsi di assegnare o associare un certificato SSL, che si concatena a un certificato radice di un membro del programma Windows root certificate, al sito Web federativo passivo in IIS \( @ no__t-1 @ no__ t-2Sites @ no__t-3Default Web site @ no__t-4adfs @ no__t-5LS @ no__t-6 in tutti i server federativi di account nella farm. Questa configurazione è importante per evitare che sui server federativi di risorsa sia necessario aggiungere manualmente il certificato SSL all'archivio certificati Persone attendibili dei computer locali o che non sia possibile risolvere l'artefatto pubblicato nell'organizzazione.  
  
### <a name="samlws---federation-token-replay-detection"></a>Rilevamento riproduzione token SAML/WS-Federation  
Il termine *riproduzione token* si riferisce all'atto con cui un client browser nell'organizzazione di un partner account prova ad inviare più volte lo stesso token ricevuto da un server federativo di account per autenticarsi con un server federativo di risorsa.  Questo atto avviene quando un utente fa clic sul pulsante **Indietro** nel browser per provare a inviare di nuovo la pagina di autenticazione.  
  
ADFS include una funzionalità detta *rilevamento riproduzione token* con la quale più richieste di token che usano lo stesso token possono essere rilevate e rimosse. Quando questa funzionalità è abilitata, il rilevamento riproduzione token protegge l'integrità delle richieste di autenticazione sia WS\-Federation passivo e il profilo WebSSO SAML, verificare che lo stesso token non è mai usato più volte. È consigliabile abilitare questa funzionalità nelle situazioni in cui la sicurezza è di importanza cruciale, ad esempio quando si usano chioschi multimediali.  
  
Nell'esempio di chiosco multimediale un utente può disconnettersi da tutti i siti Web e in seguito un utente malintenzionato può provare a usare la cronologia del browser per inviare di nuovo la pagina di autenticazione federativa caricata dall'utente precedente. Questa funzionalità limita il rischio, archiviando informazioni aggiuntive su ogni autenticazione completata correttamente da parte dell'organizzazione di un partner account, in modo da rilevare le successive riproduzioni del token e impedire che più tentativi di autenticazione abbiano esito positivo.  
  

