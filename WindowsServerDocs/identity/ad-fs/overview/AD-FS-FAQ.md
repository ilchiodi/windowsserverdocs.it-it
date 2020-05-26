---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: Domande frequenti su AD FS
description: Domande frequenti per AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/29/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 947c34e6c3a3b9a26a225221bbf29e46343b25df
ms.sourcegitcommit: f22e4d67dd2a153816acf8355e50319dbffc5acf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/18/2020
ms.locfileid: "83546562"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Domande frequenti su AD FS


Di seguito sono riportate le domande frequenti relative ad Active Directory Federation Services (AD FS).  Il documento è stato strutturato in gruppi, in base al tipo di domanda.

## <a name="deployment"></a>Distribuzione

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Come posso eseguire l'aggiornamento o la migrazione da versioni precedenti di AD FS?
Puoi aggiornare AD FS procedendo in uno dei seguenti modi:


- Da AD FS per Windows Server 2012 R2 ad AD FS per Windows Server 2016 o versioni successive. Tieni presente che la metodologia è la stessa se esegui l'aggiornamento da AD FS per Windows Server 2016 ad AD FS per Windows Server 2019. 
    - [Aggiornamento ad AD FS in Windows Server 2016 con un database interno di Windows](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Aggiornamento ad AD FS in Windows Server 2016 con un database SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Da AD FS per Windows Server 2012 ad AD FS per Windows Server 2012 R2
    - [Eseguire la migrazione ad AD FS in Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- Da AD FS 2.0 ad AD FS per Windows Server 2012
    - [Eseguire la migrazione ad AD FS in Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- Da AD FS 1.x ad AD FS 2.0
    - [Eseguire l'aggiornamento da AD FS 1.x ad AD FS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Se hai necessità di eseguire l'aggiornamento da AD FS 2.0 o 2.1 (Windows Server 2008 R2 o Windows Server 2012), devi usare gli script incorporati (disponibili in C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Perché l'installazione di AD FS richiede un riavvio del server?

Il supporto per HTTP/2 è stato aggiunto in Windows Server 2016, ma HTTP/2 non può essere usato per l'autenticazione del certificato client.  Poiché molti scenari AD FS usano l'autenticazione del certificato client e un numero significativo di client non supporta la ripetizione delle richieste con HTTP/1.1, la configurazione della farm AD FS causa la riconfigurazione delle impostazioni HTTP del server locale su HTTP/1.1.  Questa operazione richiede un riavvio del server.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>È supportato l'uso di server WAP Windows 2016 per pubblicare la farm AD FS in Internet senza aggiornare la farm AD FS back-end?
Sì, questa configurazione è supportata, ma non consente l'uso delle nuove funzionalità di AD FS 2016.  Questa configurazione ha una funzione temporanea durante la fase di migrazione da AD FS 2012 R2 ad AD FS 2016 e non deve essere distribuita per periodi di tempo prolungati.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>È possibile distribuire AD FS per Office 365 senza pubblicare un proxy in Office 365?
Sì, è possibile. Tuttavia, come effetto collaterale

1. Dovrai gestire manualmente l'aggiornamento dei certificati per la firma di token perché Azure AD non sarà in grado di accedere ai metadati della federazione. Per altre informazioni sull'aggiornamento manuale del certificato per la firma di token, leggi [Rinnovare i certificati di federazione per Office 365 e Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. Non potrai sfruttare i flussi di autenticazione legacy, ad esempio il flusso di autenticazione proxy ExO

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>Quali sono i requisiti di bilanciamento del carico per i server AD FS e WAP?

AD FS è un sistema senza stato. Di conseguenza, il bilanciamento del carico è piuttosto semplice per gli account di accesso. Di seguito sono riportati i principali consigli da seguire relativamente ai sistemi di bilanciamento del carico.


- I servizi di bilanciamento del carico non DOVREBBERO essere configurati con l'affinità IP. Questo può generare un carico eccessivo su un subset dei server in determinati scenari di Exchange Online.
- I servizi di bilanciamento del carico non DEVONO terminare le connessioni HTTPS e riavviare una nuova connessione al server ADFS.
- I servizi di bilanciamento del carico DOVREBBERO garantire la conversione dell'indirizzo IP di connessione come IP di origine nel pacchetto HTTP quando viene inviato ad ADFS. Nel caso in cui un servizio di bilanciamento del carico non sia in grado di inviare l'IP di origine nel pacchetto HTTP, il servizio DEVE aggiungere (o accodare se già presente) l'indirizzo IP all'intestazione x-forwarded-for. Questa operazione è necessaria per la corretta gestione di alcune funzionalità correlate all'IP (IP escluso, blocco intelligente Extranet e così via) e potrebbe incidere negativamente sulla sicurezza se non configurata correttamente.
- I servizi di bilanciamento del carico DOVREBBERO supportare SNI. In caso contrario, assicurati che AD FS sia configurato in modo da creare binding HTTPS per gestire i client che non supportano SNI.
- I servizi di bilanciamento del carico DOVREBBERO usare l'endpoint del probe di integrità HTTP AD FS per rilevare se i server AD FS o WAP sono operativi ed escluderli se non viene restituito 200 OK.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>Quali configurazioni multi-foresta sono supportate da AD FS?

AD FS supporta più configurazioni multi-foresta e si basa sulla rete di trust di Active Directory Domain Services sottostante per autenticare gli utenti in più aree di autenticazione attendibili. È consigliabile usare trust tra foreste bidirezionali perché si tratta di una configurazione più semplice da implementare per garantire il corretto funzionamento del sottosistema di trust ed evitare problemi. Inoltre,

- Nel caso di un trust tra foreste unidirezionale, ad esempio una foresta della rete perimetrale contenente identità partner, è consigliabile distribuire ADFS nella foresta aziendale e trattare la foresta della rete perimetrale come un altro trust del provider di attestazioni locale connesso tramite LDAP. In questo caso, l'autenticazione integrata di Windows non funzionerà per gli utenti della foresta della rete perimetrale, i quali dovranno eseguire l'autenticazione con password perché questo è l'unico meccanismo supportato per LDAP. Qualora risulti impossibile procedere in questo modo, dovrai configurare un'altra istanza di ADFS nella foresta della rete perimetrale e aggiungerla come trust del provider di attestazioni in ADFS nella foresta aziendale. Gli utenti dovranno eseguire l'individuazione dell'area di autenticazione principale, ma l'autenticazione integrata di Windows e l'autenticazione con password funzioneranno. Apporta le modifiche appropriate nelle regole di rilascio in ADFS nella foresta della rete perimetrale, in quanto ADFS nella foresta aziendale non sarà in grado di ottenere informazioni aggiuntive sull'utente da tale foresta.
- Anche se i trust a livello di dominio sono supportati e possono funzionare, è consigliabile passare a un modello di trust a livello di foresta. Inoltre, dovrai garantire il corretto funzionamento del routing UPN e della risoluzione dei nomi NETBIOS.

>[!NOTE]  
>Se viene usata l'autenticazione elettiva con una configurazione di trust bidirezionale, assicurati che all'utente chiamante venga concessa l'autorizzazione per "consentire l'autenticazione" per l'account del servizio di destinazione. 

### <a name="does-ad-fs-extranet-smart-lockout-support-ipv6"></a>Extranet Smart Lockout di AD FS supporta IPv6?
Sì, gli indirizzi IPv6 vengono considerati per posizioni note/sconosciute.


## <a name="design"></a>Progettazione

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quali provider di autenticazione a più fattori di terze parti sono disponibili per AD FS?
AD FS fornisce un meccanismo estensibile per l'integrazione dei provider di autenticazione a più fattori (MFA) di terze parti. A tale scopo, non è disponibile alcun programma di certificazione definito. Si presuppone che il fornitore abbia effettuato le convalide necessarie prima del rilascio. 

L'elenco dei fornitori che hanno informato Microsoft è pubblicato presso i [provider di autenticazione a più fattori (MFA) per AD FS](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).  Potrebbero essere sempre disponibili provider non ancora noti e Microsoft aggiornerà l'elenco non appena ne verrà a conoscenza.

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>I proxy di terze parti sono supportati con AD FS?
Sì, i proxy di terze parti possono essere posizionati di fronta a AD FS, ma qualsiasi proxy di terze parti deve supportare l'uso del [protocollo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) al posto del Proxy applicazione Web.

Di seguito è riportato un elenco di provider di terze parti di cui Microsoft è a conoscenza.  Potrebbero essere sempre disponibili provider non ancora noti e Microsoft aggiornerà l'elenco non appena ne verrà a conoscenza.

- [F5 Access Policy Manager](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Dov'è reperibile il foglio di calcolo per il dimensionamento della pianificazione della capacità per AD FS 2016?
La versione del foglio di calcolo per AD FS 2016 può essere scaricata [qui](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Può essere usata anche per AD FS in Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Come posso verificare che i server AD FS e WAP supportino i requisiti ATP di Apple?

Apple ha rilasciato un set di requisiti, noto come ATS (App Transport Security), che può influire sulle chiamate dalle app iOS che eseguono l'autenticazione in AD FS.  Puoi verificare che i server AD FS e WAP di cui disponi siano conformi assicurandoti che supportino i [requisiti per la connessione tramite ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
In particolare, devi verificare che i server AD FS e WAP supportino TLS 1.2 e che il pacchetto di crittografia negoziato della connessione TLS supporti PFS (Perfect Forward Secrecy).

Puoi abilitare e disabilitare SSL 2.0 e 3.0, nonché TLS versioni 1.0, 1.1 e 1.2 procedendo come descritto in [Gestire i protocolli SSL in AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Per assicurarti che i server AD FS e WAP negozino solo pacchetti di crittografia TLS che supportano ATP, puoi disabilitare tutti i pacchetti di crittografia non inclusi nell'[elenco dei pacchetti di crittografia conformi ad ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  A tale scopo, usa i [cmdlet di Windows TLS PowerShell](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Sviluppo

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>Quando viene generato un id_token con AD FS per un utente autenticato in Active Directory, come viene generata l'attestazione sub nell'id_token?
Il valore dell'attestazione sub è l'hash dell'ID client + il valore attestazione di ancoraggio.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>Qual è la durata del token di aggiornamento e del token di accesso quando l'utente accede tramite un trust del provider di attestazioni remoto su WS-Fed/SAML-P?
La durata del token di aggiornamento sarà la durata del token che ADFS ha ottenuto dal trust del provider di attestazioni remoto. La durata del token di accesso sarà la durata del token della relying party per cui viene emesso il token di accesso.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>Devo restituire gli ambiti di profilo (profile) e di posta elettronica (email) oltre all'ambito OpenId. Posso ottenere informazioni aggiuntive usando gli ambiti? Come devo procedere in AD FS?
Puoi usare l'id_token personalizzato per aggiungere informazioni rilevanti nell'id_token stesso. Per altre informazioni, vedi l'articolo [Personalizzare le attestazioni da emettere in id_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>Come posso rilasciare BLOB JSON all'interno dei token JWT?
A tale scopo, in AD FS 2016 sono stati aggiunti un ValueType speciale ("<http://www.w3.org/2001/XMLSchema#json>") e un carattere di escape (\x22). Per la regola di rilascio e l'output finale del token di accesso, vedi l'esempio seguente.

Regola di rilascio di esempio:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Attestazione rilasciata nel token di accesso:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>Posso passare il valore della risorsa come parte del valore dell'ambito nello stesso modo in cui le richieste vengono eseguite su Azure AD?
Con AD FS in Server 2019, ora puoi passare il valore della risorsa incorporato nel parametro relativo all'ambito (scope). Il parametro di ambito ora può essere organizzato come un elenco con valori separati da spazi in cui ogni voce è strutturata come risorsa/ambito. Ad esempio  
**<crea una richiesta di esempio valida>**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS supporta l'estensione PKCE?
AD FS in Server 2019 supporta PKCE (Proof Key for Code Exchange) per il flusso di concessione del codice di autorizzazione OAuth.

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>Quali ambiti consentiti sono supportati da AD FS?
- aza: se vengono usate le [estensioni del protocollo OAuth 2.0 per i client broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) e se il parametro relativo all'ambito (scope) contiene l'ambito "aza", il server rilascia un nuovo token di aggiornamento primario e lo imposta nel campo refresh_token della risposta, oltre a impostare il campo refresh_token_expires_in sulla durata del nuovo token di aggiornamento primario se ne viene applicato uno.
- openid: consente all'applicazione di richiedere l'uso del protocollo di autorizzazione OpenID Connect.
- logon_cert: questo ambito consente a un'applicazione di richiedere i certificati di accesso, che possono essere usati per connettere in modo interattivo gli utenti autenticati. Il server AD FS omette il parametro access_token dalla risposta e fornisce invece una catena di certificati CMS con codifica Base64 o una risposta PKI completa CMC. Altri dettagli sono disponibili [qui](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation: questo ambito è necessario per richiedere correttamente ad AD FS un token di accesso per conto di. Per informazioni dettagliate su come usare questo ambito, vedi [Creare un'applicazione multilivello con OBO (On-Behalf-Of) usando OAuth con AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert: questo ambito consente a un'applicazione di richiedere certificati VPN, che possono essere usati per stabilire connessioni VPN con l'autenticazione EAP-TLS. Non è più supportato.
- email: consente all'applicazione di richiedere un'attestazione di posta elettronica per l'utente connesso. Non è più supportato. 
- profile: consente all'applicazione di richiedere attestazioni relative al profilo per l'utente connesso. Non è più supportato. 


## <a name="operations"></a>Operazioni

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Come posso sostituire il certificato SSL per AD FS?
Il certificato SSL di AD FS è diverso dal certificato di comunicazione del servizio AD FS disponibile nello snap-in Gestione AD FS.  Per cambiare il certificato SSL di AD FS dovrai usare PowerShell. Segui le indicazioni riportate nell'articolo seguente:

[Gestione dei certificati SSL in AD FS e WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Come posso abilitare o disabilitare le impostazioni TLS/SSL per AD FS?
Per disabilitare o abilitare i protocolli SSL e i pacchetti di crittografia, vedi quanto segue:

[Gestione dei protocolli SSL in AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Il certificato SSL del proxy deve essere uguale al certificato SSL di AD FS?  
Relativamente al certificato SSL del proxy e al certificato SSL di AD FS, leggi le indicazioni seguenti:


- Se viene utilizzato il proxy per le richieste proxy ADFS che utilizzano l'autenticazione integrata di Windows, il certificato SSL del proxy devono corrispondere il (utilizzare la stessa chiave) il certificato SSL del server federativo
- Se la proprietà di AD FS "ExtendedProtectionTokenCheck" è abilitata (l'impostazione predefinita in AD FS), il certificato SSL del proxy deve essere lo stesso (utilizzare la stessa chiave) come il certificato SSL del server federativo
- In caso contrario, il certificato SSL del proxy può avere una chiave diversa dal certificato SSL di AD FS, ma deve soddisfare gli stessi [requisiti](../overview/AD-FS-2016-Requirements.md)

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>Perché in AD FS vedo solo l'accesso con password e non gli altri metodi di autenticazione che ho configurato? 
AD FS visualizza un solo metodo di autenticazione nella schermata di accesso quando l'applicazione richiede in modo esplicito un URI di autenticazione specifico mappato a un metodo di autenticazione configurato e abilitato. Questo viene trasmesso nel parametro 'wauth' per le richieste WS-Federation e nel parametro 'RequestedAuthnCtxRef' in una richiesta di protocollo SAML. Di conseguenza, viene visualizzato solo il metodo di autenticazione richiesto, ad esempio l'accesso con password.

Se AD FS viene usato con Azure AD, le applicazioni inviano spesso il parametro prompt=login ad Azure AD. Azure AD, per impostazione predefinita, converte questo parametro in una richiesta di un nuovo accesso basato su password ad AD FS. Questo è il motivo più comune per cui viene visualizzato un accesso con password in AD FS all'interno della rete o non viene visualizzata un'opzione per accedere con il certificato di cui disponi. Questo problema può essere risolto facilmente apportando una modifica alle impostazioni del dominio federato in Azure AD. 

Per informazioni su come effettuare questa configurazione, vedi [Supporto del parametro prompt=login di Active Directory Federation Services](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>Come posso cambiare l'account del servizio AD FS?
Per cambiare l'account del servizio AD FS, segui le istruzioni usando il [modulo Powershell dell'account del servizio](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule) della casella degli strumenti di AD FS.

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Come posso configurare i browser per l'uso dell'autenticazione integrata di Windows con AD FS?

Per informazioni su come configurare i browser, vedi [Configurare i browser per l'uso dell'autenticazione integrata di Windows (WIA) con AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-turn-off-browserssoenabled"></a>Posso disattivare BrowserSsoEnabled?
Se non disponi di criteri di controllo di accesso basati sul dispositivo in ADFS o nella registrazione certificato di Windows Hello for Business con ADFS, puoi disattivare BrowserSsoEnabled. Quest'ultimo consente ad ADFS di raccogliere un token di aggiornamento primario (PRT) da un client che contiene informazioni sul dispositivo. Senza tale dispositivo, l'autenticazione di ADFS non funzionerà sui dispositivi Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>Per quanto tempo sono validi i token AD FS?

Spesso questa domanda significa "per quanto tempo gli utenti ottengono l'accesso Single Sign-On (SSO) senza dover immettere nuove credenziali e come posso controllare questa situazione come amministratore?"  Questo comportamento e le impostazioni di configurazione che lo controllano sono descritte nell'articolo [Impostazioni di Single Sign-on AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Di seguito sono elencate le durate predefinite dei diversi cookie e token, oltre ai parametri che controllano la durata:

**Dispositivi registrati**

- PRT e cookie SSO: massimo 90 giorni; durata controllata da PSSOLifeTimeMins. Il dispositivo fornito viene usato almeno ogni 14 giorni, intervallo controllato da DeviceUsageWindow

- Token di aggiornamento: calcolato in base a quanto precede per garantire un comportamento coerente

- access_token: 1 ora per impostazione predefinita, in base alla relying party

- id_token: come il token di accesso

**Dispositivi non registrati**

- Cookie SSO: 8 ore per impostazione predefinita; intervallo controllato da SSOLifetimeMins.  Se è abilitata l'opzione Mantieni l'accesso, l'impostazione predefinita è 24 ore ed è configurabile tramite KMSILifetimeMins.


- Token di aggiornamento: 8 ore per impostazione predefinita. 24 ore quando è abilitata l'opzione Mantieni l'accesso

- access_token: 1 ora per impostazione predefinita, in base alla relying party

- id_token: come il token di accesso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS supporta HSTS (HTTP Strict Transport Security)?  

HSTS (HTTP Strict Transport Security) è un meccanismo di criteri di sicurezza Web che consente di attenuare gli attacchi di downgrade del protocollo e l'acquisizione del controllo dei cookie per i servizi che hanno endpoint sia HTTP che HTTPS. Consente ai server Web di dichiarare che i Web browser (o altri agenti utente conformi) devono interagire con esso solo usando HTTPS e mai tramite il protocollo HTTP.

Tutti gli endpoint AD FS per il traffico di autenticazione Web vengono aperti esclusivamente su HTTPS.  Di conseguenza, AD FS attenua in modo efficace le minacce a cui si viene esposti dal meccanismo di criteri HSTS (da progettazione, non è previsto alcun downgrade a HTTP perché non sono presenti listener in HTTP). AD FS inoltre impedisce l'invio dei cookie a un altro server con endpoint del protocollo HTTP contrassegnando tutti i cookie con il flag sicuro.

Pertanto, l'implementazione di HSTS in un server AD FS non è necessaria perché non può mai essere sottoposta a downgrade.  Ai fini della conformità, i server AD FS soddisfano questi requisiti perché non possono mai usare HTTP e tutti i cookie sono contrassegnati come sicuri.

Inoltre, AD FS 2016 (con le patch più aggiornate) e AD FS 2019 supportano l'emissione dell'intestazione HSTS. Per eseguire questa configurazione, vedi [Personalizzare le intestazioni di risposta di sicurezza HTTP con AD FS](../operations/customize-http-security-headers-ad-fs.md).

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-forwarded-client-ip non contiene l'IP del client, bensì l'IP del firewall davanti al proxy. Dove posso trovare l'IP corretto del client?
Non è consigliabile eseguire la terminazione SSL prima di WAP. Nel caso in cui la terminazione SSL venga eseguita davanti a WAP, X-ms-forwarded-client-ip conterrà l'IP del dispositivo di rete davanti a WAP. Di seguito è riportata una breve descrizione delle diverse attestazioni correlate all'IP supportate da AD FS:
 - x-ms-client-ip: IP di rete del dispositivo connesso al servizio token di sicurezza.  Nel caso di una richiesta Extranet, l'attestazione contiene sempre l'IP di WAP.
 - x-ms-forwarded-client-ip: attestazione multivalore che conterrà gli eventuali valori inoltrati ad ADFS da Exchange Online e l'indirizzo IP del dispositivo connesso a WAP.
 - Userip: per le richieste Extranet, questa attestazione conterrà il valore di x-ms-forwarded-client-ip.  Per le richieste Intranet, questa attestazione conterrà lo stesso valore di x-ms-client-ip.

 Inoltre, in AD FS 2016 (con le patch più aggiornate) e versioni successive è supportata anche l'acquisizione dell'intestazione x-forwarded-for. Qualsiasi servizio di bilanciamento del carico o dispositivo di rete che non esegue l'inoltro al livello 3 (l'IP viene mantenuto) deve aggiungere l'IP client in ingresso all'intestazione x-forwarded-for standard del settore. 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Sto tentando di ottenere altre attestazioni sull'endpoint delle informazioni utente, ma viene restituito solo l'oggetto. Come posso recuperare altre attestazioni?
L'endpoint UserInfo AD FS restituisce sempre l'attestazione dell'oggetto come specificato negli standard OpenID. AD FS non fornisce altre attestazioni richieste tramite l'endpoint UserInfo. Se hai necessità di altre attestazioni nel token ID, vedi [Token ID personalizzati in AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>Perché vengono visualizzati numerosi errori 1021 nei server AD FS?
Questo evento in genere viene registrato per un accesso non valido in AD FS per la risorsa 00000003-0000-0000-c000-000000000000. Questo errore è causato da un comportamento non corretto del client che tenta di ottenere un token di accesso per il servizio Azure AD Graph. Poiché la risorsa non è presente in AD FS, viene restituito l'ID evento 1021 nei server AD FS. Gli eventuali errori o avvertenze per la risorsa 00000003-0000-0000-c000-000000000000 possono essere ignorati tranquillamente in AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Perché viene visualizzato un avviso quando è impossibile aggiungere l'account del servizio AD FS al gruppo Enterprise Key Admins?
Questo gruppo viene creato solo quando nel dominio è presente un controller di dominio Windows 2016 con il ruolo PDC FSMO. Per correggere l'errore, puoi creare manualmente il gruppo e seguire la procedura descritta sotto per concedere l'autorizzazione necessaria dopo aver aggiunto l'account del servizio come membro del gruppo.
1.    Aprire **Utenti e computer di Active Directory**.
2.    Nel riquadro di spostamento **fai clic con il pulsante destro del mouse** sul nome del dominio e **scegli** Proprietà.
3.    **Fai clic** su Sicurezza (se la scheda Sicurezza non è visualizzata, attiva Funzionalità avanzate nel menu Visualizza).
4.    **Fai clic** su Avanzate. **Fai clic** su Aggiungi. **Fai clic** su Seleziona un'entità.
5.    Viene visualizzata la finestra di dialogo Seleziona utente, computer, account di servizio o gruppo.  Nella casella di testo Immettere il nome dell'oggetto da selezionare digita Key Admin Group.  Fare clic su OK.
6.    Nella casella di riepilogo Si applica a seleziona **Oggetti utente discendenti**.
7.    Usando la barra di scorrimento, scorri fino alla fine della pagina e **fai clic** su Cancella tutto.
8.    Nella sezione **Proprietà** seleziona **Lettura msDS-KeyCredentialLink** e **Scrittura msDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Perché l'autenticazione moderna offerta dai dispositivi Android ha esito negativo se il server non invia tutti i certificati intermedi della catena con il certificato SSL?

L'autenticazione degli utenti federati in Azure AD può avere esito negativo per le app che usano la libreria ADAL Android. L'app riceverà un'eccezione **AuthenticationException** quando tenterà di visualizzare la pagina di accesso. In Chrome la pagina di accesso AD FS potrebbe essere chiamata come non sicura.

Android, in tutte le versioni e in tutti i dispositivi, non supporta il download di certificati aggiuntivi dal campo **authorityInformationAccess** del certificato. Questo vale anche per il browser Chrome. Questo errore verrà generato per qualsiasi certificato di autenticazione server privo dei certificati intermedi se l'intera catena di certificati non viene passata da AD FS.

Una soluzione appropriata a questo problema consiste nel configurare i server AD FS e WAP per inviare i certificati intermedi necessari insieme al certificato SSL.

Quando esporti il certificato SSL, da un computer, per importarlo nell'archivio personale del computer, dei server AD FS e WAP, assicurati di esportare la chiave privata e selezionare **Scambio di informazioni personali - PKCS #12**.

È importante che sia selezionata la casella di controllo per l'**inclusione di tutti i certificati nel percorso del certificato se possibile**, ma anche **Esporta tutte le proprietà estese**.  

Esegui certlm. msc nei server Windows e importa *.PFX nell'archivio certificati personale del computer. In questo modo il server passerà l'intera catena di certificati alla libreria ADAL.

>[!NOTE]
> È inoltre necessario aggiornare l'archivio certificati dei servizi di bilanciamento del carico di rete in modo da includere l'intera catena di certificati, se presente

### <a name="does-ad-fs-support-head-requests"></a>AD FS supporta le richieste HEAD?
AD FS non supporta le richieste HEAD.  Le applicazioni non devono usare richieste HEAD per gli endpoint AD FS.  Questo può causare risposte di errore HTTP impreviste e/o ritardate.  Potrebbero inoltre risultare eventi di errore imprevisti nel registro eventi AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Perché non viene visualizzato un token di aggiornamento quando effettuo l'accesso con un IdP remoto?
Non viene rilasciato un token di aggiornamento se il token rilasciato da IdP ha una validità inferiore a 1 ora. Per essere certi che venga rilasciato un token di aggiornamento, estendi a più di 1 ora la validità del token rilasciato da IdP.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>È possibile modificare l'algoritmo di crittografia del token RP?
Per impostazione predefinita, la crittografia del token RP è impostata su AES256 e non può essere impostata su un altro valore.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>In una farm in modalità mista viene visualizzato un errore quando tento di impostare il nuovo certificato SSL usando Set-AdfsSslCertificate -Thumbprint. Come posso aggiornare il certificato SSL in una farm AD FS in modalità mista?
Le farm AD FS in modalità mista sono destinate a essere uno stato di transizione. Durante la pianificazione è consigliabile eseguire il rollover del certificato SSL prima del processo di aggiornamento oppure completare il processo e aumentare il livello di comportamento della farm prima di aggiornare il certificato SSL. Nel caso in cui questo non sia stato fatto, le istruzioni seguenti consentono di aggiornare il certificato SSL. 

Nei server WAP puoi comunque usare set-WebApplicationProxySslCertificate. Nei server ADFS devi usare netsh. Segui questi passaggi:

1. Seleziona un subset di server ADFS 2016 per la manutenzione (ad esempio, tramite rimozione dal servizio di bilanciamento del carico)
2. Nei server selezionati nel passaggio 1 importa il nuovo certificato tramite MMC
3. Elimina i certificati esistenti

    a. netsh http delete sslcert hostnameport=fs.contoso.com:443 b. netsh http delete sslcert hostnameport=localhost:443 c. netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  Aggiungi il nuovo certificato

    a. netsh http add sslcert hostnameport=fs.contoso.com:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    b. netsh http add sslcert hostnameport=localhost:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    c. netsh http add sslcert hostnameport=fs.contoso.com:49443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

5. Riavvia il servizio ADFS nel server selezionato
6. Rimuovi il subset di server WAP per la manutenzione
7. Nei server WAP selezionati importa il nuovo certificato tramite MMC
8. Imposta il nuovo certificato in WAP usando il cmdlet

    a. Set-WebApplicationProxySslCertificate -Thumbprint " CERTTHUMBPRINT"

9. Riavvia il servizio nei server WAP selezionati
10. Inserisci di nuovo nell'ambiente di produzione i server WAP e AD FS selezionati.

Esegui l'aggiornamento sul resto dei server AD FS e WAP in modo analogo.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>ADFS è supportato quando i server WAP (Web Application Proxy) sono protetti da Azure Web Application Firewall (WAF)?
I server ADFS e i server applicazioni Web supportano qualsiasi firewall che non esegue la terminazione SSL sull'endpoint. Inoltre, i server ADFS/WAP includono meccanismi integrati per impedire gli attacchi Web comuni, ad esempio attacchi tramite script da altri siti, il proxy ADFS e soddisfano tutti i requisiti definiti dal [protocollo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>Viene visualizzato il messaggio "Evento 441: È stato trovato un token con una chiave di binding del token non valida". Come posso risolvere questo problema?
In AD FS 2016 il binding del token è abilitato automaticamente e causa diversi problemi noti con scenari di proxy e di federazione che generano questo errore. Per correggerlo, esegui il comando PowerShell seguente e rimuovi il supporto per il binding del token.

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>Ho aggiornato la farm da ADFS in Windows Server 2016 R2 ad AD FS in Windows Server 2019. Il livello di comportamento per la farm AD FS è stato elevato correttamente alla versione 2019, ma la configurazione del Proxy applicazione Web è ancora visualizzata come Windows Server 2016?
Dopo un aggiornamento a Windows Server 2019, la versione della configurazione del Proxy applicazione Web continuerà a essere visualizzata come Windows Server 2016. Il Proxy applicazione Web non dispone di nuove funzionalità specifiche della versione per Windows Server 2019 e, se il livello di comportamento della farm è stato elevato correttamente in AD FS, tale proxy continuerà a essere visualizzato come Windows Server 2016 da progettazione.

### <a name="can-i-estimate-the-size-of-the-adfsartifactstore-before-enabling-esl"></a>Posso stimare le dimensioni di ADFSArtifactStore prima di abilitare ESL?
Con l'abilitazione di ESL, AD FS tiene traccia dell'attività dell'account e dei percorsi noti per gli utenti nel database ADFSArtifactStore. Le dimensioni di questo database variano in base al numero di utenti e di percorsi noti di cui viene tenuta traccia. Quando pianifichi l'abilitazione di ESL, puoi stimare le dimensioni del database ADFSArtifactStore considerando un aumento fino a un massimo di 1 GB ogni 100.000 utenti. Se la farm AD FS usa Database interno di Windows, il percorso predefinito per i file di database è C:\Windows\WID\Data. Per evitare che questa unità si riempia, verifica di avere almeno 5 GB di spazio di archiviazione disponibile prima di abilitare ESL. Oltre che dello spazio di archiviazione su disco, esegui la pianificazione tenendo conto che la memoria di elaborazione totale dopo l'abilitazione di ESL si espanderà fino a un massimo di 1 ulteriore GB di RAM per popolazioni di un massimo di 500.000 utenti.

### <a name="i-am-seeing-event-570-active-directory-trust-enumeration-was-unable-to-enumerate-one-of-more-domains-due-to-the-following-error-enumeration-will-continue-but-the-active-directory-identifier-list-may-not-be-correct-validate-that-all-expected-active-directory-identifiers-are-present-by-running-get-adfsdirectoryproperties-on-ad-fs-2019-what-is-the-mitigation-for-this-event"></a>Viene visualizzato l'evento 570 in AD FS 2019. L'enumerazione attendibile di Active Directory non è stata in grado di enumerare uno o più domini a causa dell'errore seguente. L'enumerazione continuerà, ma l'elenco di identificatori di Active Directory potrebbe non essere corretto. Verifica che tutti gli identificatori di Active Directory previsti siano presenti eseguendo Get-ADFSDirectoryProperties. Qual è la misura di prevenzione per questo evento?
Questo evento si verifica se le foreste non sono attendibili quando AD FS prova a enumerare e a connettersi a tutte le foreste di una catena di foreste attendibili. Se ad esempio la foresta A e la foresta B di AD FS sono attendibili e la foresta B e la foresta C sono attendibili, AD FS enumera tutte e tre le foreste e prova a trovare una relazione di trust tra le foreste A e C. Se gli utenti della foresta in errore devono essere autenticati da AD FS, configura una relazione di trust tra la foresta di AD FS e la foresta in errore. Se gli utenti della foresta in errore non devono essere autenticati da AD FS, questo errore dovrà essere ignorato.

### <a name="i-am-seeing-an-event-id-364-microsoftidentityserverauthenticationfailedexception-msis5015-authentication-of-the-presented-token-failed-token-binding-claim-in-token-must-match-the-binding-provided-by-the-channel-what-should-i-do-to-resolve-this"></a>Viene visualizzato il messaggio "Evento 364: Microsoft.IdentityServer.AuthenticationFailedException: MSIS5015: autenticazione del token presentato non riuscita. L'attestazione del binding nel token deve corrispondere al binding fornito dal canale". Come posso risolvere questo problema?
In AD FS 2016 il binding del token è abilitato automaticamente e causa diversi problemi noti con scenari di proxy e di federazione che generano questo errore. Per correggerlo, esegui il comando PowerShell seguente e rimuovi il supporto per il binding del token.

`Set-AdfsProperties -IgnoreTokenBinding $true`
