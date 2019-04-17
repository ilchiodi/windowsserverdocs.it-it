---
ms.assetid: 276a7f7d-5faa-4c00-a51c-3fa511fe52f9
title: Configurare un ambiente di testing di AD FS
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 53d0e24f7fcb9efc64406dc6ed01f5bb1deb2277
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="set-up-an-ad-fs-lab-environment"></a>Configurare un ambiente di testing di AD FS

>Si applica a: Windows Server 2012 R2

In questo argomento vengono illustrati i passaggi per configurare un ambiente di testing che può essere usato per completare le procedure dettagliate nelle guide seguenti:  
  
-   [Procedura dettagliata: Aggiunta alla rete aziendale con un dispositivo iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)  
  
-   [Procedura dettagliata: All'area di lavoro con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)  
  
-   [Guida allo scenario: Gestire i rischi con il controllo di accesso condizionale](Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)  
  
-   [Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)  
  
> [!NOTE]  
> Non è consigliabile installare il server web e il server federativo nello stesso computer.  
  
Per configurare questo ambiente di testing, completare i passaggi seguenti:  
  
1.  [Passaggio 1: Configurare il controller di dominio (DC1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)  
  
2.  [Passaggio 2: Configurare il server federativo (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)  
  
3.  [Passaggio 3: Configurare il server web (WebServ1) e un'applicazione basata su attestazioni di esempio](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)  
  
4.  [Passaggio 4: Configurare il computer client (Client1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)  
  
## <a name="BKMK_1"></a>Passaggio 1: Configurare il controller di dominio (DC1)  
Ai fini di questo ambiente di testing, è possibile chiamare il dominio di Active Directory radice **contoso.com** e specificare ** pass@word1 ** come la password dell'amministratore.  
  
-   Installare il servizio ruolo di dominio Active Directory e servizi di dominio Active Directory (AD DS) per configurare il computer di un controller di dominio in Windows Server 2012 R2. Questa operazione Aggiorna lo schema di Active Directory come parte della creazione del controller di dominio. For more information and step-by-step instructions, see[https://technet.microsoft.com/ library/hh472162.aspx](https://technet.microsoft.com/library/hh472162.aspx).  
  
### <a name="BKMK_2"></a>Creare gli account di Active Directory di test  
Dopo avere funzionalità del controller di dominio, è possibile creare gli account di un utente di test e del gruppo di test in questo dominio e aggiungere l'account utente all'account di gruppo. Utilizzare questi account per completare le procedure dettagliate nelle guide procedura dettagliata che vengono fatto riferimento in precedenza in questo argomento.  
  
Creare gli account seguenti:  
  
-   Utente: **Robert Hatley** con le seguenti credenziali: nome utente: **RobertH** e la password:**P@ssword**  
  
-   Gruppo: **Finanza**  
  
For information about how to create user and group accounts in Active Directory (AD), see [https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).  
  
Aggiungere il **Robert Hatley** account per il **Finance** gruppo. For information on how to add a user to a group in Active Directory, see [https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx](https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx).  
  
### <a name="create-a-gmsa-account"></a>Creare un account gestito  
L'account di servizio Account gestito di gruppo è richiesto durante l'installazione di Active Directory Federation Services (ADFS) e la configurazione.  
  
##### <a name="to-create-a-gmsa-account"></a>Per creare un account gestito  
  
1.  Aprire una finestra di comando di Windows PowerShell e digitare:  
  
    ```  
    Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)  
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com  
  
    ```  
  
## <a name="BKMK_4"></a>Passaggio 2: Configurare il server federativo (ADFS1) con Device Registration Service  
To set up another virtual machine, install  Windows Server 2012 R2  and connect it to the domain **contoso.com**. Set up the computer after you have joined it to the domain, and then proceed to install and configure the AD FS role.  
  
For a video, see [Active Directory Federation Services How-To Video Series: Installing an AD FS Server Farm](https://technet.microsoft.com/video/dn469436).  
  
### <a name="install-a-server-ssl-certificate"></a>Installare un certificato SSL del server  
È necessario installare un certificato Secure Socket Layer (SSL) del server sul server ADFS1 nell'archivio del computer locale. Il certificato deve avere gli attributi seguenti:  
  
-   Nome soggetto (CN): adfs1.contoso.com  
  
-   Nome del soggetto alternativo (DNS): adfs1.contoso.com  
  
-   Nome del soggetto alternativo (DNS): enterpriseregistration.contoso.com  
  
For more information about setting up SSL certificates, see [Configure SSL/TLS on a Web site in the domain with an Enterprise CA](https://social.technet.microsoft.com/wiki/contents/articles/12485.configure-ssltls-on-a-web-site-in-the-domain-with-an-enterprise-ca.aspx).  
  
[Active Directory Federation Services How-To Video Series: Updating Certificates](https://technet.microsoft.com/video/adfs-updating-certificates).  
  
### <a name="install-the-ad-fs-server-role"></a>Installare il ruolo server AD FS  
  
##### <a name="to-install-the-federation-service-role-service"></a>Per installare il servizio ruolo servizio federativo  
  
1.  Accedere al server utilizzando l'account di amministratore di dominio administrator@contoso.com.  
  
2.  Avviare Server Manager. Per avviare Server Manager, fare clic su **Server Manager** di Windows **Start** schermata o fare clic su **Server Manager** della barra delle applicazioni di Windows sul desktop di Windows. Nel **avvio rapido** scheda della finestra di **iniziale** riquadro il **Dashboard** pagina, fare clic su **Aggiungi ruoli e funzionalità**. In alternativa, è possibile fare clic su **Aggiungi ruoli e funzionalità** nel **Gestisci** menu.  
  
3.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.  
  
4.  Nel **Selezione tipo di installazione** pagina, fare clic su **installazione basata su ruoli o basata su funzionalità**, quindi fare clic su **Avanti**.  
  
5.  Nel **server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, verificare che il computer di destinazione sia selezionata e quindi fare clic su **Avanti**.  
  
6.  Nel **Selezione ruoli server** pagina, fare clic su **Active Directory Federation Services**, quindi fare clic su **Avanti**.  
  
7.  Nel **selezionare le funzionalità** pagina, fare clic su **Avanti**.  
  
8.  Nel **Active Directory Federation Services (ADFS)** pagina, fare clic su **Avanti**.  
  
9. Dopo aver verificato le informazioni sul **Conferma selezioni per l'installazione** pagina, selezionare il **riavvia automaticamente il server di destinazione se necessario** casella di controllo, quindi fare clic su **installare**.  
  
10. Nel **lo stato dell'installazione** pagina, verificare che tutto sia stato installato correttamente e quindi fare clic su **Chiudi**.  
  
### <a name="configure-the-federation-server"></a>Configurare il server federativo  
Il passaggio successivo consiste nel configurare il server federativo.  
  
##### <a name="to-configure-the-federation-server"></a>Per configurare il server federativo  
  
1.  In Server Manager **Dashboard** pagina, fare clic su di **notifiche** flag e quindi fare clic su **configurare il servizio federativo nel server di**.  
  
    Il **servizio Configurazione guidata di Active Directory Federation** apre.  
  
2.  Nel **iniziale** selezionare **creare il primo server federativo in una server farm federativa**, quindi fare clic su **Avanti**.  
  
3.  Nel **connettersi a servizi di dominio Active Directory** specificare un account con diritti di amministratore di dominio per il **contoso.com** a questo computer è unito al dominio Active Directory, quindi fare clic su **Avanti**.  
  
4.  Nel **specificare le proprietà del servizio** pagina, eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Importare il certificato SSL ottenuto in precedenza. Questo certificato è il certificato di autenticazione servizio obbligatorio. Selezionare il percorso del certificato SSL.  
  
    -   To provide a name for your federation service, type **adfs1.contoso.com**. This value is the same value that you provided when you enrolled an SSL certificate in Active Directory Certificate Services (AD CS).  
  
    -   Per specificare un nome visualizzato per il servizio federativo, digitare **Contoso Corporation**.  
  
5.  Nel **impostazione Account del servizio** selezionare **utilizzare un account utente di dominio o Account del servizio gestito di gruppo**e quindi specificare l'account gestito **fsgmsa** creato al momento della creazione del controller di dominio.  
  
6.  Nel **impostazione Database di configurazione** pagina, selezionare **creare un database nel server mediante il Database interno di Windows**, quindi fare clic su **Avanti**.  
  
7.  Nel **verifica opzioni** pagina, verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.  
  
8.  Nel **controlli dei prerequisiti** verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **configura**.  
  
9. Nel **risultati** pagina, esaminare i risultati, controllare se la configurazione è stata completata correttamente e quindi fare clic su **passaggi successivi necessari per completare la distribuzione del servizio federativo**.  
  
### <a name="configure-device-registration-service"></a>Configurare Device Registration Service  
Il passaggio successivo consiste nel configurare Device Registration Service del server ADFS1. For a video, see [Active Directory Federation Services How-To Video Series: Enabling the Device Registration Service](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service).  
  
##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>Per configurare dispositivo registrazione servizio per Windows Server 2012 RTM  
  
1.  > [!IMPORTANT]  
    > **Il passaggio seguente si applica alla versione RTM di Windows Server 2012 R2.**  
  
    Aprire una finestra di comando di Windows PowerShell e digitare:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
    Quando viene richiesto un account del servizio, digitare **contosofsgmsa$**.  
  
    Eseguire il cmdlet Windows PowerShell.  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Sul server ADFS1, nel **gestione di ADFS** console, passare a **criteri di autenticazione**. Selezionare **Modifica autenticazione primaria globale**. Selezionare la casella di controllo accanto a **Abilita autenticazione dispositivo**, quindi fare clic su **OK**.  
  
### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>Aggiungere Host (A) e record di risorse Alias (CNAME) al DNS  
Su DC1, è necessario assicurarsi che vengono creati i seguenti record sistema DNS (Domain Name) per il servizio Registrazione dispositivi.  
  
|Voce|Tipo|Indirizzo|  
|---------|--------|-----------|  
|adfs1|Host (A)|Indirizzo IP del server AD FS|  
|enterpriseregistration|Alias (CNAME)|adfs1.contoso.com|  
  
È possibile utilizzare la procedura seguente per aggiungere un record di risorse host (A) al server dei nomi DNS aziendale per il server federativo e Device Registration Service.  
  
L'appartenenza al gruppo Administrators o equivalente è il requisito minimo per completare questa procedura. Review details about using the appropriate accounts and group memberships in the  HYPERLINK "https://go.microsoft.com/fwlink/?LinkId=83477" Local and Domain Default Groups (https://go.microsoft.com/fwlink/p/?LinkId=83477).  
  
##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Per aggiungere un host (A) e record di risorse alias (CNAME) al DNS per il server federativo  
  
1.  Su DC1, da Server Manager, nel **strumenti** menu, fare clic su **DNS** per aprire lo snap-in DNS.  
  
2.  Nell'albero della console espandere DC1, **zone di ricerca diretta**, fare doppio clic su **contoso.com**, quindi fare clic su **nuovo Host (A o AAAA)**.  
  
3.  In **nome,** digitare il nome che si desidera utilizzare per la farm ADFS. Per questa procedura dettagliata digitare **adfs1**.  
  
4.  In **indirizzo IP**, digitare l'indirizzo IP del server ADFS1. Fare clic su **aggiungere Host**.  
  
5.  Fare doppio clic su **contoso.com**, quindi fare clic su **nuovo Alias (CNAME)**.  
  
6.  Nel **nuovo Record di risorse** la finestra di dialogo, digitare **enterpriseregistration** nel **nome Alias** casella.  
  
7.  Nel dominio nome (completo) della casella di host di destinazione, digitare **adfs1.contoso.com**, quindi fare clic su **OK**.  
  
    > [!IMPORTANT]  
    > Se la società ha più suffissi di nome dell'entità (UPN) utente, in una distribuzione reale, è necessario creare più record CNAME, uno per ognuno dei suffissi UPN nel DNS.  
  
## <a name="BKMK_5"></a>Passaggio 3: Configurare il server web (WebServ1) e un'applicazione basata su attestazioni di esempio  
Set up a virtual machine (WebServ1) by installing the  Windows Server 2012 R2  operating system and connect it to the domain **contoso.com**. After it is joined to the domain, you can proceed to install and configure the Web Server role.  
  
Per completare le procedure dettagliate citate in precedenza in questo argomento, è necessario disporre di un'applicazione di esempio protetta dal server federativo (ADFS1).  
  
You can download Windows Identity Foundation SDK ([https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451), which includes a sample claims-based application.  
  
È necessario completare i passaggi seguenti per configurare un server web con l'applicazione di esempio basata sulle attestazioni.  
  
> [!NOTE]  
> Questi passaggi sono stati testati su un server web che esegue il sistema operativo Windows Server 2012 R2.  
  
1.  [Installare il ruolo Server Web e Windows Identity Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)  
  
2.  [Installare Windows Identity Foundation SDK](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)  
  
3.  [Configurare l'app semplice basata su attestazioni in IIS](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)  
  
4.  [Creare un trust della relying party nel server federativo](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)  
  
### <a name="BKMK_15"></a>Installare il ruolo Server Web e Windows Identity Foundation  
  
1.  > [!NOTE]  
    > È necessario avere accesso al supporto di installazione di Windows Server 2012 R2.  
  
    Accedere a WebServ1 usando ** administrator@contoso.com ** e la password ** pass@word1 **.  
  
2.  Da Server Manager, nel **avvio rapido** scheda della finestra di **iniziale** riquadro il **Dashboard** pagina, fare clic su **Aggiungi ruoli e funzionalità**. In alternativa, è possibile fare clic su **Aggiungi ruoli e funzionalità** nel **Gestisci** menu.  
  
3.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.  
  
4.  Nel **Selezione tipo di installazione** pagina, fare clic su **installazione basata su ruoli o basata su funzionalità**, quindi fare clic su **Avanti**.  
  
5.  Nel **server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, verificare che il computer di destinazione sia selezionata e quindi fare clic su **Avanti**.  
  
6.  Nel **Selezione ruoli server** pagina, selezionare la casella di controllo accanto a **Server Web (IIS)**, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
7.  Nel **selezionare le funzionalità** pagina, selezionare **Windows Identity Foundation 3.5**, quindi fare clic su **Avanti**.  
  
8.  Nel **ruolo Server Web (IIS)** pagina, fare clic su **Avanti**.  
  
9. Nel **Selezione servizi ruolo** pagina, selezionare ed espandere **lo sviluppo di applicazioni**. Selezionare **ASP.NET 3.5**, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
10. Nel **Conferma selezioni per l'installazione** pagina, fare clic su **specificare un percorso di origine alternativo**. Immettere il percorso della directory Sxs che si trova nel supporto di installazione di Windows Server 2012 R2. Ad esempio D:SourcesSxs. Fare clic su **OK**, quindi fare clic su **installare**.  
  
### <a name="BKMK_13"></a>Installare Windows Identity Foundation SDK  
  
1.  Run WindowsIdentityFoundation-SDK-3.5.msi to install Windows Identity Foundation SDK 3.5 (https://www.microsoft.com/download/details.aspx?id=4451). Scegliere tutte le opzioni predefinite.  
  
### <a name="BKMK_9"></a>Configurare l'app semplice basata su attestazioni in IIS  
  
1.  Installare un certificato SSL valido nell'archivio certificati del computer. Il certificato deve contenere il nome del server web, **webserv1.contoso.com**.  
  
2.  Copiare il contenuto di C:Program file (x86) Windows Identity Foundation SDKv3.5SamplesQuick StartWeb ApplicationPassiveRedirectBasedClaimsAwareWebApp C:InetpubClaimapp.  
  
3.  Modifica il **default.aspx. cs** del file in modo da alcun filtro di attestazioni non avviene. Questo passaggio viene eseguito per assicurarsi che l'applicazione di esempio visualizzi tutte le attestazioni che vengono rilasciate dal server federativo. Eseguire le operazioni seguenti:  
  
    1.  Apri **default.aspx. cs** in un editor di testo.  
  
    2.  Cercare nel file la seconda istanza di `ExpectedClaims`.  
  
    3.  Commento l'intera `IF` istruzione e relative parentesi graffe. Indicare i commenti digitando "/ /" (senza virgolette) all'inizio di una riga.  
  
    4.  Il `FOREACH` istruzione dovrebbe ora essere simile a questo esempio di codice.  
  
        ```  
        Foreach (claim claim in claimsIdentity.Claims)  
        {  
           //Before showing the claims validate that this is an expected claim  
           //If it is not in the expected claims list then don’t show it  
           //if (ExpectedClaims.Contains( claim.ClaimType ) )  
           // {  
              writeClaim( claim, table );  
           //}  
        }  
  
        ```  
  
    5.  Salvare e chiudere **default.aspx. cs**.  
  
    6.  Apri **Web. config** in un editor di testo.  
  
    7.  Rimuovere l'intera `<microsoft.identityModel>` sezione. Rimuovere tutto a partire dal `including <microsoft.identityModel>` e fino a e incluso `</microsoft.identityModel>`.  
  
    8.  Salvare e chiudere **Web. config**.  
  
4.  **Configurare Gestione IIS**  
  
    1.  Apri **Internet Information Services (IIS) Manager**.  
  
    2.  Vai a **pool di applicazioni**, fare doppio clic su **DefaultAppPool** per selezionare **impostazioni avanzate**. Impostare **carica profilo utente** a **True**, quindi fare clic su **OK**.  
  
    3.  Fare doppio clic su **DefaultAppPool** per selezionare **le impostazioni di base**. Modifica il **versione CLR .NET** a **CLR .NET versione v 2.0.50727**.  
  
    4.  Fare doppio clic su **sito Web predefinito** per selezionare **Modifica binding**.  
  
    5.  Aggiungere un **HTTPS** binding alla porta **443** con il certificato SSL installato.  
  
    6.  Fare doppio clic su **sito Web predefinito** per selezionare **Aggiungi applicazione**.  
  
    7.  Impostare l'alias su **claimapp** e il percorso fisico su **c:inetpubclaimapp**.  
  
5.  Per configurare **claimapp** per lavorare con il server federativo, eseguire le operazioni seguenti:  
  
    1.  Eseguire FedUtil.exe si trova in **C:Program file (x86) Windows Identity Foundation SDKv3.5**.  
  
    2.  Impostare il percorso di configurazione dell'applicazione su **C:inetputclaimappweb.config** e impostare l'URI dell'applicazione sull'URL del sito, **: https://webserv1.contoso.com /claimapp/**. Fare clic su **Avanti**.  
  
    3.  Selezionare **utilizzare un STS esistente** e passare all'URL dei metadati del server AD FS **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml**. Fare clic su **Avanti**.  
  
    4.  Selezionare **disabilitare la convalida della catena di certificati**, quindi fare clic su **Avanti**.  
  
    5.  Selezionare **Nessuna crittografia**, quindi fare clic su **Avanti**. Nel **attestazioni offerte** pagina, fare clic su **Avanti**.  
  
    6.  Selezionare la casella di controllo accanto a **pianificare un'attività per eseguire aggiornamenti quotidiani i metadati di WS-Federation**. Fare clic su **fine **.  
  
    7.  L'applicazione di esempio è ora configurata. Se si prova l'URL dell'applicazione **https://webserv1.contoso.com/claimapp**, dovrebbe essere reindirizzati al server federativo. Il server federativo verrà visualizzata una pagina di errore perché non è ancora configurato il trust della relying party. In altre parole, si è protetto non l'applicazione di test da ADFS.  
  
Ora è necessario proteggere l'applicazione di esempio che viene eseguito sul server web con AD FS. È possibile farlo tramite l'aggiunta di un trust della relying party nel server federativo (ADFS1). For a video, see [Active Directory Federation Services How-To Video Series: Add a Relying Party Trust](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust).  
  
### <a name="BKMK_11"></a>Creare un trust della relying party nel server federativo  
  
1.  Nel server federativo (ADFS1), nel **console di gestione di ADFS**, passare a **attendibilità**, quindi fare clic su **Aggiungi attendibilità**.  
  
2.  Nel **Seleziona origine dati** selezionare **Importa dati sul componente pubblicati online o in una rete locale**, immettere l'URL dei metadati per **claimapp**, quindi fare clic su **Avanti**. Esecuzione FedUtil.exe creato un file XML dei metadati. Si trova in   
    **https://webserv1.contoso.com/claimapp/FederationMetadata/2007-06/FederationMetadata.XML**.  
  
3.  Nel **Specifica nome visualizzato** specificare il **nome visualizzato** per il trust della relying party **claimapp**, quindi fare clic su **Avanti**.  
  
4.  Nel **configurare multi-factor Authentication ora? ** selezionare **non desidero configurare le impostazioni di autenticazione a più fattori per questo trust della relying party in questo momento**, quindi fare clic su **Avanti**.  
  
5.  Nel **Scegli regole di autorizzazione rilascio** pagina, selezionare **consentire tutti gli utenti di accedere a questo componente**, quindi fare clic su **Avanti**.  
  
6.  Nel **Aggiunta attendibilità** pagina, fare clic su **Avanti**.  
  
7.  Nel **Modifica regole attestazione** la finestra di dialogo, fare clic su **Aggiungi regola**.  
  
8.  Nel **Scegli tipo di regola** selezionare **inviare attestazioni mediante una regola personalizzata**, quindi fare clic su **Avanti**.  
  
9. Nel **Configura regola attestazione** pagina il **nome regola attestazione** digitare **tutte le attestazioni**. Nel **regola personalizzata** , digitare la seguente regola attestazioni.  
  
    ```  
    c:[ ]  
    => issue(claim = c);  
  
    ```  
  
10. Fare clic su **fine**, quindi fare clic su **OK**.  
  
## <a name="BKMK_10"></a>Passaggio 4: Configurare il computer client (Client1)  
Configurare un'altra macchina virtuale e installare Windows 8.1. Questa macchina virtuale deve essere sulla stessa rete come le altre macchine virtuale. Questo computer non deve appartenere al dominio Contoso.  
  
Il client deve considerare attendibile il certificato SSL utilizzato per il server federativo (ADFS1) configurato in [passaggio 2: configurare il server federativo (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Deve inoltre essere in grado di convalidare le informazioni di revoca di certificato per il certificato.  
  
È inoltre necessario configurare e utilizzare un account Microsoft per accedere a Client1.  
  
## <a name="see-also"></a>Vedere anche  
[Active Directory Federation Services pratici serie di Video: Installazione una Server Farm AD FS](https://technet.microsoft.com/video/dn469436)  
[Active Directory Federation Services serie di Video illustrativi su: Aggiornamento dei certificati](https://technet.microsoft.com/video/adfs-updating-certificates)  
[Active Directory Federation Services pratici serie di Video: Aggiungere un Trust della Relying Party](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)  
[Active Directory Federation Services serie di Video illustrativi su: Il servizio Registrazione dispositivi di abilitazione](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)  
[Active Directory Federation Services serie di Video illustrativi su: Installazione di Proxy applicazione Web](https://technet.microsoft.com/video/dn469438)  
  
