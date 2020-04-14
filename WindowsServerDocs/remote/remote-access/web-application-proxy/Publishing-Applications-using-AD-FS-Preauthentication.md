---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: Pubblicazione delle applicazioni con la preautenticazione di AD FS
ms.author: kgremban
author: eross-msft
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: 97bfae42c873ecf7196138920a21d96714239da9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818704"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>Pubblicazione delle applicazioni con la preautenticazione di AD FS

>Si applica a: Windows Server 2016

**Questo contenuto è pertinente per la versione locale del proxy applicazione Web. Per abilitare l'accesso sicuro alle applicazioni locali tramite il cloud, vedere il [Azure ad contenuto del proxy di applicazione](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Questo argomento descrive come pubblicare applicazioni tramite il proxy di applicazione Web usando la preautenticazione Active Directory Federation Services (AD FS).  
  
Per tutti i tipi di applicazione che è possibile pubblicare utilizzando la preautenticazione di AD FS, è necessario aggiungere una AD FS relying party trust al Servizio federativo.  
  
Il flusso di preautenticazione AD FS generale è il seguente:  
  
> [!NOTE]  
> Questo flusso di autenticazione non è applicabile per i client che usano app Microsoft Store.  
  
1.  Il dispositivo client tenta di accedere a un'applicazione Web pubblicata in un URL di risorsa specifico. ad esempio https://app1.contoso.com/.  
  
    L'URL della risorsa è un indirizzo pubblico in cui il proxy dell'applicazione Web è in ascolto per le richieste HTTPS in ingresso.  
  
    Se è abilitato il Reindirizzamento da HTTP a HTTPS, il proxy dell'applicazione Web sarà in ascolto anche delle richieste HTTP in ingresso.  
  
2.  Proxy applicazione Web reindirizza la richiesta HTTPS al server AD FS con i parametri codificati URL, inclusi l'URL della risorsa e il Apprealm ovvero (un identificatore di relying party).  
  
    L'utente esegue l'autenticazione usando il metodo di autenticazione richiesto dal server AD FS; ad esempio nome utente e password, autenticazione a due fattori con password monouso e così via.  
  
3.  Dopo l'autenticazione dell'utente, il server di AD FS rilascia un token di sicurezza, ovvero il "token perimetrale", contenente le informazioni seguenti e reindirizza la richiesta HTTPS al server proxy applicazione Web:  
  
    -   L'identificativo della risorsa a cui l'utente ha tentato di accedere.  
  
    -   Identità dell'utente come nome dell'entità utente (UPN).  
  
    -   La scadenza dell'accesso concesso all'utente. L'utente può infatti accedere per un periodo di tempo limitato, al termine del quale deve eseguire nuovamente l'autenticazione.  
  
    -   La firma delle informazioni nel token perimetrale.  
  
4.  Il proxy dell'applicazione Web riceve la richiesta HTTPS reindirizzata dal server di AD FS con il token perimetrale e convalida e utilizza il token nel modo seguente:  
  
    -   Verifica che la firma del token perimetrale provenga dal servizio federativo configurato nella configurazione del proxy dell'applicazione Web.  
  
    -   Verifica che il token sia stato emesso per l'applicazione corretta.  
  
    -   Verifica che il token non sia scaduto.  
  
    -   Utilizza l'identità utente quando necessario, ad esempio per ottenere un ticket Kerberos se il server back-end è configurato per l'utilizzo dell'autenticazione integrata di Windows.  
  
5.  Se il token perimetrale è valido, il proxy dell'applicazione Web Invia la richiesta HTTPS all'applicazione Web pubblicata utilizzando HTTP o HTTPS.  
  
6.  Il client può quindi accedere all'applicazione Web pubblicata, che tuttavia può essere configurata in modo da richiedere all'utente l'esecuzione di passaggi di autenticazione aggiuntivi. Se ad esempio l'applicazione Web pubblicata è un sito di SharePoint che non richiede passaggi di autenticazione aggiuntivi, tale sito verrà visualizzato nel browser dell'utente.  
  
