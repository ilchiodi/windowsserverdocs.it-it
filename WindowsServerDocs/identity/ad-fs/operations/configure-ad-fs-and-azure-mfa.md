---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurare AD FS 2016 e Azure MFA
description: ''
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 62b366b8fa388319a758ab853d28d1c49cb1bf06
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719714"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>Configurare Azure MFA come provider di autenticazione con AD FS

Se l'organizzazione è federata con Azure AD, è possibile usare Azure multi-Factor Authentication per proteggere le risorse di ADFS, sia in locale e nel cloud. Azure MFA consente di eliminare le password e fornire un modo più sicuro per l'autenticazione.  A partire da Windows Server 2016, è ora possibile configurare Azure MFA per l'autenticazione principale o usarlo come provider di autenticazione aggiuntivo. 
  
A differenza con AD FS in Windows Server 2012 R2, la scheda Azure MFA di AD FS 2016 si integra direttamente con Azure AD e non richiede un server Azure MFA locale.   La scheda Azure MFA è integrata in Windows Server 2016 e non è necessario per l'installazione aggiuntivi.


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>La registrazione degli utenti per Azure MFA con AD FS

ADFS non supporta inline &#34;prova backup&#34;, o la registrazione di informazioni sulla verifica di sicurezza di Azure MFA, ad esempio il numero di telefono o app per dispositivi mobili. Ciò significa che gli utenti devono ottenere sarà backup visitando [ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx) prima di usare Azure MFA per l'autenticazione ad applicazioni di AD FS. Quando un utente non è stato eseguito ancora sarà backup in Azure AD prova a eseguire l'autenticazione con Azure MFA in AD FS, si otterrà un errore di AD FS.  In qualità di amministratore di AD FS, è possibile personalizzare questa esperienza di errore per guidare l'utente per la pagina di verifica invece.  È possibile farlo usando la personalizzazione di OnLoad per rilevare la stringa di messaggio di errore all'interno della pagina di AD FS e Mostra un nuovo messaggio guidano l'utente di visitare [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup), quindi eseguire nuovamente l'autenticazione. Per istruzioni dettagliate vedere la "personalizzare AD FS pagina web per guidare gli utenti per registrare i metodi di verifica MFA" di seguito in questo articolo.

