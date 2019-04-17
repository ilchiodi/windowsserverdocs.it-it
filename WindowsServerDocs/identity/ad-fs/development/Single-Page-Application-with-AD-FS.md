---
title: Creare un'applicazione web singola pagina con OAuth e ADAL.JS con AD FS 2016
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 7b3d48e1e38baffeb84b1f236efb43cfda5048c0
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016"></a>Creare un'applicazione web singola pagina con OAuth e ADAL.JS con AD FS 2016

>Si applica a: Windows Server 2016

Questa procedura dettagliata fornisce istruzioni per l'autenticazione con ADFS con ADAL per proteggere un AngularJS JavaScript basata su applicazione a singola pagina, implementato con un back-end di API Web ASP.NET.

>Avviso: L'esempio che è possibile compilare qui è solo a scopo didattico. Queste istruzioni sono per l'implementazione più semplice, più minima possibile esporre gli elementi necessari del modello. L'esempio potrebbe non includere tutti gli aspetti della gestione degli errori e altri riguardano funzionalità.

>[!NOTE]
>This walkthrough is applicable **only** to AD FS Server 2016 and higher 

## <a name="overview"></a>Panoramica
In questo esempio creeremo un flusso di autenticazione in ADFS per proteggere l'accesso alle risorse di API Web back-end in cui verrà autenticati un'applicazione client singola pagina. Di seguito è il flusso di autenticazione generale