7.  Il proxy dell'applicazione Web Salva un cookie nel dispositivo client. Il cookie viene utilizzato dal proxy dell'applicazione Web per identificare che questa sessione è già stata preautenticata e che non è necessaria alcuna preautenticazione aggiuntiva.  
  
> [!IMPORTANT]  
> Quando si configura l'URL esterno e l'URL del server back-end, assicurarsi di includere il nome di dominio completo e non un indirizzo IP.  
  
> [!NOTE]  
> In questo argomento sono inclusi cmdlet di Windows PowerShell di esempio che possono essere usati per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="publish-a-claims-based-application-for-web-browser-clients"></a><a name="BKMK_1.1"></a>Pubblicare un'applicazione basata sulle attestazioni per i client del Web browser  
Per pubblicare un'applicazione che utilizza le attestazioni per l'autenticazione, è necessario aggiungere al servizio federativo un trust della relying party per l'applicazione.  
  
Quando si pubblicano applicazioni basate sulle attestazioni e si accede alle applicazioni da un browser, il flusso di autenticazione generale è il seguente:  
  
1.  Il client tenta di accedere a un'applicazione basata sulle attestazioni mediante un Web browser. ad esempio, https://appserver.contoso.com/claimapp/.  
  
2.  Il Web browser invia una richiesta HTTPS al server proxy applicazione Web che reindirizza la richiesta al server AD FS.  
  
3.  Il server di AD FS esegue l'autenticazione dell'utente e del dispositivo e reindirizza la richiesta al proxy dell'applicazione Web. La richiesta contiene ora il token perimetrale. Il server di AD FS aggiunge un cookie di Single Sign-On (SSO) alla richiesta perché l'utente ha già eseguito l'autenticazione nel server AD FS.  
  
4.  Il proxy dell'applicazione Web convalida il token, aggiunge il proprio cookie e trasmette la richiesta al server back-end.  
  
5.  Il server back-end reindirizza la richiesta al server AD FS per ottenere il token di sicurezza dell'applicazione.  
  
6.  La richiesta viene reindirizzata al server back-end dal server AD FS. La richiesta contiene ora il token dell'applicazione e il cookie SSO. L'utente può accedere all'applicazione senza immettere un nome utente e una password.  
  
Questa procedura descrive come pubblicare un'applicazione basata su attestazioni, ad esempio un sito di SharePoint, a cui i client del Web browser accederanno. Prima di iniziare, assicurarsi di avere eseguito le operazioni seguenti:  
  
-   Creazione di un trust relying party per l'applicazione nella console di gestione AD FS.  
  
-   Verificare che un certificato nel server proxy applicazione Web sia adatto all'applicazione che si desidera pubblicare.  
  

  
#### <a name="to-publish-a-claims-based-application"></a>Per pubblicare un'applicazione basata su attestazioni  
  
1.  Nel server proxy applicazione Web, nel riquadro di **spostamento** della console di gestione accesso remoto, fare clic su **proxy applicazione Web**, quindi nel riquadro **attività** fare clic su **pubblica**.  
  
2.  Nella **pagina iniziale** della **Pubblicazione guidata nuova applicazione** fare clic su **Avanti**.  
  
3.  Nella pagina **preautenticazione** fare clic su **Active Directory Federation Services (ad FS)** , quindi fare clic su **Avanti**.  
  
4.  Nella pagina dei **client supportati** selezionare **Web e MSOFBA** e quindi fare clic su **Avanti**.  
  
5.  Nell'elenco di relying party della pagina **Relying party** selezionare la relying party per l'applicazione che si desidera pubblicare e quindi fare clic su **Avanti**.  
  
