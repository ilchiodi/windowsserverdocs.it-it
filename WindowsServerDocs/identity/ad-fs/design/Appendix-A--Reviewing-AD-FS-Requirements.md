---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: Appendice A-revisione dei requisiti di AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 281bb3763bc13e28b007a819254de382dc977f1c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408155"
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>Appendice A: Verifica dei requisiti di ADFS

In modo che i partner dell'organizzazione nella distribuzione di Active Directory Federation Services (AD FS) possano collaborare correttamente, è necessario assicurarsi che l'infrastruttura di rete aziendale sia configurata per supportare AD FS requisiti per gli account, nome risoluzione e certificati. AD FS presenta i seguenti tipi di requisiti:  
  
> [!TIP]  
> È possibile trovare altri collegamenti alle risorse di AD FS nella pagina della [mappa del contenuto di AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) nel sito Wiki di Microsoft TechNet. Questa pagina è gestita dai membri della community di ADFS ed è controllata regolarmente dal team del prodotto ADFS.  
  
## <a name="hardware-requirements"></a>Requisiti hardware  
I requisiti hardware minimi e consigliati seguenti si applicano ai computer del server federativo e del proxy server federativo.  
  
|Requisito hardware|Requisito minimo|Requisito consigliato|  
|------------------------|-----------------------|---------------------------|  
|Velocità di CPU|Core singolo, 1 gigahertz (GHz)|Quad-core, 2 GHz|  
|RAM|1 GB|4 GB|  
|Spazio su disco|50 MB|100 MB|  
  
## <a name="software-requirements"></a>Requisiti software  
AD FS si basa sulle funzionalità server integrate nel sistema operativo Windows Server® 2012.  
  
> [!NOTE]  
> I servizi ruolo Servizio federativo e Proxy servizio federativo non possono coesistere nello stesso computer.  
  
## <a name="certificate-requirements"></a>Requisiti dei certificati  
I certificati svolgono il ruolo più critico nella protezione delle comunicazioni tra server federativi, proxy server federativi, applicazioni in grado di riconoscere attestazioni e client Web. I requisiti per i certificati variano a seconda che si configuri un computer server federativo o proxy server federativo, come descritto in questa sezione.  
  
### <a name="federation-server-certificates"></a>Certificati dei server federativi  
Per i server federativi sono richiesti i certificati indicati nella tabella seguente.  
  
|Tipo di certificato|Descrizione|Informazioni che occorre conoscere prima della distribuzione|  
|--------------------|---------------|------------------------------------------|  
|Certificato SSL (Secure Sockets Layer)|Si tratta di un certificato SSL (Secure Sockets Layer) standard usato per proteggere le comunicazioni tra server federativi e client.|Questo certificato deve essere associato al sito Default Web Site in Internet Information Services (IIS) per un server federativo o un proxy server federativo.  Per un proxy server federativo l'associazione deve essere configurata in IIS prima di poter eseguire correttamente la Configurazione guidata del proxy del server federativo.<br /><br />**Raccomandazione** poiché questo certificato deve essere considerato attendibile dai client di ADFS, usare un certificato di autenticazione server rilasciato da un'autorità di certificazione (CA) pubblica (di terze parti), ad esempio VeriSign. **Punta** Il nome soggetto del certificato viene usato per rappresentare il nome del Servizio federativo per ogni istanza di ADFS distribuita. Per questo motivo, in un nuovo certificato rilasciato da una CA è consigliabile scegliere un nome soggetto che meglio rappresenti il nome della società o dell'organizzazione per i partner.|  
|Certificato per le comunicazioni di servizi|Questo certificato abilita la sicurezza dei messaggi WCF per proteggere le comunicazioni tra server federativi.|Per impostazione predefinita, come certificato per le comunicazioni di servizi viene usato il certificato SSL.  L'impostazione può essere modificata con la console di gestione di ADFS.|  
|Certificato per la firma di token|Questo certificato X509 standard viene usato per firmare in modo sicuro tutti i token rilasciati dal server federativo.|Il certificato per la firma di token deve contenere una chiave privata ed essere concatenato a una radice attendibile nel Servizio federativo. Per impostazione predefinita, ADFS crea un certificato autofirmato. È tuttavia possibile modificare questa impostazione in un secondo momento, secondo le esigenze dell'organizzazione, specificando un certificato rilasciato da una CA con lo snap-in Gestione ADFS.|  
|Certificato di decrittografia token|Si tratta di un certificato SSL standard usato per decrittografare i token in ingresso crittografati da un server federativo di un partner. Viene pubblicato anche nei metadati federativi.|Per impostazione predefinita, ADFS crea un certificato autofirmato. È tuttavia possibile modificare questa impostazione in un secondo momento, secondo le esigenze dell'organizzazione, specificando un certificato rilasciato da una CA con lo snap-in Gestione ADFS.|  
  
