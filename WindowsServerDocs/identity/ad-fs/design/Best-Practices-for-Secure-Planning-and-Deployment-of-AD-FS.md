---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: Procedure consigliate per la pianificazione sicuro e distribuzione di ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ed8c36d4bec455879ffd00ad40b72fd5e90484ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Procedure consigliate per la pianificazione sicuro e distribuzione di ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento fornisce informazioni di procedure consigliate per la pianificazione e la valutazione di protezione quando si progetta la distribuzione di Active Directory Federation Services (ADFS). In questo argomento è un punto di partenza per rivedere e valutare le considerazioni relative alla sicurezza complessiva per l'utilizzo di ADFS. Le informazioni contenute in questo argomento sono progettate per complementari ed estendono la pianificazione della sicurezza esistente e altre procedure consigliate di progettazione.  
  
## <a name="core-security-best-practices-for-ad-fs"></a>Dei componenti di base procedure ottimali di protezione per AD FS  
Le seguenti procedure ottimali dei componenti di base sono comuni a tutte le installazioni di AD FS in cui si desidera migliorare o estendere la protezione della progettazione o della distribuzione:  
  
-   **Utilizzare la configurazione guidata impostazioni di sicurezza per applicare procedure ottimali di protezione di AD FS specifici per i server federativi e i computer proxy server federativo**  
  
    La configurazione guidata impostazioni di sicurezza è uno strumento preinstallato in tutti i Windows Server 2008, Windows Server 2008 R2 e i computer Windows Server 2012. È possibile usarlo per applicare la protezione consigliate che consentono di riducono la superficie di attacco per un server, in base ai ruoli server che si siano installando.  
  
    Quando si installa ADFS, il programma di installazione Crea ruolo file di estensione che è possibile utilizzare con la procedura guidata per creare un criterio di sicurezza che verrà applicate allo specifico ruolo server ADFS, il server federativo o proxy server federativo, scelto durante l'installazione.  
  
    Ogni file di estensione ruoli installato rappresenta il tipo di ruolo e ruolo secondario per cui è configurato ogni computer. I seguenti file di estensione ruolo vengono installati nella directory C:WindowsADFSScw:  
  
    -   Farm.Xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.XML (questo file è presente solo se il computer è configurato nel ruolo proxy server federativo).  
  
    Per applicare le estensioni del ruolo di AD FS in sicurezza, completare i passaggi seguenti nell'ordine:  
  
    1.  Installare ADFS e scegliere il ruolo del server appropriato per tale computer. Per ulteriori informazioni, vedere [installare il servizio ruolo Proxy servizio federativo](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) nella Guida alla distribuzione di ADFS.  
  
    2.  Registrare il file di estensione ruolo appropriata utilizzando lo strumento da riga di comando Scwcmd. Vedere la tabella seguente per informazioni dettagliate sull'uso di questo strumento nel ruolo per cui il computer è configurato.  
  
    3.  Verificare che il comando è stato completato correttamente esaminando il file SCWRegister_log.xml, che si trova nella directory WindowssecurityMsscwLogs.  
  
    È necessario eseguire tutte queste operazioni in ogni server federativo o un computer proxy server federativo a cui si desidera applicare la protezione della sicurezza di Active Directory basati su ADFS.  
  
    Nella tabella seguente viene illustrato come registrare l'estensione di ruolo di sicurezza appropriato, in base al ruolo server ADFS scelto nel computer in cui è installato ADFS.  
  
    |Ruolo del server ADFS|Database di configurazione di ADFS usato|Digitare il comando seguente al prompt dei comandi:|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |Server federativo autonomo|Database interno di Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |Server federativo aggiunto alla farm|Database interno di Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |Server federativo aggiunto alla farm|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |Proxy server federativo|N/D|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    Per ulteriori informazioni sui database che è possibile utilizzare con AD FS, vedere [il ruolo del Database di configurazione AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Utilizzare il rilevamento riproduzione token nelle situazioni in cui sicurezza è un aspetto molto importante, ad esempio, quando vengono usati chioschi multimediali.**  
    Il rilevamento riproduzione token è una funzionalità di ADFS che assicura che qualsiasi tentativo di riprodurre una richiesta di token fatta al servizio federativo viene rilevato e la richiesta viene eliminata. Il rilevamento riproduzione token è abilitato per impostazione predefinita. Funziona per il profilo passivo WS-Federation sia il profilo WebSSO sicurezza Assertion Markup Language (SAML) verificando che lo stesso token non è mai usato più volte.  
  
    Quando il servizio federativo viene avviato, inizia a compilare una cache di qualsiasi le richieste di token soddisfatte. Nel corso del tempo, le richieste di token successive vengono aggiunti alla cache, la capacità di rilevare qualsiasi tentativo di riprodurre una richiesta di token più volte cresce per il servizio federativo. Se si disabilita il rilevamento riproduzione token e in seguito si sceglie di riabilitarlo, tenere presente che il servizio federativo continuerà ad accettare token per un periodo di tempo che potrebbe essere stato usato in precedenza, finché la cache di riproduzione è stata autorizzata abbastanza tempo per ricostruire il relativo contenuto. Per ulteriori informazioni, vedere [il ruolo del Database di configurazione AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Usare la crittografia di token, specialmente se si utilizza con una risoluzione artefatto SAML.**  
  
    La crittografia di token è particolarmente consigliata per migliorare la sicurezza e protezione da attacchi potenziali attacchi man-in-the-middle (MITM) che potrebbero essere provato contro la distribuzione di ADFS. Uso della crittografia potrebbe avere un leggero impatto in tutta ma in generale, consigliabile non è di solito rilevabile e in molte distribuzioni i vantaggi per maggiore sicurezza superano qualsiasi costo in termini di prestazioni del server.  
  
    Per abilitare la crittografia di token, aggiungere prima di tutto un certificato di crittografia per il trust della relying party. È possibile configurare un certificato di crittografia quando si crea una relying party trust o versione successiva. Per aggiungere un certificato di crittografia in seguito a un trust della relying party esistente, è possibile impostare un certificato da utilizzare per il **crittografia** scheda all'interno delle proprietà di attendibilità utilizzando lo snap-in AD FS. Per specificare un certificato per un trust esistente con i cmdlet di ADFS, usare il parametro EncryptionCertificate di uno di **Set-ClaimsProviderTrust** o **Set-RelyingPartyTrust** cmdlet. Per impostare un certificato per il servizio federativo da usare per decrittografare i token, usare il **Set-ADFSCertificate** cmdlet e specificare "`Token-Encryption`" per il *CertificateType* parametro. Abilitazione e disabilitazione di crittografia per specifiche relying party trust può essere eseguita utilizzando il *EncryptClaims* parametro del **Set-RelyingPartyTrust** cmdlet.  
  
-   **Usare la protezione estesa per l'autenticazione**  
  
    Per proteggere le distribuzioni, puoi impostare e usare la protezione estesa per le funzionalità di autenticazione con ADFS. Questa impostazione specifica il livello di protezione estesa per l'autenticazione supportato da un server federativo.  
  
    La protezione estesa per l'autenticazione consente di proteggere da attacchi man-in-the-middle (MITM), in cui un utente malintenzionato intercetta le credenziali del client e le inoltra a un server. Protezione da tali attacchi è reso possibile tramite un canale Binding Token (CBT) che può essere sia richiesto, consentito o non richiesto dal server quando stabilisce le comunicazioni con i client.  
  
    Per abilitare la funzionalità di protezione estesa, usare il **ExtendedProtectionTokenCheck** parametro il **Set-ADFSProperties** cmdlet. I valori possibili per questa impostazione e il livello di sicurezza che forniscono i valori sono descritti nella tabella seguente.  
  
    |Valore del parametro|Livello di sicurezza|Impostazione di protezione|  
    |-------------------|------------------|----------------------|  
    |Richiedono|Il server è completamente protetto.|La protezione estesa è applicata e sempre richiesto.|  
    |Consenti|Il server è parzialmente protetto.|Protezione estesa è applicata in sistemi interessati se sono stati configurati per supportarla.|  
    |Nessuno|Il server è vulnerabile.|Non viene applicata la protezione estesa.|  
  
-   **Se si utilizza la registrazione e traccia, garantire la riservatezza delle informazioni riservate.**  
  
    ADFS non, per impostazione predefinita, espone né tiene traccia informazioni personali (PII) direttamente come parte del servizio federativo o delle normali operazioni. Quando la registrazione degli eventi e la registrazione di traccia di debug sono abilitate in AD FS, tuttavia, a seconda dei criteri di attestazioni di configurare alcuni attestazioni tipi e i relativi valori associati potrebbero contenere informazioni personali che potrebbero essere registrate nell'evento di ADFS o i registri di traccia.  
  
    Di conseguenza, imporre il controllo degli accessi sulla configurazione di AD FS e i relativi file di log è fortemente consigliato. Se non vuoi questo tipo di informazioni sia visibile, è necessario disattivare eliminandola dai, o filtrare qualsiasi informazione personale o dati sensibili nei log prima di condividerli con altri utenti.  
  
    I suggerimenti seguenti consentono di impedire che il contenuto di un file di log sia esposto accidentalmente:  
  
    -   Assicurarsi che il registro eventi di ADFS e i file di log di traccia sono protetti da elenchi di controllo di accesso (ACL) che limitano l'accesso a solo gli amministratori attendibili che richiedono l'accesso.  
  
    -   Non copiare o archiviare file di log usando estensioni di file o percorsi che possono essere facilmente serviti usando una richiesta Web. Ad esempio, l'estensione. XML non è una scelta sicura. È possibile controllare la Guida all'amministrazione di Internet Information Services (IIS) per visualizzare un elenco di estensioni che possono essere serviti.  
  
    -   Se si modifica il percorso del file di registro, assicurarsi di specificare un percorso assoluto per il percorso del file di registro, che dovrebbe trovarsi all'esterno della directory pubblica di directory principale virtuale (radice virtuale) host Web per impedire che venga utilizzata da una parte esterna con un Web browser.  

-   **Protezione il blocco della Extranet di AD FS**  
    
    In caso di un attacco sotto forma di richieste di autenticazione con password invalid(bad) fornite tramite il Proxy dell'applicazione Web, blocco della extranet di AD FS consente di impedire agli utenti di un blocco degli account di AD FS. Oltre a proteggere gli utenti da un'istanza di ADFS di blocco account, blocco della extranet di AD FS protegge anche da attacchi di individuazione delle password, attacchi di forza bruta.  Per ulteriori informazioni vedere [protezione blocco Extranet di AD FS](../../ad-fs/operations/Configure-AD-FS-Extranet-Lockout-Protection.md).  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>SQL Server specifico procedure ottimali di protezione per AD FS  
Le seguenti procedure ottimali di protezione sono specifiche per l'utilizzo di Microsoft SQL Server® o la Database interno di Windows (WID) quando queste tecnologie database vengono usate per gestire i dati nella progettazione di ADFS e la distribuzione.  
  
> [!NOTE]  
> Questi suggerimenti sono concepiti per estendere, ma non sostituiscono, Guida alla protezione del prodotto SQL Server. Per ulteriori informazioni sulla pianificazione di un'installazione di SQL Server protetta, vedere [considerazioni sulla sicurezza per un'installazione sicura di SQL](https://go.microsoft.com/fwlink/?LinkID=139831) (https://go.microsoft.com/fwlink/?LinkID=139831).  
  
-   **Distribuire sempre SQL Server protetto da un firewall in un ambiente di rete fisicamente sicuro.**  
  
    Un'installazione di SQL Server non deve mai essere esposta direttamente a Internet. Solo i computer all'interno del Data Center devono essere in grado di raggiungere l'installazione di SQL server che supporta AD FS. Per ulteriori informazioni, vedere [elenco di controllo di sicurezza Best Practices](https://go.microsoft.com/fwlink/?LinkID=189229) (https://go.microsoft.com/fwlink/?LinkID=189229).  
  
-   **Eseguire SQL Server con un account di servizio invece di usare gli account del servizio di sistema predefiniti incorporati.**  
  
    Per impostazione predefinita, SQL Server è spesso installato e configurato per utilizzare uno degli account di sistema predefiniti supportati, ad esempio l'account LocalSystem o NetworkService. Per migliorare la sicurezza dell'installazione di SQL Server per AD FS, ovunque possibile utilizzare un account del servizio distinto per l'accesso al servizio SQL Server e abilitare l'autenticazione Kerberos registrando il nome dell'entità (SPN) di sicurezza dell'account nella distribuzione di Active Directory. In questo modo l'autenticazione reciproca tra client e server. Senza registrazione del SPN di un account del servizio distinto, SQL Server verrà utilizzata l'autenticazione basata su NTLM per Windows, in cui solo il client viene autenticato.  
  
-   **Ridurre al minimo la superficie di attacco di SQL Server.**  
  
    Abilitare solo gli endpoint di SQL Server che sono necessari. Per impostazione predefinita, SQL Server fornisce un singolo endpoint TCP predefinito che non possono essere rimossi. Per ADFS, è necessario abilitare questo endpoint TCP per l'autenticazione Kerberos. Per esaminare gli endpoint TCP correnti per vedere se altre porte TCP definite dall'utente vengono aggiunti a un'installazione di SQL, è possibile utilizzare il "Seleziona * FROM Sys. tcp_endpoints" query istruzione in una sessione Transact-SQL (T-SQL). Per ulteriori informazioni sulla configurazione di endpoint di SQL Server, vedere [How To: configurare il motore di Database per l'ascolto su più porte TCP](https://go.microsoft.com/fwlink/?LinkID=189231) (https://go.microsoft.com/fwlink/?LinkID=189231).  
  
-   **Evita di usare l'autenticazione basata su SQL.**  
  
    Per evitare di dover trasferire password come testo non crittografato attraverso la rete o archiviare password nelle impostazioni di configurazione, utilizzare l'autenticazione di Windows solo con l'installazione di SQL Server. Autenticazione di SQL Server è una modalità di autenticazione legacy. Memorizzazione delle credenziali di accesso Structured Query Language (SQL) (nome utente SQL e password) quando si utilizza l'autenticazione SQL Server non è consigliata. Per ulteriori informazioni, vedere [le modalità di autenticazione](https://go.microsoft.com/fwlink/?LinkID=189232) (https://go.microsoft.com/fwlink/?LinkID=189232).  
  
-   **Valutare con attenzione la necessità di sicurezza del canale aggiuntivo nell'installazione di SQL.**  
  
    Anche con l'autenticazione Kerberos in effetti, SQL Server Security Support Provider Interface (SSPI) non fornisce protezione a livello di canale. Tuttavia, per le installazioni in cui i server sono collocati in una rete protetta da firewall, la crittografia delle comunicazioni SQL potrebbe non essere necessaria.  
  
    Anche se la crittografia è uno strumento valido ai fini della sicurezza, non deve essere considerato per tutte le connessioni o dati. Quando si decide se implementare la crittografia, prendere in considerazione come gli utenti potranno accedere dati. Se gli utenti accedono ai dati attraverso una rete pubblica, la crittografia dei dati potrebbe essere necessario aumentare la sicurezza. Tuttavia, se tutti gli accessi di dati SQL da AD FS comporta una configurazione sicura della rete intranet, la crittografia potrebbe non essere necessaria. Qualsiasi uso della crittografia deve includere anche una strategia di manutenzione per password, chiavi e certificati.  
  
    Se esiste un problema che i dati SQL potrebbero essere visualizzati o manomessi attraverso la rete, usano Internet Protocol security (IPsec) o sicuro Sockets Layer (SSL) per migliorare la sicurezza delle connessioni SQL. Tuttavia, questo potrebbe essere un effetto negativo sulle prestazioni di SQL Server, che potrebbero influire sulla o limitare le prestazioni di AD FS in alcune situazioni. Ad esempio, le prestazioni di AD FS rilascio dei token potrebbero peggiorare nel caso quando le ricerche di attributi da un archivio attributi basato su SQL siano cruciali per il rilascio di token. È possibile eliminare meglio SQL minaccia di manomissione da parte di una configurazione di sicurezza del perimetro avanzata. Ad esempio, una soluzione migliore per la protezione di installazione di SQL Server è per assicurare che rimanga inaccessibile per gli utenti di Internet e i computer e che rimane accessibile solo da utenti o computer all'interno dell'ambiente di datacenter.  
  
    Per ulteriori informazioni, vedere [crittografia delle connessioni a SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) o [crittografia di SQL Server](https://go.microsoft.com/fwlink/?LinkID=189233).  
  
-   **Configurare l'accesso progettato in modo sicuro usando stored procedure per eseguire ricerche basato su SQL dai dati di Active Directory archiviati in ADFS di SQL.**  
  
    Per fornire un servizio migliore e isolamento di dati, è possibile creare stored procedure per tutti i comandi di ricerca di archivio di attributi. È possibile creare un ruolo del database a cui concedere poi l'autorizzazione per eseguire le stored procedure. Assegnare l'identità del servizio Windows ADFS per questo ruolo del database. Il servizio Windows ADFS non deve essere in grado di eseguire qualsiasi altre istruzioni SQL oltre alle stored procedure appropriate che vengono utilizzati per la ricerca di attributi. Il blocco dell'accesso al database di SQL Server in questo modo riduce il rischio di un attacco di elevazione dei privilegi.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
