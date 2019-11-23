---
title: Distribuire la stampa cloud ibrida di Windows Server
description: Come configurare la stampa cloud ibrida Microsoft
ms.prod: windows-server
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
ms.openlocfilehash: af5cd5f83633df7e704f4b768baf8dc6d78546aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370436"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-pre-authentication"></a>Distribuire Hybrid Cloud Print di Windows Server con la preautenticazione

>Si applica a: Windows Server 2016

Questo argomento, per gli amministratori IT, descrive la distribuzione end-to-end della soluzione di stampa cloud ibrido Microsoft. Questo livello di soluzione è costituito dai server Windows esistenti in esecuzione come server di stampa e consente ai dispositivi Azure Active Directory Uniti e MDM gestiti di individuare e stampare le stampanti gestite dall'organizzazione.

## <a name="pre-requisites"></a>Prerequisiti

Prima di iniziare questa installazione, è necessario acquisire una serie di sottoscrizioni, servizi e computer. Le visualizzazioni sono le seguenti:

-   Sottoscrizione di Azure AD Premium
    
    Per una sottoscrizione di valutazione di Azure, vedere [Introduzione a una sottoscrizione di Azure](https://azure.microsoft.com/trial/get-started-active-directory/). 

-   Servizio MDM, ad esempio Intune
    
    Vedere [Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune)per una sottoscrizione di valutazione di Intune.

-   Windows Server in esecuzione come Active Directory

    Per informazioni sulla configurazione di Active Directory, vedere [Step-by-Step: Setting up Active Directory in Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/).

-   Windows Server 2016 aggiunto al dominio in esecuzione come server di stampa
    
    Per informazioni su come installare ruoli e servizi ruolo in Windows Server, vedere [installare ruoli, servizi ruolo e funzionalità con l'aggiunta guidata ruoli e funzionalità](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw).

-   Azure AD Connect
    
    Per informazioni sulla configurazione Azure AD Connect, vedere [installazione personalizzata di Azure ad Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom).

-   applicazione Azure connettore proxy in un computer Windows Server aggiunto a un dominio
    
    Per informazioni sulla configurazione di applicazione Azure connettore proxy, vedere [Introduzione al proxy di applicazione e installazione del connettore](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).

-   Nome di dominio pubblico
    
    È possibile usare il nome di dominio creato da Azure o acquistare il proprio nome di dominio.

## <a name="deployment-steps"></a>Fasi di distribuzione

Questa guida descrive cinque (5) passaggi di installazione:

- Passaggio 1: installare Azure AD Connect per la sincronizzazione tra Azure AD e AD locale
- Passaggio 2: installare il pacchetto di stampa cloud ibrido nel server di stampa
- Passaggio 3: installare applicazione Azure proxy (AAP) con la delega vincolata Kerberos (delega vincolata Kerberos)
- Passaggio 4: configurare i criteri MDM necessari
- Passaggio 5: pubblicare stampanti condivise

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>Passaggio 1: installare Azure AD Connect per la sincronizzazione tra Azure AD e AD locale
1. Nel computer Windows Server Active Directory scaricare il software Azure AD Connect
2. Avviare il pacchetto di installazione di Azure AD Connect e selezionare **Usa impostazioni rapide**
3. Immettere le informazioni richieste nel processo di installazione e fare clic su **Installa** .

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>Passaggio 2: installare un pacchetto di stampa cloud ibrido nel server di stampa

1. Installare i moduli di PowerShell per la stampa cloud ibrida
   - Eseguire i comandi seguenti da un prompt dei comandi di PowerShell con privilegi elevati
      - `find-module -Name "PublishCloudPrinter"` per verificare che il computer sia in grado di raggiungere il PowerShell Gallery (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

     > Nota: è possibile che venga visualizzato un messaggio che informa che "PSGallery" è un repository non attendibile.  Immettere "y" per continuare l'installazione.

2. Installare la soluzione di stampa cloud ibrida
    - Nello stesso prompt dei comandi di PowerShell con privilegi elevati, passare alla directory `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - Esegui <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. Configurare 2 endpoint IIS per il supporto di SSL
   -   Il certificato SSL può essere un certificato autofirmato o uno emesso da un'autorità di certificazione attendibile (CA)
   -  Se si usa un certificato autofirmato, assicurarsi che il certificato venga importato nei computer client
4. Installare il pacchetto SQLite
   - Aprire un prompt dei comandi di PowerShell con privilegi elevati
   - Eseguire il comando seguente per scaricare i pacchetti NuGet di System. Data. SQLite <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - Eseguire il comando seguente per installare i pacchetti<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > Nota: è consigliabile scaricare e installare la versione più recente lasciando l'opzione "-requiredversion".

5. Copiare le DLL SQLite nella cartella MopriaCloudService webapp \<bin\> (**C:\\inetpub\\wwwroot\\MopriaCloudService\\bin**): <br>
   - I file binari SQLite devono trovarsi in "\\programmi\\PackageManagement\\pacchetti NuGet\\"

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

   > Nota: x. x.x. è la versione di SQLite installata in precedenza.

6. Aggiornare il file di `c:\inetpub\wwwroot\MopriaCloudService\web.config` in modo da includere la versione SQLite x. x.x. nel \<\>di runtime seguente /\<le sezioni di\> di associazione:

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
    -  Scaricare e installare i file binari degli strumenti SQLite da <https://www.sqlite.org/>
    -  Passare a **c:\\inetpub\\wwwroot\\MopriaCloudService\\database** directory
    -  Eseguire il comando seguente per creare il database in questa directory: `sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  Da Esplora file, aprire le proprietà del file MopriaDeviceDb. DB per aggiungere utenti/gruppi autorizzati alla pubblicazione nel database Mopria nella scheda sicurezza
        - Consigliare solo l'aggiunta del gruppo di utenti amministratore richiesto.
8. Registrare l'app Web 2 con Azure AD per supportare l'autenticazione OAuth2
   - Accedere come amministratore globale al portale di gestione tenant di Azure AD
     1. Vai alla scheda "Registrazioni app" per aggiungere un endpoint di stampa
        - Aggiungi applicazione, selezionare "registrazione nuova applicazione"
        - Specificare un nome appropriato e selezionare "app Web/API"
        - URL di accesso = "<http://MicrosoftEnterpriseCloudPrint/CloudPrint>"
     2. Ripeti per l'endpoint di individuazione
        - URL di accesso = "<http://MopriaDiscoveryService/CloudPrint>"
     3. Ripeti per l'applicazione client nativa
        -   Quando si specifica il nome dell'app, assicurarsi di selezionare "applicazione client nativa" come "tipo di applicazione".
        -   URI di reindirizzamento = "https://\<Services-Machine-endpoint\>/RedirectUrl"
     4. Passare all'app client nativa "Settings" (impostazioni)
        -   Copiare il valore "ID applicazione" da usare per i passaggi di installazione successivi
        -   Selezionare "autorizzazioni necessarie"
            1.  Fare clic su "Aggiungi"
            2.  Fare clic su "selezionare un'API"
            3.  Cercare l'endpoint di stampa e l'endpoint di individuazione in base al nome definito durante la creazione dell'endpoint dell'app
            4.  Aggiungere i 2 endpoint
            5.  Verificare che l'opzione "autorizzazioni delegate" per ogni endpoint dell'app sia abilitata
            6.  Assicurarsi di fare clic sul pulsante "fine" nella parte inferiore
            7.  Fare clic su "Concedi autorizzazioni", dopo l'aggiunta di entrambi gli endpoint.  Selezionare "Sì" quando viene richiesto di approvare la richiesta.
        -   Passare a "URI di reindirizzamento" e aggiungere gli URI di reindirizzamento seguenti all'elenco: `ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-kerberos-constrained-delegation-kcd"></a>Passaggio 3: installare applicazione Azure proxy (AAP) con la delega vincolata Kerberos (delega vincolata Kerberos)
1. Accedere al portale di gestione tenant di Azure AD (AAD)
    - Nell'elenco dei menu di AAD selezionare "proxy applicazione"
    - Fare clic su "Abilita proxy applicazione" nella parte superiore della schermata
    - Scaricare il "connettore del proxy di applicazione" in un computer Windows Server aggiunto a un dominio che fungerà da proxy applicazione Web (WAP).
2. Nel computer WAP accedere come amministratore e installare il connettore del proxy di applicazione
    - Durante l'installazione, concedere al connettore del proxy di applicazione le credenziali per il principio di Azure in cui si vuole abilitare AAP
    - Verificare che il computer WAP sia aggiunto al dominio locale Active Directory
3. Nel computer Active Directory passare a **strumenti-> utenti e computer**
    - Selezionare il computer che esegue il connettore del proxy di applicazione
    - Fare clic con il pulsante destro del mouse e scegliere **Proprietà-> scheda delega**
    - Selezionare **computer attendibile per la delega solo ai servizi specificati.**
    - Selezionare **Usa un qualsiasi protocollo di autenticazione.**
    - In **servizi ai quali l'account può presentare credenziali delegate**
        - Aggiungere il valore per l'identità SPN del computer che esegue i servizi (MopriaDiscoveryService e servizio MicrosoftEnterpriseCloudPrint)
            - Per nome SPN, immettere il nome SPN del computer stesso, ad esempio "HOST/\<MachineName\>.\<\>di dominio "<br>
                `HOST/appServer.Contoso.com`
4. Tornare al portale di gestione dei tenant di AAD e aggiungere i proxy dell'applicazione
   - Passare alla scheda **applicazioni aziendali**
   - Fare clic su **nuova applicazione**
   - Selezionare l' **applicazione locale** e compilare i campi
       - Nome: qualsiasi nome desiderato
       - URL interno: URL interno del servizio cloud di individuazione Mopria a cui il computer WAP può accedere
       - URL esterno: configurare in base alle esigenze dell'organizzazione
       - Metodo di preautenticazione: Azure Active Directory

     >   Nota: se non si trovano tutte le impostazioni precedenti, aggiungere il proxy con le impostazioni disponibili e quindi selezionare il proxy di applicazione appena creato e passare alla scheda **proxy applicazione** e aggiungere tutte le informazioni sopra riportate.

   - Al termine della creazione, tornare ad **applicazioni aziendali** -> **tutte le applicazioni**, selezionare la nuova applicazione appena creata
   - Passare a **Single Sign-on**, assicurarsi che la "modalità Single Sign-on" sia impostata su "autenticazione integrata di Windows".
   - Impostare "SPN applicazione interna" sul nome SPN specificato nel passaggio 3,3, sopra
   - Verificare che l'identità di accesso delegata sia impostata su "nome entità utente"

5. Ripetere 4, sopra, per il servizio Enterprise Cloud Print e fornire l'URL interno al servizio Enterprise Cloud Print
6. Tornare al portale di gestione tenant di Azure AD e passare a **registrazioni app** e selezionare l'app Client nativa > "Impostazioni".
    - Selezionare le **autorizzazioni necessarie**
        - Aggiungere le 2 nuove applicazioni proxy appena create
        - Concedere autorizzazioni delegate per queste 2 applicazioni
        - Una volta aggiunte entrambe le applicazioni proxy, fare clic su "Concedi autorizzazioni".  Selezionare "Sì" quando viene richiesto di approvare la richiesta.

7. Abilitare l'autenticazione di Windows in IIS per il servizio cloud Mopria e i computer del servizio Enterprise Cloud Print
    - Verificare che sia installata la funzionalità autenticazione di Windows:
        1. Aprire Server Manager
        2. Fare clic su **Gestisci**
        3. Fare clic su **Aggiungi ruoli e funzionalità**
        4. Selezionare l' **installazione basata su ruoli o basata su funzionalità**
        5. Selezionare il server
        6. In server Web (IIS)-> server Web-> sicurezza, selezionare **autenticazione di Windows** .
        7. Fare clic su Avanti fino a completare l'installazione
    - Abilitare l'autenticazione di Windows in IIS:
        1. Apri Gestione Internet Information Services (IIS)
        2. Per ogni servizio/sito:
            1.  Selezionare il servizio/sito
            2.  Fare doppio clic su **autenticazione**
            3.  Fare clic su **autenticazione di Windows** e fare clic su **Abilita** in **azioni**

### <a name="step-4---configure-the-required-mdm-policies"></a>Passaggio 4: configurare i criteri MDM necessari
- Accedere al provider MDM
- Trovare il gruppo di criteri Enterprise Cloud Print e configurare i criteri seguendo le linee guida riportate di seguito:
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure ID directory AD\>
  - CloudPrintOAuthClientId = valore "ID applicazione" dell'app Web nativa registrata nel portale di gestione di Azure AD
  - CloudPrinterDiscoveryEndPoint = URL esterno del servizio di individuazione Mopria applicazione Azure proxy creato nel passaggio 3,3 (deve essere esattamente lo stesso ma senza il carattere finale/)
  - MopriaDiscoveryResourceId = URL esterno del servizio di individuazione Mopria applicazione Azure proxy creato nel passaggio 3,4 (deve essere esattamente lo stesso, incluso il carattere finale/)
  - CloudPrintResourceId = URL esterno del servizio Enterprise Cloud Print applicazione Azure proxy creato nel passaggio 3,5 (deve essere esattamente lo stesso, incluso il percorso/)
  - DiscoveryMaxPrinterLimit = \<un numero intero positivo\>

>   Nota: se si usa Microsoft Intune Service, è possibile trovare queste impostazioni nella categoria "Cloud Printer".

|Nome visualizzato di Intune                     |Condizione                         |
|----------------------------------------|-------------------------------|
|URL di individuazione stampanti                   |CloudPrinterDiscoveryEndpoint  |
|URL autorità di accesso alla stampante            |CloudPrintOAuthAuthority       |
|GUID dell'app client nativa di Azure            |CloudPrintOAuthClientId        |
|URI della risorsa del servizio di stampa              |CloudPrintResourceId           |
|Numero massimo di stampanti da interrogare (solo per dispositivi mobili)  |DiscoveryMaxPrinterLimit       |
|URI della risorsa del servizio di individuazione stampanti  |MopriaDiscoveryResourceId      |

>   Nota: se il gruppo di criteri di stampa cloud non è disponibile, ma il provider MDM supporta le impostazioni URI OMA, è possibile impostare gli stessi criteri.  Per ulteriori informazioni, fare riferimento a questo <a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">articolo</a> .

- URI OMA
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - Valore = `https://login.microsoftonline.com/`\<Azure AD ID directory\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - Value = \<ID applicazione Azure AD app nativa\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - Valore = URL esterno del servizio di individuazione Mopria applicazione Azure proxy creato nel passaggio 3,3 (deve essere esattamente lo stesso, ma senza il carattere finale/)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - Value = URL esterno del servizio di individuazione Mopria applicazione Azure proxy creato nel passaggio 3,4 (deve essere esattamente lo stesso, incluso il carattere finale/)
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - Value = URL esterno del servizio Enterprise Cloud Print applicazione Azure proxy creato nel passaggio 3,5 (deve essere esattamente lo stesso, incluso il carattere finale/)
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - Value = \<un numero intero positivo\>

### <a name="step-5---publish-desired-shared-printers"></a>Passaggio 5: pubblicare le stampanti condivise desiderate
1. Installare la stampante desiderata nel server di stampa
2. Condividere la stampante tramite l'interfaccia utente delle proprietà della stampante
3. Selezionare il set di utenti desiderato a cui concedere l'accesso
4. Salvare le modifiche e chiudere la finestra delle proprietà della stampante
5. Da un computer di aggiornamento di Windows 10 Fall Creator, aprire un prompt dei comandi di Windows PowerShell con privilegi elevati
   1. Esegui questi comandi
      - `find-module -Name "PublishCloudPrinter"` per verificare che il computer sia in grado di raggiungere il PowerShell Gallery (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

        >   Nota: è possibile che venga visualizzato un messaggio che informa che "PSGallery" è un repository non attendibile.  Immettere "y" per continuare l'installazione.

      - Publish-CloudPrinter
        - Printer = nome della stampante condivisa definito
        - Produttore = produttore della stampante
        - Model = modello di stampante
        - OrgLocation = stringa JSON che specifica la posizione della stampante, ad esempio: `{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - SDDL = stringa SDDL che rappresenta le autorizzazioni per la stampante. Questa operazione può essere ottenuta modificando la scheda sicurezza Proprietà stampante in modo appropriato e quindi eseguendo il comando seguente in un prompt dei comandi: `(Get-Printer PrinterName -full).PermissionSDDL`
            ad esempio "G:DUD: (A; OICI; FA;;; WD) "

          > Nota: è necessario aggiungere **`O:BA`** come prefisso al risultato del comando del prompt dei comandi precedente, prima di impostare il valore come impostazione SDDL.  Esempio: SDDL = `O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = URL esterno del servizio di individuazione Mopria applicazione Azure proxy creato nel passaggio 3,4
        - PrintServerEndpoint = URL esterno del servizio Enterprise Cloud Print applicazione Azure proxy creato nel passaggio 3,5
        - AzureClientId = ID applicazione del valore dell'app Web nativa registrato precedente
        - AzureTenantGuid = ID directory del tenant di Azure AD
        - DiscoveryResourceId = [optional] ID applicazione del servizio cloud di individuazione Mopria proxy

        > Nota: è possibile immettere anche tutti i valori dei parametri obbligatori nella riga di comando.<br>
        **Publish-CloudPrinter** Sintassi del comando di PowerShell: <br>
        Publish-CloudPrinter-Printer \<String\>-Manufacturer \<String\>-Model \<String\>-OrgLocation \<String\>-SDDL \<String\>-DiscoveryEndpoint \<String\>-PrintServerEndpoint \<String\>-AzureClientId \<String\>-AzureTenantGuid \<String\> [-DiscoveryResourceId \<String\>] <br>
        Comando di esempio: `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifying-the-deployment"></a>Verifica della distribuzione
In un dispositivo Azure AD Unito in cui sono configurati i criteri MDM:
- Aprire un Web browser e passare a https://&lt;Services-Machine-endpoint&gt;/MCS/Services (URL esterno per l'endpoint di individuazione)
- Verrà visualizzato il testo JSON che descrive il set di funzionalità di questo endpoint
- Passare a "Impostazioni sistema operativo-dispositivi\>-\> stampanti & scanner"
    - Verrà visualizzato il collegamento "Cerca stampanti cloud"
    - Fare clic sul collegamento
    - Fare clic sul collegamento "selezionare un percorso di ricerca"
        - Verrà visualizzata la gerarchia del percorso del dispositivo
    - Selezionare un percorso e fare clic su **OK** , quindi fare clic sul pulsante **Cerca** per trovare le stampanti
    - Selezionare stampante e fare clic sul pulsante **Aggiungi dispositivo**
    - Una volta completata l'installazione della stampante, stamparla sulla stampante dall'app preferita

>   Nota: se si usa la stampante "EcpPrintTest", è possibile trovare il file di output nel computer del server di stampa nel percorso "C:\\ECPTestOutput\\EcpTestPrint. XPS".
