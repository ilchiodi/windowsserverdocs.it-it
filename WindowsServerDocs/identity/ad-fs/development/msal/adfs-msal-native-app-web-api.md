---
title: AD FS app nativa MSAL che chiama l'API Web
description: viene illustrato come creare un'app nativa che esegue l'accesso agli utenti autenticati da AD FS 2019 e acquisisce i token usando la libreria MSAL per chiamare le API Web.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8b27097ac64f981343c1d455c826fa1b9004133e
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949588"
---
# <a name="scenario-native-app-calling-web-api"></a>Scenario: app nativa che chiama l'API Web 
>Si applica a: AD FS 2019 e versioni successive 
 
Informazioni su come creare un'app nativa che esegue l'accesso agli utenti autenticati da AD FS 2019 e acquisisce i token usando la [libreria MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) per chiamare le API Web.  
 
Prima di leggere questo articolo, è necessario avere familiarità con i [concetti ad FS](../ad-fs-openid-connect-oauth-concepts.md) e il [flusso di concessione del codice di autorizzazione](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>Panoramica 
 
 ![Panoramica](media/adfs-msal-native-app-web-api/native1.png)

In questo flusso si aggiunge l'autenticazione all'app nativa (client pubblico), che può quindi accedere agli utenti e chiama un'API Web. Per chiamare un'API Web da un'app nativa che esegue l'accesso agli utenti, è possibile usare il metodo di acquisizione dei token [AcquireTokenInteractive](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.ipublicclientapplication.acquiretokeninteractive?view=azure-dotnet#Microsoft_Identity_Client_IPublicClientApplication_AcquireTokenInteractive_System_Collections_Generic_IEnumerable_System_String__) di MSAL. Per abilitare questa interazione, MSAL sfrutta un Web browser. 

 
Per comprendere meglio come configurare un'app nativa in ADFS per acquisire il token di accesso in modo interattivo, usare un esempio disponibile [qui](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi) e una procedura dettagliata per la procedura di registrazione e configurazione del codice dell'app.  
 

## <a name="pre-requisites"></a>Prerequisiti 


- Strumenti client di GitHub 
- AD FS 2019 o versione successiva configurata e in esecuzione 
- Visual Studio 2013 o versioni successive 
 

## <a name="app-registration-in-ad-fs"></a>Registrazione dell'app in AD FS 
Questa sezione illustra come registrare l'app nativa come client pubblico e API Web come relying party (RP) in AD FS 

  1. In **gestione ad FS**fare clic con il pulsante destro del mouse su **gruppi di applicazioni** e selezionare **Aggiungi gruppo di applicazioni**.   
  
  2. Nella creazione guidata gruppo di applicazioni, per il **nome** immettere **NativeAppToWebApi** e in **applicazioni client-server** selezionare l' **applicazione nativa che accede a un modello di API Web** . Fai clic su **Next**.  
  
      ![Reg app](media/adfs-msal-native-app-web-api/native2.png)  

  3. Copia il **identificatore Client** valore. Verrà usato in un secondo momento come valore per **ClientID** nel file **app. config** dell'applicazione. Immettere quanto segue per l' **URI di reindirizzamento:** https://ToDoListClient. Fai clic su **Aggiungi**. Fai clic su **Next**.  
 
     ![Reg app](media/adfs-msal-native-app-web-api/native3.png) 

  4. Nella schermata Configura API Web immettere l' **identificatore:** https://localhost:44321/. Fai clic su **Aggiungi**. Fai clic su **Next**. Questo valore verrà usato in un secondo momento nel file **app. config** e **Web.** config dell'applicazione.
 
     ![Reg app](media/adfs-msal-native-app-web-api/native4.png)   
  
  5. Nella schermata applica criteri di controllo di accesso selezionare **Consenti tutti gli utenti** e fare clic su **Avanti**. 
  
     ![Reg app](media/adfs-msal-native-app-web-api/native5.png)   
  
  6. Nella schermata Configura autorizzazioni applicazione verificare che **OpenID** sia selezionato e fare clic su **Avanti**.  
     
     ![Reg app](media/adfs-msal-native-app-web-api/native6.png) 

  7. Nella schermata Riepilogo fare clic su **Avanti**.
  
  8. Nella schermata completa fare clic su **Chiudi**. 
  
  9. In gestione AD FS fare clic su **gruppi di applicazioni** e selezionare gruppo di applicazioni **NativeAppToWebApi** . Fare clic con il pulsante destro del mouse e selezionare **Proprietà**.
  
      ![Reg app](media/adfs-msal-native-app-web-api/native7.png)

  10. Nella schermata delle proprietà di NativeAppToWebApi selezionare **NativeAppToWebApi-API Web** in **API Web** e fare clic su **modifica.** 
  
      ![Reg app](media/adfs-msal-native-app-web-api/native8.png) 

  11. Nella schermata NativeAppToWebApi-proprietà API Web selezionare la scheda **regole di trasformazione rilascio** e fare clic su **Aggiungi regola...** 
  
      ![Reg app](media/adfs-msal-native-app-web-api/native9.png) 

  12. Nella procedura guidata Aggiungi regola attestazione di trasformazione selezionare **trasformazione di un'attestazione in ingresso** dal **modello di regola attestazione:** elenco a discesa e fare clic su **Avanti**.  
  
      ![Reg app](media/adfs-msal-native-app-web-api/native10.png) 

  13. Immettere **NameID** in **Nome regola attestazione:** campo. Selezionare **nome** per **tipo di attestazione in ingresso:** , **ID nome** per **tipo di attestazione in uscita:** e **nome comune** per il **formato ID nome in uscita:** . fare clic su **fine**.
  
      ![Reg app](media/adfs-msal-native-app-web-api/native11.png) 

  14. Fare clic su OK nella schermata delle proprietà dell'API Web NativeAppToWebApi, quindi sulla schermata Proprietà NativeAppToWebApi.  
 
## <a name="code-configuration"></a>Configurazione del codice 
Questa sezione illustra come configurare un'app nativa per l'utente di accesso e recuperare il token per chiamare l'API Web 

1. Scaricare l'esempio da [qui](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi) 

2. Aprire l'esempio con Visual Studio 

3. Aprire il file App.config. Modificare quanto segue: 
   - Ida: Authority-immettere https:///[nome host AD FS]/ADFS
   - Ida: ClientID: immettere il valore dell' **identificatore client** da #3 nella registrazione dell'app nella sezione ad FS precedente. 
   - Ida: RedirectUri-immettere il valore **URI di reindirizzamento** da #3 nella sezione relativa alla registrazione dell'App nel ad FS.
   - TODO: TodoListResourceId: immettere il valore dell' **identificatore** dalla #4 nella registrazione dell'app nella sezione ad FS precedente 
   - Ida: todo: TodoListBaseAddress-immettere il valore dell' **identificatore** da #4 nella sezione relativa alla registrazione dell'App nel ad FS precedente. 
 
     ![configurazione del codice](media/adfs-msal-native-app-web-api/native12.png)

 4. Aprire il file Web. config. Modificare quanto segue: 
    - Ida: audience: immettere il valore dell' **identificatore** dall'#4 nella registrazione dell'app nella sezione ad FS precedente 
    - Ida: AdfsMetadataEndpoint-immettere https:///[nome host AD FS]/FederationMetadata/2007-06/FederationMetadata.XML 
    
      ![configurazione del codice](media/adfs-msal-native-app-web-api/native13.png)
 
  
## <a name="test-the-sample"></a>Testare l'esempio 
In questa sezione viene illustrato come eseguire il test dell'esempio configurato in precedenza. 

  1. Una volta apportate modifiche al codice, ricompilare la soluzione 
 
  2. In Visual Studio fare clic con il pulsante destro del mouse su soluzione e selezionare **Imposta progetti di avvio...**  
 
     ![Test app](media/adfs-msal-native-app-web-api/native14.png)

  3. Nelle pagine delle proprietà assicurarsi che **azione** sia impostato su **Avvia** per ogni progetto 
      
     ![Test app](media/adfs-msal-native-app-web-api/native15.png)

  4. Nella parte superiore di Visual Studio fare clic sulla freccia verde.  
 
     ![Test app](media/adfs-msal-native-app-web-api/native16.png)

  5. Nella schermata principale dell'app nativa fare clic su **Accedi**.  
  
     ![Test app](media/adfs-msal-native-app-web-api/native17.png)

    Se non viene visualizzata la schermata app nativa, cercare e rimuovere i file * msalcache. bin dalla cartella in cui è stato salvato il repository del progetto nel sistema. 

  6. Si sarà reindirizzati alla pagina di accesso di ADFS. Andare avanti ed eseguire l'accesso. 
  
      ![Test app](media/adfs-msal-native-app-web-api/native18.png)

  7. Dopo aver eseguito l'accesso, immettere testo **Compila app native nell'API Web** nell' **elemento crea un'attività**. Fare clic su **Aggiungi elemento**.  Questa operazione chiamerà il **servizio to do list (API Web)** e aggiungerà l'elemento nella cache. 
    
       ![Test app](media/adfs-msal-native-app-web-api/native19.png)
 
## <a name="next-steps"></a>Passaggi successivi
[Flussi e scenari applicativi di OpenID Connect/OAuth in AD FS](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 