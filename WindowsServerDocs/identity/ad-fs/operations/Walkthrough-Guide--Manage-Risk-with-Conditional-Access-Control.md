---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: Guida dettagliata-gestire i rischi con il controllo degli accessi condizionali
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: aefcd597a580de526a758c6d026c6c91d02d10c8
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371663"
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>Guida allo scenario: Gestire i rischi con il controllo di accesso condizionale




## <a name="about-this-guide"></a>Informazioni sulla guida
Questa procedura dettagliata fornisce istruzioni per la gestione dei rischi con uno dei fattori (dati utente) disponibili tramite il meccanismo di controllo di accesso condizionale in Active Directory Federation Services (AD FS) in Windows Server 2012 R2. Per altre informazioni sui meccanismi di autorizzazione e controllo di accesso condizionale in AD FS in Windows Server 2012 R2, vedere [gestire i rischi con il controllo di accesso condizionale](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md).

Questo scenario è suddiviso nelle sezioni seguenti:

-   [Passaggio 1: configurazione dell'ambiente lab](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Passaggio 2: verificare il meccanismo di controllo di accesso AD FS predefinito](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [Passaggio 3: configurare i criteri di controllo degli accessi condizionali in base ai dati utente](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [Passaggio 4: verificare il meccanismo di controllo di accesso condizionale](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>Passaggio 1: configurazione dell'ambiente lab
Per eseguire le procedure descritte in questo scenario, è necessario un ambiente con i componenti seguenti:

-   Un dominio Active Directory con account utente e di gruppo di test, in esecuzione in Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 con lo schema aggiornato a Windows Server 2012 R2 o un dominio di Active Directory in esecuzione in Windows Server 2012 R2

-   Server federativo in esecuzione in Windows Server 2012 R2

-   Un server Web che ospita l'applicazione di esempio

-   Un computer client da cui è possibile accedere all'applicazione di esempio

> [!WARNING]
> Sia negli ambienti di produzione che in quelli di testing è consigliabile evitare di utilizzare lo stesso computer come server federativo e come server Web.

In questo ambiente, il server federativo rilascia le attestazioni richieste affinché gli utenti possano accedere all'applicazione di esempio. Il server Web ospita un'applicazione di esempio che considererà attendibili gli utenti che presentano le attestazioni rilasciate dal server federativo.

Per istruzioni su come configurare questo ambiente, vedere [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Passaggio 2: verificare il meccanismo di controllo di accesso AD FS predefinito
In questo passaggio verrà verificato il meccanismo di controllo degli accessi predefinito di ADFS, in base al quale l'utente viene reindirizzato alla pagina di accesso ad ADFS, specifica le credenziali valide e ottiene l'accesso all'applicazione. È possibile usare l'account di Active Directory **Robert Hatley** e l'applicazione di esempio **ClaimApp** configurata in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>Per verificare il meccanismo di controllo degli accessi predefinito di ADFS

1.  Nel computer client aprire una finestra del browser e passare all'applicazione di esempio: **https://webserv1.contoso.com/claimapp** .

    Questa azione reindirizza automaticamente la richiesta al server federativo e viene richiesto di eseguire l'accesso specificando nome utente e password.

2.  Digitare le credenziali dell'account di Active Directory **Robert Hatley** creato in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Verrà concesso l'accesso all'applicazione.

## <a name="BKMK_3"></a>Passaggio 3: configurare i criteri di controllo degli accessi condizionali in base ai dati utente
In questo passaggio verranno configurati i criteri di controllo degli accessi in base ai dati sull'appartenenza a gruppi dell'utente. In altri termini, verrà configurata una **regola di autorizzazione di rilascio** nel server federativo per un trust della relying party che rappresenta l'applicazione di esempio - **claimapp**. In base alla logica di questa regola, all'utente di Active Directory **Robert Hatley** verranno rilasciate le attestazioni necessarie per accedere all'applicazione perché appartiene a un gruppo **Finance** . L'account **Robert Hatley** è stato aggiunto al gruppo **Finance** in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

È possibile eseguire questa attività tramite la console di gestione di ADFS o con Windows PowerShell.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>Per configurare i criteri di controllo di accesso condizionale in base ai dati utente tramite la console di gestione AD FS

1.  Nella console di gestione di ADFS passare a **Relazioni di attendibilità**, e quindi a **Attendibilità componente**.

2.  Selezionare un trust della relying party che rappresenta l'applicazione di esempio (**claimapp**), quindi selezionare **Modifica regole attestazione** nel riquadro **Azioni** o facendo clic con il pulsante destro del mouse su questo trust della replying party.

3.  Nella finestra **Modifica regole attestazione per claimapp** selezionare la scheda **Regole di autorizzazione rilascio** e fare clic su **Aggiungi regola**.

4.  Nella pagina **Seleziona modello di regola** dell'**Aggiunta guidata regole attestazione di autorizzazione di rilascio** selezionare il modello di regola attestazione **Consentire o negare l'accesso agli utenti in base a un'attestazione in ingresso** e quindi fare clic su **Avanti**.

5.  Nella pagina **Configura regola** eseguire tutte le operazioni seguenti e quindi fare clic su **Fine**:

    1.  Immettere un nome per la regola attestazioni, ad esempio **TestRule**.

    2.  Selezionare **SID gruppo** come **Tipo di attestazione in ingresso**.

    3.  Fare clic su **Sfoglia**, digitare in **Finance** il nome del gruppo di test di Active Directory, quindi risolverlo per il campo **Valore Attestazione in ingresso**.

    4.  Selezionare l'opzione **Nega accesso agli utenti con questa attestazione in ingresso**.

6.  Nella finestra **Modifica regole attestazione per claimapp** assicurarsi di eliminare la regola **Consenti accesso a tutti gli utenti** creata per impostazione predefinita al momento della creazione di questa attendibilità del componente.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>Per configurare criteri di controllo di accesso condizionale in base ai dati utente tramite Windows PowerShell

1.  Nel server federativo aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente:


~~~
`$rp = Get-AdfsRelyingPartyTrust -Name claimapp`
~~~


2. Nella stessa finestra di comando di Windows PowerShell eseguire il seguente comando:


~~~
`$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`
~~~

> [!NOTE]
> Assicurarsi di sostituire <group_SID> con il valore del SID del gruppo **Finance** di Active Directory.

## <a name="BKMK_4"></a>Passaggio 4: verificare il meccanismo di controllo di accesso condizionale
In questo passaggio verranno verificati i criteri di controllo di accesso condizionale impostati nel passaggio precedente. È possibile usare la procedura seguente per verificare che l'utente di Active Directory **Robert Hatley** possa accedere all'applicazione di esempio perché appartiene al gruppo **Finance** e che gli utenti di Active Directory che non appartengono al gruppo **Finance** non possano accedere all'applicazione di esempio.

1.  Nel computer client aprire una finestra del browser e passare all'applicazione di esempio: **https://webserv1.contoso.com/claimapp**

    Questa azione reindirizza automaticamente la richiesta al server federativo e viene richiesto di eseguire l'accesso specificando nome utente e password.

2.  Digitare le credenziali dell'account di Active Directory **Robert Hatley** creato in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Verrà concesso l'accesso all'applicazione.

3.  Digitare le credenziali di un altro utente di Active Directory che NON appartiene al gruppo **Finance**. Per ulteriori informazioni su come creare gli account utente in Active Directory, vedere [https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

    A questo punto, a causa dei criteri di controllo di accesso configurati nel passaggio precedente, viene visualizzato un messaggio di accesso negato per questo utente di Active Directory che non appartiene al gruppo **Finance** . Il testo del messaggio predefinito è **non si dispone dell'autorizzazione per accedere al sito. Fare clic qui per disconnettersi e accedere di nuovo o contattare l'amministratore per le autorizzazioni.** . Tuttavia, questo testo è completamente personalizzabile. Per altre informazioni su come personalizzare l'esperienza di accesso, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="see-also"></a>Vedi anche
[Gestire i rischi con il controllo degli accessi condizionali](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



