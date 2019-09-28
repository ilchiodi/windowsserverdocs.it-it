---
title: Personalizzare le attestazioni da emettere in token ID quando si usa OpenID Connect o OAuth con AD FS 2016 o versione successiva
description: Panoramica tecnica sui concetti relativi ai token ID personalizzati in AD FS 2016 o versione successiva
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 88ae6837872c5a6cf6bb1d8533a0aa14b82ca573
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358914"
---
# <a name="customize-claims-to-be-emitted-in-id_token-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>Personalizzare le attestazioni da emettere in token ID quando si usa OpenID Connect o OAuth con AD FS 2016 o versione successiva

## <a name="overview"></a>Panoramica
[Questo articolo illustra](native-client-with-ad-fs.md) come compilare un'app che usa ad FS per l'accesso OpenID Connect. Per impostazione predefinita, tuttavia, in token ID è disponibile solo un set fisso di attestazioni. AD FS 2016 e versioni successive sono in grado di personalizzare token ID in scenari di OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>Quando si usa un token ID personalizzato?
In alcuni scenari è possibile che l'applicazione client non disponga di una risorsa a cui si sta tentando di accedere. Quindi, non è necessario un token di accesso. In questi casi, l'applicazione client richiede essenzialmente solo un token ID, ma con alcune attestazioni aggiuntive per la funzionalità.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quali sono le restrizioni per ottenere le attestazioni personalizzate nel token ID?

### <a name="scenario-1"></a>Scenario 1

![limitare](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode è impostato come form_post
2.  Solo i client pubblici possono ottenere le attestazioni personalizzate nel token ID
3.  L'identificatore della relying party (identificatore dell'API Web) deve essere uguale all'identificatore client

### <a name="scenario-2"></a>Scenario 2

![limitare](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Con [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) installato nei server ad FS
1.  response_mode è impostato come form_post
2.  I client pubblici e riservati possono ottenere attestazioni personalizzate nel token ID
3.  Assegnare l'ambito allatclaims alla coppia client-RP.
È possibile assegnare l'ambito usando il cmdlet Grant-ADFSApplicationPermission come indicato nell'esempio seguente:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Creazione e configurazione di un'applicazione OAuth per la gestione di attestazioni personalizzate nel token ID
Attenersi alla procedura seguente per creare e configurare l'applicazione in AD FS per la ricezione del token ID con attestazioni personalizzate.

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>Creare e configurare un gruppo di applicazioni in AD FS 2016 o versione successiva

1. Nella gestione di ADFS, fare clic su gruppi di applicazioni e selezionare **Aggiungi gruppo di applicazioni**.

2. Nella creazione guidata gruppo di applicazioni, per il nome immettere **ADFSSSO** e in applicazioni client-server selezionare l' **applicazione nativa che accede a un modello di applicazione Web** . Fare clic su **Avanti**.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. Copia il **identificatore Client** valore.  E verrà essere utilizzato in un secondo momento come valore di ida: ClientId nel file Web. config delle applicazioni.

4. Immettere quanto segue per l' **URI di reindirizzamento:**  -  **https://localhost:44320/** .  Fai clic su **Aggiungi**. Fare clic su **Avanti**.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. Nella schermata **Configura API Web** immettere quanto segue per identificatore. -  **https://contoso.com/WebApp**  Fai clic su **Aggiungi**. Fare clic su **Avanti**.  Questo valore verrà usato in un secondo momento per **Ida: ResourceId** nel file Web. config delle applicazioni.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. Nel **scegliere Criteri di controllo di accesso** selezionare **consentire tutti gli utenti** e fare clic su **Avanti**.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. Nella schermata **Configura autorizzazioni** per le applicazioni verificare che **OpenID** e **allatclaims** siano selezionati e fare clic su **Avanti**.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. Nel **riepilogo** schermata, fare clic su **Avanti**.  

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. Nel **Complete** schermata, fare clic su **Chiudi**.

10. In gestione AD FS fare clic su gruppi di applicazioni per ottenere l'elenco di tutti i gruppi di applicazioni. Fare clic con il pulsante destro del mouse su **ADFSSSO** e scegliere **Proprietà**. Selezionare **ADFSSSO-API Web** e fare clic su **modifica...**

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. Nella schermata delle **proprietà dell'API Web di ADFSSSO** selezionare la scheda **regole di trasformazione rilascio** e fare clic su **Aggiungi regola...**

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. Nella schermata **Aggiunta guidata regola attestazione di trasformazione** selezionare **Invia attestazioni usando una regola personalizzata** nell'elenco a discesa e fare clic su **Avanti** .

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. Nella schermata **Aggiunta guidata regola attestazione di trasformazione** immettere **ForCustomIDToken** in **Nome regola attestazione** e la regola attestazione seguente nella **regola personalizzata**. Fare clic su **fine**

    ```  
    x:[]
    => issue(claim=x);  
    ```

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.png)

```

>[!NOTE]
>You can also use PowerShell to assign the allatclaims and openid scopes
>``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-id_token"></a>Scaricare e modificare l'applicazione di esempio per creare attestazioni personalizzate in token ID

Questa sezione illustra come scaricare l'APP Web di esempio e modificarla in Visual Studio.   Verrà usato l'esempio di Azure AD che è [qui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Per scaricare il progetto di esempio, utilizzare Git Bash e digitare quanto segue:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>Per modificare l'applicazione

1.  Aprire l'esempio utilizzando Visual Studio.  

2.  Ricompilare l'app in modo da ripristinare tutti i NuGet mancanti.  

3.  Aprire il file Web. config.  Modificare i valori seguenti in modo che l'aspetto simile al seguente:  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 above under section Create and configure an Application Group in AD FS 2016 or later]" />  
    <add key="ida:ResourceID" value="[Replace this with the Web API Identifier from #5 above]"  />
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with the Redirect URI from #4 above]" />  
    ```  

    ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_2.PNG)  

4.  Aprire il file Startup.Auth.cs e apportare le modifiche seguenti:  

    -   Modificare la logica l'inizializzazione di middleware OpenId Connect con le seguenti modifiche:  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   Impostare come commento le operazioni seguenti:  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   Successivamente, modificare le opzioni del middleware OpenId Connect come riportato di seguito:  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                Resource = resourceId,
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_4.PNG)

5.  Aprire il file HomeController.cs e apportare le modifiche seguenti:  

    -   Aggiungi quanto segue:  

            ```  
            using System.Security.Claims;  
            ```

    -   Aggiornare il metodo about () come illustrato di seguito:  

        ```  
        [Authorize]
        public ActionResult About()
        {
            ClaimsPrincipal cp = ClaimsPrincipal.Current;
            string userName = cp.FindFirst(ClaimTypes.WindowsAccountName).Value;
            ViewBag.Message = String.Format("Hello {0}!", userName);
            return View();
        }
        ```  

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_5.PNG)

### <a name="test-the-custom-claims-in-id-token"></a>Testare le attestazioni personalizzate nel token ID

Una volta apportate le modifiche precedenti, premere F5. Verrà visualizzata la pagina di esempio. Fare clic su Accedi.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

Si sarà reindirizzati alla pagina di accesso di ADFS. Andare avanti ed eseguire l'accesso.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

Dopo l'operazione viene completata dovrebbe essere ora verrà effettuato.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

Fare clic sul collegamento informazioni. Verrà visualizzato Hello [username] recuperato dall'attestazione username nel token ID

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>Passaggi successivi
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
