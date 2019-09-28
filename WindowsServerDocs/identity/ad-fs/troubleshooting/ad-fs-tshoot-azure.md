---
title: Risoluzione dei problemi di AD FS-Azure AD
description: In questo documento viene descritto come risolvere i diversi aspetti di AD FS e Azure AD
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 293618b3fe2a24caff8fd6b52c5528cc699f93de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407286"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>Risoluzione dei problemi di AD FS-Azure AD
Con la crescita del cloud, molte aziende si sono trasferita a usare Azure AD per le varie app e servizi.  La Federazione con Azure AD è diventata una procedura standard con molte organizzazioni.  In questo documento vengono descritti alcuni aspetti della risoluzione dei problemi che si verificano con questa federazione.  Molti degli argomenti del documento generale sulla risoluzione dei problemi riguardano ancora la Federazione con Azure, in modo che questo documento si concentri solo su specifiche con Azure AD e AD FS interazione.

## <a name="redirection-to-ad-fs"></a>Reindirizzamento a AD FS
Il reindirizzamento si verifica quando si accede a un'applicazione, ad esempio Office 365, e si viene reindirizzati all'organizzazione AD FS server per l'accesso.

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>Primi elementi da controllare
Se non si verifica il reindirizzamento, è necessario verificare alcuni elementi

   1. Verificare che il tenant di Azure AD sia abilitato per la Federazione accedendo al portale di Azure e controllando in Azure AD Connect.

![](media/ad-fs-tshoot-azure/azure2.png)

1. Verificare che il dominio personalizzato sia verificato facendo clic sul dominio accanto a Federazione nella portale di Azure.
   ![](media/ad-fs-tshoot-azure/azure3.png)

2. Infine, è necessario controllare il [DNS](ad-fs-tshoot-dns.md) e assicurarsi che i server ad FS o i server WAP stiano risolvendo da Internet.  Verificare che l'operazione venga risolta e che sia possibile accedervi.
3. Per ottenere queste informazioni, è anche possibile usare il cmdlet di PowerShell `Get-AzureADDomain`.

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>Si riceve un errore del metodo di autenticazione sconosciuto
È possibile che si verifichi un errore di "metodo di autenticazione sconosciuto" indicante che AuthnContext non è supportato a livello AD FS o STS quando si viene reindirizzati da Azure. 

Questo è più comune quando Azure AD reindirizza al AD FS o al servizio token di servizio usando un parametro che impone un metodo di autenticazione. 

Per applicare un metodo di autenticazione, usare uno dei metodi seguenti:
- Per WS-Federation, usare una stringa di query WAUTH per forzare un metodo di autenticazione preferito.

- Per SAML 2.0, usare quanto segue:
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  Quando il metodo di autenticazione applicato viene inviato con un valore non corretto o se il metodo di autenticazione non è supportato in AD FS o STS, viene visualizzato un messaggio di errore prima che venga eseguita l'autenticazione.

|Metodo di autenticazione desiderato|URI wauth|
|-----|-----|
|Mediante autenticazione con nome utente e password|urn: Oasis: Names: TC: SAML: 1.0: am: password|
|Autenticazione client SSL|urn:ietf:rfc:2246|
|Autenticazione integrata di Windows|urn: Federation: autenticazione: Windows|

Classi del contesto di autenticazione SAML supportate

|Metodo di autenticazione|URI della classe del contesto di autenticazione|
|-----|-----| 
|Nome utente e password|urn: Oasis: Names: TC: SAML: 2.0: AC: Classes: password|
|Trasporto protetto da password|urn: Oasis: Names: TC: SAML: 2.0: AC: Classes: PasswordProtectedTransport|
|Client Transport Layer Security (TLS)|urn: Oasis: Names: TC: SAML: 2.0: AC: Classes: TLSClient
|Certificato X. 509|urn: Oasis: Names: TC: SAML: 2.0: AC: Classes: X509
|Autenticazione integrata di Windows|urn: Federation: autenticazione: Windows|
|Kerberos|urn: Oasis: Names: TC: SAML: 2.0: AC: Classes: Kerberos|

