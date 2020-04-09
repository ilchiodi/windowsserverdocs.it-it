---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: Creare un'applicazione lato server usando client riservati OAuth con AD FS 2016 o versione successiva
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8a8567a497e10df66f77fb996c937791b4aa9e08
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857564"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016-or-later"></a>Creare un'applicazione lato server usando client riservati OAuth con AD FS 2016 o versione successiva


AD FS 2016 e versioni successive forniscono supporto per i client in grado di gestire il proprio segreto, ad esempio un'app o un servizio in esecuzione su un server Web.  Questi client sono noti come client riservati.
Di seguito è uno schema di un'applicazione web in esecuzione su un server web e che funge da un client riservato per ADFS:  

## <a name="pre-requisites"></a>Prerequisiti  
Di seguito sono un elenco di prerequisiti che sono necessarie prima del completamento di questo documento. In questo documento si presuppone che AD FS sia stato installato.  

-   Strumenti client di GitHub  

-   AD FS in Windows Server 2016 TP4 o versione successiva  

-   Visual Studio 2013 o versione successiva.  

## <a name="create-an-application-group-in-ad-fs-2016-or-later"></a>Creare un gruppo di applicazioni in AD FS 2016 o versione successiva
Nella sezione seguente viene descritto come configurare il gruppo di applicazioni in AD FS 2016 o versione successiva.  

#### <a name="create-the-application-group"></a>Creare il gruppo di applicazioni  

1.  Nella gestione di ADFS, fare clic su gruppi di applicazioni e selezionare **Aggiungi gruppo di applicazioni**.  

2.  Nella creazione guidata gruppo di applicazioni, per il **nome** immettere **ADFSOAUTHCC** e in **applicazioni client-server** selezionare l' **applicazione server che accede a un modello di API Web** .  Fare clic su **Avanti**.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  

3.  Copia il **identificatore Client** valore.  Verrà utilizzato in un secondo momento come valore per **ida: ClientId** nel file Web. config dell'applicazione.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  

4.  Immettere quanto segue per l' **URI di reindirizzamento:**  -  **https://localhost:44323** .  Fare clic su **Add**. Fare clic su **Avanti**.  

5.  Nel **Configura applicazione credenziali** dello schermo, inserire un segno di spunta **generare una chiave privata condivisa** e copiare la chiave privata.  Che verrà usato in un secondo momento come valore per **Ida: ClientSecret** nel file Web. config delle applicazioni.  Fare clic su **Avanti**.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)   

6. Nella schermata **Configura API Web** immettere quanto segue per **identificatore** -  **https://contoso.com/WebApp** .  Fare clic su **Add**. Fare clic su **Avanti**.  Questo valore verrà usato successivamente per **ida: GraphResourceId** nel file Web. config dell'applicazione.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  

7. Nella schermata **Applica criteri di controllo di accesso** selezionare **Consenti tutti gli utenti** e fare clic su **Avanti**.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  

8. Nella schermata **Configura autorizzazioni** per le applicazioni verificare che **OpenID** e **user_impersonation** siano selezionati e fare clic su **Avanti**.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  

9. Nel **riepilogo** schermata, fare clic su **Avanti**.  

10. Nel **Complete** schermata, fare clic su **Chiudi**.  

## <a name="upgrade-the-database"></a>Aggiornare il database  
Creazione di questa procedura dettagliata è stato utilizzato Visual Studio 2015.   Per eseguire l'esempio di utilizzo di Visual Studio 2015, è necessario aggiornare il file di database.  Utilizzare la procedura seguente per eseguire questa operazione.  

In questa sezione viene illustrato come scaricare l'esempio di API Web e aggiornare il database in Visual Studio 2015.   Verrà usato l'esempio di Azure AD che è [qui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).  

Per scaricare il progetto di esempio, utilizzare Git Bash e digitare quanto segue:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  

![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  

#### <a name="to-upgrade-the-database-file"></a>Per aggiornare il file di database  

1.  Aprire il progetto in Visual Studio. verrà visualizzata una finestra popup che informa che l'app richiede SQL Server 2012 Express o sarà necessario aggiornare il database.  Scegliere OK.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  

2.  Compilazione successiva dell'applicazione selezionando Genera -> Compila soluzione nella parte superiore.  Verranno ripristinate tutti i pacchetti NuGet.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  

3.  Nella parte superiore, selezionare **visualizzazione** -> **Esplora Server**.  Una volta che viene aperto, in **connessioni dati**, fare doppio clic su **DefaultConnection** e selezionare **Modifica connessione**.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  

4.  In **Modifica connessione**, in **nome file di database (nuovo o esistente)** Selezionare **Sfoglia** e specificare **path\filename.MDF**. Fare clic su **Sì** nella finestra di dialogo.

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)

