---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: Procedure consigliate per la pianificazione e la distribuzione sicure di ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: be488ccffee7b267d2a3a120b85436abf206f65a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359197"
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Procedure consigliate per la pianificazione e la distribuzione sicure di ADFS


In questo argomento vengono fornite informazioni sulle procedure consigliate per la pianificazione e la valutazione della sicurezza quando si progetta la distribuzione di Active Directory Federation Services (AD FS). Questo argomento è un punto di partenza per rivedere e valutare le considerazioni che influiscono sulla sicurezza complessiva dell'utilizzo di AD FS. Le informazioni in questo argomento sono complementari ed estendono la pianificazione della sicurezza esistente, nonché altre procedure consigliate per la progettazione.  
  
## <a name="core-security-best-practices-for-ad-fs"></a>Procedure consigliate per la sicurezza di base per ADFS  
Le seguenti procedure consigliate di base sono comuni a tutte le installazioni di AD FS in cui si desidera migliorare o estendere la sicurezza della progettazione o della distribuzione:  

-   **Proteggere AD FS come sistema "di livello 0"** 

    AD FS è fondamentalmente un sistema di autenticazione.  Pertanto, deve essere considerato come un sistema "di livello 0" come altro sistema di identità nella rete.  [Microsoft docs](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material) contiene altre informazioni sul modello di livello amministrativo Active Directory. 


-   **Utilizzare la configurazione guidata sicurezza per applicare procedure consigliate per la sicurezza specifiche AD FS ai server federativi e ai computer proxy server federativi**  
  
    La configurazione guidata impostazioni di sicurezza è uno strumento che viene preinstallato in tutti i computer Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012. È possibile usarla per applicare le procedure consigliate per la sicurezza che contribuiscono a ridurre la superficie di attacco di un server in base ai ruoli del server installati.  
  
    Quando si installa ADFS, il programma di installazione crea file di estensione dei ruoli che si possono usare con la Configurazione guidata impostazioni di sicurezza per creare i criteri di sicurezza che saranno applicati al ruolo del server ADFS specifico, cioè server federativo o proxy server federativo, scelto durante l'installazione.  
  
    Ogni file di estensione dei ruoli installato rappresenta il tipo di ruolo e ruolo secondario per cui è configurato ogni computer. I seguenti file di estensione del ruolo vengono installati nella directory C:WindowsADFSScw:  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml (questo file è presente solo se per il computer è configurato il ruolo di proxy server federativo).  
  
    Per applicare le estensioni dei ruoli di ADFS nella Configurazione guidata impostazioni di sicurezza, completare i seguenti passaggi nell'ordine indicato:  
  
    1.  Installare ADFS e scegliere il ruolo del server appropriato per il computer. Per ulteriori informazioni, vedere [installare il servizio ruolo Proxy servizio federativo](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) nella Guida alla distribuzione di ad FS.  
  
    2.  Registrare il file di estensione dei ruoli appropriato con lo strumento da riga di comando Scwcmd. Per dettagli sull'uso di questo strumento nel ruolo per cui è configurato il computer, vedere la tabella seguente.  
  
    3.  Verificare che il comando sia stato completato correttamente esaminando il file SCWRegister_log. XML, che si trova nella directory WindowssecurityMsscwLogs  
  
    Tutti questi passaggi devono essere eseguiti su ogni computer server federativo o proxy server federativo a cui si vogliono applicare i criteri di sicurezza della Configurazione guidata impostazioni di sicurezza basati su ADFS.  
  
    Nella seguente tabella è spiegato come registrare l'estensione dei ruoli della Configurazione guidata impostazioni di sicurezza appropriata in base al ruolo del server ADFS scelto nel computer in cui è installato ADFS.  
  
    |Ruolo del server ADFS|Database di configurazione di ADFS usato|Al prompt dei comandi digitare il comando seguente:|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |Server federativo autonomo|Database interno di Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |Server federativo aggiunto alla farm|Database interno di Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |Server federativo aggiunto alla farm|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |Proxy server federativo|N/D|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    Per altre informazioni sui database che si possono usare con ADFS, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Usare il rilevamento della riproduzione dei token in situazioni in cui la sicurezza è un problema molto importante, ad esempio quando vengono usati chioschi multimediali.**  
    Il rilevamento della riproduzione dei token è una funzionalità di AD FS che garantisce che qualsiasi tentativo di riprodurre una richiesta di token effettuata al Servizio federativo venga rilevato e che la richiesta venga eliminata. Il rilevamento riproduzione token è abilitato per impostazione predefinita. Funziona sia per il profilo passivo WS-Federation sia per il profilo WebSSO SAML (Security Assertion Markup Language), assicurando che lo stesso token non venga mai usato più di una volta.  
  
    Quando il Servizio federativo viene avviato, inizia a compilare una cache di tutte le richieste di token soddisfatte. Poiché con il tempo vengono aggiunte alla cache richieste di token successive, per il Servizio federativo aumenta la capacità di rilevare qualsiasi tentativo di riprodurre più volte una richiesta di token. Se si disabilita il rilevamento riproduzione token e in seguito si sceglie di riabilitarlo, si ricordi che il Servizio federativo continuerà ad accettare token per un periodo di tempo che potrebbe essere stato usato in precedenza, finché la cache di riproduzione non avrà avuto abbastanza tempo per ricostruire il proprio contenuto. Per altre informazioni, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Usare la crittografia dei token, soprattutto se si usa la risoluzione degli artefatti SAML di supporto.**  
  
    La crittografia dei token è fortemente consigliata per migliorare la sicurezza e la protezione contro potenziali attacchi man-in-the-Middle (MITM) che potrebbero essere tentati per la distribuzione di AD FS. L'uso della crittografia potrebbe avere un leggero impatto sulla velocità effettiva, ma in generale non è di solito rilevabile e in molte distribuzioni i vantaggi dati da una maggiore sicurezza superano qualsiasi costo in termini di prestazioni del server.  
  
    Per abilitare la crittografia di token, aggiungere prima di tutto un certificato di crittografia per i trust della relying party. È possibile configurare un certificato di crittografia quando si crea un trust della relying party o in seguito. Per aggiungere un certificato di crittografia in un secondo momento a un trust relying party esistente, è possibile impostare un certificato da utilizzare nella scheda **crittografia** all'interno delle proprietà di attendibilità durante l'utilizzo dello snap-in ad FS. Per specificare un certificato per un trust esistente usando i cmdlet di AD FS, usare il parametro EncryptionCertificate del cmdlet **set-ClaimsProviderTrust** o **set-RelyingPartyTrust** . Per impostare un certificato per il Servizio federativo da usare per la decrittografia dei token, usare il cmdlet **set-ADFSCertificate** e specificare "`Token-Encryption`" per il parametro *CertificateType* . Per abilitare e disabilitare la crittografia per il trust della relying party specifico, è possibile usare il parametro *EncryptClaims* del cmdlet **Set-RelyingPartyTrust**.  
  
