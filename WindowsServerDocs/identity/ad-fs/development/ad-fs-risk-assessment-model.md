---
title: Creazione di plug-in con AD FS 2019 modello di valutazione dei rischi
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: af7a565fb5b3745531497ed9119976418eb6dcd7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857524"
---
# <a name="build-plug-ins-with-ad-fs-2019-risk-assessment-model"></a>Creazione di plug-in con AD FS 2019 modello di valutazione dei rischi

È ora possibile creare plug-in personalizzati per bloccare o assegnare un punteggio di rischio alle richieste di autenticazione durante varie fasi: richiesta ricevuta, pre-autenticazione e post-autenticazione. Questa operazione può essere eseguita usando il nuovo modello di valutazione dei rischi introdotto con AD FS 2019. 

## <a name="what-is-the-risk-assessment-model"></a>Che cos'è il modello di valutazione dei rischi?

Il modello di valutazione dei rischi è un set di interfacce e classi che consentono agli sviluppatori di leggere le intestazioni delle richieste di autenticazione e di implementare la propria logica di valutazione dei rischi. Il codice implementato (plug-in) viene quindi eseguito in linea con AD FS processo di autenticazione. Per esempio, usando le interfacce e le classi incluse con il modello, è possibile implementare il codice per bloccare o consentire la richiesta di autenticazione in base all'indirizzo IP del client incluso nell'intestazione della richiesta. AD FS eseguirà il codice per ogni richiesta di autenticazione e intraprenderà le azioni appropriate in base alla logica implementata. 

Il modello consente di inserire il codice in una delle tre fasi di AD FS pipeline di autenticazione, come illustrato di seguito.

![modello](media/ad-fs-risk-assessment-model/risk1.png)

1.    **Fase ricezione richiesta** : consente la compilazione di plug-in per consentire o bloccare la richiesta quando ad FS riceve la richiesta di autenticazione, ad esempio prima che l'utente immetta le credenziali. È possibile usare il contesto della richiesta, ad esempio IP del client, metodo HTTP, DNS del server proxy e così via, disponibile in questa fase per eseguire la valutazione dei rischi. Ad esempio, è possibile creare un plug-in per leggere l'IP dal contesto della richiesta e bloccare la richiesta di autenticazione se l'indirizzo IP è presente nell'elenco predefinito di indirizzi IP rischiosi. 

2.    **Fase di pre-autenticazione** : consente di creare plug-in per consentire o bloccare la richiesta nel punto in cui l'utente fornisce le credenziali ma prima che ad FS le valuti. In questa fase, oltre al contesto della richiesta, sono disponibili anche informazioni sul contesto di sicurezza (ad esempio, token utente, identificatore utente e così via) e il contesto del protocollo (ad esempio, protocollo di autenticazione, ClientID, ResourceId e così via) da usare nella logica di valutazione dei rischi. Ad esempio, è possibile creare un plug-in per impedire gli attacchi di spray per le password leggendo la password utente dal token utente e bloccando la richiesta di autenticazione se la password è nell'elenco predefinito di password rischiose. 

3.    **Post-autenticazione** : consente di creare plug-in per la valutazione del rischio dopo che l'utente ha fornito le credenziali e ad FS ha eseguito l'autenticazione. In questa fase, oltre al contesto della richiesta, al contesto di sicurezza e al contesto del protocollo, sono disponibili anche informazioni sul risultato dell'autenticazione (esito positivo o negativo). Il plug-in è in grado di valutare il Punteggio di rischio in base alle informazioni disponibili e di passare il Punteggio di rischio alle regole attestazioni e criteri per un'ulteriore valutazione. 

Per comprendere meglio come creare un plug-in di valutazione dei rischi ed eseguirlo in linea con AD FS processo, viene creato un plug-in di esempio che blocca le richieste provenienti da determinati indirizzi IP **Extranet** identificati come rischiosi, registra il plug-in con ad FS e infine testa la funzionalità. 

>[!NOTE]
>Questa procedura dettagliata è solo per illustrare come è possibile creare un plug-in di esempio. Non si tratta di una soluzione pronta per la creazione di una soluzione aziendale.  

## <a name="building-a-sample-plug-in"></a>Creazione di un plug-in di esempio

