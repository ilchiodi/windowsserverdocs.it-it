---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: Configurare i criteri di autenticazione
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 46eb61db92207a73320f87790a4063076a3cac4f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817284"
---
# <a name="configure-authentication-policies"></a>Configurare i criteri di autenticazione

In ADFS, in Windows Server 2012 R2, entrambi controllo di accesso e il meccanismo di autenticazione sono stati migliorati con più fattori che includono dati utente, dispositivo, posizione e l'autenticazione. Questi miglioramenti consentono, tramite l'interfaccia utente o tramite Windows PowerShell, per gestire il rischio di concessione di autorizzazioni di accesso ad ADFS\-protetto applicazioni tramite più\-fattore di controllo di accesso e multi-\-l'autenticazione a fattore basato su utente identità o appartenenza al gruppo, il percorso di rete, dati del dispositivo di lavoro\-unita in join, e l'autenticazione quando più\-l'autenticazione a fattore \(autenticazione a più Fattori\) è stata eseguita.  

Per ulteriori informazioni sull'autenticazione a più Fattori e multi-\-fattore di controllo di accesso in Active Directory Federation Services \(ADFS\) in Windows Server 2012 R2, vedere gli argomenti seguenti:  


-   [Accedere a una rete aziendale da qualsiasi dispositivo per SSO e l'autenticazione a due fattori trasparente per tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Gestire i rischi con il controllo di accesso condizionale](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurare i criteri di autenticazione tramite lo snap di gestione di ADFS\-in  
Per eseguire queste procedure è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   

In ADFS, in Windows Server 2012 R2, è possibile specificare un criterio di autenticazione in un ambito globale che è applicabile a tutte le applicazioni e servizi protetti da ADFS. È inoltre possibile impostare criteri di autenticazione per applicazioni specifiche e i servizi che si basano su attendibilità e sono protetti da ADFS. Specifica un criterio di autenticazione per un'applicazione specifica per ogni componente attendibilità non esegue l'override di criteri di autenticazione globali. Se globale o per ogni componente attendibilità del criterio di autenticazione richiede l'autenticazione a più Fattori, autenticazione a più Fattori viene attivata quando l'utente tenta di eseguire l'autenticazione per questo trust della relying party. Criteri di autenticazione globali rappresentano il fallback per i trust della relying party per applicazioni e servizi che non dispongono di un criterio specifico di autenticazione configurato. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Per configurare l'autenticazione principale a livello globale in Windows Server 2012 R2 

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  

2.  In ADFS blocca\-fare clic su **criteri di autenticazione**.  

3.  Nel **autenticazione primaria** fare clic su **modificare** accanto a **impostazioni globali**. È inoltre possibile fare\-fare clic su **criteri di autenticazione**, e selezionare **Modifica autenticazione primaria globale**, o, nel **azioni** selezionare **Modifica autenticazione primaria globale**.  
criteri di autenticazione ![](media/Configure-Authentication-Policies/authpolicy1.png)

4.  Nel **Modifica criteri di autenticazione globali** finestra via il **primario** scheda, è possibile configurare le impostazioni seguenti come parte di criteri di autenticazione globali:  

    -   Metodi di autenticazione da utilizzare per l'autenticazione principale. È possibile selezionare i metodi di autenticazione disponibili sotto il **Extranet** e **Intranet**.  

    -   L'autenticazione del dispositivo tramite il **abilitare l'autenticazione del dispositivo** casella di controllo. Per altre informazioni, vedere [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
criteri di autenticazione ![](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Per configurare l'autenticazione principale per ogni componente attendibilità  

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  

2.  In ADFS blocca\-fare clic su **criteri di autenticazione**\\**Per attendibilità**, quindi fare clic su trust della relying party per la quale si desidera configurare i criteri di autenticazione.  

3.  Direttamente\-fare clic su trust della relying party per la quale si desidera configurare i criteri di autenticazione e quindi selezionare **Modifica autenticazione primaria personalizzata**, o, sotto il **azioni** selezionare **Modifica autenticazione primaria personalizzata**.  
criteri di autenticazione ![](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  Nel **Modifica criteri di autenticazione per < relying\_entità\_trust\_nome >** finestra, sotto il **primario** scheda, è possibile configurare l'impostazione seguente come parte del **Per attendibilità** criteri di autenticazione:  

    -   Se gli utenti devono fornire le proprie credenziali ogni volta che al momento dell'accesso\-in tramite il **gli utenti devono fornire le proprie credenziali ogni volta che al momento dell'accesso\-in** casella di controllo.  
criteri di autenticazione ![](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Per configurare l'autenticazione a più fattori a livello globale  

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  

2.  In ADFS blocca\-fare clic su **criteri di autenticazione**.  

3.  Nel **più\-fattore di autenticazione** fare clic su **modificare** accanto a **impostazioni globali**. È inoltre possibile fare\-fare clic su **criteri di autenticazione**, e selezionare **modificare più globale\-fattore di autenticazione**, o, nel **azioni** selezionare **modificare più globale\-fattore di autenticazione**.  
criteri di autenticazione ![](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  Nel **Modifica criteri di autenticazione globali** finestra, sotto il **più\-factor** scheda è possibile configurare le impostazioni seguenti come parte di più globale\-fattore di criteri di autenticazione:  

    -   Le impostazioni o le condizioni per l'autenticazione a più Fattori tramite le opzioni disponibili nel **utenti\/gruppi**, **dispositivi**, e **percorsi** sezioni.  

    -   Per abilitare l'autenticazione a più Fattori per queste impostazioni, è necessario selezionare almeno un metodo di autenticazione aggiuntivo. **L'autenticazione del certificato** predefinito disponibile l'opzione. È inoltre possibile configurare altri metodi di autenticazione aggiuntivi personalizzati, ad esempio, Windows Azure Active Authentication. Per ulteriori informazioni, vedere [Guida allo scenario: gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  

> [!WARNING]  
> È possibile configurare solo metodi di autenticazione aggiuntivi a livello globale.  
criteri di autenticazione ![](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Per configurare più\-autenticazione fattori per ogni componente attendibilità  

1.  In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**.  

2.  In ADFS blocca\-fare clic su **criteri di autenticazione**\\**Per attendibilità**, quindi fare clic su trust della relying party per la quale si desidera configurare l'autenticazione a più Fattori.  

3.  Direttamente\-fare clic su trust della relying party per la quale si desidera configurare l'autenticazione a più Fattori e quindi selezionare **modifica personalizzata più\-fattore di autenticazione**, o, nel **azioni** selezionare **modifica personalizzata più\-fattore di autenticazione**.  

4.  Nel **Modifica criteri di autenticazione per < relying\_entità\_trust\_nome >** finestra, sotto il **più\-factor** scheda, è possibile configurare le impostazioni seguenti come parte del per\-relying party criteri di autenticazione:  

    -   Le impostazioni o le condizioni per l'autenticazione a più Fattori tramite le opzioni disponibili nel **utenti\/gruppi**, **dispositivi**, e **percorsi** sezioni.  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurare i criteri di autenticazione tramite Windows PowerShell  
Windows PowerShell consente maggiore flessibilità nell'utilizzo di vari fattori di controllo di accesso e il meccanismo di autenticazione disponibili in ADFS in Windows Server 2012 R2 per configurare i criteri di autenticazione e autorizzazione delle regole che sono necessari per implementare l'accesso condizionale true per ADFS \-risorse protette.  

Per eseguire queste procedure è necessaria almeno l'appartenenza al gruppo Administrators oppure a un gruppo equivalente nel computer locale.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Per configurare un metodo di autenticazione aggiuntivo tramite Windows PowerShell  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  


~~~
`Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `
~~~


> [!WARNING]  
> Per verificare la corretta esecuzione del comando, è possibile eseguire il comando `Get-AdfsGlobalAuthenticationPolicy` .  

### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Per configurare l'autenticazione a più fattori per\-relying party Trust basata sui dati di appartenenza a gruppi di un utente  

1.  Nel server federativo aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente:  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!WARNING]  
> Assicurarsi di sostituire *< relying\_entità\_trust >* con il nome del trust della relying party.  

2. Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  


~~~
$MfaClaimRule = "c:[Type == '"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value =~ '"^(?i) <group_SID>$'"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value '"https://schemas.microsoft.com/claims/multipleauthn'");" 

Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
~~~


> [!NOTE]  
> Assicurarsi di sostituire < gruppo\_SID > con il valore dell'identificatore di sicurezza \(SID\) di Active Directory \(AD\) gruppo.  

### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Per configurare l'autenticazione a più Fattori in base a livello globale su dati di appartenenza al gruppo degli utenti  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  


~~~
$MfaClaimRule = "c:[Type == '" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value == '"group_SID'"]  
 => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = '"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Assicurarsi di sostituire *< gruppo\_SID >* con il valore del SID del gruppo di Active Directory.  

### <a name="to-configure-mfa-globally-based-on-users-location"></a>Per configurare l'autenticazione a più fattori a livello globale in base alla posizione dell'utente  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  


~~~
$MfaClaimRule = "c:[Type == '" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '"true_or_false'"]  
 => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = '"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~



> [!NOTE]  
> Assicurarsi di sostituire *< true\_o\_false >* con `true` o `false`. Il valore dipende la condizione della regola specifica che è basata su se la richiesta di accesso proviene dall'extranet o intranet.  

### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Per configurare l'autenticazione a più fattori a livello globale in base ai dati del dispositivo dell'utente  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  


~~~
$MfaClaimRule = "c:[Type == '" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false"']  
 => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = '"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Assicurarsi di sostituire *< true\_o\_false >* con `true` o `false`. Il valore dipende la condizione della regola specifico che si basa sul fatto che il dispositivo sia all'area di lavoro\-associata o non.  

### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Per configurare autenticazione a più Fattori a livello globale, se la richiesta di accesso proviene dalla rete extranet e da un non\-all'area di lavoro\-dispositivo collegato  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  


~~~
`Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
~~~


> [!NOTE]  
> Assicurarsi di sostituire le istanze di *< true\_o\_false >* con `true` o `false`, che dipende da condizioni di regola specifica. Le condizioni della regola sono in base alla presenza del dispositivo all'area di lavoro\-unito o non e se la richiesta di accesso proviene dall'extranet o intranet.  

### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Per configurare autenticazione a più Fattori a livello globale, se l'accesso proviene da un utente extranet che appartiene a un determinato gruppo  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  


~~~
Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
~~~

> [!NOTE]  
> Assicurarsi di sostituire *< gruppo\_SID >* con il valore del SID di gruppo e *< true\_o\_false >* con `true` o `false`, quale dipende la condizione della regola specifica che è basata su se la richiesta di accesso proviene dall'extranet o intranet.  

### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Per concedere l'accesso a un'applicazione in base ai dati dell'utente tramite Windows PowerShell  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Assicurarsi di sostituire *< relying\_entità\_trust >* con il valore del trust della relying party.  

2. Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  

   ```  

     $GroupAuthzRule = "@RuleTemplate = `"Authorization`" @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
   Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
   ```  

> [!NOTE]  
> > Assicurarsi di sostituire *< gruppo\_SID >* con il valore del SID del gruppo di Active Directory.  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Per concedere l'accesso a un'applicazione protetta da AD FS solo se l'identità dell'utente è stata convalidata con l'autenticazione a più fattori  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Assicurarsi di sostituire *< relying\_entità\_trust >* con il valore del trust della relying party.  

2. Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  

   ```  
   $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
   @RuleName = `"PermitAccessWithMFA`"  
   c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = '"PermitUsersWithClaim'");"  

   ```  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Per concedere l'accesso a un'applicazione protetta da solo se ADFS l'accesso richiesta proviene da un luogo di lavoro\-dispositivo unito registrato con l'utente  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Assicurarsi di sostituire *< relying\_entità\_trust >* con il valore del trust della relying party.  

2. Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
~~~



### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Per concedere l'accesso a un'applicazione protetta da solo se ADFS l'accesso richiesta proviene da un luogo di lavoro\-dispositivo unito è registrato a un utente la cui identità viene convalidata con autenticazione a più Fattori  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Assicurarsi di sostituire *< relying\_entità\_trust >* con il valore del trust della relying party.  

2. Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  

   ```  
   $GroupAuthzRule = '@RuleTemplate = "Authorization"  
   @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"  
   c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
   c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

   ```  

### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Per concedere l'accesso extranet a un'applicazione protetta da ADFS solo se la richiesta di accesso proviene da un utente la cui identità viene convalidata con autenticazione a più Fattori  

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!NOTE]  
> Assicurarsi di sostituire *< relying\_entità\_trust >* con il valore del trust della relying party.  

2. Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"RequireMFAForExtranetAccess`"  
c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
~~~

## <a name="additional-references"></a>Altri riferimenti  

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
