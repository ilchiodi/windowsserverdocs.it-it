---
title: Creazione di un'applicazione client nativa con client OAuth pubblici con AD FS 2016 o versione successiva
description: Procedura dettagliata che fornisce istruzioni per la compilazione di un'applicazione client nativa con client OAuth pubblici e AD FS 2016 o versione successiva
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.openlocfilehash: 442aef6daccda2ab3e95690a82f43f642e5a3f73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358755"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>Creazione di un'applicazione client nativa con client OAuth pubblici con AD FS 2016 o versione successiva

## <a name="overview"></a>Panoramica

Questo articolo illustra come compilare un'applicazione nativa che interagisce con un'API Web protetta da AD FS 2016 o versione successiva.

1. L'applicazione WPF TodoListClient .NET usa il Active Directory Authentication Library (ADAL) per ottenere un token di accesso JWT da Azure Active Directory (Azure AD) tramite il protocollo OAuth 2,0
2. Il token di accesso viene usato come bearer token per autenticare l'utente quando si chiama l'endpoint/ToDoList dell'API Web TodoListService.
 Verrà usato l'esempio di applicazione per Azure AD qui e quindi modificato per AD FS 2016 o versione successiva.

![Overivew applicazione](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>Prerequisiti
Di seguito sono un elenco di prerequisiti che sono necessarie prima del completamento di questo documento. In questo documento si presuppone che ADFS è stato installato e che è stata creata una farm ADFS.

* Strumenti client di GitHub
* AD FS in Windows Server 2016 o versioni successive
* Visual Studio 2013 o versione successiva

## <a name="creating-the-sample-walkthrough"></a>Creazione della procedura dettagliata di esempio

### <a name="create-the-application-group-in-ad-fs"></a>Creare il gruppo di applicazioni in AD FS

1. In gestione AD FS fare clic con il pulsante destro del mouse su **gruppi di applicazioni** e selezionare **Aggiungi gruppo di applicazioni**.

2. Nella creazione guidata gruppo di applicazioni, per il nome immettere un nome che si preferisce, ad esempio NativeToDoListAppGroup. Selezionare l' **applicazione nativa che accede a un modello di API Web** . Fare clic su **Avanti**.
 ![Aggiungi gruppo di applicazioni](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. Nella pagina **native Application (applicazione nativa** ) prendere nota dell'identificatore generato da ad FS. Si tratta dell'ID con cui AD FS riconoscerà l'app client pubblica. Copia il **identificatore Client** valore. Verrà usato in un secondo momento come valore per **Ida: ClientID** nel codice dell'applicazione. Se si desidera, è possibile assegnare qualsiasi identificatore personalizzato qui. L'URI di reindirizzamento è qualsiasi valore arbitrario, ad esempio https://ToDoListClient put ![ Native App](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. Nella pagina **Configura API Web** impostare il valore dell'identificatore per l'API Web. Per questo esempio, deve essere il valore dell' **URL SSL** in cui si suppone che l'app Web sia in esecuzione. È possibile ottenere questo valore facendo clic sulle proprietà del progetto TooListServer nella soluzione. Verrà usato in un secondo momento come valore **todo: TodoListResourceId** nel file **app. config** dell'applicazione client nativa e anche come **todo: TodoListBaseAddress**.
![API Web](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. Esaminare i **criteri applica controllo di accesso** e **configurare le autorizzazioni dell'applicazione** con i valori predefiniti sul posto. La pagina di riepilogo dovrebbe avere un aspetto simile al seguente.
![Riepilogo](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

Fare clic su Avanti e quindi completare la procedura guidata.

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>Aggiungere l'attestazione NameIdentifier all'elenco di attestazioni rilasciate
L'applicazione demo usa il valore nell'attestazione NameIdentifier in varie posizioni. A differenza di Azure AD, AD FS non rilascia un'attestazione NameIdentifier per impostazione predefinita. Pertanto, è necessario aggiungere una regola attestazione per emettere l'attestazione NameIdentifier in modo che l'applicazione possa usare il valore corretto. In questo esempio, il nome specificato dell'utente viene emesso come valore NameIdentifier per l'utente nel token.
Per configurare la regola attestazioni, aprire il gruppo di applicazioni appena creato e fare doppio clic sull'API Web. Selezionare la scheda regole di trasformazione rilascio e quindi fare clic sul pulsante Aggiungi regola. Nella regola tipo di attestazione scegliere regola attestazione personalizzata e quindi aggiungere la regola attestazione, come illustrato di seguito.

```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![Regola attestazioni NameIdentifier](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>Modificare il codice dell'applicazione

In questa sezione viene illustrato come scaricare l'esempio di API Web e modificarla in Visual Studio.   Verrà usato l'esempio di Azure AD che è [qui](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop).  

Per scaricare il progetto di esempio, utilizzare Git Bash e digitare quanto segue:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>Modificare ToDoListClient

Questo progetto nella soluzione rappresenta l'applicazione client nativa. È necessario assicurarsi che l'applicazione client sia in grado di riconoscere:

1. Dove andare per fare in modo che l'utente venga autenticato quando necessario?
2. Qual è l'ID che il client deve fornire all'autorità di autenticazione (AD FS)?
3. Qual è l'ID della risorsa a cui viene richiesto il token di accesso?
4. Qual è l'indirizzo di base dell'API Web?

Le modifiche al codice seguenti sono necessarie per ottenere le informazioni sopra riportate all'applicazione client nativa.

**App. config**

* Aggiungere la chiave **Ida: Authority** con il valore che raffigura il servizio ad FS. Ad esempio, https://fs.contoso.com/adfs/
* Modificare la chiave **Ida: ClientID** con il valore dell' **identificatore client** nella pagina **dell'applicazione nativa** durante la creazione del gruppo di applicazioni in ad FS. Ad esempio, 3f07368b-6efd-4f50-A330-d93853f4c855
* Modificare **todo: todo: TodoListResourceId** con il valore dell' **identificatore** nella pagina **Configura API Web** durante la creazione del gruppo di applicazioni in ad FS. Ad esempio, https://localhost:44321/
* Modificare **todo: TodoListBaseAddress** con il valore dell' **identificatore** nella pagina **Configura API Web** durante la creazione del gruppo di applicazioni in ad FS. Ad esempio, https://localhost:44321/
* Impostare il valore di **Ida: RedirectUri** con il valore dall' **URI di reindirizzamento** nella pagina **dell'applicazione nativa** durante la creazione del gruppo di applicazioni in ad FS. Ad esempio, https://ToDoListClient
* Per semplificare la lettura è possibile rimuovere/aggiungere commenti alla chiave per **Ida: tenant** e **Ida: AADInstance**.

  ![Configurazione app](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* Commentare la riga per aadInstance come indicato di seguito

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* Aggiungere il valore per Authority come indicato di seguito

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* Elimina la riga per la creazione del valore dell' **autorità** da aadInstance e tenant

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* Nella funzione **MainWindow**modificare la creazione dell'istanza di authContext in

        authContext = new AuthenticationContext(authority,false);

    ADAL non supporta la convalida AD FS come autorità e pertanto è necessario passare un flag di valore false per il parametro validateAuthority.

  ![Finestra principale](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>Modificare TodoListService
È necessario modificare due file in questo progetto: Web. config e Startup.Auth.cs. Le modifiche di Web. config sono necessarie per ottenere i valori corretti dei parametri. Le modifiche Startup.Auth.cs sono necessarie per impostare il WebAPI per l'autenticazione con AD FS anziché Azure AD.

**Web.config**

* Commentare la chiave **Ida: tenant** perché non è necessaria
* Aggiungere la chiave per **Ida: Authority** con valore che indica il nome di dominio completo (FQDN) del servizio federativo, ad esempio https://fs.contoso.com/adfs/
* Modificare la chiave **Ida: audience** con il valore dell'identificatore dell'API Web specificato nella pagina **Configura API Web** durante l'aggiunta del gruppo di applicazioni in ad FS.
* Aggiungere la chiave **Ida: AdfsMetadataEndpoint** con valore corrispondente all'URL dei metadati della Federazione del servizio ad FS, ad esempio: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![Configurazione Web](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

Modificare la funzione ConfigureAuth come riportato di seguito

    public void ConfigureAuth(IAppBuilder app)
    {
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
    }

Fondamentalmente, viene configurata l'autenticazione per l'uso di AD FS e vengono fornite altre informazioni sui metadati AD FS e per convalidare il token, l'attestazione audience deve essere il valore previsto dall'API Web.
Esecuzione dell'applicazione

1. Nella soluzione NativeClient-DotNet, fare clic con il pulsante destro del mouse e passare a proprietà. Modificare il progetto di avvio come illustrato di seguito in più progetti di avvio e impostare TodoListClient e TodoListService su Start.
![Proprietà della soluzione](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  Premere il pulsante F5 o selezionare debug > continua nella barra dei menu. Verrà avviata sia l'applicazione nativa che la WebAPI. Fare clic sul pulsante Accedi nell'applicazione nativa per visualizzare un accesso interattivo da AD AL e reindirizzare al servizio AD FS. Immettere le credenziali di un utente valido.
![Accesso](media/native-client-with-ad-fs-2016/sign-in.png)

In questo passaggio, l'applicazione nativa è stata reindirizzata a AD FS e ha ottenuto un token ID e un token di accesso per l'API Web

3. Immettere un elemento da eseguire nella casella di testo e fare clic su Aggiungi elemento. In questo passaggio, l'applicazione raggiunge l'API Web per aggiungere l'elemento to do e, a tale scopo, presenta il token di accesso al WebAPI ottenuto da AD FS. L'API Web corrisponde al valore audience per verificare che il token sia destinato a esso e verifica la firma del token usando le informazioni dei metadati di Federazione.

![Accesso](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>Passaggi successivi
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
