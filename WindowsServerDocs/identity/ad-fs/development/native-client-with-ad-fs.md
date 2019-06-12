---
title: Compilare un client nativo dell'applicazione mediante client pubblici OAuth con AD FS 2016 o versioni successive
description: Una procedura dettagliata che fornisce le istruzioni di compilazione di un client nativo dell'applicazione usando OAuth client pubblici e AD FS 2016 o versioni successive
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 15bb6f1e39f64ff19ebb5515188ee944e277d3b7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445484"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>Compilare un client nativo dell'applicazione mediante client pubblici OAuth con AD FS 2016 o versioni successive

## <a name="overview"></a>Panoramica

Questo articolo illustra come compilare un'applicazione nativa che interagisce con un'API Web protetta da AD FS 2016 o versione successiva.

1. .Net applicazione TodoListClient WPF utilizza Active Directory Authentication Library (ADAL) per ottenere un token di accesso JWT da Azure Active Directory (Azure AD) tramite il protocollo OAuth 2.0
2. Il token di accesso viene utilizzato come un token di connessione per autenticare l'utente quando si chiama l'endpoint /todolist dell'API web TodoListService.
 Si verrà utilizzando l'esempio di applicazione per Azure AD di seguito e quindi modificarlo per AD FS 2016 o versione successiva.

