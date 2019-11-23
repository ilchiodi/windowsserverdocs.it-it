---
title: Concetti di AD FS OpenID Connect/OAuth
description: Informazioni su AD FS concetti di autenticazione moderna.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0e680e07ce1ee27a73791e310a71b85ad76d6318
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358757"
---
# <a name="ad-fs-openid-connectoauth-concepts"></a>Concetti di AD FS OpenID Connect/OAuth
Si applica a AD FS 2016 e versioni successive
 
## <a name="modern-authentication-actors"></a>Attori di autenticazione moderni 

|Attore| Descrizione|
|-----|-----|
|Utente finale|Si tratta dell'entità di sicurezza (utenti, applicazioni, servizi e gruppi) che deve accedere alla risorsa.|  
|Client|Si tratta dell'applicazione Web identificata dal relativo ID client. Il client è in genere l'entità a cui l'utente finale interagisce e richiede i token dal server di autorizzazione.
|Server di autorizzazione/provider di identità (IdP)| Si tratta del server AD FS. È responsabile della verifica dell'identità delle entità di sicurezza esistenti nella directory di un'organizzazione. Rilascia i token di sicurezza (token di accesso di connessione, token ID e token di aggiornamento) dopo aver eseguito l'autenticazione delle entità di sicurezza.
|Server risorse/provider di risorse/componente| Questa è la posizione in cui risiedono la risorsa o i dati. Considera attendibile il server di autorizzazione per autenticare e autorizzare il client in modo sicuro e usa i token di accesso di connessione per garantire che sia possibile concedere l'accesso a una risorsa.

Il diagramma seguente fornisce la relazione più semplice tra gli attori:

![Attori di autenticazione moderni](media/adfs-modern-auth-concepts/concept1.png)

## <a name="application-types"></a>Tipi di applicazioni 
 

|Tipo di applicazione|Descrizione|Ruolo|
|-----|-----|-----|
|Applicazione nativa|Detto anche **client pubblico**, deve essere un'app client eseguita in un PC o un dispositivo e con cui l'utente interagisce.|Token richieste dal server di autorizzazione (AD FS) per l'accesso utente alle risorse. Invia richieste HTTP alle risorse protette, utilizzando il token come intestazioni HTTP.| 
|Applicazione server (app Web)|Un'applicazione web che viene eseguito in un server ed è in genere accessibili agli utenti tramite un browser. Poiché è in grado di gestire le credenziali o il segreto client, viene talvolta definito **client riservato**. |Token richieste dal server di autorizzazione (AD FS) per l'accesso utente alle risorse. Prima di richiedere un token, il client (app Web) deve eseguire l'autenticazione con il segreto. | 
|API Web|La risorsa finale, l'utente accede. Se si trattasse di come la nuova rappresentazione "relying party".|USA i token di accesso di connessione ottenuti dai client| 

## <a name="application-group"></a>Gruppo di applicazioni 
 
Ogni client OAuth (app nativa o Web) o risorsa (API Web) configurata con AD FS deve essere associata a un gruppo di applicazioni. I client in un gruppo di applicazioni possono essere configurati per accedere alle risorse nello stesso gruppo. Un gruppo di applicazioni può contenere più client e risorse.  

## <a name="security-tokens"></a>Token di sicurezza 
 
L'autenticazione moderna usa i tipi di token seguenti: 
- **id_token**: token JWT emesso dal server di autorizzazione (ad FS) e utilizzato dal client. Le attestazioni nel token ID contengono informazioni sull'utente, in modo che il client possa utilizzarlo.  
- **access_token**: token JWT emesso dal server di autorizzazione (ad FS) e destinato a essere utilizzato dalla risorsa. L'attestazione 'aud' o gruppo di destinatari di questo token deve corrispondere all'identificatore della risorsa o dell'API Web.  
- **refresh_token**: token emesso da ad FS per l'uso da parte del client quando è necessario aggiornare il id_token e access_token. Il token è opaco per il client e può essere utilizzato solo da AD FS.  

## <a name="scopes"></a>Ambiti 
 
Durante la registrazione di una risorsa in AD FS, è possibile configurare gli ambiti in modo da consentire AD FS di eseguire azioni specifiche. Oltre a configurare l'ambito, è necessario inviare anche il valore dell'ambito nella richiesta di AD FS per eseguire l'azione. Ad esempio, l'amministratore deve configurare l'ambito come OpenID durante la registrazione delle risorse e l'applicazione (client) deve inviare l'ambito = OpenID nella richiesta di autenticazione per AD FS per emettere un token ID. Di seguito sono riportati i dettagli sugli ambiti disponibili in AD FS 
 
