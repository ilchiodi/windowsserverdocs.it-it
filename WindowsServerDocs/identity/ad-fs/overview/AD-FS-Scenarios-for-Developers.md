---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: Scenari di AD FS per gli sviluppatori
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb1bc5776ea4d24f274c79563d9e346b104de6d9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444226"
---
# <a name="ad-fs-scenarios-for-developers"></a>Scenari di AD FS per gli sviluppatori


ADFS in Windows Server 2016 [AD FS 2016] consente di aggiungere settore standard OpenID Connect e OAuth 2.0 basato su autenticazione e autorizzazione alle applicazioni sviluppate e dispone di tali applicazioni autenticare gli utenti direttamente in ADFS.    
  
AD FS 2016 supporta anche il WS-Federation, WS-Trust e SAML protocolli e dei profili si sono supportati nelle versioni precedenti.  Se si è interessati alle linee guida per sviluppatori per questi protocolli, vedere l'articolo.  Questo articolo verrà illustrato solo come utilizzare e trarre vantaggio dal supporto del protocollo più recente.  
  
## <a name="why-modern-authentication"></a>Perché l'autenticazione moderna  
Sebbene sia possibile continuare con ADFS per l'accesso in con WS-Federation, WS-Trust e i protocolli SAML come hanno prima, con i protocolli più recenti, si ottengono i vantaggi seguenti:  
  
* **Semplicità e coerenza**  
    * Utilizzare lo stesso set di API e modelli di abilitazione dell'accesso per:   
        *   più tipi di applicazioni (server, desktop, mobili e browser)  
        *   più piattaforme (android, iOS, Windows)  
        *   le applicazioni all'interno della rete azienda o ospitati nel cloud  
    * Utilizzare lo stesso set di librerie che già possibile usare per autenticare gli utenti in Azure AD  
* **Flexibility**  
    * Oltre alle autorizzazioni utente standard, abilitare scenari più complessi, ad esempio:      
        * ? accesso in 3 fasi sui flussi in cui un utente autorizza un'applicazione web o un servizio per accedere alle risorse che risiedono in un'altra app web o un servizio.    
        * ? Flussi di server-server in cui un servizio di livello intermedio accede a un back-end di API  
        * ? Applicazioni a pagina singola (SPA) basate su JavaScript  
* **Standard del settore**  
    * OAuth 2.0 e OpenID Connect sfruttare ampio utilizzo nel settore, in modo conoscenza di questi modelli consentono di abilitare l'autenticazione e autorizzazione di fuori di un ambiente Active Directory nonché  
  
## <a name="how-it-works-the-basics"></a>Come funziona: Le nozioni di base  
È possibile aggiungere l'autenticazione moderna di ADFS per l'applicazione utilizzando lo stesso set di strumenti e librerie che è già possibile utilizzare per autenticare gli utenti AD Azure.   
  
Negli scenari di ADFS, naturalmente, è AD FS e non Azure AD che funge da provider di identità e autorizzazione server.  In caso contrario i concetti sono esattamente uguali: utenti devono fornire le proprie credenziali e ottenere i token, direttamente o tramite un intermediario, per accedere alle risorse.  
  
Lo scenario più semplice è costituito da un utente o il "proprietario della risorsa", l'interazione con un browser per accedere a un'applicazione web:  
  
![ADFS per gli sviluppatori](media/ADFS_DEV_1.png)  
  
L'applicazione web chiamato "client" in quanto inizia la richiesta al server di autorizzazione (AD FS) per un token di accesso alla risorsa.  La risorsa può essere ospitata dal sito web stesso o sia accessibile come un'API web in un punto qualsiasi nella rete o internet.   L'utente o il "proprietario della risorsa" autorizza l'applicazione web client di ricevere il token di accesso, fornendo le credenziali per il server di autorizzazione.    
  
