---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: Procedure consigliate per la pianificazione e la distribuzione sicure di ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: df1afc77afffd9b737965215a5c9d96f278c8129
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191674"
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Procedure consigliate per la pianificazione e la distribuzione sicure di ADFS


In questo argomento fornisce informazioni sulle procedure consigliate per facilitare la pianificazione e valutazione della sicurezza quando si progetta la distribuzione di Active Directory Federation Services (ADFS). In questo argomento è un punto di partenza per rivedere e valutare le considerazioni relative alla sicurezza complessiva per l'utilizzo di AD FS. Le informazioni in questo argomento sono complementari ed estendono la pianificazione della sicurezza esistente, nonché altre procedure consigliate per la progettazione.  
  
## <a name="core-security-best-practices-for-ad-fs"></a>Procedure consigliate per la sicurezza di base per ADFS  
Le procedure consigliate di base seguenti sono comuni a tutte le installazioni di AD FS in cui si desidera migliorare o estendere la sicurezza della progettazione o della distribuzione:  
  
-   **Utilizzare la configurazione guidata sicurezza da applicare AD FS specifiche di sicurezza consigliate per i server federativi e computer proxy server federativo**  
  
    La configurazione guidata impostazioni di sicurezza è uno strumento che viene fornito preinstallato in tutti i Windows Server 2008, Windows Server 2008 R2 e i computer Windows Server 2012. È possibile usarla per applicare le procedure consigliate per la sicurezza che contribuiscono a ridurre la superficie di attacco di un server in base ai ruoli del server installati.  
  
    Quando si installa ADFS, il programma di installazione crea file di estensione dei ruoli che si possono usare con la Configurazione guidata impostazioni di sicurezza per creare i criteri di sicurezza che saranno applicati al ruolo del server ADFS specifico, cioè server federativo o proxy server federativo, scelto durante l'installazione.  
  
    Ogni file di estensione dei ruoli installato rappresenta il tipo di ruolo e ruolo secondario per cui è configurato ogni computer. I seguenti file di estensione di ruolo vengono installati nella directory C:WindowsADFSScw:  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml (questo file è presente solo se per il computer è configurato il ruolo di proxy server federativo).  
  
    Per applicare le estensioni dei ruoli di ADFS nella Configurazione guidata impostazioni di sicurezza, completare i seguenti passaggi nell'ordine indicato:  
  
    1.  Installare ADFS e scegliere il ruolo del server appropriato per il computer. Per altre informazioni, vedere [installare il servizio ruolo Proxy servizio federativo](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) nella Guida alla distribuzione di AD FS.  
  
    2.  Registrare il file di estensione dei ruoli appropriato con lo strumento da riga di comando Scwcmd. Per dettagli sull'uso di questo strumento nel ruolo per cui è configurato il computer, vedere la tabella seguente.  
  
    3.  Verificare che il comando è stato completato correttamente, esaminare il file Scwregister_log, che si trova nella directory WindowssecurityMsscwLogs.  
  
    Tutti questi passaggi devono essere eseguiti su ogni computer server federativo o proxy server federativo a cui si vogliono applicare i criteri di sicurezza della Configurazione guidata impostazioni di sicurezza basati su ADFS.  
  
    Nella seguente tabella è spiegato come registrare l'estensione dei ruoli della Configurazione guidata impostazioni di sicurezza appropriata in base al ruolo del server ADFS scelto nel computer in cui è installato ADFS.  
  
    |Ruolo del server ADFS|Database di configurazione di ADFS usato|Al prompt dei comandi digitare il comando seguente:|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |Server federativo autonomo|Database interno di Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |Server federativo aggiunto alla farm|Database interno di Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |Server federativo aggiunto alla farm|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |Proxy server federativo|N/D|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    Per altre informazioni sui database che si possono usare con ADFS, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Usare il rilevamento riproduzione token nelle situazioni in cui sicurezza è un aspetto molto importante, ad esempio, quando vengono usati chioschi multimediali.**  
    Rilevamento riproduzione token è una funzionalità di AD FS che assicura che qualsiasi tentativo di riprodurre una richiesta di token fatta al servizio federativo viene rilevata e sia ignorata. Il rilevamento riproduzione token è abilitato per impostazione predefinita. Funziona sia per il profilo passivo WS-Federation sia per il profilo WebSSO SAML (Security Assertion Markup Language), assicurando che lo stesso token non venga mai usato più di una volta.  
  
    Quando il Servizio federativo viene avviato, inizia a compilare una cache di tutte le richieste di token soddisfatte. Poiché con il tempo vengono aggiunte alla cache richieste di token successive, per il Servizio federativo aumenta la capacità di rilevare qualsiasi tentativo di riprodurre più volte una richiesta di token. Se si disabilita il rilevamento riproduzione token e in seguito si sceglie di riabilitarlo, si ricordi che il Servizio federativo continuerà ad accettare token per un periodo di tempo che potrebbe essere stato usato in precedenza, finché la cache di riproduzione non avrà avuto abbastanza tempo per ricostruire il proprio contenuto. Per altre informazioni, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Usare la crittografia dei token, specialmente se si usa con una risoluzione artefatto SAML.**  
  
    Crittografia dei token è particolarmente consigliata per aumentare la sicurezza e protezione attacchi potenziali man-in-the-middle (MITM) che potrebbero essere eseguite sulla distribuzione di AD FS. L'uso della crittografia potrebbe avere un leggero impatto sulla velocità effettiva, ma in generale non è di solito rilevabile e in molte distribuzioni i vantaggi dati da una maggiore sicurezza superano qualsiasi costo in termini di prestazioni del server.  
  
    Per abilitare la crittografia di token, aggiungere prima di tutto un certificato di crittografia per i trust della relying party. È possibile configurare un certificato di crittografia quando si crea un trust della relying party o in seguito. Per aggiungere un certificato di crittografia in un secondo momento per un trust della relying party esistente, è possibile impostare un certificato per l'uso in di **crittografia** scheda all'interno delle proprietà di attendibilità quando si usa lo snap-in AD FS. Per specificare un certificato per un trust esistente con i cmdlet di AD FS, usare il parametro EncryptionCertificate di entrambi i **Set-ClaimsProviderTrust** oppure **Set-RelyingPartyTrust** cmdlet. Per impostare un certificato per il servizio federativo da usare per decrittografare i token, usare il **Set-ADFSCertificate** cmdlet e specificare "`Token-Encryption`" per il *CertificateType* parametro. Per abilitare e disabilitare la crittografia per il trust della relying party specifico, è possibile usare il parametro *EncryptClaims* del cmdlet **Set-RelyingPartyTrust**.  
  
