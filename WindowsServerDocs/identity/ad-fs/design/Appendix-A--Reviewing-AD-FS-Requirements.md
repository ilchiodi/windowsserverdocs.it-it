---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: Appendice A - requisiti per ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5cfb5de77843eebfc152b9c79ac55bab1fa7727
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>Appendice a: revisione AD requisiti per ADFS

>Si applica a: Windows Server 2012

In modo che i partner dell'organizzazione nella distribuzione di Active Directory Federation Services (ADFS) di collaborare senza problemi, è innanzitutto necessario assicurarsi che l'infrastruttura di rete aziendale sia configurata per supportare i requisiti di ADFS per l'account, risoluzione dei nomi e certificati. ADFS include i tipi di requisiti seguenti:  
  
> [!TIP]  
> È possibile trovare altri collegamenti alle risorse ADFS il [mappa del contenuto di AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) pagina in Microsoft TechNet Wiki. Questa pagina è gestita dai membri della Community di ADFS ed è controllata regolarmente dal Team del prodotto ADFS.  
  
## <a name="hardware-requirements"></a>Requisiti hardware  
Si applicano i seguenti requisiti hardware minimi e consigliati per il server federativo e il computer proxy server federativo.  
  
|Requisito hardware|Requisito minimo|Requisito consigliato|  
|------------------------|-----------------------|---------------------------|  
|Velocità della CPU|Core singolo, 1 gigahertz (GHz)|Quad-core, 2 GHz|  
|RAM|1 GB|4 GB|  
|Spazio su disco|50 MB|100 MB|  
  
## <a name="software-requirements"></a>Requisiti software  
ADFS si basa sulle funzionalità del server è integrata nel sistema operativo Windows Server® 2012.  
  
> [!NOTE]  
> I servizi ruolo servizio federativo e Proxy servizio federativo non possono coesistere nello stesso computer.  
  
## <a name="certificate-requirements"></a>Requisiti del certificato  
I certificati rivestono il ruolo più importante nel rendere sicure le comunicazioni tra server federativi, proxy server federativi, applicazioni in grado di riconoscere attestazioni e client Web. I requisiti per i certificati variano, a seconda se si configura un server federativo o un computer proxy server federativo, come descritto in questa sezione.  
  
### <a name="federation-server-certificates"></a>Certificati dei server federativi  
I server federativi sono richiesti i certificati nella tabella seguente.  
  
|Tipo di certificato|Descrizione|È necessario conoscere prima della distribuzione|  
|--------------------|---------------|------------------------------------------|  
|Proteggere il certificato Sockets Layer (SSL)|Si tratta di un certificato Secure Sockets Layer (SSL) standard che viene utilizzato per proteggere le comunicazioni tra server federativi e i client.|Questo certificato deve essere associato per il sito Web predefinito in Internet Information Services (IIS) per un Server federativo o un Proxy Server federativo.  Per un Proxy Server federativo, il binding deve essere configurato in IIS prima di eseguire correttamente la configurazione guidata Proxy Server federativo.<br /><br />**Raccomandazione:** poiché questo certificato deve essere considerato attendibile dai client di ADFS, utilizzare un certificato di autenticazione server rilasciato da un'autorità di certificazione (di terze parti) pubblica (CA), ad esempio VeriSign. **Suggerimento:** il nome del soggetto del certificato viene utilizzato per rappresentare il nome del servizio federativo per ogni istanza di ADFS da distribuire. Per questo motivo, potresti voler consigliabile scegliere un nome soggetto qualsiasi nuova certificati emessi dalla CA che meglio rappresenti il nome della società o organizzazione partner.|  
|Certificato di comunicazione del servizio|Questo certificato consente la protezione dei messaggi WCF per proteggere le comunicazioni tra server federativi.|Per impostazione predefinita, il certificato SSL viene utilizzato come certificato per le comunicazioni del servizio.  Questo può essere modificato utilizzando la console di gestione di ADFS.|  
|Certificato di firma di token|Questo è un standard X509 certificato utilizzato per firmare in modo sicuro tutti i token che il server federativo rilascia.|Il certificato di firma di token deve contenere una chiave privata ed essere concatenato a una radice attendibile nel servizio federativo. Per impostazione predefinita, ADFS crea un certificato autofirmato. Tuttavia, è possibile modificarlo in seguito a un certificato rilasciato dalla CA tramite lo snap-in Gestione ADFS, a seconda delle esigenze dell'organizzazione.|  
|Certificato di decrittografia di token|Si tratta di un certificato SSL standard che viene usato per decrittografare i token in ingresso crittografati da un server federativo del partner. Viene pubblicato anche nei metadati federativi.|Per impostazione predefinita, ADFS crea un certificato autofirmato. Tuttavia, è possibile modificarlo in seguito a un certificato rilasciato dalla CA tramite lo snap-in Gestione ADFS, a seconda delle esigenze dell'organizzazione.|  
  
