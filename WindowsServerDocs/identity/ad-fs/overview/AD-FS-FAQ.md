---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: DOMANDE FREQUENTI AD FS
description: Domande frequenti per AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8444e417fe089c1a3cf2acc2648b222ec5c9774c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865469"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Domande frequenti su AD FS


Nella seguente documentazione sono riportate le domande frequenti relative a Active Directory Federation Services.  Il documento è stato suddiviso in gruppi in base al tipo di domanda.

## <a name="deployment"></a>Distribuzione

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Come è possibile eseguire l'aggiornamento o la migrazione da versioni precedenti di AD FS
È possibile aggiornare AD FS usando uno dei seguenti elementi:


- Windows Server 2012 R2 AD FS a Windows Server 2016 AD FS o versione successiva. Si noti che la metodologia è la stessa se si esegue l'aggiornamento da Windows Server 2016 AD FS a Windows Server 2019 AD FS. 
    - [Aggiornamento ad AD FS in Windows Server 2016 con un database interno di Windows](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Aggiornamento ad AD FS in Windows Server 2016 con un database SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS a Windows Server 2012 R2 AD FS
    - [Eseguire la migrazione a AD FS in Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2,0 a Windows Server 2012 AD FS
    - [Eseguire la migrazione a AD FS in Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1. x AD FS 2,0
    - [Aggiornamento da AD FS 1. x a AD FS 2,0](https://technet.microsoft.com/library/ff678035.aspx)

Se è necessario eseguire l'aggiornamento da AD FS 2,0 o 2,1 (Windows Server 2008 R2 o Windows Server 2012), è necessario usare gli script predefiniti (disponibili in C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Perché AD FS installazione richiede un riavvio del server?

Il supporto HTTP/2 è stato aggiunto in Windows Server 2016, ma HTTP/2 non può essere usato per l'autenticazione del certificato client.  Poiché molti scenari AD FS usano l'autenticazione del certificato client e un numero significativo di client non supporta il nuovo tentativo di richieste tramite HTTP/1.1, AD FS configurazione della farm riconfigura le impostazioni HTTP del server locale su HTTP/1.1.  Questa operazione richiede il riavvio del server.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>USA i server WAP Windows 2016 per pubblicare la farm AD FS su Internet senza aggiornare la farm back-end AD FS supportata?
Sì, questa configurazione è supportata, ma non sono supportate nuove funzionalità di AD FS 2016 in questa configurazione.  Questa configurazione deve essere temporanea durante la fase di migrazione da AD FS 2012 R2 a AD FS 2016 e non deve essere distribuita per lunghi periodi di tempo.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>È possibile distribuire AD FS per Office 365 senza pubblicare un proxy in Office 365?
Sì, questa operazione è supportata. Tuttavia, come effetto collaterale

1. È necessario gestire manualmente i certificati di firma del token di aggiornamento perché Azure AD non sarà in grado di accedere ai metadati della Federazione. Per ulteriori informazioni sull'aggiornamento manuale del certificato per la firma di token [, vedere rinnovo dei certificati di federazione per Office 365 e Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. Non sarà possibile sfruttare i flussi di autenticazione legacy (ad esempio, il flusso di autenticazione proxy ExO)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>Quali sono i requisiti di bilanciamento del carico per i server AD FS e WAP?

AD FS è un sistema senza stato. Di conseguenza, il bilanciamento del carico è piuttosto semplice per gli account di accesso. Di seguito sono riportate le raccomandazioni principali per i sistemi di bilanciamento del carico.


- I bilanciamenti del carico non devono essere configurati con l'affinità IP. Questo può comportare un carico eccessivo su un subset di server in determinati scenari di Exchange Online.
- I bilanciamenti del carico non devono terminare le connessioni HTTPS e riavviare una nuova connessione al server ADFS.
- I bilanciamenti del carico devono assicurarsi che l'indirizzo IP di connessione venga tradotto come IP di origine nel pacchetto HTTP quando viene inviato ad ADFS. Nel caso in cui un servizio di bilanciamento del carico non è in grado di inviare l'IP di origine nel pacchetto HTTP, il servizio di bilanciamento del carico deve aggiungere (o accodare in caso di esistente) l'indirizzo IP all'intestazione x-inoltred-for. Questa operazione è necessaria per la corretta gestione di alcune funzionalità correlate a IP (IP vietato, blocco Smart Extranet,...) e potrebbe causare una riduzione della sicurezza se non è configurata correttamente.
- I bilanciamenti del carico devono supportare SNI. In caso contrario, assicurarsi che AD FS sia configurato in modo da creare associazioni HTTPS per gestire i client che non supportano SNI.
- I servizi di bilanciamento del carico devono usare l'endpoint del probe di integrità HTTP AD FS per rilevare se i server AD FS o WAP sono operativi ed escluderli se non viene restituito un 200 OK.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>Quali configurazioni a più foreste sono supportate da AD FS?

AD FS supporta più configurazioni a più foreste e si basa sulla rete di trust di servizi di dominio Active Directory sottostante per autenticare gli utenti in più aree di autenticazione attendibili. Si consiglia di considerare attendibili le foreste a 2 vie perché si tratta di una configurazione più semplice per garantire che il sottosistema di attendibilità funzioni correttamente senza problemi. Inoltre

- Nel caso di un trust tra foreste unidirezionale, ad esempio una foresta perimetrale contenente identità partner, è consigliabile distribuire ADFS nella foresta Corp e trattare la foresta perimetrale come un altro trust del provider di attestazioni locale connesso tramite LDAP. In questo caso, l'autenticazione integrata di Windows non funzionerà per gli utenti della foresta perimetrale e sarà necessario eseguire l'autenticazione con password perché è l'unico meccanismo supportato per LDAP. Nel caso in cui non sia possibile eseguire questa opzione, è necessario configurare un altro ADFS nella foresta della rete perimetrale e aggiungerlo come trust del provider di attestazioni in ADFS nella foresta Corp. Gli utenti dovranno eseguire l'individuazione dell'area di autenticazione principale, ma l'autenticazione integrata di Windows e l'autenticazione con password funzioneranno. Apportare le modifiche appropriate nelle regole di rilascio in ADFS nella foresta perimetrale poiché ADFS nella foresta Corp non sarà in grado di ottenere informazioni utente aggiuntive sull'utente dalla foresta perimetrale.
- Sebbene i trust a livello di dominio siano supportati e possano funzionare, si consiglia vivamente di eseguire il passaggio a un modello di trust a livello di foresta. Inoltre, è necessario assicurarsi che il routing UPN e la risoluzione del nome NETBIOS dei nomi debbano funzionare in modo accurato.



## <a name="design"></a>Progettazione

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quali sono i provider di autenticazione a più fattori di terze parti disponibili per AD FS?
AD FS fornisce un meccanismo estensibile per l'integrazione dei provider di autenticazione a più fattori di terze parti. Non è disponibile alcun programma di certificazione per questa impostazione. Si presuppone che il fornitore abbia eseguito le convalide necessarie prima del rilascio. 

L'elenco dei fornitori che hanno inviato una notifica a Microsoft viene pubblicato presso i provider di autenticazione a più fattori [per ad FS](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).  Potrebbero essere sempre disponibili provider che non sono in grado di conoscere e l'elenco verrà aggiornato durante l'apprendimento.

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Sono supportati proxy di terze parti con AD FS?
Sì, i proxy di terze parti possono essere posizionati prima del proxy dell'applicazione Web, ma qualsiasi proxy di terze parti deve supportare il [protocollo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) da usare al posto del proxy dell'applicazione Web.

Di seguito è riportato un elenco di provider di terze parti di cui si è a conoscenza.  Potrebbero essere sempre disponibili provider che non sono in grado di conoscere e l'elenco verrà aggiornato durante l'apprendimento.

- [Gestione criteri di accesso F5](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Dove è il foglio di calcolo per il dimensionamento della pianificazione della capacità per AD FS 2016?
È possibile scaricare la versione AD FS 2016 del foglio di calcolo [qui](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Questa operazione può essere usata anche per AD FS in Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Come è possibile garantire che i server AD FS e WAP supportino i requisiti ATP di Apple?

Apple ha rilasciato un set di requisiti denominati ATS (app Transport Security) che possono influito sulle chiamate da app iOS che eseguono l'autenticazione a AD FS.  È possibile verificare che i server di AD FS e WAP siano conformi verificando che supportino i [requisiti per la connessione tramite ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
In particolare, è necessario verificare che i server di AD FS e WAP supportino TLS 1,2 e che la suite di crittografia negoziata della connessione TLS supporti la riservatezza in diretta perfetta.

È possibile abilitare e disabilitare le versioni SSL 2,0 e 3,0 e TLS 1,0, 1,1 e 1,2 usando [Gestisci protocolli SSL in ad FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Per assicurarsi che i server AD FS e WAP negoziino solo i pacchetti di crittografia TLS che supportano ATP, è possibile disabilitare tutti i pacchetti di crittografia non inclusi nell' [elenco di pacchetti di crittografia conformi a ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  A tale scopo, usare i [cmdlet di Windows TLS PowerShell](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Sviluppatore

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>Quando si genera un token ID con ADFS per un utente autenticato in Active Directory, come viene generata l'attestazione "Sub" in token ID?
Il valore dell'attestazione "Sub" è l'hash dell'ID client + valore attestazione ancoraggio.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>Qual è la durata del token di aggiornamento/token di accesso quando l'utente accede tramite un trust del provider di attestazioni remoto su WS-Fed/SAML-P?
La durata del token di aggiornamento sarà la durata del token che ADFS ha ottenuto dal trust del provider di attestazioni remoto. La durata del token di accesso sarà la durata del token del relying party per cui viene emesso il token di accesso.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>È necessario restituire anche gli ambiti di profilo e di posta elettronica oltre all'ambito OpenId. È possibile ottenere informazioni aggiuntive usando gli ambiti? Come procedere in AD FS?
È possibile usare token ID personalizzati per aggiungere informazioni rilevanti nel token ID stesso. Per ulteriori informazioni, vedere l'articolo [personalizzare le attestazioni da emettere in token ID](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>Come rilasciare BLOB JSON all'interno di token JWT?
Un ValueType speciale ("<http://www.w3.org/2001/XMLSchema#json>") e un carattere di escape (\x22) per questo è stato aggiunto in ad FS 2016. Nell'esempio seguente viene illustrata la regola di rilascio e anche l'output finale del token di accesso.

Regola di rilascio di esempio:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Attestazione rilasciata nel token di accesso:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>È possibile passare il valore della risorsa come parte del valore dell'ambito, ad esempio il modo in cui le richieste vengono eseguite su Azure AD?
Con AD FS sul server 2019, è ora possibile passare il valore della risorsa incorporato nel parametro scope. Il parametro scope può ora essere organizzato come un elenco separato da spazi in cui ogni voce è una struttura come risorsa/ambito. Esempio:  
**< creare una richiesta di esempio valida >**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS supporta l'estensione PKCE?
AD FS nel server 2019 supporta la chiave di prova per lo scambio di codice (PKCE) per il flusso di concessione del codice di autorizzazione OAuth

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>Quali sono gli ambiti consentiti supportati da AD FS?
- Aza: se si utilizzano le [estensioni del protocollo OAuth 2,0 per i client del broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) e se il parametro scope contiene l'ambito "AZA", il server emette un nuovo token di aggiornamento primario e lo imposta nel campo refresh_token della risposta, oltre a impostare refresh_token_ campo expires_in per la durata del nuovo token di aggiornamento primario se ne viene applicato uno.
- OpenID: consente all'applicazione di richiedere l'uso del protocollo di autorizzazione OpenID Connect.
- logon_cert: l'ambito logon_cert consente a un'applicazione di richiedere certificati di accesso, che possono essere usati per accedere in modo interattivo agli utenti autenticati. Il server AD FS omette il parametro access_token dalla risposta e fornisce invece una catena di certificati CMS con codifica Base64 o una risposta PKI completa della CMC. Altre informazioni sono disponibili [qui](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation: l'ambito user_impersonation è necessario per richiedere correttamente un token di accesso per conto di AD FS. Per informazioni dettagliate su come usare questo ambito, vedere [creare un'applicazione a più livelli usando il metodo (OBO) per l'uso di OAuth con AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert: l'ambito vpn_cert consente a un'applicazione di richiedere certificati VPN, che possono essere usati per stabilire connessioni VPN con l'autenticazione EAP-TLS. Questa operazione non è più supportata.
- posta elettronica: consente all'applicazione di richiedere l'attestazione di posta elettronica per l'utente connesso. Questa operazione non è più supportata. 
- profilo: consente all'applicazione di richiedere le attestazioni relative al profilo per l'utente di accesso. Questa operazione non è più supportata. 


## <a name="operations"></a>Operazioni

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Ricerca per categorie sostituire il certificato SSL per AD FS?
Il certificato SSL AD FS non corrisponde al certificato di comunicazione del servizio AD FS disponibile nello snap-in di gestione AD FS.  Per modificare il certificato SSL di AD FS, è necessario usare PowerShell. Seguire le istruzioni riportate nell'articolo seguente:

[Gestione dei certificati SSL in AD FS e WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Come è possibile abilitare o disabilitare le impostazioni TLS/SSL per AD FS
Per disabilitare o abilitare i protocolli SSL e i pacchetti di crittografia, usare quanto segue:

[Gestire i protocolli SSL in AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Il certificato SSL del proxy deve essere uguale a quello del certificato SSL AD FS?  
Usare le linee guida seguenti per quanto riguarda il certificato SSL proxy e il certificato SSL AD FS:


- Se viene utilizzato il proxy per le richieste proxy ADFS che utilizzano l'autenticazione integrata di Windows, il certificato SSL del proxy devono corrispondere il (utilizzare la stessa chiave) il certificato SSL del server federativo
- Se la proprietà di AD FS "ExtendedProtectionTokenCheck" è abilitata (l'impostazione predefinita in AD FS), il certificato SSL del proxy deve essere lo stesso (utilizzare la stessa chiave) come il certificato SSL del server federativo
- In caso contrario, il certificato SSL proxy può avere una chiave diversa dal certificato SSL AD FS, ma deve soddisfare gli stessi [requisiti](../overview/AD-FS-2016-Requirements.md)

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>Perché viene visualizzato solo un account di accesso con password per AD FS e non altri metodi di autenticazione configurati? 
AD FS viene visualizzato un solo metodo di autenticazione nella schermata di accesso quando l'applicazione richiede in modo esplicito un URI di autenticazione specifico che esegue il mapping a un metodo di autenticazione configurato e abilitato. Questo viene trasmesso nel parametro ' wauth ' per le richieste WS-Federation e il parametro ' RequestedAuthnCtxRef ' in una richiesta di protocollo SAML. Di conseguenza, viene visualizzato solo il metodo di autenticazione richiesto, ad esempio l'accesso con password.

Quando AD FS viene usato con Azure AD, le applicazioni inviano spesso il parametro prompt = login per Azure AD. Azure AD per impostazione predefinita converte questo oggetto in modo da richiedere un nuovo account di accesso basato su password al AD FS. Questo è il motivo più comune per cui viene visualizzato un account di accesso con password AD FS all'interno della rete o non è possibile accedere con il certificato. Questa operazione può essere risolta facilmente apportando una modifica alle impostazioni del dominio federato in Azure AD. 

Per informazioni su come eseguire questa configurazione, vedere [Active Directory Federation Services prompt = supporto](../operations/AD-FS-Prompt-Login.md)per i parametri di accesso.

### <a name="how-can-i-change-the-ad-fs-service-account"></a>Come è possibile modificare l'account del servizio AD FS?
Per modificare l'account del servizio AD FS, seguire le istruzioni usando il modulo di PowerShell per l' [account del servizio](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule)ad FS casella degli strumenti.

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Come è possibile configurare I browser per l'uso dell'autenticazione integrata di Windows (WIA) con AD FS?

Per informazioni sulla configurazione dei browser, vedere [configurare i browser per l'uso dell'autenticazione integrata di Windows (WIA) con ad FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-turn-off-browserssoenabled"></a>È possibile disattivare BrowserSsoEnabled?
Se non si dispone di criteri di controllo di accesso in base al dispositivo in ADFS o alla registrazione dei certificati di Windows Hello for business con ADFS; è possibile disattivare BrowserSsoEnabled. BrowserSsoEnabled consente ad ADFS di raccogliere un PRT (token di aggiornamento primario) da un client che contiene informazioni sul dispositivo. Senza che l'autenticazione del dispositivo di ADFS non funzionerà nei dispositivi Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>Per quanto tempo sono validi i token AD FS?

Spesso questa domanda significa "per quanto tempo gli utenti ottengono l'accesso Single Sign-on (SSO) senza dover immettere nuove credenziali e come posso controllare come amministratore?"  Questo comportamento e le impostazioni di configurazione che lo controllano sono descritte nell'articolo [ad FS impostazioni di Single Sign-on](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Di seguito sono elencate le durate predefinite dei diversi cookie e token, oltre ai parametri che governano le durate.

**Dispositivi registrati**

- Cookie PRT e SSO: massimo 90 giorni, governato da PSSOLifeTimeMins. (Il dispositivo fornito viene usato almeno ogni 14 giorni, che è controllato da DeviceUsageWindow)

- Token di aggiornamento: calcolato in base alla precedente per garantire un comportamento coerente

- access_token: 1 ora per impostazione predefinita, in base al relying party

- token ID: uguale al token di accesso

**Dispositivi non registrati**

- Cookie SSO: 8 ore per impostazione predefinita, regolate da SSOLifetimeMins.  Quando l'opzione Mantieni l'accesso (KMSI) è abilitata, il valore predefinito è 24 ore e configurabile tramite KMSILifetimeMins.


- Token di aggiornamento: 8 ore per impostazione predefinita. 24 ore con KMSI abilitato

- access_token: 1 ora per impostazione predefinita, in base al relying party

- token ID: uguale al token di accesso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS supporta la sicurezza del trasporto HTTP Strict (HSTS)?  

HTTP Strict Transport Security (HSTS) è un meccanismo di criteri di sicurezza Web che consente di attenuare gli attacchi di downgrade del protocollo e il Hijack dei cookie per i servizi con endpoint HTTP e HTTPS. Consente ai server Web di dichiarare che i Web browser (o altri agenti utente conformi) devono interagire con esso solo usando HTTPS e mai tramite il protocollo HTTP.

Tutti gli endpoint AD FS per il traffico di autenticazione Web vengono aperti esclusivamente tramite HTTPS.  Di conseguenza, AD FS Attenua efficacemente le minacce fornite dal meccanismo di criteri di sicurezza del trasporto HTTP Strict (in base alla progettazione, non esiste alcun downgrade a HTTP poiché non sono presenti listener in HTTP). Inoltre, AD FS impedisce l'invio dei cookie a un altro server con gli endpoint del protocollo HTTP contrassegnando tutti i cookie con il flag protetto.

Pertanto, l'implementazione di HSTS su un server AD FS non è necessaria perché non può mai essere sottoposto a downgrade.  Ai fini della conformità, i server AD FS soddisfano questi requisiti, perché non possono mai usare HTTP e tutti i cookie sono contrassegnati come protetti.

Inoltre, AD FS 2016 (con le patch più aggiornate) e AD FS 2019 supportano la creazione dell'intestazione HSTS. Per configurare questa configurazione, vedere [personalizzare le intestazioni di risposta di sicurezza http con ad FS](../operations/customize-http-security-headers-ad-fs.md)

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-MS-inoltro-client-IP non contiene l'indirizzo IP del client, ma contiene l'IP del firewall davanti al proxy. Dove è possibile ottenere l'indirizzo IP corretto del client?
Non è consigliabile eseguire la terminazione SSL prima di WAP. Nel caso in cui la terminazione SSL venga eseguita davanti al WAP, l'indirizzo X-MS-inoltred-client-IP conterrà l'indirizzo IP del dispositivo di rete davanti a WAP. Di seguito è riportata una breve descrizione delle diverse attestazioni correlate all'IP supportate da AD FS:
 - x-MS-client-IP: IP di rete del dispositivo connesso al servizio STS.  Nel caso di una richiesta Extranet, questo contiene sempre l'indirizzo IP del WAP.
 - x-ms-inoltred-client-IP: Attestazione multivalore che conterrà tutti i valori trasmessi ad ADFS da Exchange Online e l'indirizzo IP del dispositivo connesso al WAP.
 - Userip: Per le richieste Extranet questa attestazione conterrà il valore di x-ms-inoltred-client-IP.  Per le richieste Intranet, questa attestazione conterrà lo stesso valore di x-MS-client-IP.

 Inoltre, in AD FS 2016 (con le patch più aggiornate) e versioni successive supportano anche l'acquisizione dell'intestazione x-inoltred-for. Qualsiasi servizio di bilanciamento del carico o dispositivo di rete che non è in uscita al livello 3 (IP viene mantenuto) dovrebbe aggiungere l'indirizzo IP del client in ingresso all'intestazione x-inoltred standard del settore. 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Si sta tentando di ottenere altre attestazioni sull'endpoint informazioni utente, ma l'unico oggetto restituito. Come è possibile ottenere altre attestazioni?
L'endpoint UserInfo AD FS restituisce sempre l'attestazione Subject come specificato negli standard OpenID. AD FS non fornisce altre attestazioni richieste tramite l'endpoint UserInfo. Se sono necessarie altre attestazioni nel token ID, fare riferimento ai [token ID personalizzati in ad FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>Perché vengono visualizzati numerosi errori 1021 nei server di AD FS?
Questo evento viene registrato in genere per un accesso a risorse non valido su AD FS per la risorsa 00000003-0000-0000-C000-000000000000. Questo errore è causato da un comportamento errato del client in cui tenta di ottenere un token di accesso per il servizio Azure AD Graph. Poiché la risorsa non è presente in AD FS, viene restituito l'ID evento 1021 nei server di AD FS. È possibile ignorare gli avvisi o gli errori per la risorsa 00000003-0000-0000-C000-000000000000 in AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Perché viene visualizzato un avviso in cui si verifica un errore durante l'aggiunta dell'account del servizio AD FS al gruppo Enterprise Key Admins?
Questo gruppo viene creato solo quando si trova nel dominio un controller di dominio Windows 2016 con il ruolo FSMO PDC. Per risolvere l'errore, è possibile creare manualmente il gruppo e attenersi alla seguente procedura per concedere l'autorizzazione necessaria dopo aver aggiunto l'account del servizio come membro del gruppo.
1.  Aprire **Utenti e computer di Active Directory**.
2.  Fare **clic con il pulsante destro del mouse** sul nome di dominio nel riquadro di spostamento e **scegliere** proprietà.
3.  **Fare clic su** Sicurezza (se manca la scheda sicurezza, attivare funzionalità avanzate dal menu Visualizza).
4.  **Fare clic su** Avanzate. **Fare clic su** Aggiungere. **Fare clic su** Selezionare un'entità.
5.  Verrà visualizzata la finestra di dialogo Seleziona utente, computer, account servizio o gruppo.  Nella casella di testo immettere il nome dell'oggetto da selezionare digitare Key Admin Group.  Fare clic su OK.
6.  Nella casella di riepilogo si applica a selezionare **oggetti utente discendenti**.
7.  Utilizzando la barra di scorrimento, scorrere fino alla fine della pagina e **fare clic su** Cancella tutto.
8.  Nella sezione **Proprietà** selezionare **Read MSDS-KeyCredentialLink** e **scrivere MSDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Perché l'autenticazione moderna dai dispositivi Android ha esito negativo se il server non invia tutti i certificati intermedi nella catena con il certificato SSL?

Gli utenti federati possono provare l'autenticazione per Azure AD per le app che usano la libreria Android ADAL in errore. L'app otterrà un' **autenticazioneexception** quando tenterà di visualizzare la pagina di accesso. In Chrome la pagina di accesso AD FS potrebbe essere chiamata come unsafe.

Android-in tutte le versioni e tutti i dispositivi: non supporta il download di certificati aggiuntivi dal campo **authorityInformationAccess** del certificato. Questo vale anche per il browser Chrome. Il certificato di autenticazione server privo di certificati intermedi genererà questo errore se l'intera catena di certificati non viene passata da AD FS.

Una soluzione appropriata a questo problema consiste nel configurare i server AD FS e WAP per inviare i certificati intermedi necessari insieme al certificato SSL.

Quando si esporta il certificato SSL, da un computer, per essere importato nell'archivio personale del computer, dei server AD FS e WAP, assicurarsi di esportare la chiave privata e selezionare **scambio informazioni personali-PKCS #12**.

È importante che la casella di controllo **includa tutti i certificati nel percorso del certificato, se possibile** , sia selezionata, così come vengono **esportate tutte le proprietà estese**.  

Eseguire certlm. msc nei server Windows e importare *. PFX nell'archivio certificati personale del computer. In questo modo il server passerà l'intera catena di certificati alla libreria ADAL.

>[!NOTE]
> È necessario aggiornare anche l'archivio certificati dei bilanciamenti del carico di rete per includere l'intera catena di certificati se presente

### <a name="does-ad-fs-support-head-requests"></a>AD FS supporta le richieste HEAD?
AD FS non supporta le richieste HEAD.  Le applicazioni non devono utilizzare richieste HEAD per AD FS endpoint.  Questo può causare risposte di errore HTTP impreviste e/o ritardate.  Inoltre, è possibile che vengano visualizzati eventi di errore imprevisti nel registro eventi AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Perché non viene visualizzato un token di aggiornamento quando si effettua l'accesso con un IdP remoto?
Un token di aggiornamento non viene emesso se il token emesso da IdP ha un valida di meno di un'ora. Per assicurarsi che venga emesso un token di aggiornamento, aumentare la validità del token emesso da IdP a più di un'ora.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>È possibile modificare l'algoritmo di crittografia del token RP?
Per impostazione predefinita, la crittografia del token RP è impostata su AES256 e non può essere modificata in un altro valore.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>In una farm in modalità mista si verifica un errore quando si tenta di impostare il nuovo certificato SSL usando set-AdfsSslCertificate-identificazione personale. Come è possibile aggiornare il certificato SSL in modalità mista AD FS Farm?
La modalità mista AD FS le farm sono destinate a essere uno stato di transizione. Durante la pianificazione è consigliabile eseguire il rollover del certificato SSL prima del processo di aggiornamento oppure completare il processo e aumentare il livello di comportamento della farm prima di aggiornare il certificato SSL. Nel caso in cui non sia stato fatto, le istruzioni seguenti consentono di aggiornare il certificato SSL. 

Nei server WAP è ancora possibile usare set-WebApplicationProxySslCertificate. Nei server ADFS è necessario usare netsh. Seguire i passaggi indicati di seguito:

1. Selezionare un subset di server ADFS 2016 per la manutenzione (ad esempio, Rimuovi dal servizio di bilanciamento del carico)
2. Nei server selezionati in #1 importare il nuovo certificato tramite MMC
3. Elimina i certificati esistenti

    a. netsh http delete sslcert hostnameport = FS. contoso. com: 443 b. netsh http delete sslcert hostnameport = localhost: 443 c. netsh http delete sslcert hostnameport = FS. contoso. com: 49443

4.  Aggiungere il nuovo certificato

    a. netsh http add sslcert hostnameport = FS. contoso. com: 443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = MY SslCtlStoreName = AdfsTrustedDevices

    b. netsh http add sslcert hostnameport = localhost: 443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = MY SslCtlStoreName = AdfsTrustedDevices

    c. netsh http add sslcert hostnameport = FS. contoso. com: 49443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = MY SslCtlStoreName = AdfsTrustedDevices

5. Riavvia il servizio ADFS nel server selezionato
6. Rimuovere un subset di server WAP per la manutenzione
7. Nei server WAP selezionati importare il nuovo certificato tramite MMC
8. Impostare il nuovo certificato sul WAP usando il cmdlet

    a. Set-WebApplicationProxySslCertificate-identificazione personale "CERTTHUMBPRINT"

9. Riavvia il servizio sui server WAP selezionati
10. Inserire di nuovo i server WAP e AD FS selezionati nell'ambiente di produzione.

Eseguire l'aggiornamento sul resto dei server AD FS e WAP in modo analogo.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>ADFS è supportato quando i server proxy applicazione Web (WAP) sono protetti da Web Application Firewall (WAF) di Azure?
ADFS e i server applicazioni Web supportano qualsiasi firewall che non esegue la terminazione SSL sull'endpoint. Inoltre, i server ADFS/WAP includono meccanismi integrati per impedire attacchi Web comuni, ad esempio script tra siti, proxy ADFS e soddisfano tutti i requisiti definiti dal [protocollo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>Viene visualizzato un evento 441: È stato trovato un token con una chiave di associazione del token non valida. " Cosa devo fare per risolvere questo problema?
In AD FS 2016, l'associazione di token viene abilitata automaticamente e causa più problemi noti con scenari proxy e di federazione che generano questo errore. Per risolvere il problema, eseguire il comando PowerShell seguente e rimuovere il supporto per l'associazione di token.

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>Ho aggiornato la farm da AD FS in Windows Server 2016 per AD FS in Windows Server 2019. Il livello di comportamento della farm per la farm AD FS è stato elevato a 2019 ma la configurazione del proxy dell'applicazione Web è ancora visualizzata come Windows Server 2016?
Dopo l'aggiornamento a Windows Server 2019, la versione di configurazione del proxy dell'applicazione Web continuerà a essere visualizzata come Windows Server 2016. Il proxy dell'applicazione Web non dispone di nuove funzionalità specifiche della versione per Windows Server 2019 e se il livello di comportamento della farm è stato generato correttamente in AD FS, il proxy dell'applicazione Web continuerà a essere visualizzato come Windows Server 2016 da progettazione. 