> [!CAUTION]  
> I certificati usati per la firma di token e per la decrittografia di token sono critici per la stabilità del Servizio federativo. Poiché la perdita o la rimozione non pianificata di qualsiasi certificato configurato a questo scopo può causare l'interruzione del servizio, è consigliabile eseguire il backup di tali certificati.  
  
Per altre informazioni sui certificati usati dai server federativi, vedere [Requisiti dei certificati per i server federativi](Certificate-Requirements-for-Federation-Servers.md).  
  
### <a name="federation-server-proxy-certificates"></a>Certificati dei proxy server federativi  
Per i proxy server federativi sono richiesti i certificati indicati nella tabella seguente.  
  
|Tipo di certificato|Descrizione|Informazioni che occorre conoscere prima della distribuzione|  
|--------------------|---------------|------------------------------------------|  
|Certificato di autenticazione del server|Si tratta di un certificato SSL (Secure Sockets Layer) standard usato per proteggere le comunicazioni tra un proxy server federativo e i computer client Internet.|Questo certificato deve essere associato al sito Default Web Site in Internet Information Services (IIS) prima di poter eseguire correttamente la Configurazione guidata del proxy del server federativo ADFS.<br /><br />**Raccomandazione** poiché questo certificato deve essere considerato attendibile dai client di ADFS, usare un certificato di autenticazione server rilasciato da un'autorità di certificazione (CA) pubblica (di terze parti), ad esempio VeriSign.<br /><br />**Punta** Il nome soggetto del certificato viene usato per rappresentare il nome del Servizio federativo per ogni istanza di ADFS distribuita. Per questo motivo, è consigliabile scegliere un nome soggetto che meglio rappresenti il nome della società o dell'organizzazione per i partner.|  
  
