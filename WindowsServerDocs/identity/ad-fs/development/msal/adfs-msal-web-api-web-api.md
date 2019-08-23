---
title: AD FS API Web MSAL chiamata API Web (per conto dello scenario)
description: Informazioni su come creare un'API Web che chiama un'altra API Web.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 08892fe771928fa4b68ce50bfef2b6a041c2d210
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2019
ms.locfileid: "69983539"
---
# <a name="scenario-web-api-calling-web-api-on-behalf-of-scenario"></a>Scenario: API Web che chiama l'API Web (per conto dello scenario) 
> Si applica a: AD FS 2019 e versioni successive 
 
Informazioni su come creare un'API Web che chiama un'altra API Web per conto dell'utente.  
 
Prima di leggere questo articolo, è necessario avere familiarità con i [concetti ad FS](../ad-fs-openid-connect-oauth-concepts.md) e il [flusso Behalf_Of](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#on-behalf-of-flow)

## <a name="overview"></a>Panoramica 


- Un client (app Web), non rappresentato nel diagramma seguente, chiama un'API Web protetta e fornisce un bearer token JWT nell'intestazione http "Authorization". 
- L'API Web protetta convalida il token e usa il metodo [AcquireTokenOnBehalfOf](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync?view=azure-dotnet#Microsoft_IdentityModel_Clients_ActiveDirectory_AuthenticationContext_AcquireTokenAsync_System_String_Microsoft_IdentityModel_Clients_ActiveDirectory_ClientCredential_Microsoft_IdentityModel_Clients_ActiveDirectory_UserAssertion_) di MSAL per richiedere (da ad FS) un altro token, in modo che possa chiamare una seconda API Web (denominata API Web downstream) per conto dell'utente. 
- L'API Web protetta usa questo token per chiamare un'API downstream. Può anche chiamare AcquireTokenSilentlater per richiedere token per altre API downstream (ma ancora per conto dello stesso utente). Quando necessario, AcquireTokenSilent aggiorna il token.  
 
     ![informazioni generali](media/adfs-msal-web-api-web-api/webapi1.png)
 
Per comprendere meglio come configurare per conto dello scenario di autenticazione in ADFS, è possibile usare un esempio disponibile [qui](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof) e una procedura dettagliata per la procedura di registrazione e di configurazione del codice dell'app.  
 
## <a name="pre-requisites"></a>Prerequisiti 

- Strumenti client di GitHub 
- AD FS 2019 o versione successiva configurata e in esecuzione 
- Visual Studio 2013 o versione successiva 
 
## <a name="app-registration-in-ad-fs"></a>Registrazione dell'app in AD FS 

Questa sezione illustra come registrare l'app nativa come client pubblico e API Web come relying party (RP) in AD FS 

  1. In gestione AD FS fare clic con il pulsante destro del mouse su **gruppi di applicazioni** e selezionare **Aggiungi gruppo di applicazioni**.  
  
  2. Nella creazione guidata gruppo di applicazioni, per il **nome** immettere **WebApiToWebApi** e in **applicazioni client-server** selezionare l' **applicazione nativa che accede a un modello di API Web** . Fare clic su **Avanti**.

      ![Registrazione dell'app](media/adfs-msal-web-api-web-api/webapi2.png)

  3. Copia il **identificatore Client** valore. Verrà usato in un secondo momento come valore per **ClientID** nel file **app. config** dell'applicazione. Immettere quanto segue per l' **URI di reindirizzamento:**  - https://ToDoListClient. Fare clic su **Aggiungi**. Fare clic su **Avanti**. 
  
      ![Registrazione dell'app](media/adfs-msal-web-api-web-api/webapi3.png)
  
  4. Nella schermata Configura API Web immettere l' **identificatore:** https://localhost:44321/. Fare clic su **Aggiungi**. Fare clic su **Avanti**. Questo valore verrà usato in un secondo momento nel file **app. config** e **Web.** config dell'applicazione.  
 
      ![Registrazione dell'app](media/adfs-msal-web-api-web-api/webapi4.png)

  5. Nella schermata applica criteri di controllo di accesso selezionare **Consenti tutti gli utenti** e fare clic su **Avanti**. 
  
      ![Registrazione dell'app](media/adfs-msal-web-api-web-api/webapi5.png)  

  6. Nella schermata Configura autorizzazioni applicazione selezionare **OpenID** e **user_impersonation**. Fare clic su **Avanti**.  
  
      ![Registrazione dell'app](media/adfs-msal-web-api-web-api/webapi6.png)  

  7. Nella schermata Riepilogo fare clic su **Avanti**. 

  8. Nella schermata completa fare clic su **Chiudi**. 


  9. In gestione AD FS fare clic su **gruppi di applicazioni** e selezionare gruppo di applicazioni **WebApiToWebApi** . Fare clic con il pulsante destro del mouse e selezionare **Proprietà**. 
  
      ![Registrazione dell'app](media/adfs-msal-web-api-web-api/webapi7.png)  

  10. Nella schermata delle proprietà di WebApiToWebApi fare clic su **Aggiungi applicazione.** 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi8.png)

  11. In applicazioni autonome selezionare **applicazione server**.  
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi9.png)

  12. Nella schermata applicazione server aggiungere https://localhost:44321/ come **identificatore client** e URI di **Reindirizzamento**. 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi10.png)

  13. Nella schermata Configura credenziali applicazione selezionare **genera un segreto condiviso**. Copiare il segreto per un uso successivo.
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi11.png)

  14. Nella schermata Riepilogo fare clic su **Avanti**. 

  15. Nella schermata completa fare clic su **Chiudi**. 

  16. In gestione AD FS fare clic su **gruppi di applicazioni** e selezionare gruppo di applicazioni **WebApiToWebApi** . Fare clic con il pulsante destro del mouse e selezionare **Proprietà**. 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi12.png)

  17. Nella schermata delle proprietà di WebApiToWebApi fare clic su **Aggiungi applicazione.** 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi13.png)

  18. In applicazioni autonome selezionare **API Web**. 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi14.png)  

  19. In Configura API Web aggiungere https://localhost:44300 come **identificatore**.  
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi15.png)

  20. Nella schermata applica criteri di controllo di accesso selezionare **Consenti tutti gli utenti** e fare clic su **Avanti**. 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi16.png)

  21. Nella schermata Configura autorizzazioni per le applicazioni fare clic su **Avanti**. 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi17.png)

  22. Nella schermata Riepilogo fare clic su **Avanti**.

  23. Nella schermata completa fare clic su **Chiudi**.  

  24. Fare clic su OK in WebApiToWebApi-schermata delle proprietà dell'API Web 2  

  25. Nella schermata delle proprietà di WebApiToWebApi selezionare **WebApiToWebApi-API Web** e fare clic su **modifica.**  
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi18.png)

  26. Nella schermata WebApiToWebApi-proprietà API Web selezionare la scheda **regole di trasformazione rilascio** e fare clic su **Aggiungi regola.** . 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi19.png)

  27. Nella procedura guidata Aggiungi regola attestazione di trasformazione selezionare **Invia attestazioni usando una regola personalizzata** dall'elenco a discesa e fare clic su **Avanti**. 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi20.png)

  28. Immettere **PassAllClaims** in **Nome regola attestazione:** campo e **x: [] = > problema (Claim = x);** regola attestazione nel campo regola personalizzata: e fare clic su fine.  
   
      ![Reg app](media/adfs-msal-web-api-web-api/webapi21.png)

  29. Fare clic su OK in WebApiToWebApi-schermata delle proprietà dell'API Web

  30. Nella schermata delle proprietà di WebApiToWebApi selezionare Seleziona WebApiToWebApi-API Web 2 e fare clic su modifica.</br> 
  ![Reg app](media/adfs-msal-web-api-web-api/webapi22.png)

  31. Nella schermata delle proprietà di WebApiToWebApi-Web API 2 Selezionare la scheda regole di trasformazione rilascio e fare clic su Aggiungi regola... 

  32. In aggiunta guidata regola attestazione di trasformazione selezionare Invia attestazioni usando una regola personalizzata da dopdown e ![fare clic su Next app reg](media/adfs-msal-web-api-web-api/webapi23.png)

  33. Immettere PassAllClaims in nome regola attestazione: campo e **x: [] = > problema (Claim = x);** regola attestazione nel campo **regola personalizzata:** e fare clic su **fine**.  
   
      ![Reg app](media/adfs-msal-web-api-web-api/webapi24.png)

  34.  Fare clic su OK nella schermata delle proprietà dell'API Web 2 di WebApiToWebApi, quindi nella schermata delle proprietà di WebApiToWebApi.  
 