-   **Usare la protezione estesa per l'autenticazione**  
  
    Per proteggere le distribuzioni, è possibile impostare e usare la funzionalità di protezione estesa per l'autenticazione con AD FS. Questa impostazione consente di specificare il livello di protezione estesa per l'autenticazione supportata da un server federativo.  
  
    La protezione estesa per l'autenticazione consente di contrastare gli attacchi man-in-the-middle in cui l'autore di un attacco intercetta le credenziali del client e le inoltra a un server. La protezione da tali attacchi è possibile grazie al token di binding che può essere richiesto, consentito o non richiesto dal server quando stabilisce le comunicazioni con i client.  
  
    Per abilitare la funzionalità di protezione estesa, usare il parametro **ExtendedProtectionTokenCheck** nel cmdlet **Set-ADFSProperties**. I valori possibili di questa impostazione e il livello di sicurezza fornito dai valori sono descritti nella tabella seguente.  
  
    |Valore del parametro|Livello Sicurezza|Impostazione di protezione|  
    |-------------------|------------------|----------------------|  
    |Richiedi|Il server è completamente protetto.|La protezione estesa è applicata e sempre richiesta.|  
    |Consenti|Il server è parzialmente protetto.|La protezione estesa è applicata ai sistemi interessati se sono stati configurati per supportarla.|  
    |Nessuno|Il server è vulnerabile.|La protezione estesa non è applicata.|  
  
