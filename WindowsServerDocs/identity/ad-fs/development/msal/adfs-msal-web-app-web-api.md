---
title: AD FS app Web MSAL (app Server) che chiama le API Web
description: Informazioni su come creare un utente che esegue l'accesso all'app Web autenticato da AD FS 2019.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f28e5feccb7544046104658585ab3f739f659957
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949500"
---
# <a name="scenario-web-app-server-app-calling-web-api"></a>Scenario: app Web (app Server) che chiama l'API Web 
>Si applica a: AD FS 2019 e versioni successive 
 
Informazioni su come creare un'app Web per l'accesso degli utenti autenticati da AD FS 2019 e l'acquisizione di token tramite la [libreria MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) per chiamare le API Web.  
 
Prima di leggere questo articolo, è necessario avere familiarità con i [concetti ad FS](../ad-fs-openid-connect-oauth-concepts.md) e il [flusso di concessione del codice di autorizzazione](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>Panoramica 
 
![Panoramica dell'API Web per la chiamata di app Web](media/adfs-msal-web-app-web-api/webapp1.png)

In questo flusso si aggiunge l'autenticazione all'app Web (app Server), che può quindi accedere agli utenti e chiama un'API Web. Dall'app Web, per chiamare l'API Web, usare il metodo di acquisizione dei token [AcquireTokenByAuthorizationCode](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenbyauthorizationcodeparameterbuilder?view=azure-dotnet) di MSAL. Si userà il flusso di codice di autorizzazione, archiviando il token acquisito nella cache dei token. Il controller acquisirà quindi i token automaticamente dalla cache all'occorrenza. Se necessario, MSAL aggiorna il token. 

App Web che chiamano API Web: 


- sono applicazioni client riservate. 
- per questo motivo hanno registrato un segreto (segreto condiviso dell'applicazione, certificato o account AD) con AD FS. Questo segreto viene passato durante la chiamata a AD FS per ottenere un token.  

Per comprendere meglio come registrare un'app Web in ADFS e configurarla per l'acquisizione di token per chiamare un'API Web, è possibile usare un esempio disponibile [qui](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi) e una procedura dettagliata per la procedura di registrazione e di configurazione del codice dell'app.  

 
## <a name="pre-requisites"></a>Prerequisiti 

- Strumenti client di GitHub 
- AD FS 2019 o versione successiva configurata e in esecuzione 
- Visual Studio 2013 o versioni successive 
 
## <a name="app-registration-in-ad-fs"></a>Registrazione dell'app in AD FS 
Questa sezione illustra come registrare l'app Web come un client e un'API Web riservati come relying party (RP) in AD FS. 

  1. In gestione AD FS fare clic con il pulsante destro del mouse su **gruppi di applicazioni** e selezionare **Aggiungi gruppo di applicazioni**.  
  2. Nella creazione guidata gruppo di applicazioni, per il **nome** immettere **WebAppToWebApi** e in **applicazioni client-server** selezionare l' **applicazione server che accede a un modello di API Web** . Fai clic su **Next**.  
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp2.png)
  
  3. Copia il **identificatore Client** valore. Verrà usato in un secondo momento come valore per **Ida: ClientID** nel file **Web. config** delle applicazioni. Immettere quanto segue per l' **URI di reindirizzamento:**  - https://localhost:44326. Fai clic su Aggiungi. Fai clic su **Next**. 
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp3.png)
  
  4. Nella schermata Configura credenziali applicazione inserire un segno di spunta in **genera un segreto condiviso** e copiare il segreto. Che verrà usato in un secondo momento come valore per **Ida: ClientSecret** nel file **Web. config** delle applicazioni. Fai clic su **Next**.  
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp4.png)
  
  5. Nella schermata Configura API Web immettere l' **identificatore:** https://webapi. Fai clic su **Aggiungi**. Fai clic su **Next**. Questo valore verrà usato in un secondo momento per **Ida: GraphResourceId** nel file **Web. config** delle applicazioni. 
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp5.png)
  
  6. Nella schermata applica criteri di controllo di accesso selezionare **Consenti** tutti gli utenti e fare clic su **Avanti**. 
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp6.png)
  
  7. Nella schermata Configura autorizzazioni per le applicazioni verificare che **OpenID** e **user_impersonation** siano selezionati e fare clic su **Avanti**. 
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp7.png)
  
  8. Nella schermata Riepilogo fare clic su **Avanti**. 
  
  9. Nella schermata completa fare clic su **Chiudi**.



## <a name="code-configuration"></a>Configurazione del codice 

Questa sezione illustra come configurare un'app Web ASP.NET per l'accesso all'utente e recuperare il token per chiamare l'API Web 

  1. Scaricare l'esempio da [qui](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)   
  
  2. Aprire l'esempio con Visual Studio 
  
  3. Aprire il file Web. config. Modificare quanto segue: 
       - Ida: ClientID: immettere il valore dell' **identificatore client** da #3 nella registrazione dell'app nella sezione ad FS precedente. 
       - Ida: ClientSecret-immettere il valore del **segreto** da #4 nella sezione precedente relativa alla registrazione dell'app ad FS. 
       - Ida: RedirectUri-immettere il valore **URI di reindirizzamento** da #3 nella sezione relativa alla registrazione dell'App nel ad FS. 
       - Ida: Authority-immettere https:///[nome host AD FS]/ADFS. Ad esempio, https://adfs.contoso.com/adfs 
       - Ida: Resource: immettere il valore dell' **identificatore** dall'#5 nella registrazione dell'app nella sezione ad FS precedente. 
      
          ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp8.png)
 
 
### <a name="test-the-sample"></a>Testare l'esempio 
In questa sezione viene illustrato come eseguire il test dell'esempio configurato in precedenza. 

  1. Una volta apportate modifiche al codice, ricompilare la soluzione 
  
  2. Nella parte superiore di Visual Studio, verificare che Internet Explorer sia selezionata e fare clic sulla freccia verde. 
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp9.png)

  3. Nella Home page fare clic su Accedi. 
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp10.png)

  4. Si sarà reindirizzati alla pagina di accesso di ADFS. Andare avanti ed eseguire l'accesso. 
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp11.png)

  5. Dopo aver eseguito l'accesso, fare clic su token di accesso.  
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp12.png)

  6. Facendo clic su token di accesso si otterranno le informazioni sul token di accesso chiamando l'API Web 
  
      ![Aggiungi gruppo di applicazioni](media/adfs-msal-web-app-web-api/webapp13.png)
 
 ## <a name="next-steps"></a>Passaggi successivi
[Flussi e scenari applicativi di OpenID Connect/OAuth in AD FS](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 