### <a name="pre-requisites"></a>Prerequisiti
Di seguito è riportato l'elenco dei prerequisiti necessari per compilare il plug-in di esempio

- AD FS 2019 installato e configurato
- .NET Framework 4,7 e versioni successive
- Visual Studio

### <a name="build-plug-in-dll"></a>Compila dll plug-in
Nella procedura riportata di seguito viene illustrata la creazione di una DLL plug-in di esempio.

1. Scaricare il plug-in di esempio, usare git bash e digitare quanto segue: 

   ```
   git clone https://github.com/Microsoft/adfs-sample-RiskAssessmentModel-RiskyIPBlock
   ```

2. Creare un file con **estensione CSV** in qualsiasi posizione nel server di ad FS (in questo caso, ho creato il file **authconfigdb. csv** in **C:\Extensions**) e aggiungere gli indirizzi IP che si desidera bloccare a questo file. 

   Il plug-in di esempio bloccherà le richieste di autenticazione provenienti dagli **indirizzi IP Extranet** elencati in questo file. 

   >{! Nota] Se si dispone di una farm di AD FS, è possibile creare il file in uno o tutti i server di AD FS. Tutti i file possono essere utilizzati per importare gli indirizzi IP rischiosi in AD FS. Il processo di importazione viene illustrato in dettaglio nella sezione [registrare la dll del plug-in con ad FS](#register-the-plug-in-dll-with-ad-fs) riportata di seguito. 

3. Aprire il progetto `ThreatDetectionModule.sln` con Visual Studio

4. Rimuovere il `Microsoft.IdentityServer.dll` da Esplora soluzioni, come mostrato di seguito:</br>
   modello di ![](media/ad-fs-risk-assessment-model/risk2.png)

5. Aggiungere un riferimento al `Microsoft.IdentityServer.dll` della AD FS, come illustrato di seguito

   a.    Fare clic con il pulsante destro del mouse su **riferimenti** in **Esplora soluzioni** e selezionare **Aggiungi riferimento...**</br>modello di  
   ![](media/ad-fs-risk-assessment-model/risk3.png)
   
   b.    Nella finestra **Gestione riferimenti** selezionare **Sfoglia**. Nel **selezionare i file a cui fare riferimento...** dialogo, selezionare `Microsoft.IdentityServer.dll` dalla cartella di installazione di AD FS (nel mio caso **C:\Windows\ADFS**) e fare clic su **Aggiungi**.
   
   >[!NOTE]
   >Nel mio caso, sto compilando il plug-in nel server AD FS stesso. Se l'ambiente di sviluppo si trova in un server diverso, copiare il `Microsoft.IdentityServer.dll` dalla cartella di installazione di AD FS in AD FS server nella casella di sviluppo.</br> 
   
   ![modello](media/ad-fs-risk-assessment-model/risk4.png)
   
   c.    Fare clic su **OK** nella finestra **Gestione riferimenti** dopo avere verificato `Microsoft.IdentityServer.dll` selezionata la casella di controllo</br>
   modello di ![](media/ad-fs-risk-assessment-model/risk5.png)
 
6. Tutte le classi e i riferimenti sono ora disponibili per eseguire una compilazione.   Tuttavia, poiché l'output di questo progetto è una dll, dovrà essere installato nella **global assembly cache**o nella GAC del server di ad FS e la dll deve essere firmata per prima. Questa operazione può essere eseguita come indicato di seguito:

   a.    **Fare clic con il pulsante destro del mouse** sul nome del progetto ThreatDetectionModule. Dal menu fare clic su **Proprietà**.</br>
   modello di ![](media/ad-fs-risk-assessment-model/risk6.png)
   
   b.    Nella pagina **Proprietà** fare clic su **firma**, a sinistra, quindi selezionare la casella di controllo contrassegnata come **Firma assembly**. Dal menu **a discesa Scegli un file chiave con nome sicuro**: selezionare **< nuovo... >**</br>
   modello di ![](media/ad-fs-risk-assessment-model/risk7.png)

   c.    Nella finestra di **dialogo Crea chiave con nome sicuro**Digitare un nome (è possibile scegliere qualsiasi nome) per la chiave, deselezionare la casella di controllo **Proteggi file di chiave con password**. Successivamente, scegliere **OK**.
   modello di ![](media/ad-fs-risk-assessment-model/risk8.png)</br>
 
   d.    Salvare il progetto come illustrato di seguito</br>
   modello di ![](media/ad-fs-risk-assessment-model/risk9.png)

7. Compilare il progetto facendo clic su **Compila** e quindi su **Ricompila soluzione** come illustrato di seguito.</br>
   modello di ![](media/ad-fs-risk-assessment-model/risk10.png)
 
   Controllare la **finestra di output**nella parte inferiore della schermata per verificare se si sono verificati errori</br>
   modello di ![](media/ad-fs-risk-assessment-model/risk11.png)


Il plug-in (dll) è ora pronto per l'uso e si trova nella cartella **\bin\Debug** della cartella del progetto (in questo caso, **C:\extensions\ThreatDetectionModule\bin\Debug\ThreatDetectionModule.dll**). 

Il passaggio successivo consiste nel registrare questa dll con AD FS, in modo che venga eseguita in linea con AD FS processo di autenticazione. 

### <a name="register-the-plug-in-dll-with-ad-fs"></a>Registrare la dll del plug-in con AD FS

È necessario registrare la dll in AD FS usando il comando `Register-AdfsThreatDetectionModule` PowerShell nel server AD FS, tuttavia, prima di eseguire la registrazione, è necessario ottenere il token di chiave pubblica. Questo token di chiave pubblica è stato creato al momento della creazione della chiave e ha firmato la dll con tale chiave. Per informazioni sul token di chiave pubblica per la dll, è possibile usare **sn. exe** come indicato di seguito.

1. Copiare il file dll dalla cartella **\bin\Debug** in un altro percorso (in caso di copia in **C:\Extensions**)

2. Avviare il **prompt dei comandi per gli sviluppatori** per Visual Studio e passare alla directory contenente il file **sn. exe** (nel mio caso la directory è **C:\Programmi (x86) \microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**) ![Model](media/ad-fs-risk-assessment-model/risk12.png)

3. Eseguire il comando **sn** con il parametro **-T** e il percorso del file (nel mio caso `SN -T "C:\extensions\ThreatDetectionModule.dll"`) ![modello](media/ad-fs-risk-assessment-model/risk13.png)</br>
   Il comando fornirà il token di chiave pubblica (per me, il **token di chiave pubblica è 714697626ef96b35**)

4. Aggiungere la dll alla **global assembly cache** del server ad FS la procedura consigliata consiste nel creare un programma di installazione appropriato per il progetto e utilizzare il programma di installazione per aggiungere il file alla GAC. Un'altra soluzione consiste nell'utilizzare **gacutil. exe** (ulteriori informazioni su **gacutil. exe** disponibili [qui](https://docs.microsoft.com/dotnet/framework/tools/gacutil-exe-gac-tool)) nel computer di sviluppo.  Poiché ho installato Visual Studio sullo stesso server AD FS, utilizzerò **gacutil. exe** come indicato di seguito

   a.    In Prompt dei comandi per gli sviluppatori per Visual Studio e passare alla directory contenente il file **gacutil. exe** (in questo caso la directory è **c:\Programmi (x86) \Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools**)

   b.    Eseguire il comando **gacutil** (nel mio caso `Gacutil /IF C:\extensions\ThreatDetectionModule.dll`) ![modello](media/ad-fs-risk-assessment-model/risk14.png)
 
   >[!NOTE]
   >Se si dispone di una farm di AD FS, è necessario eseguire la precedente in ogni AD FS server della farm. 

5. Aprire **Windows PowerShell** ed eseguire il comando seguente per registrare la dll
   ```
   Register-AdfsThreatDetectionModule -Name "<Add a name>" -TypeName "<class name that implements interface>, <dll name>, Version=10.0.0.0, Culture=neutral, PublicKeyToken=< Add the Public Key Token from Step 2. above>" -ConfigurationFilePath "<path of the .csv file>"
   ```
   Nel mio caso, il comando è: 
   ```
   Register-AdfsThreatDetectionModule -Name "IPBlockPlugin" -TypeName "ThreatDetectionModule.UserRiskAnalyzer, ThreatDetectionModule, Version=10.0.0.0, Culture=neutral, PublicKeyToken=714697626ef96b35" -ConfigurationFilePath "C:\extensions\authconfigdb.csv"
   ```
 
   >[!NOTE]
   >È necessario registrare la dll solo una volta, anche se si dispone di una farm AD FS. 

6. Riavviare il servizio AD FS dopo la registrazione della dll

Il file dll è ora registrato con AD FS e pronto per l'uso.

 >[!NOTE]
 > Se vengono apportate modifiche al plug-in e il progetto viene ricompilato, la DLL aggiornata deve essere registrata nuovamente. Prima di eseguire la registrazione, è necessario annullare la registrazione della dll corrente usando il comando seguente:</br></br>
 >`
  UnRegister-AdfsThreatDetectionModule -Name "<name used while registering the dll in 5. above>"
 >`</br></br> Nel mio caso, il comando è:
 >``` 
 >UnRegister-AdfsThreatDetectionModule -Name "IPBlockPlugin"
 >```

### <a name="testing-the-plug-in"></a>Test del plug-in

1. Aprire il file **authconfig. csv** creato in precedenza (nel mio caso nel percorso **C:\Extensions**) e aggiungere gli **indirizzi IP Extranet** che si desidera bloccare. Ogni IP deve trovarsi su una riga separata e non devono essere presenti spazi alla fine</br>
   modello di ![](media/ad-fs-risk-assessment-model/risk18.png)
 
2. Salvare e chiudere il file

3. Importare il file aggiornato in AD FS eseguendo il comando PowerShell seguente 

   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "<name given while registering the dll>" -ConfigurationFilePath "<path of the .csv file>"
   ```
 
   Nel mio caso, il comando è: 
   ```
   Import-AdfsThreatDetectionModuleConfiguration -name "IPBlockPlugin" -ConfigurationFilePath "C:\extensions\authconfigdb.csv")
   ```
 
4. Avviare la richiesta di autenticazione dal server con lo stesso indirizzo IP aggiunto in **authconfig. csv**.

   Per questa dimostrazione, utilizzerò [ad FS strumento di attestazione X-Ray](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) per avviare una richiesta. Per usare lo strumento X-Ray, seguire le istruzioni 

   Immettere l'istanza del server federativo e il pulsante di autenticazione di hit **test** .</br>modello di  
   ![](media/ad-fs-risk-assessment-model/risk15.png) 

5. L'autenticazione è bloccata come illustrato di seguito.</br>
   modello di ![](media/ad-fs-risk-assessment-model/risk16.png)
 
Ora che si è appreso come compilare e registrare il plug-in, è possibile eseguire la procedura dettagliata del codice del plug-in per comprendere l'implementazione usando le nuove interfacce e classi introdotte con il modello. 

## <a name="plug-in-code-walkthrough"></a>Procedura dettagliata del codice del plug-in

Aprire il progetto `ThreatDetectionModule.sln` con Visual Studio e quindi aprire il file principale **UserRiskAnalyzer.cs** da **Esplora soluzioni** a destra della schermata</br>
modello di ![](media/ad-fs-risk-assessment-model/risk17.png)
 
Il file contiene la classe principale UserRiskAnalyzer che implementa la classe astratta [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) e l'interfaccia [IRequestReceivedThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) per leggere l'IP dal contesto della richiesta, confrontare l'IP ottenuto con gli IP caricati da ad FS DB e bloccare la richiesta in caso di corrispondenza IP. Esaminare più in dettaglio questi tipi

### <a name="threatdetectionmodule-abstract-class"></a>Classe astratta ThreatDetectionModule

Questa classe astratta carica il plug-in in AD FS pipeline rendendo possibile l'esecuzione del codice del plug-in in linea con AD FS processo. 

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
La classe include i metodi e le proprietà seguenti.

|Metodo |Type|Definizione|
|-----|-----|-----| 
|[OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) |Vuoto|Chiamata eseguita da AD FS quando il plug-in viene caricato nella relativa pipeline| 
|[OnAuthenticationPipelineUnload](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineunload?view=adfs-2019) |Vuoto|Chiamata eseguita da AD FS quando il plug-in viene scaricato dalla relativa pipeline| 
|[OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019)| Vuoto|Chiamata eseguita da AD FS all'aggiornamento della configurazione |
|**Proprietà** |**Type** |**Definizione**|
|[NomeFornitore](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.vendorname?view=adfs-2019)|String |Ottiene il nome del fornitore che possiede il plug-in|
|[ModuleIdentifier](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.moduleidentifier?view=adfs-2019)|String |Ottiene l'identificatore del plug-in|

Nel plug-in di esempio vengono utilizzati i metodi [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) e [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) per leggere gli IP predefiniti da ad FS DB. [OnAuthenticationPipelineLoad](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onauthenticationpipelineload?view=adfs-2019) viene chiamato quando il plug-in viene registrato con ad FS mentre [OnConfigurationUpdate](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule.onconfigurationupdate?view=adfs-2019) viene chiamato quando il file con estensione CSV viene importato tramite il cmdlet `Import-AdfsThreatDetectionModuleConfiguration`. 

#### <a name="irequestreceivedthreatdetectionmodule-interface"></a>Interfaccia IRequestReceivedThreatDetectionModule

Questa [interfaccia](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule?view=adfs-2019) consente di implementare la valutazione dei rischi nel punto in cui ad FS riceve la richiesta di autenticazione, ma prima che l'utente immetta le credenziali, ad esempio alla fase di richiesta ricevuta del processo di autenticazione. 
 
```
public interface IRequestReceivedThreatDetectionModule
{
Task<ThrottleStatus> EvaluateRequest (
ThreatDetectionLogger logger, 
RequestContext requestContext );
}
```

L'interfaccia include il metodo [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) che consente di usare il contesto della richiesta di autenticazione passata nel parametro di input RequestContext per scrivere la logica di valutazione dei rischi. Il parametro requestContext è di tipo [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019). 

L'altro parametro di input passato è logger, che è di tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Il parametro può essere utilizzato per scrivere i messaggi di errore, di controllo e/o di debug in log di AD FS. 

Il metodo restituisce [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 se NotEvaluated, 1 to Block e 2 to allow) per ad FS che quindi blocca o consente la richiesta.

Nel plug-in di esempio, l'implementazione del metodo [EvaluateRequest](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.irequestreceivedthreatdetectionmodule.evaluaterequest?view=adfs-2019) analizza il [ClientIpAddress](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext.clientipaddresses?view=adfs-2019#Microsoft_IdentityServer_Public_ThreatDetectionFramework_RequestContext_ClientIpAddresses) dal parametro [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019) e lo confronta con tutti gli indirizzi IP caricati dal database ad FS. Se viene trovata una corrispondenza, il metodo restituisce 2 per il **blocco**. in caso contrario, restituisce 1 per **Allow**. In base al valore restituito, AD FS blocca o consente la richiesta. 

>[!NOTE]
>Il plug-in di esempio illustrato in precedenza implementa solo l'interfaccia IRequestReceivedThreatDetectionModule. Tuttavia, il modello di valutazione dei rischi fornisce due interfacce aggiuntive: IPreAuthenticationThreatDetectionModule (per implementare la fase di pre-autenticazione della logica di valutazione del rischio durante) e IPostAuthenticationThreatDetectionModule (per implementare la logica di valutazione dei rischi durante la fase di post-autenticazione). Di seguito sono riportati i dettagli sulle due interfacce. 

#### <a name="ipreauthenticationthreatdetectionmodule-interface"></a>Interfaccia IPreAuthenticationThreatDetectionModule 

Questa [interfaccia](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule?view=adfs-2019) consente di implementare la logica di valutazione dei rischi nel punto in cui l'utente fornisce le credenziali ma prima che ad FS le valuti, ad esempio la fase di pre-autenticazione. 

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
L'interfaccia include il metodo [EvaluatePreAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipreauthenticationthreatdetectionmodule.evaluatepreauthentication?view=adfs-2019) che consente di usare le informazioni passate in RequestContext [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)e [IList<Claim>](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) parametri di input di additionalClams per scrivere la logica di valutazione dei rischi di pre-autenticazione. 

>[!NOTE]
>Per l'elenco delle proprietà passate con ogni tipo di contesto, vedere le definizioni di classe [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)e [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) . 

L'altro parametro di input passato è logger, che è di tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Il parametro può essere utilizzato per scrivere i messaggi di errore, di controllo e/o di debug in log di AD FS.

Il metodo restituisce [ThrottleStatus](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.throttlestatus?view=adfs-2019) (0 se NotEvaluated, 1 to Block e 2 to allow) per ad FS che quindi blocca o consente la richiesta. 

#### <a name="ipostauthenticationthreatdetectionmodule-interface"></a>Interfaccia IPostAuthenticationThreatDetectionModule

Questa [interfaccia](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule?view=adfs-2019) consente di implementare la logica di valutazione del rischio dopo che l'utente ha fornito le credenziali e ad FS ha eseguito l'autenticazione, ad esempio la fase di post-autenticazione. 

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

L'interfaccia include il metodo [EvaluatePostAuthentication](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.ipostauthenticationthreatdetectionmodule.evaluatepostauthentication?view=adfs-2019) che consente di usare le informazioni passate in RequestContext [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019), [ProtocolContext ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019)e [IList<Claim>](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?view=netframework-4.7.2) parametri di input di additionalClams per scrivere la logica di valutazione dei rischi post-autenticazione. 

>[!NOTE]
> Per un elenco completo delle proprietà passate con ogni tipo di contesto, vedere le definizioni delle classi [RequestContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.requestcontext?view=adfs-2019), [SecurityContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.securitycontext?view=adfs-2019)e [ProtocolContext](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.protocolcontext?view=adfs-2019) . 

L'altro parametro di input passato è logger, che è di tipo [ThreatDetectionLogger](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionlogger?view=adfs-2019). Il parametro può essere utilizzato per scrivere i messaggi di errore, di controllo e/o di debug in log di AD FS. 

Il metodo restituisce il [Punteggio di rischio](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.authentication.riskscoreconstants?view=adfs-2019) che può essere utilizzato in ad FS criteri e regole attestazioni. 

>[!NOTE]
>Per il funzionamento del plug-in, la classe principale (in questo caso UserRiskAnalyzer) deve derivare la classe astratta [ThreatDetectionModule](https://docs.microsoft.com/dotnet/api/microsoft.identityserver.public.threatdetectionframework.threatdetectionmodule?view=adfs-2019) e deve implementare almeno una delle tre interfacce descritte in precedenza. Una volta registrata la dll, AD FS controlla quale delle interfacce sono implementate e le chiama nella fase appropriata della pipeline.

### <a name="faqs"></a>domande frequenti

**Perché è necessario compilare questi plug-in?**</br>
**R:** Questi plug-in non solo forniscono funzionalità aggiuntive per proteggere l'ambiente dagli attacchi, ad esempio gli attacchi di spray per le password, ma offrono anche la flessibilità necessaria per creare la propria logica di valutazione dei rischi in base alle esigenze. 

**Dove vengono acquisiti i log?**</br>
**R:** È possibile scrivere i log degli errori nel registro eventi "AD FS/admin" usando il metodo WriteAdminLogErrorMessage, i log di controllo per "AD FS il controllo" del registro di sicurezza usando il metodo WriteAuditMessage e i log di debug nel log di debug "AD FS tracing" con il metodo WriteDebugMessage. 

**È possibile aggiungere questi plug-in per aumentare AD FS latenza del processo di autenticazione?**</br>
**R:** L'impatto sulla latenza sarà determinato dal tempo impiegato per eseguire la logica di valutazione dei rischi implementata. È consigliabile valutare l'effetto di latenza prima di distribuire il plug-in nell'ambiente di produzione. 

**Perché non è possibile AD FS suggerire l'elenco di indirizzi IP rischiosi, utenti e così via?**</br>
**R:** Sebbene non sia attualmente disponibile, stiamo lavorando alla creazione dell'Intelligence per suggerire indirizzi IP rischiosi, utenti e così via nel modello di valutazione dei rischi di collegamento. Le date di lancio saranno condivise a breve. 