> [!CAUTION]  
> I certificati utilizzati per la firma di token e di decrittografia di token sono fondamentali per la stabilità del servizio federativo. Poiché la perdita o la rimozione non pianificata di eventuali certificati che sono configurati per questo scopo può causare interruzioni del servizio, è consigliabile eseguire il backup di tali certificati che sono configurati per questo scopo.  
  
Per ulteriori informazioni sui certificati che utilizzano server federativi, vedere [requisiti dei certificati per i server federativi](Certificate-Requirements-for-Federation-Servers.md).  
  
### <a name="federation-server-proxy-certificates"></a>Certificati di proxy server federativo  
I proxy server federativi sono richiesti i certificati nella tabella seguente.  
  
|Tipo di certificato|Descrizione|È necessario conoscere prima della distribuzione|  
|--------------------|---------------|------------------------------------------|  
|Certificato di autenticazione server|Si tratta di un certificato Secure Sockets Layer (SSL) standard usato per proteggere le comunicazioni tra un client Internet e un proxy server federativo di computer.|Questo certificato deve essere associato per il sito Web predefinito in Internet Information Services (IIS) prima di poter eseguire correttamente la configurazione guidata di AD FS Federation Server Proxy.<br /><br />**Raccomandazione:** poiché questo certificato deve essere considerato attendibile dai client di ADFS, utilizzare un certificato di autenticazione server rilasciato da un'autorità di certificazione (di terze parti) pubblica (CA), ad esempio VeriSign.<br /><br />**Suggerimento:** il nome del soggetto del certificato viene utilizzato per rappresentare il nome del servizio federativo per ogni istanza di ADFS da distribuire. Per questo motivo, potresti voler consigliabile scegliere un nome soggetto che meglio rappresenti il nome della società o organizzazione partner.|  
  
