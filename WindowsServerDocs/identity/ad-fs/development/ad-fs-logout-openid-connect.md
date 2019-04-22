---
title: Disconnessione singola per OpenID Connect con AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825772"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Disconnessione singola per OpenID Connect con AD FS

## <a name="overview"></a>Panoramica
Compila il supporto Oauth iniziale in ADFS in Windows Server 2012 R2, AD FS 2016 ha introdotto il supporto per OpenId Connect sign-on. Con [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), AD FS 2016 supporta ora single log-out per gli scenari di OpenId Connect. Questo articolo viene fornita una panoramica di single log-out per lo scenario di OpenId Connect e fornisce indicazioni su come usarlo per le applicazioni di OpenId Connect in AD FS.


## <a name="discovery-doc"></a>Documento di individuazione
OpenID Connect Usa un documento JSON definito un "documento di individuazione" per fornire informazioni dettagliate sulla configurazione.  Ciò include l'URI dell'autenticazione, token, userinfo e pubblico-endpoints.  Di seguito è riportato un esempio di documento di individuazione.

```
{
"issuer":"https://fs.fabidentity.com/adfs",
"authorization_endpoint":"https://fs.fabidentity.com/adfs/oauth2/authorize/",
"token_endpoint":"https://fs.fabidentity.com/adfs/oauth2/token/",
"jwks_uri":"https://fs.fabidentity.com/adfs/discovery/keys",
"token_endpoint_auth_methods_supported":["client_secret_post","client_secret_basic","private_key_jwt","windows_client_authentication"],
"response_types_supported":["code","id_token","code id_token","id_token token","code token","code id_token token"],
"response_modes_supported":["query","fragment","form_post"],
"grant_types_supported":["authorization_code","refresh_token","client_credentials","urn:ietf:params:oauth:grant-type:jwt-bearer","implicit","password","srv_challenge"],
"subject_types_supported":["pairwise"],
"scopes_supported":["allatclaims","email","user_impersonation","logon_cert","aza","profile","vpn_cert","winhello_cert","openid"],
"id_token_signing_alg_values_supported":["RS256"],
"token_endpoint_auth_signing_alg_values_supported":["RS256"],
"access_token_issuer":"http://fs.fabidentity.com/adfs/services/trust",
"claims_supported":["aud","iss","iat","exp","auth_time","nonce","at_hash","c_hash","sub","upn","unique_name","pwd_url","pwd_exp","sid"],
"microsoft_multi_refresh_token":true,
"userinfo_endpoint":"https://fs.fabidentity.com/adfs/userinfo",
"capabilities":[],
"end_session_endpoint":"https://fs.fabidentity.com/adfs/oauth2/logout",
"as_access_token_token_binding_supported":true,
"as_refresh_token_token_binding_supported":true,
"resource_access_token_token_binding_supported":true,
"op_id_token_token_binding_supported":true,
"rp_id_token_token_binding_supported":true,
"frontchannel_logout_supported":true,
"frontchannel_logout_session_supported":true
} 
 
```



I seguenti valori aggiuntivi saranno disponibili nel documento di individuazione per indicare il supporto per front-disconnessione del canale:

- frontchannel_logout_supported: valore sarà 'true'
- frontchannel_logout_session_supported: valore sarà 'true'.
- end_session_endpoint: questo è l'URI che il client può usare per avviare disconnessione nel server di disconnessione di OAuth.


## <a name="ad-fs-server-configuration"></a>Configurazione del server AD FS
La proprietà di AD FS EnableOAuthLogout verrà abilitata per impostazione predefinita.  Questa proprietà indica al server AD FS per cercare l'URL (LogoutURI) con il SID per avviare la disconnessione del client. Se non hai [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) installato è possibile usare il comando PowerShell seguente:

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` verrà contrassegnato come obsoleto parametro dopo l'installazione [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` sarà sempre true e non avrà alcun impatto sulla funzionalità di disconnessione.

>[!NOTE]
>è supportato frontchannel_logout **soltanto** dopo l'installazione non di [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configurazione del client
Client deve implementare un url quale "si disconnette' usato per l'accesso utente. Amministratore può configurare il LogoutUri nella configurazione del client usando i cmdlet di PowerShell seguenti. 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

Il `LogoutUri` è l'url usato dagli FS AF "disconnettersi" l'utente. Per l'implementazione di `LogoutUri`, le esigenze di client per assicurarsi che cancella lo stato di autenticazione dell'utente nell'applicazione, ad esempio, eliminando l'autenticazione dei token che possa. ADFS viene esplorato a tale URL, con il SID del parametro di query, la segnalazione di relying party / applicazione per disconnettere l'utente. 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **Token OAuth con ID di sessione**: ADFS include l'id di sessione nel token OAuth al momento del rilascio dei token id_token. Questo sarà utilizzabile in un secondo momento da AD FS per identificare i cookie SSO rilevanti per la pulizia per l'utente.
2.  **Disconnessione in App1 avviata dall'utente**: L'utente può avviare una disconnessione da qualsiasi applicazione ha effettuato l'accesso. In questo scenario di esempio, un utente avvia una disconnessione dalla App1.
3.  **Applicazione invia una richiesta di disconnessione a AD FS**: Dopo che l'utente avvia la disconnessione, l'applicazione invia una richiesta GET a end_session_endpoint di AD FS. L'applicazione può includere facoltativamente id_token_hint come parametro a questa richiesta. Se id_token_hint è presente, ADFS utilizzerà in combinazione con l'ID di sessione per individuare out quali URI il client deve essere reindirizzato a dopo la disconnessione (post_logout_redirect_uri).  Il post_logout_redirect_uri deve essere un uri valido, registrato con AD FS con il parametro RedirectUris.
4.  **AD FS invia disconnessione ha eseguito l'accesso client**: ADFS utilizza il valore dell'identificatore di sessione per trovare l'utente è connesso a client pertinenti. I client identificati vengono inviati richiesta sul LogoutUri registrato con AD FS per avviare una disconnessione sul lato client.

## <a name="faqs"></a>Domande frequenti
**Q:** Non è possibile visualizzare i parametri frontchannel_logout_supported e frontchannel_logout_session_supported nel documento di individuazione.</br>
**R:** Assicurarsi di aver [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) installato in tutti i server AD FS. Fare riferimento a Single log-out in Server 2016 con [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**Q:** Ho configurato di disconnessione singolo come indicato, ma rimane registrano in data altri client.</br>
**R:** Assicurarsi che `LogoutUri` è impostato per tutti i client in cui l'utente ha eseguito l'accesso. ADFS non, inoltre, un tentativo migliore per inviare la richiesta di disconnessione su registrato `LogoutUri`. Client deve implementare la logica per gestire la richiesta ed esegue azioni per la disconnessione l'utente dall'applicazione.</br>

**Q:** Se dopo la disconnessione, uno dei client torna ad AD FS con un token di aggiornamento valido, AD FS rilascerà un token di accesso?</br>
**R:** Sì. È responsabilità dell'applicazione client per eliminare elementi tutti autenticati dopo che è stata ricevuta una richiesta di disconnessione a registrato `LogoutUri`.


## <a name="next-steps"></a>Passaggi successivi
[Sviluppare AD FS](../../ad-fs/AD-FS-Development.md)  
