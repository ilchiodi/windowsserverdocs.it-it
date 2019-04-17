---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: Guida dettagliata - gestire i rischi con il controllo di accesso condizionale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11d2d567f9264dca53a3426263a172649d7d7c11
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>Guida allo scenario: Gestire i rischi con il controllo di accesso condizionale

>Si applica a: Windows Server 2012 R2


## <a name="about-this-guide"></a>Informazioni sulla Guida
Questa procedura dettagliata vengono fornite istruzioni per la gestione dei rischi con uno dei fattori (dati dell'utente) disponibili tramite il meccanismo di controllo di accesso condizionale in Active Directory Federation Services (ADFS) in Windows Server 2012 R2. Per ulteriori informazioni sui meccanismi di autorizzazione e controllo di accesso condizionale in ADFS in Windows Server 2012 R2, vedere [Manage Risk with Conditional Access Control](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md).

Questo scenario è suddiviso nelle sezioni seguenti:

-   [Passaggio 1: Configurazione dell'ambiente di testing](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Passaggio 2: Verificare il meccanismo di controllo di accesso predefinito di ADFS](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [Passaggio 3: Configurare criteri di controllo di accesso condizionale in base ai dati utente](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [Passaggio 4: Verificare il meccanismo di controllo di accesso condizionale](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>Passaggio 1: Configurazione dell'ambiente di testing
Per completare questa procedura dettagliata, è necessario un ambiente che include i componenti seguenti:

-   Un dominio di Active Directory con un utente di prova e account di gruppo in esecuzione in Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 con lo schema aggiornato a Windows Server 2012 R2 o un dominio di Active Directory in esecuzione in Windows Server 2012 R2

-   Un server federativo in esecuzione su Windows Server 2012 R2

-   Un server web che ospita l'applicazione di esempio

-   Un computer client da cui è possibile accedere all'applicazione di esempio

> [!WARNING]
> Si consiglia di (entrambi in ambienti di produzione o test) che non si utilizza lo stesso computer come server federativo e il server web.

In questo ambiente, il server federativo rilascia le attestazioni che sono necessari in modo che gli utenti possono accedere all'applicazione di esempio. Il server Web ospita un'applicazione di esempio che considererà attendibili gli utenti che presentano le attestazioni rilasciate dal server federativo.

Per istruzioni su come configurare questo ambiente, vedere [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Passaggio 2: Verificare il meccanismo di controllo di accesso predefinito di ADFS
In questo passaggio verrà verificato il valore predefinito meccanismo di controllo di accesso AD FS, in cui l'utente viene reindirizzato alla pagina di accesso di ADFS, specifica le credenziali valide e viene concesso l'accesso all'applicazione. È possibile utilizzare il **Robert Hatley** account di Active Directory e **claimapp** configurata nell'applicazione di esempio [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>Per verificare l'impostazione predefinita, ADFS accedere meccanismo di controllo

1.  Nel computer client, aprire una finestra del browser e passare all'applicazione di esempio: **https://webserv1.contoso.com/claimapp**.

    Questa azione reindirizza automaticamente la richiesta al server federativo e viene richiesto di accedere con un nome utente e password.

2.  Digitare le credenziali del **Robert Hatley** account di Active Directory creato in [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Si verrà concesso l'accesso all'applicazione.

## <a name="BKMK_3"></a>Passaggio 3: Configurare criteri di controllo di accesso condizionale in base ai dati utente
In questo passaggio si imposterà un criterio di controllo di accesso in base ai dati di appartenenza al gruppo utente. In altre parole, si configurerà un **regola di autorizzazione rilascio** nel server federativo per un trust della relying party che rappresenta l'applicazione di esempio - **claimapp**. Logica di questa regola, **Robert Hatley** utente di Active Directory verrà rilasciate le attestazioni necessarie per accedere a questa applicazione perché appartiene a un **Finance** gruppo. È stato aggiunto il **Robert Hatley** account per il **Finance** gruppo [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

È possibile completare questa attività utilizzando una Console di gestione di ADFS o tramite Windows PowerShell.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>Per configurare criteri di controllo di accesso condizionale in base ai dati utente tramite la Console di gestione ADFS

1.  Nella Console di gestione di ADFS, passare a **relazioni di Trust**e quindi **attendibilità**.

2.  Selezionare il trust della relying party che rappresenta l'applicazione di esempio (**claimapp**), quindi selezionare il **azioni** riquadro o facendo clic con il trust della relying party, selezionare **Modifica regole attestazione**.

3.  Nel **Modifica regole attestazione per claimapp** finestra, seleziona **regole di autorizzazione rilascio** scheda e fare clic su **Aggiungi regola**.

4.  Nel **Aggiunta guidata autorizzazione di rilascio attestazione regola**via il **pagina Seleziona modello di regola**, selezionare **consentire o negare agli utenti in base a un'attestazione in ingresso** modello di regola attestazione e quindi fare clic su **Avanti**.

5.  Nel **configurare la regola** pagina, eseguire le operazioni seguenti e quindi fare clic su **fine**:

    1.  Immettere un nome per la regola attestazione, ad esempio **TestRule**.

    2.  Selezionare **SID di gruppo** come **tipo di attestazione in ingresso**.

    3.  Fare clic su **Sfoglia**, digitare **Finance** gruppo di test per il nome di Active Directory e risolverlo per il **valore attestazione in ingresso** campo.

    4.  Selezionare il **negare l'accesso agli utenti con questa attestazione in ingresso** opzione.

6.  Nel **Modifica regole attestazione per claimapp** finestra, assicurarsi di eliminare il **Consenti accesso a tutti gli utenti** regola creata per impostazione predefinita, quando si crea questo trust della relying party.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>Per configurare criteri di controllo di accesso condizionale in base ai dati utente tramite Windows PowerShell

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente:


    `$rp = Get-AdfsRelyingPartyTrust -Name claimapp`


2.  Nella stessa finestra di comando Windows PowerShell, eseguire il comando seguente:


    `$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`

> [!NOTE]
> Assicurarsi di sostituire < group_SID > con il valore del SID di Active Directory **Finance** gruppo.

## <a name="BKMK_4"></a>Passaggio 4: Verificare il meccanismo di controllo di accesso condizionale
In questo passaggio è necessario verificare i criteri di controllo di accesso condizionale impostati nel passaggio precedente. È possibile utilizzare la procedura seguente per verificare che **Robert Hatley** utente di Active Directory possa accedere all'applicazione di esempio perché appartiene al **Finance** gruppo e gli utenti di Active Directory che non appartengono al **Finance** gruppo non può accedere all'applicazione di esempio.

1.  Nel computer client, aprire una finestra del browser e passare all'applicazione di esempio: **https://webserv1.contoso.com/claimapp**

    Questa azione reindirizza automaticamente la richiesta al server federativo e viene richiesto di accedere con un nome utente e password.

2.  Digitare le credenziali del **Robert Hatley** account di Active Directory creato in [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Si verrà concesso l'accesso all'applicazione.

3.  Digitare le credenziali di un altro utente di Active Directory che non appartengono al **Finance** gruppo. (Per ulteriori informazioni su come creare gli account utente in Active Directory, vedere [https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

    A questo punto, tenendo conto dei criteri di controllo di accesso è configurato nel passaggio precedente, viene visualizzato un messaggio "accesso negato" per questo utente di Active Directory che non appartengono al **Finance** gruppo. Il testo del messaggio predefinito è **non dispone dell'autorizzazione per accedere al sito. Fare clic qui per disconnettersi e accedere di nuovo o contattare l'amministratore per le autorizzazioni.** Tuttavia, questo testo è completamente personalizzabile. Per ulteriori informazioni su come personalizzare l'esperienza di accesso, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="see-also"></a>Vedere anche
[Gestire i rischi con il controllo di accesso condizionale](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