-   **Se si usano la registrazione e la traccia, assicurarsi che le informazioni riservate siano riservate.**  
  
    Per impostazione predefinita, AD FS non espone o tiene traccia direttamente delle informazioni personali come parte del Servizio federativo o delle normali operazioni. Quando la registrazione degli eventi e la registrazione della traccia di debug sono abilitate in AD FS, tuttavia, a seconda dei criteri delle attestazioni configurati alcuni tipi di attestazione e i relativi valori associati possono contenere informazioni personali che potrebbero essere registrate nei log eventi AD FS o traccia.  
  
    Pertanto, è consigliabile applicare il controllo di accesso nella configurazione del AD FS e i relativi file di log. Se non si vuole che questo tipo di informazioni sia visibile, disabilitare la registrazione o filtrare qualsiasi informazione personale o riservata eliminandola dai log prima di condividerli con altri.  
  
    I suggerimenti seguenti possono essere utili per evitare che il contenuto di un file di log sia esposto accidentalmente:  
  
    -   Verificare che i file del registro eventi e di log di traccia di AD FS siano protetti da elenchi di controllo di accesso (ACL) che limitano l'accesso solo agli amministratori attendibili che ne richiedono l'accesso.  
  
    -   Non copiare o archiviare file di log usando estensioni di file o percorsi che possono essere facilmente serviti usando una richiesta Web. Ad esempio, l'estensione di file XML non è una scelta sicura. Per un elenco di estensioni che possono essere servite, vedere la guida all'amministrazione di Internet Information Services (IIS).  
  
    -   Se si modifica il percorso del file di log, accertarsi di specificare un percorso assoluto che dovrebbe trovarsi all'esterno della directory radice virtuale pubblica dell'host Web, in modo da evitarne l'accesso da una parte esterna con un Web browser.  

-   **AD FS il blocco flessibile Extranet e la protezione con blocco intelligente AD FS Extranet**  
    
    Nel caso di un attacco sotto forma di richieste di autenticazione con password non valide che passano attraverso il proxy applicazione Web, AD FS blocco Extranet consente di proteggere gli utenti da un blocco dell'account AD FS. Oltre a proteggere gli utenti da un blocco di account AD FS, AD FS blocco Extranet protegge anche dagli attacchi di forza bruta per la password.  
    
    Per il blocco software Extranet per AD FS in Windows Server 2012 R2, vedere la pagina relativa alla [protezione del blocco soft di ad FS Extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md).  

     Per il blocco Smart Extranet per AD FS in Windows Server 2016, vedere la pagina relativa alla [protezione del blocco intelligente di ad FS Extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md).  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>Procedure consigliate per la sicurezza di base specifiche di SQL Server per ADFS  
Le seguenti procedure consigliate per la protezione sono specifiche per l'utilizzo di Microsoft SQL Server® o database interno di Windows quando tali tecnologie di database vengono utilizzate per gestire i dati in AD FS progettazione e distribuzione.  
  