Per altre informazioni sui certificati usati dai proxy server federativi, vedere [Requisiti dei certificati per i proxy server federativi](Certificate-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="browser-requirements"></a>Requisiti del browser  
Anche se qualsiasi Web Browser corrente con funzionalità JavaScript può essere usato come client ADFS, le pagine Web fornite per impostazione predefinita sono state testate solo con Internet Explorer versioni 7.0, 8.0 e 9.0, Mozilla Firefox 3.0 e Safari 3.1 su Windows. È necessario abilitare JavaScript e i cookie per il corretto funzionamento dell'accesso e della disconnessione basati su browser.  
  
Il team del prodotto AD FS in Microsoft ha testato correttamente le configurazioni del browser e del sistema operativo nella tabella seguente.  
  
|Browser|Windows 7|Windows Vista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7,0|x|x|  
|Internet Explorer 8,0|x|x|  
|Internet Explorer 9,0|x|Non testato|  
|FireFox 3.0|x|x|  
|Safari 3.1|x|x|  
  
> [!NOTE]  
> ADFS supporta le versioni a 32 bit e 64 bit di tutti i browser inclusi nella tabella precedente.  
  
### <a name="cookies"></a>Cookie  
ADFS crea cookie permanenti e basati su sessione che devono essere archiviati nei computer client per fornire funzionalità di accesso, disconnessione, Single Sign-On (SSO) e altre. Il browser client deve quindi essere configurato in modo da accettare i cookie. I cookie usati per l'autenticazione sono sempre cookie di sessioni HTTPS (Secure Hypertext Transfer Protocol) scritti per il server di origine. Se il browser client non è configurato per abilitare questi cookie, ADFS non potrà funzionare correttamente. I cookie permanenti vengono usati per mantenere la selezione del provider di attestazioni effettuata dall'utente. È possibile disabilitarli usando un'impostazione di configurazione nel file di configurazione relativo alle pagine di accesso di ADFS.  
  
Per motivi di sicurezza, è richiesto il supporto per TLS/SSL.  
  
## <a name="network-requirements"></a>Requisiti di rete  
La configurazione appropriata dei seguenti servizi di rete è fondamentale per la corretta distribuzione di AD FS nell'organizzazione.  
  
### <a name="tcpip-network-connectivity"></a>Connettività di rete TCP/IP  
Per il funzionamento di AD FS, deve esistere una connettività di rete TCP/IP tra il client. un controller di dominio; e i computer che ospitano il Servizio federativo, il Proxy servizio federativo (quando viene usato) e il Agente Web ADFS.  
  
### <a name="dns"></a>DNS  
Il servizio di rete primario fondamentale per il funzionamento di AD FS, AD eccezione di Active Directory Domain Services (AD DS), è Domain Name System (DNS). La distribuzione del DNS consente agli utenti di usare nomi di computer descrittivi e facili da ricordare per connettersi a computer e altre risorse sulle reti IP.  
  
 Windows Server 2008 utilizza DNS per la risoluzione dei nomi anziché la risoluzione dei nomi NetBIOS di Windows Internet Name Service (WINS) utilizzata nelle reti basate su Windows NT 4.0. È comunque possibile usare WINS per le applicazioni che lo richiedono. Servizi di dominio Active Directory e AD FS richiedono tuttavia la risoluzione dei nomi DNS.  
  
Il processo di configurazione del DNS per il supporto di AD FS varia a seconda che:  
  
-   L'organizzazione ha già di un'infrastruttura DNS esistente. Nella maggior parte degli scenari, il DNS è già configurato in tutta la rete per consentire ai client Web browser della rete aziendale di accedere a Internet. Poiché l'accesso a Internet e la risoluzione dei nomi sono requisiti di AD FS, si presuppone che questa infrastruttura sia attiva per la distribuzione di AD FS.  
  
-   Si prevede di aggiungere un server federativo alla rete aziendale. Allo scopo di autenticare gli utenti sulla rete aziendale, è necessario configurare i server DNS interni nella foresta della rete aziendale in modo che restituiscano il CNAME del server interno che esegue il Servizio federativo. Per altre informazioni, vedere [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   Si prevede di aggiungere un proxy server federativo alla rete perimetrale. Quando si desidera autenticare gli account utente che si trovano nella rete aziendale dell'organizzazione partner identità, è necessario configurare i server DNS interni nella foresta della rete aziendale per restituire il record CNAME del proxy server federativo interno. Per informazioni su come configurare DNS per supportare l'aggiunta di proxy server federativi, vedere [requisiti di risoluzione dei nomi per i proxy server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   Si configura il DNS per un ambiente di testing. Se si prevede di usare AD FS in un ambiente di testing in cui nessun server DNS radice è autorevole, è probabile che sia necessario configurare i server d'inoltri DNS in modo che le query sui nomi tra due o più foreste vengano trasmesse in modo appropriato. Per informazioni generali su come configurare un ambiente di testing AD FS, vedere [ad FS istruzioni dettagliate e guide](https://go.microsoft.com/fwlink/?LinkId=180357).  
  
## <a name="attribute-store-requirements"></a>Requisiti dell'archivio attributi  
ADFS richiede almeno un archivio di attributi da utilizzare per l'autenticazione degli utenti e l'estrazione delle attestazioni di sicurezza per gli utenti. Per un elenco di archivi attributi che supportano ADFS, vedere [Ruolo degli archivi attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md) nella Guida alla progettazione di ADFS.  
  
> [!NOTE]  
> Per impostazione predefinita, ADFS crea automaticamente un archivio attributi di Active Directory.  
  
I requisiti dell'archivio attributi dipendono dal ruolo svolto dall'organizzazione, cioè come partner account (che ospita gli utenti federati) o come partner risorse (che ospita l'applicazione federata).  
  
### <a name="adds"></a>AD DS  
Per il corretto funzionamento di AD FS, i controller di dominio nell'organizzazione partner account o nell'organizzazione partner risorse devono eseguire Windows Server 2003 SP1, Windows Server 2003 R2, Windows Server 2008 o Windows Server 2012.  
  
Quando si installa e si configura ADFS in un computer appartenente a un dominio, l'archivio account utente di Active Directory di tale dominio sarà disponibile come archivio attributi selezionabile.  
  
> [!IMPORTANT]  
> Poiché AD FS richiede l'installazione di Internet Information Services (IIS), è consigliabile non installare il software di AD FS in un controller di dominio in un ambiente di produzione per motivi di sicurezza. Questa configurazione è tuttavia supportata dal Supporto tecnico Microsoft.  
  
#### <a name="schema-requirements"></a>Requisiti dello schema  
AD FS non richiede modifiche dello schema o modifiche a livello funzionale per servizi di dominio Active Directory.  
  
#### <a name="functional-level-requirements"></a>Requisiti a livello funzionale  
Il corretto funzionamento della maggior parte delle funzionalità di ADFS non richiede modifiche a livello funzionale per Servizi di dominio Active Directory. Tuttavia, per il corretto funzionamento dell'autenticazione del certificato client, se il certificato è mappato in modo esplicito a un account utente in Servizi di dominio Active Directory, sarà necessario il livello di funzionalità del dominio di Windows Server 2008 o superiore.  
  
#### <a name="service-account-requirements"></a>Requisiti dell'account del servizio  
Se si crea una server farm federativa, è prima di tutto necessario creare un account del servizio dedicato basato sul dominio in Servizi di dominio Active Directory che possa essere usato dal Servizio federativo. In seguito si configurerà ogni server federativo della farm per l'uso di questo account. Per altre informazioni su come eseguire questa operazione, vedere [Configurare manualmente un account del servizio per una server farm federativa](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) nella Guida alla distribuzione di ADFS.  
  
### <a name="ldap"></a>LDAP  
Quando si usano altri archivi attributi basati su LDAP (Lightweight Directory Access Protocol), è necessario connettersi a un server LDAP che supporti l'autenticazione integrata di Windows. Anche la stringa di connessione LDAP deve essere scritta nel formato di URL LDAP, come descritto nella RFC 2255.  
  
### <a name="sql-server"></a>SQL Server  
Per il corretto funzionamento di AD FS, i computer che ospitano l'archivio di attributi del server Structured Query Language (SQL) devono eseguire Microsoft SQL Server 2005 o SQL Server 2008. Quando si usano archivi attributi basati su SQL, occorre configurare anche una stringa di connessione.  
  
### <a name="custom-attribute-stores"></a>Archivi attributi personalizzati  
Per abilitare scenari avanzati, è possibile sviluppare archivi attributi personalizzati. Il linguaggio dei criteri integrato in ADFS consente di fare riferimento ad archivi attributi personalizzati per il miglioramento di uno qualsiasi degli scenari seguenti:  
  
-   Creazione di attestazioni per un utente autenticato localmente  
  
-   Integrazione di attestazioni per un utente autenticato esternamente  
  
-   Autorizzazione di un utente a ottenere un token  
  
-   Autorizzazione di un servizio a ottenere un token sul comportamento di un utente  
  
Quando si usa un archivio attributi personalizzato, può essere necessario configurare anche una stringa di connessione. In questo caso, è possibile immettere codice personalizzato per abilitare una connessione all'archivio attributi personalizzato. La stringa di connessione, in questo caso, è un set di coppie nome/valore che vengono interpretate come un'implementazione dell'archivio attributi personalizzato da parte dello sviluppatore.  
  
Per altre informazioni sullo sviluppo e sull'uso degli archivi di attributi personalizzati, vedere [Cenni preliminari sull'archivio attributi](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="application-requirements"></a>Requisiti delle applicazioni  
I server federativi possono comunicare con le applicazioni federative, ad esempio le applicazioni in grado di riconoscere attestazioni, e proteggerle.  
  
## <a name="authentication-requirements"></a>Requisiti dell'autenticazione  
AD FS si integra naturalmente con l'autenticazione di Windows esistente, ad esempio autenticazione Kerberos, NTLM, smart card e certificati lato client X. 509 v3. I server federativi usano l'autenticazione Kerberos standard per autenticare un utente a un dominio. I client possono autenticarsi usando l'autenticazione basata su moduli, l'autenticazione con smart card e l'autenticazione integrata di Windows, a seconda di come è stata configurata l'autenticazione.  
  
Il ruolo del proxy server federativo di ADFS rende possibile uno scenario in cui l'utente si autentica esternamente con l'autenticazione client SSL. È anche possibile configurare il ruolo del server federativo in modo che richieda l'autenticazione client SSL, anche se di solito l'esperienza utente ottimale si ottiene configurando il server federativo di account per l'autenticazione integrata di Windows. In questo caso, ADFS non ha alcun controllo sul tipo di credenziali usate dall'utente per l'accesso al desktop Windows.  
  
### <a name="smart-card-logon"></a>Accesso con smart card  
Sebbene AD FS possibile imporre il tipo di credenziali utilizzato per l'autenticazione (password, autenticazione client SSL o autenticazione integrata di Windows), non applica direttamente l'autenticazione con smart card. Pertanto, AD FS non fornisce un'interfaccia utente sul lato client per ottenere le credenziali del codice PIN (Personal Identification Number) smart card. Ciò è dovuto al fatto che i client basati su Windows non forniscono intenzionalmente i dettagli delle credenziali utente ai server federativi o ai server Web.  
  
### <a name="smart-card-authentication"></a>Autenticazione con smart card  
Per l'autenticazione con smart card viene usato il protocollo Kerberos per l'autenticazione a un server federativo di account. Non è possibile estendere AD FS per aggiungere nuovi metodi di autenticazione. Per il concatenamento a una radice attendibile nel computer client non è richiesto il certificato nella smart card. L'utilizzo di un certificato basato su smart card con AD FS richiede le condizioni seguenti:  
  
-   Il lettore e il provider del servizio di crittografia (CSP) della smart card devono essere in funzione nel computer in cui risiede il browser.  
  
-   Il certificato della smart card deve essere concatenato a una radice attendibile nel server federativo di account e nel proxy server federativo di account.  
  
-   Il certificato deve essere mappato all'account utente in Servizi di dominio Active Directory con uno dei metodi seguenti:  
  
    -   Il nome soggetto del certificato corrisponde al nome distinto LDAP di un account utente in Servizi di dominio Active Directory.  
  
    -   L'estensione relativa al nome alternativo soggetto (SubjectAltName) del certificato ha il nome dell'entità utente (UPN) di un account utente in Servizi di dominio Active Directory.  
  
Per supportare requisiti di complessità dell'autenticazione specifici in alcuni scenari, è anche possibile configurare ADFS per la creazione di un'attestazione che indichi in che modo è stato autenticato l'utente. Un relying party può quindi usare questa attestazione per prendere decisioni di autorizzazione.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
