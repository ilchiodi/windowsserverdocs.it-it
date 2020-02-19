---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: Creazione di un'applicazione a più livelli con l'uso di OAuth con AD FS 2016 o versione successiva
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 028396bffff6449a296e2922846fe2fc379fe624
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465615"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>Creazione di un'applicazione a più livelli con l'uso di OAuth con AD FS 2016 o versione successiva


In questa procedura dettagliata vengono fornite istruzioni per l'implementazione di un'autenticazione di (OBO) con AD FS in Windows Server 2016 TP5 o versione successiva. Per altre informazioni sull'autenticazione di OBO [, vedere ad FS i flussi OpenID Connect/OAuth e gli scenari di applicazione](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)

>AVVISO: L'esempio che è possibile compilare qui è solo a scopo didattico. Queste istruzioni sono per l'implementazione più semplice, più minima possibile esporre gli elementi necessari del modello. L'esempio potrebbe non includere tutti gli aspetti della gestione degli errori e altri riguardano funzionalità e si concentra SOLO nella Guida il completamento dell'autenticazione OBO.

## <a name="overview"></a>Panoramica

In questo esempio creeremo un flusso di autenticazione in cui un client accederà a un servizio Web di livello intermedio e il servizio web verrà quindi agire per conto del client autenticato per ottenere un token di accesso.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

Di seguito è riportato il flusso di autenticazione in modo da ottenere il codice di esempio
1. Il client esegue l'autenticazione per AD FS endpoint di autorizzazione e richiede un codice di autorizzazione
2. Endpoint di autorizzazione restituisce il codice di autenticazione client
3. Utilizza codice di autenticazione client e le presenta all'endpoint del token di ADFS per il token di accesso di richiesta per il servizio Web di livello intermedio come API Web
4. ADFS restituisce il token di accesso al servizio Web di livello intermedio. Per altre funzionalità, il servizio di livello intermedio deve accedere al back-end WebAPI
5. Client utilizza il token di accesso per utilizzare servizio di livello intermedio.
6. Servizio di livello intermedio fornisce il token di accesso all'endpoint token ADFS e le richieste di token di accesso per Backend WebAPI sul conto dell'utente autenticato
7. ADFS restituisce il token di accesso per back-end WebAPI al servizio di livello intermedio actiing come client
8. Servizio di livello intermedio utilizza il token di accesso fornito da ADFS nel passaggio 7 per accedere ai back-end di API Web come client ed eseguire le funzioni necessarie

## <a name="sample-structure"></a>Esempio di struttura

Esempio includerà tre moduli


Modulo | Descrizione
-------|------------
ToDoClient | Client nativo con cui interagisce l'utente
ToDoService | API che agisce come un client per l'API Web back-end web di livello intermedio
WebAPIOBO | Back-end web api utilizzata per eseguire l'operazione necessaria quando l'utente aggiunge un oggetto ToDoItem da ToDoService




## <a name="setting-up-the-development-box"></a>Impostare la casella di sviluppo