-   **Usare la protezione estesa per l'autenticazione**  
  
    Per proteggere le distribuzioni, è possibile impostare e usare la protezione estesa per la funzionalità di autenticazione con AD FS. Questa impostazione specifica il livello di protezione estesa per l'autenticazione supportata da un server federativo.  
  
    La protezione estesa per l'autenticazione consente di contrastare gli attacchi man-in-the-middle in cui l'autore di un attacco intercetta le credenziali del client e le inoltra a un server. La protezione da tali attacchi è possibile grazie al token di binding che può essere richiesto, consentito o non richiesto dal server quando stabilisce le comunicazioni con i client.  
  
    Per abilitare la funzionalità di protezione estesa, usare il parametro **ExtendedProtectionTokenCheck** nel cmdlet **Set-ADFSProperties**. I valori possibili di questa impostazione e il livello di sicurezza fornito dai valori sono descritti nella tabella seguente.  
  
    |Valore del parametro|Livello Sicurezza|Impostazione di protezione|  
    |-------------------|------------------|----------------------|  
    |Richiedi|Il server è completamente protetto.|La protezione estesa è applicata e sempre richiesta.|  
    |Consenti|Il server è parzialmente protetto.|La protezione estesa è applicata ai sistemi interessati se sono stati configurati per supportarla.|  
    |Nessuno|Il server è vulnerabile.|La protezione estesa non è applicata.|  
  
