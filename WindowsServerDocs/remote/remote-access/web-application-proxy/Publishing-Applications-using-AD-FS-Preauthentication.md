---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: Pubblicazione delle applicazioni con la preautenticazione di AD FS
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: c7dab1dbf97d2dcbda1fe0375e61300f2a1cc373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862242"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>Pubblicazione delle applicazioni con la preautenticazione di AD FS

>Si applica a: Windows Server 2016

**Questo contenuto è rilevante per la versione locale del Proxy applicazione Web. Per abilitare l'accesso sicuro alle applicazioni locali tramite il cloud, vedere la [contenuto di Azure AD Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
In questo argomento descrive come pubblicare applicazioni mediante Proxy applicazione Web usando Active Directory Federation Services (ADFS) la preautenticazione.  
  
Per tutti i tipi di applicazione che è possibile pubblicare usando la preautenticazione di ADFS, è necessario aggiungere un trust della relying party nel servizio federativo di AD FS.  
  
Il flusso di preautenticazione generico di AD FS è come segue:  
  
> [!NOTE]  
> Questo flusso di autenticazione non è applicabile per i client che usano le app di Microsoft Store.  
  
1.  Il dispositivo client tenta di accedere a un'applicazione web pubblicata nell'URL di un particolare risorsa; ad esempio https://app1.contoso.com/.  
  
    L'URL della risorsa è un indirizzo pubblico in cui il Proxy di applicazione Web è in ascolto per le richieste HTTPS in ingresso.  
  
    Se HTTP per il reindirizzamento HTTPS è abilitato, Proxy applicazione Web verrà anche restare in ascolto delle richieste HTTP in ingresso.  
  
2.  Proxy applicazione Web reindirizza la richiesta HTTPS al server AD FS con parametri codificati nell'URL, inclusi l'URL della risorsa e appRealm (un identificatore di entità relying party).  
  
    L'utente viene autenticato mediante il metodo di autenticazione richiesto dal server AD FS. ad esempio, nome utente e password, autenticazione a due fattori con un passcode monouso e così via.  
  
3.  Dopo che l'utente viene autenticato, il server ADFS rilascia un security token, il "token perimetrale", contenente le informazioni seguenti e reindirizza la richiesta di HTTPS al server Proxy applicazione Web:  
  
    -   L'identificativo della risorsa a cui l'utente ha tentato di accedere.  
  
    -   L'identità dell'utente come nome dell'entità utente (UPN).  
  
    -   La scadenza dell'accesso concesso all'utente. L'utente può infatti accedere per un periodo di tempo limitato, al termine del quale deve eseguire nuovamente l'autenticazione.  
  
    -   La firma delle informazioni nel token perimetrale.  
  
4.  Proxy applicazione Web riceve la richiesta HTTPS reindirizzata dal server AD FS con il token perimetrale e convalida e utilizza il token nel modo seguente:  
  
    -   Verifica che la firma del token perimetrale dal servizio federativo è configurato nella configurazione del Proxy applicazione Web.  
  
    -   Verifica che il token sia stato emesso per l'applicazione corretta.  
  
    -   Verifica che il token non sia scaduto.  
  
    -   Utilizza l'identità utente quando necessario, ad esempio per ottenere un ticket Kerberos se il server back-end è configurato per l'utilizzo dell'autenticazione integrata di Windows.  
  
5.  Se il token perimetrale è valido, Proxy applicazione Web inoltra la richiesta HTTPS all'applicazione web pubblicata utilizzando HTTP o HTTPS.  
  
6.  Il client può quindi accedere all'applicazione Web pubblicata, che tuttavia può essere configurata in modo da richiedere all'utente l'esecuzione di passaggi di autenticazione aggiuntivi. Se ad esempio l'applicazione Web pubblicata è un sito di SharePoint che non richiede passaggi di autenticazione aggiuntivi, tale sito verrà visualizzato nel browser dell'utente.  
  
