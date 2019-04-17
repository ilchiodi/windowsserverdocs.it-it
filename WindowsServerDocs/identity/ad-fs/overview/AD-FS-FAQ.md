---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: DOMANDE FREQUENTI DI AD FS 2016
description: Domande frequenti per AD FS 2016
author: jenfieldmsft
ms.author: billmath
manager: femila
ms.date: 03/06/2018
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 313447d2c92c15505434ec5c39898ca84aef46db
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Domande frequenti su AD FS

>Si applica a: Windows Server 2016

La seguente documentazione è una specie di domande frequenti per Active Directory Federation Services.  Il documento è stato suddiviso in gruppi in base al tipo di domanda.

## <a name="deployment"></a>Distribuzione 

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Come posso la aggiornamento/migrazione da versioni precedenti di ADFS
È possibile eseguire l'aggiornamento di ADFS con uno dei seguenti:


- Windows Server 2012 R2 AD ADFS in Windows Server 2016 AD ADFS
    - [L'aggiornamento a AD FS in Windows Server 2016 tramite un database interno di Windows](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [L'aggiornamento a AD FS in Windows Server 2016 tramite un database SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD ADFS in Windows Server 2012 R2 AD ADFS
    - [Eseguire la migrazione a AD FS in Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 per Windows Server 2012 AD ADFS
    - [Eseguire la migrazione a AD FS in Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1. x ADFS 2.0 
    - [L'aggiornamento da AD FS 1. x ADFS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Se è necessario eseguire l'aggiornamento da ADFS 2.0 o 2.1 (Windows Server 2008 R2 o Windows Server 2012), è necessario utilizzare gli script nella casella (nello C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Perché l'installazione di ADFS richiede un riavvio del server?

In Windows Server 2016 è stato aggiunto il supporto HTTP/2, ma HTTP/2 può essere utilizzato per l'autenticazione del certificato client.  Perché molti scenari di ADFS uso dell'autenticazione del certificato client e un numero significativo di ai client non supporta verranno eseguiti altri tentativi richieste che utilizzano HTTP/1.1, ADFS farm configurazione nuovamente consente di configurare le impostazioni HTTP del server locale a HTTP/1.1.  Richiede un riavvio del server.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>Usa Windows 2016 WAP server per pubblicare la farm ADFS a internet senza eseguire l'aggiornamento della farm di ADFS back-end è supportata?
Sì, questa configurazione è supportata, ma non include nuove funzionalità AD FS 2016 sarebbe supportato in questa configurazione.  Questa configurazione è progettata per essere temporaneo durante la fase di migrazione da AD FS 2012 R2 con AD FS 2016 e non deve essere distribuita per lunghi periodi di tempo.

## <a name="design"></a>Progettazione

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quali provider di autenticazione a più fattori di terze parti sono disponibili per ADFS? 
Di seguito è riportato un elenco dei provider di terze parti che siamo consapevoli che.  È sempre possibile dei provider disponibili che non si dispone di informazioni e l'elenco verrà aggiornato come abbiamo informazioni su di essi.

- [Gemalto identità e servizi di sicurezza](http://www.gemalto.com/identity)
- [Servizio di autenticazione Enterprise inWebo](http://www.inwebo.com/)
- [Connettore di login People MFA API](https://www.loginpeople.com)
- [Agente di autenticazione RSA SecurID per Microsoft Active Directory Federation Services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [SafeNet Authentication Service (SAS) agente per AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Servizio di autenticazione ID Swisscom Mobile](http://swisscom.ch/mid)
- [Symantec convalida e il servizio di protezione ID (VIP)](http://www.symantec.com/vip-authentication-service) 

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Proxy di terze parti sono supportate con AD FS?
Sì, il proxy di terze parti può essere posizionato davanti il Proxy dell'applicazione Web, ma qualsiasi proxy di terze parti deve supportare il [protocollo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) per essere usato al posto di Proxy applicazione Web.

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>Il proxy di terze parti sono disponibili per ADFS che supportano MS-ADFSPIP?

Di seguito è riportato un elenco dei provider di terze parti che siamo consapevoli che.  È sempre possibile dei provider disponibili che non si dispone di informazioni e l'elenco verrà aggiornato come abbiamo informazioni su di essi.

- [F5 Gestione dei criteri di accesso](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>In cui è la pianificazione della capacità del foglio di calcolo di ridimensionamento per AD FS 2016?
La versione di AD FS 2016 del foglio di calcolo può essere scaricata [qui](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Può essere utilizzato anche per ADFS in Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Come posso per assicurare che il supporto di server ADFS e WAP requisiti ATP Apple?

Apple ha rilasciato un insieme di requisiti denominato sicurezza trasporto App (ATS) che può influire sulle chiamate da app iOS che eseguono l'autenticazione ad ADFS.  È possibile garantire l'ADFS e WAP server soddisfino verificando che supportano il [requisiti per la connessione utilizzando ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
In particolare, è necessario verificare che i server ADFS e WAP supportano TLS 1.2 e che il pacchetto di crittografia negoziata della connessione TLS supporterà la funzionalità PFS.

È possibile abilitare e disabilitare le versioni di SSL 2.0 e 3.0 e TLS 1.0, 1.1 e 1.2 utilizzando [gestire i protocolli SSL in AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Per garantire l'ADFS e WAP server negoziano soli pacchetti di crittografia TLS che supportano ATP, è possibile disabilitare tutti i pacchetti di crittografia che non sono presenti nel [elenco dei pacchetti di crittografia conformi ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  A tale scopo, utilizzare il [cmdlet di Windows PowerShell TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index). 


## <a name="operations"></a>Operazioni

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Come sostituire il certificato SSL per ADFS 
Il certificato SSL ADFS non è lo stesso certificato per le comunicazioni servizio ADFS disponibile nello snap-in Gestione ADFS.  Per modificare il certificato SSL ADFS, è necessario utilizzare PowerShell. Seguire le istruzioni nell'articolo seguente:

[La gestione di certificati SSL in ADFS e WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Come è possibile abilitare o disabilitare le impostazioni di TLS/SSL per ADFS
Per disabilitare o abilitare i protocolli SSL e pacchetti di crittografia, attenersi a quanto segue:

[Gestire i protocolli SSL in ADFS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Il certificato SSL del proxy deve essere lo stesso certificato SSL ADFS?  
Utilizzare le seguenti linee guida per il certificato SSL del proxy e il certificato SSL ADFS:


- Se il proxy viene utilizzato per le richieste proxy ADFS che utilizzano l'autenticazione integrata di Windows, il certificato SSL del proxy devono essere lo stesso (utilizzare la stessa chiave) come il certificato SSL del server federativo
- Se la proprietà di AD FS "ExtendedProtectionTokenCheck" è abilitata (l'impostazione predefinita in AD FS), il certificato SSL del proxy deve essere lo stesso (utilizzare la stessa chiave) come il certificato SSL del server federativo
- In caso contrario, il certificato SSL del proxy può avere una chiave diversa dal certificato SSL ADFS, ma deve soddisfare lo stesso [requisiti](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>Come posso configurare richiesta = comportamento di accesso per AD FS?
Per informazioni su come configurare prompt = account di accesso, vedere [Active Directory Federation Services prompt = accesso parametro supporto](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Come è possibile configurare i browser di usare l'autenticazione integrata Windows (WIA) con AD FS

Per informazioni su come configurare i browser vedere [configurare browser per utilizzare l'autenticazione integrata Windows (WIA) con AD FS](../operations/Configure-AD-FS-Browser-WIA.md).


### <a name="how-long-are-ad-fs-tokens-valid"></a>Quanto tempo sono i token AD FS validi?

Questa domanda indica spesso 'quanto tempo gli utenti ottengono single sign-on (SSO) senza dover immettere le nuove credenziali, e come è possibile in qualità di amministratore controllare che'?  Questo comportamento e le impostazioni di configurazione che controllano, sono descritti nell'articolo [qui](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

La durata predefinita dei vari token e i cookie è elencata di seguito (nonché i parametri che controllano la durata di):

**Dispositivi registrati**

- I cookie PRT e SSO: 90 giorni di massimi, disciplinati dalle PSSOLifeTimeMins. (Dispositivo specificato viene utilizzato almeno ogni 14 giorni, che è controllata dal DeviceUsageWindow)

- Aggiornare token: calcolato in base dell'esempio precedente per offrire un comportamento coerente

- access_token: 1 ora per impostazione predefinita, in base alla relying party

- id_token: come token di accesso

**Annullare la registrazione di dispositivi**

- I cookie SSO: 8 ore per impostazione predefinita, è disciplinato dal SSOLifetimeMins.  Quando è abilitata mantenere Me connesso (KMSI), valore predefinito è 24 ore e configurabile tramite KMSILifetimeMins.


- Aggiornare token: 8 ore per impostazione predefinita. 24 ore con KMSI abilitato

- access_token: 1 ora per impostazione predefinita, in base alla relying party

- id_token: come token di accesso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>ADFS supporta HTTP Strict Transport Security HSTS ()?  

HTTP Strict Transport Security HSTS () è un meccanismo di criteri di sicurezza web quali riduce i rischi di attacchi di downgrade di protocollo e Hijack cookie per i servizi che presentano endpoint HTTP e HTTPS. Consente il server web dichiarare che i web browser (o altri agenti utente a soddisfare) devono solo interagiscono con HTTPS e mai tramite il protocollo HTTP.

Tutti gli endpoint ADFS per il traffico di autenticazione web vengono aperti esclusivamente tramite HTTPS.  Di conseguenza, ADFS attenua in modo efficace le minacce che fornisce il meccanismo di criteri HTTP Strict Transport Security (per impostazione predefinita non vi è alcun downgrade a HTTP poiché non sono presenti listener HTTP). AD FS impedisce inoltre i cookie vengano inviate a un altro server con gli endpoint di protocollo HTTP contrassegnando tutti i cookie con il flag di protezione.

Di conseguenza, l'implementazione di HSTS su un server ADFS non è necessario perché non è possibile eseguire il downgrade.  Per motivi di conformità, server AD FS soddisfare questi requisiti, perché non possono utilizzare HTTP e tutti i cookie vengono contrassegnati sicuri.

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-inoltrati-client-ip non contiene l'indirizzo IP del client, ma contiene IP del firewall davanti il proxy. In cui è possibile reperire l'indirizzo IP corretto del client?
Non è consigliabile eseguire la terminazione SSL prima WAP. Nel caso in cui viene eseguita la terminazione SSL davanti il WAP, X-ms-inoltrati-client-ip conterrà l'indirizzo IP del dispositivo di rete davanti WAP. Di seguito viene fornita una breve descrizione di IP diversi attestazioni che sono supportate da AD FS:
 - x-ms-client-ip: rete IP di dispositivi connessi al servizio token di sicurezza.  Nel caso di una richiesta extranet sempre contiene l'indirizzo IP nel WAP.
 - x-ms-inoltrati-client-ip: più valori attestazione che conterrà i valori inoltrati ad ADFS da Exchange Online e l'indirizzo IP del dispositivo che è connesso a nel WAP.
 - Userip: Per le richieste extranet questa attestazione conterrà il valore di x-ms-inoltrati-client-ip.  Per le richieste di rete intranet, l'attestazione contiene lo stesso valore x-ms-client-ip.

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Sto cercando di ottenere le attestazioni aggiuntive nell'endpoint informazioni utente, ma restituisce solo soggetto. Come posso ottenere altre attestazioni?
Informazioni sull'utente endpoint ADFS restituisce sempre l'attestazione di oggetto come specificato negli standard OpenID. ADFS non fornisce altre richieste tramite l'endpoint di informazioni sull'utente. Se hai bisogno di ulteriori attestazioni nel token ID, fare riferimento a [token ID personalizzati in ADFS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-alot-of-1021-errors-on-my-ad-fs-servers"></a>Why do I see alot of 1021 errors on my AD FS servers?
This event is logged usually for an invalid resource access on AD FS for resource 00000003-0000-0000-c000-000000000000. This error is caused by an erroneous behavior of the client where it tries to get an access token for the Azure AD Graph service. Since the resource is not present on AD FS, this results in event ID 1021 on the AD FS servers. It’s safe to ignore any warnings or errors for resource 00000003-0000-0000-c000-000000000000 on AD FS. 

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Why am I seeing a warning for failure to add the AD FS service account to the Enterprise Key Admins group?
This group is only created when a Windows 2016 Domain Controller with the FSMO PDC role exists in the Domain. To resolve the error, you can create the Group manually and follow the below to give the required permission after adding the service account as member of the group.
1.  Open **Active Directory Users and Computers**. 
2.  **Right-click** your domain name from the navigation pane and **click** Properties.
3.  **Click** Security (if the Security tab is missing, turn on Advanced Features from the View menu).
4.  **Click** Advanced. **Click** Add. **Click** Select a principal.
5.  The Select User, Computer, Service Account, or Group dialog box appears.  In the Enter the object name to select text box, type Key Admin Group.  Fare clic su OK.
6.  In the Applies to list box, select **Descendant User objects**.
7.  Using the scroll bar, scroll to the bottom of the page and **click** Clear all.
8.  In the **Properties** section, select **Read msDS-KeyCredentialLink** and **Write msDS-KeyCrendentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Why does modern authentication from Android devices fail if the server does not send all the intermediate certificates in the chain with the SSL cert?

Federated users may experience authentication to Azure AD for apps that use the Android ADAL library failing. The app will get an **AuthenticationException** when it tries to show the login page. In chrome the AD FS login page might be called out as unsafe.

Android - across all versions and all devices - does not support downloading additional certificates from the **authorityInformationAccess** field of the certificate. This is true of the Chrome browser as well. Any Server Authentication certificate that’s missing intermediate certificates will result in this error if the entire certificate chain is not passed from AD FS. 

A proper solution to this problem is to configure the AD FS and WAP servers to send the necessary intermediate certificates along with the SSL certificate. 

When exporting the SSL certificate, from one machine, to be imported to the computer’s personal store, of the AD FS and WAP server(s), make sure to export the Private key and select **Personal Information Exchange - PKCS #12**. 

It is important that the check box to **Include all certificates in the certificate path if possible** is checked, as well as **Export all extended properties**. 

Run certlm.msc on the Windows servers and import the *.PFX into the Computer’s Personal Certificate store. This will cause the server to pass the entire certificate chain to the ADAL library. 

>[!NOTE]
> The certificate store of Network Load Balancers should also be updated to include the entire certificate chain if present

### <a name="does-ad-fs-support-head-requests"></a>Does AD FS support HEAD requests?
AD FS does not support HEAD requests.  Applications should not be using HEAD requests against AD FS endpoints.  This may cause HTTP error responses that are unexpected and/or delayed.  Additionally, you may see unexpected error events in the AD FS event log.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Perché viene non visualizzato un token di aggiornamento se si è eseguito con un IdP remoto?
Un token di aggiornamento non viene generato se il token emesso da IdP ha un validty di meno di 1 ora. Per garantire che un token di aggiornamento viene emesso, aumentare la validità del token emesso da IdP per più di 1 ora.