-   **Se si usa la registrazione e traccia, garantire la privacy delle informazioni riservate.**  
  
    AD FS non, per impostazione predefinita, esporre o tenere traccia delle informazioni personali (PII) direttamente come parte del servizio federativo o del normale funzionamento. Quando la registrazione degli eventi e la registrazione di traccia di debug sono abilitate in AD FS, tuttavia, a seconda dei criteri delle attestazioni di configurare alcune attestazioni di tipi e i relativi valori associati potrebbero contenere informazioni personali che potrebbero essere registrate nell'evento di ADFS o i log di traccia.  
  
    Pertanto, l'applicazione di controllo di accesso nella configurazione di AD FS e i relativi file di log è consigliabile. Se non si vuole che questo tipo di informazioni sia visibile, disabilitare la registrazione o filtrare qualsiasi informazione personale o riservata eliminandola dai log prima di condividerli con altri.  
  
    I suggerimenti seguenti possono essere utili per evitare che il contenuto di un file di log sia esposto accidentalmente:  
  
    -   Assicurarsi che il registro eventi di ADFS e i file di log di traccia sono protetti da elenchi di controllo di accesso (ACL) che limitano l'accesso al solo amministratori attendibili che richiedono l'accesso ad essi.  
  
    -   Non copiare o archiviare file di log usando estensioni di file o percorsi che possono essere facilmente serviti usando una richiesta Web. Ad esempio, l'estensione di file XML non è una scelta sicura. Per un elenco di estensioni che possono essere servite, vedere la guida all'amministrazione di Internet Information Services (IIS).  
  
    -   Se si modifica il percorso del file di log, accertarsi di specificare un percorso assoluto che dovrebbe trovarsi all'esterno della directory radice virtuale pubblica dell'host Web, in modo da evitarne l'accesso da una parte esterna con un Web browser.  

-   **AD FS Extranet Lockout Soft e AD FS Extranet di protezione di blocco Smart**  
    
    In caso di un attacco sotto forma di richieste di autenticazione con password invalid(bad) sempre passare attraverso il Proxy applicazione Web, blocco della extranet di AD FS consente di proteggere gli utenti da un blocco degli account AD FS. Oltre a proteggere gli utenti da un'istanza di ADFS di blocco degli account, blocco della extranet di AD FS anche protegge contro gli attacchi di individuazione delle password, attacchi di forza bruta.  
    
    Per il blocco della Extranet Soft per AD FS in Windows Server 2012 R2 vedere [AD FS Extranet Soft protezione di blocco](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md).  

     Per il blocco Smart Extranet per ADFS in Windows Server 2016, vedere [AD FS Smart protezione di blocco Extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md).  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>Procedure consigliate per la sicurezza di base specifiche di SQL Server per ADFS  
Le seguenti procedure ottimali di sicurezza sono specifiche per l'uso di Microsoft SQL Server® o la Database interno di Windows (WID) quando queste tecnologie di database vengono utilizzate per gestire i dati in Progettazione di ADFS e la distribuzione.  
  
