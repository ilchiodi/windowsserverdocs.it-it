---
title: Distribuire Windows Server Hybrid Cloud Print
description: Come impostare la stampa Cloud ibrido Microsoft
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
ms.openlocfilehash: 6e9833489277c84739d489da34d352db8f565663
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881842"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-pre-authentication"></a>Distribuire Hybrid Cloud Print di Windows Server con la preautenticazione

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
- Passaggio 3: Installare Azure Application Proxy (AAP) con Kerberos Constrained Delegation (KCD)
- Passaggio 4: Configurare i criteri MDM richiesti
- Passaggio 5: Pubblicare stampanti condivise

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>Passaggio 1: installare Azure AD Connect per la sincronizzazione tra Azure AD e Active Directory locale
1. Nel computer Windows Server Active Directory, scaricare il software di Azure AD Connect
2. Avviare il pacchetto di installazione di Azure AD Connect e selezionare **Usa impostazioni rapide**
3. Immettere le informazioni richieste nel processo di installazione e fare clic su **installare**

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>Passaggio 2 - pacchetto di installazione Hybrid Cloud stampa nel Server di stampa

1. Installare i moduli di PowerShell stampa di Cloud ibrido
    -  Eseguire i comandi seguenti da un prompt dei comandi di PowerShell con privilegi elevati
        - `find-module -Name "PublishCloudPrinter"` per confermare che il computer possa raggiungere PowerShell Gallery (PSGallery)
        - `install-module -Name "PublishCloudPrinter"`

    > NOTA: È possibile visualizzare una messaggistica indicante che 'PSGallery' non è un repository non attendibile.  Immettere 'y' per continuare l'installazione.