## <a name="code-configuration"></a>Configurazione del codice 

Questa sezione illustra come configurare un'API Web per chiamare un'altra API Web 

  1. Scaricare l'esempio da [qui](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof)  
  
  2. Aprire l'esempio con Visual Studio 
  
  3. Aprire il file app. config. Modificare quanto segue: 
       - Ida: Authority-immettere https:///[nome host AD FS]/ADFS/
       - Ida: ClientID: immettere il valore da #3 nella registrazione dell'app nella sezione AD FS precedente. 
       - Ida: RedirectUri-immettere il valore da #3 in registrazione app nella sezione AD FS precedente. 
       - TODO: TodoListResourceId: immettere il valore dell'identificatore dalla #4 nella registrazione dell'app nella sezione AD FS precedente 
       - Ida: todo: TodoListBaseAddress-immettere il valore dell'identificatore da #4 nella sezione relativa alla registrazione dell'app nel AD FS precedente. 
      
            ![Reg app](media/adfs-msal-web-api-web-api/webapi25.png)

  4. Aprire il file Web. config in ToDoListService. Modificare quanto segue: 
       - Ida: audience-immettere il valore dell'identificatore client da #12 nella sezione relativa alla registrazione dell'app AD FS precedente
       - Ida: ClientID: immettere il valore dell'identificatore client da #12 nella registrazione dell'app nella sezione AD FS precedente. 
       - Ida ClientSecret-immettere il segreto condiviso copiato da #13 nella registrazione dell'app in AD FS sezione precedente.
       - Ida: RedirectUri-immettere il valore RedirectUri da #12 nella sezione precedente relativa alla AD FS registrazione dell'app. 
       - Ida AdfsMetadataEndpoint-immettere https://[your AD FS hostname]/FederationMetadata/2007-06/FederationMetadata.XML 
       - Ida: OBOWebAPIBase-immettere il valore dell'identificatore da #19 nella sezione relativa alla registrazione dell'app AD FS precedente. 
       - Ida: Authority-immettere https:///[nome host AD FS]/ADFS 
  
          ![Reg app](media/adfs-msal-web-api-web-api/webapi26.png) 

 5. Aprire il file Web. config in WebAPIOBO. Modificare quanto segue: 
       - Ida AdfsMetadataEndpoint-immettere https://[your AD FS hostname]/FederationMetadata/2007-06/FederationMetadata.XML 
       - Ida: audience-immettere il valore dell'identificatore client da #12 nella sezione relativa alla registrazione dell'app AD FS precedente 
 
          ![Reg app](media/adfs-msal-web-api-web-api/webapi27.png)
 