>[!NOTE]
> In precedenza, gli utenti erano necessarie per l'autenticazione MFA per la registrazione (che visitano [ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx), ad esempio tramite il collegamento [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)).  A questo punto, un utente di AD FS che le informazioni di verifica MFA non è ancora registrato può accedere ad Azure AD&#34;pagina di verifica s tramite il collegamento [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) usando solo l'autenticazione principale (ad esempio l'autenticazione integrata di Windows o nome utente e password tramite AD FS pagine web).  Se l'utente dispone di alcun metodo di verifica configurato, Azure AD eseguirà registrazione inline in cui l'utente visualizza il messaggio &#34;l'amministratore ha richiesto che configuri questo account per la verifica aggiuntiva di sicurezza&#34;, e l'utente può quindi Selezionare questa opzione per &#34;Configuralo subito&#34;.
> Gli utenti che dispongono già di almeno un metodo di verifica MFA configurato verranno ancora richiesto per fornire l'autenticazione a più fattori quando si visita la pagina di verifica.

## <a name="recommended-deployment-topologies"></a>Topologie di distribuzione consigliate

### <a name="azure-mfa-as-primary-authentication"></a>Azure MFA come metodo di autenticazione primaria

Esistono un paio di motivi per usare Azure MFA come metodo di autenticazione principale con AD FS:

 - Per evitare le password per l'accesso ad Azure AD, Office 365 e ad altre app di AD FS
 - Per proteggere la password basati su accesso richiedendo un fattore aggiuntivo, ad esempio il codice di verifica prima della password

Se si vuole usare Azure MFA come metodo di autenticazione principale in AD FS per ottenere questi vantaggi, si consiglia anche di mantenere la possibilità di usare, ad esempio l'accesso condizionale di Azure AD &#34;true MFA&#34; tramite la richiesta di altri fattori in ADFS.

È possibile eseguire questa operazione configurando l'impostazione di dominio Azure AD per eseguire l'autenticazione a più fattori in locale (impostazione di &#34;SupportsMfa&#34; per $True).  In questa configurazione, ADFS può essere richiesto da Azure AD per eseguire l'autenticazione aggiuntiva o &#34;true MFA&#34; per gli scenari di accesso condizionale che lo richiedono.  

Come descritto in precedenza, qualsiasi utente di AD FS che non è ancora registrata (informazioni di verifica MFA configurate) deve essere chiesto tramite una pagina di errore di ADFS personalizzata visita [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) per configurare le informazioni di verifica, quindi eseguire nuovamente l'accesso di AD FS.  
Poiché Azure MFA come primario viene considerato un fattore singolo, dopo la configurazione iniziale è necessario fornire un fattore aggiuntivo per gestire o aggiornare le informazioni di verifica di Azure AD o per accedere ad altre risorse che richiedono l'autenticazione a più fattori.

>[!NOTE]
> Con ad FS 2019, è necessario apportare una modifica al tipo di attestazione ancoraggio per la relazione di trust di Provider di attestazioni Active Directory e modificare questo valore dal windowsaccountname UPN. Eseguire il cmdlet di PowerShell riportato di seguito. Ciò non ha alcun impatto sul funzionamento interno della farm AD FS. È possibile riscontrare che alcuni utenti potrebbero essere nuovamente richieste le credenziali una volta effettuata questa modifica. Dopo l'accesso anche in questo caso, gli utenti finali non vedranno alcuna differenza. 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA come metodo di autenticazione aggiuntiva per Office 365

In precedenza, se si desidera disporre di Azure MFA come un metodo di autenticazione aggiuntivo in AD FS per Office 365 o altre relying party, l'opzione migliore per configurare Azure AD per MFA, in cui l'autenticazione principale viene eseguito in locale in AD FS e autenticazione a più fattori è tr iggered da Azure AD. A questo punto, è possibile usare Azure MFA come metodo di autenticazione aggiuntivo in AD FS, quando il dominio SupportsMfa è impostato su $True.  

Come descritto in precedenza, qualsiasi utente di AD FS che non è ancora registrata (informazioni di verifica MFA configurate) deve essere chiesto tramite una pagina di errore di ADFS personalizzata visita [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) per configurare le informazioni di verifica, quindi eseguire nuovamente l'accesso di AD FS.  

## <a name="pre-requisites"></a>Prerequisiti

Quando si utilizza l'autenticazione a più Fattori di Azure per l'autenticazione con ADFS, sono necessari i seguenti prerequisiti:  
  
- Un [sottoscrizione di Azure con Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Azure Multi-Factor Authentication](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) 


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
>Assicurarsi che questi passaggi vengono eseguiti su **tutti** server ADFS nella farm. Se si dispone di più server AD FS nella farm, è possibile eseguire la configurazione necessaria in remoto con Azure AD PowerShell.  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Passaggio 1: Generare un certificato per Azure MFA in ogni server AD FS tramite il `New-AdfsAzureMfaTenantCertificate` cmdlet

La prima cosa che è necessario eseguire è generare un certificato per autenticazione a più Fattori di Azure da utilizzare.  Questa operazione può essere eseguita tramite PowerShell.  Il certificato generato è reperibile nell'archivio certificati computer locali ed è contrassegnata con un nome di oggetto contenente il TenantID per la directory di Azure AD.

![AD FS e autenticazione a più Fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

Si noti che TenantID è il nome della directory in Azure AD.  Utilizzare il seguente cmdlet PowerShell per generare il nuovo certificato.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![AD FS e autenticazione a più Fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>Passaggio 2: Aggiungere le nuove credenziali per il multi-Factor Authentication Client entità servizio di Azure

Per abilitare i server AD FS comunicare con il Client di Azure multi-Factor Authentication, è necessario aggiungere le credenziali all'entità servizio per il Client di Azure multi-Factor Authentication. I certificati generati utilizzando il `New-AdfsAzureMFaTenantCertificate` cmdlet fungerà da queste credenziali. Eseguire le operazioni seguenti usando PowerShell per aggiungere le nuove credenziali per il multi-Factor Authentication Client entità servizio di Azure.  

> [!NOTE]
> Per completare questo passaggio è necessario connettersi all'istanza di Azure AD con PowerShell tramite `Connect-MsolService`.  Questi passaggi presuppongono che si sia già connessi tramite PowerShell.  Per informazioni, vedere [ `Connect-MsolService`.](https://msdn.microsoft.com/library/dn194123.aspx)  

**Impostare il certificato come le nuove credenziali contro il Client di Azure multi-Factor Authentication**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> Questo comando deve essere eseguita su tutti i server AD FS nella farm.  MFA Azure AD avrà esito negativo nei server che non sono disponibili il certificato impostato come le nuove credenziali contro il Client di Azure multi-Factor Authentication.

> [!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 è il GUID per il Client di Azure multi-Factor Authentication.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurare la Farm ADFS  
  
Dopo aver completato la sezione precedente per ogni server ADFS, è necessario eseguire il `Set-AdfsAzureMfaTenant` cmdlet.  
  
Questo cmdlet deve essere eseguito solo una volta per una farm ADFS.  Utilizzare PowerShell per completare questo passaggio.
  
> [!NOTE]  
> È necessario riavviare il servizio ADFS in ogni server nella farm, prima che le modifiche diventano effettive.  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  

![AD FS e autenticazione a più Fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
Successivamente, si noterà che è disponibile come un metodo di autenticazione principale per intranet ed extranet utilizzare autenticazione a più Fattori di Azure.    
  
![AD FS e autenticazione a più Fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Il rinnovo e la gestione di AD FS ad Azure i certificati di autenticazione a più fattori

Il materiale sussidiario seguente illustra come gestire i certificati di autenticazione a più fattori di Azure nei server AD FS.
Per impostazione predefinita, quando si configura ADFS con Azure MFA, i certificati generati tramite il `New-AdfsAzureMfaTenantCertificate` cmdlet di PowerShell sono valide per 2 anni.  Per determinare la modalità simile a scadenza dei certificati sono e quindi per rinnovare e installare nuovi certificati, utilizzare la procedura seguente.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Valutare la data di scadenza certificato di Azure MFA per AD FS

In ogni server AD FS, nel computer locale un archivio personale, sarà presente un certificato autofirmato con &#34;OU = Microsoft AD FS Azure MFA&#34; nell'autorità emittente e il soggetto.  Si tratta del certificato di autenticazione a più fattori di Azure.  Controllare il periodo di validità del certificato in ogni server AD FS per determinare la data di scadenza.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Crea nuovo certificato per autenticazione a più fattori Azure AD FS su ogni server AD FS

Se il periodo di validità dei certificati è prossima alla fine, avviare il processo di rinnovo tramite la generazione di un nuovo certificato di autenticazione a più fattori di Azure in ogni server AD FS. In una finestra di comando di PowerShell, generare un nuovo certificato in ogni server AD FS usando il cmdlet seguente:

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

In seguito a questo cmdlet, verrà generato un nuovo certificato valido da 2 giorni nel futuro per 2 giorni + 2 anni.  Operazioni di AD FS e Azure MFA non influirà da questo cmdlet o il nuovo certificato. (Nota: il ritardo di 2 giorni, è intenzionale e assicura tempo sufficiente per eseguire la procedura seguente per configurare il nuovo certificato nel tenant prima di AD FS inizia a usarla per Azure MFA.)

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurare ogni nuovo certificato di Azure MFA per AD FS nel tenant di Azure AD

Usando il modulo Azure AD PowerShell, per ogni nuovo certificato (in ogni server AD FS), aggiornare le impostazioni del tenant Azure AD come indicato di seguito (Nota: è necessario innanzitutto connettersi al tenant usando `Connect-MsolService` per eseguire i comandi seguenti).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

`$certbase64` è il nuovo certificato.  Il certificato con codificata base 64 può essere ottenuto eseguendo l'esportazione del certificato (senza la chiave privata) come file e apertura in Notepad.exe, quindi copiando e incollando alla sessione di PowerShell e l'assegnazione alla variabile con codifica DER `$certbase64`.

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Verificare i nuovi certificati verranno utilizzati per Azure MFA

Una volta il nuovo certificato diventi valido, AD FS verrà vengono prelevati e iniziare a usare ogni rispettivo certificato per Azure MFA in poche ore al giorno.  Una volta in questo caso, in ogni server viene visualizzato un evento registrato nel registro eventi di amministrazione di AD FS con le informazioni seguenti:

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

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personalizzare la pagina web di ADFS per guidare gli utenti per registrare i metodi di verifica MFA

Usare gli esempi seguenti per personalizzare le pagine web di ADFS per gli utenti che non hanno ancora sarà backup (configurato con le informazioni di verifica MFA).

### <a name="find-the-error"></a>Individuare l'errore

In primo luogo, esistono un paio di messaggi di errore diversi che restituirà AD FS nel caso in cui l'utente non dispone di informazioni di verifica.
Se si usa Azure MFA come metodo di autenticazione primaria, l'utente non scalabile viene visualizzata una pagina di errore di ADFS che contiene i messaggi seguenti:
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
Quando si tenta di Azure AD come l'autenticazione aggiuntiva, l'utente non scalabile viene visualizzata una pagina di errore di ADFS che contiene i messaggi seguenti:
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

Per rilevare l'errore e far visualizzare all'utente indicazioni personalizzate sufficiente aggiungere il codice javascript alla fine del file OnLoad che fa parte del tema web AD FS.  In questo modo è possibile eseguire le operazioni seguenti:
 - cercare le stringhe di errore di identificazione
 - fornire contenuto web personalizzato.  

> [!NOTE]
> Per informazioni aggiuntive in generale su come personalizzare il file OnLoad, vedere l'articolo [Advanced Customization of AD FS Sign-in Pages](advanced-customization-of-ad-fs-sign-in-pages.md).

Di seguito è riportato un esempio semplice, è possibile estendere:

1. Aprire Windows PowerShell nel server ADFS primario e creare un nuovo tema Web di AD FS eseguendo il comando seguente:
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. Successivamente, creare la cartella ed esportare il tema Web di AD FS predefinito:

    ``` PowerShell
       New-Item -Path 'c:\Theme' -ItemType Directory;Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. Aprire il file C:\Theme\script\onload.js in un editor di testo
4. Aggiungere il codice seguente alla fine del file OnLoad
    
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
    > È necessario modificare "< YOUR_DOMAIN_NAME_HERE >"; Per usare il nome di dominio. Ad esempio: `var domain_hint = "contoso.com";`
    
5. Salvare il file OnLoad
6. Importare il file OnLoad nel tema personalizzato digitando il comando Windows PowerShell seguente:
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}
    ```
7. Infine, applicare il tema Web di personalizzati AD FS digitando il comando Windows PowerShell seguente:
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName "ProofUp"
    ```

## <a name="next-steps"></a>Passaggi successivi

[Gestire i protocolli TLS/SSL e pacchetti di crittografia usati da AD FS e Azure MFA](manage-ssl-protocols-in-ad-fs.md)
