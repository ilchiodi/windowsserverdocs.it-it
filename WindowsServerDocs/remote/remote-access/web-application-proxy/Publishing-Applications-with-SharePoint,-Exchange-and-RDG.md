---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: Pubblicazione delle applicazioni con SharePoint, Exchange e RDG
description: ''
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: fd706f61216ab8760d94faf98d651d17b24efc91
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447140"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>Pubblicazione delle applicazioni con SharePoint, Exchange e RDG

>Si applica a: Windows Server 2016

**Questo contenuto è rilevante per la versione locale del Proxy applicazione Web. Per abilitare l'accesso sicuro alle applicazioni locali tramite il cloud, vedere la [contenuto di Azure AD Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  

In questo argomento vengono descritte le attività necessarie per la pubblicazione di SharePoint Server, Exchange Server o Gateway di Desktop remoto (RDP) con il Proxy applicazione Web.  

>[!NOTE]
>Queste informazioni vengono fornite come-è.  Servizi Desktop remoto supporta e consiglia di usare [Proxy App di Azure per fornire accesso remoto sicuro alle applicazioni locali](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="BKMK_6.1"></a>Publish SharePoint Server  
È possibile pubblicare un sito di SharePoint mediante Proxy applicazione Web quando il sito di SharePoint è configurato per l'autenticazione basata su attestazioni o l'autenticazione Windows integrata. Se si vuole usare Active Directory Federation Services (ADFS) per la preautenticazione, è necessario configurare una relying party usando una delle procedure guidate.  

-   Se il sito di SharePoint usa l'autenticazione basata sulle attestazioni, è necessario usare l'Aggiunta guidata attendibilità componente per configurare l'attendibilità della relying party per l'applicazione.  

-   Se il sito di SharePoint usa l'autenticazione integrata di Windows, è necessario usare la procedura di aggiunta guidata attendibilità della relying party non basata sulle attestazioni per configurare l'attendibilità della relying party per l'applicazione. È possibile usare l'autenticazione integrata di Windows con un'applicazione Web basata sulle attestazioni, a condizione che venga configurato un servizio KDC.  

    Per consentire agli utenti di eseguire l'autenticazione usando l'autenticazione Windows integrata, il server Proxy applicazione Web deve appartenere a un dominio.  

    È necessario configurare l'applicazione in modo da supportare la delega vincolata Kerberos. È possibile eseguire questa operazione nel controller di dominio per ogni applicazione È anche possibile configurare l'applicazione direttamente nel server di back-end se è in esecuzione in Windows Server 2012 R2 o Windows Server 2012. Per altre informazioni, vedere [Novità dell'autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx). È anche necessario assicurarsi che i server Proxy applicazione Web sono configurati per la delega ai nomi dell'entità servizio dei server back-end. Per una procedura dettagliata per configurare il Proxy applicazione Web per pubblicare un'applicazione usando l'autenticazione Windows integrata, vedere [configurare un sito per usare l'autenticazione Windows integrata](assetId:///b0788958-627f-450f-877c-209b1bd0db52).  

Se il sito di SharePoint è configurato per l'uso di mapping di accesso alternativo o raccolte siti con nome host, per pubblicare l'applicazione è possibile usare URL diversi per il server back-end e il server esterno. Se, tuttavia, non si configura il sito di SharePoint usando il mapping di accesso alternativo o le raccolte siti con nome host, è necessario usare gli stessi URL per il server back-end e il server esterno.  

## <a name="BKMK_6.2"></a>Pubblicazione di Exchange Server  
La tabella seguente descrive i servizi di Exchange che è possibile pubblicare tramite Proxy applicazione Web e la preautenticazione supportata per questi servizi:  


|    Servizio Exchange    |                                                                            Preautenticazione                                                                            |                                                                                                                                       Note                                                                                                                                        |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Outlook Web App     | -AD FS Usa l'autenticazione non basata su attestazioni<br />-Pass-through<br />-AD FS Usa l'autenticazione basata su attestazioni per on-premises Exchange 2013 Service Pack 1 (SP1) |                                                                  Per altre informazioni, vedi: [Uso dell'autenticazione basata sulle attestazioni di AD FS con Outlook Web App ed EAC](https://go.microsoft.com/fwlink/?LinkId=393723)                                                                  |
| Pannello di controllo di Exchange |                                                                               Pass-through                                                                               |                                                                                                                                                                                                                                                                                    |
|    Outlook via Internet    |                                                                               Pass-through                                                                               | Per il corretto funzionamento di Outlook via Internet, è necessario pubblicare tre URL:<br /><br />-L'URL di individuazione automatica.<br />-Il nome host esterno del Server di Exchange. vale a dire, l'URL configurato per la connessione dei client.<br />-Il nome FQDN interno del Server Exchange. |
|  Exchange ActiveSync   |                                                     Pass-through<br/> AD FS usando il protocollo di autorizzazione HTTP di base                                                      |                                                                                                                                                                                                                                                                                    |

Per pubblicare Outlook Web App con l'autenticazione integrata di Windows, è necessario usare la procedura di aggiunta guidata attendibilità della relying party non basata sulle attestazioni per configurare l'attendibilità della relying party per l'applicazione.  

Per consentire agli utenti di autenticarsi con Kerberos vincolata delega che il server Proxy applicazione Web deve appartenere a un dominio.  

È necessario configurare l'applicazione per supportare l'autenticazione Kerberos. Inoltre è necessario registrare un nome dell'entità servizio (SPN) per l'account di cui è in esecuzione il servizio web. È possibile farlo nel controller di dominio o nei server back-end. In un carico bilanciato ambiente Exchange ciò richiederebbe usando l'Account del servizio alternativo, vedere [configurazione dell'autenticazione Kerberos per i server Accesso Client con bilanciamento del carico](https://technet.microsoft.com/library/ff808312(v=exchg.150).aspx)  

È anche possibile configurare l'applicazione direttamente nel server di back-end se è in esecuzione in Windows Server 2012 R2 o Windows Server 2012. Per altre informazioni, vedere [Novità dell'autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx). È anche necessario assicurarsi che i server Proxy applicazione Web sono configurati per la delega ai nomi dell'entità servizio dei server back-end.  

## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>Gateway Desktop remoto mediante Proxy applicazione Web di pubblicazione  
Se si desidera limitare l'accesso al Gateway di accesso remoto e aggiungere l'autenticazione preliminare per l'accesso remoto, è possibile distribuirlo mediante Proxy applicazione Web. Si tratta di un metodo molto valido per assicurarsi di avere avanzata pre-autenticazione per Gateway Desktop remoto tra cui autenticazione a più fattori. La pubblicazione senza la preautenticazione è anche possibile usare e offre un singolo punto di ingresso nei sistemi.  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>Come pubblicare un'applicazione in Gateway Desktop remoto usando l'autenticazione pass-through di Proxy applicazione Web  

1. Installazione saranno diversa a seconda che l'accesso Web desktop remoto (/ rdweb) e i ruoli di Gateway Desktop remoto (rpc) sono nello stesso server o in server diversi.  

2. Se i ruoli di accesso Web desktop remoto e Gateway Desktop remoto sono ospitati nello stesso server Gateway Desktop remoto, è sufficiente pubblicare il nome di dominio completo di radice nel Proxy applicazione Web, ad esempio, https://rdg.contoso.com/.  

   È anche possibile pubblicare le due directory virtuali singolarmente, ad esempio<https://rdg.contoso.com/rdweb/> e https://rdg.contoso.com/rpc/.  

3. Se l'accesso Web desktop remoto e Gateway Desktop remoto sono ospitate nel server RDG separati, è necessario pubblicare le due directory virtuali singolarmente. È possibile usare, ad esempio l'uguali o diverso esterno FQDN https://rdweb.contoso.com/rdweb/ e https://gateway.contoso.com/rpc/.  

4. Se esterno e interno FQDN sono diversi è necessario non disabilitare la traduzione di intestazione richiesta per la regola di pubblicazione RDWeb. Questa operazione può essere eseguita tramite l'esecuzione di script di PowerShell seguente nel server Proxy applicazione Web, ma deve essere abilitato per impostazione predefinita.

   ```  
   Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false  
   ```  

   > [!NOTE]  
   > Se è necessario supportare rich client, ad esempio RemoteApp e Desktop connessioni o iOS le connessioni Desktop remoto, queste non supportano l'autenticazione preliminare è quindi necessario pubblicare Gateway Desktop remoto usando l'autenticazione pass-through.  

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>Come pubblicare un'applicazione in Gateway Desktop remoto Usa il Proxy applicazione Web con la preautenticazione  

1.  Pre-autenticazione Proxy applicazione Web con gateway desktop remoto funziona passando il cookie di autenticazione preliminare ottenuto da Internet Explorer vengono passati nel client connessione Desktop remoto (mstsc.exe). Viene quindi utilizzato dal client connessione Desktop remoto (mstsc.exe). Viene quindi utilizzato dall'applicazione client connessione Desktop remoto come prova dell'autenticazione.  

    La procedura seguente indica al server di raccolta per includere le necessarie proprietà RDP personalizzate nei file RDP App Remote che vengono inviati ai client. Questi elementi si indica al client che pre-autenticazione è necessario e passare i cookie per la pre-autenticazione ' indirizzo del server a client connessione Desktop remoto (mstsc.exe). In combinazione con la disabilitazione della funzionalità HttpOnly nell'applicazione Proxy dell'applicazione Web, in questo modo il client connessione Desktop remoto (mstsc.exe) usare il cookie di Proxy applicazione Web ottenuto tramite il browser.  

    L'autenticazione al server Accesso Web desktop remoto continuerà a utilizzare l'account di accesso di form di accesso Web desktop remoto. In questo modo il minor numero di autenticazione dell'utente chiede come modulo di accesso di accesso Web desktop remoto consente di creare un archivio credenziali del client che può quindi essere utilizzato dai client connessione Desktop remoto (mstsc.exe) per qualsiasi successivo avvio dell'App Remote.  

2.  Innanzitutto, creare un manuale Trust della Relying Party in AD FS come se si stavano pubblicando un'app con riconoscimento delle attestazioni. Ciò significa che è necessario creare un dummy trust della relying party che è presente per imporre l'autenticazione preliminare, in modo che ottengano pre-autenticazione senza delega vincolata Kerberos per il server pubblicato. Una volta che un utente è autenticato, tutto il resto viene passato.  

    > [!WARNING]  
    > Potrebbe sembrare che è preferibile utilizzare la delega ma non risolve completamente i requisiti di accesso SSO di mstsc e sono presenti problemi quando si delega nella directory /rpc perché prevede che il client gestire l'autenticazione Gateway Desktop remoto.  

3.  Per creare un manuale Trust della Relying Party, seguire i passaggi nella Console di gestione di AD FS:  

    1.  Usare la **Aggiungi Trust Relying Party** guidata  

    2.  Selezionare **Immetti dati sul componente manualmente**.  

    3.  Accettare tutte le impostazioni predefinite.  

    4.  Per l'identificatore del Trust della Relying Party, immettere il nome di dominio completo esterno si userà per l'accesso a gateway desktop remoto, ad esempio https://rdg.contoso.com/.  

        Questo è il trust della relying party, che verrà usato durante la pubblicazione dell'app nel Proxy applicazione Web.  

4.  Pubblicare la radice del sito (ad esempio, https://rdg.contoso.com/ ) nel Proxy applicazione Web. Impostare la preautenticazione di ADFS e utilizzare i trust della relying party creato in precedenza. Ciò consentirà /rdweb e /rpc usare lo stesso cookie di autenticazione Proxy applicazione Web.  

    È possibile pubblicare /rdweb e /rpc come applicazioni separate e anche di usare server pubblicato diversi. È sufficiente Assicurarsi di pubblicare sia utilizzando la stessa Trust della Relying Party, come il token di Proxy applicazione Web viene generato per i Trust della Relying Party e pertanto valido tra le applicazioni pubblicate con stesso Trust della Relying Party.  

5.  Se esterno e interno FQDN sono diversi è necessario non disabilitare la traduzione di intestazione richiesta per la regola di pubblicazione RDWeb. Questa operazione può essere eseguita tramite l'esecuzione di script di PowerShell seguente nel server Proxy applicazione Web, ma deve essere abilitato per impostazione predefinita:

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false 
    ```  

6.  Disabilitare la proprietà del cookie HttpOnly nel Proxy applicazione Web nel gateway desktop remoto pubblicata dell'applicazione. Per consentire l'accesso di controllo ActiveX di Gateway Desktop remoto per il cookie di autenticazione Proxy applicazione Web, è necessario disabilitare la proprietà HttpOnly nel cookie Proxy applicazione Web.  

    Ciò è necessario quanto segue per essere installata [Hotfix di Proxy applicazione Web](https://support.microsoft.com/en-gb/kb/3000850) o nella [ https://support.microsoft.com/en-gb/kb/3000850 ](https://support.microsoft.com/en-gb/kb/3000850).  

    Dopo aver installato l'hotfix eseguire lo script di PowerShell seguente nel server Proxy applicazione Web che specifica il nome dell'applicazione pertinente:  

    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true  
    ```  

    La disabilitazione di HttpOnly consente l'accesso di controllo ActiveX di Gateway Desktop remoto per il cookie di autenticazione Proxy applicazione Web.  

7.  Configurare la raccolta di Gateway Desktop remoto rilevante nel server di raccolta per consentire la connessione Desktop remoto (mstsc.exe) client può sapere che pre-autenticazione è necessaria nel file rdp.  

    -   In Windows Server 2012 e Windows Server 2012 R2 è possibile eseguendo i cmdlet di PowerShell seguente nel server Gateway Desktop remoto insieme:  

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```

        Assicurarsi di rimuovere le parentesi quadre quando si sostituisce con i propri valori, ad esempio < e >:

        ```
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```

    -   In Windows Server 2008 R2:  

        1.  Accedere al Server Terminal con un account con privilegi di amministratore.  

        2.  Passare a **avviare** >**strumenti di amministrazione** > **servizi Terminal** > **gestione RemoteApp di Servizi terminal.**  

        3.  Nel **Overview** riquadro di gestione di RemoteApp di Servizi terminal, accanto a impostazioni RDP, fare clic su **modifica**.  

        4.  Nel **impostazioni RDP personalizzate** scheda, digitare le seguenti impostazioni RDP nella casella di impostazioni RDP personalizzate:  

            `pre-authentication server address: s: https://externalfqdn/rdweb/`  

            `require pre-authentication:i:1`  

        5.  Al termine, fare clic su **applica**.  

            Ciò indica al server di raccolta per includere le proprietà RDP personalizzate nei file RDP che vengono inviati ai client. Questi elementi si indica al client che pre-autenticazione è necessario e passare i cookie per la pre-autenticazione ' indirizzo del server per il client connessione Desktop remoto (mstsc.exe). Questa operazione, in combinazione con la disabilitazione di HttpOnly nell'applicazione Proxy dell'applicazione Web, consente al client connessione Desktop remoto (mstsc.exe) usano il cookie di autenticazione Proxy applicazione Web ottenuto tramite il browser.  

            Per altre informazioni su RDP, vedere [configurare lo Scenario OTP di Gateway Servizi terminal](https://technet.microsoft.com/library/cc731249(v=ws.10).aspx).  

## <a name="BKMK_Links"></a>Vedere anche  

-   [Prevede di pubblicare applicazioni mediante Proxy applicazione Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))  

-   [Risoluzione dei problemi relativi a Proxy applicazione Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))  

-   [Guida allo scenario di Web Application Proxy](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))  