## <a name="test-the-sample"></a>Testare l'esempio 

In questa sezione viene illustrato come eseguire il test dell'esempio configurato in precedenza. 

Una volta apportate modifiche al codice, ricompilare la soluzione 
 
  1. In Visual Studio fare clic con il pulsante destro del mouse su soluzione e selezionare **Imposta progetti di avvio...** 
      
      ![Reg app](media/adfs-msal-web-api-web-api/webapi28.png)

  2. Nelle pagine delle proprietà assicurarsi che **azione** sia impostato su **Avvia** per ogni progetto, ad eccezione di TodoListSPA.  
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi29.png)
  
  3. Nella parte superiore di Visual Studio fare clic sulla freccia verde.  
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi30.png)

  4. Nella schermata principale dell'app nativa fare clic su **Accedi**. 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi31.png)

     Se non viene visualizzata la schermata app nativa, cercare e rimuovere i file * msalcache. bin dalla cartella in cui è stato salvato il repository del progetto nel sistema. 
  
  5. Si sarà reindirizzati alla pagina di accesso di ADFS. Andare avanti ed eseguire l'accesso. 
  
      ![Reg app](media/adfs-msal-web-api-web-api/webapi32.png)

  6. Dopo aver eseguito l'accesso, immettere Text Web API to Web API call nell' **elemento create a to do Item**. Fare clic su **Aggiungi elemento**.  Questa operazione chiamerà l'API Web (to do List Service) che quindi chiama l'API Web 2 (WebAPIOBO) e aggiunge l'elemento nella cache.  
 
      ![Reg app](media/adfs-msal-web-api-web-api/webapi33.png)
 
 ## <a name="next-steps"></a>Passaggi successivi
[AD FS i flussi OpenID Connect/OAuth e gli scenari di applicazione](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 
 