![Panoramica dell'applicazione](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>Prerequisiti
Di seguito sono un elenco di prerequisiti che sono necessarie prima del completamento di questo documento. In questo documento si presuppone che ADFS è stato installato e che è stata creata una farm ADFS.

* Strumenti client di GitHub
* ADFS in Windows Server 2016 o versioni successive
* Visual Studio 2013 o versioni successive

## <a name="creating-the-sample-walkthrough"></a>Creare la procedura dettagliata di esempio

### <a name="create-the-application-group-in-ad-fs"></a>Creare il gruppo di applicazioni in AD FS

1. In Gestione AD FS, fare clic su **gruppi di applicazioni** e selezionare **Aggiungi gruppo di applicazioni**.

2. Creazione guidata gruppo di applicazioni, per il nome immettere qualsiasi nome desiderato, ad esempio NativeToDoListAppGroup. Selezionare il **applicazione nativa che accede a un'API web** modello. Fare clic su **Avanti**.
 ![Aggiungi gruppo di applicazioni](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. Nel **applicazione nativa** pagina, prendere nota dell'identificatore generato da AD FS. Questo è l'id con cui ADFS riconoscerà l'app client pubblico. Copia il **identificatore Client** valore. Verrà usato in un secondo momento come valore per **ida: ClientId** nel codice dell'applicazione. Se si desidera è possibile assegnare qualsiasi identificatore personalizzato qui. L'URI di reindirizzamento è qualsiasi valore arbitrario, esempio, inserire https://ToDoListClient ![app nativa](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. Nel **configurare Web API** pagina, impostare il valore dell'identificatore per l'API Web. Per questo esempio, deve essere il valore della **URL SSL** in cui l'App Web dovrebbe essere in esecuzione. È possibile ottenere questo valore facendo clic sulle proprietà del progetto TooListServer nella soluzione. Verrà usato in un secondo momento come le **todo:TodoListResourceId** valore nelle **app. config** file dell'applicazione client nativa e anche come il **todo:TodoListBaseAddress**.
![API Web](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. Seguire i passaggi di **si applicano criteri di controllo di accesso** e **configurare le autorizzazioni dell'applicazione** con i valori predefiniti in posizione. Nella pagina di riepilogo dovrebbe essere simile di sotto.
![Riepilogo](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

Fare clic su Avanti e quindi completare la procedura guidata.

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>Aggiungere l'attestazione NameIdentifier all'elenco delle attestazioni rilasciate
L'applicazione demo Usa il valore in un'attestazione NameIdentifier in varie posizioni. A differenza di Azure AD, AD FS non emette un'attestazione NameIdentifier per impostazione predefinita. Pertanto, è necessario aggiungere una regola attestazioni per rilasciare l'attestazione NameIdentifier in modo che l'applicazione può usare il valore corretto. In questo esempio, il nome assegnato all'utente viene inviato come valore NameIdentifier per l'utente nel token.
Per configurare la regola di attestazione, aprire il gruppo di applicazioni appena creato e fare doppio clic su API Web. Selezionare la scheda regole di trasformazione rilascio e quindi fare clic sul pulsante Aggiungi regola. Nel tipo di regola attestazione, scegliere la regola di attestazione personalizzato e quindi aggiungere la regola di attestazione, come illustrato di seguito.

```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![Regola di attestazione NameIdentifier](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>Modificare il codice dell'applicazione

In questa sezione viene illustrato come scaricare l'esempio di API Web e modificarla in Visual Studio.   Verrà usato l'esempio di Azure AD che è [qui](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop).  

Per scaricare il progetto di esempio, utilizzare Git Bash e digitare quanto segue:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>Modificare ToDoListClient

Questo progetto nella soluzione rappresenta l'applicazione client nativa. È necessario assicurarsi che l'applicazione client è a conoscenza:

1. Dove è possibile per ottenere l'utente autenticato se necessario?
2. Qual è l'ID client deve fornire per l'autorità di autenticazione (AD FS)?
3. Che cos'è l'ID della risorsa che viene richiesto il token di accesso per?
4. Che cos'è l'indirizzo di base dell'API Web?

Le seguenti modifiche al codice sono necessari per ottenere le informazioni riportate sopra per l'applicazione client nativa.

**App.config**

* Aggiungere la chiave **ida: autorità** con il valore che descrivono il servizio AD FS. Ad esempio, https://fs.contoso.com/adfs/
* Modificare **ida: ClientId** con il valore di chiave **identificatore Client** nel **applicazione nativa** pagina durante la creazione del gruppo di applicazioni in AD FS. Ad esempio, 3f07368b-6efd-4f50-a330-d93853f4c855
* Modificare il **todo:todo:TodoListResourceId** con il valore da **Identifier** nel **configurare Web API** pagina durante la creazione del gruppo di applicazioni in AD FS. Ad esempio, https://localhost:44321/
* Modificare il **todo:TodoListBaseAddress** con il valore da **Identifier** nel **configurare Web API** pagina durante la creazione del gruppo di applicazioni in AD FS. Ad esempio, https://localhost:44321/
* Impostare il valore della **URI di reindirizzamento: ida** con il valore dal **URI di reindirizzamento** nel **applicazione nativa** pagina durante la creazione del gruppo di applicazioni in AD FS. Ad esempio, https://ToDoListClient
* Per facilitare la lettura è possibile rimuovere / la chiave per commentare **ida: Tenant** e **ida: AADInstance**.

  ![Configurazione dell'App](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* Impostare come commento la riga per aadInstance come indicato di seguito

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* Aggiungere il valore dell'autorità come indicato di seguito

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* Eliminare la riga per la creazione di **autorità** dal tenant e aadInstance

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* Nella funzione **MainWindow**, modificare le istanze di authContext a

        authContext = new AuthenticationContext(authority,false);

    ADAL non supporta la convalida di AD FS come autorità e pertanto è necessario passare un flag con valore false per il parametro validateAuthority.

  ![Finestra principale](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>Modificare TodoListService
Due file necessitano di modifiche in questo progetto – Startup.Auth.cs e Web. config. Sono necessarie modifiche di Web. config per ottenere i valori dei parametri corretti. Sono necessarie modifiche Startup.Auth.cs per impostare l'API Web per l'autenticazione con AD FS piuttosto che Azure AD.

**Web.config**

* La chiave di commento **ida: Tenant** perché non è necessaria
* Aggiungere la chiave per **ida: autorità** con il valore che indica il nome di dominio completo della federazione servizio, ad esempio, https://fs.contoso.com/adfs/
* Modificare la chiave **ida: gruppo di destinatari** con il valore dell'identificatore specificato nell'API Web di **configurare Web API** pagina durante Aggiungi gruppo di applicazioni in AD FS.
* Aggiungi chiave **ida: AdfsMetadataEndpoint** con valore corrispondente all'URL dei metadati di federazione di AD FS servizio, ad esempio: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![Configurazione Web](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

Modificare la funzione ConfigureAuth come indicato di seguito

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

In pratica, viene configurato l'autenticazione per usare AD FS e fornire ulteriori informazioni sui metadati di AD FS e per convalidare il token, l'attestazione dei destinatari deve corrispondere al valore previsto dall'API Web.
Esecuzione dell'applicazione

1. Nella soluzione NativeClient-DotNet, fare clic e passare alle proprietà. Modificare il progetto di avvio come mostrato di seguito per i progetti di avvio più e impostare sia TodoListClient sia TodoListService per l'avvio.
![Proprietà della soluzione](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  Fare clic sul pulsante F5 o selezionare Debug > Continua nella barra dei menu. Verrà avviata l'applicazione nativa e l'API Web. Fare clic sul pulsante di accesso dell'applicazione nativa che e verrà visualizzata un accesso interattivo da Active Directory AL reindirizzamento al servizio AD FS. Immettere le credenziali di un utente valido.
![Sign-in](media/native-client-with-ad-fs-2016/sign-in.png)

In questo passaggio, l'applicazione nativa reindirizzato ad ADFS e ottenuto un token ID e un token di accesso per l'API Web

3. Immettere un elemento nella casella di testo e fare clic su Aggiungi elemento. In questo passaggio, l'applicazione raggiunge l'API Web per aggiungere l'attività e per eseguire questa operazione, presenta il token di accesso per l'API Web ottenuto da ADFS. L'API Web corrisponde al valore di gruppo di destinatari per assicurarsi che il token è destinato e verifica la firma del token usando le informazioni dai metadati di federazione.

![Accesso](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>Passaggi successivi
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
