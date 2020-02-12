---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurare AD FS 2016 e Azure MFA
description: ''
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b658644d1ba7cec1b02a2a51331cd7b7152efc77
ms.sourcegitcommit: 75e611fd5de8b8aa03fc26c2a3d5dbf8211b8ce3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145486"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>Configurare l'autenticazione a più fattori di Azure come provider di autenticazione con AD FS

Se l'organizzazione è federata con Azure AD, è possibile usare Multi-Factor Authentication di Azure per proteggere le risorse AD FS, sia in locale che nel cloud. Autenticazione a più fattori di Azure consente di eliminare le password e fornire un modo più sicuro per l'autenticazione.  A partire da Windows Server 2016, è ora possibile configurare l'autenticazione a più fattori di Azure per l'autenticazione primaria o usarla come provider di autenticazione aggiuntivo. 
  
A differenza di quanto accade con AD FS in Windows Server 2012 R2, l'adapter multi-factor authentication di Azure AD FS 2016 si integra direttamente con Azure AD e non richiede un server di autenticazione a più fattori di Azure in locale.   La scheda multi-factor authentication di Azure è incorporata in Windows Server 2016 e non è necessaria un'installazione aggiuntiva.


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>Registrazione degli utenti per l'autenticazione a più fattori di Azure con AD FS

AD FS non supporta la verifica inline &#34;o&#34;la registrazione delle informazioni di verifica della sicurezza di Azure multi-factor authentication, ad esempio il numero di telefono o l'app per dispositivi mobili. Questo significa che gli utenti devono essere riprovati visitando [https://account.activedirectory.windowsazure.com/Proofup.aspx](https://account.activedirectory.windowsazure.com/Proofup.aspx) prima di usare l'autenticazione a più fattori di Azure per l'autenticazione a ad FS applicazioni. Quando un utente che non ha ancora eseguito la prova in Azure AD prova a eseguire l'autenticazione con l'autenticazione a più fattori di Azure in AD FS, riceverà un errore di AD FS.  In qualità di amministratore di AD FS, è possibile personalizzare questa esperienza di errore per indirizzare l'utente alla pagina verifica.  A tale scopo, è possibile usare la personalizzazione OnLoad. js per rilevare la stringa del messaggio di errore all'interno della pagina AD FS e visualizzare un nuovo messaggio per guidare gli utenti a visitare [https://aka.ms/mfasetup](https://aka.ms/mfasetup), quindi ripetere l'autenticazione. Per istruzioni dettagliate, vedere la pagina Web relativa alla personalizzazione del AD FS per guidare gli utenti nella registrazione dei metodi di verifica dell'autenticazione a più fattori più avanti in questo articolo.

