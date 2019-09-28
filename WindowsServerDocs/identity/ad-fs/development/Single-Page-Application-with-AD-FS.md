---
title: Creare un'applicazione Web a pagina singola con OAuth e ADAL. JS con AD FS 2016 o versione successiva
description: Procedura dettagliata che fornisce istruzioni per l'autenticazione rispetto a AD FS usando ADAL per JavaScript che protegge un'applicazione a pagina singola basata su AngularJS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/13/2018
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.openlocfilehash: d54c33e092204f208590bd15db0d3c7fe7f852f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407892"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>Creare un'applicazione Web a pagina singola con OAuth e ADAL. JS con AD FS 2016 o versione successiva

Questa procedura dettagliata fornisce istruzioni per l'autenticazione rispetto a AD FS usando ADAL per JavaScript che protegge un'applicazione a singola pagina basata su AngularJS, implementata con una API Web ASP.NET back-end.

In questo scenario, quando l'utente accede, il front-end JavaScript usa [Active Directory Authentication Library per JavaScript (adal. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) e la concessione di autorizzazione implicita per ottenere un token ID (token ID) da Azure ad. Il token viene memorizzato nella cache e il client lo connette alla richiesta come bearer token quando si effettuano chiamate al relativo back-end dell'API Web, protetto tramite il middleware OWIN.

>[!IMPORTANT]
>L'esempio che è possibile compilare qui è esclusivamente a scopo didattico. Queste istruzioni sono per l'implementazione più semplice, più minima possibile esporre gli elementi necessari del modello. Nell'esempio potrebbero non essere inclusi tutti gli aspetti della gestione degli errori e altre funzionalità correlate.

>[!NOTE]
>Questa procedura dettagliata è applicabile **solo** a AD FS server 2016 e versioni successive 

## <a name="overview"></a>Panoramica
In questo esempio verrà creato un flusso di autenticazione in cui verrà eseguita l'autenticazione di un client di applicazione a pagina singola con AD FS per proteggere l'accesso alle risorse WebAPI nel back-end. Di seguito è riportato il flusso di autenticazione generale