- Aza: se si utilizzano le [estensioni del protocollo OAuth 2,0 per i client Broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) e se il parametro scope contiene l'ambito "AZA", il server emette un nuovo token di aggiornamento primario e lo imposta nel campo refresh_token della risposta, nonché impostando il campo refresh_token_expires_in sulla durata del nuovo token di aggiornamento primario, se ne viene applicato uno. 
- OpenID: consente all'applicazione di richiedere l'uso del protocollo di autorizzazione OpenID Connect. 
- logon_cert: l'ambito logon_cert consente a un'applicazione di richiedere certificati di accesso, che possono essere usati per accedere in modo interattivo agli utenti autenticati. Il server AD FS omette il parametro access_token dalla risposta e fornisce invece una catena di certificati CMS con codifica Base64 o una risposta PKI completa di CMC. Altre informazioni sono disponibili [qui](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e).
- user_impersonation: l'ambito user_impersonation è necessario per richiedere correttamente un token di accesso per conto di AD FS. Per informazioni dettagliate su come usare questo ambito, vedere la pagina relativa alla creazione di un'applicazione a più livelli con l'uso [di OAuth con AD FS 2016](ad-fs-on-behalf-of-authentication-in-windows-server.md). 
- allatclaims: l'ambito allatclaims consente all'applicazione di richiedere le attestazioni nel token di accesso da aggiungere anche nel token ID.   
- vpn_cert: l'ambito vpn_cert consente a un'applicazione di richiedere certificati VPN, che possono essere usati per stabilire connessioni VPN usando l'autenticazione EAP-TLS. Questa operazione non è più supportata. 
- posta elettronica: consente all'applicazione di richiedere l'attestazione di posta elettronica per l'utente connesso.  
- profilo: consente all'applicazione di richiedere le attestazioni relative al profilo per l'utente di accesso.  

## <a name="claims"></a>Attestazioni 
 
I token di sicurezza (ID e token di accesso) rilasciati da AD FS contengono attestazioni o asserzioni di informazioni sull'oggetto autenticato. Le applicazioni possono utilizzare attestazioni per varie attività, tra cui: 
- Convalidare il token 
- Identificare il tenant di directory dell'oggetto 
- Visualizzare le informazioni utente 
- Determinare l'autorizzazione del soggetto le attestazioni presenti in un determinato token di sicurezza dipendono dal tipo di token, dal tipo di credenziale usato per autenticare l'utente e dalla configurazione dell'applicazione.  
 
## <a name="high-level-ad-fs-authentication-flow"></a>Flusso di autenticazione AD FS di alto livello 