6.  Nella pagina **Impostazioni di pubblicazione** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Nella casella **Nome** immettere un nome descrittivo per l'applicazione.  
  
        Questo nome viene utilizzato solo nell'elenco di applicazioni pubblicate nella console Gestione accesso remoto.  
  
    -   Nella casella **URL esterno** immettere l'URL esterno per l'applicazione, ad esempio https://sp.contoso.com/app1/.  
  
    -   Nell'elenco **Certificato esterno** selezionare un certificato il cui soggetto includa l'URL esterno.  
  
    -   Nella casella **URL server back-end** immettere l'URL del server back-end. Si noti che questo valore viene immesso automaticamente quando si immette l'URL esterno ed è consigliabile modificarlo solo se l'URL del server back-end è diverso. ad esempio, https://sp/app1/.  
  
        > [!NOTE]  
        > Il proxy dell'applicazione Web può convertire i nomi host in URL, ma non può convertire i nomi di percorso. È pertanto possibile immettere nomi host diversi, ma è necessario immettere lo stesso nome di percorso. Ad esempio, è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://app-server/app1/. Tuttavia, non è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://apps.contoso.com/internal-app1/.  
  
7.  Nella pagina **Conferma** verificare le impostazioni e quindi fare clic su **Pubblica**. È possibile copiare il comando di PowerShell per configurare altre applicazioni pubblicate.  
  
8.  Nella pagina **Risultati** verificare che l'applicazione sia stata pubblicata correttamente e quindi fare clic su **Chiudi**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif) ***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://sp.contoso.com/app1/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://sp.contoso.com/app1/'  
    -Name 'SP'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'SP_Relying_Party'  
