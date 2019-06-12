---
title: Creare Plug-in con modello di valutazione di AD FS 2019 rischio
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: ''
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e43f505a02ec2241a84f74ff57e217c2fb95157b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445348"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>Creare Plug-in con modello di valutazione di AD FS 2019 rischio

È ora possibile compilare il proprio plug-in per bloccare o assegnare un punteggio di rischio per le richieste di autenticazione durante le varie fasi – richiesta ricevuto, pre- autenticazione e post-autenticazione. Ciò è possibile usare il nuovo modello di valutazione dei rischi introdotti con AD FS 2019. 

## <a name="what-is-the-risk-assessment-model"></a>Che cos'è il modello di valutazione del rischio?

Il modello di valutazione dei rischi è un set di interfacce e classi che consentono agli sviluppatori di leggere le intestazioni di richiesta di autenticazione e implementare la propria logica di valutazione dei rischi. Il codice implementato (plug-in) viene quindi eseguito in linea con il processo di autenticazione di AD FS. Per Usa le interfacce e classi incluse con il modello, ad esempio, è possibile implementare uno dei blocchi di codice o consentire richiesta di autenticazione basato su indirizzo IP del client incluso nell'intestazione della richiesta. AD FS verrà eseguito il codice per ogni richiesta di autenticazione e intraprendere l'azione appropriata in base alla logica implementata. 

Il modello consente al codice del plug-in uno qualsiasi dei tre fasi della pipeline di autenticazione ADFS come illustrato di seguito

![modello](media/ad-fs-risk-assessment-model/risk1.png)

1.  **Richiesta ricevuta fase** : abilita la creazione di plug-in per consentire o bloccare richiesta quando ADFS riceve la richiesta di autenticazione, ovvero prima che l'utente immette le credenziali. È possibile usare il contesto della richiesta (ad esempio, IP client, il metodo Http, il server proxy DNS e così via) disponibili in questa fase per la valutazione dei rischi. Per ad esempio, è possibile creare un plug-in per leggere l'indirizzo IP dal contesto della richiesta e blocco, la richiesta di autenticazione se l'indirizzo IP nell'elenco di indirizzi IP rischiosi predefinito. 

2.  **Fase di preautenticazione** : abilita la creazione di plug-in per consentire o bloccare richiesta nel punto in cui l'utente fornisce le credenziali ma prima che li valuterà AD FS. In questa fase, oltre al contesto della richiesta è necessario anche informazioni sul contesto di sicurezza (ad esempio, token dell'utente, identificatore utente e così via) e il contesto di protocollo (ad esempio, il protocollo di autenticazione, ID client, resourceid, e così via) da utilizzare nella logica di valutazione del rischio. Per ad esempio, è possibile creare un plug-in per impedire attacchi di spray password leggendo la password dell'utente dal token di utente e bloccando la richiesta di autenticazione, se la password non è nell'elenco predefinito delle password a rischio. 

3.  **Post-autenticazione** : abilita la creazione di plug-in per valutare i rischi dopo che l'utente ha fornito le credenziali e AD FS ha eseguito l'autenticazione. In questa fase, oltre al contesto della richiesta, contesto di sicurezza e il contesto di protocollo, è necessario anche informazioni sul risultato autenticazione (esito positivo o negativo). Il plug-in può valutare il punteggio di rischio basato sulle informazioni disponibili e passare il punteggio di rischio per l'attestazione e le regole di criteri per l'ulteriore valutazione. 

Per comprendere meglio come creare una valutazione dei rischi del plug-in ed eseguirla in linea con il processo di AD FS, è possibile compilare un plug-in esempio che blocca le richieste provenienti da determinate **extranet** gli indirizzi IP identificato come rischioso, registrare il plug-in con AD FS e infine testare la funzionalità. 

>[!NOTE]
>Questa procedura dettagliata è solo per mostrarvi come è possibile creare un esempio di plug-in. È in alcun modo la soluzione che stiamo creando una soluzione aziendale per pronto.  

## <a name="building-a-sample-plug-in"></a>La compilazione di un plug-in esempio

### <a name="pre-requisites"></a>Prerequisiti
Seguito è riportato l'elenco dei prerequisiti necessari per compilare questo esempio plug-in

- AD FS 2019 installato e configurato
- .NET framework 4.7 e versioni successive
- Visual Studio

### <a name="build-plug-in-dll"></a>Compilare la dll del plug-in
La procedura seguente guiderà si compilava una dll plug-in esempio.

