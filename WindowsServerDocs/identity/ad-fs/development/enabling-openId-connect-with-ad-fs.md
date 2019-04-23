---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Creare un'applicazione web con OpenID Connect con AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74a493e6568b71a05116140ec67586d36f439aa8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882662"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>Creare un'applicazione web con OpenID Connect con AD FS 2016

>Si applica a: Windows Server 2016

Compila il supporto Oauth iniziale in ADFS in Windows Server 2012 R2, AD FS 2016 introduce il supporto per l'utilizzo di sign-on OpenId Connect.  
  
## <a name="pre-requisites"></a>Prerequisiti  
Di seguito sono un elenco di prerequisiti che sono necessarie prima del completamento di questo documento. In questo documento si presuppone che ADFS è stato installato e che è stata creata una farm ADFS.  
  
-   Strumenti client di GitHub  
  
-   AD FS in Windows Server 2016 TP4 o versione successiva  
  
-   Visual Studio 2013 o versione successiva.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Creare un gruppo di applicazioni in ADFS 2016  
Nella sezione seguente viene descritto come configurare il gruppo di applicazioni in AD FS 2016.  
  
#### <a name="create-application-group"></a>Creare gruppo di applicazioni  
  
1.  Nella gestione di ADFS, fare clic su gruppi di applicazioni e selezionare **Aggiungi gruppo di applicazioni**.  
  
2.  Creazione guidata gruppo di applicazioni, immettere il nome **ADFSSSO** e in **applicazioni autonome** selezionare il **application Server o un sito Web** modello.  Fare clic su **Avanti**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  Copia il **identificatore Client** valore.  E verrà essere utilizzato in un secondo momento come valore di ida: ClientId nel file Web. config delle applicazioni.  
  
4.  Immettere le informazioni seguenti per **URI di reindirizzamento:** - **https://localhost:44320/**.  Fai clic su **Aggiungi**. Fare clic su **Avanti**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  Nel **Configura applicazione credenziali** dello schermo, inserire un segno di spunta **generare una chiave privata condivisa** e copiare la chiave privata. Fare clic su **Avanti**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  Nel **riepilogo** schermata, fare clic su **Avanti**.  
  
7.  Nel **Complete** schermata, fare clic su **Chiudi**.  
  
8.  Ora, attiva il pulsante destro del mouse, il nuovo gruppo di applicazioni e selezionare **proprietà**.  
  
9. Nel **ADFSSSO proprietà** fare clic su **aggiungere applicazione**.  
  
10. Nel **aggiungere una nuova applicazione all'applicazione di esempio** selezionare **API Web** e fare clic su **Avanti**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. Nel **configurare Web API** schermata, immettere le informazioni seguenti per **Identifier** - **https://contoso.com/WebApp**.  Fai clic su **Aggiungi**. Fare clic su **Avanti**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG) 
    
12. Nel **scegliere Criteri di controllo di accesso** selezionare **consentire tutti gli utenti** e fare clic su **Avanti**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. Nel **configurare autorizzazioni per l'applicazione** schermata, assicurarsi che **openid** è selezionata e fare clic su **Avanti**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. Nel **riepilogo** schermata, fare clic su **Avanti**.  
  
15. Nel **Complete** schermata, fare clic su **Chiudi**.  
  
16. Nel **proprietà dell'applicazione di esempio** fare clic su **OK**.  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>Scaricare e modificare App MVP per l'autenticazione tramite OpenId Connect e ADFS  
In questa sezione viene illustrato come scaricare l'esempio di API Web e modificarla in Visual Studio.   Verrà usato l'esempio di Azure AD che è [qui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  
  
Per scaricare il progetto di esempio, utilizzare Git Bash e digitare quanto segue:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>Per modificare l'applicazione  
  
1.  Aprire l'esempio utilizzando Visual Studio.  
  
2.  Compilare l'applicazione in modo che tutti i NuGets mancanti vengono ripristinati.  
  
3.  Aprire il file Web. config.  Modificare i valori seguenti in modo che l'aspetto simile al seguente:  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  Aprire il file Startup.Auth.cs e apportare le modifiche seguenti:  
  
    -   Impostare come commento le operazioni seguenti:  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
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
  
    -   Ricorra verso il basso, modificare le opzioni di middleware OpenId Connect come illustrato di seguito:  
  
        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                RedirectUri = postLogoutRedirectUri,  
                PostLogoutRedirectUri = postLogoutRedirectUri 
        ```  
  
        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  
  
        Modificando il codice precedente ci stiamo le operazioni seguenti:  
  
        -   Invece di usare l'autorità per la comunicazione i dati relativi all'emittente attendibile, specifichiamo il percorso di documento di individuazione direttamente tramite MetadataAddress  
  
        -   Azure AD non impone la presenza di un redirect_uri nella richiesta, a differenza ADFS. Pertanto, è necessario aggiungerlo qui  
  
## <a name="verify-the-app-is-working"></a>Verificare il che funzionamento dell'app  
Una volta apportate le modifiche precedenti, premere F5.  Verrà visualizzata la pagina di esempio.  Fare clic su Accedi.  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  
  
Si sarà reindirizzati alla pagina di accesso di ADFS.  Andare avanti ed eseguire l'accesso.  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  
  
Dopo l'operazione viene completata dovrebbe essere ora verrà effettuato.  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  
  
## <a name="next-steps"></a>Passaggi successivi
[Sviluppare AD FS](../../ad-fs/AD-FS-Development.md)  