> [!NOTE]  
> Questi suggerimenti costituiscono un'estensione della guida alla sicurezza del prodotto SQL Server, ma non la sostituiscono. Per altre informazioni sulla pianificazione di un'installazione di SQL Server protetta, vedere [considerazioni sulla sicurezza per un'installazione di SQL protette](https://go.microsoft.com/fwlink/?LinkID=139831) (https://go.microsoft.com/fwlink/?LinkID=139831).  
  
-   **Distribuire sempre SQL Server protetti da un firewall in un ambiente di rete fisicamente sicuro.**  
  
    Un'installazione di SQL Server non deve mai essere esposta direttamente a Internet. Solo i computer che si trovano all'interno del Data Center devono essere in grado di raggiungere l'installazione di SQL server che supporta AD FS. Per altre informazioni, vedere [Security Best Practices Checklist](https://go.microsoft.com/fwlink/?LinkID=189229) (https://go.microsoft.com/fwlink/?LinkID=189229).  
  
-   **Eseguire SQL Server con un account del servizio invece di usare gli account di servizio di sistema predefiniti incorporati.**  
  
    Per impostazione predefinita, SQL Server è spesso installato e configurato per l'uso di uno degli account di sistema predefiniti supportati, ad esempio LocalSystem o NetworkService. Per migliorare la sicurezza dell'installazione di SQL Server per AD FS, ovunque possibile usare un oggetto separato, account del servizio per l'accesso al servizio SQL Server e abilitare l'autenticazione Kerberos tramite la registrazione il nome dell'entità sicurezza (SPN) di questo account di Distribuzione di Active Directory. In questo modo si abilita l'autenticazione reciproca tra client e server. Senza la registrazione dell'SPN di un account del servizio distinto, SQL Server userà NTLM per l'autenticazione basata su Windows in cui solo il client viene autenticato.  
  
-   **Ridurre al minimo la superficie di SQL Server.**  
  
    Abilitare solo gli endpoint di SQL Server necessari. Per impostazione predefinita, SQL Server include un singolo endpoint TCP predefinito che non può essere rimosso. Per AD FS, è consigliabile abilitare questo endpoint TCP per l'autenticazione Kerberos. Per esaminare gli endpoint TCP correnti e verificare se a un'installazione di SQL sono state aggiunte altre porte TCP definite dall'utente, è possibile usare l'istruzione di query "SELECT * FROM sys.tcp_endpoints" in una sessione Transact-SQL (T-SQL). Per altre informazioni sulla configurazione di endpoint di SQL Server, vedere [How To: Configurare il motore di Database per l'attesa su più porte TCP](https://go.microsoft.com/fwlink/?LinkID=189231) (https://go.microsoft.com/fwlink/?LinkID=189231).  
  
-   **Evitare di usare l'autenticazione basata su SQL.**  
  
    Per evitare di dover trasferire password non crittografate attraverso la rete o di archiviare password nelle impostazioni di configurazione, usare l'autenticazione di Windows solo con l'installazione di SQL Server. L'autenticazione di SQL Server è una modalità di autenticazione legacy. Non è consigliabile archiviare le credenziali di accesso SQL (Structured Query Language), cioè nomi utente e password SQL, quando si usa l'autenticazione di SQL Server. Per altre informazioni, vedere [modalità di autenticazione](https://go.microsoft.com/fwlink/?LinkID=189232) (https://go.microsoft.com/fwlink/?LinkID=189232).  
  
-   **Valutare con attenzione la necessità di sicurezza del canale aggiuntivo nell'installazione di SQL.**  
  
    Anche con l'autenticazione Kerberos attivata, Security Support Provider Interface (SSPI) di SQL Server non offre la sicurezza a livello di canale. Tuttavia, per le installazioni in cui i server sono collocati in una rete protetta da firewall, la crittografia delle comunicazioni SQL potrebbe non essere necessaria.  
  
    Anche se la crittografia è uno strumento valido ai fini della sicurezza,non deve essere necessariamente considerata per tutti i tipi di dati e di connessioni. Quando si decide se implementare la crittografia, considerare la modalità di accesso ai dati da parte degli utenti. Se gli utenti accedono ai dati attraverso una rete pubblica, la crittografia dei dati potrebbe essere indispensabile per migliorare la sicurezza. Tuttavia, se tutti gli accessi di dati SQL da AD FS implica una configurazione intranet protetta, crittografia potrebbe non essere necessaria. Qualsiasi uso della crittografia dovrebbe includere anche una strategia di manutenzione per password, chiavi e certificati.  
  
    Nel caso si tema che i dati SQL possano essere visualizzati o manomessi attraverso la rete, usare il protocollo IPSec (Internet Protocol Security) o SSL (Secure Sockets Layer) per migliorare la sicurezza delle connessioni SQL. Tuttavia, ciò potrebbe avere un effetto negativo sulle prestazioni di SQL Server, che possono influire o limitare le prestazioni di AD FS in alcune situazioni. Le prestazioni di AD FS nel rilascio dei token, ad esempio, potrebbero ridurre le prestazioni durante le ricerche di attributi da un archivio di attributi basati su SQL sono fondamentali per il rilascio dei token. È possibile scegliere un modo migliore per eliminare una minaccia di manomissione di SQL usando una configurazione di sicurezza del perimetro avanzata. Ad esempio, per proteggere in modo più efficace l'installazione di SQL Server, la soluzione migliore consiste nell'assicurare che rimanga inaccessibile per gli utenti e i computer Internet e accessibile solo per gli utenti e i computer nel proprio ambiente di datacenter.  
  
    Per altre informazioni, vedere [crittografia delle connessioni a SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) oppure [crittografia di SQL Server](https://go.microsoft.com/fwlink/?LinkID=189233).  
  
-   **Configurare l'accesso progettato in modo sicuro usando stored procedure per eseguire ricerche basata su SQL per i dati archiviati in ADFS di SQL di AD.**  
  
    Per offrire un migliore isolamento di dati e servizi, si possono creare stored procedure per tutti i comandi di ricerca nell'archivio attributi. È possibile creare un ruolo del database al quale concedere poi l'autorizzazione di eseguire le stored procedure. Assegnare l'identità del servizio del servizio AD FS Windows a questo ruolo del database. Il servizio AD FS Windows non deve essere in grado di eseguire altre istruzioni SQL, diverse da stored procedure appropriate usate per la ricerca degli attributi. Il blocco dell'accesso al database SQL Server in questo modo riduce il rischio di un attacco tramite elevazione dei privilegi.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
