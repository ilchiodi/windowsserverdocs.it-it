---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: "Guida dettagliata - gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 414f37e86f0072863e5fa2f107c39e5518e560ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili

>Si applica a: Windows Server 2012 R2


## <a name="about-this-guide"></a>Informazioni sulla Guida
Questa procedura dettagliata vengono fornite istruzioni per la configurazione di autenticazione a più fattori (MFA) in Active Directory Federation Services (ADFS) in Windows Server 2012 R2 in base ai dati di appartenenza al gruppo dell'utente.

Per ulteriori informazioni sui meccanismi di autenticazione a più fattori e l'autenticazione in ADFS, vedere [gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

Questo scenario è suddiviso nelle sezioni seguenti:

-   [Passaggio 1: Configurazione dell'ambiente di testing](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Passaggio 2: Verificare il meccanismo di autenticazione AD FS predefinito](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [Passaggio 3: Configurare l'autenticazione a più fattori nel server federativo](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [Passaggio 4: Verificare il meccanismo di autenticazione a più fattori](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>Passaggio 1: Configurazione dell'ambiente di testing
Per completare questa procedura dettagliata, è necessario un ambiente che include i componenti seguenti:

-   Un dominio di Active Directory con un utente di prova e account di gruppo in esecuzione in Windows Server 2012 R2 o un dominio di Active Directory in esecuzione in Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 con lo schema aggiornato a Windows Server 2012 R2

-   Un server federativo in esecuzione su Windows Server 2012 R2

-   Un server web che ospita l'applicazione di esempio

-   Un computer client da cui è possibile accedere all'applicazione di esempio

> [!WARNING]
> Si consiglia di (entrambi in ambienti di produzione e test) che non si utilizza lo stesso computer come server federativo e il server web.

In questo ambiente, il server federativo rilascia le attestazioni che sono necessari in modo che gli utenti possono accedere all'applicazione di esempio. Il server Web ospita un'applicazione di esempio che considererà attendibili gli utenti che presentano le attestazioni rilasciate dal server federativo.

Per istruzioni su come configurare questo ambiente, vedere [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Passaggio 2: Verificare il meccanismo di autenticazione AD FS predefinito
In questo passaggio verrà verificato il meccanismo di controllo di accesso predefinito di ADFS (**autenticazione basata su form** per extranet e **l'autenticazione di Windows** per la rete intranet), in cui l'utente viene reindirizzato alla pagina di accesso AD ADFS, specifica le credenziali valide e viene concesso l'accesso all'applicazione. È possibile utilizzare il **Robert Hatley** account di Active Directory e **claimapp** configurata nell'applicazione di esempio [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

1.  Nel computer client, aprire una finestra del browser e passare all'applicazione di esempio: **https://webserv1.contoso.com/claimapp**.

    Questa azione reindirizza automaticamente la richiesta al server federativo e viene richiesto di accedere con un nome utente e password.

2.  Digitare le credenziali del **Robert Hatley** account di Active Directory creato in [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Si verrà concesso l'accesso all'applicazione.

## <a name="BKMK_3"></a>Passaggio 3: Configurare l'autenticazione a più fattori nel server federativo
Esistono due parti per configurare autenticazione a più fattori in ADFS in Windows Server 2012 R2:

-   [Selezionare un metodo di autenticazione aggiuntivo](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [Impostare i criteri di autenticazione a più fattori](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>Selezionare un metodo di autenticazione aggiuntivo
Per configurare l'autenticazione a più fattori, è necessario selezionare un metodo di autenticazione aggiuntivo. In questa procedura dettagliata per il metodo di autenticazione aggiuntivi, è possibile scegliere tra le opzioni seguenti:

-   Selezionare [l'autenticazione del certificato](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7) metodo che è disponibile in ADFS in Windows Server 2012 R2 per impostazione predefinita

-   Configurare e selezionare [Windows Azure multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>Autenticazione del certificato
Eseguire una delle procedure seguenti per selezionare l'autenticazione del certificato come metodo di autenticazione aggiuntivo:

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>Per configurare l'autenticazione del certificato come metodo aggiuntivo tramite la Console di gestione ADFS

1.  Nel server federativo, nella Console di gestione di ADFS, passare al **criteri di autenticazione** nodo e in **multi-factor Authentication** fare clic su di **modifica** collegamento accanto al **impostazioni globali** sottosezione.

2.  Nel **Modifica criteri di autenticazione globali** finestra, seleziona **l'autenticazione del certificato** come metodo di autenticazione aggiuntivo e quindi fare clic su **OK**.

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>Per configurare l'autenticazione del certificato come metodo aggiuntivo tramite Windows PowerShell

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente:

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > Per verificare che questo comando è stato eseguito correttamente, è possibile eseguire il `Get-AdfsGlobalAuthenticationPolicy` comando.

#### <a name="BKMK_8"></a>Windows Azure multi-Factor Authentication
Completare le procedure seguenti per scaricare e configurare e selezionare **Windows Azure multi-Factor Authentication** come l'autenticazione aggiuntiva nel server federativo:

1.  [Creare un Provider multi-Factor Authentication tramite Windows portale di Azure](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [Scaricare il Server Windows Azure multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [Installare il Server Windows Azure multi-Factor Authentication nel Server federativo](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [Configurare Windows Azure multi-Factor Authentication come metodo di autenticazione aggiuntivi](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>Creare un Provider multi-Factor Authentication tramite Windows portale di Azure

1.  Accedere a Windows Azure portale come amministratore.

2.  A sinistra, selezionare Active Directory.

3.  Nella pagina Active Directory, nella parte superiore, selezionare **multi-Factor Authentication provider**.  Quindi, nella parte inferiore, fare clic su **New**.

4.  In **servizi App -> Active Directory**selezionare **Provider multi-Factor Authentication**e seleziona **creazione rapida**.

5.  In **servizi App**selezionare **provider di autenticazione attivi**e seleziona **creazione rapida**.

6.  Compilare i campi seguenti e selezionare **crea**.

    1.  **Nome** -il nome del Provider multi-Factor Authentication.

    2.  **Modello di utilizzo** -modello di utilizzo del Provider di autenticazione a più fattori.

        -   **Per autenticazione** -modello di acquisto con addebito per ogni autenticazione. Utilizzato generalmente per gli scenari che utilizzano Windows Azure multi-Factor Authentication in un'applicazione per consumatori.

        -   **Per ogni utente abilitato** -modello di acquisto con addebito per ogni utente abilitato.  In genere utilizzato per gli scenari esposti ai dipendenti, ad esempio Office 365.

        Per ulteriori informazioni sui modelli di utilizzo, vedere [dettagli dei prezzi di Windows Azure](http://www.windowsazure.com/pricing/details/active-directory/).

    3.  **Directory** -tenant di Windows Azure Active Directory che è associato il Provider multi-Factor Authentication. Questa opzione è facoltativa perché il provider non deve essere collegato a Windows Azure Active Directory quando la protezione di applicazioni locali.

7.  Si fa clic su Crea, verrà creato il Provider multi-Factor Authentication e verrà visualizzato un messaggio indicante: creazione completata Provider multi-Factor Authentication.  Fare clic su **Ok**.

Successivamente, è necessario scaricare il Server Windows Azure multi-Factor Authentication. Per eseguire questa operazione avviando il portale Windows Azure multi-Factor Authentication tramite il portale Windows Azure.

##### <a name="BKMK_b"></a>Scaricare il Server Windows Azure multi-Factor Authentication

1.  Accedere al portale di Windows Azure come amministratore e fare clic sul Provider di autenticazione a più fattori creato nella procedura precedente. Fare clic su di **Gestisci** pulsante.

    Verrà avviata la **Windows Azure multi-Factor Authentication** portale.

2.  Nel **Windows Azure multi-Factor Authentication** portale, fare clic su **download**, quindi fare clic su **scaricare** per scaricare una copia di Server Windows Azure multi-Factor Authentication.

Dopo aver scaricato l'eseguibile per il Server Windows Azure multi-Factor Authentication, è necessario installarlo nel server federativo.

##### <a name="BKMK_c"></a>Installare il Server Windows Azure multi-Factor Authentication nel Server federativo

1.  Scaricare e fare doppio clic sul file eseguibile per il Server Windows Azure multi-Factor Authentication.  Verrà avviata l'installazione.

2.  Nella schermata Contratto di licenza, leggere il contratto, selezionare **accetto** e fare clic su **Avanti**.

3.  Assicurarsi che la cartella di destinazione sia corretta e fare clic su **Avanti**.

4.  Una volta completata l'installazione, fare clic su **fine**.

Ora sei pronto per avviare il server Windows Azure multi-Factor Authentication installata sul server federativo e configurarlo come un metodo di autenticazione aggiuntivo.

##### <a name="BKMK_d"></a>Configurare Windows Azure multi-Factor Authentication come metodo di autenticazione aggiuntivi

1.  Avviare **Windows Azure multi-Factor Authentication** da in cui è installato nel server federativo e nella pagina di benvenuto, controllare il **ignorare la configurazione guidata autenticazione** casella di controllo e fare clic su **Avanti**.

2.  Per attivare il Server multi-Factor Authentication, tornare alla pagina nel portale di gestione multi-Factor Authentication in cui è stato scaricato il Server multi-Factor Authentication e fare clic su di **genera codice di attivazione** pulsante. Nell'interfaccia utente di multi-Factor Authentication, immettere le credenziali che sono state generate e fare clic su **attiva**.

3.  Successivamente, il **Server multi-Factor Authentication** interfaccia utente viene richiesto di eseguire il **configurazione guidata multiserver**.  Selezionare **No**.

    > [!IMPORTANT]
    > È possibile evitare l'esecuzione di **configurazione guidata multiserver** dato dell'ambiente di testing con un solo server federativo che viene utilizzato per completare questa procedura dettagliata. Tuttavia, se l'ambiente contiene più server federativi, è necessario installare il Server multi-Factor Authentication e completare il **configurazione guidata multiserver** in ogni server federativo per abilitare la replica tra i server multi-Factor in esecuzione nei server federativi.

4.  Nel **Server multi-Factor Authentication** interfaccia utente, seleziona il **utenti** icona, fare clic su **Importa da Active Directory**, selezionare il **Robert Hatley** account per eseguirne il provisioning in Windows Azure multi-Factor Authentication e quindi fare clic su **importazione**.

5.  Nel **utenti** elenco, selezionare il **Robert Hatley** account, fare clic su **modifica**e il **modifica utente** finestra, specificare un numero di telefono cellulare dell'account, assicurarsi che il **abilitato** casella di controllo è selezionata, quindi fare clic su **applica**.

6.  Nel **utenti** elenco, seleziona il **Robert Hatley** account, quindi scegliere **Test**. Nel **utente Test** finestra, fornire le credenziali per il **Robert Hatley** account. Quando lo squillo del cellulare, premere '#' per completare la verifica.

7.  Nel **Server multi-Factor Authentication** interfaccia utente, seleziona il **ADFS** icona, assicurarsi che **registrazione utente Consenti**, **consentire agli utenti di selezionare metodo** (inclusi **chiamata telefonica** e **SMS**), **usare domande di sicurezza per il fallback** e **abilitare la registrazione** siano selezionate le caselle di controllo, fare clic su **installa scheda ADFS**e completare il **scheda ADFS di multi-Factor Authentication** installazione guidata.

    > [!NOTE]
    > Il **scheda ADFS di multi-Factor Authentication** installazione guidata crea un gruppo di sicurezza denominato **PhoneFactor Admins** in Active Directory e quindi aggiunge l'account del servizio ADFS del servizio federativo a questo gruppo.
    > 
    > È consigliabile verificare nel controller di dominio che il **PhoneFactor Admins** effettiva creazione del gruppo e che account del servizio ADFS sia un membro di questo gruppo.
    > 
    > Se necessario, aggiungere l'account del servizio ADFS per il **PhoneFactor Admins** gruppo manualmente nel controller di dominio.

    Per ulteriori dettagli sull'installazione la scheda ADFS, fare clic sul collegamento Guida nell'angolo superiore destro del Server multi-Factor Authentication.

8.  Per registrare l'adattatore nel servizio federativo, nel server federativo, avviare la finestra di comando Windows PowerShell ed eseguire il comando seguente: `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`. La scheda è ora registrata come **WindowsAzureMultiFactorAuthentication**.  È necessario riavviare il servizio ADFS rendere effettiva la registrazione.

9. Per configurare Windows Azure multi-Factor Authentication come metodo di autenticazione aggiuntivo, nella Console di gestione di ADFS, passare al **criteri di autenticazione** nodo e in **multi-factor Authentication** fare clic su di **modifica** collegamento accanto al **impostazioni globali** sottosezione. Nel **Modifica criteri di autenticazione globali** finestra, seleziona **multi-Factor Authentication** come metodo di autenticazione aggiuntivo e quindi fare clic su **OK**.

    > [!NOTE]
    > È possibile personalizzare il nome e la descrizione del metodo Windows Azure multi-Factor Authentication, nonché qualsiasi configurato il metodo di autenticazione di terze parti, così come appare nell'interfaccia utente AD FS, eseguendo il **Set-AdfsAuthenticationProviderWebContent** cmdlet. Per ulteriori informazioni, vedere [https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>Impostare i criteri di autenticazione a più fattori
Per abilitare l'autenticazione a più fattori, è necessario impostare i criteri di autenticazione a più fattori nel server federativo. Per questa procedura dettagliata per i criteri di autenticazione a più fattori, **Robert Hatley** account è necessario per eseguire l'autenticazione a più fattori perché appartiene al **Finance** gruppo impostati in [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

È possibile impostare i criteri di autenticazione a più fattori tramite la Console di gestione ADFS o l'utilizzo di Windows PowerShell.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>Per configurare i criteri di autenticazione a più fattori in base ai dati di appartenenza a gruppi dell'utente per 'claimapp' tramite la Console di gestione ADFS

1.  Nel server federativo, nella Console di gestione di ADFS, passare a **criteri di autenticazione**\\**Per attendibilità** nodo e selezionare il trust della relying party che rappresenta l'applicazione di esempio (**claimapp**).

2.  Nel **azioni** pagina o facendo **claimapp**selezionare **Modifica autenticazione a più fattori personalizzata**.

3.  Nel **modificare Trust della Relying Party per claimapp** finestra, fare clic su di **Aggiungi** accanto al pulsante il **utenti/gruppi** elenco. Digitare **Finance** per il nome del gruppo di Active Directory creato in [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)e fare clic su **Controlla nomi**, quando il nome viene risolto, fare clic su **OK**.

4.  Fare clic su **OK** nel **modificare Trust della Relying Party per claimapp** finestra.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>Per configurare i criteri di autenticazione a più fattori in base ai dati di appartenenza a gruppi dell'utente per 'claimapp' tramite Windows PowerShell

1.  Nel server federativo, aprire la finestra di comando Windows PowerShell ed eseguire il comando seguente:

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  Nella stessa finestra di comando Windows PowerShell, eseguire il comando seguente:

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > Assicurarsi di sostituire < group_SID > con il valore del SID del gruppo di Active Directory **Finance**.

## <a name="BKMK_4"></a>Passaggio 4: Verificare il meccanismo di autenticazione a più fattori
In questo passaggio verrà verificato le funzionalità di autenticazione a più fattori impostati nel passaggio precedente. È possibile utilizzare la procedura seguente per verificare che **Robert Hatley** utente di Active Directory possa accedere all'applicazione di esempio e questa volta è necessario eseguire l'autenticazione a più fattori perché appartiene al **Finance** gruppo.

1.  Nel computer client, aprire una finestra del browser e passare all'applicazione di esempio: **https://webserv1.contoso.com/claimapp**.

    Questa azione reindirizza automaticamente la richiesta al server federativo e viene richiesto di accedere con un nome utente e password.

2.  Digitare le credenziali del **Robert Hatley** account di Active Directory.

    A questo punto, tenendo conto dei criteri di autenticazione a più fattori configurata, l'utente verrà richiesto per eseguire un'ulteriore autenticazione. Il testo del messaggio predefinito è **per motivi di sicurezza sono necessarie informazioni aggiuntive per verificare il tuo account.** Tuttavia, questo testo è completamente personalizzabile. Per ulteriori informazioni su come personalizzare l'esperienza di accesso, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

    Se come metodo di autenticazione aggiuntivo è stato configurato l'autenticazione del certificato, il testo del messaggio predefinito è **selezionare un certificato che si desidera utilizzare per l'autenticazione. Se si annulla l'operazione, chiudere il browser e riprovare.**

    Se come metodo di autenticazione aggiuntivo è stato configurato Windows Azure multi-Factor Authentication, il testo del messaggio predefinito è **verrà effettuata una chiamata al telefono per completare l'autenticazione.** Per ulteriori informazioni sulla firma con Windows Azure multi-Factor Authentication e l'uso di varie opzioni per il metodo di verifica preferito, vedere [Panoramica di Windows Azure multi-Factor Authentication](https://technet.microsoft.com/library/dn249479.aspx).

## <a name="see-also"></a>Vedere anche
[Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



