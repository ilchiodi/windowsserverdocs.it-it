---
title: Risoluzione dei problemi di AD FS - Azure AD
description: Questo documento descrive come risolvere i vari aspetti di AD FS e Azure AD
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 228ef34ab25276c1cf98f9b2b64e997390023c87
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444016"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>Risoluzione dei problemi di AD FS - Azure AD
Con la crescita del cloud, molte aziende hanno stati lo spostamento di usare Azure AD per le App e servizi diversi.  La federazione con Azure AD è diventata una procedura standard con molte organizzazioni.  Questo documento illustra alcuni degli aspetti di risoluzione dei problemi che si verificano con questa federazione.  Alcuni degli argomenti nel documento di risoluzione dei problemi generale ancora relativi alla federazione con Azure in modo che questo documento verrà illustrato sola specifiche con Azure AD e l'interazione di AD FS.

## <a name="redirection-to-ad-fs"></a>Reindirizzamento al servizio AD FS
Il reindirizzamento si verifica quando avere effettuato l'accesso a un'applicazione, ad esempio Office 365 e sono "reindirizzata" per le organizzazioni a server AD FS sign-in.

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>Prima di tutto controllare
Se il reindirizzamento avviene non che sono presenti alcuni aspetti da controllare

   1. Assicurarsi che i tenant di Azure AD è abilitato per la federazione eseguendo l'accesso al portale di Azure e controllare in Azure AD Connect.

![](media/ad-fs-tshoot-azure/azure2.png)

1. Assicurarsi che il dominio personalizzato viene verificato, fare clic sul dominio accanto alla federazione nel portale di Azure.
   ![](media/ad-fs-tshoot-azure/azure3.png)

2. Infine, si desidera controllare [DNS](ad-fs-tshoot-dns.md) e assicurarsi che il server AD FS o server WAP stanno risolvendo da internet.  Verificare che questo venga risolto e che sia possibile passare a tale percorso.
3. È anche possibile usare il cmdlet di PowerShell `Get-AzureADDomain` per ottenere queste informazioni anche.

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>Si riceve un errore di autenticazione sconosciuto (metodo)
È possibile riscontrare un errore di "Metodo di autenticazione sconosciuto" che indica che AuthnContext non è supportato a livello di AD FS o servizio token di sicurezza quando si verrà reindirizzati da Azure. 

Questo è più comune quando Azure AD reindirizza ad AD FS o servizio token di sicurezza tramite un parametro che applica un metodo di autenticazione. 

Per imporre un metodo di autenticazione, usare uno dei metodi seguenti:
- Per WS-Federation, usare una stringa di query WAUTH per forzare un metodo di autenticazione preferito.

- Per SAML2.0, usare il comando seguente:
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  Quando il metodo di autenticazione applicato viene inviato con un valore non corretto o se tale metodo di autenticazione non è supportato su AD FS o servizio token di sicurezza, viene visualizzato un messaggio di errore prima che l'utente sia autenticato.

|Metodo di autenticazione desiderato|wauth URI|
|-----|-----|
|Mediante autenticazione con nome utente e password|urn:oasis:names:tc:SAML:1.0:am:password|
|Autenticazione client SSL|urn:ietf:rfc:2246|
|Autenticazione integrata di Windows|urn:federation:authentication:windows|

Classi supportate contesto di autenticazione SAML

|Metodo di autenticazione|Classe contesto di autenticazione URI|
|-----|-----| 
|Nome utente e password|urn:oasis:names:tc:SAML:2.0:ac:classes:Password|
|Trasporto protetto da password|urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport|
|Client di Transport Layer Security (TLS)|urn:oasis:names:tc:SAML:2.0:ac:classes:TLSClient
|Certificato X.509|urn:oasis:names:tc:SAML:2.0:ac:classes:X509
|Autenticazione integrata di Windows|urn:federation:authentication:windows|
|Kerberos|urn:oasis:names:tc:SAML:2.0:ac:classes:Kerberos|

Per assicurarsi che il metodo di autenticazione è supportato a livello di AD FS, verificare quanto segue.

#### <a name="ad-fs-20"></a>AD FS 2.0 

Sotto **/adfs/ls/web.config**, assicurarsi che sia presente la voce per il tipo di autenticazione.

