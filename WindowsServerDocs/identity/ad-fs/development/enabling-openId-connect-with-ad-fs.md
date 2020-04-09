---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Creare un'applicazione Web con OpenID Connect con AD FS 2016 e versioni successive
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 49d952a49cf474708f57a0ae2a7760d2470af607
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857494"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>Creare un'applicazione Web con OpenID Connect con AD FS 2016 e versioni successive

## <a name="pre-requisites"></a>Prerequisiti  
Di seguito sono un elenco di prerequisiti che sono necessarie prima del completamento di questo documento. In questo documento si presuppone che ADFS è stato installato e che è stata creata una farm ADFS.  

-   Strumenti client di GitHub  

-   AD FS in Windows Server 2016 TP4 o versione successiva  

-   Visual Studio 2013 o versione successiva.  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>Creare un gruppo di applicazioni in AD FS 2016 e versioni successive
Nella sezione seguente viene descritto come configurare il gruppo di applicazioni in AD FS 2016 e versioni successive.  

#### <a name="create-application-group"></a>Creare gruppo di applicazioni  

1.  Nella gestione di ADFS, fare clic su gruppi di applicazioni e selezionare **Aggiungi gruppo di applicazioni**.  

2.  Nella creazione guidata gruppo di applicazioni, per il nome immettere **ADFSSSO** e in **applicazioni client-Server** Selezionare il **Web browser accedere a un modello di applicazione Web** .  Fare clic su **Avanti**.

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  Copia il **identificatore Client** valore.  E verrà essere utilizzato in un secondo momento come valore di ida: ClientId nel file Web. config delle applicazioni.  

4.  Immettere quanto segue per l' **URI di reindirizzamento:**  -  **https://localhost:44320/** .  Fare clic su **Add**. Fare clic su **Avanti**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  Nel **riepilogo** schermata, fare clic su **Avanti**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  Nel **Complete** schermata, fare clic su **Chiudi**.  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>Scaricare e modificare l'applicazione di esempio per l'autenticazione tramite OpenID Connect e AD FS  
Questa sezione illustra come scaricare l'APP Web di esempio e modificarla in Visual Studio.   Verrà usato l'esempio di Azure AD che è [qui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Per scaricare il progetto di esempio, utilizzare Git Bash e digitare quanto segue:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>Per modificare l'applicazione  

1.  Aprire l'esempio utilizzando Visual Studio.  

2.  Ricompilare l'app in modo da ripristinare tutti i NuGet mancanti.  

3.  Aprire il file Web. config.  Modificare i valori seguenti in modo che l'aspetto simile al seguente:  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  Aprire il file Startup.Auth.cs e apportare le modifiche seguenti:  

    -   Impostare come commento le operazioni seguenti:  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   Modificare la logica l'inizializzazione di middleware OpenId Connect con le seguenti modifiche:  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  

    -   Successivamente, modificare le opzioni del middleware OpenId Connect come riportato di seguito:  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  

        Modificando il codice precedente ci stiamo le operazioni seguenti:  

        -   Anziché utilizzare l'autorità per la comunicazione dei dati relativi all'autorità emittente attendibile, si specifica il percorso del documento di individuazione direttamente tramite MetadataAddress  

        -   Azure AD non impone la presenza di un redirect_uri nella richiesta, a differenza ADFS. Pertanto, è necessario aggiungerlo qui  

## <a name="verify-the-app-is-working"></a>Verificare il che funzionamento dell'app  
Una volta apportate le modifiche precedenti, premere F5.  Verrà visualizzata la pagina di esempio.  Fare clic su Accedi.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  

Si sarà reindirizzati alla pagina di accesso di ADFS.  Andare avanti ed eseguire l'accesso.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  

Dopo l'operazione viene completata dovrebbe essere ora verrà effettuato.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  

## <a name="next-steps"></a>Passaggi successivi
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