![Autorizzazione di AD FS](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Quando si utilizza un'applicazione a singola pagina, l'utente si sposta in una posizione di partenza, da dove partire pagina e una raccolta di file JavaScript e HTML visualizzazioni vengono caricate. È necessario configurare il protocollo Active Directory Authentication Library (ADAL) per conoscere le informazioni critiche relative all'applicazione, ad esempio, l'istanza di AD FS, ID client, in modo da poter indirizzare l'autenticazione di ADFS.

Se ADAL vede un trigger per l'autenticazione, Usa le informazioni fornite dall'applicazione e indirizza l'autenticazione per il servizio token di sicurezza ADFS Active Directory.  L'applicazione a pagina singola, viene registrato come un client pubblico in ADFS, viene configurato automaticamente per il flusso di concedere implicito. La richiesta di autorizzazione genera un token di ID che viene restituito all'applicazione tramite un #fragment. Altre chiamate per il back-end WebAPI trasporterà questo token ID come token bearer nell'intestazione per ottenere l'accesso per l'API Web.

## <a name="setting-up-the-development-box"></a>Configurare la casella sviluppo
Questa procedura dettagliata viene utilizzato Visual Studio 2015. Il progetto utilizza la libreria ADAL JS. Per informazioni su ADAL, leggere [libreria di classi .NET l'autenticazione di Active Directory.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Configurazione dell'ambiente
Per questa procedura dettagliata verrà usato una configurazione di base:

1.  : Controller di dominio Controller di dominio per il dominio in cui verrà ospitato ADFS
2.  Server ADFS: Il Server ADFS per il dominio
3.  Computer di sviluppo: Computer in cui è installato Visual Studio e sviluppo di esempio

È possibile, se si desidera, utilizzare solo due macchine. Uno per il controller di dominio/ADFS e l'altro per lo sviluppo dell'esempio.

Come configurare il controller di dominio e AD FS non rientra nell'ambito di questo articolo. Per informazioni aggiuntive sulla distribuzione, vedere:

- [Distribuzione di Active Directory](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [Distribuzione di ADFS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clonare o scaricare questo repository
Abbiamo applicazione di esempio creata per l'integrazione di Azure AD in un'app singola pagina AngularJS e la modifica verrà utilizzato per proteggere invece della risorsa back-end con AD FS.

Dalla shell o della riga di comando:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Informazioni sul codice
Di seguito sono elencati i file della chiave che contiene la logica di autenticazione.

**App.js** - inserisce la dipendenza del modulo ADAL, fornisce i valori di configurazione app utilizzati da ADAL per le interazioni di protocollo con AAD e indica le route non devono essere accedere senza autenticazione precedente.

**index.HTML** -contiene un riferimento a adal.js

**HomeController.js**-illustra come sfruttare i metodi Login () e logOut() in ADAL.

**UserDataController.js** -viene illustrato come estrarre informazioni utente da id_token memorizzato nella cache.

**Startup.Auth.cs** -contiene la configurazione per l'API Web come utilizzare Active Directory Federation Services per l'autenticazione bearer.

## <a name="registering-the-public-client-in-ad-fs"></a>Registrazione di client pubblico in AD FS
Nell'esempio, l'API Web è configurato per l'ascolto su https://localhost:44326 /. Il gruppo di applicazioni **Web browser, l'accesso a un'applicazione web** può essere usato per configurazione applicazione flusso grant implicito.

1. Aprire la console di gestione di ADFS e fare clic su **Aggiungi gruppo di applicazioni**. Nel **Aggiunta guidata gruppo di applicazioni** immettere il nome dell'applicazione, descrizione e selezionare il **Web browser, l'accesso a un'applicazione web** modello dal **applicazioni Client e Server** sezione come illustrato di seguito
    <br>![Crea nuovo gruppo di applicazioni](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. Nella pagina successiva **applicazione nativa**, specificare l'identificatore client e URI di reindirizzamento, come illustrato di seguito
    <br>![Crea nuovo gruppo di applicazioni](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. Nella pagina successiva **applicare criteri di controllo di accesso** lasciare le autorizzazioni *consentire tutti gli utenti*

4. Pagina di riepilogo è simile al seguente
    <br>![Crea nuovo gruppo di applicazioni](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Fare clic su **Avanti** per completare l'aggiunta del gruppo di applicazioni e chiudere la procedura guidata.

## <a name="modifying-the-sample"></a>Modifica il codice di esempio
Configurare ADAL JS

Aprire il **app.js** file e modificare il **adalProvider.init** definizione per:

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|Configurazione|Descrizione
|--------|--------
|istanza|URL di servizio token di sicurezza, ad esempio https://fs.contoso.com/
|tenant|Mantenerlo come 'adfs'
|clientID|Questo è l'ID client specificato durante la configurazione del client pubblico per l'applicazione a singola pagina

## <a name="configure-webapi-to-use-ad-fs"></a>Configurare l'API Web per l'utilizzo di ADFS
Aprire il **Startup.Auth.cs** nell'esempio di file e Aggiungi il codice seguente all'inizio: 

    using System.IdentityModel.Tokens;

rimuovere:

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

|Parametro|Descrizione
|--------|--------
|ValidAudience|Ciò consente di configurare il valore di 'pubblico' che verrà confrontato con il token
|ValidIssuer|Ciò consente di configurare il valore di ' dell'autorità di certificazione che verrà confrontato con il token
|MetadataEndpoint|Questo fa riferimento alle informazioni dei metadati del tuo servizio token di sicurezza

## <a name="add-application-configuration-for-ad-fs"></a>Aggiungere la configurazione dell'applicazione per AD FS
Modificare appsettings come indicato di seguito:

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>Esecuzione della soluzione
Pulire la soluzione, ricostruire la soluzione ed eseguirlo. Se si desidera visualizzare le tracce dettagliate, avvia Fiddler e abilitare la decrittografia HTTPS.

Il browser verrà caricato il SPA e verrà visualizzata la schermata seguente:

![Registrare il client](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Fare clic su Login.  L'elenco ToDo si attiverà il flusso di autenticazione e JS ADAL indirizzeranno l'autenticazione ad ADFS

![Account di accesso](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

In Fiddler è possibile visualizzare il token viene restituito come parte dell'URL nel frammento di #.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Sarà in grado di chiamare il back-end API per aggiungere elementi ToDo per l'utente connesso:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Passaggi successivi
[Sviluppare AD FS](../../ad-fs/AD-FS-Development.md)  