2. Installare la soluzione Cloud ibrida Print
    - Nella stessa con privilegi elevati PowerShell prompt dei comandi, passare alla directory `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - Esegui <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3.  Configurare l'endpoint IIS 2 per il supporto SSL
    -   Il certificato SSL può essere un certificato autofirmato o emesso da alcune autorità di certificazione (CA) attendibile
    -  Se si usa certificato autofirmato, assicurarsi che il certificato viene importato per i computer client
4.  Installare il pacchetto di SQLite
    - Aprire un prompt dei comandi di PowerShell con privilegi elevati
    - Eseguire il comando seguente per scaricare i pacchetti nuget System.Data.SQLite <br>
        `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
    - Eseguire il comando seguente per installare i pacchetti<br>
    `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

    > NOTA: È consigliabile scaricare e installare la versione più recente con l'omissione di "-requiredversion" opzione.

5.  Copiare le DLL di SQLite per la MopriaCloudService Webapp \<bin\> cartella (**c:.\\inetpub\\wwwroot\\MopriaCloudService\\bin**): <br>
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

6.  Aggiorna il `c:\inetpub\wwwroot\MopriaCloudService\web.config` file da includere la versione di SQLite x.x.x. x nel seguente \<runtime\>/\<assemblyBinding\> sezioni:

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
    -   Eseguire l'accesso come amministratore globale al portale di gestione del tenant Azure AD
        1. Passare la scheda "Registrazioni per l'App" per aggiungere endpoint di stampa
            -   Aggiungere l'applicazione, selezionare "Registrazione nuova applicazione"
            -   Specificare un nome appropriato e selezionare "app Web / API"
            -   URL Sign-on = "http://MicrosoftEnterpriseCloudPrint/CloudPrint"
        2.  Ripetere per l'endpoint di individuazione
            -   URL Sign-on = "http://MopriaDiscoveryService/CloudPrint"
        3.  Ripetere per l'applicazione client nativa
            -   Quando si fornisce il nome dell'app, assicurarsi di che selezionare "Applicazione Client nativa" come "tipo di applicazione"
            -   URI di reindirizzamento = "https://\<servizi-computer-endpoint\>/RedirectUrl"
        4.  Passare all'App Client nativa "Impostazioni"
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

### <a name="step-3---install-azure-application-proxy-aap-with-kerberos-constrained-delegation-kcd"></a>Passaggio 3: installare il Proxy di applicazione di Azure (AAP) con Kerberos Constrained Delegation (KCD)
1. Account di accesso al portale di gestione del tenant Azure Active Directory (AAD)
    - Nell'elenco di menu AAD, selezionare "Proxy di applicazione"
    - Fare clic su "Abilita proxy di applicazione" nella parte superiore della schermata
    - Scaricare il "connettore del Proxy dell'applicazione" in una macchina Windows Server appartenenti a un dominio che verrà utilizzato come il Proxy di applicazione Web (WAP).
2. Sul computer WAP, effettuare l'accesso come amministratore e installare il "connettore del Proxy dell'applicazione"
    - Durante l'installazione, assegnare il connettore del proxy dell'applicazione le credenziali per il principio di Azure che si desidera abilitare AAP in
    - Assicurarsi che il computer WAP fa parte di un'istanza locale di Active Directory
3. Nel computer di Active Directory, passare a **strumenti -> utenti e computer**
    - Selezionare la macchina che esegue il connettore del Proxy dell'applicazione
    - Fare doppio clic e selezionare **proprietà -> delega** scheda
    - Selezionare **computer attendibile per la delega solo ai servizi specificati.**
    - Selezionare **utilizza un qualsiasi protocollo di autenticazione.**
    - In **servizi a cui l'account può presentare credenziali delegate**
        - Aggiungere il valore per l'identità SPN del computer che esegue i servizi (service MopriaDiscoveryService e MicrosoftEnterpriseCloudPrint)
            - Per nome SPN, immettere il nome SPN del computer stesso, ad esempio "HOST /\<MachineName\>.\< Dominio\>"<br>
                `HOST/appServer.Contoso.com`
4. Tornare al portale di gestione del tenant AAD e aggiungere i proxy di applicazione
    - Andare alla **applicazioni aziendali** scheda
    - Fare clic su **nuova applicazione**
    - Selezionare **On-premises application** e compilare i campi
        - Nome: Qualsiasi nome desiderato
        - URL interno: Si tratta dell'URL interno per il servizio Cloud di individuazione Mopria cui può accedere il computer WAP
        - URL esterno: Configurare nel modo appropriato per l'organizzazione
        - Metodo di preautenticazione: Azure Active Directory

    >   Nota: Se non si trova tutte le impostazioni indicate sopra, aggiungere il proxy con le impostazioni disponibili e quindi selezionare il proxy dell'applicazione appena creata e passare al **proxy di applicazione** scheda e aggiungere tutte le informazioni sopra riportate.

    - Una volta creata, tornare alla **applicazioni aziendali** -> **tutte le applicazioni**, selezionare la nuova applicazione appena creata
    - Passare a **l'accesso Single sign-on**, assicurarsi che la "modalità Single Sign-on" sia impostata su "Autenticazione integrata di Windows"
    - Impostare "SPN applicazione interna" per il nome SPN specificato nel passaggio 3.3, sopra
    - Verificare che il "identità di accesso delegata" è impostata su "Nome dell'entità utente"

5. Ripetere 4, descritti in precedenza, per il servizio di stampa Cloud Enterprise e fornire l'URL interno al servizio Enterprise Cloud Print
6. Tornare al portale di gestione del tenant di Azure AD e passare a **registrazioni per l'App** e selezionare l'App Client Native -> "Impostazioni"
    - Selezionare **autorizzazioni necessarie**
        - Aggiungere il 2 nuove applicazioni proxy che appena creato
        - Concedere le autorizzazioni delegate per queste 2 applicazioni
        - Dopo aver aggiunto entrambe le applicazioni proxy, fare clic su "Concessione autorizzazioni".  Selezionare "Sì" quando viene richiesto di approvare una richiesta.

7. Abilitare l'autenticazione di Windows in IIS per le macchine virtuali del servizio Cloud Mopria ed Enterprise Cloud Print Service
    - Verificare che la funzionalità di autenticazione di Windows sia installata:
        1. Aprire Server Manager
        2. Fare clic su **gestire**
        3. Fare clic su **Aggiungi ruoli e funzionalità**
        4. Selezionare **installazione basata su ruoli o basata su funzionalità**
        5. Selezionare il Server
        6. In Server Web (IIS) -> Server Web -> sicurezza e selezionare **l'autenticazione di Windows**
        7. Fare clic su Avanti finché non viene completata l'installazione
    - Abilitare l'autenticazione di Windows in IIS:
        1. Open Internet Information Services (IIS) Manager
        2. Per ogni servizio o sito:
            1.  Selezionare il servizio/sito
            2.  Fare doppio clic **autenticazione**
            3.  Fare clic su **Windows autenticazione** e fare clic su **abilitare** sotto **azioni**

### <a name="step-4---configure-the-required-mdm-policies"></a>Passaggio 4: configurare i criteri MDM richiesti
- Account di accesso al provider di MDM
- Trovare il gruppo di criteri di Enterprise Cloud Print e configurare i criteri seguendo le linee guida seguenti:
    - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure ID Active Directory\>
    - CloudPrintOAuthClientId = valore "Application ID" dell'App Web nativa che è stato registrato nel portale di gestione di Azure AD
    - CloudPrinterDiscoveryEndPoint = URL esterno di Mopria Individuazione servizio Proxy di applicazione Azure creato nel passaggio 3.3 (deve essere esattamente lo stesso ma senza il carattere finale /)
    - MopriaDiscoveryResourceId = URL esterno di Mopria Individuazione servizio Proxy di applicazione Azure creato nel passaggio 3.4 (deve essere esattamente allo stesso tra cui la barra finale /)
    - CloudPrintResourceId = URL esterno di Enterprise Cloud Print servizio Proxy di applicazione Azure creato nel passaggio 3.5 (deve essere esattamente allo stesso tra cui la barra finale /)
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
        - Valore = URL esterno di Mopria Individuazione servizio Proxy di applicazione Azure creato nel passaggio 3.4 (deve essere esattamente allo stesso tra cui la barra finale /)
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - Valore = URL esterno di Enterprise Cloud Print servizio Proxy di applicazione Azure creato nel passaggio 3.5 (deve essere esattamente allo stesso tra cui la barra finale /)
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

            - DiscoveryEndpoint = URL esterno di Mopria Individuazione servizio Proxy di applicazione Azure creato nel passaggio 3.4
            - PrintServerEndpoint = URL esterno di Enterprise Cloud Print servizio Proxy di applicazione Azure creato nel passaggio 3.5
            - AzureClientId = ID applicazione del valore Native App Web registrato in precedenza
            - AzureTenantGuid = ID di Directory del tenant di Azure AD
            - DiscoveryResourceId = [facoltativo] ID dell'applicazione del servizio Cloud di individuazione Mopria trasmessa tramite proxy

        > NOTA: È possibile immettere tutti i valori dei parametri obbligatori in anche la riga di comando.<br>
**CloudPrinter pubblicare** sintassi dei comandi di PowerShell: <br>
CloudPrinter pubblicare-Printer \<stringa\> -produttore \<stringa\> -Model \<stringa\> - OrgLocation \<stringa\> - Sddl \<stringa\> - DiscoveryEndpoint \<stringa\> - PrintServerEndpoint \<stringa\> - AzureClientId \<stringa\> - AzureTenantGuid \<stringa\> [-DiscoveryResourceId \<stringa\>] <br>
Comando di esempio: `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifying-the-deployment"></a>Verifica della distribuzione
Su un dispositivo aggiunto AD Azure con i criteri MDM configurati:
- Aprire un web browser e passare a https://&lt;servizi-computer-endpoint &gt; /mcs/services (l'URL esterno per l'endpoint di individuazione)
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
