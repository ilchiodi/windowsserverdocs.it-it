---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: Guida dettagliata-gestire i rischi con ulteriori Multi-Factor Authentication per le applicazioni sensibili
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 08aadcf0322fcb937bdde17d18aa5d30e3da68ce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357793"
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari




## <a name="about-this-guide"></a>Informazioni sulla guida
Questa procedura dettagliata fornisce le istruzioni per la configurazione dell'autenticazione a più fattori (multi-factor authentication) in Active Directory Federation Services (AD FS) in Windows Server 2012 R2 in base ai dati di appartenenza a gruppi dell'utente.

Per ulteriori informazioni sull'autenticazione a più fattori e sui meccanismi di autenticazione in AD FS, vedere [gestire i rischi con multi-factor authentication aggiuntivi per le applicazioni riservate](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

Questo scenario è suddiviso nelle sezioni seguenti:

-   [Passaggio 1: configurazione dell'ambiente lab](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Passaggio 2: verificare il meccanismo di autenticazione AD FS predefinito](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [Passaggio 3: configurare l'autenticazione a più fattori nel server federativo](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [Passaggio 4: verificare il meccanismo di autenticazione a più fattori](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>Passaggio 1: configurazione dell'ambiente lab
Per eseguire le procedure descritte in questo scenario, è necessario un ambiente con i componenti seguenti:

-   Un dominio di Active Directory con un utente di test e account di gruppo in esecuzione in Windows Server 2012 R2 o un dominio di Active Directory in esecuzione in Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 con lo schema aggiornato a Windows Server 2012 R2

-   Server federativo in esecuzione in Windows Server 2012 R2

-   Un server Web che ospita l'applicazione di esempio

-   Un computer client da cui è possibile accedere all'applicazione di esempio

> [!WARNING]
> Sia negli ambienti di produzione che in quelli di testing è consigliabile evitare di utilizzare lo stesso computer come server federativo e come server Web.

In questo ambiente, il server federativo rilascia le attestazioni richieste affinché gli utenti possano accedere all'applicazione di esempio. Il server Web ospita un'applicazione di esempio che considererà attendibili gli utenti che presentano le attestazioni rilasciate dal server federativo.

Per istruzioni su come configurare questo ambiente, vedere [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Passaggio 2: verificare il meccanismo di autenticazione AD FS predefinito
In questo passaggio verrà verificato il meccanismo di controllo degli accessi predefinito di ADFS, (**Autenticazione basata su form** per la rete Extranet e **Autenticazione di Windows** per la rete Intranet), in base al quale l'utente viene reindirizzato alla pagina di accesso ad ADFS, specifica le credenziali valide e ottiene l'accesso all'applicazione. È possibile usare l'account di Active Directory **Robert Hatley** e l'applicazione di esempio **ClaimApp** configurata in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

1.  Nel computer client aprire una finestra del browser e passare all'applicazione di esempio: **https://webserv1.contoso.com/claimapp** .

    Questa azione reindirizza automaticamente la richiesta al server federativo e viene richiesto di eseguire l'accesso specificando nome utente e password.

2.  Digitare le credenziali dell'account di Active Directory **Robert Hatley** creato in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Verrà concesso l'accesso all'applicazione.

## <a name="BKMK_3"></a>Passaggio 3: configurare l'autenticazione a più fattori nel server federativo
Per la configurazione dell'autenticazione a più fattori in AD FS in Windows Server 2012 R2 sono disponibili due parti:

-   [Selezionare un metodo di autenticazione aggiuntivo](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [Configurare i criteri di autenticazione a più fattori](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>Selezionare un metodo di autenticazione aggiuntivo
Per configurare l'autenticazione a più fattori, è necessario selezionare un metodo di autenticazione aggiuntivo. Per gli scopi di questa guida è possibile scegliere una delle opzioni seguenti per il metodo di autenticazione aggiuntivo:

-   Selezionare il metodo di [autenticazione del certificato](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7) disponibile in ad FS in Windows Server 2012 R2 per impostazione predefinita

-   Configurare e selezionare [Windows Azure Multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>Autenticazione del certificato
Eseguire una delle procedure seguenti per selezionare l'autenticazione del certificato come metodo di autenticazione aggiuntivo:

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>Per configurare l'autenticazione del certificato come metodo aggiuntivo tramite la console di gestione di ADFS

1.  Nel server federativo, nella console di gestione di ADFS passare al nodo **Criteri di autenticazione** e nella sezione **Autenticazione a più fattori** fare clic sul collegamento **Modifica** accanto alla sottosezione **Impostazioni globali** .

2.  Nella finestra **Modifica criteri di autenticazione globali** selezionare **Autenticazione certificato** come metodo di autenticazione aggiuntivo e quindi fare clic su **OK**.

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>Per configurare l'autenticazione del certificato come metodo aggiuntivo tramite Windows PowerShell

1.  Nel server federativo aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente:

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > Per verificare la corretta esecuzione del comando, è possibile eseguire il comando `Get-AdfsGlobalAuthenticationPolicy` .

#### <a name="BKMK_8"></a>Multi-Factor Authentication di Windows Azure
Eseguire la procedura seguente per scaricare, configurare e selezionare **Windows Azure Multi-Factor Authentication** come metodo di autenticazione aggiuntivo nel server federativo:

1.  [Creazione di un provider di Multi-Factor Authentication tramite il portale di Windows Azure](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [Scarica Windows Azure server Multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [Installare il server Multi-Factor Authentication di Windows Azure nel server federativo](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [Configurare Multi-Factor Authentication di Windows Azure come metodo di autenticazione aggiuntivo](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>Creazione di un provider di Multi-Factor Authentication tramite il portale di Windows Azure

1.  Accedere al portale Windows Azure come amministratore.

2.  Selezionare Active Directory a sinistra.

3.  Nella parte superiore della pagina Active Directory selezionare **Provider di Autenticazione a più fattori**.  Nella parte inferiore fare quindi clic su **Nuovo**.

4.  In **Servizi app->Active Directory** selezionare **Provider di Autenticazione a più fattori** e quindi selezionare **Creazione rapida**.

5.  In **Servizi app**selezionare **Provider di Autenticazione attivi**e quindi selezionare **Creazione rapida**.

6.  Compilare i campi seguenti e selezionare **Crea**.

    1.  **Nome** : nome del provider di autenticazione a più fattori.

    2.  **Modello di utilizzo** : il modello di utilizzo del Provider di multi-factor authentication.

        -   **Per autenticazione** : modello di acquisto con addebito per autenticazione. Utilizzato generalmente per gli scenari che utilizzano Windows Azure Multi-Factor in un'applicazione per consumatori.

        -   Per modello di acquisto **utente abilitato** con addebito per utente abilitato.  Utilizzato generalmente per gli scenari esposti ai dipendenti, come Office 365.

        Per altre informazioni sui modelli di utilizzo, vedere [Informazioni sui prezzi di Azure](http://www.windowsazure.com/pricing/details/active-directory/).

    3.  **Directory** : tenant di Windows Azure Active Directory a cui è associato il Provider di multi-factor authentication. Questa opzione è facoltativa perché il provider non deve essere collegato a Windows Azure Active Directory per la protezione di applicazioni locali.

7.  Dopo aver fatto clic su Crea, verrà creato il provider di Autenticazione a più fattori e dovrebbe essere visualizzato il messaggio:  Creazione del provider di Autenticazione a più fattori completata.  Fare clic su **OK**.

A questo punto è necessario scaricare il server Windows Azure Multi-Factor Authentication. È possibile eseguire questa operazione avviando il portale Windows Azure Multi-Factor Authentication tramite il portale Windows Azure.

##### <a name="BKMK_b"></a>Scarica Windows Azure server Multi-Factor Authentication

1.  Accedere al portale Windows Azure come amministratore e fare clic sul provider di Autenticazione a più fattori creato nella procedura precedente. Fare quindi clic sul pulsante **Gestisci** .

    Verrà avviato il portale **Windows Azure Multi-Factor Authentication**.

2.  Nel portale **Windows Azure Multi-Factor Authentication** fare clic su **Download**e quindi su **Scarica** per scaricare una copia del server Windows Azure Multi-Factor Authentication.

Dopo aver scaricato l'eseguibile per il server Windows Azure Multi-Factor Authentication, è necessario installarlo nel server federativo.

##### <a name="BKMK_c"></a>Installare il server Multi-Factor Authentication di Windows Azure nel server federativo

1.  Scaricare l'eseguibile per il server Windows Azure Multi-Factor Authentication e fare doppio clic su di esso.  Verrà avviata l'installazione.

2.  Nella schermata del Contratto di licenza, leggere il contratto, selezionare **Accetto** e fare clic su **Avanti**.

3.  Assicurarsi che la cartella di destinazione sia corretta e fare clic su **Avanti**.

4.  Al termine dell'installazione fare clic su **Fine**.

È ora possibile avviare il server Windows Azure Multi-Factor Authentication installato nel server federativo e configurarlo come metodo di autenticazione aggiuntivo.

##### <a name="BKMK_d"></a>Configurare Multi-Factor Authentication di Windows Azure come metodo di autenticazione aggiuntivo

1.  Avviare **Windows Azure Multi-Factor Authentication** dalla posizione di installazione nel server federativo e nella pagina iniziale selezionare la casella di controllo **Non utilizzare la Configurazione guidata autenticazione** , quindi fare clic su **Avanti**.

2.  Per attivare il server Multi-Factor Authentication, tornare al portale di gestione dell'autenticazione a più fattori da cui si è scaricato il server Multi-Factor Authentication e fare clic sul pulsante **Genera codice di attivazione** . Nell'interfaccia utente del server Multi-Factor Authentication immettere le credenziali generate e fare clic su **Attiva**.

3.  L'interfaccia utente di **Multi-Factor Authentication Server** richiede quindi di eseguire **Configurazione guidata multiserver**.  Selezionare **No**.

    > [!IMPORTANT]
    > È possibile evitare l'esecuzione della procedura guidata **Configurazione guidata multiserver** perché l'ambiente di testing include un solo server federativo utilizzato per eseguire le procedure di questa guida. Se l'ambiente include più server federativi, tuttavia, è necessario installare il server Multi-Factor Authentication ed eseguire la procedura guidata **Configurazione guidata multiserver** in ogni server federativo per abilitare la replica tra i server Multi-Factor Authentication in esecuzione nei server federativi.

4.  Nell'interfaccia utente del **server Multi-Factor Authentication** selezionare l'icona **Utenti** , fare clic su **Importa da Active Directory**, selezionare l'account **Robert Hatley** per effettuarne il provisioning in Windows Azure Multi-Factor Authentication e quindi fare clic su **Importa**.

5.  Nell'elenco **Utenti** selezionare l'account **Robert Hatley**, fare clic su **Modifica** e quindi, nella finestra **Modifica utente**, specificare un numero di cellulare per l'account, assicurarsi che la casella di controllo **Abilitato** sia selezionata e fare clic su **Applica**.

6.  Nell'elenco **Utenti** selezionare l'account **Robert Hatley** e fare clic su **Verifica**. Nella finestra **Utente test** specificare le credenziali per l'account **Robert Hatley** . Quando il telefono cellulare squilla, premere ' #' per completare la verifica dell'account.

7.  Nell'interfaccia utente di **Multi-Factor Authentication Server** selezionare l'icona **ADFS** , assicurarsi che le caselle di controllo **Consenti registrazione utente**, **Consenti agli utenti di selezionare il metodo** (incluse le opzioni **Telefonata** e **SMS**), **Usa domande di sicurezza per il fallback** e **Abilita registrazione** siano selezionate, fare clic su **Installa scheda ADFS**e completare l'installazione guidata della scheda ADFS per l'autenticazione a più fattori .

    > [!NOTE]
    > L'installazione guidata della scheda ADFS per l'autenticazione a più fattoricrea un gruppo di sicurezza denominato **PhoneFactor Admins** in Active Directory e quindi aggiunge l'account del servizio ADFS del servizio federativo a questo gruppo.
    > 
    > È consigliabile verificare nel controller di dominio l'effettiva creazione del gruppo **PhoneFactor Admins** e che l'account del servizio ADFS sia un membro di questo gruppo.
    > 
    > Se necessario, aggiungere manualmente l'account del servizio ADFS al gruppo **PhoneFactor Admins** nel controller di dominio.

    Per ulteriori dettagli sull'installazione dell'adattatore ADFS, fare clic sul collegamento alla Guida nell'angolo superiore destro del server Multi-Factor Authentication.

8.  Per registrare l'adattatore nel servizio federativo, avviare la finestra di comando di Windows PowerShell nel server federativo ed eseguire il comando seguente: `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`. L'adattatore è ora registrato come **WindowsAzureMultiFactorAuthentication**.  È necessario riavviare il servizio ADFS per rendere effettiva la registrazione.

9. Per configurare Windows Azure Multi-Factor Authentication come metodo di autenticazione aggiuntivo, nella console di gestione di ADFS passare al nodo **Criteri di autenticazione** e nella sezione **Autenticazione a più fattori** fare clic sul collegamento **Modifica** accanto alla sottosezione **Impostazioni globali**. Nella finestra **Modifica criteri di autenticazione globali** selezionare **Autenticazione a più fattori** come metodo di autenticazione aggiuntivo e quindi fare clic su **OK**.

    > [!NOTE]
    > È possibile personalizzare il nome e la descrizione del metodo Windows Azure Multi-Factor Authentication, nonché di qualsiasi metodo di autenticazione di terze parti configurato, per la visualizzazione nell'interfaccia utente di ADFS eseguendo il cmdlet **Set-AdfsAuthenticationProviderWebContent**. Per ulteriori informazioni, vedere [https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>Configurare i criteri di autenticazione a più fattori
Per abilitare l'autenticazione a più fattori, è necessario configurare i criteri di autenticazione a più fattori nel server federativo. Per questa procedura dettagliata, in base ai criteri di autenticazione a più fattori, l'account **Robert Hatley** deve eseguire l'autenticazione a più fattori perché appartiene al gruppo **Finance** configurato in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

È possibile configurare i criteri di autenticazione a più fattori tramite la console di gestione di ADFS oppure con Windows PowerShell.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>Per configurare i criteri di autenticazione a più fattori in base ai dati di appartenenza a gruppi dell'utente per ' ClaimApp ' tramite la console di gestione di AD FS

1.  Nel server federativo, nella console di gestione di ADFS passare al nodo **Criteri di autenticazione**\\**Per attendibilità del componente** e selezionare l'attendibilità del componente che rappresenta l'applicazione di esempio (**claimapp**).

2.  Nella pagina **Azioni** o facendo clic con il pulsante destro del mouse su **claimapp**selezionare **Modifica autenticazione a più fattori personalizzata**.

3.  Nella finestra di dialogo per la modifica dell'attendibilità del componente claimapp fare clic sul pulsante **Aggiungi** accanto all'elenco **Utenti/Gruppi** . Digitare **Finance** per il nome del gruppo di Active Directory creato in [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md), quindi fare clic su **Controlla nomi**e quando il nome viene risolto fare clic su **OK**.

4.  Fare clic su **OK** nella finestra di dialogo per la modifica dell'attendibilità del componente claimapp.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>Per configurare i criteri di autenticazione a più fattori in base ai dati di appartenenza a gruppi dell'utente per ' ClaimApp ' tramite Windows PowerShell

1.  Nel server federativo aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente:

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  Nella stessa finestra di comando di Windows PowerShell eseguire il comando seguente:

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > Assicurarsi di sostituire <group_SID> con il valore del SID del gruppo **Finance** di Active Directory.

## <a name="BKMK_4"></a>Passaggio 4: verificare il meccanismo di autenticazione a più fattori
In questo passaggio verrà verificata la funzionalità di autenticazione a più fattori configurata nel passaggio precedente. È possibile usare la procedura seguente per verificare che l'utente di Active Directory **Robert Hatley** possa accedere all'applicazione di esempio e che in questo caso debba eseguire l'autenticazione a più fattori perché appartiene al gruppo **Finance** .

1.  Nel computer client aprire una finestra del browser e passare all'applicazione di esempio: **https://webserv1.contoso.com/claimapp** .

    Questa azione reindirizza automaticamente la richiesta al server federativo e viene richiesto di eseguire l'accesso specificando nome utente e password.

2.  Digitare le credenziali dell'account di Active Directory **Robert Hatley** .

    A questo punto, tenendo conto dei criteri di autenticazione a più fattori configurati, all'utente verrà richiesto di eseguire un'ulteriore autenticazione. Il testo del messaggio predefinito è **Per motivi di sicurezza sono necessarie informazioni aggiuntive per verificare l'account** . Tuttavia, questo testo è completamente personalizzabile. Per altre informazioni su come personalizzare l'esperienza di accesso, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

    Se come metodo di autenticazione aggiuntivo è stata configurata l'autenticazione del certificato, il testo del messaggio predefinito è **Selezionare un certificato che si vuole usare per l'autenticazione. Se si annulla l'operazione, chiudere il browser e riprovare.**

    Se come metodo di autenticazione aggiuntivo è stato configurato Windows Azure Multi-Factor Authentication, il testo del messaggio predefinito è **Per completare l'autenticazione, verrà effettuata una chiamata al telefono dell'utente.** Per altre informazioni sulla firma con Windows Azure multi-Factor Authentication e sull'uso di varie opzioni per il metodo di verifica preferito, vedere [Panoramica di Windows Azure multi-Factor Authentication](https://technet.microsoft.com/library/dn249479.aspx).

## <a name="see-also"></a>Vedi anche
[Gestire i rischi con ulteriori multi-factor authentication per le applicazioni sensibili](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



