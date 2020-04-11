---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: Flussi e scenari di applicazione di OpenID Connect/OAuth in AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0b7fef9c74ba5da1b94772cb5f6ff3d717a5359
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855974"
---
# <a name="ad-fs-openid-connectoauth-flows-and-application-scenarios"></a>Flussi e scenari di applicazione di OpenID Connect/OAuth in AD FS
Si applica ad AD FS 2016 e versioni successive


|Scenario|Procedura dettagliata per lo scenario con esempi|Flusso/concessione OAuth 2.0|Tipo di client|
|-----|-----|-----|-----|
|App a singola pagina</br> | &bull; [Esempio di utilizzo di ADAL](../development/Single-Page-Application-with-AD-FS.md)|[Implicito](#implicit-grant-flow)|Pubblico| 
|App Web che esegue l'accesso degli utenti</br> | &bull; [Esempio di utilizzo di OWIN](../development/enabling-openid-connect-with-ad-fs.md)|[Codice di autorizzazione](#authorization-code-grant-flow)|Pubblico, riservato|  
|App nativa che chiama l'API Web</br>|&bull; [Esempio di utilizzo di MSAL](../development/msal/adfs-msal-native-app-web-api.md)</br>&bull; [Esempio di utilizzo di ADAL](../development/native-client-with-ad-fs.md)|[Codice di autorizzazione](#authorization-code-grant-flow)|Pubblico|   
|App Web che chiama l'API Web</br>|&bull; [Esempio di utilizzo di MSAL](../development/msal/adfs-msal-web-app-web-api.md)</br>&bull; [Esempio di utilizzo di ADAL](../development/enabling-oauth-confidential-clients-with-ad-fs.md)|[Codice di autorizzazione](#authorization-code-grant-flow)|Riservato| 
|API Web che chiama un'altra API Web per conto  dell'utente (OBO)</br>|&bull; [Esempio di utilizzo di MSAL](../development/msal/adfs-msal-web-api-web-api.md)</br>&bull; [Esempio di utilizzo di ADAL](../development/ad-fs-on-behalf-of-authentication-in-windows-server.md)|[On-behalf-of](#on-behalf-of-flow)|L'app Web funziona come riservata| 
|App daemon che chiama l'API Web||[Credenziali del client](#client-credentials-grant-flow)|Riservato| 
|App Web che chiama l'API Web usando le credenziali dell'utente||[Credenziali password del proprietario della risorsa](#resource-owner-password-credentials-grant-flow-not-recommended)|Pubblico, riservato| 
|App senza browser che chiama l'API Web||[Codice del dispositivo](#device-code-flow)|Pubblico, riservato| 

## <a name="implicit-grant-flow"></a>Flusso di concessione implicita 
 
Per le applicazioni a pagina singola (AngularJS, Ember.js, React.js e così via), AD FS supporta il flusso di concessione implicita OAuth 2.0. Il flusso implicito è descritto nella  [specifica di OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.2). Offre come vantaggio principale la possibilità per l'app di ottenere i token da AD FS senza eseguire uno scambio di credenziali con il server back-end. Ciò consente all'app di eseguire l'accesso dell'utente, gestire la sessione e ottenere i token per altre API Web all'interno del codice JavaScript del client. Esistono alcune importanti considerazioni sulla sicurezza di cui tenere conto quando usi il flusso implicito in particolare per il  [client](https://tools.ietf.org/html/rfc6749#section-10.3).  
 
Se vuoi usare il flusso implicito e AD FS per aggiungere l'autenticazione all'app JavaScript, segui la procedura generale riportata di seguito.  
  
### <a name="protocol-diagram"></a>Diagramma del protocollo

Il diagramma seguente illustra l'intero flusso di accesso implicito e le sezioni che seguono descrivono ogni passaggio in modo più dettagliato.  

![Accesso implicito](media/adfs-scenarios-for-developers/implicit.png)

### <a name="request-id-token-and-access-token"></a>Token ID richiesta e token di accesso 
 
Per eseguire l'accesso iniziale dell'utente nell'app, puoi inviare una richiesta di autenticazione OpenID Connect e ottenere id_token e il token di accesso dall'endpoint AD FS.  
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=id_token+token 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&scope=openid 
&response_mode=fragment 
&state=12345 
```


|Parametro|Obbligatorio/facoltativo|Description| 
|-----|-----|-----|
|client_id|necessarie|ID dell'applicazione (client) assegnato da AD FS all'app.| 
|response_type|necessarie|Deve includere `id_token` per l'accesso OpenID Connect. Può includere anche response_type `token`. L'uso del token consente all'app di ricevere immediatamente un token di accesso dall'endpoint di autorizzazione senza dover effettuare una seconda richiesta all'endpoint del token.| 
|redirect_uri|necessarie|Valore redirect_uri dell'app, dove possono essere inviate e ricevute dall'app le risposte di autenticazione. Deve corrispondere esattamente a uno dei valori redirect_uri configurati in AD FS.| 
|nonce|necessarie|Valore incluso nella richiesta, generato dall'app, che verrà incluso come attestazione nel parametro id_token risultante. L'app può quindi verificare questo valore per l'attenuazione degli attacchi di riproduzione del token. Il valore è in genere una stringa casuale univoca che può essere usata per identificare l'origine della richiesta. Obbligatorio solo quando viene richiesto un valore id_token.|
|ambito|facoltative|Elenco di ambiti separato da spazi. Per OpenID Connect, deve includere l'ambito `openid`.|
|risorse|facoltative|URL dell'API Web.</br>Nota: se usi la libreria client MSAL, il parametro resource non viene inviato. Viene inviato invece resource url come parte del parametro scope: `scope = [resource url]//[scope values e.g., openid]`</br>Se non viene passato il parametro resource qui o in scope, ADFS userà come parametro resource predefinito urn:microsoft:userinfo. I criteri di risorsa userinfo, ad esempio i criteri MFA, di rilascio o di autorizzazione, non possono essere personalizzati.| 
|response_mode|facoltative| Specifica il metodo da usare per inviare il token risultante a un'app. Il valore predefinito è `fragment`.| 
|state|facoltative|Valore incluso nella richiesta che verrà restituito anche nella risposta del token. Può trattarsi di una stringa di qualsiasi contenuto desiderato. Per evitare attacchi di richiesta intersito falsa, viene in genere usato un valore univoco generato in modo casuale. Il parametro state viene anche usato per codificare le informazioni sullo stato dell'utente nell'app attivo prima dell'esecuzione della richiesta di autenticazione, ad esempio la pagina o la vista attiva.| 
|prompt|facoltative|Indica il tipo di interazione utente richiesto. Gli unici valori validi al momento sono login e none.</br>- `prompt=login` impone all'utente di immettere le credenziali nella richiesta, negando l'accesso Single Sign-On. </br>- `prompt=none` è l'opposto: garantisce che all'utente non venga visualizzata alcuna richiesta interattiva. Se la richiesta non può essere completata automaticamente tramite Single-Sign-On, AD FS restituirà un errore interaction_required.| 
|login_hint|facoltative|Può essere usato per precompilare il campo nome utente/indirizzo di posta elettronica della pagina di accesso per l'utente, se ne conosci già il nome. Le app useranno spesso questo parametro durante la riautenticazione, avendo già estratto il nome utente da un accesso precedente usando l'attestazione  `upn` da `id_token`.| 
|domain_hint|facoltative|Se incluso, questo parametro ignorerà il processo di individuazione basato su dominio a cui viene sottoposto l'utente nella pagina di accesso, garantendo un'esperienza utente leggermente più semplice.| 

A questo punto, all'utente verrà richiesto di immettere le credenziali e completare l'autenticazione. Dopo che l'utente ha eseguito l'autenticazione, l'endpoint di autorizzazione AD FS restituisce una risposta all'app in corrispondenza del valore redirect_uri indicato, usando il metodo specificato nel parametro response_mode.  
 
### <a name="successful-response"></a>Risposta di esito positivo 
 
Una risposta di esito positivo con `response_mode=fragment and response_type=id_token+token` ha un aspetto simile al seguente  
 
```
// Line breaks for legibility only 
 
GET https://localhost/myapp/# 
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZEstZnl0aEV... 
&token_type=Bearer 
&expires_in=3599 
&scope=openid  
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZstZnl0aEV1Q... 
&state=12345 
```


|Parametro|Description| 
|-----|-----|
|access_token|Parametro incluso se response_type include `token`.|
|token_type|Parametro incluso se response_type include `token`. Sarà sempre Bearer.| 
|expires_in| Parametro incluso se response_type include `token`. Indica il numero di secondi di validità del token, ai fini della memorizzazione nella cache.| 
|ambito| Indica gli ambiti per i quali avrà validità access_token.|  
|id_token|Parametro incluso se response_type include `id_token`. Token JSON Web (JWT) firmato. L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.| 
|state|Se nella richiesta è incluso un parametro state, lo stesso valore deve essere visualizzato nella risposta. L'app deve verificare che i valori del parametro state siano identici nella richiesta e nella risposta.|

### <a name="refresh-tokens"></a>Token di aggiornamento 
La concessione implicita non fornisce token di aggiornamento. Sia `id_tokens` che `access_tokens` scadono dopo un breve periodo di tempo, quindi l'app deve essere preparata ad aggiornare periodicamente questi token. Per aggiornare entrambi i tipi di token, puoi eseguire la stessa richiesta iframe nascosta precedente usando il parametro `prompt=none` per controllare il comportamento della piattaforma di identità. Se vuoi ricevere `new id_token`, usa `response_type=id_token`. 

## <a name="authorization-code-grant-flow"></a>Flusso di concessione del codice di autorizzazione 
 
La concessione del codice di autorizzazione OAuth 2.0 può essere usata nelle app Web per ottenere l'accesso a risorse protette, ad esempio le API Web. Il flusso del codice di autorizzazione OAuth 2.0 è descritto nella  [sezione 4.1 della specifica OAuth 2.0](https://tools.ietf.org/html/rfc6749). Viene usato per eseguire l'autenticazione e l'autorizzazione nella maggior parte dei tipi di app, tra cui app Web e app installate in modo nativo. Il flusso consente alle app di acquisire in modo sicuro parametri access_token che possono essere usati per accedere a risorse che considerano AD FS attendibile.  
 
### <a name="protocol-diagram"></a>Diagramma del protocollo 
 
A livello generale, il flusso di autenticazione per un'applicazione nativa ha un aspetto simile al seguente:

![Flusso di concessione del codice di autorizzazione](media/adfs-scenarios-for-developers/authorization.png)

### <a name="request-an-authorization-code"></a>Richiedere un codice di autorizzazione 
 
Il flusso del codice di autorizzazione inizia con il client che indirizza l'utente all'endpoint /authorize. In questa richiesta il client indica le autorizzazioni che deve acquisire dall'utente: 
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=code 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&response_mode=query 
&resource=https://webapi.com/ 
&scope=openid 
&state=12345 
```

|Parametro|Obbligatorio/facoltativo|Description|
|-----|-----|-----| 
|client_id|necessarie|ID dell'applicazione (client) assegnato da AD FS all'app.|  
|response_type|necessarie| Deve includere il codice per il flusso del codice di autorizzazione.| 
|redirect_uri|necessarie|Parametro `redirect_uri` dell'app, dove possono essere inviate e ricevute dall'app le risposte di autenticazione. Deve corrispondere esattamente a uno dei parametri redirect_uri registrati in AD FS per il client.|  
|risorse|facoltative|URL dell'API Web.</br>Nota: se usi la libreria client MSAL, il parametro resource non viene inviato. Viene inviato invece resource url come parte del parametro scope: `scope = [resource url]//[scope values e.g., openid]`</br>Se non viene passato il parametro resource qui o in scope, ADFS userà come parametro resource predefinito urn:microsoft:userinfo. I criteri di risorsa userinfo, ad esempio i criteri MFA, di rilascio o di autorizzazione, non possono essere personalizzati.| 
|ambito|facoltative|Elenco di ambiti separato da spazi.|
|response_mode|facoltative|Specifica il metodo da usare per inviare il token risultante a un'app. Può essere uno dei seguenti: </br>- query </br>- fragment </br>- form_post</br>`query` fornisce il codice come parametro della stringa di query nell'URI di reindirizzamento. Se stai richiedendo il codice, puoi usare query, fragment o form_post. `form_post` esegue un'operazione POST contenente il codice nell'URI di reindirizzamento.|
|state|facoltative|Valore incluso nella richiesta che verrà restituito anche nella risposta del token. Può trattarsi di una stringa di qualsiasi contenuto desiderato. Per evitare attacchi di richiesta intersito falsa, viene in genere usato un valore univoco generato in modo casuale. Il valore può anche codificare le informazioni sullo stato dell'utente nell'app attivo prima dell'esecuzione della richiesta di autenticazione, ad esempio la pagina o la vista attiva.|
|prompt|facoltative|Indica il tipo di interazione utente richiesto. Gli unici valori validi al momento sono login e none.</br>- `prompt=login` impone all'utente di immettere le credenziali nella richiesta, negando l'accesso Single Sign-On. </br>- `prompt=none` è l'opposto: garantisce che all'utente non venga visualizzata alcuna richiesta interattiva. Se la richiesta non può essere completata automaticamente tramite Single-Sign-On, AD FS restituirà un errore interaction_required.|
|login_hint|facoltative|Può essere usato per precompilare il campo nome utente/indirizzo di posta elettronica della pagina di accesso per l'utente, se ne conosci già il nome. Le app useranno spesso questo parametro durante la riautenticazione, avendo già estratto il nome utente da un accesso precedente usando l'attestazione  `upn` da `id_token`.|
|domain_hint|facoltative|Se incluso, questo parametro ignorerà il processo di individuazione basato su dominio a cui viene sottoposto l'utente nella pagina di accesso, garantendo un'esperienza utente leggermente più semplice.|
|code_challenge_method|facoltative|Metodo usato per codificare code_verifier per il parametro code_challenge. I possibili valori sono i seguenti: </br>- plain </br>- S256 </br>Se è escluso, si presuppone che il valore di code_challenge sia testo non crittografato se è incluso `code_challenge` . AD FS supporta sia plain che S256. Per altre informazioni, vedi l' [RFC PKCE](https://tools.ietf.org/html/rfc7636).|
|code_challenge|facoltative| Usato per proteggere le concessioni del codice di autorizzazione tramite PKCE (Proof Key for Code Exchange) da un client nativo. Obbligatorio se è incluso `code_challenge_method` . Per altre informazioni, vedi l' [RFC PKCE](https://tools.ietf.org/html/rfc7636).|

A questo punto, all'utente verrà richiesto di immettere le credenziali e completare l'autenticazione. Dopo che l'utente ha eseguito l'autenticazione, AD FS restituisce una risposta all'app in corrispondenza del valore di  `redirect_uri` indicato, usando il metodo specificato nel parametro `response_mode` .  
 
### <a name="successful-response"></a>Risposta di esito positivo 
 
Una risposta di esito positivo con response_mode=query è simile alla seguente: 
 
```
GET https://adfs.contoso.com/common/oauth2/nativeclient? 
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq... 
&state=12345  
```


|Parametro|Description|
|-----|-----|
|codice|`authorization_code` richiesto dall'app. L'app può usare il codice di autorizzazione per richiedere un token di accesso per la risorsa di destinazione. I parametri authorization_code hanno breve durata, in genere scadono dopo circa 10 minuti.|
|state|Se nella richiesta è incluso un parametro `state`, lo stesso valore deve essere visualizzato nella risposta. L'app deve verificare che i valori del parametro state siano identici nella richiesta e nella risposta.|

### <a name="request-an-access-token"></a>Richiedere un token di accesso 
 
Ora che hai acquisito un parametro `authorization_code` e che ti è stata concessa l'autorizzazione da parte dell'utente, puoi riscattare il codice per un  `access_token`  per la risorsa desiderata. A tale scopo, invia una richiesta POST all'endpoint /token:  
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com/ 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr... 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&grant_type=authorization_code 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for confidential clients (web apps)  
```

|Parametro|Obbligatoria/facoltativa|Description|
|-----|-----|-----| 
|client_id|necessarie|ID dell'applicazione (client) assegnato da AD FS all'app.| 
|grant_type|necessarie|Deve essere  `authorization_code`  per il flusso del codice di autorizzazione.| 
|codice|necessarie|`authorization_code` acquisito nella prima parte del flusso.| 
|redirect_uri|necessarie|Lo stesso valore `redirect_uri` usato per acquisire `authorization_code`.| 
|client_secret|obbligatorio per le app Web|Segreto dell'applicazione creato durante la registrazione dell'app in AD FS. Non è consigliabile usare il segreto dell'applicazione in un'app nativa perché i parametri client_secret non possono essere archiviati in modo affidabile nei dispositivi. Questo parametro è obbligatorio per le app Web e le API Web, che hanno la possibilità di archiviare il parametro client_secret in modo sicuro sul lato server. Il segreto client deve essere codificato nell'URL prima dell'invio. Queste app possono anche usare un'autenticazione basata su chiave firmando un token JWT e aggiungendolo come parametro client_assertion.| 
|code_verifier|facoltative|Lo stesso `code_verifier` usato per ottenere il valore authorization_code. Obbligatorio se è stato usato PKCE nella richiesta di concessione del codice di autorizzazione. Per altre informazioni, vedi l' [RFC PKCE](https://tools.ietf.org/html/rfc7636).</br>Nota: si applica ad AD FS 2019 e versioni successive| 

### <a name="successful-response"></a>Risposta di esito positivo 
 
Una risposta di token di esito positivo sarà simile alla seguente: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
} 
```


|Parametro|Description| 
|-----|-----|
|access_token|Token di accesso richiesto. L'app può usare questo token per eseguire l'autenticazione nella risorsa protetta (API Web).| 
|token_type|Indica il valore del tipo di token. L'unico tipo supportato da AD FS è Bearer.
|expires_in|Tempo di validità del token di accesso (in secondi).
|token di aggiornamento|Token di aggiornamento di OAuth 2.0. L'app può usare questo token per acquisire token di accesso aggiuntivi dopo che quello corrente è scaduto. I parametri refresh_token sono di lunga durata e possono essere usati per mantenere l'accesso alle risorse per lunghi periodi di tempo.| 
|refresh_token_expires_in|Tempo di validità del token di aggiornamento (in secondi).| 
|id_token|Token JWT (JSON Web Token). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.|

### <a name="use-the-access-token"></a>Usare il token di accesso 
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q... 
 ```

### <a name="refresh-the-access-token"></a>Aggiornare il token di accesso 
 
I parametri access_token sono di breve durata e devi aggiornarli dopo la scadenza per continuare ad accedere alle risorse. Puoi eseguire questa operazione inviando un'altra richiesta POST all'endpoint `/token` , questa volta specificando il parametro refresh_token anziché il codice. I token di aggiornamento sono validi per tutte le autorizzazioni per cui il client ha già ricevuto il token di accesso. 
 
I token di aggiornamento non hanno una durata specificata. In genere, la durata dei token di aggiornamento è relativamente lunga. In alcuni casi tuttavia i token di aggiornamento scadono, vengono revocati o non dispongono di privilegi sufficienti per l'azione desiderata. L'applicazione deve prevedere e gestire correttamente gli errori restituiti dall'endpoint di rilascio dei token.  
 
Anche se i token di aggiornamento non vengono revocati quando vengono usati per acquisire nuovi token di accesso, sei tenuto a rimuovere il token di aggiornamento precedente. La specifica OAuth 2.0 indica quanto segue: "Il server di autorizzazione PUÒ rilasciare un nuovo token di aggiornamento, nel qual caso il client DEVE rimuovere il token di aggiornamento precedente e sostituirlo con quello nuovo. Il server di autorizzazione PUÒ revocare il token di aggiornamento precedente dopo aver rilasciato un nuovo token di aggiornamento per il client". 
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq... 
&grant_type=refresh_token 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for confidential clients (web apps)  
```


|Parametro|Obbligatorio/facoltativo|Description| 
|-----|-----|-----|
|client_id|necessarie|ID dell'applicazione (client) assegnato da AD FS all'app.| 
|grant_type|necessarie|Deve essere `refresh_token` per questa parte del flusso del codice di autorizzazione.| 
|risorse|facoltative|URL dell'API Web.</br>Nota: se usi la libreria client MSAL, il parametro resource non viene inviato. Viene inviato invece resource url come parte del parametro scope: `scope = [resource url]//[scope values e.g., openid]`</br>Se non viene passato il parametro resource qui o in scope, ADFS userà come parametro resource predefinito urn:microsoft:userinfo. I criteri di risorsa userinfo, ad esempio i criteri MFA, di rilascio o di autorizzazione, non possono essere personalizzati.|
|ambito|facoltative|Elenco di ambiti separato da spazi.| 
|token di aggiornamento|necessarie|Parametro refresh_token acquisito nella seconda parte del flusso.| 
|client_secret|obbligatorio per le app Web| Segreto dell'applicazione creato nel portale di registrazione dell'app. Non è consigliabile usarlo in un'app nativa perché i parametri client_secret non possono essere archiviati in modo affidabile nei dispositivi. Questo parametro è obbligatorio per le app Web e le API Web, che hanno la possibilità di archiviare il parametro client_secret in modo sicuro sul lato server. Queste app possono anche usare un'autenticazione basata su chiave firmando un token JWT e aggiungendolo come parametro client_assertion.|

### <a name="successful-response"></a>Risposta di esito positivo 
Una risposta di token di esito positivo sarà simile alla seguente: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
}  
```
|Parametro|Description| 
|-----|-----|
|access_token|Token di accesso richiesto. L'app può usare questo token per eseguire l'autenticazione nella risorsa protetta, ad esempio un'API Web.| 
|token_type|Indica il valore del tipo di token. L'unico tipo supportato da AD FS è Bearer|
|expires_in|Tempo di validità del token di accesso (in secondi).|
|ambito|Ambiti per i quali è valido access_token.| 
|token di aggiornamento|Token di aggiornamento di OAuth 2.0. L'app può usare questo token per acquisire token di accesso aggiuntivi dopo che quello corrente è scaduto. I parametri refresh_token sono di lunga durata e possono essere usati per mantenere l'accesso alle risorse per lunghi periodi di tempo.| 
|refresh_token_expires_in|Tempo di validità del token di aggiornamento (in secondi).| 
|id_token|Token JWT (JSON Web Token). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.|

## <a name="on-behalf-of-flow"></a>Flusso On-Behalf-Of 
 
Il flusso On-Behalf-Of (OBO) di OAuth 2.0 rappresenta il caso d'uso in cui un'applicazione richiama un'API servizio/Web, che a sua volta deve chiamare un'altra API servizio/Web. L'idea consiste nel propagare l'identità e le autorizzazioni dell'utente delegato tramite la catena di richieste. Per effettuare richieste autenticate al servizio downstream, il servizio di livello intermedio deve proteggere un token di accesso da AD FS per conto dell'utente.  
 
### <a name="protocol-diagram"></a>Diagramma del protocollo 
Supponi che l'utente sia stato autenticato in un'applicazione tramite il flusso di concessione del codice di autorizzazione OAuth 2.0 descritto in precedenza. A questo punto, l'applicazione dispone di un token di accesso per l'API A (token A) con le attestazioni dell'utente e il consenso per accedere all'API Web di livello intermedio (API A). Verifica che il client richieda l'ambito user_impersonation nel token. A questo punto, l'API A deve effettuare una richiesta autenticata all'API Web downstream (API B). 

I passaggi che seguono costituiscono il flusso OBO e vengono spiegati con l'aiuto del diagramma seguente. 

![Flusso On-Behalf-Of](media/adfs-scenarios-for-developers/obo.png)

  1. L'applicazione client effettua una richiesta all'API A con il token A.  
  Nota: durante la configurazione del flusso OBO in AD FS, verifica che sia selezionato l'ambito `user_impersonation` e che il client richieda l'ambito `user_impersonation` nella richiesta. 
  2. L'API A esegue l'autenticazione nell'endpoint di rilascio dei token AD FS e richiede un token per accedere all'API B. Nota: durante la configurazione di questo flusso in AD FS, verifica che l'API A venga registrata anche come applicazione server con ClientID con lo stesso valore dell'ID risorsa dell'API A.
  3. L'endpoint di rilascio dei token AD FS convalida le credenziali dell'API A con il token A e rilascia il token di accesso per l'API B (token B). 
  4. Il token B è impostato nell'intestazione dell'autorizzazione della richiesta sull'API B. 
  5. I dati della risorsa protetta vengono restituiti dall'API B. 

### <a name="service-to-service-access-token-request"></a>Richiesta del token di accesso da servizio a servizio 
 
Per richiedere un token di accesso, effettua una richiesta HTTP POST all'endpoint del token AD FS con i parametri seguenti.  


### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primo caso: richiesta di un token di accesso con un segreto condiviso 
 
Quando viene usato un segreto condiviso, una richiesta di token di accesso da servizio a servizio contiene i parametri seguenti: 


|Parametro|Obbligatorio/facoltativo|Description|
|-----|-----|-----| 
|grant_type|necessarie|Tipo della richiesta di token. Per una richiesta che usa un token JWT, il valore deve essere urn:ietf:params:oauth:grant-type:jwt-bearer.|  
|client_id|necessarie|ID client configurato per la registrazione della prima API Web come app server (app di livello intermedio). Deve corrispondere all'ID risorsa usato nella prima parte, ovvero all'URL della prima API Web.| 
|client_secret|necessarie|Segreto dell'applicazione creato durante la registrazione dell'app server in AD FS.| 
|assertion|necessarie|Valore del token usato nella richiesta.|  
|requested_token_use|necessarie|Specifica la modalità di elaborazione della richiesta. Nel flusso OBO il valore deve essere impostato su on_behalf_of| 
|risorse|necessarie|ID risorsa fornito durante la registrazione della prima API Web come app server (app di livello intermedio). L'ID risorsa deve essere l'URL della seconda API Web che verrà chiamato dall'app di livello intermedio per conto del client.|
|ambito|facoltative|Elenco di ambiti separato da spazi per la richiesta di token.| 

#### <a name="example"></a>Esempio 
 
La richiesta `HTTP POST` seguente richiede un token di accesso e un token di aggiornamento 
 
```
//line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer 
&client_id=https://webapi.com/ 
&client_secret=BYyVnAt56JpLwUcyo47XODd 
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIm… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope=openid    
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Secondo caso: richiesta di un token di accesso con un certificato 
 
Una richiesta di token di accesso da servizio a servizio con un certificato contiene i parametri seguenti: 


|Parametro|Obbligatorio/facoltativo|Description|
|-----|-----|-----| 
|grant_type|necessarie|Tipo della richiesta di token. Per una richiesta che usa un token JWT, il valore deve essere urn:ietf:params:oauth:grant-type:jwt-bearer. |
|client_id|necessarie|ID client configurato per la registrazione della prima API Web come app server (app di livello intermedio). Deve corrispondere all'ID risorsa usato nella prima parte, ovvero all'URL della prima API Web.|  
|client_assertion_type|necessarie|Il valore deve essere urn:ietf:params:oauth:client-assertion-type:jwt-bearer.| 
|client_assertion|necessarie|Asserzione (token Web JSON) che devi creare e firmare con il certificato registrato come credenziali per l'applicazione.|  
|assertion|necessarie|Valore del token usato nella richiesta.| 
|requested_token_use|necessarie|Specifica la modalità di elaborazione della richiesta. Nel flusso OBO il valore deve essere impostato su on_behalf_of| 
|risorse|necessarie|ID risorsa fornito durante la registrazione della prima API Web come app server (app di livello intermedio). L'ID risorsa deve essere l'URL della seconda API Web che verrà chiamato dall'app di livello intermedio per conto del client.|
|ambito|facoltative|Elenco di ambiti separato da spazi per la richiesta di token.|


Considera che i parametri sono quasi uguali a quelli illustrati per la richiesta con un segreto condiviso, con la differenza che il parametro client_secret è sostituito da due parametri: client_assertion_type e client_assertion. 

#### <a name="example"></a>Esempio 
La richiesta HTTP POST seguente richiede un token di accesso per l'API Web con un certificato.

``` 
// line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer 
&client_id= https://webapi.com/ 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNS… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope= openid 
```    

### <a name="service-to-service-access-token-response"></a>Risposta del token di accesso da servizio a servizio 
 
Una risposta di esito positivo è una risposta OAuth 2.0 JSON con i parametri seguenti. 


|Parametro|Description|
|-----|-----| 
|token_type|Indica il valore del tipo di token. L'unico tipo supportato da AD FS è Bearer. | 
|ambito|Ambito di accesso concesso nel token.| 
|expires_in|Intervallo di tempo, in secondi, durante il quale il token di accesso è valido.| 
|access_token|Token di accesso richiesto. Il servizio chiamante può usare questo token per l'autenticazione nel servizio ricevente.| 
|id_token|Token JWT (JSON Web Token). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.| 
|token di aggiornamento|Token di aggiornamento per il token di accesso richiesto. Il servizio chiamante può usare questo token per richiedere un altro token di accesso dopo la scadenza del token di accesso corrente.|
|Refresh_token_expires_in|Intervallo di tempo, in secondi, durante il quale il token di aggiornamento è valido. 

### <a name="success-response-example"></a>Esempio di risposta di esito positivo 
 
Nell'esempio seguente viene illustrata una risposta di esito positivo a una richiesta di un token di accesso per l'API Web. 

``` 
{ 
  "token_type": "Bearer", 
  "scope": openid, 
  "expires_in": 3269, 
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1t" 
  "id_token": "aWRfdG9rZW49ZXlKMGVYQWlPaUpLVjFRaUxDSmhiR2NpT2lKU1V6STFOa" 
  "refresh_token": "OAQABAAAAAABnfiG…" 
  "refresh_token_expires_in": 28800, 
} 
```  
 
 
Usa il token di accesso per accedere alla risorsa protetta. Ora il servizio di livello intermedio può usare il token acquisito in precedenza per eseguire richieste autenticate all'API Web downstream, impostando il token nell'intestazione dell'autorizzazione.  

#### <a name="example"></a>Esempio 
``` 
GET /v1.0/me HTTP/1.1 
Host: https://secondwebapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQ… 
``` 

## <a name="client-credentials-grant-flow"></a>Flusso di concessione di credenziali client 
 
Per accedere alle risorse ospitate sul Web tramite l'identità di un'applicazione, puoi usare la concessione di credenziali client OAuth 2.0 specificata in [RFC 6749](https://tools.ietf.org/html/rfc6749#section-4.4). Questo tipo di concessione viene comunemente usato per le interazioni da server a server che devono essere eseguite in background, senza interazione immediata con un utente. Questi tipi di applicazioni sono spesso denominati daemon o account di servizio. 

Il flusso di concessione di credenziali client OAuth 2.0 permette a un servizio Web (client riservato) di usare le proprie credenziali per l'autenticazione in caso di chiamata a un altro servizio Web, invece di rappresentare un utente. In questo scenario il client è in genere un servizio Web di livello intermedio, un servizio daemon o un sito Web. Per un livello di sicurezza più elevato, AD FS consente inoltre al servizio chiamante di usare come credenziali un certificato, anziché un segreto condiviso. 

### <a name="protocol-diagram"></a>Diagramma del protocollo 

Il diagramma seguente illustra il flusso di concessione delle credenziali client. 

![Credenziali client](media/adfs-scenarios-for-developers/credentials.png)

### <a name="request-a-token"></a>Richiedere un token 
 
Per ottenere un token usando la concessione delle credenziali client, invia una richiesta `POST` all'endpoint /token di AD FS:  
 
### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primo caso: richiesta di un token di accesso con un segreto condiviso 
 
```
POST /adfs/oauth2/token HTTP/1.1            
//Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=535fb089-9ff3-47b6-9bfb-4f1264799865 
&client_secret=qWgdYAmab0YSkuL1qKv5bPX 
&grant_type=client_credentials 
```

|Parametro|Obbligatorio/facoltativo|Description|
|-----|-----|-----| 
|client_id|necessarie|ID dell'applicazione (client) assegnato da AD FS all'app.| 
|ambito|facoltative|Elenco di ambiti separato da spazi per cui l'utente deve fornire il consenso.| 
|client_secret|necessarie|Segreto client generato per l'app nel portale di registrazione delle app. Il segreto client deve essere codificato nell'URL prima dell'invio.| 
|grant_type|necessarie|Deve essere impostato su  `client_credentials`.|

### <a name="second-case-access-token-request-with-a-certificate"></a>Secondo caso: richiesta di un token di accesso con un certificato 

``` 
POST /adfs/oauth2/token HTTP/1.1                
 
// Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg 
&grant_type=client_credentials  
```

|Parametro|Obbligatorio/facoltativo|Description| 
|-----|-----|-----|
|client_assertion_type|necessarie|Il valore deve essere impostato su urn:ietf:params:oauth:client-assertion-type:jwt-bearer.| 
|client_assertion|necessarie|Asserzione (token Web JSON) che devi creare e firmare con il certificato registrato come credenziali per l'applicazione.|  
|grant_type|necessarie|Deve essere impostato su  `client_credentials`.|
|client_id|facoltative|ID dell'applicazione (client) assegnato da AD FS all'app. Fa parte di client_assertion, quindi non è necessario passarlo qui.| 
|ambito|facoltative|Elenco di ambiti separato da spazi per cui l'utente deve fornire il consenso.| 

### <a name="use-a-token"></a>Usare un token 
 
Ora che hai acquisito un token, usalo per effettuare richieste alla risorsa. Alla scadenza del token, ripeti la richiesta all'endpoint /token per acquisire un nuovo token di accesso.  
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...  
```

## <a name="resource-owner-password-credentials-grant-flow-not-recommended"></a>Flusso di concessione delle credenziali password del proprietario della risorsa (operazione non consigliata) 
 
La concessione delle credenziali password del proprietario della risorsa consente a un'applicazione di eseguire l'accesso dell'utente gestendo direttamente la password. Questo flusso richiede un elevato livello di attendibilità e di esposizione degli utenti ed è consigliabile usarlo solo quando non è possibile usare altri flussi, più sicuri.  
 
### <a name="protocol-diagram"></a>Diagramma del protocollo 
 
Il diagramma seguente mostra il flusso di concessione delle credenziali password del proprietario della risorsa.

![Flusso di concessione delle credenziali password del proprietario della risorsa](media/adfs-scenarios-for-developers/resource.png)

### <a name="authorization-request"></a>Richiesta di autorizzazione 
Il flusso di concessione delle credenziali password del proprietario della risorsa è una singola richiesta. Invia le credenziali dell'utente e di identificazione del client all'IDP e quindi riceve i token come restituzione. Prima di eseguire questa operazione, il client deve richiedere l'indirizzo di posta elettronica (UPN) e la password dell'utente. Subito dopo una richiesta con esito positivo, il client deve rilasciare in modo sicuro le credenziali dell'utente dalla memoria. Non deve mai salvarle.  

```
// Line breaks and spaces are for legibility only. 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&scope= openid  
&username=myusername@contoso.com 
&password=SuperS3cret 
&grant_type=password 
```


|Parametro|Obbligatorio/facoltativo|Description| 
|-----|-----|-----|
|client_id|necessarie|ID client| 
|grant_type|necessarie|Questo parametro deve essere impostato su password.| 
|nomeutente|necessarie|Indirizzo di posta elettronica dell'utente| 
|password|necessarie|Password dell'utente.| 
|ambito|facoltative|Elenco di ambiti separato da spazi.|

### <a name="successful-authentication-response"></a>Risposta di autenticazione di esito positivo 
Nell'esempio seguente viene illustrata una risposta di token di esito positivo: 

```
{ 
    "token_type": "Bearer", 
    "scope": "openid", 
    "expires_in": 3599, 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIn...", 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDR..." 
}  
```


|Parametro|Description| 
|-----|-----|
|token_type|Questo parametro deve essere sempre impostato su Bearer.| 
|ambito|Se è stato restituito un token di accesso, questo parametro elenca gli ambiti per i quali è valido il token di accesso.| 
|expires_in|Numero di secondi durante i quali è valido il token di accesso incluso.| 
|access_token|Parametro rilasciato per gli ambiti richiesti.| 
|id_token|Token JWT (JSON Web Token). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.| 
|refresh_token_expires_in|Numero di secondi durante i quali è valido il token di aggiornamento incluso.| 
|token di aggiornamento|Viene rilasciato se il parametro scope originale include offline_access.|

Puoi usare il token di aggiornamento per acquisire nuovi token di accesso e di aggiornamento usando lo stesso flusso descritto nella sezione precedente relativa al flusso di concessione del codice di autenticazione.   

## <a name="device-code-flow"></a>Flusso di codice del dispositivo 
 
La concessione del codice del dispositivo consente agli utenti di accedere a dispositivi con vincoli di input, ad esempio Smart TV, dispositivi IoT o stampanti. Per abilitare questo flusso, il dispositivo richiede all'utente di visitare una pagina Web nel browser di un altro dispositivo per eseguire l'accesso. Dopo che l'utente ha eseguito l'accesso, il dispositivo è in grado di ottenere i token di accesso e i token di aggiornamento in base alle esigenze. 
 
### <a name="protocol-diagram"></a>Diagramma del protocollo 
 
L'intero flusso del codice del dispositivo ha un aspetto simile al diagramma seguente. Ciascuno di questi passaggi viene descritto più avanti in questo articolo. 
 
![Flusso di codice del dispositivo](media/adfs-scenarios-for-developers/device.png)

### <a name="device-authorization-request"></a>Richiesta di autorizzazione per il dispositivo 
Il client deve prima cercare con il server di autenticazione un dispositivo e un codice utente usato per avviare l'autenticazione. Il client raccoglie la richiesta dall'endpoint /devicecode. In questa richiesta il client deve anche includere le autorizzazioni che deve acquisire dall'utente. Dal momento dell'invio della richiesta, l'utente ha solo 15 minuti per accedere (il valore abituale per expires_in), pertanto effettua questa richiesta solo quando l'utente ha indicato di essere pronto per l'accesso. 

```
// Line breaks are for legibility only. 
 
POST https://adfs.contoso.com/adfs/oauth2/devicecode 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
scope=openid 
```


|Parametro|Condizione|Description|
|-----|-----|-----| 
|client_id|necessarie|ID dell'applicazione (client) assegnato da AD FS all'app.| 
|ambito|facoltative|Elenco di ambiti separato da spazi.|

### <a name="device-authorization-response"></a>Risposta di autorizzazione per il dispositivo 
Una risposta di esito positivo sarà un oggetto JSON contenente le informazioni necessarie per consentire all'utente di eseguire l'accesso. 


|Parametro|Description|
|-----|-----| 
|device_code|Stringa lunga usata per verificare la sessione tra il client e il server di autorizzazione. Il client usa questo parametro per richiedere al server di autorizzazione il token di accesso.| 
|user_code|Stringa breve mostrata all'utente e usata per identificare la sessione in un dispositivo secondario.| 
|verification_uri|URI a cui l'utente deve accedere con user_code per eseguire l'accesso.| 
|verification_uri_complete|URI a cui l'utente deve accedere con user_code per eseguire l'accesso. Questo parametro è precompilato con user_code in modo che l'utente non debba immettere tale valore| 
|expires_in|Numero di secondi prima della scadenza di device_code e user_code.| 
|interval|Numero di secondi di attesa del client tra le richieste di polling.| 
|message|Stringa leggibile con le istruzioni per l'utente. Può essere localizzata includendo un parametro di query nella richiesta del modulo ?mkt=xx-XX, specificando il codice delle impostazioni cultura della lingua appropriato.  

### <a name="authenticating-the-user"></a>Autenticazione dell'utente 
Dopo aver ricevuto user_code e verification_uri, il client li mostra all'utente, indicandogli di eseguire l'accesso con il browser del telefono cellulare o del PC. Inoltre, il client può usare un codice a matrice o un meccanismo simile per visualizzare il parametro verfication_uri_complete, che eseguirà il passaggio di immissione del valore user_code per l'utente. Mentre l'utente esegue l'autenticazione in verification_uri, il client deve eseguire il polling dell'endpoint /token per il token richiesto usando device_code. 

```
POST https://adfs.contoso.com /adfs/oauth2/token 
Content-Type: application/x-www-form-urlencoded 
 
grant_type: urn:ietf:params:oauth:grant-type:device_code 
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e 
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8 
```


|Parametro|necessarie|Description|
|-----|-----|-----| 
|grant_type|necessarie|Deve essere urn:ietf:params:oauth:grant-type:device_code| 
|client_id|necessarie|Deve corrispondere al valore client_id usato nella richiesta iniziale.| 
|codice|necessarie|Valore device_code restituito nella richiesta di autorizzazione per il dispositivo.|

### <a name="successful-authentication-response"></a>Risposta di autenticazione di esito positivo 
Una risposta di token di esito positivo sarà simile alla seguente:  


|Parametro|Description|
|-----|-----| 
|token_type|Sempre "Bearer.| 
|ambito|Se è stato restituito un token di accesso, questo parametro elenca gli ambiti per i quali è valido il token di accesso.| 
|expires_in|Numero di secondi durante i quali è valido il token di accesso incluso.| 
|access_token|Parametro rilasciato per gli ambiti richiesti.| 
|id_token|Viene rilasciato se il parametro scope originale include l'ambito openid.| 
|token di aggiornamento|Viene rilasciato se il parametro scope originale include offline_access.| 
|refresh_token_expires_in|Numero di secondi durante i quali è valido il token di aggiornamento incluso.| 


## <a name="related-content"></a>Contenuti correlati  
Per l'elenco completo degli articoli con le procedure, vedi [Sviluppo in AD FS](../AD-FS-Development.md), che fornisce istruzioni dettagliate sull'uso dei flussi correlati. 