7.  Proxy applicazione Web Salva nel dispositivo client un cookie. Il cookie viene utilizzato da Proxy applicazione Web per identificare questa sessione è già stata preautenticata e che non è necessaria alcuna preautenticazione aggiuntiva.  
  
> [!IMPORTANT]  
> Quando si configura l'URL esterno e l'URL del server back-end, assicurarsi di includere il nome di dominio completo e non un indirizzo IP.  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1.1"></a>Pubblicare un'applicazione basata su attestazioni per i client del Web Browser  
Per pubblicare un'applicazione che utilizza le attestazioni per l'autenticazione, è necessario aggiungere al servizio federativo un trust della relying party per l'applicazione.  
  
Quando si pubblicano applicazioni basate sulle attestazioni e si accede alle applicazioni da un browser, il flusso di autenticazione generale è il seguente:  
  
1.  Il client tenta di accedere a un'applicazione basata sulle attestazioni tramite un web browser; ad esempio, https://appserver.contoso.com/claimapp/.  
  
2.  Il web browser invia una richiesta HTTPS al server Proxy applicazione Web che reindirizza la richiesta al server AD FS.  
  
3.  Il server AD FS autentica l'utente e il dispositivo e reindirizza la richiesta al Proxy applicazione Web. La richiesta contiene ora il token perimetrale. Server AD FS aggiunge un unico sign-on (SSO) cookie alla richiesta perché l'utente ha già eseguito l'autenticazione con il server AD FS.  
  
4.  Proxy applicazione Web convalida il token, aggiunge il proprio cookie e inoltra la richiesta al server back-end.  
  
5.  Il server back-end reindirizza la richiesta al server AD FS per ottenere un token di sicurezza delle applicazioni.  
  
6.  La richiesta viene reindirizzata al server back-end dal server AD FS. La richiesta contiene ora il token dell'applicazione e il cookie SSO. L'utente può accedere all'applicazione senza immettere un nome utente e una password.  
  
Questa procedura descrive come pubblicare un'applicazione basata su attestazioni, ad esempio un sito di SharePoint, a cui i client del Web browser accederanno. Prima di iniziare, assicurarsi di avere eseguito le operazioni seguenti:  
  
-   Creazione di un trust della relying party per l'applicazione nella console di gestione di AD FS.  
  
-   Verificare che un certificato del server Proxy applicazione Web è adatto per l'applicazione che si desidera pubblicare.  
  

  
#### <a name="to-publish-a-claims-based-application"></a>Per pubblicare un'applicazione basata su attestazioni  
  
1.  Nel server Proxy applicazione Web, nella console di gestione accesso remoto, nel **navigazione** riquadro, fare clic su **Proxy applicazione Web**e quindi nel **attività** riquadro, fare clic su **Pubblicare**.  
  
2.  Nella **pagina iniziale** della **Pubblicazione guidata nuova applicazione** fare clic su **Avanti**.  
  
3.  Nel **la preautenticazione** pagina, fare clic su **Active Directory Federation Services (ADFS)** e quindi fare clic su **Avanti**.  
  
4.  Nella pagina dei **client supportati** selezionare **Web e MSOFBA**e quindi fare clic su **Avanti**.  
  
5.  Nell'elenco di relying party della pagina **Relying party** selezionare la relying party per l'applicazione che si desidera pubblicare e quindi fare clic su **Avanti**.  
  
6.  Nella pagina **Impostazioni di pubblicazione** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Nella casella **Nome** immettere un nome descrittivo per l'applicazione.  
  
        Questo nome viene utilizzato solo nell'elenco di applicazioni pubblicate nella console Gestione accesso remoto.  
  
    -   Nella casella **URL esterno** immettere l'URL esterno per l'applicazione, ad esempio https://sp.contoso.com/app1/.  
  
    -   Nell'elenco **Certificato esterno** selezionare un certificato il cui soggetto includa l'URL esterno.  
  
    -   Nella casella **URL server back-end** immettere l'URL del server back-end. Si noti che questo valore viene immesso automaticamente quando si immette l'URL esterno ed è consigliabile modificarlo solo se l'URL del server back-end è diverso. ad esempio, https://sp/app1/.  
  
        > [!NOTE]  
        > Proxy applicazione Web può convertire nomi host in URL, ma non è possibile convertire nomi di percorso. È pertanto possibile immettere nomi host diversi, ma è necessario immettere lo stesso nome di percorso. Ad esempio, è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://app-server/app1/. Tuttavia, è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://apps.contoso.com/internal-app1/.  
  