> [!NOTE]  
> Questi suggerimenti costituiscono un'estensione della guida alla sicurezza del prodotto SQL Server, ma non la sostituiscono. Per ulteriori informazioni sulla pianificazione di un'installazione SQL Server protetta, vedere [considerazioni sulla sicurezza per un'installazione di SQL protetto](https://go.microsoft.com/fwlink/?LinkID=139831) (https://go.microsoft.com/fwlink/?LinkID=139831).  
  
-   **Distribuire sempre SQL Server protetto da un firewall in un ambiente di rete fisicamente protetto.**  
  
    Un'installazione di SQL Server non deve mai essere esposta direttamente a Internet. Solo i computer che si trovano all'interno del Data Center dovrebbero essere in grado di raggiungere l'installazione di SQL Server che supporta AD FS. Per ulteriori informazioni, vedere [elenco di controllo delle procedure consigliate](https://go.microsoft.com/fwlink/?LinkID=189229) per la protezione (https://go.microsoft.com/fwlink/?LinkID=189229).  
  
-   **Eseguire SQL Server in un account del servizio anziché utilizzare gli account di servizio di sistema predefiniti predefiniti.**  
  
    Per impostazione predefinita, SQL Server è spesso installato e configurato per l'uso di uno degli account di sistema predefiniti supportati, ad esempio LocalSystem o NetworkService. Per migliorare la sicurezza dell'installazione di SQL Server per AD FS, laddove possibile usare un account di servizio separato per accedere al servizio SQL Server e abilitare l'autenticazione Kerberos registrando il nome dell'entità di sicurezza (SPN) di questo account nel Active Directory la distribuzione. In questo modo si abilita l'autenticazione reciproca tra client e server. Senza la registrazione dell'SPN di un account del servizio distinto, SQL Server userà NTLM per l'autenticazione basata su Windows in cui solo il client viene autenticato.  
  
-   **Ridurre a icona la superficie di attacco del SQL Server.**  
  
    Abilitare solo gli endpoint di SQL Server necessari. Per impostazione predefinita, SQL Server include un singolo endpoint TCP predefinito che non può essere rimosso. Per AD FS, è necessario abilitare questo endpoint TCP per l'autenticazione Kerberos. Per esaminare gli endpoint TCP correnti e verificare se a un'installazione di SQL sono state aggiunte altre porte TCP definite dall'utente, è possibile usare l'istruzione di query "SELECT * FROM sys.tcp_endpoints" in una sessione Transact-SQL (T-SQL). Per ulteriori informazioni sulla configurazione dell'endpoint SQL Server, vedere [procedura: configurare il motore di database per l'ascolto su più porte TCP](https://go.microsoft.com/fwlink/?LinkID=189231) (https://go.microsoft.com/fwlink/?LinkID=189231).  
  
-   **Evitare di usare l'autenticazione basata su SQL.**  
  
    Per evitare di dover trasferire password non crittografate attraverso la rete o di archiviare password nelle impostazioni di configurazione, usare l'autenticazione di Windows solo con l'installazione di SQL Server. L'autenticazione di SQL Server è una modalità di autenticazione legacy. Non è consigliabile archiviare le credenziali di accesso SQL (Structured Query Language), cioè nomi utente e password SQL, quando si usa l'autenticazione di SQL Server. Per ulteriori informazioni, vedere [modalità di autenticazione](https://go.microsoft.com/fwlink/?LinkID=189232) (https://go.microsoft.com/fwlink/?LinkID=189232).  
  
-   **Valutare con attenzione la necessità di ulteriore sicurezza del canale nell'installazione di SQL.**  
  
    Anche con l'autenticazione Kerberos attivata, Security Support Provider Interface (SSPI) di SQL Server non offre la sicurezza a livello di canale. Tuttavia, per le installazioni in cui i server sono collocati in una rete protetta da firewall, la crittografia delle comunicazioni SQL potrebbe non essere necessaria.  
  
    Anche se la crittografia è uno strumento valido ai fini della sicurezza,non deve essere necessariamente considerata per tutti i tipi di dati e di connessioni. Quando si decide se implementare la crittografia, considerare la modalità di accesso ai dati da parte degli utenti. Se gli utenti accedono ai dati attraverso una rete pubblica, la crittografia dei dati potrebbe essere indispensabile per migliorare la sicurezza. Tuttavia, se tutti gli accessi ai dati SQL per AD FS implicano una configurazione Intranet sicura, la crittografia potrebbe non essere necessaria. Qualsiasi uso della crittografia dovrebbe includere anche una strategia di manutenzione per password, chiavi e certificati.  
  
    Nel caso si tema che i dati SQL possano essere visualizzati o manomessi attraverso la rete, usare il protocollo IPSec (Internet Protocol Security) o SSL (Secure Sockets Layer) per migliorare la sicurezza delle connessioni SQL. Questo potrebbe tuttavia avere un effetto negativo sulle prestazioni SQL Server, che può influire o limitare le prestazioni di AD FS in alcune situazioni. Ad esempio, AD FS le prestazioni nel rilascio di token possono peggiorare quando le ricerche di attributi da un archivio di attributi basato su SQL sono cruciali per il rilascio di token. È possibile scegliere un modo migliore per eliminare una minaccia di manomissione di SQL usando una configurazione di sicurezza del perimetro avanzata. Ad esempio, per proteggere in modo più efficace l'installazione di SQL Server, la soluzione migliore consiste nell'assicurare che rimanga inaccessibile per gli utenti e i computer Internet e accessibile solo per gli utenti e i computer nel proprio ambiente di datacenter.  
  
    Per ulteriori informazioni, vedere Crittografia delle [connessioni a SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) o [crittografia SQL Server](https://go.microsoft.com/fwlink/?LinkID=189233).  
  
-   **Configurare l'accesso progettato in modo sicuro usando stored procedure per eseguire tutte le ricerche basate su SQL tramite AD FS di dati archiviati in SQL.**  
  
    Per offrire un migliore isolamento di dati e servizi, si possono creare stored procedure per tutti i comandi di ricerca nell'archivio attributi. È possibile creare un ruolo del database al quale concedere poi l'autorizzazione di eseguire le stored procedure. Assegnare l'identità del servizio AD FS servizio Windows a questo ruolo del database. Il servizio di AD FS Windows non deve essere in grado di eseguire altre istruzioni SQL, ad eccezione delle stored procedure appropriate utilizzate per la ricerca di attributi. Il blocco dell'accesso al database SQL Server in questo modo riduce il rischio di un attacco tramite elevazione dei privilegi.  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
