---
title: Compilare un'applicazione web a pagina singola con OAuth e ADAL. JS con AD FS 2016 o versione successiva
description: Una procedura dettagliata che fornisce istruzioni per l'autenticazione in AD FS Usa ADAL per JavaScript proteggere una AngularJS basata applicazione a pagina singola
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/13/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: f8a8d6b81f63a691954eecf02dba4e33215a462a
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811743"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>Compilare un'applicazione web a pagina singola con OAuth e ADAL. JS con AD FS 2016 o versione successiva

Questa procedura dettagliata fornisce istruzioni per l'autenticazione in AD FS Usa ADAL per proteggere una AngularJS JavaScript basato su applicazione a singola pagina, implementata con un back-end API Web ASP.NET.

In questo scenario, quando l'utente accede, il codice JavaScript front-end Usa [Active Directory Authentication Library per JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) e concessione di autorizzazione implicita per ottenere un token ID (id_token) da Azure AD. Il token viene memorizzato nella cache e il client lo associa alla richiesta come token di connessione quando si effettuano chiamate alla propria API Web back-end, che è protetto tramite il middleware OWIN.

>[!IMPORTANT]
>L'esempio che è possibile compilare qui è solo a scopo didattico. Queste istruzioni sono per l'implementazione più semplice, più minima possibile esporre gli elementi necessari del modello. L'esempio potrebbe non includere tutti gli aspetti della gestione degli errori e altri riguardano funzionalità.

>[!NOTE]
>Questa procedura dettagliata riguarda **solo** al Server AD FS 2016 e versioni successive 

## <a name="overview"></a>Panoramica
In questo esempio verranno creati un flusso di autenticazione in cui un client applicazione singola pagina verrà autenticati su AD FS per proteggere l'accesso alle risorse al back-end WebAPI. Di seguito è riportato il flusso di autenticazione generale