## <a name="how-it-works-components"></a>Come funziona: componenti  
Utilizzare lo stesso set di strumenti e librerie da utilizzare quando Azure AD è il provider di identità OpenID Connect scenari nel rendere ADFS e OAuth 2.0.  Questi componenti sono:  
* Active Directory Authentication Library (ADAL): le librerie client che semplificano raccolta delle credenziali utente, creazione e invio di richieste di token e il recupero dei token risultanti.    
* Middleware OWIN (Open Web Interface for .NET): OWIN è un progetto basato su community, Microsoft ha creato un set di librerie sul lato server che per la protezione di applicazioni web e API web con OpenID Connect e OAuth 2.0  
  
I ruoli di questi componenti vengono visualizzati nel diagramma seguente:  
  
![ADFS per gli sviluppatori](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>Questi scenari di modellazione in AD FS 2016  
  
### <a name="application-groups"></a>Gruppi per le applicazioni  
Per rappresentare questi scenari in Criteri di ADFS, è stata introdotta un nuovo concetto, ovvero gruppi di applicazioni.  Un gruppo di applicazioni può contenere qualsiasi numero e combinazione dei seguenti tipi fondamentali dell'applicazione:  
  
  
  
Gruppo di applicazioni / tipo di applicazione  |Descrizione  |Ruolo    
---------|---------|---------  
Applicazione nativa     |  A volte chiamato un client pubblico, questo deve essere un'applicazione client che viene eseguito in un dispositivo o un pc e con cui interagisce l'utente.       | Token richieste dal server di autorizzazione (AD FS) per l'accesso utente alle risorse.  Invia richieste HTTP alle risorse protette, utilizzando il token come intestazioni HTTP.        
Applicazione server     |   Un'applicazione web che viene eseguito in un server ed è in genere accessibili agli utenti tramite un browser.  Poiché è in grado di mantenere il proprio segreto' client' o credenziali, viene chiamato talvolta un client riservato.      | Token richieste dal server di autorizzazione (AD FS) per l'accesso utente alle risorse.  Invia richieste HTTP alle risorse protette, utilizzando il token come intestazioni HTTP.               
API Web     |  La risorsa finale, l'utente accede. Se si trattasse di come la nuova rappresentazione "relying party".| Utilizza i token ottenuti dai client  
  
### <a name="differences-from-ad-fs-2012-r2"></a>Differenze rispetto AD FS 2012 R2  
Gruppi di applicazioni di combinano gli elementi di trust e l'autorizzazione AD FS 2012 R2 esposte separatamente, come relying party, client e autorizzazioni per l'applicazione.  
  
Nelle tabelle seguenti vengono confrontati i metodi mediante il quale gli oggetti di attendibilità dell'applicazione corrispondente vengono creati in AD FS 2012 R2 vs AD FS 2016:  
  
AD FS in Windows Server 2012 R2|In PowerShell|Gestione di ADFS  
---------|---------|---------  
Aggiungere native client|Aggiungere AdfsClient|N/D  
Aggiungere l'applicazione server come client|Aggiungere AdfsClient|N/D  
Aggiungere l'API Web / risorse|Aggiungere-AdfsRelyingPartyTrust|Creare Trust della Relying Party  
  
AD FS 2016|In PowerShell|Gestione di ADFS  
---------|---------|---------  
Aggiungere native client|Aggiungere AdfsNativeClientApplication|Aggiungere il gruppo di applicazioni Native  
Aggiungere l'applicazione server come client|Aggiungere AdfsServerApplication|Aggiungere il gruppo di applicazioni Server  
Aggiungere l'API Web / risorse|Aggiungere AdfsWebApiApplication|Aggiungi gruppo di un'applicazione all'API Web  
  
### <a name="application-permissions-and-consent"></a>Consenso e autorizzazioni per l'applicazione  
Per impostazione predefinita, i client in un gruppo di applicazioni sono consentiti per accedere alle risorse nello stesso gruppo.  Non è necessario che l'amministratore di configurare le autorizzazioni specifiche dell'applicazione.  Gruppi di applicazioni consentono inoltre agli amministratori di specificare gli ambiti può, ad esempio openid o user_impersonation.  Le descrizioni degli scenari seguenti specificare esattamente quali ambiti sono necessari per la quale scenario.  
  
Poiché ADFS utilizza un modello di consenso dell'amministratore, gli utenti non vengono richiesto il consenso quando accedono alle risorse.  Configurando il gruppo di applicazioni, l'amministratore attiva fornisce il consenso per conto di tutti gli utenti dell'applicazione.  
  
## <a name="supported-scenarios"></a>Scenari supportati  
Nella sezione seguente vengono descritti gli scenari che sono supportati in modo più dettagliato.  
  
### <a name="tokens-used"></a>Token utilizzati  
Rendere questi scenari di utilizzo di tre tipi di token:  
  
* **id_token:** Un token JWT usato per rappresentare l'identità dell'utente. L'attestazione 'aud' o gruppo di destinatari di id_token corrisponde all'ID client dell'applicazione nativa o server.  
* **access_token:** Un token JWT usato in Oauth e OpenID connect scenari ed è progettato per essere utilizzato per la risorsa.  L'attestazione 'aud' o gruppo di destinatari di questo token deve corrispondere all'identificatore della risorsa o dell'API Web.  
* **refresh_token:** Questo token viene inviato al posto di raccolta delle credenziali utente per fornire un solo segno di esperienza.  Questo token viene rilasciato e utilizzato da ADFS e non è leggibile dai client o risorse.    
  
### <a name="native-client-to-web-api"></a>Native client per l'API Web  
Questo scenario consente all'utente di un'applicazione client nativa per chiamare un'API Web di AD FS 2016 protetti.  
* L'applicazione client nativa Usa ADAL per inviare l'autorizzazione e le richieste ad ADFS, vengono richieste le credenziali dell'utente in base alle esigenze, quindi invia il token risultante come un'intestazione HTTP nella richiesta all'API Web  
* [Questa parte è solo per scopi dimostrativi] L'API web legge le attestazioni dall'oggetto ClaimsPrincipal che risulta dal token di accesso inviato dal client e li invia al client.  
  
![Descrizione del flusso del protocollo](media/ADFS_DEV_3.png)  
  
1.  L'applicazione client nativa avvia il flusso con una chiamata per la libreria ADAL.  Questo attiva un browser basato su HTTP GET per ADFS endpoint di autorizzazione:  
  
**Richiesta di autorizzazione:**  
GET <https://fs.contoso.com/adfs/oauth2/authorize?>  
  
Parametro|Value  
---------|---------  
response_type|"codice"  
resource|RP ID (identificatore) dell'API Web nel gruppo di applicazioni  
client_id|Id dell'applicazione nativa nel gruppo di applicazioni client  
redirect_uri|URI di reindirizzamento dell'applicazione nativa in gruppo di applicazioni  
  
**Risposta richiesta di autorizzazione:**  
Se l'utente non è firmato precedenza, l'utente verrà richieste le credenziali.    
ADFS risponde restituendo un codice di autorizzazione come parametro "code" nel componente di query del redirect_uri.  Ad esempio:  Protocollo HTTP/1.1 302 trovato percorso:  **<http://redirect_uri:80/?code=&lt;code&gt>;.**  
  
2. Il client nativo invia quindi il codice, con i seguenti parametri, all'endpoint del token di ADFS:  
  
**Richiesta di token:**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Parametro|Value  
---------|---------  
grant_type|"authorization_code" 
code|codice di autorizzazione da 1  
resource|RP ID (identificatore) dell'API Web nel gruppo di applicazioni  
client_id|Id dell'applicazione nativa nel gruppo di applicazioni client  
redirect_uri|URI di reindirizzamento dell'applicazione nativa in gruppo di applicazioni  
  
**Risposta alla richiesta di token:**  
ADFS risponde con HTTP 200 con l'oggetto access_token, token di aggiornamento e id_token nel corpo.  
  
3. Quindi l'applicazione nativa invia la parte access_token della risposta precedente come intestazione di autorizzazione nella richiesta HTTP all'API web.  
  
### <a name="single-sign-on-behavior"></a>Single sign-on comportamento  
Client successive, le richieste all'interno di 1 ora (per impostazione predefinita) l'oggetto access_token sarà comunque valido nella cache e una nuova richiesta non comporta alcun traffico ad ADFS.  L'oggetto access_token verranno automaticamente recuperati dalla cache da ADAL.  
  
Dopo che il token di accesso scade, ADAL invia automaticamente una richiesta basata su token di aggiornamento all'endpoint del token di ADFS (ignorando la richiesta di autorizzazione automaticamente).  
**Richiesta di token di aggiornamento:**  
POST https://fs.contoso.com/adfs/oauth2/token
   

Parametro|Value|
---------|---------
grant_type|"token di aggiornamento"|
resource|RP ID (identificatore) dell'API Web nel gruppo di applicazioni|
client_id|Id dell'applicazione nativa nel gruppo di applicazioni client
token di aggiornamento|il token di aggiornamento emesso da ADFS in risposta alla richiesta di token iniziale

  
  
**Risposta richiesta di token di aggiornamento:**  
Se il token di aggiornamento all'interno di < SSO_period >, la richiesta comporterà un nuovo token di accesso. L'utente non verrà richieste le credenziali.  Per ulteriori informazioni sul SSO, vedere impostazioni [AD ADFS Single Sign On Settings](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
Se il token di aggiornamento è scaduto, la richiesta determina un HTTP 401 con errore "invalid_grant" e "error_description" "MSIS9615: Il token di aggiornamento ricevuto nel parametro di token di aggiornamento è scaduto". In questo caso, ADAL invia automaticamente una nuova richiesta di autorizzazione che somiglia proprio ad #1.    
  
### <a name="web-browser-to-web-app"></a>Web Browser per App Web   
In questo scenario, un utente con un browser deve accedere alle risorse ospitate da un'applicazione web.    
Esistono due scenari in cui tale scopo.  
  
#### <a name="oauth-confidential-client"></a>OAuth client riservato  
Questo scenario è simile a quanto sopra in che è una richiesta di autorizzazione, seguita da un codice per lo scambio di token.  Il sito web (modellato come un'applicazione Server in AD FS) avvia la richiesta di autorizzazione tramite il browser e scambia il codice per il token (una connessione diretta a AD FS)  
  
![Descrizione del flusso del protocollo](media/ADFS_DEV_4.png)  
  
1. Endpoint di autorizzare l'utente avvia App Web un'autorizzazione richiesta tramite il browser, che invia una richiesta HTTP GET per ADFS  
   **Richiesta di autorizzazione**:  
   GET <https://fs.contoso.com/adfs/oauth2/authorize?>  
  
Parametro|Value  
---------|---------  
response_type|"codice"  
resource|RP ID (identificatore) dell'API Web nel gruppo di applicazioni  
client_id|Id client dell'applicazione nativa nel gruppo di applicazioni  
redirect_uri|URI di reindirizzamento dell'app web, applicazione server, nel gruppo di applicazioni  
  
Risposta richiesta di autorizzazione:  
Se l'utente non è firmato precedenza, l'utente verrà richieste le credenziali.  
ADFS risponde restituendo un codice di autorizzazione come parametro "code" nel componente di query del redirect_uri, ad esempio: Protocollo HTTP/1.1 302 trovato percorso: <https://webapp.contoso.com/?code=&lt;code&gt>;.  
  
2. In seguito a 302 precedente, il browser avvia un'operazione HTTP GET all'app web, ad esempio: GET <http://redirect_uri:80/?code=&lt;code&gt>;.   
  
3. A questo punto l'app web, dopo aver ricevuto il codice inizia una richiesta all'endpoint token di ADFS, l'invio di seguito  
   **Richiesta di token:**  
   POST https://fs.contoso.com/adfs/oauth2/token  
  
Parametro|Value  
---------|---------  
grant_type|"authorization_code"  
code|codice di autorizzazione da 2 precedente  
resource|RP ID (identificatore) dell'API Web nel gruppo di applicazioni  
client_id|Id client dell'app web, applicazione server, nel gruppo di applicazioni  
redirect_uri|URI di reindirizzamento dell'app web, applicazione server, nel gruppo di applicazioni  
client_secret|Segreto dell'app web, applicazione server, nel gruppo di applicazioni. **Nota: Le credenziali del client non sono necessario essere un client_secret.  ADFS supporta la possibilità di usare anche i certificati o l'autenticazione integrata di Windows.**  
  
**Risposta alla richiesta di token:**  
ADFS risponde con HTTP 200 con l'oggetto access_token, token di aggiornamento e id_token nel corpo.  
attestazioni  
4. L'applicazione web, quindi uno utilizza la parte access_token della risposta precedente (nel caso in cui la stessa app web ospita la risorsa), o in caso contrario Invia come intestazione di autorizzazione nella richiesta HTTP all'API web.  
  
#### <a name="single-sign-on-behavior"></a>Single sign-on comportamento  
Mentre il token di accesso sarà comunque valido per 1 ora (per impostazione predefinita) nella cache del client, si potrebbe pensare che la seconda richiesta funzionerà come nello scenario di native client precedente, che una nuova richiesta non comporta alcun traffico ad ADFS come token di accesso verrà automaticamente recuperato dalla cache da ADAL.  Tuttavia, è possibile che l'app web può inviare richieste di token, il primo tramite collegamento URL distinto, come il nostro esempio e l'autorizzazione distinte.  
  
In questo caso, è il cookie SSO di ADFS browser che consente di ADFS emettere un nuovo codice di autorizzazione senza chiedere conferma all'utente le credenziali. Chiama quindi l'applicazione web di ADFS per il nuovo codice di autorizzazione per un nuovo token di accesso di exchange.  L'utente non verrà richieste le credenziali.  
  
In caso contrario, se l'applicazione web è smart sufficiente sapere che se l'utente è già autenticato, è possibile ignorare la richiesta di autorizzazione e il valore:  
* il token di accesso memorizzato nella cache, se non è scaduto, recuperare e utilizzare, o   
* una richiesta in base al tipo di token richiesta può essere inviata all'endpoint del token di ADFS, come descritto di seguito  
  
**Richiesta di token di aggiornamento:**  
POST https://fs.contoso.com/adfs/oauth2/token
   
Parametro|Value  
---------|---------  
grant_type|"token di aggiornamento"  
resource|RP ID (identificatore) dell'API Web nel gruppo di applicazioni  
client_id|Id client dell'app web, applicazione server, nel gruppo di applicazioni  
token di aggiornamento|Token emesso da ADFS in risposta alla richiesta di token iniziale di aggiornamento  
client_secret|Segreto dell'app web, applicazione server, nel gruppo di applicazioni  
  
**Risposta richiesta di token di aggiornamento:**  
Se il token di aggiornamento all'interno di < SSO_period >, la richiesta comporterà un nuovo token di accesso. L'utente non verrà richieste le credenziali. Per ulteriori informazioni sul SSO, vedere impostazioni [AD ADFS Single Sign On Settings](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
Se il token di aggiornamento è scaduto, la richiesta determina un HTTP 401 con errore "invalid_grant" e "error_description" "MSIS9615: Il token di aggiornamento ricevuto nel parametro di token di aggiornamento è scaduto". In questo caso, ADAL invia automaticamente una nuova richiesta di autorizzazione che somiglia proprio ad #1.    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID Connect: Flusso ibrido  
Questo scenario è simile a quanto sopra in cui vi viene avviata una richiesta di autorizzazione per l'applicazione web tramite il reindirizzamento del browser e un codice per lo scambio di token da app web per ADFS.  La differenza in questo scenario è che ADFS rilascia un id_token come parte della risposta alla richiesta di autorizzazione iniziale.  
  
![Descrizione del flusso del protocollo](media/ADFS_DEV_5.png)  
  
1.  Endpoint di autorizzare l'utente avvia App Web un'autorizzazione richiesta tramite il browser, che invia una richiesta HTTP GET per ADFS  
  
**Richiesta di autorizzazione:**  
GET <https://fs.contoso.com/adfs/oauth2/authorize?>  
  
Parametro|Value  
---------|---------  
response_type|"codice + id_token"  
response_mode|"form_post"  
resource|RP ID (identificatore) dell'API Web nel gruppo di applicazioni  
client_id|Id client dell'app web, applicazione server, nel gruppo di applicazioni  
redirect_uri|URI di reindirizzamento dell'app web, applicazione server, nel gruppo di applicazioni  
  
**Risposta richiesta di autorizzazione:**  
Se l'utente non è firmato precedenza, l'utente verrà richieste le credenziali.  
AD FS risponde con un HTTP 200 e un form contenente il seguente come elementi nascosti:  
* codice: il codice di autorizzazione  
* id_token: un token JWT contenente le attestazioni che descrivono l'autenticazione degli utenti  
* Il modulo viene inviato automaticamente a redirect_uri dell'applicazione web, inviare il codice e id_token all'app web.  
  
3. A questo punto l'app web, dopo aver ricevuto il codice inizia una richiesta all'endpoint token di ADFS, l'invio di seguito  
  
**Richiesta di token:**  
POST https://fs.contoso.com/adfs/oauth2/token
  
  
  
Parametro|Value  
---------|---------  
grant_type|"authorization_code"  
code|codice di autorizzazione precedente  
resource|RP ID (identificatore) dell'API Web nel gruppo di applicazioni  
client_id|Id client dell'app web, applicazione server, nel gruppo di applicazioni  
redirect_uri|URI di reindirizzamento dell'app web, applicazione server, nel gruppo di applicazioni  
client_secret|Segreto dell'app web, applicazione server, nel gruppo di applicazioni  
  
**Risposta alla richiesta di token:**  
ADFS risponde con HTTP 200 con l'oggetto access_token, token di aggiornamento e id_token nel corpo.  
  
4. L'applicazione web, quindi uno utilizza la parte access_token della risposta precedente (nel caso in cui la stessa app web ospita la risorsa), o in caso contrario Invia come intestazione di autorizzazione nella richiesta HTTP all'API web.  
  
#### <a name="single-sign-on-behavior"></a>Single Sign-on comportamento  
Il single sign-on comportamento è identico a quello precedente il flusso di client riservato Oauth 2.0.  
  
### <a name="on-behalf-of"></a>Per conto di  
In questo scenario, un'applicazione web utilizza il token di accesso originale da un utente di richiedere e ottenere un altro token di accesso per un'altra API Web, che l'app web verrà quindi accedere come utente finale.  Si tratta di un flusso "per conto di".  
  
![Descrizione del flusso del protocollo](media/ADFS_DEV_6.png)  
  
I passaggi 1 e 2 funzionano come i passaggi 3 e 4 nel flusso precedente.  
Nel passaggio 3, il requisito principale è che il parametro client_id, l'ID client dell'app Web 2, deve corrispondere la relying Party ID di API Web A.  In altre parole, i destinatari del token di accesso per il nuovo token scambiati devono corrispondere l'ID client dell'entità che richiede il nuovo token.  

## <a name="related-content"></a>Contenuti correlati  
Visualizzare [sviluppare AD FS](../AD-FS-Development.md) per l'elenco completo di articoli, che forniscono istruzioni dettagliate sull'uso di flussi correlati. 