1. Scaricare il plug-in di esempio, usare Git Bash e digitare quanto segue: 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

2. Creare un **CSV** file in qualsiasi posizione nel server AD FS (In questo caso, ho creato il **authconfigdb.csv** file alla **C:\extensions**) e aggiungere gli indirizzi IP da bloccare per questo file. 

   Il plug-in esempio blocca tutte le richieste provenienti dal **gli indirizzi IP Extranet** elencati in questo file. 

   >{! Nota] Se si dispone di una Farm AD FS, è possibile creare il file in uno o tutti i server AD FS. Uno dei file è utilizzabile per importare gli indirizzi IP rischiosi in AD FS. Verranno illustrati il processo di importazione in modo dettagliato nel [registrare la dll del plug-in con AD FS](#register-the-plug-in-dll-with-ad-fs) sezione riportata di seguito. 

3. Aprire il progetto `ThreatDetectionModule.sln` usando Visual Studio

4. Rimuovere il `Microsoft.IdentityServer.dll` da Esplora soluzioni come illustrato di seguito:</br>
   ![model](media/ad-fs-risk-assessment-model/risk2.png)

5. Aggiungi riferimento al `Microsoft.IdentityServer.dll` di ADFS come illustrato di seguito

   a.   Fare clic con il pulsante destro sul **riferimenti** nelle **Esplora soluzioni** e selezionare **Aggiungi riferimento...**</br> 
   ![Modello](media/ad-fs-risk-assessment-model/risk3.png)
   
   b.   Nel **gestione riferimenti** finestra selezionare **Sfoglia**. Nel **selezionare i file a cui fare riferimento...** finestra di dialogo, seleziona `Microsoft.IdentityServer.dll` dalla cartella di installazione di AD FS (in questo caso **C:\Windows\ADFS**) e fare clic su **Add**.
   
   >[!NOTE]
   >In questo caso che sta creando il plug-nel server AD FS. Se l'ambiente di sviluppo è in un server diverso, copiare il `Microsoft.IdentityServer.dll` dalla cartella di installazione di AD FS nel server AD FS alla casella di sviluppo.</br> 
   
   ![modello](media/ad-fs-risk-assessment-model/risk4.png)
   
   c.   Fare clic su **OK** nel **gestione riferimenti** finestra dopo essersi assicurati che `Microsoft.IdentityServer.dll` casella di controllo è selezionata</br>
   ![model](media/ad-fs-risk-assessment-model/risk5.png)
 
6. Tutte le classi e i riferimenti sono ora in grado di eseguire la compilazione.   Tuttavia, poiché l'output di questo progetto è una dll, dovrà essere installato nel **Global Assembly Cache**, o nella GAC, del server ADFS e la dll deve essere firmato prima di tutto. Questa operazione può essere eseguita come indicato di seguito:

   a.   **Fare doppio clic su** sul nome del progetto, ThreatDetectionModule. Dal menu, fare clic su **proprietà**.</br>
   ![model](media/ad-fs-risk-assessment-model/risk6.png)
   
   b.   Dal **proprietà** pagina, fare clic su **firma**, a sinistra, quindi selezionare la casella di controllo è contrassegnato **firmare l'assembly**. Dal **Scegli un file chiave con nome sicuro**: dal menu a discesa **< New... >**</br>
   ![model](media/ad-fs-risk-assessment-model/risk7.png)

   c.   Nel **finestra di dialogo Crea chiave con nome sicuro**, digitare un nome (è possibile scegliere qualsiasi nome) per la chiave, deselezionare la casella di controllo **Proteggi file di chiave con password**. Fare clic su **OK**.
   ![model](media/ad-fs-risk-assessment-model/risk8.png)</br>
 
   d.   Salvare il progetto, come illustrato di seguito</br>
   ![model](media/ad-fs-risk-assessment-model/risk9.png)

7. Compilare il progetto facendo clic **compilare** e quindi **Ricompila soluzione** come illustrato di seguito</br>
   ![model](media/ad-fs-risk-assessment-model/risk10.png)
 
   Verificare i **finestra di Output**, nella parte inferiore della schermata, per vedere se si verificano errori</br>
   ![model](media/ad-fs-risk-assessment-model/risk11.png)


Il plug-in (dll) è ora pronta per l'uso ed è nel **\bin\Debug** cartella della cartella del progetto (nel mio caso, ecco **C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**). 

Il passaggio successivo consiste nel registrare la dll con AD FS, affinché venga eseguito in linea con il processo di autenticazione di AD FS. 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>Registrare la dll del plug-in con AD FS

È necessario registrare la dll in AD FS usando la `Register-AdfsThreatDetectionModule` comando di PowerShell nel server AD FS, tuttavia, prima della registrazione, è necessario ottenere il Token di chiave pubblica. Questo token di chiave pubblica è stato creato quando è creata la chiave e la dll utilizzando tale chiave di firma. Per informazioni su ciò che il Token di chiave pubblica per la dll, è possibile usare la **SN.exe** come indicato di seguito

1. Copiare il file dll dal **\bin\Debug** cartella a un'altra posizione (In questo caso copiarla negli **C:\extensions**)

2. Avviare il **prompt dei comandi sviluppatore** per Visual Studio e andare alla directory contenente il **sn.exe** (In questo caso la directory è **\Microsoft SDKs\Windows\v10.0A C:\Program Files (x86) strumenti \bin\NETFX 4.7.2**) ![modello](media/ad-fs-risk-assessment-model/risk12.png)

3. Eseguire la **SN** con il **-T** parametro e il percorso del file (In questo caso `SN -T “C:\extensions\ThreatDetectionModule.dll”`) ![modello](media/ad-fs-risk-assessment-model/risk13.png)</br>
   Il comando verrà visualizzato il token di chiave pubblica (per me, la **Token di chiave pubblica è 714697626ef96b35**)

4. Aggiungere la dll per il **Global Assembly Cache** del server AD FS nostro consigliata sarebbe creare un programma di installazione appropriato per il progetto e usare il programma di installazione per aggiungere il file alla Global Assembly Cache. Un'altra soluzione consiste nell'usare **Gacutil.exe** (altre informazioni sul **Gacutil.exe** disponibili [qui](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)) nel computer di sviluppo.  Poiché ho my visual studio nello stesso server di AD FS, si userà **Gacutil.exe** come indicato di seguito

   a.   Nel prompt dei comandi sviluppatore per Visual Studio e andare alla directory contenente il **Gacutil.exe** (In questo caso la directory è **c:\Programmi (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**)

   b.   Eseguire la **Gacutil** comando (In questo caso `Gacutil /IF C:\extensions\ThreatDetectionModule.dll`) ![modello](media/ad-fs-risk-assessment-model/risk14.png)
 
   >[!NOTE]
   >Se si dispone di una farm AD FS, il codice precedente deve essere eseguito in ogni server AD FS nella farm. 

5. Aprire **Windows PowerShell** ed eseguire il comando seguente per registrare la dll
   ```
   Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>”
   ```
   Nel mio caso, il comando è: 
   ```
   Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv”
   ```
 
   >[!NOTE]
   >È necessario registrare la dll di una sola volta, anche se si dispone di una farm AD FS. 

6. Riavviare il servizio AD FS dopo la registrazione della dll

A questo punto, la dll è ora registrata in AD FS e pronto per l'uso.

 >[!NOTE]
 > Se vengono apportate modifiche per il plug-in e il progetto viene ricompilato, le dll aggiornate deve quindi essere registrati nuovamente. Prima della registrazione, è necessario annullare la registrazione della dll corrente usando il comando seguente:</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> Nel mio caso, il comando è:
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>Il plug-in di test

1. Aprire il **authconfig.csv** file creato in precedenza (nel mio caso nella posizione **C:\extensions**) e aggiungere il **Extranet gli indirizzi IP** si vuole bloccare. Ogni indirizzo IP deve essere su una riga separata e non devono essere presenti spazi alla fine</br>
   ![model](media/ad-fs-risk-assessment-model/risk18.png)
 
2. Salvare e chiudere il file

3. Importare il file aggiornato in ADFS eseguendo il comando PowerShell seguente 

   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
   ```
 
   Nel mio caso, il comando è: 
   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
   ```
 
4. Richiesta l'autenticazione venga inizializzata dal server con lo stesso indirizzo IP aggiunto nel **authconfig.csv**.

   Per questa dimostrazione, si userà [dello strumento di AD FS della Guida Claims x-ray](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) per avviare una richiesta. Se si desidera utilizzare lo strumento di x-ray, seguire le istruzioni 

   Immettere l'istanza del server federativo e hit **verifica autenticazione** pulsante.</br> 
   ![Modello](media/ad-fs-risk-assessment-model/risk15.png) 

5. L'autenticazione è bloccata come illustrato di seguito.</br>
   ![model](media/ad-fs-risk-assessment-model/risk16.png)
 
Ora che abbiamo imparato a creare e registrare il plug-in, è possibile procedura dettagliata il codice del plug-in per capire l'implementazione utilizzando le classi e nuove interfacce introdotte con il modello. 

## <a name="plug-in-code-walkthrough"></a>Procedura dettagliata del codice del plug-in

Aprire il progetto `ThreatDetectionModule.sln` tramite Visual Studio e quindi aprire il file principale **UserRiskAnalyzer.cs** dal **Esplora soluzioni** a destra della schermata</br>
![model](media/ad-fs-risk-assessment-model/risk17.png)
 
Il file contiene la classe principale che implementa la classe astratta UserRiskAnalyzer [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) e l'interfaccia [IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) per leggere l'indirizzo IP della richiesta contesto, confrontare l'indirizzo IP ottenuto con gli indirizzi IP caricati dal database di AD FS e del blocco richiesta se viene trovata una corrispondenza IP. È opportuno discutere di questi tipi in modo più dettagliato

### <a name="threatdetectionmodule-abstract-class"></a>Classe astratta ThreatDetectionModule

Questa classe astratta carica il plug-nella pipeline di ADFS rende possibile eseguire il codice del plug-in linea con il processo di AD FS. 

```
public abstract class ThreatDetectionModule 
{ 
    protected ThreatDetectionModule(); 
 
    public abstract string VendorName { get; } 
    public abstract string ModuleIdentifier { get; } 
 
 public abstract void OnAuthenticationPipelineLoad(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData); 
 public abstract void OnAuthenticationPipelineUnload(ThreatDetectionLogger logger); 
  public abstract void OnConfigurationUpdate(ThreatDetectionLogger logger, ThreatDetectionModuleConfiguration configData); 
 }   
```
La classe include metodi e le proprietà seguenti.

|Metodo |Type|Definizione|
|-----|-----|-----| 
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |Void|Chiamato da AD FS, quando il plug-in viene caricato nella relativa pipeline| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |Void|Chiamato da AD FS quando il plug-in viene scaricato dalla relativa pipeline| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| Void|Chiamato da AD FS al aggiornamento della configurazione |
|**Proprietà** |**Tipo** |**Definizione**|
|[VendorName](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|Stringa |Ottiene il nome del fornitore proprietario il plug-in|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|Stringa |Ottiene l'identificatore del plug-in|

Nel nostro esempio di plug-in, utilizziamo [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) e [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) metodi per la lettura di indirizzi IP predefiniti da AD FS DB. [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) viene chiamato quando il plug-in è registrato con AD FS durante [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) viene chiamato quando viene importato il CSV usando il `Import-AdfsThreatDetectionModuleConfiguration` cmdlet. 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>IRequestReceivedThreatDetectionModule Interface

Ciò [interfaccia](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) consente di implementare valutazione dei rischi in corrispondenza del punto in cui ADFS riceve la richiesta di autenticazione, ma prima che l'utente immette le credenziali, ad esempio in fase di richiesta ricevuto del processo di autenticazione. 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

Include l'interfaccia [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) metodo che consente di utilizzare il contesto della richiesta di autenticazione passato nel parametro di input requestContext per scrivere la logica di valutazione dei rischi. Il parametro requestContext JE typu [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019). 

L'altro parametro di input passato è logger che è di tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Il parametro può essere utilizzato per scrivere l'errore, controllare e/o eseguire il debug dei messaggi per i log di AD FS. 

Il metodo restituisce [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 se NotEvaluated, al blocco, 1 e 2 per Consenti) ad AD FS che quindi si blocca o consente la richiesta.

Nel nostro esempio plug-in [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) implementazione del metodo analizza il [clientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) dal [requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019) parametro e lo confronta con tutti gli indirizzi IP caricati dal database di AD FS. Se viene trovata una corrispondenza, metodo restituisce 2 per **Block**, in caso contrario restituisce 1 per **Consenti**. Basato sul valore restituito, ADFS blocca o consente la richiesta. 

>[!NOTE]
>Plug-in di esempio illustrato in precedenza implementa l'interfaccia IRequestReceivedThreatDetectionModule solo. Tuttavia, il modello di valutazione dei rischi fornisce altre due interfacce: IPreAuthenticationThreatDetectionModule (per implementare fase di preautenticazione durante la logica della valutazione dei rischi) e IPostAuthenticationThreatDetectionModule (per implementare rischio valutazione per la logica durante la fase di post-autenticazione). Vengono forniti i dettagli su due interfacce di seguito. 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>Interfaccia IPreAuthenticationThreatDetectionModule 

Ciò [interfaccia](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019) consente di implementare la logica di valutazione dei rischi in corrispondenza del punto in cui l'utente fornisce le credenziali, ma prima di AD FS viene valutata in fase di preautenticazione, ovvero. 

```
public interface IPreAuthenticationThreatDetectionModule
{
Task<ThrottleStatus> EvaluatePreAuthentication (
ThreatDetectionLogger logger, 
RequestContext requestContext, 
SecurityContext securityContext, 
ProtocolContext protocolContext, 
IList<Claim> additionalClams
);
}
```
Include l'interfaccia [EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019) metodo che consente di usare le informazioni passate nel [RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext securityContext ](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019), e [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) parametri per scrivere la logica di valutazione dei rischi di pre-autenticazione di input. 

>[!NOTE]
>Per l'elenco delle proprietà passato con ogni tipo di contesto, visitare [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), e [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) definizioni di classe. 

L'altro parametro di input passato è logger che è di tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Il parametro può essere utilizzato per scrivere l'errore, controllare e/o eseguire il debug dei messaggi per i log di AD FS.

Il metodo restituisce [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 se NotEvaluated, al blocco, 1 e 2 per Consenti) ad AD FS che quindi si blocca o consente la richiesta. 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>Interfaccia IPostAuthenticationThreatDetectionModule

Ciò [interfaccia](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019) consente di implementare la logica di valutazione dei rischi dopo che l'utente ha fornito le credenziali e AD FS ha eseguito l'autenticazione, ad esempio post-l'autenticazione di fase. 

```
public interface IPostAuthenticationThreatDetectionModule
{
Task<RiskScore> EvaluatePostAuthentication (
ThreatDetectionLogger logger, 
RequestContext requestContext, 
SecurityContext securityContext, 
ProtocolContext protocolContext, 
AuthenticationResult authenticationResult, 
IList<Claim> additionalClams
);
}
```

Include l'interfaccia [EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019) metodo che consente di usare le informazioni passate nel [RequestContext requestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext securityContext ](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext protocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019), e [IList<Claim> additionalClams](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) parametri per scrivere la logica di valutazione dei rischi di post-autenticazione di input. 

>[!NOTE]
> Per un elenco completo delle proprietà passato con ogni tipo di contesto, vedere [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), e [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) definizioni di classe. 

L'altro parametro di input passato è logger che è di tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Il parametro può essere utilizzato per scrivere l'errore, controllare e/o eseguire il debug dei messaggi per i log di AD FS. 

Il metodo restituisce il [punteggio di rischio](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019) che può essere usato nei criteri di AD FS e le regole di attestazione. 

>[!NOTE]
>Per plug-in del funzionamento, la classe principale (in questo caso UserRiskAnalyzer) è necessario derivare [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) classe astratta e deve implementare almeno una delle tre interfacce descritte in precedenza. Dopo aver registrata la dll, AD FS controlla quali interfacce vengono implementate e li chiama alla fase della pipeline appropriati.

### <a name="faqs"></a>Domande frequenti

**Perché creare questi plug-in?**</br>
**R:** Questi plug-in non solo si forniscono funzionalità aggiuntive per proteggere l'ambiente da attacchi di tipo password spray attacchi, ma offrono anche la flessibilità per creare una propria logica di valutazione dei rischi in base alle esigenze. 

**In cui vengono acquisiti i log?**</br>
**R:** È possibile scrivere i log degli errori in "ADFS/amministratore" log di eventi usando il metodo WriteAdminLogErrorMessage, i log di controllo di sicurezza "Controllo di AD FS" accedere usando il metodo WriteAuditMessage ed eseguire il debug dei log "Traccia di AD FS" log di debug usando il metodo WriteDebugMessage. 

**Possono aggiungere questi plug-in aumentare della latenza processo di autenticazione di AD FS?**</br>
**R:** Verrà determinato impatto della latenza per il tempo impiegato per eseguire la logica di valutazione dei rischi che è implementare. È consigliabile valutare l'impatto della latenza prima di distribuire il plug-nell'ambiente di produzione. 

**Perché ADFS non è possibile suggerire l'elenco di indirizzi IP rischiosi, utenti e così via?**</br>
**R:** Anche se non è attualmente disponibile, stiamo lavorando sulla compilazione di intelligence per suggerire gli indirizzi IP rischiosi, gli utenti e così via nel modello di valutazione dei rischi collegabile. Si condividerà il lancio date a breve. 
