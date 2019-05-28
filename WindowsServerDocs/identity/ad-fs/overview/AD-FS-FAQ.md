---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: Domande frequenti su AD FS 2016
description: Domande frequenti per AD FS 2016
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdd31a8b7c2c6ef87d1d22d901b5c6ca69b5c70d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188724"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Domande frequenti (FAQ) di ADFS


La documentazione seguente è un indirizzo di base ad alcune domande frequenti in materia di Active Directory Federation Services.  Il documento è stata suddivisa in gruppi in base al tipo di domanda.

## <a name="deployment"></a>Distribuzione

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Come posso aggiornamento o la migrazione da versioni precedenti di AD FS
È possibile eseguire l'aggiornamento di ADFS usando uno dei seguenti:


- Windows Server 2012 R2 AD ADFS per Windows Server 2016 AD FS
    - [Aggiornamento ad AD FS in Windows Server 2016 con un database interno di Windows](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Aggiornamento ad AD FS in Windows Server 2016 con un database SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD ADFS per Windows Server 2012 R2 AD ADFS
    - [Eseguire la migrazione ad AD FS in Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 per Windows Server 2012 AD ADFS
    - [Eseguire la migrazione ad AD FS in Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x ad AD FS 2.0
    - [Eseguire l'aggiornamento da AD FS 1.x ad AD FS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Se è necessario eseguire l'aggiornamento da AD FS 2.0 o 2.1 (Windows Server 2008 R2 o Windows Server 2012), è necessario usare gli script in arrivo (situati nella C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Perché richiede un riavvio del server di installazione di AD FS?

È stato aggiunto il supporto di HTTP/2 in Windows Server 2016, ma HTTP/2 non può essere utilizzato per l'autenticazione del certificato client.  Perché molti scenari di ADFS rendono uso dell'autenticazione del certificato client e un numero significativo di i client non supportano ritentare richiede mediante HTTP/1.1, ADFS farm configuration nuovamente consente di configurare le impostazioni HTTP del server locale al protocollo HTTP/1.1.  Ciò richiede un riavvio del server.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>Usa i server Windows 2016 WAP per pubblicare la farm AD FS a internet senza aggiornare la farm AD FS di back-end supportata?
Sì, questa configurazione è supportata, ma non nuove funzionalità di AD FS 2016 potrebbe essere supportate in questa configurazione.  Questa configurazione è concepita per essere temporaneo durante la fase di migrazione da AD FS 2012 R2 ad AD FS 2016 e non deve essere distribuita per lunghi periodi di tempo.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>È possibile distribuire AD FS per Office 365 senza la pubblicazione di un proxy a Office 365?
Sì, questa è supportata. Tuttavia, come effetto collaterale

1. È necessario gestire manualmente l'aggiornamento di certificati firma del token, perché Azure AD non sarà in grado di accedere ai metadati di federazione. Per altre informazioni sul certificato di firma di token di aggiornamento manuale leggere [rinnovare i certificati di federazione per Office 365 e Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. Non sarà in grado di sfruttare flussi di autenticazione legacy (ad esempio ExO proxy Authentication flow)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>Quali sono i requisiti di bilanciamento del carico per i server AD FS e WAP?

ADFS è un sistema senza stato. Di conseguenza, il bilanciamento del carico è abbastanza semplice per gli account di accesso. Ecco le indicazioni principali per i sistemi di bilanciamento del carico.


- Servizi di bilanciamento del carico non devono essere configurati con affinità IP di. Ciò può inserire preoccupata carico su un subset dei server in determinati scenari di Exchange Online.
- Servizi di bilanciamento del carico non deve terminare le connessioni HTTPS e reinizializzare una nuova connessione al server ad FS.
- Servizi di bilanciamento del carico è necessario assicurarsi che l'indirizzo IP deve essere tradotta come IP di origine nel pacchetto HTTP durante l'invio ad ad FS. Nel caso in cui un servizio di bilanciamento del carico non è possibile inviare l'indirizzo IP di origine nel pacchetto HTTP, servizio di bilanciamento del carico deve aggiungere (o accodamento in caso di esistente) l'indirizzo IP per l'intestazione x-forwarded-for. Ciò è obbligatorio per la gestione corretta di determinati IP relative funzionalità (IP da escludere, intelligente il blocco della Extranet...) e potrebbe causare una riduzione della protezione se non è configurato correttamente.
- Servizi di bilanciamento del carico deve supportare SNI. Nell'evento non esiste, verificare che AD FS sia configurato per creare associazioni HTTPS per gestire i client in grado di supportare SNI non.
- Servizi di bilanciamento del carico deve usare l'endpoint di probe di integrità di AD FS HTTP per rilevare se i server AD FS o WAP siano in esecuzione ed escluderli se un messaggio 200 OK non viene restituito.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>Quali configurazioni a più foreste sono supportati da AD FS?

ADFS supporta più la configurazione a più foreste e si basa su Active Directory Domain Services trust rete sottostante per autenticare gli utenti in più aree di autenticazione trusted. Trust tra foreste bidirezionale 2 è fortemente consigliabile poiché si tratta di un programma di installazione più semplice per assicurarsi che il sottosistema di trust funzioni correttamente senza problemi. Inoltre,

- In caso di un trust tra foreste unidirezionale, ad esempio un insieme di strutture di rete Perimetrale che contiene le identità del partner, è consigliabile la distribuzione di ADFS nella foresta corp e considerando l'insieme di strutture di rete Perimetrale come un altro trust del provider di attestazioni locale connessa tramite LDAP. In questo caso autenticazione integrata di Windows non funziona per gli utenti dell'insieme di strutture di rete Perimetrale e saranno necessarie per eseguire auth Password perché questo è l'unico meccanismo supportato per l'accesso LDAP. Nel caso in cui è possibile adottare questa opzione, è necessario configurare un altro ad FS dell'insieme di strutture di rete Perimetrale e aggiungerle come attendibilità Provider di attestazioni in di ADFS nella foresta corp. Gli utenti dovranno Home individuazione dell'area di autenticazione, ma sia autenticazione integrata di Windows e autenticazione Password funzioneranno. Apportare le modifiche appropriate le regole di rilascio in ADFS nella foresta DMZ poiché ADFS nella foresta corp non saranno in grado di ottenere le informazioni utente aggiuntive sull'utente dall'insieme di strutture di rete Perimetrale.
- Mentre i trust a livello di dominio sono supportati e possono funzionare, è fortemente consigliabile spostare in un modello di trust a livello di foresta. Inoltre, è necessario garantire che UPN routing e la risoluzione dei nomi NETBIOS dei nomi necessario lavorare in modo accurato.



## <a name="design"></a>Progettazione

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quali provider di multi-factor authentication di terze parti sono disponibili per AD FS?
Di seguito è un elenco di provider di terze parti che sono compatibili con.  È sempre possibile dei provider disponibili che non si sa sulle e verrà aggiornato l'elenco e spiega in merito.

- [Gemalto identità e servizi di sicurezza](http://www.gemalto.com/identity)
- [servizio di autenticazione Enterprise inWebo](http://www.inwebo.com/)
- [People MFA API connector account di accesso](https://www.loginpeople.com)
- [Agente di autenticazione RSA SecurID per Microsoft Active Directory Federation Services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [SafeNet Authentication Service (SAS) agente per AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Swisscom Mobile ID Authentication Service](http://swisscom.ch/mid)
- [Convalida di Symantec e ID servizio di protezione (VIP)](http://www.symantec.com/vip-authentication-service)

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>I proxy di terze parti sono supportati con AD FS?
Sì, i proxy di terze parti possono essere posizionati davanti il Proxy applicazione Web, ma i proxy di terze parti deve supportare le [protocollo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) da usare al posto di Proxy applicazione Web.

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>Il proxy di terze parti sono disponibili per AD FS che supportano MS-ADFSPIP?

Di seguito è un elenco di provider di terze parti che sono compatibili con.  È sempre possibile dei provider disponibili che non si sa sulle e verrà aggiornato l'elenco e spiega in merito.

- [F5 Access Policy Manager](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Dove è la pianificazione della capacità del foglio di calcolo di ridimensionamento per AD FS 2016?
È possibile scaricare la versione di AD FS 2016 del foglio di calcolo [qui](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Questo è anche utilizzabile per AD FS in Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Come è possibile garantire il supporto per i server AD FS e WAP requisiti ATP di Apple?

Apple ha rilasciato una serie di requisiti chiamato App Transport Security (ATS) che potrebbe influire negativamente le chiamate dalle App iOS che eseguono l'autenticazione ad AD FS.  È possibile assicurarsi di AD FS e i server WAP conforme assicurandosi che supportano il [requisiti per la connessione con ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
In particolare, è necessario verificare che i server AD FS e WAP supportano TLS 1.2 e che il pacchetto di crittografia negoziato della connessione TLS supporterà la funzionalità PFS.

È possibile abilitare e disabilitare le versioni di SSL 2.0 e 3.0 e TLS 1.0, 1.1 e 1.2 usando [gestire i protocolli SSL in AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Per assicurarsi di AD FS e WAP server negoziano soli pacchetti di crittografia TLS che supportano ATP, è possibile disabilitare tutti i pacchetti di crittografia che non sono presenti il [elenco di pacchetti di crittografia conformi ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  A questo scopo, usare il [i cmdlet di Windows PowerShell di TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Sviluppatore

### <a name="when-generating-an-idtoken-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-idtoken"></a>Quando si genera un token ID con ad FS per un utente autenticato in Active Directory, come viene l'attestazione "sub" generato nell'id_token?
Il valore dell'attestazione "sub" è l'hash dell'ID client + valore di attestazione ancoraggio.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>Che cos'è la durata del token di aggiornamento token/accesso quando l'utente accede tramite un trust del provider di attestazioni remota tramite WS-Fed, SAML-P?
La durata dei token di aggiornamento sarà la durata del token di ad FS è stata ricevuta dall'attendibilità provider di attestazioni remoto. La durata del token di accesso sarà la durata del token della relying party per il quale è in corso emessi token di accesso.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>È necessario restituire gli ambiti di profilo e indirizzo di posta elettronica anche oltre l'ambito OpenId. È possibile ottenere ulteriori informazioni sull'utilizzo degli ambiti? Come eseguire questa operazione in AD FS?
È possibile utilizzare token ID personalizzati per aggiungere le informazioni pertinenti nell'id_token stesso. Per altre informazioni, vedere l'articolo [personalizzare le attestazioni da generare nel token ID](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>Come rilasciare i BLOB json in token JWT?
Un speciale ValueType ("http://www.w3.org/2001/XMLSchema#json") e carattere di escape character(\x22) per questo è stato aggiunto in AD FS 2016. Vedere l'esempio riportato di seguito per la regola di rilascio e anche l'output finale dal token di accesso.

Esempio di regola di rilascio:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Attestazioni rilasciate nel token di accesso:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>È possibile passare il valore di risorsa come parte del valore di ambito, ad esempio come le richieste vengono eseguite da Azure AD?
Con AD FS 2019 Server, è ora possibile passare il valore della risorsa incorporato nel parametro di ambito. Il parametro di ambito ora può essere organizzato in un elenco separato da spazi in cui ogni voce è struttura come risorsa/ambito. Per esempio  
**< crea una richiesta di esempio valida >**

### <a name="does-ad-fs-support-pkce-extension"></a>ADFS supporta estensione PKCE?
AD FS nel Server 2019 supporta chiave di prova per Code Exchange (PKCE) per il flusso di concessione del codice di autorizzazione OAuth

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>Quali ambiti consentiti sono supportati da AD FS?
- aza - se si usa [estensioni del protocollo OAuth 2.0 per i client di Service Broker](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) e, se il parametro di ambito contiene l'ambito "aza", il server rilascia un nuovo token di aggiornamento primari e lo imposta nel campo della risposta, nonché impostazione token di aggiornamento il campo refresh_token_expires_in alla durata del token di aggiornamento primari nuovo se verrà applicato.
- openid - consente l'applicazione richiedere l'uso del protocollo di autorizzazione OpenID Connect.
- logon_cert - l'ambito logon_cert consente a un'applicazione per richiedere certificati di accesso, che può essere utilizzato per gli utenti autenticati in modo interattivo. Il server AD FS omette il parametro di token di accesso dalla risposta e fornisce una catena di certificati CMS con codifica base64 o una risposta di infrastruttura a chiave pubblica completa CMC. Sono disponibili altri dettagli [qui](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation - l'ambito user_impersonation è necessario per richiedere un token di accesso di on-behalf-of di AD FS. Per informazioni su come usare questo ambito, vedere [creare un'applicazione a più livelli con On-Behalf-Of (OBO) tramite OAuth con AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert - l'ambito vpn_cert consente a un'applicazione per richiedere certificati VPN, che può essere usato per stabilire le connessioni VPN mediante l'autenticazione EAP-TLS. Ciò non è più supportata.
- messaggio di posta elettronica - consente l'applicazione richiedere attestazioni di posta elettronica per l'utente connesso. Ciò non è più supportata. 
- profilo - consente di richiesta profilo applicazione correlate le attestazioni per l'utente di accesso. Ciò non è più supportata. 


## <a name="operations"></a>Operazioni

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Come è possibile sostituire il certificato SSL per AD FS?
Il certificato SSL di AD FS non è quello utilizzato per il certificato di comunicazioni di servizio AD FS trovato nello snap-in Gestione ADFS.  Per modificare il certificato SSL di AD FS, è necessario usare PowerShell. Seguire le indicazioni fornite nell'articolo seguente:

[Gestione dei certificati SSL in AD FS e WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Come è possibile abilitare o disabilitare le impostazioni di TLS/SSL per AD FS
Per disabilitare o abilitare i protocolli SSL e pacchetti di crittografia, usare il comando seguente:

[Gestire i protocolli SSL in AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Il certificato SSL del proxy è necessario essere lo stesso certificato SSL di AD FS?  
Usare le linee guida seguenti per quanto riguarda il certificato SSL del proxy e il certificato SSL di AD FS:


- Se viene utilizzato il proxy per le richieste proxy ADFS che utilizzano l'autenticazione integrata di Windows, il certificato SSL del proxy devono corrispondere il (utilizzare la stessa chiave) il certificato SSL del server federativo
- Se la proprietà di AD FS "ExtendedProtectionTokenCheck" è abilitata (l'impostazione predefinita in AD FS), il certificato SSL del proxy deve essere lo stesso (utilizzare la stessa chiave) come il certificato SSL del server federativo
- In caso contrario, il certificato SSL del proxy può avere una chiave diversa dal certificato SSL di AD FS, ma devono soddisfare lo stesso [requisiti](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>Come si configura prompt = il comportamento di account di accesso per AD FS?
Per informazioni su come configurare i prompt = login, vedere [Active Directory Federation Services prompt = account di accesso parametro supporto](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>Come si può modificare l'account del servizio AD FS?
Per modificare l'account del servizio AD FS, seguire le istruzioni usando la casella degli strumenti di AD FS [modulo di Powershell di Service Account](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule). 

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Come si configurano i browser da usare l'autenticazione integrata Windows (WIA) con AD FS?

Per informazioni su come configurare i browser, vedere [configurare i browser per usare l'autenticazione integrata Windows (WIA) con AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-trun-off-browserssoenabled"></a>È possibile disattivare BrowserSsoEnabled trun?
Se non si dispone di criteri di controllo di accesso basati su dispositivo in ad FS o Windows Hello per la registrazione certificato Business Usa ad FS; è possibile disattivare BrowserSsoEnabled. BrowserSsoEnabled consente ad FS raccogliere un PRT (Token di aggiornamento primari) dai client che contiene informazioni sul dispositivo. L'autenticazione di ADFS senza che il dispositivo non funziona nei dispositivi Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>Per quanto tempo sono validi i token di AD FS?

Spesso questa domanda significa 'quanto tempo si gli utenti ottengono single sign-on (SSO) senza dover immettere nuove credenziali e come è possibile l'amministratore di controllare che?'  Questo comportamento e le impostazioni di configurazione che controllano, sono descritti nell'articolo [AD ADFS Single Sign-On Settings](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

La durata predefinita dei vari token e i cookie è elencata di seguito (nonché i parametri che determinano la durata):

**Dispositivi registrati**

- Cookie PRT e SSO: 90 giorni di massimi, disciplinato dalle PSSOLifeTimeMins. (Dispositivo fornito viene usato almeno ogni 14 giorni, che viene controllata da DeviceUsageWindow)

- Token di aggiornamento: calcolato in base a quello precedente per ottenere un comportamento coerente

- access_token: 1 ora per impostazione predefinita, in base alla relying party

- id_token: stesso come token di accesso

**Dispositivi non registrati**

- Cookie SSO: 8 ore per impostazione predefinita, è disciplinato dalle SSOLifetimeMins.  Quando è abilitato Mantieni l'accesso (kmsi), valore predefinito è 24 ore e configurabile tramite KMSILifetimeMins.


- Token di aggiornamento: 8 ore per impostazione predefinita. 24 ore con KMSI abilitata

- access_token: 1 ora per impostazione predefinita, in base alla relying party

- id_token: stesso come token di accesso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>ADFS supporta HTTP Strict Transport Security (HSTS)?  

HTTP Strict Transport Security (HSTS) è un meccanismo di criteri di sicurezza web che consente di attenuare gli attacchi di protocollo downgrade e Hijack del cookie per i servizi che hanno endpoint HTTP e HTTPS. Consente i server web dichiarare che i browser web (o altri agenti utente a soddisfare) devono solo interagire con questa usando HTTPS e mai tramite il protocollo HTTP.

Tutti gli endpoint AD FS per il traffico di autenticazione web aperti esclusivamente tramite HTTPS.  Di conseguenza, AD FS in modo efficace consente di ridurre le minacce che fornisce il meccanismo di criterio HTTP Strict Transport Security (per impostazione predefinita non vi è alcun effettua il downgrade a HTTP poiché non sono presenti listener HTTP). Inoltre, ADFS impedisce l'invio a un altro server con l'endpoint del protocollo HTTP, contrassegnando tutti i cookie con il flag di protezione i cookie.

Pertanto, implementare HSTS in un server AD FS non è necessario perché non è possibile eseguire il downgrade.  Per motivi di conformità, i server AD FS soddisfano questi requisiti, perché non possono mai usare HTTP e tutti i cookie sono contrassegnata come sicuri.

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-inoltrati--indirizzo ip del client non contiene l'indirizzo IP del client, ma contiene IP del firewall davanti al proxy. Dove si reperiscono l'indirizzo IP corretto del client?
È consigliabile non eseguire la terminazione SSL prima di WAP. Nel caso in cui viene eseguita la terminazione SSL davanti WAP, X-ms-inoltrati--ip client conterrà l'indirizzo IP del dispositivo di rete davanti WAP. Ecco che una breve descrizione dell'indirizzo IP diversi correlate le attestazioni che sono supportate da AD FS:
 - x-ms-client-ip: Indirizzo IP di dispositivi connessi al servizio token di sicurezza di rete.  Nel caso di una richiesta extranet contiene sempre l'indirizzo IP di WAP.
 - x-ms-inoltrati-client-ip: Attestazione multivalore che conterrà tutti i valori inoltrati ad ad FS da Exchange Online e l'indirizzo IP del dispositivo che connesso a WAP.
 - Userip: Per le richieste extranet questa attestazione contiene il valore di x-ms-inoltrati--indirizzo ip del client.  Per le richieste di rete intranet, questa attestazione conterrà lo stesso valore di x-ms-client-ip.

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Sto cercando di ottenere attestazioni aggiuntive su endpoint delle informazioni utente, ma la restituzione del solo oggetto. Come posso ottenere attestazioni aggiuntive?
L'endpoint userinfo di ad FS restituisce sempre l'attestazione dell'oggetto come specificato negli standard OpenID. ADFS non forniscono attestazioni aggiuntive richieste tramite l'endpoint UserInfo. Se è necessario ulteriori attestazioni nel token ID, fare riferimento a [token ID personalizzati in AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>Il motivo per cui vengono visualizzati numerosi errori 1021 nei server AD FS?
Questo evento viene registrato in genere per un accesso alle risorse non è valido in AD FS per la risorsa 00000003-0000-0000-c000-000000000000. Questo errore è causato da un comportamento errato del client che tenta di ottenere un token di accesso per il servizio di Azure AD Graph. Poiché la risorsa non è presente in AD FS, il risultato evento ID 1021 nei server AD FS. È possibile ignorare eventuali avvisi o errori per la risorsa 00000003-0000-0000-c000-000000000000 in AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Perché viene visualizzato un avviso di errore aggiungere l'account del servizio ADFS al gruppo Enterprise Admins chiave?
Questo gruppo viene creato solo in presenza di un Controller di dominio Windows 2016 con il ruolo FSMO PDC del dominio. Per risolvere l'errore, è possibile creare manualmente il gruppo e seguire la seguente per concedere l'autorizzazione necessaria dopo aver aggiunto l'account del servizio come membro del gruppo.
1.  Aprire **Utenti e computer di Active Directory**.
2.  **Fare doppio clic su** il nome di dominio nel riquadro di spostamento e **fare clic su** proprietà.
3.  **Fare clic su** sicurezza (se la scheda di sicurezza è manca, attivare caratteristiche avanzate dal menu Visualizza).
4.  **Fare clic su** avanzate. **Fare clic su** aggiungere. **Fare clic su** seleziona un'entità.
5.  Viene visualizzata la finestra di dialogo Seleziona utenti, Computer, Account del servizio o gruppo.  Immetti il nome dell'oggetto alla casella di testo selezionare, digitare gruppo Admin di chiave.  Fare clic su OK.
6.  Nell'articolo si applicano alla casella di riepilogo, selezionare **oggetti utente discendenti**.
7.  Usando la barra di scorrimento, scorrere fino alla fine della pagina e **fare clic su** Cancella tutto.
8.  Nel **delle proprietà** sezione, selezionare **Leggi msDS-KeyCredentialLink** e **scrittura msDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Il motivo per cui l'autenticazione moderna da dispositivi Android non riesce se il server non invia tutti i certificati intermedi nella catena, con il certificato SSL?

Gli utenti federati potrebbero verificarsi l'autenticazione ad Azure AD per le app che usano l'errore della libreria ADAL per Android. L'app Ottiene un **AuthenticationException** quando tenta di visualizzare la pagina di accesso. In chrome AD FS pagina di accesso potrebbe essere segnalata come non sicura.

Android - tra tutti i dispositivi - e tutte le versioni non supporta il download di certificati aggiuntivi dal **authorityInformationAccess** campo del certificato. Ciò vale anche il browser Chrome. Qualsiasi certificato di autenticazione Server in cui Manca i certificati intermedi verrà generato questo errore se l'intera catena di certificati non viene passata da AD FS.

Una soluzione appropriata al problema consiste nel configurare i server AD FS e WAP per inviare i certificati intermedi insieme al certificato SSL.

Quando esportazione del certificato SSL, da un computer, per essere importato in un archivio personale, di AD FS del computer e i server WAP, assicurarsi di esportare la chiave privata e selezionare **scambio informazioni personali - PKCS #12**.

È importante che la casella di controllo per **se possibile, Includi tutti i certificati nel percorso certificazione** è selezionata, nonché **esportare tutte le proprietà estese**.  

Eseguire certlm. msc sul server di Windows e importare il *. File PFX nell'archivio certificati personale del Computer. In questo modo il server per passare l'intera catena di certificati per la libreria ADAL.

>[!NOTE]
> Il certificato di archivio di rete i bilanciamenti del carico deve essere aggiornato per includere l'intera catena di certificati se presente

### <a name="does-ad-fs-support-head-requests"></a>ADFS supporta le richieste HEAD?
ADFS non supporta le richieste HEAD.  Le applicazioni non devono utilizzare le richieste HEAD eseguito negli endpoint ADFS.  È possibile che le risposte di errore HTTP che sono impreviste o posticipata.  Inoltre, è possibile visualizzare gli eventi di errore imprevisto nel registro eventi di ADFS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Perché viene non visualizzato un token di aggiornamento quando ho eseguito l'accesso con un provider di identità remoto?
Un token di aggiornamento non viene generato se il token rilasciato dal provider di identità ha un validty di minore di 1 ora. Per garantire che viene rilasciato un token di aggiornamento, aumentare la validità del token rilasciato dal provider di identità a più di 1 ora.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>È disponibile alcun modo per modificare l'algoritmo di crittografia dei token di relying Party?
Per impostazione predefinita la crittografia di token relying Party è impostata su AES256 e non può essere modificato in qualsiasi altro valore.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>In una farm in modalità mista, viene visualizzato errore durante il tentativo di impostare il nuovo certificato SSL utilizzando Set-AdfsSslCertificate-identificazione personale. Come è possibile aggiornare il certificato SSL in modalità mista farm AD FS?
Nei server WAP è comunque possibile usare Set-WebApplicationProxySslCertificate. Nei server ad FS, è necessario usare netsh. Seguire i passaggi come indicato di seguito:

1. Selezionare i subset di server ad FS 2016 per la manutenzione (ad esempio, rimuovere dal servizio di bilanciamento del carico)
2. Nei server selezionati in #1, importare il nuovo certificato tramite MMC
3. Eliminare i certificati esistenti

    a. netsh http delete sslcert hostnameport=fs.contoso.com:443 b. netsh http delete sslcert hostnameport=localhost:443 c. netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  Aggiungere il nuovo certificato

    a. netsh http aggiungere sslcert hostnameport=fs.contoso.com:443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

    b. netsh http aggiungere sslcert hostnameport = localhost:443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

    c. netsh http aggiungere sslcert hostnameport=fs.contoso.com:49443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

5. Riavviare il servizio ad FS nel server selezionato
6. Rimuovere i subset dei server WAP per la manutenzione
7. Per i server WAP selezionati, importare il nuovo certificato tramite MMC
8. Impostare il nuovo certificato in WAP con cmdlet

    a. Set-WebApplicationProxySslCertificate-identificazione personale "CERTTHUMBPRINT"

9. Riavviare il servizio per i server WAP selezionati
10. Collocare i server AD FS e WAP selezionati nuovamente nell'ambiente di produzione.

Eseguire l'aggiornamento nel resto di AD FS e WAP in modo analogo.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>ADFS è supportato quando i server Proxy applicazione Web (WAP) si trovano dietro Firewall(WAF) dell'applicazione Web di Azure?
Server applicazioni Web e ad FS supporta qualsiasi firewall che non esegue la terminazione SSL nell'endpoint. Inoltre, i server ad FS/WAP hanno meccanismi integrati per impedire gli attacchi web più comuni, ad esempio il cross-site scripting, proxy ad FS e soddisfare tutti i requisiti definiti dal [protocollo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).