7.  Nella pagina **Conferma** verificare le impostazioni e quindi fare clic su **Pubblica**. È possibile copiare il comando di PowerShell per configurare altre applicazioni pubblicate.  
  
8.  Nella pagina **Risultati** verificare che l'applicazione sia stata pubblicata correttamente e quindi fare clic su **Chiudi**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://sp.contoso.com/app1/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://sp.contoso.com/app1/'  
    -Name 'SP'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'SP_Relying_Party'  
```  
  
## <a name="BKMK_1.2"></a>Pubblicare un'applicazione basata sull'autenticazione Windows integrata per i client del Web Browser  
Proxy applicazione Web utilizzabile per pubblicare applicazioni che usano l'autenticazione Windows integrata. Proxy applicazione Web, ovvero esegue la preautenticazione come richiesto e può quindi eseguire SSO nell'applicazione pubblicata che utilizza l'autenticazione Windows integrata. Per pubblicare un'applicazione che utilizza l'autenticazione integrata di Windows, è necessario aggiungere al servizio federativo un trust della relying party non in grado di riconoscere attestazioni per l'applicazione.  
  
Per consentire il Proxy applicazione Web per eseguire il single sign-on (SSO) e la delega delle credenziali Kerberos tramite la delega vincolata, il Proxy applicazione Web server deve appartenere a un dominio. Visualizzare [pianificare Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Per consentire agli utenti di accedere alle applicazioni che usano l'autenticazione Windows integrata, il server Proxy applicazione Web deve essere in grado di fornire la delega per gli utenti all'applicazione pubblicata. È possibile eseguire questa operazione nel controller di dominio per ogni applicazione È anche possibile eseguire nel server back-end se è in esecuzione in Windows Server 2012 R2 o Windows Server 2012. Per altre informazioni, vedere [Novità dell'autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx).  
  
Per una procedura dettagliata per configurare il Proxy applicazione Web per pubblicare un'applicazione usando l'autenticazione Windows integrata, vedere [configurare un sito per usare l'autenticazione Windows integrata](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3).  
  
Quando si usa l'autenticazione di Windows integrata ai server back-end, l'autenticazione del Proxy applicazione Web e l'applicazione pubblicata non è basata sulle attestazioni, Usa invece la delega vincolata Kerberos per autenticare gli utenti finali nell'applicazione. Il flusso generale è quello descritto di seguito:  
  
1.  Il client tenta di accedere a un'applicazione non basata su attestazioni usando un browser web. ad esempio, https://appserver.contoso.com/nonclaimapp/.  
  
2.  Il web browser invia una richiesta HTTPS al server Proxy applicazione Web che reindirizza la richiesta al server AD FS.  
  
3.  Il server AD FS autentica l'utente e reindirizza la richiesta al Proxy applicazione Web. La richiesta contiene ora il token perimetrale.  
  
4.  Proxy applicazione Web convalida il token.  
  
5.  Se il token è valido, Proxy applicazione Web Ottiene un ticket Kerberos dal controller di dominio per conto dell'utente.  
  
6.  Proxy applicazione Web aggiunge il ticket Kerberos alla richiesta come parte del token Simple and Protected GSS-API Negotiation meccanismo (SPNEGO) e inoltra la richiesta al server back-end. Poiché la richiesta contiene il ticket Kerberos, l'utente può accedere all'applicazione senza che siano necessari ulteriori passaggi di autenticazione.  
  
Questa procedura descrive come pubblicare un'applicazione che utilizza l'autenticazione integrata di Windows, ad esempio Outlook Web App, a cui i client del Web browser accederanno. Prima di iniziare, assicurarsi di avere eseguito le operazioni seguenti:  
  
-   Creato un trust della relying party non riconoscere attestazioni per l'applicazione nella console di gestione di AD FS.  
  
-   Configurare il server back-end per il supporto della delega vincolata Kerberos nel controller di dominio oppure utilizzando il cmdlet Set-ADUser con il parametro -PrincipalsAllowedToDelegateToAccount. Si noti che se il server back-end è in esecuzione in Windows Server 2012 R2 o Windows Server 2012, può anche eseguire questo comando di PowerShell nel server back-end.  
  
-   Assicurati che i server Proxy applicazione Web sono configurati per la delega ai nomi dell'entità servizio dei server back-end.  
  
-   Verificare che un certificato del server Proxy applicazione Web è adatto per l'applicazione che si desidera pubblicare.  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>Per pubblicare un'applicazione non basata su attestazioni  
  
1.  Nel server Proxy applicazione Web, nella console di gestione accesso remoto, nel **navigazione** riquadro, fare clic su **Proxy applicazione Web**e quindi nel **attività** riquadro, fare clic su **Pubblicare**.  
  
2.  Nella **pagina iniziale** della **Pubblicazione guidata nuova applicazione** fare clic su **Avanti**.  
  
3.  Nel **la preautenticazione** pagina, fare clic su **Active Directory Federation Services (ADFS)** e quindi fare clic su **Avanti**.  
  
4.  Nella pagina dei **client supportati** selezionare **Web e MSOFBA**e quindi fare clic su **Avanti**.  
  
5.  Nell'elenco di relying party della pagina **Relying party** selezionare la relying party per l'applicazione che si desidera pubblicare e quindi fare clic su **Avanti**.  
  
6.  Nella pagina **Impostazioni di pubblicazione** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Nella casella **Nome** immettere un nome descrittivo per l'applicazione.  
  
        Questo nome viene utilizzato solo nell'elenco di applicazioni pubblicate nella console Gestione accesso remoto.  
  
    -   Nella casella **URL esterno** immettere l'URL esterno per l'applicazione, ad esempio https://owa.contoso.com/.  
  
    -   Nell'elenco **Certificato esterno** selezionare un certificato il cui soggetto includa l'URL esterno.  
  
    -   Nella casella **URL server back-end** immettere l'URL del server back-end. Si noti che questo valore viene immesso automaticamente quando si immette l'URL esterno ed è consigliabile modificarlo solo se l'URL del server back-end è diverso. ad esempio, https://owa/.  
  
        > [!NOTE]  
        > Proxy applicazione Web può convertire nomi host in URL, ma non è possibile convertire nomi di percorso. È pertanto possibile immettere nomi host diversi, ma è necessario immettere lo stesso nome di percorso. Ad esempio, è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://app-server/app1/. Tuttavia, è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://apps.contoso.com/internal-app1/.  
  
    -   Nella casella **SPN server back-end** immettere il nome dell'entità servizio per il server back-end, ad esempio HTTP/owa.contoso.com.  
  
7.  Nella pagina **Conferma** verificare le impostazioni e quindi fare clic su **Pubblica**. È possibile copiare il comando di PowerShell per configurare altre applicazioni pubblicate.  
  
8.  Nella pagina **Risultati** verificare che l'applicazione sia stata pubblicata correttamente e quindi fare clic su **Chiudi**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerAuthenticationSpn 'HTTP/owa.contoso.com'  
    -BackendServerURL 'https://owa.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://owa.contoso.com/'  
    -Name 'OWA'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Non-Claims_Relying_Party'  
```  
  