```
<microsoft.identityServer.web>
<localAuthenticationTypes>
<add name="Forms" page="FormsSignIn.aspx" />
<add name="Integrated" page="auth/integrated/" />
<add name="TlsClient" page="auth/sslclient/" />
<add name="Basic" page="auth/basic/" />
</localAuthenticationTypes>
```

#### <a name="ad-fs-2012-r2"></a>AD FS 2012 R2

Sotto **gestione di AD FS**, fare clic su **criteri di autenticazione** in AD FS lo snap-in.

Nel **l'autenticazione principale** sezione, fare clic su Modifica accanto alle impostazioni globali. È possibile anche fare doppio clic su criteri di autenticazione e quindi selezionare Modifica autenticazione primaria globale. In alternativa, nel riquadro azioni, selezionare Modifica autenticazione primaria globale.

Nella finestra Modifica criteri di autenticazione globali, nella scheda principale, è possibile configurare le impostazioni come parte di criteri di autenticazione globali. Ad esempio, per l'autenticazione primaria, è possibile selezionare i metodi di autenticazione disponibili nella Extranet e Intranet.

* * Verificare che che sia selezionata la casella di controllo di metodo di autenticazione necessari. 

#### <a name="ad-fs-2016"></a>AD FS 2016

Sotto **gestione di AD FS**, fare clic su **Service** e **metodi di autenticazione** in AD FS lo snap-in.

Nel **l'autenticazione principale** sezione, fare clic su Modifica.

Nel **metodi di autenticazione modifica** finestra, nella scheda principale, è possibile configurare impostazioni come parte dei criteri di autenticazione.

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>Token emessi da AD FS

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD genera l'errore dopo l'emissione di token
Dopo che ADFS rilascia un token, Azure AD possono generare un errore. In questo caso, verificare i problemi seguenti:
- Le attestazioni rilasciate da AD FS nel token devono corrispondere i rispettivi attributi dell'utente in Azure AD.
- il token per Azure AD deve contenere le attestazioni necessarie seguenti:
    - WSFED: 
        - UPN: Il valore di questa attestazione deve corrispondere l'UPN degli utenti in Azure AD.
        - ImmutableID: Il valore di questa attestazione deve corrispondere l'attributo sourceAnchor o ImmutableID dell'utente in Azure AD.

Per ottenere il valore dell'attributo utente in Azure AD, eseguire la seguente riga di comando: `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2.0:
       - IDPEmail: Il valore di questa attestazione deve corrispondere il nome dell'entità utente degli utenti in Azure AD.
       - NAMEID: Il valore di questa attestazione deve corrispondere l'attributo sourceAnchor o ImmutableID dell'utente in Azure AD.

Per altre informazioni, vedere [usare un provider di identità SAML 2.0 per implementare single sign-on](https://technet.microsoft.com/library/dn641269.aspx).

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>Token di firma certificato mancata corrispondenza tra ADFS e Azure AD.

ADFS utilizza il certificato di firma di token per firmare il token che viene inviato all'utente o applicazione. La relazione di trust tra il AD FS e Azure AD è una relazione di trust federativa che è basato su questo certificato di firma di token.

Tuttavia, se il certificato di firma di token sul lato ADFS viene modificato a causa di rollover dei certificati automaticamente o tramite un intervento, è necessario aggiornare i dettagli del nuovo certificato sul lato Azure AD per il dominio federato. Quando il certificato di firma di token primario in AD FS è diverso da istanze di Azure ad, il token emesso da AD FS non è considerato attendibile da Azure AD. Quindi, l'utente federato non è consentito effettuare l'accesso.

Per risolvere il problema è possibile usare nei passaggi descritto nella [rinnovare i certificati di federazione per Office 365 e Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).

## <a name="other-common-things-to-check"></a>Altri elementi comuni da controllare
Di seguito è riportato un rapido elenco di elementi da controllare se si sono verificati problemi durante l'interazione di AD FS e Azure AD.
- credenziali non aggiornate o memorizzato nella cache in Gestione credenziali di Windows
- Algoritmo Hash sicuro configurato nel Trust della Relying Party di Office 365 è impostata su SHA1

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)