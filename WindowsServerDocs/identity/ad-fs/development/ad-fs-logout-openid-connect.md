---
title: Disconnessione singola per OpenID Connect con AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5f0127e60243ca81f7e25282adc79e01c54b4b32
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407851"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Disconnessione singola per OpenID Connect con AD FS

## <a name="overview"></a>Panoramica
Basandosi sul supporto OAuth iniziale in AD FS in Windows Server 2012 R2, AD FS 2016 ha introdotto il supporto per l'accesso OpenId Connect. Con [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), ad FS 2016 supporta ora l'accesso single-out per gli scenari di OpenID Connect. Questo articolo fornisce una panoramica dello scenario di accesso single-out per OpenId Connect e fornisce indicazioni su come usarlo per le applicazioni OpenId Connect in AD FS.


## <a name="discovery-doc"></a>Documento di individuazione
OpenID Connect usa un documento JSON denominato "documento di individuazione" per fornire informazioni dettagliate sulla configurazione.  Sono inclusi gli URI dell'autenticazione, del token, della UserInfo e degli endpoint pubblici.  Di seguito è riportato un esempio del documento di individuazione.

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



I seguenti valori aggiuntivi saranno disponibili nel documento di individuazione per indicare il supporto per la disconnessione del canale front-end:

- frontchannel_logout_supported: il valore sarà' true '
- frontchannel_logout_session_supported: il valore sarà' true '.
- end_session_endpoint: URI di disconnessione OAuth che il client può usare per avviare la disconnessione nel server.


## <a name="ad-fs-server-configuration"></a>Configurazione del server AD FS
La proprietà AD FS EnableOAuthLogout sarà abilitata per impostazione predefinita.  Questa proprietà indica al server AD FS di cercare l'URL (LogoutURI) con il SID per avviare la disconnessione nel client. Se [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) non è installato, è possibile usare il comando di PowerShell seguente:

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> il parametro `EnableOAuthLogout` verrà contrassegnato come obsoleto dopo l'installazione di [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` sarà sempre true e non avrà alcun effetto sulla funzionalità di disconnessione.

>[!NOTE]
>frontchannel_logout è supportato **solo** dopo installtion di [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configurazione client
Il client deve implementare un URL che "si disconnette" dall'utente connesso. L'amministratore può configurare LogoutUri nella configurazione client usando i cmdlet di PowerShell seguenti. 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

Il `LogoutUri` è l'URL usato da AF FS per disconnettersi dall'utente. Per l'implementazione del `LogoutUri`, il client deve assicurarsi che cancelli lo stato di autenticazione dell'utente nell'applicazione, ad esempio eliminando i token di autenticazione di cui dispone. AD FS passerà a tale URL, con il SID come parametro di query, segnalando al relying party/applicazione di disconnettere l'utente. 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **Token OAuth con ID sessione**: AD FS include l'ID sessione nel token OAuth al momento del rilascio del token token ID. Questa operazione verrà utilizzata in seguito da AD FS per identificare i cookie SSO rilevanti da pulire per l'utente.
2.  L' **utente avvia la disconnessione in App1**: L'utente può avviare una disconnessione dalle applicazioni registrate. In questo scenario di esempio, un utente avvia una disconnessione da App1.
3.  L' **applicazione invia la richiesta di disconnessione a ad FS**: Dopo l'avvio della disconnessione da parte dell'utente, l'applicazione invia una richiesta GET a end_session_endpoint di AD FS. L'applicazione può facoltativamente includere id_token_hint come parametro per questa richiesta. Se id_token_hint è presente, AD FS lo userà insieme all'ID sessione per individuare l'URI al quale il client deve essere reindirizzato dopo la disconnessione (post_logout_redirect_uri).  Post_logout_redirect_uri deve essere un URI valido registrato con AD FS tramite il parametro RedirectUris.
4.  **Ad FS Invia la disconnessione ai client connessi**: AD FS utilizza il valore dell'identificatore di sessione per individuare i client rilevanti a cui l'utente è connesso. Il client identificato riceve una richiesta sul LogoutUri registrato con AD FS per avviare una disconnessione sul lato client.

## <a name="faqs"></a>Domande frequenti
**D** I parametri frontchannel_logout_supported e frontchannel_logout_session_supported non sono visibili nel documento di individuazione.</br>
**R:** Assicurarsi che [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) sia installato in tutti i server ad FS. Vedere Single Logout in server 2016 with [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**D** Ho configurato la disconnessione singola come diretta, ma l'utente rimane connesso ad altri client.</br>
**R:** Verificare che `LogoutUri` sia impostato su tutti i client in cui l'utente è connesso. Inoltre, AD FS esegue un tentativo migliore di inviare la richiesta di disconnessione nel `LogoutUri` registrato. Il client deve implementare la logica per gestire la richiesta e intraprendere un'azione per la disconnessione dell'utente dall'applicazione.</br>

**D** Se dopo la disconnessione uno dei client torna a AD FS con un token di aggiornamento valido, AD FS emettere un token di accesso?</br>
**R:** Sì. È responsabilità dell'applicazione client eliminare tutti gli artefatti autenticati dopo che è stata ricevuta una richiesta di disconnessione nel `LogoutUri` registrato.


## <a name="next-steps"></a>Passaggi successivi
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