## <a name="BKMK_1.3"></a>Pubblicare un'applicazione che viene utilizzato MS-OFBA  
Proxy applicazione Web supporta l'accesso dai client di Microsoft Office, ad esempio Microsoft Word che accede a documenti e dati nei server back-end. L'unica differenza tra queste applicazioni e un browser standard è che il reindirizzamento al servizio token di sicurezza viene eseguito non mediante il reindirizzamento standard HTTP ma con intestazioni, come specificato nel MS-OFBA: [ https://msdn.microsoft.com/library/dd773463(v=office.12).aspx ](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx). L'applicazione back-end può essere basata sulle attestazioni o sull'autenticazione integrata di Windows.   
Per pubblicare un'applicazione per i client che usano MS-OFBA, è necessario aggiungere un trust della relying party per l'applicazione al servizio federativo. A seconda dell'applicazione, è possibile utilizzare l'autenticazione basata sulle attestazioni oppure l'autenticazione integrata di Windows. È pertanto necessario aggiungere il trust della relying party pertinente in base all'applicazione.  
  
Per consentire il Proxy applicazione Web per eseguire il single sign-on (SSO) e la delega delle credenziali Kerberos tramite la delega vincolata, il Proxy applicazione Web server deve appartenere a un dominio. Visualizzare [pianificare Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Se l'applicazione usa l'autenticazione basata su attestazioni non sono necessarie altre fase di pianificazione. Se l'applicazione usa l'autenticazione Windows integrata, vedere [pubblicare un'applicazione basata sull'autenticazione Windows integrata per i client del Web Browser](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2).  
  
Di seguito viene descritto il flusso di autenticazione per i client che usano il protocollo MS-OFBA usando l'autenticazione basata su attestazioni. L'autenticazione per questo scenario può utilizzare il token dell'applicazione nell'URL oppure nel corpo.  
  
1.  L'utente sta utilizzando un'applicazione di Office e dall'elenco **Documenti recenti** apre un file in un sito di SharePoint.  
  
2.  Nell'applicazione di Office viene visualizzata una finestra con un controllo browser che richiede l'immissione di credenziali.  
  
    > [!NOTE]  
    > In alcuni casi è possibile che la finestra non venga visualizzata perché il client è già autenticato.  
  
3.  Proxy applicazione Web reindirizza la richiesta al server ADFS, che esegue l'autenticazione.  
  
4.  Il server AD FS reindirizza la richiesta al Proxy applicazione Web. La richiesta contiene ora il token perimetrale.  
  
5.  Server AD FS aggiunge un unico sign-on (SSO) cookie alla richiesta perché l'utente ha già eseguito l'autenticazione con il server AD FS.  
  
6.  Proxy applicazione Web convalida il token e inoltra la richiesta al server back-end.  
  
7.  Il server back-end reindirizza la richiesta al server AD FS per ottenere un token di sicurezza delle applicazioni.  
  
8.  La richiesta viene reindirizzata al server back-end. La richiesta contiene ora il token dell'applicazione e il cookie SSO. L'utente può accedere al sito di SharePoint senza immettere un nome utente e una password per visualizzare il file.  
  
I passaggi per pubblicare un'applicazione che utilizza MS-OFBA sono identici a quelli per un'applicazione basata su attestazioni o un'applicazione non basata su attestazioni. Per le applicazioni basate su attestazioni, vedere [pubblicare un'applicazione basata su attestazioni per i client del Web Browser](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1), per le applicazioni non basate su attestazioni, vedere [pubblicare un'applicazione basata sull'autenticazione Windows integrata per il Web I client del browser](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2). Proxy applicazione Web rileva automaticamente il client ed eseguirà l'autenticazione dell'utente come richiesto.  
  
## <a name="publish-an-application-that-uses-http-basic"></a>Pubblicare un'applicazione che utilizza HTTP di base  

Base HTTP è il protocollo di autorizzazione utilizzato da numerosi protocolli, per la connessione rich client, tra cui Smartphone, la cassetta postale di Exchange. Per altre informazioni su HTTP di base, vedere [RFC 2617](https://www.ietf.org/rfc/rfc2617.txt). Proxy applicazione Web in genere interagisce con AD FS mediante reindirizzamenti; la maggior parte dei rich client non supporta i cookie o la gestione dello stato. In questo modo Proxy applicazione Web consente all'app HTTP di ricevere un non-attestazioni trust della relying party per l'applicazione al servizio federativo. Visualizzare [pianificare Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Il flusso di autenticazione per i client che usano HTTP di base è descritte di seguito e nella figura seguente:  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  L'utente tenta di accedere a un'applicazione web pubblicata un client di telefono.  
  
2.  L'app invia una richiesta HTTPS all'URL pubblicato dal Proxy applicazione Web.  
  
3.  Se la richiesta non contiene le credenziali, Proxy applicazione Web restituisce una risposta HTTP 401 all'app contenente l'URL del server di autenticazione AD FS.  
  
4.  L'utente invia la richiesta HTTPS all'app anche in questo caso con autorizzazione impostato su Basic e il nome utente e in Base 64 password crittografata dell'utente nel World Wide Web-intestazione di richiesta di autenticazione.  
  
5.  Poiché il dispositivo non può essere reindirizzato ad ADFS, il Proxy applicazione Web invia una richiesta di autenticazione ad ADFS con le credenziali che siano inclusi il nome utente e password. È necessario acquisire il token per conto del dispositivo.  
  
6.  Per ridurre al minimo il numero di richieste inviate da AD FS, Proxy applicazione Web, convalida le richieste client successive usando i token memorizzati nella cache per, purché il token è valido. Proxy applicazione Web pulisce periodicamente la cache. È possibile visualizzare le dimensioni della cache mediante il contatore delle prestazioni.  
  
7.  Se il token è valido, Proxy applicazione Web inoltra la richiesta al server back-end e l'utente viene concesso l'accesso all'applicazione web pubblicata.  
  
La procedura seguente illustra come pubblicare le applicazioni di base HTTP.  
  
#### <a name="to-publish-an-http-basic-application"></a>Per pubblicare un'applicazione di base HTTP  
  
1.  Nel server Proxy applicazione Web, nella console di gestione accesso remoto, nel **navigazione** riquadro, fare clic su **Proxy applicazione Web**e quindi nel **attività** riquadro, fare clic su **Pubblicare**.  
  
2.  Nella **pagina iniziale** della **Pubblicazione guidata nuova applicazione** fare clic su **Avanti**.  
  
3.  Nel **la preautenticazione** pagina, fare clic su **Active Directory Federation Services (ADFS)** e quindi fare clic su **Avanti**.  
  
4.  Nel **client supportati** pagina, selezionare **HTTP di base** e quindi fare clic su **Next**.  
  
    Se si desidera abilitare l'accesso di Exchange solo dai dispositivi all'area di lavoro, selezionare la **abilitare l'accesso all'area di lavoro solo per i dispositivi aggiunti all'identità** casella. Per altre informazioni, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tra le applicazioni aziendali](https://technet.microsoft.com/library/dn280945.aspx).  
  
5.  Nell'elenco di relying party della pagina **Relying party** selezionare la relying party per l'applicazione che si desidera pubblicare e quindi fare clic su **Avanti**. Si noti che questo elenco contiene solo su basato su attestazioni relying party.  
  
6.  Nella pagina **Impostazioni di pubblicazione** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Nella casella **Nome** immettere un nome descrittivo per l'applicazione.  
  
        Questo nome viene utilizzato solo nell'elenco di applicazioni pubblicate nella console Gestione accesso remoto.  
  
    -   Nel **URL esterno** immettere l'URL esterno per l'applicazione, ad esempio mail.contoso.com  
  
    -   Nell'elenco **Certificato esterno** selezionare un certificato il cui soggetto includa l'URL esterno.  
  
    -   Nella casella **URL server back-end** immettere l'URL del server back-end. Si noti che questo valore viene immesso automaticamente quando si immette l'URL esterno ed è consigliabile modificarlo solo se l'URL del server back-end è diverso. ad esempio mail.contoso.com.  
  
7.  Nella pagina **Conferma** verificare le impostazioni e quindi fare clic su **Pubblica**. È possibile copiare il comando di PowerShell per configurare altre applicazioni pubblicate.  
  
8.  Nella pagina **Risultati** verificare che l'applicazione sia stata pubblicata correttamente e quindi fare clic su **Chiudi**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Questo script di Windows PowerShell consente la preautenticazione per tutti i dispositivi, non solo i dispositivi all'area di lavoro.  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
Di seguito Preautentica solo dispositivi con workplace join:  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -EnableHTTPRedirect:$true   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
## <a name="BKMK_1.4"></a>Pubblicare un'applicazione che usa OAuth2, ad esempio un'App di Microsoft Store  
Per pubblicare un'applicazione per le app di Microsoft Store, è necessario aggiungere un trust della relying party per l'applicazione al servizio federativo.  
  
Per consentire il Proxy applicazione Web per eseguire il single sign-on (SSO) e la delega delle credenziali Kerberos tramite la delega vincolata, il Proxy applicazione Web server deve appartenere a un dominio. Visualizzare [pianificare Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
> [!NOTE]  
> Proxy applicazione Web supporta la pubblicazione solo per le app di Microsoft Store che usano il protocollo OAuth 2.0.  
  
Nella console di gestione di ADFS, è necessario assicurarsi che l'endpoint OAuth sia abilitato per il proxy. A tale scopo, aprire la console di gestione di ADFS, espandere **Servizio**e fare clic su **Endpoint**. Nell'elenco **Endpoint** individuare l'endpoint OAuth e verificare che il valore nella colonna **Proxy abilitato** sia **Sì**.  
  
Il flusso di autenticazione per i client che usano le app di Microsoft Store è il seguente:  
  
> [!NOTE]  
> Il Proxy applicazione Web reindirizza al server AD FS per l'autenticazione. Poiché le app di Microsoft Store non supportano il reindirizzamento, se si usano le app di Microsoft Store, è necessario impostare l'URL del server ADFS utilizzando il cmdlet Set-WebApplicationProxyConfiguration e il parametro OAuthAuthenticationURL.  
>   
> È possibile pubblicare le app di Microsoft Store solo tramite Windows PowerShell.  
  
1.  Il client tenta di accedere a un'applicazione web pubblicata usando un'app di Microsoft Store.  
  
2.  L'app invia una richiesta HTTPS all'URL pubblicato dal Proxy applicazione Web.  
  
3.  Proxy applicazione Web restituisce una risposta HTTP 401 all'app contenente l'URL del server di autenticazione AD FS. Questo processo è noto come "individuazione".  
  
    > [!NOTE]  
    > Se l'app conosce l'URL del server di autenticazione AD FS e si dispone già di un token combinato contenente il token OAuth e il token perimetrale, i passaggi 2 e 3 vengono ignorati in questo flusso di autenticazione.  
  
4.  L'app invia una richiesta HTTPS al server AD FS.  
  
5.  L'app Usa il gestore delle autenticazioni web per generare una finestra di dialogo in cui l'utente immette le credenziali per l'autenticazione al server AD FS. Per informazioni sul gestore delle autenticazioni Web, vedere [Gestore di autenticazioni Web](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx).  
  
6.  Al termine dell'autenticazione, il server ADFS crea un token combinato contenente il token OAuth e il token perimetrale e invia il token all'app.  
  
7.  L'app invia una richiesta HTTPS contenente il token combinato all'URL pubblicato dal Proxy applicazione Web.  
  
8.  Proxy applicazione Web divide il token combinato nelle due parti componenti e convalida il token perimetrale.  
  
9. Se il token perimetrale è valido, Proxy applicazione Web inoltra la richiesta al server back-end con solo il token OAuth. L'utente può accedere all'applicazione Web pubblicata.  
  
Questa procedura descrive come pubblicare un'applicazione per OAuth2. Questo tipo di applicazione può essere pubblicato solo tramite Windows PowerShell. Prima di iniziare, assicurarsi di avere eseguito le operazioni seguenti:  
  
-   Creazione di un trust della relying party per l'applicazione nella console di gestione di AD FS.  
  
-   Assicurati che l'endpoint OAuth sia abilitato nella console di gestione di ADFS per il proxy e prendere nota del percorso dell'URL.  
  
-   Verificare che un certificato del server Proxy applicazione Web è adatto per l'applicazione che si desidera pubblicare.  
  
#### <a name="to-publish-an-oauth2-app"></a>Per pubblicare un'app OAuth2  
  
1.  Nel server Proxy applicazione Web, nella console di gestione accesso remoto, nel **navigazione** riquadro, fare clic su **Proxy applicazione Web**e quindi nel **attività** riquadro, fare clic su **Pubblicare**.  
  
2.  Nella **pagina iniziale** della **Pubblicazione guidata nuova applicazione** fare clic su **Avanti**.  
  
3.  Nel **la preautenticazione** pagina, fare clic su **Active Directory Federation Services (ADFS)** e quindi fare clic su **Avanti**.  
  
4.  Nel **client supportati** pagina, selezionare **OAuth2** e quindi fare clic su **Next**.  
  
5.  Nell'elenco di relying party della pagina **Relying party** selezionare la relying party per l'applicazione che si desidera pubblicare e quindi fare clic su **Avanti**.  
  
6.  Nella pagina **Impostazioni di pubblicazione** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Nella casella **Nome** immettere un nome descrittivo per l'applicazione.  
  
        Questo nome viene utilizzato solo nell'elenco di applicazioni pubblicate nella console Gestione accesso remoto.  
  
    -   Nella casella **URL esterno** immettere l'URL esterno per l'applicazione, ad esempio https://server1.contoso.com/app1/.  
  
    -   Nell'elenco **Certificato esterno** selezionare un certificato il cui soggetto includa l'URL esterno.  
  
        Per assicurarsi che gli utenti possono accedere all'app, anche se essi trascura digitare l'URL HTTPS, selezionare la **abilitare HTTP per il reindirizzamento HTTPS** casella.  
  
    -   Nella casella **URL server back-end** immettere l'URL del server back-end. Si noti che questo valore viene immesso automaticamente quando si immette l'URL esterno ed è consigliabile modificarlo solo se l'URL del server back-end è diverso. ad esempio, https://sp/app1/.  
  
        > [!NOTE]  
        > Proxy applicazione Web può convertire nomi host in URL, ma non è possibile convertire nomi di percorso. È pertanto possibile immettere nomi host diversi, ma è necessario immettere lo stesso nome di percorso. Ad esempio, è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://app-server/app1/. Tuttavia, è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://apps.contoso.com/internal-app1/.  
  
7.  Nella pagina **Conferma** verificare le impostazioni e quindi fare clic su **Pubblica**. È possibile copiare il comando di PowerShell per configurare altre applicazioni pubblicate.  
  
8.  Nella pagina **Risultati** verificare che l'applicazione sia stata pubblicata correttamente e quindi fare clic su **Chiudi**.  
  
Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Indirizzo di fs.contoso.com e un percorso URL ADFS/oauth2/ oauth2 /, impostare l'URL di autenticazione OAuth per un server federativo:  
  
```  
Set-WebApplicationProxyConfiguration -OAuthAuthenticationURL 'https://fs.contoso.com/adfs/oauth2/'  
```  
  
Per pubblicare l'applicazione:  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://storeapp.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://storeapp.contoso.com/'  
    -Name 'Microsoft Store app Server'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Store_app_Relying_Party'  
    -UseOAuthAuthentication  
```  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Risoluzione dei problemi di Proxy applicazione Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   [Pubblicare applicazioni mediante Proxy applicazione Web](https://technet.microsoft.com/library/dn383659.aspx)  
  
-   [Prevede di pubblicare applicazioni mediante Proxy applicazione Web](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Guida allo scenario di Web Application Proxy](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Cmdlet del Proxy applicazione Web in Windows PowerShell](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Add-WebApplicationProxyApplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-WebApplicationProxyConfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  