>[!NOTE]
> In precedenza, agli utenti veniva richiesto di eseguire l'autenticazione con l'autenticazione a più fattori per la registrazione (visitando [https://account.activedirectory.windowsazure.com/Proofup.aspx](https://account.activedirectory.windowsazure.com/Proofup.aspx), ad esempio tramite il collegamento [https://aka.ms/mfasetup](https://aka.ms/mfasetup)).  A questo punto, un utente AD FS che non ha ancora registrato le informazioni di verifica&#34;dell'autenticazione a più fattori può accedere alla pagina verifica di Azure ad s tramite il collegamento [https://aka.ms/mfasetup](https://aka.ms/mfasetup) usando solo l'autenticazione primaria, ad esempio l'autenticazione integrata di Windows o il nome utente e la password tramite le pagine Web di ad FS.  Se per l'utente non sono configurati metodi di verifica, Azure AD eseguirà la registrazione inline in cui &#34;l'utente Visualizza il messaggio richiesto dall'amministratore per la configurazione di questo account&#34;per la verifica aggiuntiva di sicurezza e &#34;l'utente può scegliere&#34;di impostarlo ora.
> Agli utenti che dispongono già di almeno un metodo di verifica dell'autenticazione a più fattori configurato verrà comunque richiesto di fornire l'autenticazione a più fattori quando si visita la pagina verifica.

## <a name="recommended-deployment-topologies"></a>Topologie di distribuzione consigliate

### <a name="azure-mfa-as-primary-authentication"></a>Autenticazione a più fattori di Azure come autenticazione primaria

Esistono due motivi eccezionali per usare l'autenticazione a più fattori di Azure come autenticazione primaria con AD FS:

 - Per evitare la password per l'accesso Azure AD, Office 365 e altre app AD FS
 - Per proteggere l'accesso basato su password richiedendo un fattore aggiuntivo, ad esempio il codice di verifica prima della password

Se si vuole usare l'autenticazione a più fattori di Azure come metodo di autenticazione principale in AD FS per ottenere questi vantaggi, è probabile che si voglia anche usare Azure AD l'accesso &#34;condizionale&#34; , inclusa la vera autenticazione a più fattori, richiedendo altri fattori in ad FS.

A questo punto è possibile configurare l'impostazione del dominio Azure AD per eseguire l'autenticazione a più fattori &#34;in&#34; locale, impostando SupportsMfa su $true.  In questa configurazione AD FS possibile richiedere Azure AD di eseguire l'autenticazione aggiuntiva o &#34;l'autenticazione a&#34; più fattori vera per gli scenari di accesso condizionale che lo richiedono.  

Come descritto in precedenza, qualsiasi utente AD FS che non ha ancora eseguito la registrazione (informazioni di verifica dell'autenticazione a più fattori configurata) deve essere richiesto tramite una pagina di errore AD FS personalizzata per visitare [https://aka.ms/mfasetup](https://aka.ms/mfasetup) per configurare le informazioni di verifica, quindi eseguire nuovamente il tentativo di accesso ad FS.  
Poiché l'autenticazione a più fattori di Azure come primaria è considerata un fattore singolo, dopo la configurazione iniziale gli utenti dovranno fornire un fattore aggiuntivo per gestire o aggiornare le informazioni di verifica in Azure AD o per accedere ad altre risorse che richiedono l'autenticazione a più fattori.

>[!NOTE]
> Con ADFS 2019, è necessario apportare una modifica al tipo di attestazione ancoraggio per il Active Directory attendibilità del provider di attestazioni e modificarlo da WindowsAccountName a UPN. Eseguire il cmdlet di PowerShell fornito di seguito. Questa operazione non ha alcun effetto sul funzionamento interno della farm AD FS. Una volta apportata la modifica, è possibile notare che è possibile che vengano richieste le credenziali a pochi utenti. Dopo l'accesso, gli utenti finali non vedranno alcuna differenza. 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Autenticazione a più fattori di Azure come autenticazione aggiuntiva per Office 365

In precedenza, se si desiderava usare l'autenticazione a più fattori di Azure come metodo di autenticazione aggiuntivo in AD FS per Office 365 o altre relying party, l'opzione migliore consisteva nel configurare Azure AD per l'autenticazione a più fattori, in cui l'autenticazione principale viene eseguita in locale in AD FS e l'autenticazione a più fattori è TR iggered per Azure AD. A questo punto, è possibile usare l'autenticazione a più fattori di Azure come autenticazione aggiuntiva in AD FS quando l'impostazione Domain SupportsMfa è impostata su $True.  

Come descritto in precedenza, qualsiasi utente AD FS che non ha ancora eseguito la registrazione (informazioni di verifica dell'autenticazione a più fattori configurata) deve essere richiesto tramite una pagina di errore AD FS personalizzata per visitare [https://aka.ms/mfasetup](https://aka.ms/mfasetup) per configurare le informazioni di verifica, quindi eseguire nuovamente il tentativo di accesso ad FS.  

## <a name="pre-requisites"></a>Prerequisiti

Quando si utilizza l'autenticazione a più Fattori di Azure per l'autenticazione con ADFS, sono necessari i seguenti prerequisiti:  
  
- Un [sottoscrizione di Azure con Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Multi-Factor Authentication di Azure](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) 


> [!NOTE]
> Azure AD e autenticazione a più Fattori di Azure sono incluse in Azure AD Premium ed Enterprise Mobility Suite (EMS).  Se si dispone di uno di questi elementi non è necessario sottoscrizioni individuali.

- Un ambiente locale di ADFS di Windows Server 2016.  
   - Il server deve essere in grado di comunicare con gli URL seguenti sulla porta 443.
      - https://adnotifications.windowsazure.com
      - https://login.microsoftonline.com
- L'ambiente locale è [federata con Azure AD.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Modulo di Windows Azure Active Directory per Windows PowerShell](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).  
- Autorizzazioni di amministratore globale sull'istanza di Azure AD per configurarlo mediante Azure AD PowerShell.  
- Credenziali di amministratore dell'organizzazione per configurare la farm di ADFS per l'autenticazione a più Fattori di Azure.

## <a name="configure-the-ad-fs-servers"></a>Configurare il server ADFS

Per completare configurazione per Azure MFA per ADFS, è necessario configurare ciascun server ADFS eseguendo la procedura descritta. 

>[!NOTE]
>Assicurarsi che questi passaggi vengono eseguiti su **tutti** server ADFS nella farm. Se nella farm sono presenti più server AD FS, è possibile eseguire la configurazione necessaria in remoto usando Azure AD PowerShell.  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Passaggio 1: generare un certificato per l'autenticazione a più fattori di Azure in ogni server di AD FS usando il cmdlet `New-AdfsAzureMfaTenantCertificate`

La prima cosa che è necessario eseguire è generare un certificato per autenticazione a più Fattori di Azure da utilizzare.  Questa operazione può essere eseguita tramite PowerShell.  Il certificato generato è reperibile nell'archivio certificati computer locali ed è contrassegnata con un nome di oggetto contenente il TenantID per la directory di Azure AD.

![AD FS e autenticazione a più Fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

Si noti che TenantID è il nome della directory in Azure AD.  Utilizzare il seguente cmdlet PowerShell per generare il nuovo certificato.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![AD FS e autenticazione a più Fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>Passaggio 2: aggiungere le nuove credenziali all'entità servizio del client Azure Multifactor Authentication

Per consentire ai server AD FS di comunicare con il client Azure Multifactor Authentication, è necessario aggiungere le credenziali all'entità servizio per il client Azure Multifactor Authentication. I certificati generati utilizzando il `New-AdfsAzureMFaTenantCertificate` cmdlet fungerà da queste credenziali. Eseguire le operazioni seguenti usando PowerShell per aggiungere le nuove credenziali all'entità servizio del client Azure Multifactor Authentication.  

> [!NOTE]
> Per completare questo passaggio, è necessario connettersi all'istanza di Azure AD con PowerShell usando `Connect-MsolService`.  Questi passaggi presuppongono che si sia già connessi tramite PowerShell.  Per informazioni [, vedere`Connect-MsolService`.](https://msdn.microsoft.com/library/dn194123.aspx)  

**Impostare il certificato come nuova credenziale per il client Azure Multifactor Authentication**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> Questo comando deve essere eseguito in tutti i server AD FS della farm.  Azure AD autenticazione a più fattori avrà esito negativo nei server per i quali non è stato impostato il certificato come nuove credenziali per il client Azure multi-factor authentication.

> [!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 è il GUID del client Azure Multifactor Authentication.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurare la Farm ADFS  
  
Dopo aver completato la sezione precedente in ogni server AD FS, impostare le informazioni sul tenant di Azure usando il cmdlet [set-AdfsAzureMfaTenant](https://docs.microsoft.com/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) . Questo cmdlet deve essere eseguito solo una volta per una farm ADFS.

Aprire un prompt di PowerShell e immettere il proprio *tenantId* con il cmdlet [set-AdfsAzureMfaTenant](https://docs.microsoft.com/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) . Per i clienti che usano Microsoft Azure per enti pubblici cloud, aggiungere il parametro `-Environment USGov`:

> [!NOTE]
> Prima che queste modifiche abbiano effetto, è necessario riavviare il servizio AD FS in ogni server della farm. Per avere un effetto minimo, estrarre ogni AD FS server dalla rotazione NLB una alla volta e attendere lo svuotamento di tutte le connessioni.

```powershell
Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720
```

![AD FS e autenticazione a più Fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)

Windows Server senza la Service Pack più recente non supporta il parametro `-Environment` per il cmdlet [set-AdfsAzureMfaTenant](https://docs.microsoft.com/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) . Se si usa il cloud di Azure per enti pubblici e i passaggi precedenti non sono riusciti a configurare il tenant di Azure a causa del parametro `-Environment` mancante, completare la procedura seguente per creare manualmente le voci del registro di sistema. Ignorare questa procedura se il cmdlet precedente ha registrato correttamente le informazioni sul tenant oppure non si è nel cloud di Azure per enti pubblici:

1. Aprire l' **Editor del registro di sistema** nel server ad FS.
1. Passare a `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ADFS`. Creare i valori della chiave del registro di sistema seguenti:

    | Chiave del Registro di sistema       | Valore |
    |--------------------|-----------------------------------|
    | SasUrl             | https://adnotifications.windowsazure.us/StrongAuthenticationService.svc/Connector |
    | StsUrl             | https://login.microsoftonline.us |
    | ResourceUri        | https://adnotifications.windowsazure.us/StrongAuthenticationService.svc/Connector |

1. Riavviare il servizio AD FS in ogni server della farm prima che le modifiche abbiano effetto. Per avere un effetto minimo, estrarre ogni AD FS server dalla rotazione NLB una alla volta e attendere lo svuotamento di tutte le connessioni.

Successivamente, si noterà che è disponibile come un metodo di autenticazione principale per intranet ed extranet utilizzare autenticazione a più Fattori di Azure.

![AD FS e autenticazione a più Fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Rinnovare e gestire AD FS certificati multi-factor authentication di Azure

Le linee guida seguenti illustrano come gestire i certificati di autenticazione a più fattori di Azure nei server AD FS.
Per impostazione predefinita, quando si configura AD FS con l'autenticazione a più fattori di Azure, i certificati generati tramite il cmdlet `New-AdfsAzureMfaTenantCertificate` PowerShell sono validi per 2 anni.  Per determinare la distanza di scadenza dei certificati, quindi per rinnovare e installare nuovi certificati, attenersi alla procedura riportata di seguito.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Valutazione AD FS data di scadenza del certificato di autenticazione a più fattori

In ogni server AD FS, nel computer locale archivio personale, sarà presente un certificato autofirmato con &#34;ou = Microsoft ad FS Azure multi-&#34; Factor Authentication nell'emittente e nell'oggetto.  Questo è il certificato di autenticazione a più fattori di Azure.  Verificare il periodo di validità del certificato in ogni server AD FS per determinare la data di scadenza.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Crea nuovo AD FS certificato di autenticazione a più fattori di Azure in ogni server AD FS

Se il periodo di validità dei certificati sta per scadere, avviare il processo di rinnovo generando un nuovo certificato di autenticazione a più fattori di Azure in ogni server AD FS. In una finestra di comando di PowerShell generare un nuovo certificato in ogni server AD FS usando il cmdlet seguente:

> [!CAUTION]
> Se il certificato è già scaduto, non aggiungere il parametro `-Renew $true` al comando seguente. In questo scenario, il certificato scaduto esistente viene sostituito con un nuovo certificato anziché essere lasciato disponibile e viene creato un altro certificato.

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

Se il certificato non è già scaduto, viene generato un nuovo certificato valido da 2 giorni in futuro a 2 giorni + 2 anni. Le operazioni di AD FS e di autenticazione a più fattori di Azure non sono interessate da questo cmdlet o dal nuovo certificato. Nota: il ritardo di 2 giorni è intenzionale e fornisce il tempo necessario per eseguire la procedura seguente per configurare il nuovo certificato nel tenant prima che AD FS inizi a usarlo per l'autenticazione a più fattori di Azure.

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurare ogni nuovo certificato di autenticazione a più fattori di AD FS Azure nel tenant di Azure AD

Con il modulo Azure AD PowerShell, per ogni nuovo certificato (in ogni server AD FS) aggiornare le impostazioni del tenant Azure AD come indicato di seguito (Nota: è necessario prima connettersi al tenant usando `Connect-MsolService` per eseguire i comandi seguenti).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

Se il certificato precedente è già scaduto, riavviare il servizio AD FS per selezionare il nuovo certificato. Non è necessario riavviare il servizio AD FS se è stato rinnovato un certificato prima della scadenza.

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Verificare che i nuovi certificati verranno usati per l'autenticazione a più fattori di Azure

Una volta che i nuovi certificati sono diventati validi, AD FS li preleverà e inizierà a usare ogni rispettivo certificato per l'autenticazione a più fattori di Azure entro poche ore a un giorno.  Una volta eseguita questa operazione, in ogni server verrà visualizzato un evento registrato nel registro eventi di AD FS amministrazione con le informazioni seguenti:

```
Log Name:      AD FS/Admin
Source:        AD FS
Date:          2/27/2018 7:33:31 PM
Event ID:      547
Task Category: None
Level:         Information
Keywords:      AD FS
User:          DOMAIN\adfssvc
Computer:      ADFS.domain.contoso.com
Description:
The tenant certificate for Azure MFA has been renewed.  

TenantId: contoso.onmicrosoft.com.
Old thumbprint: 7CC103D60967318A11D8C51C289EF85214D9FC63.
Old expiration date: 9/15/2019 9:43:17 PM.
New thumbprint: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF.
New expiration date: 2/27/2020 2:16:07 AM.
```

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personalizzare la pagina Web AD FS per guidare gli utenti a registrare i metodi di verifica dell'autenticazione a più fattori

Usare gli esempi seguenti per personalizzare le pagine Web di AD FS per gli utenti che non hanno ancora eseguito la prova (informazioni di verifica dell'autenticazione a più fattori configurate).

### <a name="find-the-error"></a>Trovare l'errore

In primo luogo, sono presenti due messaggi di errore diversi AD FS restituiranno nel caso in cui l'utente non disponga di informazioni di verifica.
Se si usa l'autenticazione a più fattori di Azure come autenticazione primaria, l'utente non corretto visualizzerà una pagina di errore AD FS contenente i messaggi seguenti:
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
Quando si tenta di Azure AD come un'autenticazione aggiuntiva, l'utente non corretto visualizzerà una pagina di errore AD FS contenente i messaggi seguenti:
```
<div id='mfaGreetingDescription' class='groupMargin'>For security reasons, we require additional information to verify your account (mahesh@jenfield.net)</div>
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            The selected authentication method is not available for &#39;username@contoso.com&#39;. Choose another authentication method or contact your system administrator for details.
        </div>
```

### <a name="catch-the-error-and-update-the-page-text"></a>Rilevare l'errore e aggiornare il testo della pagina

Per intercettare l'errore e visualizzare le indicazioni personalizzate per l'utente, è sufficiente aggiungere il codice JavaScript alla fine del file OnLoad. js che fa parte del tema Web AD FS.  In questo modo è possibile eseguire le operazioni seguenti:
 - cercare le stringhe di errore di identificazione
 - fornire contenuto Web personalizzato.  

> [!NOTE]
> Per informazioni generali su come personalizzare il file OnLoad. js, vedere l'articolo [personalizzazione avanzata delle pagine di accesso ad FS](advanced-customization-of-ad-fs-sign-in-pages.md).

Di seguito è riportato un semplice esempio che può essere utile estendere:

1. Aprire Windows PowerShell nel server di AD FS primario e creare un nuovo tema Web AD FS eseguendo il comando seguente:
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. Successivamente, creare la cartella ed esportare il tema predefinito AD FS Web:

    ``` PowerShell
       New-Item -Path 'c:\Theme' -ItemType Directory;Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. Aprire il file C:\Theme\script\onload.js in un editor di testo
4. Aggiungere il codice seguente alla fine del file OnLoad. js
    
    ``` JavaScript
    //Custom Code
    //Customize MFA exception
    //Begin

    var domain_hint = "<YOUR_DOMAIN_NAME_HERE>";
    var mfaSecondFactorErr = "The selected authentication method is not available for";
    var mfaProofupMessage = "You will be automatically redirected in 5 seconds to set up your account for additional security verification. Once you have completed the setup, please return to the application you are attempting to access.<br><br>If you are not redirected automatically, please click <a href='{0}'>here</a>."
    var authArea = document.getElementById("authArea");
    if (authArea) {
        var errorMessage = document.getElementById("errorMessage");
        if (errorMessage) {
            if (errorMessage.innerHTML.indexOf(mfaSecondFactorErr) >= 0) {

                //Hide the error message
                var openingMessage = document.getElementById("openingMessage");
                if (openingMessage) {
                    openingMessage.style.display = 'none'
                }
                var errorDetailsLink = document.getElementById("errorDetailsLink");
                if (errorDetailsLink) {
                    errorDetailsLink.style.display = 'none'
                }

                //Provide a message and redirect to Azure AD MFA Registration Url
                var mfaRegisterUrl = "https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1&whr=" + domain_hint;
                errorMessage.innerHTML = "<br>" + mfaProofupMessage.replace("{0}", mfaRegisterUrl);
                window.setTimeout(function () { window.location.href = mfaRegisterUrl; }, 5000);
            }
        }
    }

    //End Customize MFA Exception
    //End Custom Code
    ```
    > [!IMPORTANT]
    > È necessario modificare "< YOUR_DOMAIN_NAME_HERE >"; per usare il nome di dominio. Ad esempio: `var domain_hint = "contoso.com";`
    
5. Salvare il file OnLoad. js
6. Importare il file OnLoad. js nel tema personalizzato digitando il comando seguente di Windows PowerShell:
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}
    ```
7. Infine, applicare il tema Web AD FS personalizzato digitando il comando seguente di Windows PowerShell:
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName "ProofUp"
    ```

## <a name="next-steps"></a>Passaggi successivi

[Gestire i protocolli TLS/SSL e i pacchetti di crittografia usati da AD FS e dall'autenticazione a più fattori di Azure](manage-ssl-protocols-in-ad-fs.md)
