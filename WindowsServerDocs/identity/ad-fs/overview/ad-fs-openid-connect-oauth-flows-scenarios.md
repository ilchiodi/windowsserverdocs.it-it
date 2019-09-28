---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: AD FS i flussi OpenID Connect/OAuth e gli scenari di applicazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e1e0235e50945fadd09fe9dd5ffeaf6d7119e482
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385600"
---
# <a name="ad-fs-openid-connectoauth-flows-and-application-scenarios"></a>AD FS i flussi OpenID Connect/OAuth e gli scenari di applicazione
Si applica a AD FS 2016 e versioni successive


|Scenario|Scenario procedurale con esempi|Flusso/concessione OAuth 2,0|Tipo di client|
|-----|-----|-----|-----|
|App a singola pagina</br> | &bull;[Esempio di utilizzo di adal](../development/Single-Page-Application-with-AD-FS.md)|[Implicita](#implicit-grant-flow)|Public| 
|App Web che esegue l'accesso degli utenti</br> | &bull;[Esempio di utilizzo di OWIN](../development/enabling-openid-connect-with-ad-fs.md)|[Codice di autorizzazione](#authorization-code-grant-flow)|Pubblico, riservato|  
|App native chiama API Web</br>|&bull;[Esempio di utilizzo di MSAL](../development/msal/adfs-msal-native-app-web-api.md)</br>&bull;[Esempio di utilizzo di adal](../development/native-client-with-ad-fs.md)|[Codice di autorizzazione](#authorization-code-grant-flow)|Public|   
|App Web chiama API Web</br>|&bull;[Esempio di utilizzo di MSAL](../development/msal/adfs-msal-web-app-web-api.md)</br>&bull;[Esempio di utilizzo di adal](../development/enabling-oauth-confidential-clients-with-ad-fs.md)|[Codice di autorizzazione](#authorization-code-grant-flow)|Riservate| 
|API Web chiama un'altra API Web per conto di (OBO) dell'utente</br>|&bull;[Esempio di utilizzo di MSAL](../development/msal/adfs-msal-web-api-web-api.md)</br>&bull;[Esempio di utilizzo di adal](../development/ad-fs-on-behalf-of-authentication-in-windows-server.md)|[On-behalf-of](#on-behalf-of-flow)|L'app Web funziona come riservata| 
|App daemon chiama API Web||[Credenziali client](#client-credentials-grant-flow)|Riservate| 
|L'app Web chiama l'API Web usando l'utente credenziali||[Credenziali password del proprietario della risorsa](#resource-owner-password-credentials-grant-flow-not-recommended)|Pubblico, riservato| 
|API Web per chiamate app non in browser||[Codice dispositivo](#device-code-flow)|Pubblico, riservato| 

## <a name="implicit-grant-flow"></a>Flusso di concessione implicita 
 
Per le applicazioni a pagina singola (AngularJS, Brac. js, React. js e così via), AD FS supporta il flusso di concessione implicita OAuth 2,0. Il flusso implicito è descritto nella [specifica OAuth 2,0](https://tools.ietf.org/html/rfc6749#section-4.2). Il vantaggio principale è che consente all'app di ottenere i token da AD FS senza eseguire uno scambio di credenziali del server back-end. Ciò consente all'app di eseguire l'accesso dell'utente, gestire la sessione e ottenere i token per altre API Web all'interno del codice JavaScript del client. Esistono alcune importanti considerazioni sulla sicurezza da tenere in considerazione quando si usa il flusso implicito in modo specifico per il [client](https://tools.ietf.org/html/rfc6749#section-10.3).  
 
Se si vuole usare il flusso implicito e AD FS per aggiungere l'autenticazione all'app JavaScript, attenersi alla procedura generale riportata di seguito.  
  
### <a name="protocol-diagram"></a>Diagramma di protocollo

Il diagramma seguente illustra l'aspetto dell'intero flusso di accesso implicito e le sezioni che seguono descrivono ogni passaggio in modo più dettagliato.  

![Accesso implicito](media/adfs-scenarios-for-developers/implicit.png)

### <a name="request-id-token-and-access-token"></a>Token ID richiesta e token di accesso 
 
Per firmare inizialmente l'utente nell'app, è possibile inviare una richiesta di autenticazione OpenID Connect e ottenere token ID e il token di accesso dall'endpoint AD FS.  
 
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


|Parametro|Obbligatorio/facoltativo|Descrizione| 
|-----|-----|-----|
|client_id|necessarie|ID dell'applicazione (client) assegnato dall'AD FS all'app.| 
|response_type|necessarie|Deve includere `id_token` per l'accesso OpenID Connect. Può inoltre includere response_type @ no__t-0. L'uso di token consente all'app di ricevere immediatamente un token di accesso dall'endpoint di autorizzazione senza dover effettuare una seconda richiesta all'endpoint del token.| 
|redirect_uri|necessarie|Redirect_uri dell'app, in cui le risposte di autenticazione possono essere inviate e ricevute dall'app. Deve corrispondere esattamente a uno dei Reindirizzamento configurati in AD FS.| 
|nonce|necessarie|Valore incluso nella richiesta, generato dall'app, che verrà incluso nel token ID risultante come attestazione. L'app può quindi verificare questo valore per attenuare gli attacchi di riproduzione del token. Il valore è in genere una stringa casuale univoca che può essere usata per identificare l'origine della richiesta. Obbligatorio solo quando viene richiesto un token ID.|
|scope|facoltative|Elenco di ambiti separati da spazi. Per OpenID Connect, deve includere l'ambito `openid`.|
|resource|facoltative|URL dell'API Web.</br>Nota: se si usa la libreria client MSAL, il parametro della risorsa non viene inviato. L'URL della risorsa viene invece inviato come parte del parametro Scope:`scope = [resource url]//[scope values e.g., openid]`</br>Se la risorsa non viene passata qui o nell'ambito di ADFS utilizzerà una risorsa predefinita urn: Microsoft: UserInfo. non è possibile personalizzare i criteri delle risorse UserInfo, ad esempio l'autenticazione a più fattori, il rilascio o i criteri di autorizzazione.| 
|response_mode|facoltative| Specifica il metodo da usare per inviare di nuovo il token risultante all'app. Il valore predefinito è `fragment`.| 
|state|facoltative|Valore incluso nella richiesta che verrà restituito anche nella risposta del token. Può trattarsi di una stringa di qualsiasi contenuto desiderato. Per evitare attacchi di richiesta intersito falsa, viene in genere usato un valore univoco generato in modo casuale. Lo stato viene anche usato per codificare le informazioni sullo stato dell'utente nell'app prima che venga eseguita la richiesta di autenticazione, ad esempio la pagina o la vista in cui si trovava.| 
|prompt|facoltative|Indica il tipo di interazione dell'utente richiesto. Gli unici valori validi al momento sono login e None.</br>- `prompt=login` impone all'utente di immettere le credenziali per la richiesta, negando l'accesso Single Sign-on. </br>- `prompt=none` è l'opposto: garantisce che all'utente non venga visualizzata alcuna richiesta interattiva. Se la richiesta non può essere completata automaticamente tramite Single-Sign-on, AD FS restituirà un errore interaction_required.| 
|login_hint|facoltative|Può essere usato per pre-compilare il campo nome utente/indirizzo di posta elettronica della pagina di accesso per l'utente, se si conosce il proprio nome utente prima del tempo. Spesso le app utilizzeranno questo parametro durante la riautenticazione, avendo già estratto il nome utente da un accesso precedente usando l' `upn` attestazione `id_token`di.| 
|domain_hint|facoltative|Se incluso, ignorerà il processo di individuazione basato su dominio utilizzato dall'utente nella pagina di accesso, ottenendo un'esperienza utente leggermente più semplificata.| 

A questo punto, all'utente verrà richiesto di immettere le credenziali e completare l'autenticazione. Una volta che l'utente esegue l'autenticazione, l'endpoint AD FS autorizzazione restituirà una risposta all'app in corrispondenza del redirect_uri indicato, usando il metodo specificato nel parametro response_mode.  
 
### <a name="successful-response"></a>Risposta riuscita 
 
Una risposta con esito positivo con `response_mode=fragment and response_type=id_token+token` avrà un aspetto simile al seguente  
 
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


|Parametro|Descrizione| 
|-----|-----|
|access_token|Incluso se response_type include @ no__t-0.|
|token_type|Incluso se response_type include @ no__t-0. Sarà sempre Bearer.| 
|expires_in| Incluso se response_type include @ no__t-0. Indica il numero di secondi di validità del token, per scopi di memorizzazione nella cache.| 
|scope| Indica gli ambiti per i quali access_token sarà valido.|  
|token ID|Incluso se response_type include @ no__t-0. Un token JSON Web firmato (JWT). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.| 
|state|Se nella richiesta è incluso un parametro di stato, lo stesso valore dovrebbe essere visualizzato nella risposta. L'app deve verificare che i valori dello stato nella richiesta e nella risposta siano identici.|

### <a name="refresh-tokens"></a>Token di aggiornamento 
La concessione implicita non fornisce token di aggiornamento. Sia `id_tokens`che scadranno dopo un breve periodo di tempo, quindi l'app deve essere preparata ad aggiornare periodicamente questi token. `access_tokens` Per aggiornare entrambi i tipi di token, è possibile eseguire la stessa richiesta iframe nascosta precedente usando il `prompt=none` parametro per controllare il comportamento della piattaforma di identità. Se si desidera ricevere un `new id_token`, assicurarsi di utilizzare. `response_type=id_token` 

## <a name="authorization-code-grant-flow"></a>Flusso di concessione del codice di autorizzazione 
 
La concessione del codice di autorizzazione OAuth 2,0 può essere usata nelle app Web per ottenere l'accesso alle risorse protette, ad esempio le API Web. Il flusso del codice di autorizzazione OAuth 2,0 è descritto nella [sezione 4,1 della specifica oauth 2,0](https://tools.ietf.org/html/rfc6749). Viene usato per eseguire l'autenticazione e l'autorizzazione nella maggior parte dei tipi di app, tra cui app Web e app installate in modo nativo. Il flusso consente alle app di acquisire in modo sicuro i token che possono essere usate per accedere alle risorse che considerano attendibile AD FS.  
 
### <a name="protocol-diagram"></a>Diagramma di protocollo 
 
A un livello elevato, il flusso di autenticazione per un'applicazione nativa ha un aspetto simile al seguente:

![Flusso di concessione del codice di autorizzazione](media/adfs-scenarios-for-developers/authorization.png)

### <a name="request-an-authorization-code"></a>Richiedere un codice di autorizzazione 
 
Il flusso del codice di autorizzazione inizia con il client che indirizza l'utente all'endpoint/Authorize. In questa richiesta il client indica le autorizzazioni che deve acquisire dall'utente: 
 
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

|Parametro|Obbligatorio/facoltativo|Descrizione|
|-----|-----|-----| 
|client_id|necessarie|ID dell'applicazione (client) assegnato dall'AD FS all'app.|  
|response_type|necessarie| Deve includere il codice per il flusso del codice di autorizzazione.| 
|redirect_uri|necessarie|Dell' `redirect_uri` app, in cui le risposte di autenticazione possono essere inviate e ricevute dall'app. Deve corrispondere esattamente a uno dei Reindirizzamento registrati nella AD FS per il client.|  
|resource|facoltative|URL dell'API Web.</br>Nota: se si usa la libreria client MSAL, il parametro della risorsa non viene inviato. L'URL della risorsa viene invece inviato come parte del parametro Scope:`scope = [resource url]//[scope values e.g., openid]`</br>Se la risorsa non viene passata qui o nell'ambito di ADFS utilizzerà una risorsa predefinita urn: Microsoft: UserInfo. non è possibile personalizzare i criteri delle risorse UserInfo, ad esempio l'autenticazione a più fattori, il rilascio o i criteri di autorizzazione.| 
|scope|facoltative|Elenco di ambiti separati da spazi.|
|response_mode|facoltative|Specifica il metodo da usare per inviare di nuovo il token risultante all'app. Può essere uno dei seguenti: </br>-query </br>-frammento </br>- form_post</br>`query` fornisce il codice come parametro della stringa di query nell'URI di reindirizzamento. Se si sta richiedendo il codice, è possibile usare query, Fragment o form_post.  `form_post` @ no__t-1executes un POST contenente il codice per l'URI di reindirizzamento.|
|state|facoltative|Valore incluso nella richiesta che verrà restituito anche nella risposta del token. Può trattarsi di una stringa di qualsiasi contenuto desiderato. Per evitare attacchi di richiesta intersito falsa, viene in genere usato un valore univoco generato in modo casuale. Il valore può anche codificare le informazioni sullo stato dell'utente nell'app prima che venga eseguita la richiesta di autenticazione, ad esempio la pagina o la vista in cui si trovava.|
|prompt|facoltative|Indica il tipo di interazione dell'utente richiesto. Gli unici valori validi al momento sono login e None.</br>- `prompt=login` impone all'utente di immettere le credenziali per la richiesta, negando l'accesso Single Sign-on. </br>- `prompt=none` è l'opposto: garantisce che all'utente non venga visualizzata alcuna richiesta interattiva. Se la richiesta non può essere completata automaticamente tramite Single-Sign-on, AD FS restituirà un errore interaction_required.|
|login_hint|facoltative|Può essere usato per precompilare il campo nome utente/indirizzo di posta elettronica della pagina di accesso per l'utente, se si conosce in anticipo il nome utente. Spesso le app utilizzeranno questo parametro durante la riautenticazione, avendo già estratto il nome utente da un accesso precedente usando l' `upn`attestazione `id_token`di.|
|domain_hint|facoltative|Se incluso, ignorerà il processo di individuazione basato su dominio utilizzato dall'utente nella pagina di accesso, ottenendo un'esperienza utente leggermente più semplificata.|
|code_challenge_method|facoltative|Metodo utilizzato per codificare code_verifier per il parametro code_challenge. Il valore può essere uno dei seguenti: </br>-normale </br>- S256 </br>Se è escluso, si presuppone che code_challenge sia testo normale se @ no__t-0 @ no__t-1is è incluso. AD FS supporta sia Plain che S256. Per ulteriori informazioni, vedere [PKCE RFC](https://tools.ietf.org/html/rfc7636).|
|code_challenge|facoltative| Usato per proteggere il codice di autorizzazione concesso tramite la chiave di prova per lo scambio di codice (PKCE) da un client nativo. Obbligatorio se `code_challenge_method` è incluso. Per ulteriori informazioni, vedere [PKCE RFC](https://tools.ietf.org/html/rfc7636)|

A questo punto, all'utente verrà richiesto di immettere le credenziali e completare l'autenticazione. Una volta che l'utente esegue l'autenticazione, il ad FS restituirà una risposta all'app in `redirect_uri`corrispondenza dell'oggetto indicato, usando il `response_mode`metodo specificato nel parametro.  
 
### <a name="successful-response"></a>Risposta riuscita 
 
Una risposta con esito positivo utilizzando response_mode = query è simile alla seguente: 
 
```
GET https://adfs.contoso.com/common/oauth2/nativeclient? 
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq... 
&state=12345  
```


|Parametro|Descrizione|
|-----|-----|
|code|Oggetto `authorization_code` richiesto dall'app. L'app può usare il codice di autorizzazione per richiedere un token di accesso per la risorsa di destinazione. I codici hanno sono di breve durata, in genere scadono dopo circa 10 minuti.|
|state|Se nella `state` richiesta è incluso un parametro, lo stesso valore dovrebbe essere visualizzato nella risposta. L'app deve verificare che i valori dello stato nella richiesta e nella risposta siano identici.|

### <a name="request-an-access-token"></a>Richiedere un token di accesso 
 
Ora che è stato acquisito un `authorization_code` e a cui è stata concessa l'autorizzazione da parte dell'utente, è possibile `access_token`riscattare il codice per un alla risorsa desiderata. A tale scopo, inviare una richiesta POST all'endpoint/token:  
 
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

|Parametro|Obbligatorio/facoltativo|Descrizione|
|-----|-----|-----| 
|client_id|necessarie|ID dell'applicazione (client) assegnato dall'AD FS all'app.| 
|grant_type|necessarie|Deve essere `authorization_code` per il flusso del codice di autorizzazione.| 
|code|necessarie|Oggetto `authorization_code` acquisito nella prima parte del flusso.| 
|redirect_uri|necessarie|Stesso `redirect_uri` valore utilizzato per `authorization_code`acquisire.| 
|client_secret|obbligatorio per le app Web|Il segreto dell'applicazione creato durante la registrazione dell'app in AD FS. Non è consigliabile usare il segreto dell'applicazione in un'app nativa perché i segreti client non può essere archiviato in modo affidabile nei dispositivi. È necessario per le app Web e le API Web, che hanno la possibilità di archiviare il client_secret in modo sicuro sul lato server. Il segreto client deve essere codificato in URL prima dell'invio. Queste app possono anche usare un'autenticazione basata su chiave firmando un JWT e aggiungendolo come parametro client_assertion.| 
|code_verifier|facoltative|Lo stesso `code_verifier` usato per ottenere autorizzazione. Obbligatorio se PKCE è stato usato nella richiesta di concessione del codice di autorizzazione. Per ulteriori informazioni, vedere [PKCE RFC](https://tools.ietf.org/html/rfc7636).</br>Nota: si applica a AD FS 2019 e versioni successive| 

### <a name="successful-response"></a>Risposta riuscita 
 
Una risposta di token corretta avrà un aspetto simile al seguente: 
 
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


|Parametro|Descrizione| 
|-----|-----|
|access_token|Token di accesso richiesto. L'app può usare questo token per l'autenticazione alla risorsa protetta (API Web).| 
|token_type|Indica il valore del tipo di token. L'unico tipo supportato da AD FS è Bearer.
|expires_in|Tempo di validità del token di accesso (in secondi).
|token di aggiornamento|Un token di aggiornamento OAuth 2,0. L'app può usare questo token per acquisire token di accesso aggiuntivi dopo la scadenza del token di accesso corrente. Token sono di lunga durata e possono essere usati per mantenere l'accesso alle risorse per lunghi periodi di tempo.| 
|refresh_token_expires_in|Tempo di validità del token di aggiornamento (in secondi).| 
|token ID|Un token Web JSON (JWT). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.|

### <a name="use-the-access-token"></a>Usare il token di accesso 
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q... 
 ```

### <a name="refresh-the-access-token"></a>Aggiornare il token di accesso 
 
I token sono di breve durata ed è necessario aggiornarli dopo la scadenza per continuare ad accedere alle risorse. È possibile eseguire questa operazione inviando un'altra richiesta POST a @ no__t-0 @ no__t-1endpoint, questa volta specificando il refresh_token anziché il codice. I token di aggiornamento sono validi per tutte le autorizzazioni per cui il client ha già ricevuto il token di accesso. 
 
I token di aggiornamento non hanno durata specificata. In genere, la durata dei token di aggiornamento è relativamente lunga. In alcuni casi, tuttavia, i token di aggiornamento scadono, vengono revocati o non dispongono di privilegi sufficienti per l'azione desiderata. L'applicazione deve prevedere e gestire correttamente gli errori restituiti dall'endpoint di rilascio dei token.  
 
Anche se i token di aggiornamento non vengono revocati quando vengono usati per acquisire nuovi token di accesso, è prevedibile rimuovere il vecchio token di aggiornamento. La specifica OAuth 2,0 indica: "Il server di autorizzazione può emettere un nuovo token di aggiornamento, nel qual caso il client deve rimuovere il token di aggiornamento precedente e sostituirlo con il nuovo token di aggiornamento. Il server di autorizzazione può revocare il token di aggiornamento precedente dopo aver emesso un nuovo token di aggiornamento per il client ". 
 
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


|Parametro|Obbligatorio/facoltativo|Descrizione| 
|-----|-----|-----|
|client_id|necessarie|ID dell'applicazione (client) assegnato dall'AD FS all'app.| 
|grant_type|necessarie|Deve essere `refresh_token` per questa parte del flusso del codice di autorizzazione.| 
|resource|facoltative|URL dell'API Web.</br>Nota: se si usa la libreria client MSAL, il parametro della risorsa non viene inviato. L'URL della risorsa viene invece inviato come parte del parametro Scope:`scope = [resource url]//[scope values e.g., openid]`</br>Se la risorsa non viene passata qui o nell'ambito di ADFS utilizzerà una risorsa predefinita urn: Microsoft: UserInfo. non è possibile personalizzare i criteri delle risorse UserInfo, ad esempio l'autenticazione a più fattori, il rilascio o i criteri di autorizzazione.|
|scope|facoltative|Elenco di ambiti separati da spazi.| 
|token di aggiornamento|necessarie|Refresh_token acquisito nella seconda parte del flusso.| 
|client_secret|obbligatorio per le app Web| Il segreto dell'applicazione creato nel portale di registrazione delle app per l'app. Non deve essere usato in un'app nativa, perché i segreti client non può essere archiviato in modo affidabile nei dispositivi. È necessario per le app Web e le API Web, che hanno la possibilità di archiviare il client_secret in modo sicuro sul lato server. Queste app possono anche usare un'autenticazione basata su chiave firmando un JWT e aggiungendolo come parametro client_assertion.|

### <a name="successful-response"></a>Risposta riuscita 
Una risposta di token corretta avrà un aspetto simile al seguente: 
 
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
|Parametro|Descrizione| 
|-----|-----|
|access_token|Token di accesso richiesto. L'app può usare questo token per l'autenticazione alla risorsa protetta, ad esempio un'API Web.| 
|token_type|Indica il valore del tipo di token. L'unico tipo supportato da AD FS è Bearer|
|expires_in|Tempo di validità del token di accesso (in secondi).|
|scope|Ambiti per i quali access_token è valido.| 
|token di aggiornamento|Un token di aggiornamento OAuth 2,0. L'app può usare questo token per acquisire token di accesso aggiuntivi dopo la scadenza del token di accesso corrente. Token sono di lunga durata e possono essere usati per mantenere l'accesso alle risorse per lunghi periodi di tempo.| 
|refresh_token_expires_in|Tempo di validità del token di aggiornamento (in secondi).| 
|token ID|Un token Web JSON (JWT). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.|

## <a name="on-behalf-of-flow"></a>Flusso per conto di 
 
Il flusso di servizio (OBO) di OAuth 2,0 serve il caso d'uso in cui un'applicazione richiama un servizio o un'API Web, che a sua volta deve chiamare un'altra API del servizio/Web. L'idea è propagare l'identità e le autorizzazioni dell'utente delegato tramite la catena di richieste. Affinché il servizio di livello intermedio effettui richieste autenticate al servizio downstream, deve proteggere un token di accesso dal AD FS per conto dell'utente.  
 
### <a name="protocol-diagram"></a>Diagramma di protocollo 
Si supponga che l'utente sia stato autenticato in un'applicazione usando il flusso di concessione del codice di autorizzazione OAuth 2,0 descritto in precedenza. A questo punto, l'applicazione ha un token di accesso per l'API A (token A) con le attestazioni dell'utente e il consenso per accedere all'API Web di livello intermedio (API A). Verificare che il client richieda l'ambito user_impersonation nel token. A questo punto, l'API a deve effettuare una richiesta autenticata all'API Web downstream (API B). 

I passaggi che seguono costituiscono il flusso OBO e vengono spiegati con la guida del diagramma seguente. 

![Flusso per conto di](media/adfs-scenarios-for-developers/obo.png)

  1. L'applicazione client effettua una richiesta all'API A con il token A.  
  Nota: Durante la configurazione del flusso OBO in ad FS assicurarsi `user_impersonation` che l'ambito sia selezionato e `user_impersonation` che il client esegua l'ambito della richiesta nella richiesta. 
  2. L'API A esegue l'autenticazione all'endpoint di rilascio del token AD FS e richiede un token per accedere all'API B. Nota: Durante la configurazione di questo flusso in AD FS assicurarsi che l'API A venga registrata anche come applicazione server con ClientID con lo stesso valore dell'ID risorsa nell'API A. Per altri dettagli, vedere l'articolo relativo all'aggiunta di un collegamento per conto dell'esempio.  
  3. L'endpoint di rilascio dei token AD FS convalida le credenziali dell'API A con il token A e rilascia il token di accesso per l'API B (token B). 
  4. Il token B è impostato nell'intestazione dell'autorizzazione della richiesta all'API B. 
  5. I dati della risorsa protetta vengono restituiti dall'API B. 

### <a name="service-to-service-access-token-request"></a>Richiesta di token di accesso da servizio a servizio 
 
Per richiedere un token di accesso, effettuare un POST HTTP all'endpoint del token AD FS con i parametri seguenti.  


### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primo caso: Richiesta di token di accesso con un segreto condiviso 
 
Quando si usa un segreto condiviso, una richiesta di token di accesso da servizio a servizio contiene i parametri seguenti: 


|Parametro|Obbligatorio/facoltativo|Descrizione|
|-----|-----|-----| 
|grant_type|necessarie|Tipo di richiesta di token. Per una richiesta che usa un JWT, il valore deve essere urn: IETF: params: OAuth: Grant-Type: JWT-Bearer.|  
|client_id|necessarie|ID client configurato quando si registra la prima API Web come app Server (app di livello intermedio). Deve corrispondere all'ID risorsa usato nella prima parte, ovvero l'URL della prima API Web.| 
|client_secret|necessarie|Il segreto dell'applicazione creato durante la registrazione dell'app Sever in AD FS.| 
|asserzione|necessarie|Valore del token utilizzato nella richiesta.|  
|requested_token_use|necessarie|Specifica la modalità di elaborazione della richiesta. Nel flusso di OBO, il valore deve essere impostato su on_behalf_of| 
|resource|necessarie|ID risorsa fornito durante la registrazione della prima API Web come app Server (app di livello intermedio). L'ID risorsa deve essere l'URL della seconda app di livello intermedio dell'API Web che chiamerà per conto del client.|
|scope|facoltative|Elenco di ambiti separati da spazi per la richiesta di token.| 

#### <a name="example"></a>Esempio 
 
L'esempio `HTTP POST` seguente richiede un token di accesso e un token di aggiornamento 
 
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

### <a name="second-case-access-token-request-with-a-certificate"></a>Secondo caso: Richiesta di token di accesso con un certificato 
 
Una richiesta di token di accesso da servizio a servizio con un certificato contiene i parametri seguenti: 


|Parametro|obbligatorio/facoltativo|Descrizione|
|-----|-----|-----| 
|grant_type|necessarie|Tipo di richiesta di token. Per una richiesta che usa un JWT, il valore deve essere urn: IETF: params: OAuth: Grant-Type: JWT-Bearer. |
|client_id|necessarie|ID client configurato quando si registra la prima API Web come app Server (app di livello intermedio). Deve corrispondere all'ID risorsa usato nella prima parte, ovvero l'URL della prima API Web.|  
|client_assertion_type|necessarie|Il valore deve essere urn: IETF: params: OAuth: client-Assertion-Type: JWT-Bearer.| 
|client_assertion|necessarie|Un'asserzione (un token JSON Web) che è necessario creare e firmare con il certificato registrato come credenziale per l'applicazione.|  
|asserzione|necessarie|Valore del token utilizzato nella richiesta.| 
|requested_token_use|necessarie|Specifica la modalità di elaborazione della richiesta. Nel flusso di OBO, il valore deve essere impostato su on_behalf_of| 
|resource|necessarie|ID risorsa fornito durante la registrazione della prima API Web come app Server (app di livello intermedio). L'ID risorsa deve essere l'URL della seconda app di livello intermedio dell'API Web che chiamerà per conto del client.|
|scope|facoltative|Elenco di ambiti separati da spazi per la richiesta di token.|


Si noti che i parametri sono quasi uguali a quelli del caso della richiesta da parte di un segreto condiviso, ad eccezione del fatto che il parametro client_secret viene sostituito da due parametri: client_assertion_type e client_assertion. 

#### <a name="example"></a>Esempio 
Il POST HTTP seguente richiede un token di accesso per l'API Web con un certificato.

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
 
Una risposta con esito positivo è una risposta OAuth 2,0 JSON con i parametri seguenti. 


|Parametro|Descrizione|
|-----|-----| 
|token_type|Indica il valore del tipo di token. L'unico tipo supportato da AD FS è Bearer. | 
|scope|L'ambito di accesso concesso nel token.| 
|expires_in|Intervallo di tempo, in secondi, in cui il token di accesso è valido.| 
|access_token|Token di accesso richiesto. Il servizio chiamante può usare questo token per l'autenticazione al servizio ricevente.| 
|token ID|Un token Web JSON (JWT). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.| 
|token di aggiornamento|Token di aggiornamento per il token di accesso richiesto. Il servizio chiamante può usare questo token per richiedere un altro token di accesso dopo la scadenza del token di accesso corrente.|
|Refresh_token_expires_in|Periodo di tempo, in secondi, in cui il token di aggiornamento è valido. 

### <a name="success-response-example"></a>Esempio di risposta riuscita 
 
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
 
 
Usare il token di accesso per accedere alla risorsa protetta adesso il servizio di livello intermedio può usare il token acquisito in precedenza per eseguire richieste autenticate all'API Web downstream, impostando il token nell'intestazione dell'autorizzazione.  

#### <a name="example"></a>Esempio 
``` 
GET /v1.0/me HTTP/1.1 
Host: https://secondwebapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQ… 
``` 

## <a name="client-credentials-grant-flow"></a>Flusso di concessione delle credenziali client 
 
Per accedere alle risorse ospitate sul Web tramite l'identità di un'applicazione, è possibile utilizzare la concessione di credenziali client OAuth 2,0 specificata in [RFC 6749](https://tools.ietf.org/html/rfc6749#section-4.4). Questo tipo di concessione viene comunemente usato per le interazioni da server a server che devono essere eseguite in background, senza interazione immediata con un utente. Questi tipi di applicazioni sono spesso denominati daemon o account di servizio. 

Il flusso di concessione delle credenziali client OAuth 2,0 consente a un servizio Web (client riservato) di usare le proprie credenziali, anziché rappresentare un utente, per eseguire l'autenticazione quando si chiama un altro servizio Web. In questo scenario il client è in genere un servizio Web di livello intermedio, un servizio daemon o un sito Web. Per un livello di sicurezza più elevato, il AD FS consente inoltre al servizio chiamante di utilizzare un certificato, anziché un segreto condiviso, come credenziale. 

### <a name="protocol-diagram"></a>Diagramma di protocollo 

Il diagramma seguente illustra il flusso di concessione delle credenziali client. 

![Credenziali client](media/adfs-scenarios-for-developers/credentials.png)

### <a name="request-a-token"></a>Richiedere un token 
 
Per ottenere un token usando la concessione delle credenziali client, inviare una `POST` richiesta all'endpoint ad FS/token:  
 
### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primo caso: Richiesta di token di accesso con un segreto condiviso 
 
```
POST /adfs/oauth2/token HTTP/1.1            
//Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=535fb089-9ff3-47b6-9bfb-4f1264799865 
&client_secret=qWgdYAmab0YSkuL1qKv5bPX 
&grant_type=client_credentials 
```

|Parametro|Obbligatorio/facoltativo|Descrizione|
|-----|-----|-----| 
|client_id|necessarie|ID dell'applicazione (client) assegnato dall'AD FS all'app.| 
|scope|facoltative|Elenco di ambiti separati da spazi a cui si desidera che l'utente acconsente.| 
|client_secret|necessarie|Il segreto client generato per l'app nel portale di registrazione delle app. Il segreto client deve essere codificato in URL prima dell'invio.| 
|grant_type|necessarie|Deve essere impostato su `client_credentials`.|

### <a name="second-case-access-token-request-with-a-certificate"></a>Secondo caso: Richiesta di token di accesso con un certificato 

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

|Parametro|Obbligatorio/facoltativo|Descrizione| 
|-----|-----|-----|
|client_assertion_type|necessarie|Il valore deve essere impostato su urn: IETF: params: OAuth: client-Assertion-Type: JWT-Bearer.| 
|client_assertion|necessarie|Un'asserzione (un token JSON Web) che è necessario creare e firmare con il certificato registrato come credenziale per l'applicazione.|  
|grant_type|necessarie|Deve essere impostato su `client_credentials`.|
|client_id|facoltative|ID dell'applicazione (client) assegnato dall'AD FS all'app. Questo fa parte di client_assertion, quindi non è necessario passarlo qui.| 
|scope|facoltative|Elenco di ambiti separati da spazi a cui si desidera che l'utente acconsente.| 

### <a name="use-a-token"></a>Usare un token 
 
Ora che è stato acquisito un token, usare il token per effettuare richieste alla risorsa. Alla scadenza del token, ripetere la richiesta all'endpoint/token per acquisire un token di accesso aggiornato.  
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...  
```

## <a name="resource-owner-password-credentials-grant-flow-not-recommended"></a>Flusso di concessione delle credenziali della password del proprietario della risorsa (scelta non consigliata) 
 
La concessione delle credenziali password del proprietario della risorsa (ROPC) consente a un'applicazione di accedere all'utente gestendo direttamente la password. Il flusso ROPC richiede un elevato livello di attendibilità e di esposizione degli utenti ed è consigliabile usare questo flusso solo quando non è possibile usare altri flussi, più sicuri.  
 
### <a name="protocol-diagram"></a>Diagramma di protocollo 
 
Il diagramma seguente mostra il flusso ROPC.

![Flusso ROPC](media/adfs-scenarios-for-developers/resource.png)

### <a name="authorization-request"></a>Richiesta di autorizzazione 
Il flusso ROPC è una singola richiesta, che invia le credenziali dell'utente e dell'identificazione del client al provider di identità e quindi riceve i token in fase di restituzione. Prima di eseguire questa operazione, il client deve richiedere l'indirizzo di posta elettronica (UPN) e la password dell'utente. Subito dopo una richiesta con esito positivo, il client deve rilasciare in modo sicuro le credenziali dell'utente dalla memoria. Non deve mai salvarli.  

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


|Parametro|Obbligatorio/facoltativo|Descrizione| 
|-----|-----|-----|
|client_id|necessarie|ID client| 
|grant_type|necessarie|Deve essere impostato su password.| 
|userName|necessarie|Indirizzo di posta elettronica dell'utente| 
|password|necessarie|Password dell'utente.| 
|scope|facoltative|Elenco di ambiti separati da spazi.|

### <a name="successful-authentication-response"></a>Risposta di autenticazione riuscita 
Nell'esempio seguente viene illustrata una risposta di token riuscita: 

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


|Parametro|Descrizione| 
|-----|-----|
|token_type|Sempre impostato su Bearer.| 
|scope|Se è stato restituito un token di accesso, questo parametro elenca gli ambiti per i quali il token di accesso è valido.| 
|expires_in|Numero di secondi per cui il token di accesso incluso è valido.| 
|access_token|Rilasciato per gli ambiti richiesti.| 
|token ID|Un token Web JSON (JWT). L'app può decodificare i segmenti di questo token per richiedere informazioni sull'utente che ha eseguito l'accesso. L'app può memorizzare nella cache i valori e visualizzarli, ma non deve basarsi su di essi per i limiti di autorizzazione o di sicurezza.| 
|refresh_token_expires_in|Numero di secondi per cui il token di aggiornamento incluso è valido.| 
|token di aggiornamento|Viene emesso se il parametro di ambito originale include offline_access.|

È possibile usare il token di aggiornamento per acquisire nuovi token di accesso e i token di aggiornamento usando lo stesso flusso descritto nella sezione precedente del flusso di concessione del codice di autenticazione.   

## <a name="device-code-flow"></a>Flusso del codice del dispositivo 
 
La concessione del codice del dispositivo consente agli utenti di accedere a dispositivi con vincoli di input, ad esempio una Smart TV, un dispositivo Internet o una stampante. Per abilitare questo flusso, il dispositivo ha l'utente che visita una pagina Web nel browser in un altro dispositivo per eseguire l'accesso. Quando l'utente accede, il dispositivo è in grado di ottenere i token di accesso e i token di aggiornamento in base alle esigenze. 
 
### <a name="protocol-diagram"></a>Diagramma di protocollo 
 
L'intero flusso del codice del dispositivo ha un aspetto simile al diagramma seguente. Ogni passaggio viene descritto più avanti in questo articolo. 
 
![Flusso del codice del dispositivo](media/adfs-scenarios-for-developers/device.png)

### <a name="device-authorization-request"></a>Richiesta di autorizzazione del dispositivo 
Il client deve prima verificare con il server di autenticazione per un dispositivo e un codice utente usato per avviare l'autenticazione. Il client raccoglie la richiesta dall'endpoint/devicecode. In questa richiesta, il client deve includere anche le autorizzazioni necessarie per l'acquisizione da parte dell'utente. Dal momento dell'invio della richiesta, l'utente ha solo 15 minuti per accedere (il valore usuale per expires_in), quindi effettuare questa richiesta solo quando l'utente ha indicato che è pronto per l'accesso. 

```
// Line breaks are for legibility only. 
 
POST https://adfs.contoso.com/adfs/oauth2/devicecode 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
scope=openid 
```


|Parametro|Condizione|Descrizione|
|-----|-----|-----| 
|client_id|necessarie|ID dell'applicazione (client) assegnato dall'AD FS all'app.| 
|scope|facoltative|Elenco di ambiti separati da spazi.|

### <a name="device-authorization-response"></a>Risposta di autorizzazione del dispositivo 
Una risposta con esito positivo sarà un oggetto JSON contenente le informazioni necessarie per consentire all'utente di eseguire l'accesso. 


|Parametro|Descrizione|
|-----|-----| 
|device_code|Stringa estesa utilizzata per verificare la sessione tra il client e il server di autorizzazione. Il client usa questo parametro per richiedere il token di accesso dal server di autorizzazione.| 
|user_code|Una stringa breve mostrata all'utente usato per identificare la sessione in un dispositivo secondario.| 
|verification_uri|URI a cui l'utente deve accedere con user_code per eseguire l'accesso.| 
|verification_uri_complete|URI a cui l'utente deve accedere con user_code per eseguire l'accesso. Questa operazione è precompilata con user_code in modo che l'utente non debba immettere user_code| 
|expires_in|Il numero di secondi prima della scadenza di device_code e user_code.| 
|Intervallo|Numero di secondi di attesa del client tra le richieste di polling.| 
|messaggio|Stringa leggibile con le istruzioni per l'utente. Questo può essere localizzato includendo un parametro di query nella richiesta del modulo? MKT = XX-XX, compilando il codice delle impostazioni cultura della lingua appropriato.  

### <a name="authenticating-the-user"></a>Autenticazione dell'utente 
Dopo aver ricevuto user_code e verification_uri, il client li visualizza all'utente, indicando loro di eseguire l'accesso con il telefono cellulare o il browser per PC. Inoltre, il client può usare un codice a matrice o un meccanismo simile per visualizzare il verfication_uri_complete, che eseguirà il passaggio di immissione di user_code per l'utente. Mentre l'utente esegue l'autenticazione in verification_uri, il client deve eseguire il polling dell'endpoint/token per il token richiesto usando device_code. 

```
POST https://adfs.contoso.com /adfs/oauth2/token 
Content-Type: application/x-www-form-urlencoded 
 
grant_type: urn:ietf:params:oauth:grant-type:device_code 
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e 
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8 
```


|Parametro|necessarie|Descrizione|
|-----|-----|-----| 
|grant_type|necessarie|Deve essere urn: IETF: params: OAuth: Grant-Type: device_code| 
|client_id|necessarie|Deve corrispondere al client_id usato nella richiesta iniziale.| 
|code|necessarie|Device_code restituito nella richiesta di autorizzazione del dispositivo.|

### <a name="successful-authentication-response"></a>Risposta di autenticazione riuscita 
Una risposta di token corretta avrà un aspetto simile al seguente:  


|Parametro|Descrizione|
|-----|-----| 
|token_type|Sempre "Bearer.| 
|scope|Se è stato restituito un token di accesso, verranno elencati gli ambiti per i quali il token di accesso è valido.| 
|expires_in|Numero di secondi prima del quale il token di accesso incluso è valido per.| 
|access_token|Rilasciato per gli ambiti richiesti.| 
|token ID|Viene emesso se il parametro di ambito originale include l'ambito OpenID.| 
|token di aggiornamento|Viene emesso se il parametro di ambito originale include offline_access.| 
|refresh_token_expires_in|Numero di secondi prima del quale il token di aggiornamento incluso è valido per.| 


## <a name="related-content"></a>Contenuti correlati  
Per informazioni dettagliate sull'uso dei flussi correlati, vedere [sviluppo ad FS](../AD-FS-Development.md) per l'elenco completo degli articoli dettagliati. 
