---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: Configurare criteri di autenticazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7faffb7ccbb4b0ea3c65329d18f915d7dafcd46f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configure-authentication-policies"></a>Configurare criteri di autenticazione

>Si applica a: Windows Server 2012 R2

In ADFS, in Windows Server 2012 R2, entrambi controllo di accesso e il meccanismo di autenticazione sono stati migliorati con più fattori che includono dati utente, dispositivo, posizione e autenticazione. Questi miglioramenti consentono, tramite l'interfaccia utente o tramite Windows PowerShell, per gestire il rischio di concessione di autorizzazioni di accesso per applicazioni AD FS\ protette tramite il controllo di accesso più fattori e l'autenticazione a più fattori che sono basate su utente identità o appartenenza al gruppo, percorso di rete, i dati di dispositivo che viene aggiunto a workplace\, e lo stato di autenticazione quando l'autenticazione a più fattori \(MFA\) è stata eseguita.  
  
Per ulteriori informazioni su controllo degli accessi a più fattori e a più fattori in Active Directory Federation Services \(AD FS\) in Windows Server 2012 R2, vedere gli argomenti seguenti:  


-   [Accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Gestire i rischi con il controllo di accesso condizionale](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurare i criteri di autenticazione tramite snap-in di gestione di ADFS  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo per completare queste procedure.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
In ADFS, in Windows Server 2012 R2, è possibile specificare un criterio di autenticazione in ambito globale applicabili a tutte le applicazioni e servizi protetti da ADFS. È inoltre possibile impostare criteri di autenticazione per applicazioni specifiche e i servizi che si basano su attendibilità e sono protetti da ADFS. Se si specifica un criterio di autenticazione per una particolare applicazione per ogni componente attendibilità non esegue l'override di criteri di autenticazione globali. Se globale o per ogni componente attendibilità criterio di autenticazione richiede l'autenticazione a più fattori, autenticazione a più fattori viene attivata quando l'utente tenta l'autenticazione per questo trust della relying party. I criteri di autenticazione globali sono rappresentano il fallback per i trust della relying party per applicazioni e servizi che non dispongono di un criterio specifico di autenticazione configurato. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Per configurare l'autenticazione principale a livello globale in Windows Server 2012 R2 
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  In ADFS snap-in, fare clic su **criteri di autenticazione**.  
  
3.  Nel **autenticazione primaria** fare clic su **modifica** accanto a **impostazioni globali**. È inoltre possibile fare clic con **criteri di autenticazione**e seleziona **Modifica autenticazione primaria globale**, o, nel **azioni** selezionare **Modifica autenticazione primaria globale**.  
![criteri di autenticazione](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  Nel **Modifica criteri di autenticazione globali** finestra via il **primario** scheda, è possibile configurare le impostazioni seguenti come parte di criteri di autenticazione globali:  
  
    -   Metodi di autenticazione da utilizzare per l'autenticazione principale. È possibile selezionare i metodi di autenticazione disponibili sotto il **Extranet** e **Intranet**.  
  
    -   Autenticazione del dispositivo tramite il **abilitare l'autenticazione dispositivo** casella di controllo. Per ulteriori informazioni, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![criteri di autenticazione](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Per configurare l'autenticazione principale per ogni componente attendibilità  
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  In ADFS snap-in, fare clic su **criteri di autenticazione**\\**Per attendibilità**, quindi fare clic su trust della relying party per cui si desidera configurare i criteri di autenticazione.  
  
3.  Con-fare clic su trust di relying party per cui si desidera configurare criteri di autenticazione e quindi seleziona **Modifica autenticazione primaria personalizzata**, o, nel **azioni** selezionare **Modifica autenticazione primaria personalizzata**.  
![criteri di autenticazione](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  Nel **Modifica criteri di autenticazione per < relying\_party\_trust\_name >** finestra, nel **primario** scheda, è possibile configurare l'impostazione seguente come parte del **per ogni Trust della Relying Party** criteri di autenticazione:  
  
    -   Indica se gli utenti vengono richiesto di fornire le proprie credenziali ogni volta che al momento dell'accesso tramite il **agli utenti verrà richiesto di fornire le proprie credenziali ogni volta che al momento dell'accesso** casella di controllo.  
![criteri di autenticazione](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Per configurare l'autenticazione a più fattori a livello globale  
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  In ADFS snap-in, fare clic su **criteri di autenticazione**.  
  
3.  Nel **l'autenticazione a più fattori** fare clic su **modifica** accanto a **impostazioni globali**. È inoltre possibile fare clic con **criteri di autenticazione**e seleziona **globale Modifica autenticazione a più fattori**, o, nel **azioni** selezionare **globale Modifica autenticazione a più fattori**.  
![criteri di autenticazione](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  Nel **Modifica criteri di autenticazione globali** finestra, sotto il **più fattori** scheda è possibile configurare le impostazioni seguenti come parte dei criteri di autenticazione globali più fattori:  
  
    -   Le impostazioni o le condizioni per l'autenticazione a più fattori tramite le opzioni disponibili di **MSOCache\All Users \ / gruppi**, **dispositivi**, e **percorsi** sezioni.  
  
    -   Per abilitare l'autenticazione a più fattori per queste impostazioni, è necessario selezionare almeno un metodo di autenticazione aggiuntivo. **L'autenticazione del certificato** predefinito disponibile l'opzione. È inoltre possibile configurare altri metodi di autenticazione aggiuntivi personalizzati, ad esempio, Windows Azure Active Authentication. Per ulteriori informazioni, vedere [Guida allo scenario: gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
> [!WARNING]  
> È possibile configurare metodi di autenticazione aggiuntivi solo a livello globale.  
![criteri di autenticazione](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Per configurare l'autenticazione a più fattori per ogni componente attendibilità  
  
1.  In Server Manager, fare clic su **strumenti**e quindi seleziona **gestione di ADFS **.  
  
2.  In ADFS snap-in, fare clic su **criteri di autenticazione**\\**Per attendibilità**, quindi fare clic su trust della relying party per cui si desidera configurare l'autenticazione a più fattori.  
  
3.  Con-fare clic su trust di relying party per cui si desidera configurare l'autenticazione a più fattori e quindi seleziona **l'autenticazione a più fattori personalizzata modifica**, o, nel **azioni** selezionare **l'autenticazione a più fattori personalizzata modifica**.  
  
4.  Nel **Modifica criteri di autenticazione per < relying\_party\_trust\_name >** finestra, nel **più fattori** scheda, è possibile configurare le impostazioni seguenti come parte di criteri di autenticazione di attendibilità il componente per\:  
  
    -   Le impostazioni o le condizioni per l'autenticazione a più fattori tramite le opzioni disponibili di **MSOCache\All Users \ / gruppi**, **dispositivi**, e **percorsi** sezioni.  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurare i criteri di autenticazione tramite Windows PowerShell  
Windows PowerShell consente maggiore flessibilità nell'utilizzo di vari fattori di controllo di accesso e il meccanismo di autenticazione disponibili in ADFS in Windows Server 2012 R2 per configurare criteri di autenticazione e l'autorizzazione delle regole che sono necessari per implementare l'accesso condizionale true per le risorse \-secured AD FS.  
  
Appartenenza al gruppo Administrators, o equivalente, nel computer locale è il requisito minimo per completare queste procedure.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Per configurare un metodo di autenticazione aggiuntivo tramite Windows PowerShell  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> Per verificare che questo comando è stato eseguito correttamente, è possibile eseguire il `Get-AdfsGlobalAuthenticationPolicy` comando.  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Per configurare l'autenticazione a più fattori per\ del componente attendibilità basato su dati di appartenenza al gruppo dell'utente  
  
1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente:  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> Assicurarsi di sostituire *< relying\_party\_trust >* con il nome del trust della relying party.  
  
2.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  
  
 
    $MfaClaimRule = "c: [tipo = = '" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", valore = ~ '" ^(?i) < group_SID >$ ' "] = > problema (tipo = '" https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valore '" https://schemas.microsoft.com/claims/multipleauthn'");" 
    
    Set-AdfsRelyingPartyTrust – TargetRelyingParty $rp – AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> Assicurarsi di sostituire < group\_SID > con il valore del SID del gruppo di Active Directory \(AD\) \(SID\).  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Per configurare l'autenticazione a più fattori a livello globale in base ai dati di appartenenza al gruppo degli utenti  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  

    $MfaClaimRule = "c: [tipo = = '" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", valore = = '" group_SID'"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valore = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
      
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Assicurarsi di sostituire *< group\_SID >* con il valore del SID del gruppo di Active Directory.  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>Per configurare l'autenticazione a più fattori a livello globale in base alla posizione dell'utente  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  
 
    $MfaClaimRule = "c: [tipo = = '" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", valore = = '" true_or_false'"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valore = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> Assicurarsi di sostituire *< true\_or\_false >* con `true` o `false`. Il valore dipende la condizione della regola specifica che si basa su se la richiesta di accesso proviene dall'extranet o intranet.  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Per configurare l'autenticazione a più fattori a livello globale in base ai dati di dispositivo dell'utente  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  

    $MfaClaimRule = "c: [tipo = = '" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", valore = =""true_or_false"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valore = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Assicurarsi di sostituire *< true\_or\_false >* con `true` o `false`. Il valore dipende la condizione della regola specifica che si basa sul fatto che il dispositivo è aggiunto al workplace\ o non.  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Per configurare autenticazione a più fattori a livello globale, se la richiesta di accesso proviene dall'extranet e da un dispositivo workplace\ appartenenti a un  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> Assicurarsi di sostituire le istanze di *< true\_or\_false >* con `true` o `false`, che dipende da condizioni di regola specifica. Le condizioni della regola sono basate su se il dispositivo è aggiunto workplace\ o non e se la richiesta di accesso proviene dall'extranet o intranet.  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Per configurare autenticazione a più fattori a livello globale, se l'accesso proviene da un utente extranet che appartiene a un determinato gruppo  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  

    Set-AdfsAdditionalAuthenticationRule "c: [tipo = = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", valore = = `"group_SID`"] & & c2: [tipo = = `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", valore = = `"true_or_false`"] = > problema (tipo = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", valore ='"https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> Assicurarsi di sostituire *< group\_SID >* con il valore del SID di gruppo e *< true\_or\_false >* con `true` o `false`, quale dipende la condizione della regola specifica che si basa su se la richiesta di accesso proviene dall'extranet o intranet.  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Per concedere l'accesso a un'applicazione in base ai dati utente tramite Windows PowerShell  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Assicurarsi di sostituire *< relying\_party\_trust >* con il valore del trust della relying party.  
  
2.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > Assicurarsi di sostituire *< group\_SID >* con il valore del SID del gruppo di Active Directory.  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Per concedere l'accesso a un'applicazione protetta da solo se ADFS l'identità dell'utente viene convalidata con autenticazione a più fattori  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Assicurarsi di sostituire *< relying\_party\_trust >* con il valore del trust della relying party.  
  
2.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Per concedere l'accesso a un'applicazione protetta da solo se ADFS l'accesso richiesta proviene da un dispositivo appartenente a un workplace\ registrato per l'utente  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Assicurarsi di sostituire *< relying\_party\_trust >* con il valore del trust della relying party.  
  
2.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c: [tipo = = `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", valore = ~ `"^(?i)true$`"] = > problema (tipo = `"https://schemas.microsoft.com/authorization/claims/permit`", valore = `"PermitUsersWithClaim`");  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Per concedere l'accesso a un'applicazione protetta da ADFS solo se la richiesta di accesso proviene da un dispositivo appartenente a un workplace\ registrato a un utente la cui identità viene convalidata con autenticazione a più fattori  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Assicurarsi di sostituire *< relying\_party\_trust >* con il valore del trust della relying party.  
  
2.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Per concedere l'accesso extranet a un'applicazione protetta da ADFS solo se la richiesta di accesso proviene da un utente la cui identità viene convalidata con autenticazione a più fattori  
  
1.  Nel server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> Assicurarsi di sostituire *< relying\_party\_trust >* con il valore del trust della relying party.  
  
2.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    C1: [tipo = = `"https://schemas.microsoft.com/claims/authnmethodsreferences`", valore = ~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] & &  
    C2: [tipo = = `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", valore = ~ `"^(?i)false$`"] = > problema (tipo = `"https://schemas.microsoft.com/authorization/claims/permit`", valore = `"PermitUsersWithClaim`"); "  

## <a name="additional-references"></a>Riferimenti aggiuntivi  

[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md)