Per ulteriori informazioni sui certificati che utilizzano i proxy server federativi, vedere [requisiti dei certificati per i proxy Server federativi](Certificate-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="browser-requirements"></a>Requisiti del browser  
Anche se qualsiasi Web browser corrente con funzionalità JavaScript può essere eseguiti come client ADFS, le pagine Web che vengono fornite per impostazione predefinita sono state testate solo con Internet Explorer versioni 7.0, 8.0 e 9.0, Mozilla Firefox 3.0 e Safari 3.1 su Windows. È necessario abilitare JavaScript e i cookie devono essere abilitato per il basate su browser accesso e disconnessione funzioni correttamente.  
  
Il team del prodotto AD FS in Microsoft ha testato correttamente le configurazioni di browser e del sistema operativo nella tabella seguente.  
  
|Browser|Windows 7|Windows Vista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7.0|X|X|  
|Internet Explorer 8.0|X|X|  
|Internet Explorer 9.0|X|Non testato|  
|FireFox 3.0|X|X|  
|Safari 3.1|X|X|  
  
> [!NOTE]  
> ADFS supporta le versioni a 32 bit e 64 bit di tutti i browser inclusi nella tabella precedente.  
  
### <a name="cookies"></a>Cookie  
ADFS crea cookie permanenti e basati su sessione che devono essere archiviati nei computer client per fornire accesso, disconnessione, single sign-on (SSO) e altre funzionalità. Di conseguenza, il browser client deve essere configurato per accettare i cookie. I cookie usati per l'autenticazione sono sempre cookie di sessione Secure Hypertext Transfer Protocol (HTTPS) che vengono scritti per il server di origine. Se il browser client non è configurato per abilitare questi cookie, ADFS non può funzionare correttamente. I cookie permanenti vengono usati per mantenere la selezione dell'utente del provider di attestazioni. È possibile disabilitarli usando un'impostazione di configurazione nel file di configurazione per le pagine di accesso AD FS.  
  
Supporto per TLS/SSL è necessario per motivi di sicurezza.  
  
## <a name="network-requirements"></a>Requisiti di rete  
Configurazione dei seguenti servizi di rete in modo appropriato è fondamentale per la corretta distribuzione di ADFS nell'organizzazione.  
  
### <a name="tcpip-network-connectivity"></a>Connettività di rete TCP/IP  
Per AD FS alla funzione, deve esistere una connettività di rete TCP/IP tra il client. un controller di dominio. e i computer che ospitano il servizio federativo, il servizio federativo (quando viene usato) e l'agente Web ADFS.  
  
### <a name="dns"></a>DNS  
Il servizio di rete primario fondamentale per il funzionamento di AD FS, diverso da servizi di dominio Active Directory (AD DS), è sistema DNS (Domain Name). Quando viene distribuito DNS, gli utenti possono utilizzare nomi di computer descrittivi facili da ricordare per connettersi a computer e altre risorse su reti IP.  
  
 Windows Server 2008 utilizza il DNS per la risoluzione dei nomi invece la risoluzione dei nomi NetBIOS di Windows Internet Name Service (WINS) utilizzato nelle reti basate su Windows NT 4.0. È comunque possibile utilizzare WINS per le applicazioni che lo richiedono. Tuttavia, di dominio Active Directory e AD FS richiedono la risoluzione dei nomi DNS.  
  
Il processo di configurazione DNS per il supporto di ADFS varia a seconda se:  
  
-   L'organizzazione ha già un'infrastruttura DNS esistente. Nella maggior parte degli scenari DNS è già configurato in tutta la rete in modo che i client del Web browser della rete aziendale hanno accesso a Internet. Poiché Internet access e risoluzione dei nomi sono requisiti di AD FS, questa infrastruttura è considerata presenti per la distribuzione di ADFS.  
  
-   Si prevede di aggiungere un server federativo alla rete aziendale. Ai fini dell'autenticazione degli utenti nella rete aziendale, server DNS interni nella foresta della rete aziendale deve essere configurato per restituiscano il CNAME del server interno che è in esecuzione il servizio federativo. Per ulteriori informazioni, vedere [requisiti di risoluzione dei nomi per i server federativi](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   Si prevede di aggiungere un proxy server federativo alla rete perimetrale. Quando si vuole autenticare gli account utente che si trovano nella rete aziendale dell'organizzazione partner di identità, il server DNS interni nella foresta della rete aziendale deve essere configurato restituiscano il CNAME del proxy server federativo interna. Per informazioni su come configurare DNS per supportare l'aggiunta di server federativi, vedere [requisiti di risoluzione dei nomi per i proxy Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   Si imposta DNS per un ambiente di testing. Se si prevede di utilizzare ADFS in un ambiente di testing in cui nessun server DNS radice è autorevole, è probabile che sarà necessario impostare i server d'inoltro DNS in modo che le query sui nomi tra due o più foreste vengano inoltrate correttamente. Per informazioni generali su come configurare un ambiente di testing AD FS, vedere [ADFS dettagliate e Guide](https://go.microsoft.com/fwlink/?LinkId=180357).  
  
## <a name="attribute-store-requirements"></a>Requisiti dell'archivio attributi  
ADFS richiede almeno un archivio di attributi da utilizzare per l'autenticazione degli utenti e l'estrazione delle attestazioni di sicurezza per gli utenti. Per un elenco di attributi di archivi che supportano ADFS, vedere [il ruolo degli archivi attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md) nella Guida alla progettazione di ADFS.  
  
> [!NOTE]  
> ADFS crea automaticamente un archivio di attributi di Active Directory, per impostazione predefinita.  
  
Requisiti dell'archivio attributi dipendono se l'organizzazione funge da partner account (che ospita gli utenti federati) o il partner risorse (che ospita l'applicazione federata).  
  
### <a name="ad-ds"></a>SERVIZI DI DOMINIO ACTIVE DIRECTORY  
Per AD FS per il corretto funzionamento, i controller di dominio nell'organizzazione partner account o dell'organizzazione partner risorse devono eseguire Windows Server 2003 SP1, Windows Server 2003 R2, Windows Server 2008 o Windows Server 2012.  
  
Quando AD FS è installato e configurato in un computer aggiunto al dominio, l'archivio account utente di Active Directory per tale dominio sarà disponibile come archivio attributi selezionabile.  
  
> [!IMPORTANT]  
> Poiché ADFS richiede l'installazione di Internet Information Services (IIS), è consigliabile non installare il software ADFS in un controller di dominio in un ambiente di produzione per motivi di sicurezza. Tuttavia, questa configurazione è supportata dal servizio supporto tecnico clienti Microsoft.  
  
#### <a name="schema-requirements"></a>Requisiti dello schema  
ADFS non richiede modifiche dello schema o modifiche a livello funzionale di dominio Active Directory.  
  
#### <a name="functional-level-requirements"></a>Requisiti a livello funzionale  
La maggior parte delle funzionalità di ADFS non richiedono modifiche livello funzionale di dominio Active Directory per il corretto funzionamento. Tuttavia, funzionalità del dominio Windows Server 2008 livello o versione successiva è richiesto per l'autenticazione del certificato client per il corretto funzionamento se il certificato è mappato in modo esplicito a un account utente di dominio Active Directory.  
  
#### <a name="service-account-requirements"></a>Requisiti dell'account del servizio  
Se si sta creando una server farm federativa, è innanzitutto necessario creare un account di servizio dedicato basato su dominio in Active Directory che è possibile utilizzare il servizio federativo. In un secondo momento, si configurerà ogni server federativo della farm per utilizzare questo account. Per ulteriori informazioni su come eseguire questa operazione, vedere [configurare manualmente un Account del servizio per una Server Farm federativa](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) nella Guida alla distribuzione di ADFS.  
  
### <a name="ldap"></a>LDAP  
Quando si usano altri archivi attributi basati su LDAP Lightweight Directory Access Protocol, è necessario connettersi a un server LDAP che supporta l'autenticazione integrata di Windows. La stringa di connessione LDAP deve inoltre essere scritto nel formato di URL LDAP, come descritto nella RFC 2255.  
  
### <a name="sql-server"></a>SQL Server  
Per AD FS per il corretto funzionamento, i computer che ospitano l'archivio di attributi Structured Query Language (SQL) Server devono eseguire Microsoft SQL Server 2005 o SQL Server 2008. Quando si usano archivi attributi basati su SQL, è necessario configurare una stringa di connessione.  
  
### <a name="custom-attribute-stores"></a>Archivi di attributi personalizzati  
È possibile sviluppare archivi di attributi personalizzati per abilitare scenari avanzati. Il linguaggio dei criteri integrato in ADFS può fare riferimento archivi di attributi personalizzati, in modo che può essere migliorato uno qualsiasi dei seguenti scenari:  
  
-   Creazione di attestazioni per un utente autenticato localmente  
  
-   Integrazione di attestazioni per un utente autenticato esternamente  
  
-   Autorizzazione di un utente di ottenere un token  
  
-   Autorizzazione di un servizio per ottenere un token sul comportamento di un utente  
  
Quando si lavora con un archivio attributi personalizzato, è inoltre necessario configurare una stringa di connessione. In questo caso, è possibile immettere codice personalizzato preferisci che consente la connessione all'archivio di attributi personalizzati. La stringa di connessione in questa situazione è un set di coppie nome/valore che vengono interpretate come implementato dallo sviluppatore dell'archivio di attributi personalizzati.  
  
Per ulteriori informazioni sullo sviluppo e l'uso di archivi di attributi personalizzati, vedere [Cenni preliminari sull'archivio di attributi](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="application-requirements"></a>Requisiti dell'applicazione  
Server federativi possono comunicare con e proteggere le applicazioni di federazione, ad esempio le applicazioni in grado di riconoscere attestazioni.  
  
## <a name="authentication-requirements"></a>Requisiti di autenticazione  
AD FS integra naturale con l'autenticazione di Windows esistente, ad esempio, l'autenticazione Kerberos, NTLM, smart card e certificati di x. 509 v3 sul lato client. Server federativi usano l'autenticazione Kerberos standard per autenticare un utente in un dominio. I client possono autenticarsi usando l'autenticazione basata su form, l'autenticazione con smart card e autenticazione integrata di Windows, a seconda di come si configura l'autenticazione.  
  
Il ruolo di proxy server federativo di ADFS rende possibile uno scenario in cui l'utente si autentica esternamente con l'autenticazione client SSL. È inoltre possibile configurare il ruolo del server federativo per richiedere l'autenticazione client SSL, anche se in genere esperienza utente ottimale si ottiene configurando il server federativo di account per l'autenticazione integrata di Windows. In questo caso, ADFS non ha alcun controllo sul tipo di credenziali usate dall'utente per l'accesso al desktop Windows.  
  
### <a name="smart-card-logon"></a>Accesso con smart card  
Anche se ADFS è possibile applicare il tipo di credenziali utilizzate per l'autenticazione (password, l'autenticazione client SSL o autenticazione integrata di Windows), non applica direttamente l'autenticazione con smart card. Di conseguenza, ADFS non fornisce un'interfaccia utente del client (UI) per ottenere le credenziali (PIN) numero di identificazione personale di smart card. Questo avviene perché i client basati su Windows non forniscono intenzionalmente i dettagli delle credenziali utente per i server federativi o server Web.  
  
### <a name="smart-card-authentication"></a>Autenticazione con smart card  
Autenticazione con smart card Usa il protocollo Kerberos per autenticare un server federativo di account. ADFS non può essere esteso per aggiungere nuovi metodi di autenticazione. Il certificato nella smart card non è necessario alla catena fino a una radice attendibile nel computer client. Utilizzo di un certificato basato su smart card con AD FS richiede le seguenti condizioni:  
  
-   Il lettore e il provider del servizio di crittografia (CSP) per la smart card devono funzionare nel computer in cui si trova il browser.  
  
-   Il certificato smart card deve conduce a una radice attendibile nel server federativo di account e il proxy server federativo di account.  
  
-   Il certificato deve essere mappato all'account utente di dominio Active Directory utilizzando uno dei metodi seguenti:  
  
    -   Il nome soggetto del certificato corrisponde al nome distinto LDAP di un account utente di dominio Active Directory.  
  
    -   L'estensione relativa al nome alternativo soggetto del certificato ha il nome dell'entità utente (UPN) di un account utente di dominio Active Directory.  
  
Per supportare determinati requisiti di complessità dell'autenticazione in alcuni scenari, è anche possibile configurare ADFS per creare un'attestazione che indica come l'autenticazione dell'utente. Una relying party può quindi usare questa attestazione per prendere decisioni di autorizzazione.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