![Autorizzazione di AD FS](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Quando si usa un'applicazione a singola pagina, l'utente passa a una posizione di inizio, da dove a partire da pagina e una raccolta di file JavaScript e HTML visualizzazioni vengono caricate. È necessario configurare il protocollo Active Directory Authentication Library (ADAL) per altre informazioni critiche relative all'applicazione, vale a dire l'istanza di AD FS, ID client, in modo da poter indirizzare l'autenticazione per AD FS.

Se ADAL rileva un trigger per l'autenticazione, Usa le informazioni fornite dall'applicazione e indirizza l'autenticazione per il ruolo STS ADFS di Active Directory.  L'applicazione a pagina singola, che viene registrato come un client pubblico in AD FS, viene configurato automaticamente per il flusso di concessione implicita. La richiesta di autorizzazione genera un token ID che viene restituito all'applicazione tramite un #fragment. Ulteriori chiamate al back-end WebAPI includeranno il token ID come token di connessione nell'intestazione per ottenere l'accesso per l'API Web.

## <a name="setting-up-the-development-box"></a>Impostare la casella di sviluppo
Questa procedura dettagliata viene utilizzato Visual Studio 2015. Il progetto usa la libreria ADAL JS. Per informazioni su ADAL, leggere [Active Directory Authentication Library .NET.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Impostazione dell'ambiente
Per questa procedura dettagliata si userà una configurazione di base:

1.  CONTROLLER DI DOMINIO: Controller di dominio per il dominio in cui verrà ospitato ADFS
2.  Server AD FS: Il Server ADFS per il dominio
3.  Computer di sviluppo: Computer in cui è installato Visual Studio e verrà sviluppo di esempio

È possibile, se si desidera, utilizzare solo due macchine. Uno per controller di dominio/AD FS e l'altra per lo sviluppo dell'esempio.

Come configurare il controller di dominio e ADFS esula dall'ambito di questo articolo. Per informazioni aggiuntive sulla distribuzione, vedere:

- [Distribuzione di Active Directory Domain Services](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Distribuzione di AD FS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clonare o scaricare questo repository
Si utilizzerà l'applicazione di esempio creata per l'integrazione di Azure AD in un'app a pagina singola AngularJS e modificarlo invece proteggere la risorsa back-end usando AD FS.

Dalla shell o della riga di comando:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Informazioni sul codice
I file di chiave che contiene la logica di autenticazione sono i seguenti:

**App. js** - inserisce la dipendenza modulo ADAL, fornisce i valori di configurazione di app usati da ADAL per guidare le interazioni di protocollo con AAD e indica le route che non è consentito accedervi senza autenticazione precedente.

**index. HTML** -contiene un riferimento a adal. js

**HomeController.js**-illustra come sfruttare i vantaggi dei metodi Login () e logOut() in ADAL.

**UserDataController.js** -illustra come estrarre informazioni sull'utente dall'id_token memorizzato nella cache.

**Startup.Auth.cs** -contiene la configurazione per l'API Web usare Active Directory Federation Services per l'autenticazione della connessione.

## <a name="registering-the-public-client-in-ad-fs"></a>Registrazione del client pubblico in AD FS
Nell'esempio, l'API Web è configurato per ascoltare https://localhost:44326/. Il gruppo di applicazioni **browser Web che accede a un'applicazione web** può essere utilizzato per la configurazione dell'applicazione flusso di concessione implicita.

1. Aprire la console di gestione di ADFS e fare clic su **Aggiungi gruppo di applicazioni**. Nel **Aggiunta guidata gruppo di applicazioni** immettere il nome dell'applicazione, descrizione e selezionare il **l'accesso a un'applicazione web di Web browser** modello dal **Client-Server le applicazioni** sezione come illustrato di seguito

    ![Crea nuovo gruppo di applicazioni](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. Nella pagina successiva **applicazione nativa**, specificare l'identificatore client dell'applicazione e URI di reindirizzamento, come illustrato di seguito

    ![Crea nuovo gruppo di applicazioni](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. Nella pagina successiva **si applicano criteri di controllo di accesso** lasciare le autorizzazioni necessarie per *consentire tutti gli utenti*

4. Nella pagina di riepilogo dovrebbe essere simile al seguente

    ![Crea nuovo gruppo di applicazioni](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Fare clic su **successivo** per completare l'aggiunta del gruppo di applicazioni e chiudere la procedura guidata.

## <a name="modifying-the-sample"></a>Modifica il codice di esempio
Configurare ADAL JS

Aprire il **app. js** file e modificare le **adalProvider.init** definizione per:

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|Configurazione|Descrizione|
|--------|--------|
|Istanza|L'URL del servizio token di sicurezza, ad esempio, https://fs.contoso.com/|
|tenant|Mantenerlo come 'adfs'|
|clientID|Questo è l'ID client specificato durante la configurazione del client pubblico per l'applicazione a pagina singola|

## <a name="configure-webapi-to-use-ad-fs"></a>Configurare l'API Web per usare AD FS
Aprire il **Startup.Auth.cs** nell'esempio di file e aggiungere il codice seguente all'inizio:

    using System.IdentityModel.Tokens;

Rimuovere:

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

e aggiungere:

    app.UseActiveDirectoryFederationServicesBearerAuthentication(
    new ActiveDirectoryFederationServicesBearerAuthenticationOptions
    {
    MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
    TokenValidationParameters = new TokenValidationParameters()
    {
    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"],
    ValidIssuer = ConfigurationManager.AppSettings["ida:Issuer"]
    }
    }
    );

|Parametro|Descrizione|
|--------|--------|
|ValidAudience|Ciò consente di configurare il valore di "audience" che verrà confrontato con il token|
|ValidIssuer|Ciò consente di configurare il valore di ' autorità di certificazione che verrà confrontato con il token|
|MetadataEndpoint|Indica le informazioni sui metadati del servizio token di sicurezza|

## <a name="add-application-configuration-for-ad-fs"></a>Aggiungere la configurazione dell'applicazione per AD FS
Modificare appsettings come indicato di seguito:

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>Esecuzione della soluzione
Pulire la soluzione, ricompilare la soluzione ed eseguirla. Se si desidera visualizzare le tracce dettagliate, avviare Fiddler e abilitare la decrittografia HTTPS.

Il browser (usare browser Chrome) verrà caricata l'applicazione a singola pagina e verrà visualizzata la schermata seguente:

![Registrare il client](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Fare clic sull'account di accesso.  L'elenco ToDo attiverà il flusso di autenticazione e ADAL JS indirizzerà l'autenticazione ad ADFS

![Login](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

In Fiddler è possibile visualizzare il token viene restituito come parte dell'URL nel frammento di &.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Sarà possibile a questo punto chiamare l'API back-end per aggiungere elementi elenco attività per l'utente ha eseguito l'accesso:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Passaggi successivi
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
