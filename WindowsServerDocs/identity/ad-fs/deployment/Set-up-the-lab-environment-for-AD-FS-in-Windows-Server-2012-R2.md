---
ms.assetid: 6b38480e-5b1c-49f0-9d46-8cf22f70f0d2
title: Configurare l'ambiente lab per AD FS in Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 44de547b0a9c8636b07886d35c451bca6ec46341
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855174"
---
# <a name="set-up-the-lab-environment-for-ad-fs-in-windows-server-2012-r2"></a>Configurare l'ambiente lab per AD FS in Windows Server 2012 R2


In questo argomento sono illustrati i passaggi per configurare un ambiente di testing che può essere usato per completare le procedure dettagliate nelle guide agli scenari seguenti:

-   [Procedura dettagliata: Workplace Join con un dispositivo iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

-   [Procedura dettagliata: Workplace Join con un dispositivo Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)


-   [Guida allo scenario: gestire i rischi con il controllo degli accessi condizionali](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)

-   [Guida allo scenario: gestire i rischi con ulteriori Multi-Factor Authentication per le applicazioni sensibili](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

> [!NOTE]
> Non è consigliabile installare il server Web e il server federativo nello stesso computer.

Per configurare un ambiente di testing, completare i passaggi seguenti:

1.  [Passaggio 1: configurare il controller di dominio (DC1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)

2.  [Passaggio 2: configurare il server federativo (ADFS1) con Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)

3.  [Passaggio 3: configurare il server Web (WebServ1) e un'applicazione di esempio basata su attestazioni](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)

4.  [Passaggio 4: configurare il computer client (CLIENT1)](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)

## <a name="step-1-configure-the-domain-controller-dc1"></a><a name="BKMK_1"></a>Passaggio 1: configurare il controller di dominio (DC1)
Ai fini di questo ambiente di testing, è possibile chiamare la radice Active Directory dominio **contoso.com** e specificare <strong>pass@word1</strong> come password dell'amministratore.

-   Installare il servizio ruolo Servizi di dominio Active Directory e installare Active Directory Domain Services (AD DS) per rendere il computer un controller di dominio in Windows Server 2012 R2. Questa azione Aggiorna lo schema di servizi di dominio Active Directory come parte della creazione del controller di dominio. Per ulteriori informazioni e istruzioni dettagliate, vedere[https://technet.microsoft.com/library/hh472162.aspx](https://technet.microsoft.com/library/hh472162.aspx).

### <a name="create-test-active-directory-accounts"></a><a name="BKMK_2"></a>Creare account di Active Directory di test
Una volta che il controller di dominio sarà operativo, è possibile creare account utente di test e del gruppo di test in questo dominio e aggiungere l'account utente all'account di gruppo. Usare questi account per completare le procedure dettagliate nelle guide agli scenari elencate in precedenza in questo articolo.

Creare i seguenti account:

- Utente: **Robert Hatley** con le credenziali seguenti: nome utente: **RobertH** e password: <strong>P@ssword</strong>

- Gruppo: **Finance**

Per informazioni su come creare account utente e di gruppo in Active Directory (AD), vedere [https://technet.microsoft.com/library/cc783323%28v.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

Aggiungere l'account **Robert Hatley** al gruppo **Finance**. Per informazioni su come aggiungere un utente a un gruppo in Active Directory, vedere [https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx](https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx).

### <a name="create-a-gmsa-account"></a>Creare un account del servizio gestito del gruppo
L'account del servizio gestito del gruppo (GMSA) è necessario durante l'installazione e la configurazione di Active Directory Federation Services (AD FS).

##### <a name="to-create-a-gmsa-account"></a>Per creare un account del servizio gestito del gruppo

1.  Aprire una finestra di comando di Windows PowerShell e digitare:

    ```
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com

    ```

## <a name="step-2-configure-the-federation-server-adfs1-by-using-device-registration-service"></a><a name="BKMK_4"></a>Passaggio 2: configurare il server federativo (ADFS1) con Device Registration Service
Per configurare un'altra macchina virtuale, installare Windows Server 2012 R2 e connetterlo al dominio **contoso.com**. Configurare il computer dopo che è stato aggiunto al dominio e quindi procedere con l'installazione e la configurazione del ruolo AD FS.

Per un video, vedere la [serie di video illustrativi su Active Directory Federation Services: Installazione di una server farm AD FS](https://technet.microsoft.com/video/dn469436).

### <a name="install-a-server-ssl-certificate"></a>Installare un certificato SSL del server
È necessario installare un certificato SSL (Secure Socket Layer) del server nell'archivio del computer locale sul server ADFS1. Il certificato DEVE avere i seguenti attributi:

-   Nome soggetto (CN): adfs1.contoso.com

-   Nome alternativo del soggetto (DNS): adfs1.contoso.com

-   Nome alternativo del soggetto (DNS): enterpriseregistration.contoso.com

Per altre informazioni sulla configurazione dei certificati SSL, vedere [Configurare SSL/TLS in un sito Web nel dominio con una CA globale (enterprise)](https://social.technet.microsoft.com/wiki/contents/articles/12485.configure-ssltls-on-a-web-site-in-the-domain-with-an-enterprise-ca.aspx).

[Serie di video illustrativi su Active Directory Federation Services: Aggiornamento dei certificati](https://technet.microsoft.com/video/adfs-updating-certificates).

### <a name="install-the-ad-fs-server-role"></a>Installare il ruolo del server ADFS

##### <a name="to-install-the-federation-service-role-service"></a>Per installare il servizio ruolo del servizio federativo

1. Accedere al server utilizzando l'account di amministratore di dominio administrator@contoso.com.

2. Avviare Server Manager. Per avviare Server Manager, fare clic su **Server Manager** nella schermata **Start** di Windows oppure fare clic su **Server Manager** sulla barra delle applicazioni di Windows del desktop Windows. Nella scheda **Avvio rapido** del **riquadro iniziale** nella pagina **Dashboard** fare clic su **Aggiungi ruoli e funzionalità**. In alternativa, è possibile scegliere **Aggiungi ruoli e funzionalità** dal menu **Gestione**.

3. Nella pagina **Prima di iniziare** fare clic su **Avanti**.

4. Nella pagina **Selezione tipo di installazione** selezionare **Installazione basata su ruoli o basata su funzionalità**e quindi fare clic su **Avanti**.

5. Nella pagina **Selezione server di destinazione** fare clic su **Selezionare un server dal pool di server**, verificare che il computer di destinazione sia selezionato e quindi fare clic su **Avanti**.

6. Nella pagina **Selezione ruoli server** fare clic su **Active Directory Federation Services**, quindi su **Avanti**.

7. Nella pagina **Selezione funzionalità** fare clic su **Avanti**.

8. Nella pagina **Active Directory Federation Services (ADFS)** fare clic su **Avanti**.

9. Dopo aver verificato le informazioni nella pagina **Conferma selezioni per l'installazione**, selezionare la casella di controllo **Riavvia automaticamente il server di destinazione se necessario**, quindi fare clic su **Installa**.

10. Nella pagina **Stato installazione** verificare che tutti gli elementi siano installati correttamente e quindi fare clic su **Chiudi**.

### <a name="configure-the-federation-server"></a>Configurare il server federativo
Nel prossimo passaggio viene configurato il server federativo.

##### <a name="to-configure-the-federation-server"></a>Per configurare il server federativo

1.  Nella pagina **Dashboard** di Server Manager fare clic sul flag **Notifiche** e quindi su **Configurare il servizio federativo nel server**.

    Viene aperta la **Configurazione guidata Servizi di dominio Active Directory**.

2.  Nella **pagina iniziale** selezionare **Crea il primo server di federazione di una server farm di federazione** e quindi fare clic su **Avanti**.

3.  Nella pagina **Connessione a Servizi di dominio Active Directory** specificare un account con diritti di amministratore di dominio per il dominio di Active Directory **contoso.com** al quale viene aggiunto questo computer e quindi fare clic su **Avanti**.

4.  Nella pagina **Impostazione proprietà del servizio** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:

    -   Importare il certificato SSL ottenuto in precedenza. Questo è certificato di autenticazione del servizio obbligatorio. Passare al percorso del certificato SSL.

    -   Per specificare un nome per il servizio federativo, digitare **adfs1.contoso.com**. Questo valore è lo stesso fornito al momento della registrazione di un certificato SSL in Servizi certificati Active Directory.

    -   Per specificare un nome visualizzato per il servizio federativo, digitare **Contoso Corporation**.

5.  Nella pagina **Impostazione account del servizio** selezionare **Usa un account del servizio gestito di account utente o gruppo di dominio esistente** e quindi specificare l'account del servizio gestito del gruppo **fsgmsa** creato al momento della creazione del controller di dominio.

6.  Nella pagina **Impostazione database di configurazione** selezionare **Creare un database nel server mediante il database interno di Windows** e quindi fare clic su **Avanti**.

7.  Nella pagina **Verifica opzioni** verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.

8.  Nella pagina **Controlli dei prerequisiti** verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **Configura**.

9. Nella pagina **Risultati** verificare i risultati, controllare se la configurazione è stata completata correttamente e quindi fare clic su **Passaggi successivi necessari per completare la distribuzione del servizio di federazione**.

### <a name="configure-device-registration-service"></a>Configurare il servizio DRS (Device Registration Service)
Nel passaggio successivo viene configurato il servizio DRS sul server ADFS1. Per un video, vedere la [serie di video illustrativi su Active Directory Federation Services: Abilitazione del servizio DRS (Device Registration Service)](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service).

##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>Per configurare il servizio DRS (Device Registration Service) per Windows Server 2012 RTM

1.  > [!IMPORTANT]
    > **Il passaggio seguente si applica alla build di Windows Server 2012 R2 RTM.**

    Aprire una finestra di comando di Windows PowerShell e digitare:

    ```
    Initialize-ADDeviceRegistration
    ```

    Quando viene chiesto di specificare un account del servizio, digitare **contoso\fsgmsa$** .

    Eseguire ora il cmdlet di Windows PowerShell.

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  Nella console di **gestione ADFS** del server ADFS1 passare a **Criteri di autenticazione**. Selezionare **Modifica autenticazione primaria globale**. Selezionare la casella di controllo accanto a **Abilita autenticazione dispositivo** e quindi fare clic su **OK**.

### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>Aggiungere i record di risorse host (A) e alias (CNAME) al DNS
È necessario assicurarsi che su DC1 vengano creati i record DNS (Domain Name System) seguenti per il servizio DRS (Device Registration Service).

|Voce|Type|Address|
|---------|--------|-----------|
|adfs1|Host (A)|Indirizzo IP del server di AD FS|
|enterpriseregistration|Alias (CNAME)|adfs1.contoso.com|

Per aggiungere un record di risorse host (A) ai server dei nomi DNS dell'azienda per i server federativi e il servizio DRS, è possibile usare la procedura seguente.

L'appartenenza al gruppo Administrators o a un gruppo equivalente è il requisito minimo per eseguire questa procedura. Esaminare i dettagli relativi all'uso degli account appropriati e delle appartenenze ai gruppi nel collegamento ipertestuale "<https://go.microsoft.com/fwlink/?LinkId=83477>" gruppi predefiniti locali e di dominio (<https://go.microsoft.com/fwlink/p/?LinkId=83477>).

##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Per aggiungere record di risorse host (A) e alias (CNAME) al DNS per il server federativo

1.  Su DC1, nel menu **Strumenti** di Server Manager fare clic su **DNS** per aprire lo snap-in DNS.

2.  Nell'albero della console espandere DC1, espandere **Zone di ricerca diretta**, fare clic con il pulsante destro del mouse su **contoso.com** e quindi scegliere **Nuovo host (A o AAAA)** .

3.  In **Nome** digitare il nome che si vuole usare per la farm ADFS. Per questa procedura dettagliata digitare **adfs1**.

4.  In **Indirizzo IP** digitare l'indirizzo IP del server ADFS1. Fare clic su **Aggiungi host**.

5.  Fare clic con il pulsante destro del mouse su **contoso.com** e quindi scegliere **Nuovo alias (CNAME)** .

6.  Nella finestra di dialogo **Nuovo record di risorse** digitare **enterpriseregistration** nella casella **Nome alias**.

7.  Nella casella Nome di dominio completo (FQDN) per host destinazione digitare **adfs1.contoso.com** e quindi fare clic su **OK**.

    > [!IMPORTANT]
    > In una distribuzione reale, se l'azienda usa più suffissi del nome dell'entità utente (UPN), è necessario creare più record CNAME, uno per ognuno dei suffissi UPN nel DNS.

## <a name="step-3-configure-the-web-server-webserv1-and-a-sample-claims-based-application"></a><a name="BKMK_5"></a>Passaggio 3: configurare il server Web (WebServ1) e un'applicazione di esempio basata su attestazioni
Configurare una macchina virtuale (WebServ1) installando il sistema operativo Windows Server 2012 R2 e connetterlo al dominio **contoso.com**. Dopo l'aggiunta al dominio, è possibile procedere all'installazione e alla configurazione del ruolo Server Web.

Per completare le procedure dettagliate citate in precedenza in questo argomento, è necessario avere un'applicazione di esempio protetta dal server federativo (ADFS1).

È possibile scaricare Windows Identity Foundation SDK ([https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451), che include un'applicazione di esempio basata su attestazioni.

Per configurare un server Web con questa applicazione di esempio basata su attestazioni, è necessario completare la procedura seguente.

> [!NOTE]
> Questi passaggi sono stati testati su un server Web che esegue il sistema operativo Windows Server 2012 R2.

1.  [Installare il ruolo server Web e Windows Identity Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)

2.  [Installare Windows Identity Foundation SDK](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)

3.  [Configurare la semplice app basata su attestazioni in IIS](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)

4.  [Creare un trust relying party nel server federativo](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)

### <a name="install-the-web-server-role-and-windows-identity-foundation"></a><a name="BKMK_15"></a>Installare il ruolo server Web e Windows Identity Foundation

1. > [!NOTE]
   > È necessario avere accesso al supporto di installazione di Windows Server 2012 R2.

   Accedere a WebServ1 usando <strong>administrator@contoso.com</strong> e la <strong>pass@word1</strong>di password.

2. In Server Manager, nella scheda **Avvio rapido** del **riquadro iniziale** nella pagina **Dashboard** fare clic su **Aggiungi ruoli e funzionalità**. In alternativa, è possibile scegliere **Aggiungi ruoli e funzionalità** dal menu **Gestione**.

3. Nella pagina **Prima di iniziare** fare clic su **Avanti**.

4. Nella pagina **Selezione tipo di installazione** selezionare **Installazione basata su ruoli o basata su funzionalità**e quindi fare clic su **Avanti**.

5. Nella pagina **Selezione server di destinazione** fare clic su **Selezionare un server dal pool di server**, verificare che il computer di destinazione sia selezionato e quindi fare clic su **Avanti**.

6. Nella pagina **Selezione ruoli server** selezionare la casella di controllo accanto a **Server Web (IIS)** , fare clic su **Aggiungi funzionalità** e quindi su **Avanti**.

7. Nella pagina **Selezione funzionalità** selezionare **Windows Identity Foundation 3.5** e quindi fare clic su **Avanti**.

8. Nella pagina **Ruolo Server Web (IIS)** fare clic su **Avanti**.

9. Nella pagina **Selezione servizi ruolo** selezionare ed espandere **Sviluppo di applicazioni**. Selezionare **ASP.NET 3.5**, fare clic su **Aggiungi funzionalità** e quindi su **Avanti**.

10. Nella pagina **Conferma selezioni per l'installazione** fare clic su **Specificare un percorso di origine alternativo**. Immettere il percorso della directory SxS che si trova nel supporto di installazione di Windows Server 2012 R2. Ad esempio D:\Sources\Sxs. Fare clic su **OK** e quindi su **Installa**.

### <a name="install-windows-identity-foundation-sdk"></a><a name="BKMK_13"></a>Installare Windows Identity Foundation SDK

1.  Eseguire WindowsIdentityFoundation-SDK-3.5. msi per installare Windows Identity Foundation SDK 3,5 (https://www.microsoft.com/download/details.aspx?id=4451). Scegliere tutte le opzioni predefinite.

### <a name="configure-the-simple-claims-app-in-iis"></a><a name="BKMK_9"></a>Configurare la semplice app claims in IIS

1.  Installare un certificato SSL valido nell'archivio certificati del computer. Il certificato dovrà contenere il nome del server Web, **webserv1.contoso.com**.

2.  Copiare il contenuto di C:\Programmi (x86)\Windows Identity Foundation SDK\v3.5\Samples\Quick Start\Web Application\PassiveRedirectBasedClaimsAwareWebApp in C:\Inetpub\Claimapp.

3.  Modificare il file **Default.aspx.cs** in modo che non venga applicato alcun filtro di attestazioni. Questo passaggio viene eseguito per verificare che l'applicazione di esempio visualizzi tutte le attestazioni rilasciate dal server federativo. eseguire le operazioni descritte di seguito.

    1.  Aprire **Default.aspx.cs** in un editor di testo.

    2.  Cercare nel file la seconda istanza di `ExpectedClaims`.

    3.  Impostare come commento l'intera istruzione `IF` e le relative parentesi graffe. Per indicare i commenti, digitare "//" (senza le virgolette) all'inizio di una riga.

    4.  L'istruzione `FOREACH` dovrebbe ora essere simile a questo esempio di codice.

        ```
        Foreach (claim claim in claimsIdentity.Claims)
        {
           //Before showing the claims validate that this is an expected claim
           //If it is not in the expected claims list then don't show it
           //if (ExpectedClaims.Contains( claim.ClaimType ) )
           // {
              writeClaim( claim, table );
           //}
        }

        ```

    5.  Salvare e chiudere **Default.aspx.cs**.

    6.  Aprire **web.config** in un editor di testo.

    7.  Rimuovere l'intera sezione `<microsoft.identityModel>` . Rimuovere tutto a partire da `including <microsoft.identityModel>` fino a `</microsoft.identityModel>`incluso.

    8.  Salvare e chiudere **web.config**.

4.  **Configurare Gestione IIS**

    1.  Aprire **Gestione Internet Information Services (IIS)** .

    2.  Passare a **Pool di applicazioni**, fare clic con il pulsante destro del mouse su **DefaultAppPool** e selezionare **Impostazioni avanzate**. Impostare **Carica profilo utente** su **True** e quindi fare clic su **OK**.

    3.  Fare clic con il pulsante destro del mouse su **DefaultAppPool** e selezionare **Impostazioni di base**. Modificare **Versione CLR .NET** in **CLR .NET versione v2.0.50727**.

    4.  Fare clic con il pulsante destro del mouse su **Sito Web predefinito** e selezionare **Modifica binding**.

    5.  Aggiungere un binding **HTTPS** alla porta **443** con il certificato SSL installato.

    6.  Fare clic con il pulsante destro del mouse su **Sito Web predefinito** e selezionare **Aggiungi applicazione**.

    7.  Impostare l'alias su **claimapp** e il percorso fisico su **c:\inetpub\claimapp**.

5.  Per configurare il funzionamento di **claimapp** con il server federativo eseguire le operazioni seguenti:

    1.  Eseguire FedUtil.exe disponibile in **C:\Program Files (x86)\Windows Identity Foundation SDK\v3.5**.

    2.  Impostare il percorso di configurazione dell'applicazione su **C:\inetput\claimapp\web.config** e impostare l'URI dell'applicazione sull'URL per il sito, **https://webserv1.contoso.com/ClaimApp/** . Fare clic su **Avanti**.

    3.  Selezionare **Usa un STS esistente** e passare all'URL dei metadati del server ad FS **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml** . Fare clic su **Avanti**.

    4.  Selezionare **Disable certificate chain validation** e quindi fare clic su **Next**.

    5.  Selezionare **No encryption** e quindi fare clic su **Next**. Nella pagina **Offerto attestazioni** fare clic su **Avanti**.

    6.  Selezionare la casella di controllo accanto a **Pianificare un’attività per eseguire gli aggiornamenti dei metadati di WS-Federation giornalieri**. Fare clic su **Fine**.

    7.  L'applicazione di esempio è ora configurata. Se si prova l'URL dell'applicazione **https://webserv1.contoso.com/claimapp** , è consigliabile reindirizzare l'utente al server federativo. Nel server federativo verrà visualizzata una pagina di errore, perché non è ancora stata configurato il trust della relying party. In altre parole, l'applicazione di test non è stata protetta da AD FS.

È ora necessario proteggere l'applicazione di esempio in esecuzione nel server Web con AD FS. A questo scopo è possibile aggiungere un trust della relying party nel server federativo (ADFS1). Per un video, vedere la [serie di video illustrativi su Active Directory Federation Services: Aggiungere un trust della relying party](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust).

### <a name="create-a-relying-party-trust-on-your-federation-server"></a><a name="BKMK_11"></a>Creare un trust relying party nel server federativo

1.  Nel server federativo (ADFS1), nella **console di gestione di ADFS** passare ad **Attendibilità componente** e quindi fare clic su **Aggiungi attendibilità componente**.

2.  Nella pagina **Seleziona origine dati** selezionare **Importa dati sul componente pubblicati online o in una rete locale**, immettere l'URL dei metadati per **claimapp** e quindi fare clic su **Avanti**. Con l'esecuzione di FedUtil.exe e stato creato un file di metadati con estensione xml. Si trova in **https://webserv1.contoso.com/claimapp/federationmetadata/2007-06/federationmetadata.xml** .

3.  Nella pagina **Specifica nome visualizzato** indicare il **nome visualizzato** per il trust della relying party **claimapp** e quindi fare clic su **Avanti**.

4.  Nella pagina **Configurare l'autenticazione a più fattori?** selezionare **Non desidero configurare le impostazioni di autenticazione a più fattori per l'attendibilità componente al momento** e quindi fare clic su **Avanti**.

5.  Nella pagina **Scegli regole di autorizzazione rilascio** selezionare **Consenti a tutti gli utenti l'accesso a questo componente** e quindi fare clic su **Avanti**.

6.  Nella pagina **Aggiunta attendibilità** fare clic su **Avanti**.

7.  Nella finestra di dialogo **Modifica regole attestazione** fare clic su **Aggiungi regola**.

8.  Nella pagina **Scegli tipo di regola** selezionare **Inviare attestazioni mediante una regola personalizzata** e quindi fare clic su **Avanti**.

9. Nella pagina **Configura regola attestazione** digitare **All Claims** nella casella **Nome regola attestazione**. Nella casella **Regola personalizzata** digitare la seguente regola attestazioni.

    ```
    c:[ ]
    => issue(claim = c);

    ```

10. Fare clic su **Fine** e quindi su **OK**.

## <a name="step-4-configure-the-client-computer-client1"></a><a name="BKMK_10"></a>Passaggio 4: configurare il computer client (CLIENT1)
Configurare un'altra macchina virtuale e installare Windows 8.1. Questa macchina virtuale dovrà appartenere alla stessa rete virtuale delle altre. Questa macchina virtuale NON dovrà essere aggiunta al dominio Contoso.

Il client DEVE considerare attendibile il certificato SSL usato per il server federativo (ADFS1) configurato nel [Step 2: Configure the federation server (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Deve anche riuscire a convalidare le informazioni di revoca per il certificato.

È anche necessario configurare e usare un account Microsoft per accedere a Client1.

## <a name="see-also"></a>Vedi anche


- [Serie di video illustrativi su Active Directory Federation Services: installazione di una server farm AD FS](https://technet.microsoft.com/video/dn469436)
- [Serie di video illustrativi su Active Directory Federation Services: aggiornamento dei certificati](https://technet.microsoft.com/video/adfs-updating-certificates)
- [Serie di video illustrativi su Active Directory Federation Services: aggiungere un trust della relying party](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)
- [Serie di video illustrativi su Active Directory Federation Services: abilitazione del servizio Device Registration](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)
- [Serie di video illustrativi su Active Directory Federation Services: installazione del proxy dell'applicazione Web](https://technet.microsoft.com/video/dn469438)



