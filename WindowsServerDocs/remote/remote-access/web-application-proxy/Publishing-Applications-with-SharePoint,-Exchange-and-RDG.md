---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: Pubblicazione delle applicazioni con SharePoint, Exchange e RDG
description: ''
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: 0e5c9779fae66e7edec3c1bba471d5cadbe48a72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404236"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>Pubblicazione delle applicazioni con SharePoint, Exchange e RDG

>Si applica a: Windows Server 2016

il contenuto **Stanziamento è pertinente per la versione locale del proxy applicazione Web. Per abilitare l'accesso sicuro alle applicazioni locali tramite il cloud, vedere il [Azure ad contenuto del proxy dell'applicazione](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  

In questo argomento vengono descritte le attività necessarie per la pubblicazione di SharePoint Server, Exchange Server o Desktop remoto Gateway (RDP) tramite proxy applicazione Web.  

>[!NOTE]
>Queste informazioni vengono fornite così come sono.  Servizi Desktop remoto supporta e consiglia di usare [app Azure proxy per fornire l'accesso remoto sicuro alle applicazioni locali](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="BKMK_6.1"></a>Pubblicare SharePoint Server  
È possibile pubblicare un sito di SharePoint tramite proxy applicazione Web quando il sito di SharePoint è configurato per l'autenticazione basata sulle attestazioni o l'autenticazione integrata di Windows. Se si desidera utilizzare Active Directory Federation Services (AD FS) per la pre-autenticazione, è necessario configurare una relying party utilizzando una delle procedure guidate.  

-   Se il sito di SharePoint usa l'autenticazione basata sulle attestazioni, è necessario usare l'Aggiunta guidata attendibilità componente per configurare l'attendibilità della relying party per l'applicazione.  

-   Se il sito di SharePoint usa l'autenticazione integrata di Windows, è necessario usare la procedura di aggiunta guidata attendibilità della relying party non basata sulle attestazioni per configurare l'attendibilità della relying party per l'applicazione. È possibile usare l'autenticazione integrata di Windows con un'applicazione Web basata sulle attestazioni, a condizione che venga configurato un servizio KDC.  

    Per consentire agli utenti di eseguire l'autenticazione con l'autenticazione integrata di Windows, il server proxy applicazione Web deve essere aggiunto a un dominio.  

    È necessario configurare l'applicazione in modo da supportare la delega vincolata Kerberos. È possibile eseguire questa operazione nel controller di dominio per ogni applicazione È anche possibile configurare l'applicazione direttamente nel server back-end se è in esecuzione in Windows Server 2012 R2 o Windows Server 2012. Per altre informazioni, vedere [Novità dell'autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx). È inoltre necessario assicurarsi che i server proxy applicazione Web siano configurati per la delega ai nomi dell'entità servizio dei server back-end. Per una procedura dettagliata sulla configurazione del proxy dell'applicazione Web per la pubblicazione di un'applicazione con l'autenticazione integrata di Windows, vedere [configurare un sito per l'utilizzo dell'autenticazione integrata di Windows](assetId:///b0788958-627f-450f-877c-209b1bd0db52).  

Se il sito di SharePoint è configurato per l'uso di mapping di accesso alternativo o raccolte siti con nome host, per pubblicare l'applicazione è possibile usare URL diversi per il server back-end e il server esterno. Se, tuttavia, non si configura il sito di SharePoint usando il mapping di accesso alternativo o le raccolte siti con nome host, è necessario usare gli stessi URL per il server back-end e il server esterno.  

## <a name="BKMK_6.2"></a>Pubblicare Exchange Server  
La tabella seguente descrive i servizi di Exchange che è possibile pubblicare tramite proxy applicazione Web e la preautenticazione supportata per questi servizi:  


|    Servizio Exchange    |                                                                            Preautenticazione                                                                            |                                                                                                                                       Note                                                                                                                                        |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Outlook Web App     | -AD FS usando l'autenticazione non basata sulle attestazioni<br />-Pass-through<br />-AD FS uso dell'autenticazione basata sulle attestazioni per Exchange 2013 Service Pak 1 (SP1) locale |                                                                  Per altre informazioni, vedi: [Uso dell'autenticazione basata sulle attestazioni di AD FS con Outlook Web App ed EAC](https://go.microsoft.com/fwlink/?LinkId=393723)                                                                  |
| Pannello di controllo di Exchange |                                                                               Pass-through                                                                               |                                                                                                                                                                                                                                                                                    |
|    Outlook via Internet    |                                                                               Pass-through                                                                               | Per il corretto funzionamento di Outlook via Internet, è necessario pubblicare tre URL:<br /><br />: URL di individuazione automatica.<br />: Nome host esterno del server Exchange. ovvero, l'URL configurato per la connessione dei client a.<br />: FQDN interno del server Exchange. |
|  Exchange ActiveSync   |                                                     Pass-through<br/> AD FS utilizzo del protocollo di autorizzazione HTTP Basic                                                      |                                                                                                                                                                                                                                                                                    |

Per pubblicare Outlook Web App con l'autenticazione integrata di Windows, è necessario usare la procedura di aggiunta guidata attendibilità della relying party non basata sulle attestazioni per configurare l'attendibilità della relying party per l'applicazione.  

Per consentire agli utenti di eseguire l'autenticazione utilizzando la delega vincolata Kerberos, il server proxy applicazione Web deve essere aggiunto a un dominio.  

È necessario configurare l'applicazione in modo che supporti l'autenticazione Kerberos. Inoltre, è necessario registrare un nome dell'entità servizio (SPN) per l'account con cui viene eseguito il servizio Web. È possibile eseguire questa operazione nel controller di dominio o nei server back-end. In un ambiente Exchange con bilanciamento del carico questo richiederebbe l'uso dell'account del servizio alternativo, vedere [configurazione dell'autenticazione Kerberos per i server Accesso client con bilanciamento del carico](https://technet.microsoft.com/library/ff808312(v=exchg.150).aspx)  

È anche possibile configurare l'applicazione direttamente nel server back-end se è in esecuzione in Windows Server 2012 R2 o Windows Server 2012. Per altre informazioni, vedere [Novità dell'autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx). È inoltre necessario assicurarsi che i server proxy applicazione Web siano configurati per la delega ai nomi dell'entità servizio dei server back-end.  

## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>Pubblicazione del Gateway Desktop remoto tramite proxy applicazione Web  
Se si vuole limitare l'accesso al gateway di accesso remoto e aggiungere la pre-autenticazione per l'accesso remoto, è possibile implementarla tramite il proxy applicazione Web. Si tratta di un modo davvero efficace per assicurarsi di avere una preautenticazione avanzata per RDG, inclusa l'autenticazione a più fattori. La pubblicazione senza pre-autenticazione è anche un'opzione e fornisce un singolo punto di ingresso nei sistemi.  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>Come pubblicare un'applicazione in RDG usando l'autenticazione pass-through del proxy dell'applicazione Web  

1. L'installazione sarà diversa a seconda che i ruoli di Accesso Web RD (/RDWeb) e Gateway Desktop remoto (RPC) si trovino nello stesso server o in server diversi.  

2. Se i ruoli Accesso Web Desktop remoto e Gateway Desktop remoto sono ospitati nello stesso server RDG, è possibile pubblicare semplicemente il nome FQDN radice in proxy applicazione Web, ad esempio, https://rdg.contoso.com/.  

   È anche possibile pubblicare le due directory virtuali singolarmente, ad esempio <https://rdg.contoso.com/rdweb/> e https://rdg.contoso.com/rpc/.  

3. Se il Accesso Web Desktop remoto e il Gateway Desktop remoto sono ospitati in server RDG distinti, è necessario pubblicare singolarmente le due directory virtuali. È possibile usare lo stesso FQDN esterno o uno diverso, ad esempio https://rdweb.contoso.com/rdweb/ e https://gateway.contoso.com/rpc/.  

4. Se gli FQDN esterni e interni sono diversi, non è consigliabile disabilitare la conversione dell'intestazione della richiesta nella regola di pubblicazione RDWeb. Questa operazione può essere eseguita eseguendo lo script di PowerShell seguente nel server proxy applicazione Web, ma deve essere abilitato per impostazione predefinita.

   ```  
   Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false  
   ```  

   > [!NOTE]  
   > Se è necessario supportare client avanzati, ad esempio connessioni RemoteApp e desktop o connessioni iOS Desktop remoto, questi non supportano la pre-autenticazione, quindi è necessario pubblicare RDG usando l'autenticazione pass-through.  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>Come pubblicare un'applicazione in RDG usando il proxy dell'applicazione Web con la pre-autenticazione  

1.  La pre-autenticazione del proxy dell'applicazione Web con RDG funziona passando il cookie di pre-autenticazione ottenuto da Internet Explorer passato al client di Connessione Desktop remoto (mstsc. exe). Questa operazione viene quindi utilizzata dal client Connessione Desktop remoto (mstsc. exe). Questa operazione viene quindi utilizzata da Connessione Desktop remoto client come prova dell'autenticazione.  

    La procedura seguente indica al server di raccolta di includere le proprietà RDP personalizzate necessarie nei file RDP dell'app remota inviati ai client. Queste segnalano al client che la preautenticazione è necessaria e per passare i cookie per l'indirizzo del server di pre-autenticazione a Connessione Desktop remoto client (mstsc. exe). In combinazione con la disabilitazione della funzionalità HttpOnly nell'applicazione proxy applicazione Web, ciò consente al client di Connessione Desktop remoto (mstsc. exe) di utilizzare il cookie del proxy dell'applicazione Web ottenuto tramite il browser.  

    L'autenticazione al server Accesso Web RD utilizzerà comunque l'accesso al modulo di Accesso Web Desktop remoto. In questo modo viene fornito il minor numero di richieste di autenticazione utente, perché il form di accesso Accesso Web RD crea un archivio di credenziali sul lato client che può essere usato da Connessione Desktop remoto client (mstsc. exe) per qualsiasi avvio successivo dell'app remota.  

2.  Prima di tutto, creare un trust della relying party manuale in AD FS come se si stesse pubblicando un'app in grado di riconoscere attestazioni. Ciò significa che è necessario creare un'attendibilità relying party fittizia per applicare la pre-autenticazione, in modo da ottenere la pre-autenticazione senza la delega vincolata Kerberos al server pubblicato. Una volta che un utente ha eseguito l'autenticazione, viene passato tutto il resto.  

    > [!WARNING]  
    > Potrebbe sembrare che l'uso della delega sia preferibile, ma non risolve completamente i requisiti di mstsc SSO e si verificano problemi quando si delega alla directory/RPC perché il client prevede di gestire l'autenticazione di Gateway Desktop remoto.  

3.  Per creare un trust della relying party manuale, attenersi alla procedura descritta in AD FS Management Console:  

    1.  Usare la procedura guidata **Aggiungi Trust della relying party**  

    2.  Selezionare **immettere manualmente i dati sul relying party**.  

    3.  Accettare tutte le impostazioni predefinite.  

    4.  Per l'identificatore del trust della relying party, immettere il nome FQDN esterno da usare per l'accesso RDG, ad esempio https://rdg.contoso.com/.  

        Si tratta dell'attendibilità relying party che verrà usata quando si pubblica l'app in proxy applicazione Web.  

4.  Pubblicare la radice del sito, ad esempio https://rdg.contoso.com/, in proxy applicazione Web. Impostare la pre-autenticazione su AD FS e usare il relying party attendibilità creato in precedenza. In questo modo,/RDWeb e/RPC utilizzeranno lo stesso cookie di autenticazione del proxy dell'applicazione Web.  

    È possibile pubblicare/RDWeb e/RPC come applicazioni separate e anche per utilizzare server pubblicati diversi. È sufficiente assicurarsi di pubblicare entrambi usando lo stesso trust della relying party in cui viene emesso il token del proxy dell'applicazione Web per l'attendibilità del componente ed è quindi valido tra le applicazioni pubblicate con lo stesso trust della relying party.  

5.  Se gli FQDN esterni e interni sono diversi, non è consigliabile disabilitare la conversione dell'intestazione della richiesta nella regola di pubblicazione RDWeb. Questa operazione può essere eseguita eseguendo lo script di PowerShell seguente nel server proxy applicazione Web, ma deve essere abilitata per impostazione predefinita:

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false 
    ```  

6.  Disabilitare la proprietà cookie HttpOnly in proxy applicazione Web nell'applicazione pubblicata RDG. Per consentire al controllo ActiveX RDG di accedere al cookie di autenticazione del proxy dell'applicazione Web, è necessario disabilitare la proprietà HttpOnly nel cookie del proxy dell'applicazione Web.  

    Per questa operazione è necessario installare l' [hotfix del proxy dell'applicazione Web](https://support.microsoft.com/en-gb/kb/3000850) o il [https://support.microsoft.com/en-gb/kb/3000850](https://support.microsoft.com/en-gb/kb/3000850).  

    Dopo aver installato l'hotfix, eseguire lo script di PowerShell seguente nel server proxy applicazione Web specificando il nome dell'applicazione pertinente:  

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true  
    ```  

    La disabilitazione di HttpOnly consente al controllo ActiveX RDG di accedere al cookie di autenticazione del proxy dell'applicazione Web.  

7.  Configurare la raccolta di RDG pertinente nel server di raccolta per consentire al client di Connessione Desktop remoto (mstsc. exe) di tenere presente che la pre-autenticazione è necessaria nel file RDP.  

    -   In Windows Server 2012 e Windows Server 2012 R2 questa operazione può essere eseguita eseguendo il cmdlet di PowerShell seguente nel server di raccolta RDG:  

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```

        Assicurarsi di rimuovere il < e > parentesi quadre quando si sostituisce con valori personalizzati, ad esempio:

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```

    -   In Windows Server 2008 R2:  

        1.  Accedere al Terminal Server con un account che disponga dei privilegi di amministratore.  

        2.  Passare a **Start** >**strumenti di amministrazione** > **Servizi Terminal** > **TS Gestione RemoteApp.**  

        3.  Nel riquadro **Panoramica** di gestione RemoteApp di Servizi terminal, accanto a impostazioni RDP, fare clic su **Cambia**.  

        4.  Nella scheda **Impostazioni RDP personalizzate** Digitare le seguenti impostazioni RDP nella casella impostazioni RDP personalizzate:  

            `pre-authentication server address: s: https://externalfqdn/rdweb/`  

            `require pre-authentication:i:1`  

        5.  Al termine, fare clic su **applica**.  

            Ciò indica al server di raccolta di includere le proprietà RDP personalizzate nei file RDP inviati ai client. Queste segnalano al client che la preautenticazione è necessaria e per passare i cookie per l'indirizzo del server di pre-autenticazione al client di Connessione Desktop remoto (mstsc. exe). In questo modo, insieme alla disabilitazione di HttpOnly nell'applicazione proxy applicazione Web, consente al client di Connessione Desktop remoto (mstsc. exe) di utilizzare il cookie di autenticazione proxy applicazione Web ottenuto tramite il browser.  

            Per ulteriori informazioni su RDP, vedere [configurazione dello scenario OTP del gateway di Servizi terminal](https://technet.microsoft.com/library/cc731249(v=ws.10).aspx).  

## <a name="BKMK_Links"></a>Vedere anche  

-   [Pianificazione della pubblicazione di applicazioni mediante il proxy applicazione Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))  

-   [Risoluzione dei problemi relativi a Proxy applicazione Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))  

-   [Guida alla procedura dettagliata del proxy dell'applicazione Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))  