![Autorizzazione AD FS](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Quando si usa un'applicazione a pagina singola, l'utente passa a una posizione iniziale, da dove viene caricata la pagina iniziale e una raccolta di file JavaScript e visualizzazioni HTML. È necessario configurare il Active Directory Authentication Library (ADAL) per conoscere le informazioni critiche sull'applicazione, ad esempio l'istanza AD FS, l'ID client, in modo da poter indirizzare l'autenticazione al AD FS.

Se ADAL rileva un trigger per l'autenticazione, USA le informazioni fornite dall'applicazione e indirizza l'autenticazione al servizio token di AD FS.  L'applicazione a pagina singola, registrata come client pubblico in AD FS, viene configurata automaticamente per il flusso di concessione implicita. La richiesta di autorizzazione restituisce un token ID restituito all'applicazione tramite un #fragment. Altre chiamate al WebAPI back-end porteranno questo token ID come bearer token nell'intestazione per ottenere l'accesso a WebAPI.

## <a name="setting-up-the-development-box"></a>Impostare la casella di sviluppo
Questa procedura dettagliata viene utilizzato Visual Studio 2015. Il progetto usa la libreria ADAL JS. Per informazioni su ADAL, vedere [Active Directory Authentication Library .NET.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Impostazione dell'ambiente
Per questa procedura dettagliata verrà usata una configurazione di base di:

1.  DC Controller di dominio per il dominio in cui verrà ospitato AD FS
2.  Server AD FS: Server AD FS per il dominio
3.  Computer di sviluppo: Il computer in cui è installato Visual Studio e verrà sviluppato l'esempio

È possibile, se si desidera, utilizzare solo due macchine. Uno per DC/AD FS e l'altro per lo sviluppo dell'esempio.

Come configurare il controller di dominio e ADFS esula dall'ambito di questo articolo. Per informazioni aggiuntive sulla distribuzione, vedere:

- [Distribuzione di Active Directory Domain Services](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Distribuzione di AD FS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clonare o scaricare questo repository
Si utilizzerà l'applicazione di esempio creata per l'integrazione di Azure AD in un'app a pagina singola AngularJS e la si modifica in modo da proteggere la risorsa back-end usando AD FS.

Dalla shell o della riga di comando:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Informazioni sul codice
I file di chiave che contengono la logica di autenticazione sono i seguenti:

**App. js** : inserisce la dipendenza del modulo Adal, fornisce i valori di configurazione dell'app usati da Adal per la guida delle interazioni del protocollo con AAD e indica le route a cui non si accede senza l'autenticazione precedente.

**index. html** : contiene un riferimento a adal. js

**HomeController. js**: Mostra come sfruttare i metodi di accesso () e di disconnessione () in adal.

**UserDataController. js** : Mostra come estrarre le informazioni utente dal token ID memorizzato nella cache.

**Startup.auth.cs** : contiene la configurazione per il WebAPI da usare Active Directory servizio federativo per l'autenticazione di Bearer.

## <a name="registering-the-public-client-in-ad-fs"></a>Registrazione del client pubblico in AD FS
Nell'esempio, WebAPI è configurato per l'ascolto a https://localhost:44326/. Il gruppo di applicazioni **Web browser l'accesso a un'applicazione Web** può essere usato per la configurazione dell'applicazione del flusso di concessione implicita.

1. Aprire la console di gestione di AD FS e fare clic su **Aggiungi gruppo di applicazioni**. Nella **procedura guidata Aggiungi gruppo di applicazioni** immettere il nome dell'applicazione, la descrizione e selezionare il **Web browser l'accesso a un modello di applicazione Web** dalla sezione **applicazioni client-server** , come illustrato di seguito.

    ![Crea nuovo gruppo di applicazioni](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. Nell' **applicazione nativa**della pagina successiva specificare l'identificatore del client dell'applicazione e l'URI di reindirizzamento come illustrato di seguito.

    ![Crea nuovo gruppo di applicazioni](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. Nella pagina successiva **applicare i criteri di controllo di accesso** lasciare le autorizzazioni come *Consenti Everyone*

4. La pagina di riepilogo dovrebbe essere simile alla seguente

    ![Crea nuovo gruppo di applicazioni](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Fare clic su Avanti per completare l'aggiunta del **gruppo di applicazioni** e chiudere la procedura guidata.

## <a name="modifying-the-sample"></a>Modifica il codice di esempio
Configurare ADAL JS

Aprire il file **app. js** e modificare la definizione di **adalProvider. init** in:

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
|istanza|URL STS, ad esempio https://fs.contoso.com/|
|tenant|Mantenerlo come ' ADFS '|
|ClientID|Si tratta dell'ID client specificato durante la configurazione del client pubblico per l'applicazione a pagina singola|

## <a name="configure-webapi-to-use-ad-fs"></a>Configurare WebAPI per l'uso di AD FS
Aprire il file **Startup.auth.cs** nell'esempio e aggiungere quanto segue all'inizio:

    using System.IdentityModel.Tokens;

rimuovere

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
|ValidAudience|Viene configurato il valore di ' audience ' che verrà sottoposto a verifica nel token|
|ValidIssuer|Viene configurato il valore dell'autorità emittente che verrà controllata nel token|
|MetadataEndpoint|Che punta alle informazioni sui metadati del servizio token di servizio|

## <a name="add-application-configuration-for-ad-fs"></a>Aggiungere la configurazione dell'applicazione per AD FS
Modificare il appSettings come riportato di seguito:

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>Esecuzione della soluzione
Pulire la soluzione, ricompilare la soluzione ed eseguirla. Se si desidera visualizzare tracce dettagliate, avviare Fiddler e abilitare la decrittografia HTTPS.

Il browser (usare il browser Chrome) caricherà la SPA e verrà visualizzata la schermata seguente:

![Registrare il client](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Fare clic su login.  L'elenco attività attiverà il flusso di autenticazione e ADAL JS indirizza l'autenticazione a AD FS

![Accesso](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

In Fiddler è possibile visualizzare il token restituito come parte dell'URL nel frammento #.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Sarà ora possibile chiamare l'API back-end per aggiungere elementi elenco ToDo per l'utente connesso:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Passaggi successivi
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
