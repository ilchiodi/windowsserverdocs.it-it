---
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: Aggiornamenti necessari per Active Directory Federation Services (ADFS)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 434bf65c9f5ec977318b5efcdeebd19a5f1ed475
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>Aggiornamenti necessari per Active Directory Federation Services (ADFS) e Proxy applicazione Web (WAP)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1

A partire da ottobre 2016, tutti gli aggiornamenti a tutti i componenti di Windows Server vengono rilasciati solo tramite Windows Update (WU).  Sono disponibili senza ulteriori correzioni o singoli download.
Questo vale per Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 SP1.

Questa pagina elenca i pacchetti di aggiornamento cumulativo di particolare interesse per ADFS e WAP, nonché l'elenco di aggiornamenti rapidi consigliati per ADFS e WAP cronologico.

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>Aggiornamenti per ADFS e WAP in Windows Server 2016
Gli aggiornamenti per Windows Server 2016 vengono recapitati mensile tramite Windows Update e sono cumulativi. Il pacchetto di aggiornamento elencato di seguito è consigliato per tutti i server ADFS e WAP 2016 e include tutti gli aggiornamenti necessari in precedenza, nonché le correzioni più recenti.

|KB N. |Descrizione|Data di rilascio
|----- | ----- |-----
|[4041688 (OS Build 14393.1794)](https://support.microsoft.com/kb/4041688)|This fix addresses an issue that intermittently misdirects AD Authority requests to the wrong Identity Provider because of incorrect caching behavior. This can effect authentication features like Multi Factor Authentication. </br></br>Added the ability for AAD Connect Health to report ADFS server health with correct fidelity (using verbose auditing) on mixed WS2012R2 and WS2016 ADFS farms.</br></br>Fixed a problem where during upgrade of 2012 R2 ADFS farm to ADFS 2016, the powershell cmdlet to raise the farm behavior level fails with a timeout when there are many relying party trusts.</br></br>Addressed an issue where AD FS causes authentication failures by modifying the wct parameter value while federating the requests to other Security Token Server (STS).|October 2017|
|[4038801 (OS Build 14393.1737)](https://support.microsoft.com/kb/4038801)|Support added for OIDC logout using federated LDPs. This will allow "Kiosk Scenarios" where multiple users might be serially logged into a single device where there is federation with an LDP.</br></br>Fixed a WinHello issue where CEP/CES based certificates don't work with gMSA accounts.</br></br>Fixes a problem where the Windows Internal Database (WID) on Windows Server 2016 ADFS servers fails to sync some settings, such as the ApplicationGroupId columns from IdentityServerPolicy.Scopes and IdentityServerPolicy.Clients tables)  due to a foreign key constraint. Such sync failures can cause different claim, claim provider and application experiences between primary to secondary ADFS servers. Also, if the WID primary role is moved to a secondary node, application groups will no longer be manageable in the ADFS management UX.</br></br>This update fixes an issues where Multi Factor Authentication does not work correctly with Mobile devices that use custom culture definitions|September 2017|
|[4034661 (OS Build 14393.1613)](https://support.microsoft.com/kb/4034661)|Fixes a problem where the caller IP address is nog logged by 411 events in the Security Event log of ADFS 4.0 \ Windows Server 2016 RS1 ADFS servers even after enabling “success audits” and “failure audits”.</br></br>This fix addresses an issue with Azure Multi Factor Authentication (MFA) when an ADFX server is configured to use an HTTP Proxy.</br></br>"Addressed an issue where presenting an expired or revoked certificate to the ADFS Proxy server does not return an error to the user."|August 2017|
|[4034658 (OS Build 14393.1593)](https://support.microsoft.com/kb/4034658)|Fix for 2016 AD FS server in order to support MFA certificate enrollment for Windows Hello For Business for on prem deployments|August 2017| 
|[4025334 (OS Build 14393.1532)](https://support.microsoft.com/kb/4025334)|Addressed an issue where the PkeyAuth token handler could fail an authentication if the pkeyauth request contains incorrect data. The authentication should still continue without performing device authentication|July 2017|
|[4022723 (OS Build 14393.1378)](https://support.microsoft.com/kb/4022723)|[Web Application Proxy] Value of DisableHttpOnlyCookieProtection configuration property is not picked up by WAP 2016 in 2012R2/2016 mixed deployment </br></br>[Web Application Proxy] Unable to obtain user access token from AD FS in EAS Pre-auth scenarios.</br></br>AD FS 2016 : WSFED sign-out leads to an exception|Giugno 2017 
|[3213986](https://support.microsoft.com/kb/3213986)|Aggiornamento cumulativo per Windows Server 2016 per sistemi basati su x64 (KB3213986)| Gennaio 2017

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>Aggiornamenti per ADFS e WAP in Windows Server 2012 R2
Di seguito è riportato l'elenco di aggiornamenti rapidi e gli aggiornamenti cumulativi che sono stati rilasciati per Active Directory Federation Services (ADFS) in Windows Server 2012 R2.

|KB N. |Descrizione|Data di rilascio
|----- | ----- |-----
|[4041685](https://support.microsoft.com/kb/4041685)|Addressed an AD FS issue where MSISConext cookies in request headers can eventually overflow the headers size limit and cause failure to authenticate with HTTP status code 400 “Bad Request - Header Too Long".</br></br>Fixed a problem where ADFS can no longer ignore "prompt=login" during authentication. A "Disabled" option was added to restore scenarios where non-password authentication is used.|October 2017 Preview of Update Rollup|
|[4019217](https://support.microsoft.com/kb/4019217)|Work Folders clients using token broker do not work when using a Server 2012 R2 AD FS Server|May 2017 Preview Update Rollup| 
|[4015550](https://support.microsoft.com/kb/4015550)|Fixed an issue with AD FS not authenticating External users and AD FS WAP randomly failing to forward request|April 2017 Update Rollup| 
|[4015547](https://support.microsoft.com/kb/4015547)|Fixed an issue with AD FS not authenticating External users and AD FS WAP randomly failing to forward request|April 2017 Security Update| 
|[4012216](https://support.microsoft.com/kb/4009970)|MS17-019 This security update resolves a vulnerability in Active Directory Federation Services (ADFS). The vulnerability could allow information disclosure if an attacker sends a specially crafted request to an AD FS server, allowing the attacker to read sensitive information about the target system.|March 2017 Update Rollup| 
|[3179574](https://support.microsoft.com/kb/3179574)|Risoluzione del problema con l'aggiornamento della password extranet AD FS. |Aggiornamento cumulativo di agosto 2016
|[3172614](https://support.microsoft.com/kb/3172614)|Prompt dei comandi introdotti = accesso [supportano](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/ad-fs-faq#BKMK_7), risolto un problema con la console di gestione di ADFS e AlwaysRequireAuthentication impostazione. |Aggiornamento cumulativo di luglio 2016
|[3163306](https://support.microsoft.com/kb/3163306)|Active Directory Federation Services (ADFS) 3.0 non è possibile connettersi archivi di attributi Lightweight Directory Access Protocol (LDAP) che sono configurati per utilizzare la porta Secure Sockets Layer (SSL) 636 o 3269 nella stringa di connessione. |Aggiornamento cumulativo di giugno 2016
|[3148533](https://support.microsoft.com/kb/3148533)|Errore dell'autenticazione fallback autenticazione a più fattori tramite Proxy ADFS in Windows Server 2012 R2 |Maggio 2016
|[3134787](https://support.microsoft.com/kb/3134787)|Registri di ADFS non contengono l'indirizzo IP del client per gli scenari di blocco degli account in Windows Server 2012 R2 |Febbraio 2016
|[3134222](https://support.microsoft.com/kb/3134222)|MS16-020: Aggiornamento della sicurezza di Active Directory Federation Services di negazione di indirizzo del servizio: 9 febbraio 2016|Febbraio 2016
|[3105881](https://support.microsoft.com/kb/3105881)|Non è in grado di accedere alle applicazioni quando è abilitata l'autenticazione del dispositivo nel server ADFS basato su Windows Server 2012 R2|Ottobre 2015
|[3092003](https://support.microsoft.com/kb/3092003)|Caricamento della pagina più volte e autenticazione non riesce quando gli utenti utilizzano l'autenticazione a più fattori in Windows Server 2012 R2 AD ADFS|Agosto 2015
|[3080778](https://support.microsoft.com/kb/3080778)|ADFS non chiama OnError quando scheda MFA genera un'eccezione in Windows Server 2012 R2|Luglio 2015
|[3075610](https://support.microsoft.com/kb/3075610)|Relazioni di trust vengono persi nel server secondario di ADFS dopo l'aggiunta o rimozione di provider di attestazioni in Windows Server 2012 R2|Luglio 2015
|[3070080](https://support.microsoft.com/kb/3070080)|Individuazione principale dell'area di autenticazione non funziona correttamente per Non-claims compatibile con Trust della Relying Party|Giugno 2015
|[3052122](https://support.microsoft.com/kb/3052122)|Aggiornamento aggiunge il supporto per composta ID attestazioni nei token AD FS in Windows Server 2012 R2|Maggio 2015
|[3045711](https://support.microsoft.com/kb/3045711)|MS15-040: Una vulnerabilità in Active Directory Federation Services può consentire la divulgazione di informazioni|Aprile 2015
|[3042127](https://support.microsoft.com/kb/3042127)|"HTTP 400 - richiesta non valida" quando si apre una cassetta postale condivisa tramite WAP in Windows Server 2012 R2|Marzo 2015
|[3042121](https://support.microsoft.com/kb/3042121)|AD FS riproduzione token protezione per i token di autenticazione Proxy applicazione Web in Windows Server 2012 R2|Marzo 2015
|[3035025](https://support.microsoft.com/kb/3035025)|Funzionalità di correzione rapida per l'aggiornamento della password in modo che gli utenti non devono utilizzare dispositivo registrato in Windows Server 2012 R2|Gennaio 2015
|[3033917](https://support.microsoft.com/kb/3033917)|ADFS non è in grado di elaborare la risposta SAML in Windows Server 2012 R2|Gennaio 2015
|[3025080](https://support.microsoft.com/kb/3025080)|Operazione non riesce quando si tenta di salvare un file di Office mediante Proxy applicazione Web in Windows Server 2012 R2|Gennaio 2015
|[3025078](https://support.microsoft.com/kb/3025078)|Non richiesto per il nome utente nuovamente quando si utilizza un nome utente non corretto per accedere a Windows Server 2012 R2|Gennaio 2015
|[3020813](https://support.microsoft.com/kb/3020813)|Richiesto per l'autenticazione quando si esegue un'applicazione web in Windows Server 2012 R2 AD ADFS|Gennaio 2015
|[3020773](https://support.microsoft.com/kb/3020773)|Errori di timeout dopo la distribuzione iniziale del servizio di registrazione del dispositivo in Windows Server 2012 R2|Gennaio 2015
|[3018886](https://support.microsoft.com/kb/3018886)|Viene richiesto un nome utente e password due volte quando si accede a Windows Server 2012 R2 AD ADFS dalla rete intranet|Gennaio 2015
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 Update Rollup|Dicembre 2014
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 Update Rollup|Novembre 2014
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 Update Rollup|Agosto 2014
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 Update Rollup|Luglio 2014
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 Update Rollup|Giugno 2014
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 Update Rollup|Maggio del 2014
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 Update Rollup|Aprile 2014

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>Gli aggiornamenti per ADFS in Windows Server 2012 (AD FS 2.1) e AD FS 2.0
Di seguito è riportato l'elenco di aggiornamenti rapidi e gli aggiornamenti cumulativi che sono stati rilasciati per AD FS 2.0 e 2.1.

|KB N. |Descrizione|Data di rilascio|Si applica a:
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|L'autenticazione tramite proxy ha esito negativo in Windows Server 2012 (questo è il rilascio generale di hotfix 3094446)|Aggiornamento cumulativo qualitativo di novembre 2016|ADFS 2.1
|[3197869](https://support.microsoft.com/kb/3197869)|L'autenticazione tramite proxy ha esito negativo in Windows Server 2008 R2 SP1 (questo è il rilascio generale di hotfix 3094446)|Aggiornamento cumulativo qualitativo di novembre 2016|AD FS 2.0
|[3094446](https://support.microsoft.com/kb/3094446)|L'autenticazione tramite proxy ha esito negativo in Windows Server 2012 o Windows Server 2008 R2 SP1|Settembre 2015|AD FS 2.0 e 2.1
|[3070078](https://support.microsoft.com/kb/3070078)|ADFS 2.1 genera un'eccezione durante l'autenticazione rispetto a un certificato di crittografia in Windows Server 2012|Luglio 2015|ADFS 2.1
|[3062577](https://support.microsoft.com/kb/3062577)|MS15-062: Una vulnerabilità in Active Directory federation services potrebbe consentire l'elevazione dei privilegi|Giugno 2015|ADFS 2.0 o 2.1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077: Una vulnerabilità in Active Directory Federation Services può consentire l'intercettazione di informazioni personali: 14 aprile 2015|Novembre 2014|ADFS 2.0 o 2.1
|[2987843](https://support.microsoft.com/kb/2987843)|Utilizzo della memoria del server federativo ADFS continua ad aumentare quando molti utenti accedono a un'applicazione web in Windows Server 2012|Luglio 2014|ADFS 2.1
|[2957619](https://support.microsoft.com/kb/2957619)|Trust della relying party in ADFS viene interrotta quando viene effettuata una richiesta ad ADFS per un token di delegati|Maggio del 2014|ADFS 2.1
|[2926658](https://support.microsoft.com/kb/2926658)|Distribuzione di farm di ADFS SQL ha esito negativo se non si dispone delle autorizzazioni di SQL|Ottobre 2014|ADFS 2.1
|[2896713](https://support.microsoft.com/kb/2896713) or [2989956](https://support.microsoft.com/kb/2989956)|Aggiornamento è disponibile per risolvere diversi problemi dopo l'installazione dell'aggiornamento 2843638 su un server AD FS|Novembre 2013</br></br>Settembre 2014|ADFS 2.0 o 2.1
|[2877424](https://support.microsoft.com/kb/2877424)|Aggiornamento consente di utilizzare un certificato per più Relying Party trust in un'istanza di ADFS farm 2.1|Ottobre 2013|ADFS 2.1
|[2873168](https://support.microsoft.com/kb/2873168)|FIX: Si verifica un errore quando si utilizza un CSP di terze parti e le stesse e quindi configurare un trust del provider di attestazioni nell'aggiornamento cumulativo 3 per AD FS 2.0 in Windows Server 2008 R2 Service Pack 1|Settembre 2013|AD FS 2.0
|[2861090](https://support.microsoft.com/kb/2861090)|Una virgola nel nome soggetto di un certificato di crittografia genererà un'eccezione in Windows Server 2008 R2 SP1|Agosto 2013|AD FS 2.0
|[2843639](https://support.microsoft.com/kb/2843639)|[Sicurezza] Una vulnerabilità in Active Directory Federation Services può consentire la divulgazione di informazioni|Novembre 2013|ADFS 2.1
|[2843638](https://support.microsoft.com/kb/2843638)|MS13-066: Descrizione dell'aggiornamento della sicurezza di Active Directory Federation Services 2.0: 13 agosto 2013|Agosto 2013|AD FS 2.0
|[2827748](https://support.microsoft.com/kb/2827748)|File FederationMetadata XML non contiene le informazioni sull'endpoint MEX per gli endpoint WS-Trust e WS-Federation in Windows Server 2012|Maggio 2013|ADFS 2.1
|[2790338](https://support.microsoft.com/kb/2790338)|Descrizione dell'aggiornamento cumulativo 3 per Active Directory Federation Services (ADFS) 2.0|Marzo 2013|AD FS 2.0