![Flusso di autenticazione AD FS](media/adfs-modern-auth-concepts/adfsauthflow.png)


 1. AD FS riceve la richiesta di autenticazione dal client.  
 
 2. AD FS convalida l'ID client nella richiesta di autenticazione con l'ID client ottenuto durante la registrazione delle risorse e del client nel AD FS. Se si usa Confidential client, AD FS convalida anche il segreto client specificato nella richiesta di autenticazione. AD FS inoltre convalidare l'URI di reindirizzamento del client. 
 
 3. AD FS identifica la risorsa a cui il client desidera accedere tramite il parametro della risorsa passato nella richiesta di autenticazione. Se si usa la libreria client MSAL, il parametro Resource non viene inviato. L'URL della risorsa viene invece inviato come parte del parametro Scope: *scope = [URL risorsa]//[valori di ambito, ad esempio, OpenID]* . 

    Se la risorsa non viene passata utilizzando il parametro resource o scope, ADFS utilizzerà una risorsa predefinita urn: Microsoft: UserInfo i cui criteri, ad esempio l'autenticazione a più fattori, il rilascio o il criterio di autorizzazione, non possono essere configurati. 
 
 4. AD FS successiva verifica che il client disponga delle autorizzazioni per accedere alla risorsa. AD FS inoltre verificare se gli ambiti passati nella richiesta di autenticazione corrispondono agli ambiti configurati durante la registrazione della risorsa. Se il client non dispone delle autorizzazioni o se gli ambiti corretti non vengono inviati nella richiesta di autenticazione, il flusso di autenticazione viene terminato.   
 
 5. Dopo aver convalidato le autorizzazioni e gli ambiti, AD FS autentica l'utente usando il [metodo di autenticazione](../operations/configure-authentication-policies.md)configurato.   

 6. Se è necessario un [metodo di autenticazione aggiuntivo](../operations/configure-additional-authentication-methods-for-ad-fs.md) in base ai criteri delle risorse o ai criteri di autenticazione globali, ad FS attiva l'autenticazione aggiuntiva. 

 7. AD FS usa l'autenticazione a più fattori di [Azure](../operations/configure-ad-fs-and-azure-mfa.md) o di [terze parti](../operations/additional-authentication-methods-ad-fs.md) per eseguire l'autenticazione.   
 
 8. Dopo l'autenticazione dell'utente, AD FS applica le [regole attestazioni](../deployment/configuring-claim-rules.md) (determina le attestazioni inviate alla risorsa come parte dei token di sicurezza) e i [criteri di controllo di accesso](../operations/ad-fs-client-access-policies.md) (determina che l'utente ha soddisfatto le condizioni necessarie per accedere alla risorsa).   

 9. Successivamente, AD FS genera i token di accesso e di aggiornamento. 

 10. AD FS genera anche il token ID. 
 
 11. Se l'ambito = allatclaims è incluso nella richiesta di autenticazione, il [token ID viene personalizzato](custom-id-tokens-in-ad-fs.md) per includere le attestazioni nel token di accesso in base alle regole attestazioni definite. 
    
 12. Una volta generati e personalizzati i token necessari, AD FS risponde al client, inclusi i token. Solo se la richiesta di autenticazione include scope = OpenID, il token ID viene incluso nella risposta. Il client può sempre ottenere il token ID dopo l'autenticazione usando l'endpoint del token. 

## <a name="types-of-libraries"></a>Tipi di librerie 
  
Con AD FS vengono usati due tipi di librerie: 
- **Librerie client**: i client nativi e le app server usano le librerie client per acquisire i token di accesso per chiamare una risorsa, ad esempio un'API Web. Microsoft Authentication Library (MSAL) è la libreria client più recente e consigliata quando si usa AD FS 2019. È consigliabile Active Directory Authentication Library (ADAL) per AD FS 2016.  

- **Librerie middleware server**: le app Web usano le librerie middleware del server per l'accesso utente. Le API Web utilizzano le librerie middleware del server per convalidare i token inviati dai client nativi o da altri server. OWIN (Open Web Interface for .NET) è la libreria middleware consigliata. 

## <a name="customize-id-token-additional-claims-in-id-token"></a>Personalizzare il token ID (altre attestazioni nel token ID)
 
In alcuni scenari è possibile che l'app Web (client) necessiti di altre attestazioni in un token ID per facilitare la funzionalità. Questa operazione può essere eseguita tramite una delle opzioni seguenti. 

**Opzione 1:** Deve essere usato quando si usa un client pubblico e l'app Web non dispone di una risorsa a cui sta tentando di accedere. L'opzione richiede 
1.  response_mode impostato come form_post 
2.  L'identificatore della relying party (identificatore dell'API Web) è uguale all'identificatore client

![Opzione di personalizzazione del token AD FS 1](media/adfs-modern-auth-concepts/option1.png)

**Opzione 2:** Deve essere usato quando l'app Web ha una risorsa a cui sta tentando di accedere e deve passare altre attestazioni tramite il token ID. È possibile usare sia i client pubblici che quelli riservati. L'opzione richiede 
1.  response_mode impostato come form_post 
2.  KB4019472 viene installato nei server AD FS 
3.  Ambito allatclaims assegnato alla coppia client-RP. È possibile assegnare l'ambito usando il cmdlet di PowerShell Grant-ADFSApplicationPermission (use set-AdfsApplicationPermission se già concesso una volta) come indicato nell'esempio seguente: 

    ``` powershell
    Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
    ```

![Opzione Personalizzazione del token AD FS 2](media/adfs-modern-auth-concepts/option2.png)

Per comprendere meglio come configurare un'app Web in ADFS per acquisire un token ID personalizzato, vedere [personalizzare le attestazioni da emettere in id_token quando si usa OpenID Connect o OAuth con AD FS 2016 o versione successiva](Custom-Id-Tokens-in-AD-FS.md).

## <a name="single-log-out"></a>Disconnessione singola

La disconnessione singola comporta l'interruzione di tutte le sessioni client usando l'ID sessione. AD FS 2016 e versioni successive supporta Single logout per OpenID Connect/OAuth. Per altri dettagli, vedere la pagina relativa al [Single logout per OpenID Connect con ad FS](ad-fs-logout-openid-connect.md).


## <a name="ad-fs-endpoints"></a>Endpoint AD FS

|Endpoint AD FS|Descrizione|
|-----|-----|
|/Authorize|AD FS restituisce un codice di autorizzazione che può essere utilizzato per ottenere il token di accesso|
|/token|AD FS restituisce un token di accesso che può essere usato per accedere alla risorsa (API Web)|
|/userinfo|AD FS restituisce le attestazioni relative all'utente autenticato|
|/devicecode|AD FS restituisce il codice del dispositivo e il codice utente|
|/logout|AD FS disconnette l'utente|
|/keys|AD FS le chiavi pubbliche usate per firmare le risposte|
|/.well-known/openid-configuration|AD FS restituisce i metadati OAuth/OpenID Connect|