Questa procedura dettagliata viene utilizzato Visual Studio 2015. Il progetto utilizza ampiamente Active Directory Authentication Library (ADAL). Per informazioni su ADAL, leggere [libreria di classi .NET l'autenticazione di Active Directory](https://msdn.microsoft.com/library/azure/mt417579.aspx)

Nell'esempio viene inoltre utilizzato v 11.0 LocalDB di SQL. Installazione del database locale SQL prima di lavorare sull'esempio.

## <a name="setting-up-the-environment"></a>Impostazione dell’ambiente
Verrà utilizzato con una configurazione di base:

1. **Controller di dominio**: controller di dominio per il dominio in cui verrà ospitato ADFS
2. **Server ADFS**: il Server ADFS per il dominio
3. **Computer di sviluppo**: computer in cui è installato Visual Studio e sviluppo di esempio

È possibile, se si desidera, utilizzare solo due macchine. Uno per il controller di dominio/ADFS e l'altra per lo sviluppo dell'esempio.

Come configurare il controller di dominio e ADFS esula dall'ambito di questo articolo. Per informazioni aggiuntive sulla distribuzione, vedere:

- [Distribuzione di Active Directory Domain Services](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Distribuzione di AD FS](../AD-FS-Deployment.md)

L'esempio è basata sull'esempio OBO esistente in Azure creato di Vittorio Bertocci e disponibili [qui](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Seguire le istruzioni per clonare il progetto nel computer di sviluppo e creare una copia dell'esempio per iniziare a utilizzare.

## <a name="clone-or-download-this-repository"></a>Clonare o scaricare questo repository

Dalla shell o della riga di comando:

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modifica il codice di esempio

Non appena si apre la soluzione WebAPI-OnBehalfOf-DotNet. sln, si noterà che nella soluzione sono presenti due progetti

* **ToDoListClient**: questo fungerà da client che l'utente verrà interagisce con OpenID
* **ToDoListService**: questo verrà l'APP server Web di livello intermedio / Service che verrà interagisce con un altro back-end WebAPI OBO l'utente autenticato

Come può notare, è necessario aggiungere un altro progetto in un secondo momento verrà utilizzato come la risorsa che verrà utilizzata da ToDoListService di livello intermedio.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>Configurazione di ADFS per il Client e server Web App

Nel modulo corrente dell'esempio, l'autenticazione è configurato per essere eseguita in Azure AD. Si desidera modificare il meccanismo di autenticazione e diretto verso ADFS distribuito in locale. A tale scopo, è necessario configurare ADFS per riconoscere il client e server Web App abbiamo nell'esempio.

**Creazione di un gruppo di applicazioni**

Aprire la MMC di gestione di ADFS e aggiungere un nuovo gruppo di applicazioni. Selezionare il modello API Native-applicazione Web.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Fare clic su Avanti e verrà visualizzata la pagina per fornire informazioni sull'applicazione Client. Assegnare un nome appropriato per il client App in ADFS. Copiare l'identificatore client e salvarlo in un punto che è possibile accedere in seguito, quando ciò è richiesto nella configurazione dell'applicazione in visual studio.

>Nota: L'URI di reindirizzamento può essere qualsiasi URI arbitrario come non viene effettivamente utilizzato in caso di client nativi

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Fare clic su Avanti e verrà visualizzata la pagina per fornire informazioni sull'API Web. Assegnare un nome adatto per la voce di ADFS per l'API Web e immettere l'URI di reindirizzamento all'URI viene visualizzato in Visual Studio per ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Fare clic su Avanti per visualizzare la pagina Criteri di controllo di accesso scegliere. Garantire di vedere "Consenti tutti gli utenti" nella sezione criteri.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Fare clic su Avanti e verrà visualizzato con la pagina autorizzazioni per l'applicazione di configurazione. In questa pagina, selezionare gli ambiti consentiti come openid (selezionata per impostazione predefinita) e user_impersonation. L'ambito 'user_impersonation' è necessario essere correttamente in grado di richiedere un token di accesso per conto di ADFS.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

Fare clic su Avanti verrà visualizzata la pagina di riepilogo. Scorrere il resto della procedura guidata e completare la configurazione.

Per abilitare l'autenticazione per conto di, è necessario verificare che ADFS restituisce un token di accesso con ambito user_impersonation al client. Modificare il rilascio delle attestazioni per ToDoListServiceWebApi includere le seguenti tre regole personalizzate:

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "http://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Aggiunta di ToDoListService come client nel gruppo di applicazioni**

In questa fase è necessario apportare una voce aggiuntiva in ADFS per l'applicazione Web di agire come un client e non solo come una risorsa. Aprire il gruppo di applicazioni che appena creato e fare clic su Aggiungi applicazione.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

Verrà visualizzata la pagina "Aggiungi una nuova applicazione MySampleGroup". Nella pagina, selezionare "Applicazione o sito Web del Server" come applicazione autonoma

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Fare clic su Avanti e verrà visualizzata la pagina per specificare i dettagli dell'applicazione. Specificare un nome adatto per la voce di configurazione nella sezione relativa al nome. Assicurarsi che l'identificatore Client corrisponda come identificatore per il ToDoListServiceWebAPI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Fare clic su Avanti e verrà visualizzata la pagina per configurare le credenziali dell'applicazione. Fare clic su "Generazione di un segreto condiviso". Verrà visualizzato con una chiave privata generata automaticamente. Copiare la chiave privata in un percorso poiché sarà necessario quando si configura ToDoListService in visual studio.


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Fare clic su Next e completare la procedura guidata.

### <a name="modifying-the-todolistclient-code"></a>Modifica del codice ToDoListClient

#### <a name="modify-the-application-config"></a>Modificare la configurazione dell'applicazione

Passare all'utente è il progetto ToDoListClient WebAPI-OnBehalfOf-DotNet soluzione. Aprire il file app. config e apportare le modifiche seguenti

* Impostare come commento la voce della chiave ida: Tenant
* Per l'URI di reindirizzamento: ida, immettere l'URI arbitrario fornito durante la configurazione di MySampleGroup_ClientApplication in ADFS.
* Per la chiave di ida: ClientID, fornire al client identificatore ID assegnato durante la configurazione di MySampleGroup_ClientApplication ADFS.
* Di ida: ToDoListResourceID forniscono l'ID risorsa assegnato durante la configurazione di ToDoListServiceWebApi in ADFS
* Impostare come commento la chiave ida: AADInstance
* Immettere l'ID risorsa del ToDoListServiceWebApi ida: ToDoListBaseAddress. Verrà utilizzato durante la chiamata di ToDoList WebAPI.
* Aggiungere una chiave ida: autorità e fornire il valore dell'URI per ADFS.

Il **appSettings** in App. config dovrebbe essere simile al seguente:

    <appSettings>
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />-->
    <add key="ida:ClientId" value="c7f7b85c-497c-4589-877f-b17a0bd13398" />
    <add key="ida:RedirectUri" value="https://arbitraryuri.com/" />
    <add key="ida:TodoListResourceId" value="https://localhost:44321/" />
    <!--<add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
    <add key="ida:TodoListBaseAddress" value="https://localhost:44321" />
    <add key="ida:Authority" value="https://fs.anandmsft.com/adfs/"/>
    </appSettings>

#### <a name="modifying-the-code"></a>Modifica del codice

**MainWindow.xaml.cs**

Impostare come commento la riga contenente le informazioni sul tenant dalla configurazione dell'applicazione

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Modificare il valore dell'autorità di stringa per

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Modificare il codice per leggere i valori corretti di ToDoListResourceId e ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

Nella funzione MainWindow () modificare l'inizializzazione di authcontext come:

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Aggiunta della risorsa back-end

Per completare il flusso on-behalf-of, è necessario creare una risorsa back-end che accederà a ToDoListService sul conto dell'utente autenticato. La scelta della risorsa back-end può variare in base al requisito, ma ai fini di questo esempio è possibile creare un'API Web di base.

* Fare clic con il pulsante destro sulla soluzione 'WebAPI-OnBehalfOf-DotNet' in Esplora soluzioni e scegliere Aggiungi -> Nuovo progetto
* Scegliere il modello di applicazione Web ASP.NET

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* Al successivo prompt dei comandi fare clic su 'Modifica autenticazione'
* Selezionare 'Account aziendale o dell'istituto di istruzione' e la destra nell'elenco a discesa selezionare 'Locale'
* Immettere il percorso FederationMetadata per la distribuzione di ADFS e specificare un URI di App (fornire qualsiasi URI per il momento e la modifica viene applicata in un secondo momento) e fare clic su Ok per aggiungere il progetto alla soluzione.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Fare clic su controller in Esplora soluzioni il nuovo progetto creato. Selezionare Aggiungi -> Controller
* Nella selezione del modello, selezionare 'Web API 2 Controller - vuoto' e fare clic su Ok.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Assegnare al controller un nome appropriato.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* Aggiungere il codice seguente nel controller:

```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http;
    namespace WebAPIOBO.Controllers
    {
        [Authorize]
        public class WebAPIOBOController : ApiController
        {
            public IHttpActionResult Get()
            {
                return Ok($"WebAPI via OBO (user: {User.Identity.Name}");
            }
        }
    }
```

Questo codice verrà semplicemente restituire la stringa quando tutti gli utenti di inserire una richiesta Get per il WebAPI WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Aggiungere il nuovo back-end WebAPI per ADFS

Aprire il gruppo di applicazioni MySampleGroup. Fare clic su Aggiungi applicazione e selezionare il modello API Web e fare clic su Avanti.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

Nella pagina configurazione Web API fornire un nome appropriato per la voce di API Web e l'identificatore. L'identificatore deve essere il valore URL SSL dal progetto WebAPIOBO in visual studio (simile a quanto avveniva per BackendWebAPIAdfsAdd).

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Continuare con il resto della procedura guidata stessa come quando è stato configurato il ToDoListService WebAPI. Alla fine il gruppo di applicazioni dovrebbe essere simile di sotto:

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modifica del codice ToDoListService

#### <a name="modifying-the-application-config"></a>Modifica di configurazione dell'applicazione

* Aprire il file Web. config
* Modificare le seguenti chiavi

| Chiave                      | Valore                                                                                                                                                                                                                   |
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ida: pubblico             | ID del ToDoListService come specificato per AD FS durante la configurazione di ToDoListService WebAPI, ad esempio https://localhost:44321/                                                                                         |
| ida: ClientID             | ID del ToDoListService come specificato per AD FS durante la configurazione di ToDoListService WebAPI, ad esempio <https://localhost:44321/> </br>**È molto importante che Ida: audience e Ida: ClientID corrispondano tra loro** |
| ida: ClientSecret         | Questo è il segreto che ADFS generate momento di configurare il client ToDoListService in ADFS                                                                                                                   |
| Ida: AdfsMetadataEndpoint | Si tratta dell'URL dei metadati di AD FS, ad esempio https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml                                                                                             |
| ida: OBOWebAPIBase        | Si tratta dell'indirizzo di base che verrà usato per chiamare l'API back-end, ad esempio https://localhost:44300                                                                                                                     |
| ida: autorità            | Si tratta dell'URL per il servizio AD FS, ad esempio https://fs.anandmsft.com/adfs/                                                                                                                                          |

Le chiavi di tutti gli altri ida: XXXXXXX nel **appsettings** nodo può essere impostato come commento o eliminare

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Modificare l'autenticazione di Azure Active Directory a AD FS

* Aprire il file Startup.Auth.cs
* Rimuovere il codice seguente

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

con

        app.UseActiveDirectoryFederationServicesBearerAuthentication(
            new ActiveDirectoryFederationServicesBearerAuthenticationOptions
            {
                MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
                TokenValidationParameters = new TokenValidationParameters()
                {
                    SaveSigninToken = true,
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                }
            });

#### <a name="modifying-the-todolistcontroller"></a>Modifica di ToDoListController

Aggiungi riferimento a System.Web.Extensions. Modificare i membri della classe, sostituendo il codice riportato di seguito

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The App Key is a credential used by the application to authenticate to Azure AD.
    // The Tenant is the name of the Azure AD tenant in which this application is registered.
    // The AAD Instance is the instance of Azure, for example public Azure or Azure China.
    // The Authority is the sign-in URL of the tenant.
    //
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string appKey = ConfigurationManager.AppSettings["ida:AppKey"];

    //
    // To authenticate to the Graph API, the app needs to know the Grah API's App ID URI.
    // To contact the Me endpoint on the Graph API we need the URL as well.
    //
    private static string graphResourceId = ConfigurationManager.AppSettings["ida:GraphResourceId"];
    private static string graphUserUrl = ConfigurationManager.AppSettings["ida:GraphUserUrl"];
    private const string TenantIdClaimType = "https://schemas.microsoft.com/identity/claims/tenantid";

con

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**Modificare l'attestazione utilizzata per il nome**

Da ADFS rilascio l'attestazione Nmae ma non rilascio attestazione NameIdentifier. L'esempio Usa NameIdentifier alla chiave in modo univoco gli elementi ToDo. Per semplicità, è possibile rimuovere in modo sicuro con un nome di attestazione NameIdentifier nel codice. Trovare e sostituire tutte le occorrenze di NameIdentifier con il nome.

**Modificare la routine post e CallGraphAPIOnBehalfOfUser ()**

Copiare e incollare il codice seguente in ToDoListController.cs e sostituire il codice per registrare e CallGraphAPIOnBehalfOfUser

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
      if (!ClaimsPrincipal.Current.FindFirst("https://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
        {
            throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found" });
        }

      //
      // Call the WebAPIOBO On Behalf Of the user who called the To Do list web API.
      //

      string augmentedTitle = null;
      string custommessage = await CallGraphAPIOnBehalfOfUser();

      if (custommessage != null)
        {
            augmentedTitle = String.Format("{0}, Message: {1}", todo.Title, custommessage);
        }
        else
        {
            augmentedTitle = todo.Title;
        }

      if (null != todo && !string.IsNullOrWhiteSpace(todo.Title))
        {
            db.TodoItems.Add(new TodoItem { Title = augmentedTitle, Owner = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value });
            db.SaveChanges();
        }
      }

      public static async Task<string> CallGraphAPIOnBehalfOfUser()
      {
        string accessToken = null;
        AuthenticationResult result = null;
        AuthenticationContext authContext = null;
        HttpClient httpClient = new HttpClient();
        string custommessage = "";

        //
        // Use ADAL to get a token On Behalf Of the current user.  To do this we will need:
        // The Resource ID of the service we want to call.
        // The current user's access token, from the current request's authorization header.
        // The credentials of this application.
        // The username (UPN or email) of the user calling the API
        //

        ClientCredential clientCred = new ClientCredential(clientId, clientSecret);
        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        string userName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn) != null ? ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value : ClaimsPrincipal.Current.FindFirst(ClaimTypes.Email).Value;
        string userAccessToken = bootstrapContext.Token;
        UserAssertion userAssertion = new UserAssertion(bootstrapContext.Token, "urn:ietf:params:oauth:grant-type:jwt-bearer", userName);

        string userId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
        authContext = new AuthenticationContext(authority, false);

        // In the case of a transient error, retry once after 1 second, then abandon.
        // Retrying is optional.  It may be better, for your application, to return an error immediately to the user and have the user initiate the retry.
        bool retry = false;
        int retryCount = 0;

        do
          {
              retry = false;
              try
                {
                    result = await authContext.AcquireTokenAsync(OBOWebAPIBase, clientCred, userAssertion);
                    //result = await authContext.AcquireTokenAsync(...);
                    accessToken = result.AccessToken;
                }
              catch (AdalException ex)
                {
                    if (ex.ErrorCode == "temporarily_unavailable")
                    {
                        // Transient error, OK to retry.
                        retry = true;
                        retryCount++;
                        Thread.Sleep(1000);
                    }
                }
          } while ((retry == true) && (retryCount < 1));

        if (accessToken == null)
          {
              // An unexpected error occurred.
              return (null);
          }

        // Once the token has been returned by ADAL, add it to the http authorization header, before making the call to access the To Do list service.
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

        // Call the WebAPIOBO.
        HttpResponseMessage response = await httpClient.GetAsync(OBOWebAPIBase + "/api/WebAPIOBO");


        if (response.IsSuccessStatusCode)
          {
              // Read the response and databind to the GridView to display To Do items.
              string s = await response.Content.ReadAsStringAsync();
              JavaScriptSerializer serializer = new JavaScriptSerializer();
              custommessage = serializer.Deserialize<string>(s);
              return custommessage;
          }
        else
          {
              custommessage = "Unsuccessful OBO operation : " + response.ReasonPhrase;
          }
        // An unexpected error occurred calling the Graph API.  Return a null profile.
        return (null);
    }

## <a name="running-the-solution"></a>Esecuzione della soluzione


Per impostazione predefinita, visual studio è configurato per eseguire un progetto quando si fa clic su debug per l'esecuzione.

* Fare clic sulla soluzione e scegliere Proprietà.
* Nella pagina delle proprietà selezionare progetti di avvio più e modificare l'azione di avvio per tutte le tre voci.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Premere F5 ed eseguire la soluzione

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Fare clic sul pulsante Accedi. Verrà richiesto di accedere tramite ADFS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Dopo l'accesso, aggiungere un elemento ToDo nell'elenco. Dietro le quinte si configureranno di eseguire un'operazione Post a ToDoListService cui ulteriormente eseguirà un Post all'API web WebAPIOBO.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

Su operazione completata correttamente verrà visualizzato l'elemento è stato aggiunto all'elenco con il messaggio aggiuntivo dal back-end di API Web che ha effettuato l'accesso tramite flusso di autenticazione OBO.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

È inoltre possibile visualizzare le tracce dettagliate su Fiddler. Avviare Fiddler e abilitare la decrittografia HTTPS. Noterete che abbiamo effettuare due richieste all'endpoint /adfs/oautincludes.
Nella prima interazione si presenta il codice di accesso all'endpoint token e si ottiene un token di accesso per https://localhost:44321/ ![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

Nella seconda interazione con l'endpoint token, è possibile osservare che è stato impostato **requested_token_use** come **on_behalf_of** e si usa il token di accesso ottenuto per il servizio Web di livello intermedio, ad esempio https://localhost:44321/ come asserzione per ottenere il token per conto di.
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Passaggi successivi
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