```  
  
## <a name="publish-an-integrated-windows-authenticated-based-application-for-web-browser-clients"></a><a name="BKMK_1.2"></a>Pubblicare un'applicazione basata sull'autenticazione integrata di Windows per i client del Web browser  
Il proxy dell'applicazione Web può essere utilizzato per pubblicare applicazioni che utilizzano l'autenticazione integrata di Windows. Questo significa che proxy applicazione Web esegue la preautenticazione come richiesto e può quindi eseguire l'accesso SSO all'applicazione pubblicata che utilizza l'autenticazione integrata di Windows. Per pubblicare un'applicazione che utilizza l'autenticazione integrata di Windows, è necessario aggiungere al servizio federativo un trust della relying party non in grado di riconoscere attestazioni per l'applicazione.  
  
Per consentire al proxy applicazione Web di eseguire Single Sign-On (SSO) e di eseguire la delega delle credenziali utilizzando la delega vincolata Kerberos, il server proxy applicazione Web deve essere aggiunto a un dominio. Vedere [pianificare Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Per consentire agli utenti di accedere alle applicazioni che utilizzano l'autenticazione integrata di Windows, il server proxy applicazione Web deve essere in grado di fornire la delega per gli utenti all'applicazione pubblicata. È possibile eseguire questa operazione nel controller di dominio per ogni applicazione È anche possibile eseguire questa operazione nel server back-end se è in esecuzione in Windows Server 2012 R2 o Windows Server 2012. Per altre informazioni, vedere [Novità dell'autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx).  
  
Per una procedura dettagliata sulla configurazione del proxy dell'applicazione Web per la pubblicazione di un'applicazione con l'autenticazione integrata di Windows, vedere [configurare un sito per l'utilizzo dell'autenticazione integrata di Windows](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3).  
  
Quando si usa l'autenticazione integrata di Windows per i server back-end, l'autenticazione tra il proxy applicazione Web e l'applicazione pubblicata non è basata sulle attestazioni, ma usa la delega vincolata Kerberos per autenticare gli utenti finali nell'applicazione. Il flusso generale è quello descritto di seguito:  
  
1.  Il client tenta di accedere a un'applicazione non basata su attestazioni mediante un Web browser. ad esempio, https://appserver.contoso.com/nonclaimapp/.  
  
2.  Il Web browser invia una richiesta HTTPS al server proxy applicazione Web che reindirizza la richiesta al server AD FS.  
  
3.  Il server di AD FS esegue l'autenticazione dell'utente e reindirizza la richiesta al proxy dell'applicazione Web. La richiesta contiene ora il token perimetrale.  
  
4.  Il proxy dell'applicazione Web convalida il token.  
  
5.  Se il token è valido, il proxy dell'applicazione Web ottiene un ticket Kerberos dal controller di dominio per conto dell'utente.  
  
6.  Il proxy dell'applicazione Web aggiunge il ticket Kerberos alla richiesta come parte del token SPNEGO (Simple and Protected GSS-API Negotiation Mechanism) e Invia la richiesta al server back-end. Poiché la richiesta contiene il ticket Kerberos, l'utente può accedere all'applicazione senza che siano necessari ulteriori passaggi di autenticazione.  
  
Questa procedura descrive come pubblicare un'applicazione che utilizza l'autenticazione integrata di Windows, ad esempio Outlook Web App, a cui i client del Web browser accederanno. Prima di iniziare, assicurarsi di avere eseguito le operazioni seguenti:  
  
-   Creazione di un trust relying party non in grado di riconoscere attestazioni per l'applicazione nella console di gestione AD FS.  
  
-   Configurare il server back-end per il supporto della delega vincolata Kerberos nel controller di dominio oppure utilizzando il cmdlet Set-ADUser con il parametro -PrincipalsAllowedToDelegateToAccount. Si noti che se il server back-end è in esecuzione in Windows Server 2012 R2 o Windows Server 2012, è possibile eseguire questo comando di PowerShell anche nel server back-end.  
  
-   Assicurarsi che i server proxy applicazione Web siano configurati per la delega ai nomi dell'entità servizio dei server back-end.  
  
-   Verificare che un certificato nel server proxy applicazione Web sia adatto all'applicazione che si desidera pubblicare.  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>Per pubblicare un'applicazione non basata su attestazioni  
  
1.  Nel server proxy applicazione Web, nel riquadro di **spostamento** della console di gestione accesso remoto, fare clic su **proxy applicazione Web**, quindi nel riquadro **attività** fare clic su **pubblica**.  
  
2.  Nella **pagina iniziale** della **Pubblicazione guidata nuova applicazione** fare clic su **Avanti**.  
  
3.  Nella pagina **preautenticazione** fare clic su **Active Directory Federation Services (ad FS)** , quindi fare clic su **Avanti**.  
  
4.  Nella pagina dei **client supportati** selezionare **Web e MSOFBA** e quindi fare clic su **Avanti**.  
  
5.  Nell'elenco di relying party della pagina **Relying party** selezionare la relying party per l'applicazione che si desidera pubblicare e quindi fare clic su **Avanti**.  
  
6.  Nella pagina **Impostazioni di pubblicazione** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Nella casella **Nome** immettere un nome descrittivo per l'applicazione.  
  
        Questo nome viene utilizzato solo nell'elenco di applicazioni pubblicate nella console Gestione accesso remoto.  
  
    -   Nella casella **URL esterno** immettere l'URL esterno per l'applicazione, ad esempio https://owa.contoso.com/.  
  
    -   Nell'elenco **Certificato esterno** selezionare un certificato il cui soggetto includa l'URL esterno.  
  
    -   Nella casella **URL server back-end** immettere l'URL del server back-end. Si noti che questo valore viene immesso automaticamente quando si immette l'URL esterno ed è consigliabile modificarlo solo se l'URL del server back-end è diverso. ad esempio, https://owa/.  
  
        > [!NOTE]  
        > Il proxy dell'applicazione Web può convertire i nomi host in URL, ma non può convertire i nomi di percorso. È pertanto possibile immettere nomi host diversi, ma è necessario immettere lo stesso nome di percorso. Ad esempio, è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://app-server/app1/. Tuttavia, non è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://apps.contoso.com/internal-app1/.  
  
    -   Nella casella **SPN server back-end** immettere il nome dell'entità servizio per il server back-end, ad esempio HTTP/owa.contoso.com.  
  
7.  Nella pagina **Conferma** verificare le impostazioni e quindi fare clic su **Pubblica**. È possibile copiare il comando di PowerShell per configurare altre applicazioni pubblicate.  
  
8.  Nella pagina **Risultati** verificare che l'applicazione sia stata pubblicata correttamente e quindi fare clic su **Chiudi**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif) ***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
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
  
## <a name="publish-an-application-that-uses-ms-ofba"></a><a name="BKMK_1.3"></a>Pubblicare un'applicazione che usa MS-OFBA  
Proxy applicazione Web supporta l'accesso da client di Microsoft Office, ad esempio Microsoft Word, che accedono a documenti e dati nei server back-end. L'unica differenza tra queste applicazioni e un browser standard consiste nel fatto che il reindirizzamento al servizio token di servizio viene eseguito non tramite il normale reindirizzamento HTTP ma con intestazioni MS-OFBA speciali come specificato in: [https://msdn.microsoft.com/library/dd773463(v=office.12).aspx](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx). L'applicazione back-end può essere basata sulle attestazioni o sull'autenticazione integrata di Windows.   
Per pubblicare un'applicazione per i client che utilizzano MS-OFBA, è necessario aggiungere al Servizio federativo un trust di relying party per l'applicazione. A seconda dell'applicazione, è possibile utilizzare l'autenticazione basata sulle attestazioni oppure l'autenticazione integrata di Windows. È pertanto necessario aggiungere il trust della relying party pertinente in base all'applicazione.  
  
Per consentire al proxy applicazione Web di eseguire Single Sign-On (SSO) e di eseguire la delega delle credenziali utilizzando la delega vincolata Kerberos, il server proxy applicazione Web deve essere aggiunto a un dominio. Vedere [pianificare Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Se l'applicazione usa l'autenticazione basata su attestazioni non sono necessarie altre fase di pianificazione. Se l'applicazione usa l'autenticazione integrata di Windows, vedere [pubblicare un'applicazione basata su autenticazione integrata di Windows per i client del browser Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2).  
  
Il flusso di autenticazione per i client che usano il protocollo MS-OFBA usando l'autenticazione basata sulle attestazioni è descritto di seguito. L'autenticazione per questo scenario può utilizzare il token dell'applicazione nell'URL oppure nel corpo.  
  
1.  L'utente sta utilizzando un'applicazione di Office e dall'elenco **Documenti recenti** apre un file in un sito di SharePoint.  
  
2.  Nell'applicazione di Office viene visualizzata una finestra con un controllo browser che richiede l'immissione di credenziali.  
  
    > [!NOTE]  
    > In alcuni casi è possibile che la finestra non venga visualizzata perché il client è già autenticato.  
  
3.  Proxy applicazione Web reindirizza la richiesta al server AD FS, che esegue l'autenticazione.  
  
4.  Il server AD FS reindirizza la richiesta al proxy dell'applicazione Web. La richiesta contiene ora il token perimetrale.  
  
5.  Il server di AD FS aggiunge un cookie di Single Sign-On (SSO) alla richiesta perché l'utente ha già eseguito l'autenticazione nel server AD FS.  
  
6.  Il proxy dell'applicazione Web convalida il token e lo trasmette al server back-end.  
  
7.  Il server back-end reindirizza la richiesta al server AD FS per ottenere il token di sicurezza dell'applicazione.  
  
8.  La richiesta viene reindirizzata al server back-end. La richiesta contiene ora il token dell'applicazione e il cookie SSO. L'utente può accedere al sito di SharePoint senza immettere un nome utente e una password per visualizzare il file.  
  
I passaggi per la pubblicazione di un'applicazione che utilizza MS-OFBA sono identici ai passaggi di un'applicazione basata su attestazioni o di un'applicazione non basata su attestazioni. Per le applicazioni basate su attestazioni, vedere [pubblicare un'applicazione basata sulle attestazioni per i client del Web browser](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1), per le applicazioni non basate su attestazioni, vedere [pubblicare un'applicazione basata su autenticazione integrata di Windows per i client del browser Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2). Il proxy dell'applicazione Web rileva automaticamente il client ed esegue l'autenticazione dell'utente come richiesto.  
  
## <a name="publish-an-application-that-uses-http-basic"></a>Pubblicare un'applicazione che usa HTTP Basic  

HTTP Basic è il protocollo di autorizzazione usato da molti protocolli per connettere i client avanzati, inclusi gli smartphone, alla cassetta postale di Exchange. Per ulteriori informazioni su HTTP Basic, vedere la [specifica RFC 2617](https://www.ietf.org/rfc/rfc2617.txt). Il proxy dell'applicazione Web in genere interagisce con AD FS usando i reindirizzamenti. la maggior parte dei client avanzati non supporta i cookie o la gestione dello stato. In questo modo, il proxy dell'applicazione Web consente all'app HTTP di ricevere un trust relying party non attestazioni per l'applicazione al Servizio federativo. Vedere [pianificare Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Il flusso di autenticazione per i client che usano HTTP Basic è descritto di seguito e in questo diagramma:  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  L'utente tenta di accedere a un'applicazione Web pubblicata come client telefonico.  
  
2.  L'app invia una richiesta HTTPS all'URL pubblicato dal proxy dell'applicazione Web.  
  
3.  Se la richiesta non contiene credenziali, il proxy dell'applicazione Web restituisce una risposta HTTP 401 all'app contenente l'URL del server AD FS di autenticazione.  
  
4.  L'utente invia nuovamente la richiesta HTTPS all'app con l'autorizzazione impostata su base e il nome utente e la password crittografata base 64 dell'utente nell'intestazione della richiesta WWW-Authenticate.  
  
5.  Poiché il dispositivo non può essere reindirizzato a AD FS, il proxy dell'applicazione Web invia una richiesta di autenticazione a AD FS con le credenziali che contiene, inclusi nome utente e password. Il token viene acquisito per conto del dispositivo.  
  
6.  Per ridurre al minimo il numero di richieste inviate al AD FS, proxy applicazione Web, convalida le successive richieste client usando i token memorizzati nella cache per tutto il tempo in cui il token è valido. Il proxy dell'applicazione Web pulisce periodicamente la cache. È possibile visualizzare le dimensioni della cache usando il contatore delle prestazioni.  
  
7.  Se il token è valido, il proxy dell'applicazione Web Invia la richiesta al server back-end e all'utente viene concesso l'accesso all'applicazione Web pubblicata.  
  
Nella procedura seguente viene illustrato come pubblicare applicazioni di base HTTP.  
  
#### <a name="to-publish-an-http-basic-application"></a>Per pubblicare un'applicazione HTTP di base  
  
1.  Nel server proxy applicazione Web, nel riquadro di **spostamento** della console di gestione accesso remoto, fare clic su **proxy applicazione Web**, quindi nel riquadro **attività** fare clic su **pubblica**.  
  
2.  Nella **pagina iniziale** della **Pubblicazione guidata nuova applicazione** fare clic su **Avanti**.  
  
3.  Nella pagina **preautenticazione** fare clic su **Active Directory Federation Services (ad FS)** , quindi fare clic su **Avanti**.  
  
4.  Nella pagina **client supportati** selezionare **http Basic** , quindi fare clic su **Avanti**.  
  
    Se si vuole abilitare l'accesso a Exchange solo da dispositivi aggiunti all'area di lavoro, selezionare la casella **Abilita l'accesso solo per i dispositivi aggiunti all'area di lavoro** . Per altre informazioni [, vedere Aggiungere un'area di lavoro da qualsiasi dispositivo per SSO e l'autenticazione a due fattori trasparente per tutte le applicazioni aziendali](https://technet.microsoft.com/library/dn280945.aspx).  
  
5.  Nell'elenco di relying party della pagina **Relying party** selezionare la relying party per l'applicazione che si desidera pubblicare e quindi fare clic su **Avanti**. Si noti che questo elenco contiene solo le relying party on-Claims.  
  
6.  Nella pagina **Impostazioni di pubblicazione** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Nella casella **Nome** immettere un nome descrittivo per l'applicazione.  
  
        Questo nome viene utilizzato solo nell'elenco di applicazioni pubblicate nella console Gestione accesso remoto.  
  
    -   Nella casella **URL esterno** immettere l'URL esterno per l'applicazione. ad esempio, mail.contoso.com  
  
    -   Nell'elenco **Certificato esterno** selezionare un certificato il cui soggetto includa l'URL esterno.  
  
    -   Nella casella **URL server back-end** immettere l'URL del server back-end. Si noti che questo valore viene immesso automaticamente quando si immette l'URL esterno ed è consigliabile modificarlo solo se l'URL del server back-end è diverso. ad esempio, mail.contoso.com.  
  
7.  Nella pagina **Conferma** verificare le impostazioni e quindi fare clic su **Pubblica**. È possibile copiare il comando di PowerShell per configurare altre applicazioni pubblicate.  
  
8.  Nella pagina **Risultati** verificare che l'applicazione sia stata pubblicata correttamente e quindi fare clic su **Chiudi**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif) ***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
Questo script di Windows PowerShell Abilita la preautenticazione per tutti i dispositivi, non solo per i dispositivi aggiunti all'area di lavoro.  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
Il codice seguente esegue la preautenticazione solo dei dispositivi aggiunti all'area di lavoro:  
  
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
  
## <a name="publish-an-application-that-uses-oauth2-such-as-a-microsoft-store-app"></a><a name="BKMK_1.4"></a>Pubblicare un'applicazione che usa OAuth2, ad esempio un'app Microsoft Store  
Per pubblicare un'applicazione per Microsoft Store app, è necessario aggiungere una relying party attendibilità per l'applicazione al Servizio federativo.  
  
Per consentire al proxy applicazione Web di eseguire Single Sign-On (SSO) e di eseguire la delega delle credenziali utilizzando la delega vincolata Kerberos, il server proxy applicazione Web deve essere aggiunto a un dominio. Vedere [pianificare Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
> [!NOTE]  
> Il proxy dell'applicazione Web supporta la pubblicazione solo per Microsoft Store app che usano il protocollo OAuth 2,0.  
  
Nella console di gestione AD FS è necessario assicurarsi che l'endpoint OAuth sia abilitato per il proxy. A tale scopo, aprire la console di gestione di ADFS, espandere **Servizio** e fare clic su **Endpoint**. Nell'elenco **Endpoint** trovare l'endpoint OAuth e verificare che il valore nella colonna **Proxy abilitato** sia **Sì**.  
  
Il flusso di autenticazione per i client che usano Microsoft Store app è descritto di seguito:  
  
> [!NOTE]  
> Il proxy dell'applicazione Web reindirizza al server AD FS per l'autenticazione. Poiché le app Microsoft Store non supportano i reindirizzamenti, se si usano Microsoft Store app, è necessario impostare l'URL del server AD FS usando il cmdlet Set-WebApplicationProxyConfiguration e il parametro OAuthAuthenticationURL.  
>   
> Microsoft Store le app possono essere pubblicate solo con Windows PowerShell.  
  
1.  Il client tenta di accedere a un'applicazione Web pubblicata usando un'app Microsoft Store.  
  
2.  L'app invia una richiesta HTTPS all'URL pubblicato dal proxy dell'applicazione Web.  
  
3.  Proxy applicazione Web restituisce una risposta HTTP 401 all'app contenente l'URL del server AD FS di autenticazione. Questo processo è noto come "individuazione".  
  
    > [!NOTE]  
    > Se l'app conosce l'URL del server di AD FS di autenticazione e dispone già di un token combinato contenente il token OAuth e il token perimetrale, i passaggi 2 e 3 vengono ignorati in questo flusso di autenticazione.  
  
4.  L'app invia una richiesta HTTPS al server AD FS.  
  
5.  L'app usa il broker di autenticazione Web per generare una finestra di dialogo in cui l'utente immette le credenziali per l'autenticazione nel server AD FS. Per informazioni sul gestore delle autenticazioni Web, vedere [Gestore di autenticazioni Web](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx).  
  
6.  Al termine dell'autenticazione, il server di AD FS crea un token combinato contenente il token OAuth e il token perimetrale e invia il token all'app.  
  
7.  L'app invia una richiesta HTTPS contenente il token combinato all'URL pubblicato dal proxy dell'applicazione Web.  
  
8.  Il proxy applicazione Web suddivide il token combinato nelle due parti e convalida il token perimetrale.  
  
9. Se il token perimetrale è valido, il proxy dell'applicazione Web Invia la richiesta al server back-end solo con il token OAuth. L'utente può accedere all'applicazione Web pubblicata.  
  
Questa procedura descrive come pubblicare un'applicazione per OAuth2. Questo tipo di applicazione può essere pubblicato solo con Windows PowerShell. Prima di iniziare, assicurarsi di avere eseguito le operazioni seguenti:  
  
-   Creazione di un trust relying party per l'applicazione nella console di gestione AD FS.  
  
-   Assicurarsi che l'endpoint OAuth sia abilitato per il proxy nella console di gestione di AD FS e prendere nota del percorso dell'URL.  
  
-   Verificare che un certificato nel server proxy applicazione Web sia adatto all'applicazione che si desidera pubblicare.  
  
#### <a name="to-publish-an-oauth2-app"></a>Per pubblicare un'app OAuth2  
  
1.  Nel server proxy applicazione Web, nel riquadro di **spostamento** della console di gestione accesso remoto, fare clic su **proxy applicazione Web**, quindi nel riquadro **attività** fare clic su **pubblica**.  
  
2.  Nella **pagina iniziale** della **Pubblicazione guidata nuova applicazione** fare clic su **Avanti**.  
  
3.  Nella pagina **preautenticazione** fare clic su **Active Directory Federation Services (ad FS)** , quindi fare clic su **Avanti**.  
  
4.  Nella pagina **client supportati** selezionare **OAuth2** e quindi fare clic su **Avanti**.  
  
5.  Nell'elenco di relying party della pagina **Relying party** selezionare la relying party per l'applicazione che si desidera pubblicare e quindi fare clic su **Avanti**.  
  
6.  Nella pagina **Impostazioni di pubblicazione** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Nella casella **Nome** immettere un nome descrittivo per l'applicazione.  
  
        Questo nome viene utilizzato solo nell'elenco di applicazioni pubblicate nella console Gestione accesso remoto.  
  
    -   Nella casella **URL esterno** immettere l'URL esterno per l'applicazione, ad esempio https://server1.contoso.com/app1/.  
  
    -   Nell'elenco **Certificato esterno** selezionare un certificato il cui soggetto includa l'URL esterno.  
  
        Per assicurarsi che gli utenti possano accedere all'app, anche se trascurano il tipo HTTPS nell'URL, selezionare la casella **di abilitazione del Reindirizzamento da http a https** .  
  
    -   Nella casella **URL server back-end** immettere l'URL del server back-end. Si noti che questo valore viene immesso automaticamente quando si immette l'URL esterno ed è consigliabile modificarlo solo se l'URL del server back-end è diverso. ad esempio, https://sp/app1/.  
  
        > [!NOTE]  
        > Il proxy dell'applicazione Web può convertire i nomi host in URL, ma non può convertire i nomi di percorso. È pertanto possibile immettere nomi host diversi, ma è necessario immettere lo stesso nome di percorso. Ad esempio, è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://app-server/app1/. Tuttavia, non è possibile immettere un URL esterno di https://apps.contoso.com/app1/ e un URL del server back-end di https://apps.contoso.com/internal-app1/.  
  
7.  Nella pagina **Conferma** verificare le impostazioni e quindi fare clic su **Pubblica**. È possibile copiare il comando di PowerShell per configurare altre applicazioni pubblicate.  
  
8.  Nella pagina **Risultati** verificare che l'applicazione sia stata pubblicata correttamente e quindi fare clic su **Chiudi**.  
  
Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
Per impostare l'URL di autenticazione OAuth per un indirizzo server federativo di fs.contoso.com e il percorso URL di/ADFS/OAuth2/:  
  
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
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vedere anche  
  
-   [Risoluzione dei problemi relativi a Proxy applicazione Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   [Pubblicare applicazioni tramite il proxy applicazione Web](https://technet.microsoft.com/library/dn383659.aspx)  
  
-   [Pianificazione della pubblicazione di applicazioni mediante il proxy applicazione Web](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Guida alla procedura dettagliata del proxy dell'applicazione Web](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Cmdlet del proxy dell'applicazione Web in Windows PowerShell](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Add-WebApplicationProxyApplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-WebApplicationProxyConfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  


