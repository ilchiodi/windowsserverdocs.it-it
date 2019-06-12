---
title: Distribuire Windows Server Hybrid Cloud Print - autenticazione pass-through
description: Come impostare la stampa Cloud ibrida Microsoft con l'autenticazione pass-through
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: Windows Server 2016
ms.tgt_pltfrm: na
ms.topic: ''
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: 95cced8dc06cc4ee3768addf65e17cf379e20454
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435754"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-passthrough-authentication"></a>Distribuire Windows Server Hybrid Cloud stampa con l'autenticazione pass-through

>Si applica a: Windows Server 2016

Questo argomento, per gli amministratori IT, descrive la distribuzione end-to-end della soluzione stampa Cloud ibride Microsoft. Livelli questa soluzione nella parte superiore esistente uno o più server Windows in esecuzione come Server di stampa e consente di aggiunti ad Azure Active Directory e gestiti da MDM, le stampanti di gestire i dispositivi di individuare e di stampa all'organizzazione.

## <a name="pre-requisites"></a>Prerequisiti

Esistono numerose sottoscrizioni, servizi e computer, occorre acquisire prima di avviare questa installazione. Le visualizzazioni sono le seguenti:

-   Sottoscrizione di Azure AD premium
    
    Visualizzare [iniziare con una sottoscrizione di Azure](https://azure.microsoft.com/trial/get-started-active-directory/), per una sottoscrizione di valutazione in Azure. 

-   Servizio MDM, ad esempio Intune
    
    Visualizzare [Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune), per una sottoscrizione di valutazione di Intune.

-   Windows Server in esecuzione come Active Directory

    Vedere [dettagliate: Configurazione di Active Directory in Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/), per informazioni sulla configurazione di Active Directory.

-   Dominio Windows Server 2016 in esecuzione come Server di stampa
    
    Visualizzare [installare ruoli, servizi ruolo e funzionalità mediante l'aggiunta guidata ruoli e funzionalità](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw), per informazioni su come installare ruoli e dei servizi in Windows Server.

-   Azure AD Connect
    
    Visualizzare [installazione personalizzata di Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom), per informazioni sulla configurazione di Azure AD Connect.

-   Connettore del Proxy applicazione Azure in un dominio separato unita macchina Windows Server
    
    Visualizzare [iniziare a usare il Proxy di applicazione e installare il connettore](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable), per informazioni sulla configurazione di connettore del Proxy applicazione Azure.

-   Nome di dominio lato pubblico
    
    È possibile usare il nome di dominio creato automaticamente da Azure o acquistare il nome di dominio.

## <a name="deployment-steps"></a>Fasi di distribuzione

Questa guida illustra i passaggi di installazione di cinque (5):

- Passaggio 1: Installare Azure AD Connect per la sincronizzazione tra Azure AD e Active Directory locale
- Passaggio 2: Installare il pacchetto di stampa di Cloud ibrido nel Server di stampa
- Passaggio 3: Installare il Proxy di applicazione di Azure (AAP) con l'autenticazione pass-through
- Passaggio 4: Configurare i criteri MDM richiesti
- Passaggio 5: Pubblicare stampanti condivise

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>Passaggio 1: installare Azure AD Connect per la sincronizzazione tra Azure AD e Active Directory locale
1. Nel computer Windows Server Active Directory, scaricare il software di Azure AD Connect
2. Avviare il pacchetto di installazione di Azure AD Connect e selezionare **Usa impostazioni rapide**
3. Immettere le informazioni richieste nel processo di installazione e fare clic su **installare**

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>Passaggio 2 - pacchetto di installazione Hybrid Cloud stampa nel Server di stampa

1. Installare i moduli di PowerShell stampa di Cloud ibrido
   - Eseguire i comandi seguenti da un prompt dei comandi di PowerShell con privilegi elevati
      - `find-module -Name "PublishCloudPrinter"` per confermare che il computer possa raggiungere PowerShell Gallery (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

     > NOTA: È possibile visualizzare una messaggistica indicante che 'PSGallery' non è un repository non attendibile.  Immettere 'y' per continuare l'installazione.

2. Installare la soluzione Cloud ibrida Print
    - Nella stessa con privilegi elevati PowerShell prompt dei comandi, passare alla directory `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - Esegui <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. Configurare l'endpoint IIS 2 per il supporto SSL
   -   Il certificato SSL può essere un certificato autofirmato o emesso da alcune autorità di certificazione (CA) attendibile
   -  Se si usa certificato autofirmato, assicurarsi che il certificato viene importato per i computer client
4. Installare il pacchetto di SQLite
   - Aprire un prompt dei comandi di PowerShell con privilegi elevati
   - Eseguire il comando seguente per scaricare i pacchetti nuget System.Data.SQLite <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - Eseguire il comando seguente per installare i pacchetti<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > NOTA: È consigliabile scaricare e installare la versione più recente con l'omissione di "-requiredversion" opzione.

5. Copiare le DLL di SQLite per la MopriaCloudService Webapp \<bin\> cartella (**c:.\\inetpub\\wwwroot\\MopriaCloudService\\bin**): <br>
   - I file binari SQLite devono essere in "\\Program Files\\PackageManagement\\NuGet\\pacchetti"

           \\System.Data.SQLite.**Core**.x.x.x.x\\lib\\net46\\System.Data.SQLite.dll
           --\> \<bin\>\\System.Data.SQLite.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x86\\SQLite.Interop.dll
           --\> \<bin\>\\**x86**\\SQLite.Interop.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x64\\SQLite.Interop.dll
           --\> \<bin\>\\**x64**\\SQLite.Interop.dll
           \\System.Data.SQLite.**Linq**.x.x.x.x\\lib\\net46\\System.Data.SQLite.Linq.dll
           --\> \<bin\>\\System.Data.SQLite.Linq.dll  
           \\System.Data.SQLite.**EF6**.x.x.x.x\\lib\\net46\\System.Data.SQLite.EF6.dll
           --\> \<bin\>\\System.Data.SQLite.EF6.dll

   > Nota: x.x.x. x è la versione di SQLite installata in precedenza.

6. Aggiorna il `c:\inetpub\wwwroot\MopriaCloudService\web.config` file da includere la versione di SQLite x.x.x. x nel seguente \<runtime\>/\<assemblyBinding\> sezioni:

       <dependentAssembly>
       assemblyIdentity name="System.Data.SQLite" culture="neutral" publicKeyToken="db937bc2d44ff139" /
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Core" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.EF6" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Linq" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>

7. Creare il database SQLite:
    -  Scaricare e installare i file binari SQLite strumenti da <https://www.sqlite.org/>
    -  Passare a **c:\\inetpub\\wwwroot\\MopriaCloudService\\Database** directory
    -  Eseguire il comando seguente per creare il database in questa directory:  `sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  Da Esplora File, aprire le proprietà di file MopriaDeviceDb.db per aggiungere utenti o gruppi che sono consentiti per pubblicare database Mopria nella scheda sicurezza
        - Consiglia di aggiungere solo il gruppo di utenti amministratore obbligatorio.
8. Registrare l'app 2 web con Azure AD per supportare l'autenticazione OAuth2
   - Eseguire l'accesso come amministratore globale al portale di gestione del tenant Azure AD
     1. Passare la scheda "Registrazioni per l'App" per aggiungere endpoint di stampa
        - Aggiungere l'applicazione, selezionare "Registrazione nuova applicazione"
        - Specificare un nome appropriato e selezionare "app Web / API"
        - URL Sign-on = "<http://MicrosoftEnterpriseCloudPrint/CloudPrint>"
     2. Ripetere per l'endpoint di individuazione
        - URL Sign-on = "<http://MopriaDiscoveryService/CloudPrint>"
     3. Ripetere per l'applicazione client nativa
        -   Quando si fornisce il nome dell'app, assicurarsi di che selezionare "Applicazione Client nativa" come "tipo di applicazione"
        -   URI di reindirizzamento = "https://\<servizi-computer-endpoint\>/RedirectUrl"
     4. Passare all'App Client nativa "Impostazioni"
        -   Copiare il valore di "Application ID" da utilizzare per i passaggi successivi il programma di installazione
        -   Selezionare "Autorizzazioni necessarie"
            1.  Fare clic su "Aggiungi"
            2.  Fare clic su "Selezionare un'API"
            3.  Cerca per l'endpoint di stampa e l'endpoint di individuazione per il nome che è definito quando si crea l'endpoint dell'app
            4.  Aggiungere gli 2 endpoint
            5.  Verificare che sia abilitata l'opzione "Autorizzazioni delegate" per ogni endpoint di app
            6.  Assicurarsi che fare clic sul pulsante "Fine" nella parte inferiore
            7.  Fare clic su "Concessione autorizzazioni", dopo che entrambi gli endpoint sono stati aggiunti.  Selezionare "Sì" quando viene richiesto di approvare una richiesta.
        -   Passare a "REDIRECT URIS" e aggiungere l'URI di reindirizzamento seguente all'elenco: `ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-passthrough-authentication"></a>Passaggio 3: installare il Proxy di applicazione di Azure (AAP) con l'autenticazione pass-through
1. Account di accesso al portale di gestione del tenant Azure Active Directory (AAD)
    - Nell'elenco di menu AAD, selezionare "Proxy di applicazione"
    - Fare clic su "Abilita proxy di applicazione" nella parte superiore della schermata
    - Scaricare il "connettore del Proxy dell'applicazione" in una macchina Windows Server appartenenti a un dominio che verrà utilizzato come il Proxy di applicazione Web (WAP).
2. Sul computer WAP, effettuare l'accesso come amministratore e installare il "connettore del Proxy dell'applicazione"
    - Durante l'installazione, assegnare il connettore del proxy dell'applicazione le credenziali per il principio di Azure che si desidera abilitare AAP in
    - Assicurarsi che il computer WAP fa parte di un'istanza locale di Active Directory
3. Tornare al portale di gestione del tenant AAD e aggiungere i proxy di applicazione
   - Andare alla **applicazioni aziendali** scheda
   - Fare clic su **nuova applicazione**
   - Selezionare **On-premises application** e compilare i campi
       - Nome: Qualsiasi nome desiderato
       - URL interno: Si tratta dell'URL interno per il servizio Cloud di individuazione Mopria cui può accedere il computer WAP
       - URL esterno: Configurare nel modo appropriato per l'organizzazione
       - Metodo di preautenticazione: Pass-through

     >   Nota: Se non si trova tutte le impostazioni indicate sopra, aggiungere il proxy con le impostazioni disponibili e quindi selezionare il proxy dell'applicazione appena creata e passare al **proxy di applicazione** scheda e aggiungere tutte le informazioni sopra riportate.

4. Ripetere i 3, descritti in precedenza, per il servizio di stampa Cloud Enterprise e fornire l'URL interno al servizio Enterprise Cloud Print

    >   Nota: La parte https://&lt;servizi-computer-endpoint &gt; /mcs URL indicato di seguito deve essere l'URL esterno è configurare per il servizio Cloud Mopria e/o Enterprise Cloud Print Service.


### <a name="step-4---configure-the-required-mdm-policies"></a>Passaggio 4: configurare i criteri MDM richiesti
- Account di accesso al provider di MDM
- Trovare il gruppo di criteri di Enterprise Cloud Print e configurare i criteri seguendo le linee guida seguenti:
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure ID Active Directory\>
  - CloudPrintOAuthClientId = valore "Application ID" dell'App Web nativa che è stato registrato nel portale di gestione di Azure AD
  - CloudPrinterDiscoveryEndPoint = URL esterno di Mopria Individuazione servizio Proxy di applicazione Azure creato nel passaggio 3.3 (deve essere esattamente lo stesso ma senza il carattere finale /)
  - MopriaDiscoveryResourceId = il "URI ID App" dell'app Web / API per l'endpoint di individuazione registrato nel passaggio 2.8.  È possibile trovarlo in base alle impostazioni -> proprietà dell'app.
  - CloudPrintResourceId = il "URI ID App" dell'app Web / API per l'endpoint di stampa registrato nel passaggio 2.8. È possibile trovarlo in base alle impostazioni -> proprietà dell'app.
  - DiscoveryMaxPrinterLimit = \<a positive integer\>

>   Nota: Se si usa il servizio di Microsoft Intune, è possibile trovare queste impostazioni sotto la categoria "Cloud Printer".

|Nome visualizzato di Intune                     |Condizione                         |
|----------------------------------------|-------------------------------|
|URL di individuazione stampanti                   |CloudPrinterDiscoveryEndpoint  |
|URL dell'autorità di accesso della stampante            |CloudPrintOAuthAuthority       |
|GUID dell'app client nativa di Azure            |CloudPrintOAuthClientId        |
|URI della risorsa del servizio di stampa              |CloudPrintResourceId           |
|Numero massimo di stampanti da query (solo dispositivi mobili)  |DiscoveryMaxPrinterLimit       |
|Risorsa service individuazione URI  |MopriaDiscoveryResourceId      |


>   Nota: Se il gruppo di criteri di Cloud Print non è disponibile, ma il provider MDM supporta le impostazioni OMA-URI, è possibile impostare gli stessi criteri.  Consultare questo <a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">articolo</a> per informazioni aggiuntive.

- URI OMA
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - Value = `https://login.microsoftonline.com/`\<Azure AD Directory ID\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - Valore = \<ID applicazione dell'App nativa di Azure AD\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - Valore = URL esterno di Mopria Individuazione servizio Proxy di applicazione Azure creato nel passaggio 3.3 (deve essere esattamente lo stesso ma senza il carattere finale /)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - Valore = il "URI ID App" dell'app Web / API per l'endpoint di individuazione registrato nel passaggio 2.8.  È possibile trovarlo in base alle impostazioni -> proprietà dell'app.
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - Valore = il "URI ID App" dell'app Web / API per l'endpoint di stampa registrato nel passaggio 2.8. È possibile trovarlo in base alle impostazioni -> proprietà dell'app.
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - Valore = \<un numero intero positivo\>
  
### <a name="step-5---publish-desired-shared-printers"></a>Passaggio 5: pubblicare stampanti condivise desiderate
1. Installare la stampante desiderata nel Server di stampa
2. Condividere la stampante tramite l'interfaccia utente delle proprietà della stampante
3. Selezionare il set di utenti per concedere l'accesso desiderato
4. Salvare le modifiche e chiudere la finestra delle proprietà della stampante
5. Da un computer Windows 10 Fall Creators Update, aprire un prompt dei comandi di Windows PowerShell con privilegi elevati
   1. Eseguire i comandi seguenti
      - `find-module -Name "PublishCloudPrinter"` per confermare che il computer possa raggiungere PowerShell Gallery (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

        >   NOTA: È possibile visualizzare una messaggistica indicante che 'PSGallery' non è un repository non attendibile.  Immettere 'y' per continuare l'installazione.

      - CloudPrinter pubblicare
        - Stampante = il nome della stampante condivisa definita in precedenza
        - Produttore = produttore della stampante
        - Modello = modello di stampante
        - OrgLocation = stringa oggetto JSON che specifica il percorso della stampante, ad esempio:   `{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - SDDL = stringa SDDL che rappresenta le autorizzazioni per la stampante. Ciò può essere ottenuto modificando la scheda di sicurezza di proprietà della stampante in modo appropriato e quindi eseguendo il comando seguente in un prompt dei comandi: `(Get-Printer PrinterName -full).PermissionSDDL`
            Ad esempio "G:DUD:(A;OICI;FA;;;WD)"

          > NOTA: È necessario aggiungere **`O:BA`** come prefisso al risultato del comando prompt dei comandi sopra prima di impostare il valore come impostazione SDDL.  Esempio: SDDL = `O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = https://&lt;services-machine-endpoint&gt;/mcs
        - PrintServerEndpoint = https://&lt;servizi-computer-endpoint&gt;/ecp
        - AzureClientId = ID applicazione del valore Native App Web registrato in precedenza
        - AzureTenantGuid = ID di Directory del tenant di Azure AD
        - DiscoveryResourceId = [facoltativo] ID dell'applicazione del servizio Cloud di individuazione Mopria trasmessa tramite proxy

        > NOTA: È possibile immettere tutti i valori dei parametri obbligatori in anche la riga di comando.<br>
        **CloudPrinter pubblicare** sintassi dei comandi di PowerShell: <br>
        CloudPrinter pubblicare-Printer \<stringa\> -produttore \<stringa\> -Model \<stringa\> - OrgLocation \<stringa\> - Sddl \<stringa\> - DiscoveryEndpoint \<stringa\> - PrintServerEndpoint \<stringa\> - AzureClientId \<stringa\> - AzureTenantGuid \<stringa\> [-DiscoveryResourceId \<stringa\>] <br>
        Comando di esempio: `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifing-the-deployment"></a>In corso di verifica della distribuzione
Su un dispositivo aggiunto AD Azure con i criteri MDM configurati:
- Aprire un web browser e passare a https://&lt;servizi-computer-endpoint &gt; /mcs/services
- Si dovrebbe essere visualizzato il testo JSON che descrive il set di funzionalità di questo endpoint
- Passare a "Impostazioni - sistema operativo\> i dispositivi -\> stampanti e scanner"
    - Dovrebbe essere un collegamento "Ricerca per le stampanti cloud"
    - Fare clic sul collegamento
    - Fare clic sul collegamento "Selezionare un percorso di ricerca"
        - Si dovrebbe visualizzare la gerarchia di percorso dispositivo
    - Selezionare un percorso e fare clic su **OK** e quindi fare clic su **ricerca** pulsante per individuare le stampanti
    - Seleziona stampante e fare clic su **Aggiungi dispositivo** pulsante
    - Dopo l'installazione ha esito positivo della stampante, stampare sulla stampante dalla propria app preferite

>   Nota: Se si usa la stampante "EcpPrintTest", è possibile trovare il file di output nel computer Server di stampa nella "c:\\ECPTestOutput\\EcpTestPrint.xps" posizione.