5.  In **Modifica connessione**, selezionare **Avanzate**.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  

6.  In proprietà avanzate, individuare l'origine dati e utilizzare l'elenco a discesa per modificarlo da **(LocalDb\v11.0)** a **(local DB) \MSSQLLocalDB**.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  

7.  Scegliere OK. Scegliere OK.  Fare clic su Sì per aggiornare il database.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  

8.  Dopo il completamento, oltre a destra, copiare il valore nella casella accanto a **stringa di connessione.**  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  

9.  A questo punto, aprire il file Web. config e sostituire il valore di connectionString con il valore copiato in precedenza.  Salvare il file Web.config.  

    > [!NOTE]  
    > I passaggi precedenti sono necessari in modo che è possibile ottenere connectionString nuovo.  In caso contrario, quando si esegue Update-Database seguente verrà generato un errore.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  

10. Nella parte superiore di Visual Studio, selezionare **visualizzazione** -> **altre finestre** -> **Console di gestione pacchetti**.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  

11. Nella parte inferiore, nella Console di gestione pacchetti immettere:  `Enable-Migrations` e premere INVIO.  

    > [!NOTE]  
    > Se viene visualizzato un messaggio di errore Enable-Migrations non è riconosciuto come un cmdlet, immettere Install-Package EntityFramework per aggiornare la EntityFramework.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  

12. Nella parte inferiore, nella Console di gestione pacchetti immettere:  `Add-Migration <anynamehere>` e premere INVIO.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  

13. Nella parte inferiore, nella Console di gestione pacchetti immettere:  `Update-Database` e premere INVIO.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  

## <a name="modify-the-webapi-in-visual-studio"></a>Modificare l'API Web in Visual Studio  

#### <a name="to-modify-the-sample-web-api"></a>Per modificare l'API Web di esempio  

1.  Aprire l'esempio utilizzando Visual Studio.  

2.  Aprire il file Web. config.  Modificare i valori seguenti:  

    -   Ida: ClientID: immettere il valore da #3 nella sezione creare il gruppo di applicazioni precedente.  

    -   Ida: ClientSecret-immettere il valore da #5 nella sezione creare il gruppo di applicazioni precedente.  

    -   Ida: GraphResourceId-immettere il valore da #6 nella sezione creare il gruppo di applicazioni precedente.  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  

3.  Aprire il file Startup.Auth.cs in App_Start e apportare le modifiche seguenti:  

    -   Impostare come commento le righe seguenti:  

        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   Aggiungere quanto segue nella relativa posizione:  

        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  

        dove < your_fsname > viene sostituito con la parte DNS di url del servizio federativo, ad esempio adfs.contoso.com  

        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  

4.  Aprire il file UserProfileController.cs e apportare le modifiche seguenti:  

    -   Impostare come commento le operazioni seguenti:  

        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  

    -   Sostituire le due occorrenze con il codice seguente:  

        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  

        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  

    -   Impostare come commento le operazioni seguenti:  

        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  

    -   Sostituire le due occorrenze con il codice seguente:  

        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  

        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  

    -   Ora commento tutte le istanze delle operazioni seguenti:  

        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString() + "/OAuth");  
        ```  

    -   Sostituire tutte le occorrenze con il codice seguente:  

        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString());  
        ```  

        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  

## <a name="test-the-solution"></a>Testare la soluzione  
In questa sezione verranno testate le soluzioni client riservato.  Utilizzare la procedura seguente per testare la soluzione.  

#### <a name="testing-the-confidential-client-solution"></a>Test della soluzione client riservato  

1. Nella parte superiore di Visual Studio, verificare che Internet Explorer sia selezionata e fare clic sulla freccia verde.  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  

2. Una volta visualizzata la pagina ASP.Net, fare clic su **Register (registra** ) in alto a destra nella pagina.  Immettere un nome utente e una password e quindi fare clic sul pulsante **Register (registra** ).  Consente di creare un account locale nel database SQL.  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  

3. Si noti ora che il sito di ASP.NET dice Hello abby@contoso.com!.  Fare clic su **Profile**.  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  

4. Questo consente di visualizzare una pagina senza le informazioni e che è necessario fare clic qui per accedere.  Fare clic su **qui**.  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  

5. Verrà richiesto di accedere ad ADFS.  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  

## <a name="next-steps"></a>Passaggi successivi
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