Per assicurarsi che il metodo di autenticazione sia supportato a livello di AD FS, controllare quanto segue.

#### <a name="ad-fs-20"></a>AD FS 2.0 

In **/ADFS/LS/Web.config**verificare che sia presente la voce relativa al tipo di autenticazione.

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

In **gestione ad FS**fare clic su **criteri di autenticazione** nello snap-in ad FS.

Nella sezione **autenticazione primaria** fare clic su Modifica accanto a impostazioni globali. È anche possibile fare clic con il pulsante destro del mouse su criteri di autenticazione, quindi scegliere Modifica autenticazione primaria globale. In alternativa, nel riquadro azioni selezionare Modifica autenticazione primaria globale.

Nella finestra Modifica criteri di autenticazione globali, nella scheda primario, è possibile configurare le impostazioni come parte dei criteri di autenticazione globali. Per l'autenticazione primaria, ad esempio, è possibile selezionare i metodi di autenticazione disponibili in Extranet e Intranet.

\* * Assicurarsi che sia selezionata la casella di controllo metodo di autenticazione richiesto. 

#### <a name="ad-fs-2016"></a>AD FS 2016

In **gestione ad FS**fare clic su **metodi di autenticazione** e **servizio** nello snap-in ad FS.

Nella sezione **autenticazione primaria** fare clic su modifica.

Nella finestra **modifica metodi di autenticazione** , nella scheda primario, è possibile configurare le impostazioni come parte dei criteri di autenticazione.

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>Token emessi da AD FS

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD genera un errore dopo il rilascio del token
Quando AD FS emette un token, Azure AD possibile che venga generato un errore. In questa situazione, verificare i seguenti problemi:
- Le attestazioni rilasciate da AD FS nel token devono corrispondere ai rispettivi attributi dell'utente in Azure AD.
- il token per Azure AD deve contenere le attestazioni obbligatorie seguenti:
    - WSFED 
        - UPN Il valore di questa attestazione deve corrispondere all'UPN degli utenti in Azure AD.
        - ImmutableId Il valore di questa attestazione deve corrispondere a sourceAnchor o ImmutableID dell'utente in Azure AD.

Per ottenere il valore dell'attributo utente in Azure AD, eseguire la riga di comando seguente: `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2,0:
       - Idpemail tratta Il valore di questa attestazione deve corrispondere al nome dell'entità utente degli utenti in Azure AD.
       - NAMEID Il valore di questa attestazione deve corrispondere a sourceAnchor o ImmutableID dell'utente in Azure AD.

Per altre informazioni, vedere [usare un provider di identità SAML 2,0 per implementare Single Sign-on](https://technet.microsoft.com/library/dn641269.aspx).

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>Mancata corrispondenza del certificato per la firma di token tra AD FS e Azure AD.

AD FS usa il certificato per la firma di token per firmare il token inviato all'utente o all'applicazione. Il trust tra il AD FS e il Azure AD è un trust federato basato su questo certificato per la firma di token.

Tuttavia, se il certificato per la firma di token sul lato AD FS è stato modificato a causa del rollover automatico del certificato o di un intervento, i dettagli del nuovo certificato devono essere aggiornati sul lato Azure AD per il dominio federato. Quando il certificato per la firma di token primario nel AD FS è diverso da quello di Azure ADs, il token emesso da AD FS non è considerato attendibile da Azure AD. Pertanto, l'utente federato non è autorizzato ad accedere.

Per risolvere questo problema, è possibile usare la procedura illustrata in [rinnovo dei certificati di federazione per Office 365 e Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).

## <a name="other-common-things-to-check"></a>Altri elementi comuni da controllare
Di seguito è riportato un elenco rapido di elementi da controllare se si verificano problemi con AD FS e Azure AD interazione.
- credenziali obsolete o memorizzate nella cache in Gestione credenziali di Windows
- L'algoritmo hash sicuro configurato nel trust della relying party per Office 365 è impostato su SHA1

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)