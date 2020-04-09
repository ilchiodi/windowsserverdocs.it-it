---
title: Distribuire la stampa cloud ibrida di Windows Server
description: Come configurare la stampa cloud ibrida Microsoft
ms.prod: windows-server
ms.technology: windows server 2016
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: c06aafb015b065f307eca02abc7a6adaa8ba763c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852114"
---
# <a name="deploy-windows-server-hybrid-cloud-print"></a>Distribuire la stampa cloud ibrida di Windows Server

>Si applica a: Windows Server 2016

Questo argomento, per gli amministratori IT, descrive la distribuzione end-to-end della soluzione Microsoft Hybrid Cloud Print (HPC). Questo livello di soluzione è costituito dai server Windows esistenti in esecuzione come server di stampa e consente ai dispositivi Azure Active Directory (Azure AD) aggiunti e gestiti da MDM di individuare e stampare le stampanti gestite dall'organizzazione.

## <a name="pre-requisites"></a>Prerequisiti

Prima di iniziare questa installazione, è necessario acquisire una serie di sottoscrizioni, servizi e computer. Le visualizzazioni sono le seguenti:

- Sottoscrizione di Azure AD Premium.

  Per una sottoscrizione di valutazione di Azure, vedere [Introduzione a una sottoscrizione di Azure](https://azure.microsoft.com/trial/get-started-active-directory/) .

- Servizio MDM, ad esempio Intune.

  Vedere [Microsoft Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) per una sottoscrizione di valutazione di Intune.

- Computer Windows Server 2016 o versioni successive che eseguono Active Directory.

  Per informazioni sulla configurazione di Active Directory, vedere [Step-by-Step: Setting up Active Directory in Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/) .

- Un computer Windows Server 2016 o versione successiva dedicato a un dominio che esegue come server di stampa.

- Un computer Windows Server 2016 o versione successiva dedicato a un dominio che esegue come server del connettore.

  Per ulteriori informazioni, vedere informazioni sui [connettori del proxy di applicazione Azure ad](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors) .

- Un computer Windows 10 Fall Creators Update o versioni successive per la pubblicazione delle stampanti.

- Nome di dominio pubblico.

  È possibile usare il nome di dominio creato automaticamente da Azure (*NomeDominio*. onmicrosoft.com) oppure acquistare il proprio nome di dominio. Vedere [aggiungere il nome di dominio personalizzato usando il portale di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain).

## <a name="deployment-steps"></a>Fasi di distribuzione

I passaggi seguenti sono relativi a una tipica distribuzione di stampa cloud ibrida.

### <a name="step-1---install-azure-ad-connect"></a>Passaggio 1: installare Azure AD Connect

1. Azure AD Connect sincronizza Azure AD ad Active Directory locale. Nel computer Windows Server con Active Directory, scaricare e installare il software Azure AD Connect con le impostazioni rapide. Vedere [Introduzione all'uso di Azure ad Connect con le impostazioni rapide](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express).

### <a name="step-2---install-application-proxy"></a>Passaggio 2: installare il proxy di applicazione

1. Il proxy di applicazione consente agli utenti dell'organizzazione di accedere alle applicazioni locali dal cloud. Installare il proxy di applicazione nel server del connettore.
    - Per istruzioni sull'installazione, vedere [esercitazione: aggiungere un'applicazione locale per l'accesso remoto tramite il proxy di applicazione in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).
    - Un gruppo di connettori dedicato è consigliato se l'organizzazione dispone di una topologia di rete complessa. Vedere [pubblicare applicazioni in reti e posizioni separate usando i gruppi di connettori](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connector-groups).

### <a name="step-3---register-and-configure-applications"></a>Passaggio 3: registrare e configurare le applicazioni

Per abilitare la comunicazione autenticata con i servizi di HPC, è necessario creare 3 applicazioni: 2 applicazioni Web per rappresentare i due servizi HPC e 1 applicazione nativa per comunicare con tali servizi.

1. Accedere a portale di Azure per registrare le app Web.
    - In Azure Active Directory passare a **Registrazioni app** > **nuova registrazione**.

    ![Registrazione dell'app AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration.png)

    - Immettere un nome di app per il servizio di individuazione Mopria. Fare clic su **registra** per terminare.

    ![Registrazione dell'app AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria.png)

    - Ripetere il servizio Enterprise Cloud Print.
    - Ripetere l'applicazione nativa.
    - Le tre applicazioni dovrebbero essere visualizzate in **registrazioni app**.

    ![Registrazione dell'app AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-AllApps.png)

2. Esporre l'API per le due applicazioni Web.
    - Sempre nel pannello **registrazioni app** fare clic sull'app del servizio di individuazione Mopria, selezionare **esporre un'API**e quindi fare clic su **imposta** accanto a URI ID applicazione.

    ![AAD esporre l'API 1](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI.png)

    - Fare clic su **Salva** senza modificare il valore predefinito per URI ID applicazione. Questo valore deve essere impostato ora e verrà modificato in un secondo momento.

    ![AAD esporre l'API 2](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-Save.png)

    - Fare clic su **Aggiungi un ambito**.

    ![AAD esporre l'API 3](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-AddScope.png)

    - Specificare un nome ambito, consentire a amministratori e utenti di acconsentire, immettere la descrizione del consenso e quindi fare clic su **Aggiungi ambito** per terminare.

    ![AAD esporre l'API 4](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-ScopeName.png)

    - Ripetere il servizio Enterprise Cloud Print. Usare un nome di ambito e una descrizione di consenso diversi.

    ![AAD esporre l'API 5](../media/hybrid-cloud-print/AAD-AppRegistration-ECP-ExposeAPI-ScopeName.png)

3. Aggiungi autorizzazioni API
    - Tornare al pannello Registrazioni app. Fare clic sull'app nativa e selezionare autorizzazioni API. Fare clic su **Aggiungi un'autorizzazione**.

    ![Autorizzazione dell'API AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission.png)

    - Passa alle **API utilizzate dall'organizzazione**, quindi usa la casella di ricerca per trovare il servizio di individuazione Mopria aggiunto in precedenza. Fare clic sul servizio nei risultati della ricerca.

    ![Autorizzazione dell'API AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria.png)

    - Selezionare **autorizzazioni delegate**. Selezionare la casella accanto all'ambito dell'API. Fare clic su **Aggiungi autorizzazioni**.

    ![Autorizzazione dell'API AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria-Add.png)

    - Ripetere l'aggiunta per aggiungere autorizzazioni al servizio Enterprise Cloud Print.

    ![Autorizzazione dell'API AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-ECP-Add.png)

    - Una volta ripristinato il pannello delle autorizzazioni dell'API, attendere 10 secondi prima di fare clic sul **consenso dell'amministratore Grand...** .

    ![Autorizzazione API AAD 5](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent.png)

    - Quando richiesto, fare clic su **Sì** .

    ![Autorizzazione dell'API AAD 6](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent-Yes.png)

    - Verificare che la colonna stato dell'autorizzazione API sia visualizzata con segni di spunta verdi.

    ![Autorizzazione dell'API AAD 7](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Verify.png)

4. Configurare il proxy di applicazione per le applicazioni Web
    - Passare a **Azure Active Directory** > **applicazioni aziendali** > **tutte le applicazioni**. Cercare il servizio di individuazione Mopria e fare clic su di esso.

    ![Proxy app AAD 1](../media/hybrid-cloud-print/AAD-EnterpriseApp-AllApps.png)

    - Fare clic su **proxy di applicazione**. Immettere l'URL interno usando il formato `https://<fully qualified domain name of the Print Server>/mcs/`. Fare clic su **Salva** per terminare.

    ![Proxy app AAD 2](../media/hybrid-cloud-print/AAD-EnterpriseApp-Mopria-AppProxy.png)

    - Ripetere il servizio Enterprise Cloud Print. Si noti che l'URL interno è `https://<fully qualified domain name of the Print Server>/ecp/`.

    ![Proxy applicazione AAD 3](../media/hybrid-cloud-print/AAD-EnterpriseApp-ECP-AppProxy.png)

    - Passare a **Azure Active Directory** > **registrazioni app**. Fare clic sul servizio di individuazione Mopria. In **Panoramica**si noti che l'URI dell'ID applicazione è stato modificato da quello predefinito all'URL esterno in **proxy applicazione**. L'URI verrà utilizzato durante l'installazione del server di stampa, nei criteri MDM client e per la pubblicazione della stampante.

    ![Proxy app AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-Overview.png)

5. Assegnare gli utenti alle applicazioni
    - Passare a **Azure Active Directory** > **applicazioni aziendali** > **tutte le applicazioni**. Cercare il servizio di individuazione Mopria e fare clic su di esso
    - Fare clic su **utenti e gruppi** e assegnare gli utenti oppure fare clic su **Proprietà** e modificare l' **assegnazione utente obbligatoria?** **No**
    - Ripetere il servizio Enterprise Cloud Print.

6. Configurare l'URI di reindirizzamento nell'app nativa
    - Passare a **Azure Active Directory** > **registrazioni app**. Fare clic sull'app nativa. Passare a **Panoramica** e copiare l' **ID applicazione (client)** .

    ![URI di reindirizzamento AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Overview.png)

    - Passare a **Authentication**. Modificare la casella a discesa **tipo** in `Public...`e immettere due URI di Reindirizzamento usando il formato riportato di seguito, dove `<NativeClientAppID>` si trova nel passaggio precedente:

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/<NativeClientAppID>`

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

    ![URI di reindirizzamento AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Authentication.png)

    - Fare clic su **Salva** per terminare.

### <a name="step-4---setup-the-print-server"></a>Passaggio 4: configurare il server di stampa

1. Verificare che nel server di stampa siano installati tutti i Windows Update disponibili. Nota: è necessario applicare patch al server 2019 per compilare 17763,165 o versione successiva.
    - Installare i ruoli del server seguenti:
        - Ruolo server di stampa
        - Internet Information Services (IIS)
    - Per informazioni dettagliate su come installare i ruoli del server [, vedere installare ruoli, servizi ruolo e funzionalità con l'aggiunta guidata ruoli e funzionalità](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw) .

    ![Ruoli del server di stampa](../media/hybrid-cloud-print/PrintServer-Roles.png)

2. Installare i moduli di PowerShell di stampa cloud ibrido.
    - Eseguire i comandi seguenti da un prompt dei comandi di PowerShell con privilegi elevati:

        `find-module -Name PublishCloudPrinter` per verificare che il computer sia in grado di raggiungere il PowerShell Gallery (PSGallery)

        `install-module -Name PublishCloudPrinter`

    > Nota: è possibile che venga visualizzato un messaggio che informa che "PSGallery" è un repository non attendibile.  Immettere "y" per continuare l'installazione.

    ![Stampante del cloud di pubblicazione del server di stampa](../media/hybrid-cloud-print/PrintServer-PublishCloudPrinter.png)

3. Installare la soluzione di stampa cloud ibrida.
    - Nello stesso prompt dei comandi di PowerShell con privilegi elevati, passare alla directory seguente (virgolette necessarie):

        `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`

    - Esegui

        `.\CloudPrintDeploy.ps1 -AzureTenant <Azure Active Directory domain name> -AzureTenantGuid <Azure Active Directory ID>`

    - Per trovare il nome di dominio Azure Active Directory, vedere la schermata seguente.

    ![Server di stampa come ottenere il nome di dominio AAD](../media/hybrid-cloud-print/PrintServer-GetAADDomainName.png)

    - Per trovare l'ID Azure Active Directory, vedere la schermata seguente.

    ![Distribuzione stampa cloud server di stampa](../media/hybrid-cloud-print/PrintServer-GetAADId.png)

    - L'output dello script CloudPrintDeploy è simile al seguente:

    ![Distribuzione stampa cloud server di stampa](../media/hybrid-cloud-print/PrintServer-CloudPrintDeploy.png)

    - Controllare il file di log per verificare se è presente un errore: `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0\CloudPrintDeploy.log`

4. Eseguire **RegitEdit** in un prompt dei comandi con privilegi elevati. Vai a computer \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\EnterpriseCloudPrintService.
    - Assicurarsi che AzureAudience sia impostato sull'URI ID applicazione dell'app Enterprise Cloud Print.
    - Verificare che AzureTenant sia impostato sul nome di dominio Azure AD.

    ![Chiavi del registro di sistema ECP del server di stampa](../media/hybrid-cloud-print/PrintServer-RegEdit-ECP.png)

5. Vai a computer \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\MopriaDiscoveryService.
    - Assicurarsi che AzureAudience sia l'URI dell'ID applicazione dell'app del servizio Mopria Discovery.
    - Verificare che AzureTenant sia il nome di dominio Azure AD.
    - Verificare che URL sia l'URI dell'ID applicazione dell'app del servizio di individuazione Mopria.

    ![Chiavi del registro di sistema Mopria del server di stampa](../media/hybrid-cloud-print/PrintServer-RegEdit-Mopria.png)

6. Eseguire **iisreset** in un prompt dei comandi di PowerShell elevate. In questo modo, le modifiche apportate al registro di sistema nel passaggio precedente hanno effetto.

7. Configurare gli endpoint IIS per il supporto di SSL.
    - Il certificato SSL può essere un certificato autofirmato o uno emesso da un'autorità di certificazione (CA) attendibile.
    - Se si usa un certificato autofirmato, **assicurarsi che il certificato venga importato nei computer client**.
    - Se si registra il dominio con il provider di terze parti, sarà necessario configurare gli endpoint IIS con il certificato SSL. Per informazioni dettagliate, vedere questa [Guida](https://www.sslsupportdesk.com/microsoft-server-2016-iis-10-10-5-ssl-installation/) .

8. Installare il pacchetto SQLite.
   - Aprire un prompt dei comandi di PowerShell con privilegi elevati.
   - Eseguire il comando seguente per scaricare i pacchetti NuGet di System. Data. SQLite.

        `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`

   - Eseguire il comando seguente per installare i pacchetti.

        `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > Nota: è consigliabile scaricare e installare la versione più recente lasciando l'opzione-requiredversion.

    ![Chiavi del registro di sistema Mopria del server di stampa](../media/hybrid-cloud-print/PrintServer-InstallSQLite.png)

9. Copiare le DLL SQLite nella cartella MopriaCloudService webapp bin (C:\inetpub\wwwroot\MopriaCloudService\bin).
    - Creare un file con estensione ps1 che contiene lo script di PowerShell riportato di seguito.
    - Modificare la variabile $version nella versione SQLite installata nel passaggio precedente.
    - Eseguire il file con estensione ps1 in un prompt dei comandi di PowerShell con privilegi elevati.

    ```powershell
    $source = \Program Files\PackageManagement\NuGet\Packages
    $core = System.Data.SQLite.Core
    $linq = System.Data.SQLite.Linq
    $ef6 = System.Data.SQLite.EF6
    $version = x.x.x.x
    $target = C:\inetpub\wwwroot\MopriaCloudService\bin

    xcopy /y $source\$core.$version\lib\net46\System.Data.SQLite.dll $target\
    xcopy /y $source\$core.$version\build\net46\x86\SQLite.Interop.dll $target\x86\
    xcopy /y $source\$core.$version\build\net46\x64\SQLite.Interop.dll $target\x64\
    xcopy /y $source\$linq.$version\lib\net46\System.Data.SQLite.Linq.dll $target\
    xcopy /y $source\$ef6.$version\lib\net46\System.Data.SQLite.EF6.dll $target\
    ```

10. Aggiornare il file c:\inetpub\wwwroot\MopriaCloudService\web.config in modo da includere la versione SQLite x. x. x.x. nelle sezioni `<runtime>/<assemblyBinding>` seguenti. Si tratta della stessa versione utilizzata nel passaggio precedente.

    ```xml
    ...
    <dependentAssembly>
    assemblyIdentity name=System.Data.SQLite culture=neutral publicKeyToken=db937bc2d44ff139 /
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Core culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.EF6 culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Linq culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    ...
    ```

11. Creare il database SQLite.
    - Scaricare e installare i file binari degli strumenti SQLite da `https://www.sqlite.org/`.
    - Passare alla directory `c:\inetpub\wwwroot\MopriaCloudService\Database`.
    - Eseguire il comando seguente per creare il database in questa directory:

        `sqlite3.exe MopriaDeviceDb.db .read MopriaSQLiteDb.sql`

    - Da Esplora file aprire le proprietà del file MopriaDeviceDb. DB per aggiungere utenti o gruppi autorizzati alla pubblicazione nel database Mopria nella scheda sicurezza. Gli utenti o i gruppi devono esistere in locale Active Directory e sincronizzati con Azure AD.
    - Se la soluzione viene distribuita in un dominio non instradabile (ad esempio, *dominio*. local), il dominio Azure ad (ad esempio *domainname*. onmicrosoft.com o uno acquistato da un fornitore di terze parti) deve essere aggiunto come suffisso UPN al Active Directory locale. In questo modo lo stesso utente che pubblicherà le stampanti, ad esempio admin@*DomainName*. onmicrosoft.com, può essere aggiunto nell'impostazione di sicurezza del file di database. Vedere [preparare un dominio non instradabile per la sincronizzazione della directory](https://docs.microsoft.com/office365/enterprise/prepare-a-non-routable-domain-for-directory-synchronization).

    ![Chiavi del registro di sistema Mopria del server di stampa](../media/hybrid-cloud-print/PrintServer-SQLiteDB.png)

### <a name="step-5-optional---configure-pre-authentication-with-azure-ad"></a>Passaggio 5 \[facoltativo\] configurare la pre-autenticazione con Azure AD

1. Esaminare la [delega vincolata Kerberos del documento per Single Sign-on alle app con il proxy di applicazione](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd).

2. Configurare Active Directory locali.
    - Nel computer Active Directory aprire Server Manager e passare a **strumenti** > **Active Directory utenti e computer**.
    - Passare al nodo **computer** e selezionare il server del connettore.
    - Fare clic con il pulsante destro del mouse e scegliere **proprietà** -> scheda **delega** .
    - Selezionare **computer attendibile per la delega solo ai servizi specificati**.
    - Selezionare **Usa un qualsiasi protocollo di autenticazione**.
    - In **servizi ai quali l'account può presentare credenziali delegate**.
        - Aggiungere il nome dell'entità servizio (SPN) del computer del server di stampa.
        - Selezionare HOST per tipo di servizio.
    ![Active Directory delega](../media/hybrid-cloud-print/AD-Delegation.png)

3. Verificare che l'autenticazione di Windows sia abilitata in IIS.
    - Nel server di stampa aprire Server Manager strumenti di > > Gestione Internet Information Services (IIS).
    - Passare al sito.
    - Fare doppio clic su **autenticazione**.
    - Fare clic su **autenticazione di Windows** e fare clic su **Abilita** in **azioni**.
    ![l'autenticazione IIS del server di stampa](../media/hybrid-cloud-print/PrintServer-IIS-Authentication.png)

4. Configurare l'accesso Single Sign-on.
    - In portale di Azure passare a **Azure Active Directory** > **applicazioni aziendali** > **tutte le applicazioni**.
    - Selezionare App MopriaDiscoveryService.
    - Passare al **proxy di applicazione**. Modificare il metodo di pre-autenticazione per **Azure Active Directory**.
    - Passare a **Single Sign-on**. Selezionare autenticazione integrata di Windows come metodo Single Sign-On.
    - Impostare **SPN dell'applicazione interna** sul nome SPN del computer del server di stampa.
    - Impostare l' **identità di accesso delegato** sul nome dell'entità utente.
    - Ripetere l'app EntperiseCloudPrint.
    ![AAD Single Sign-on di AAD](../media/hybrid-cloud-print/AAD-SingleSignOn-IWA.png)

### <a name="step-6---configure-the-required-mdm-policies"></a>Passaggio 6: configurare i criteri MDM necessari

1. Accedere al provider MDM.
2. Trovare il gruppo di criteri Enterprise Cloud Print e configurare i criteri seguendo le linee guida riportate di seguito:
    - CloudPrintOAuthAuthority = `https://login.microsoftonline.com/<Azure AD Directory ID>`. L'ID directory si trova in Azure Active Directory proprietà >.
    - CloudPrintOAuthClientId = Application \(client\) valore ID dell'app nativa. È possibile trovarlo in Azure Active Directory > Registrazioni app > selezionare l'app nativa > Panoramica.
    - CloudPrinterDiscoveryEndPoint = URL esterno dell'app del servizio di individuazione Mopria. È possibile trovarlo in Azure Active Directory > applicazioni aziendali > selezionare l'app del servizio di individuazione Mopria > proxy di applicazione. **Deve essere esattamente la stessa, ma senza il carattere finale/** .
    - MopriaDiscoveryResourceId = URI ID applicazione dell'app del servizio di individuazione Mopria. È possibile trovarlo in Azure Active Directory > Registrazioni app > selezionare l'app del servizio di individuazione Mopria > Panoramica. **Deve essere esattamente lo stesso con il carattere finale/** .
    - CloudPrintResourceId = URI ID applicazione dell'app Enterprise Cloud Print. È possibile trovarlo in Azure Active Directory > Registrazioni app > selezionare l'app Enterprise Cloud Print > Panoramica. **Deve essere esattamente lo stesso con il carattere finale/** .
    - DiscoveryMaxPrinterLimit = \<un\>Integer positivo.

> Nota: se si usa Microsoft Intune servizio, è possibile trovare queste impostazioni nella categoria stampante cloud.

|Nome visualizzato di Intune                     |Condizione                         |
|----------------------------------------|-------------------------------|
|URL di individuazione stampanti                   |CloudPrinterDiscoveryEndpoint  |
|URL autorità di accesso alla stampante            |CloudPrintOAuthAuthority       |
|GUID dell'app client nativa di Azure            |CloudPrintOAuthClientId        |
|URI della risorsa del servizio di stampa              |CloudPrintResourceId           |
|Numero massimo di stampanti da interrogare (solo per dispositivi mobili)  |DiscoveryMaxPrinterLimit       |
|URI della risorsa del servizio di individuazione stampanti  |MopriaDiscoveryResourceId      |

> Nota: se il gruppo di criteri di stampa cloud non è disponibile, ma il provider MDM supporta le impostazioni URI OMA, è possibile impostare gli stessi criteri.  Per ulteriori informazioni, fare riferimento a [questo](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority) .

    - Valori per URI OMA
        - CloudPrintOAuthAuthority =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority
            - Valore = https://login.microsoftonline.com/<Azure AD Directory ID>
        - CloudPrintOAuthClientId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId
            - Value = < ID applicazione Azure AD app nativa >
        - CloudPrinterDiscoveryEndPoint =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint
            - Valore = URL esterno dell'app del servizio di individuazione Mopria (deve essere esattamente lo stesso, ma senza il carattere finale/)
        - MopriaDiscoveryResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId
            - Value = URI ID applicazione dell'app del servizio di individuazione Mopria
        - CloudPrintResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId
            - Value = URI ID applicazione dell'app Enterprise Cloud Print
        - DiscoveryMaxPrinterLimit =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit
            - Value = valore intero positivo
    
### <a name="step-7---publish-the-shared-printer"></a>Passaggio 7: pubblicare la stampante condivisa

1. Installare la stampante desiderata nel server di stampa.
2. Condividere la stampante tramite l'interfaccia utente delle proprietà della stampante.
3. Selezionare il set di utenti desiderato a cui concedere l'accesso.
4. Salvare le modifiche e chiudere la finestra Proprietà stampante.
5. Preparare un computer con Windows 10 Fall Creators Update o versioni successive. Aggiungere il computer a Azure AD e accedere come utente sincronizzato con Active Directory locale e disporre delle autorizzazioni appropriate per il file MopriaDeviceDb. DB.
6. Dal computer Windows 10 aprire un prompt dei comandi di Windows PowerShell con privilegi elevati.
    - Eseguire i comandi seguenti.
        - `find-module -Name PublishCloudPrinter` per verificare che il computer sia in grado di raggiungere il PowerShell Gallery (PSGallery)
        - `install-module -Name PublishCloudPrinter`

            > Nota: è possibile che venga visualizzato un messaggio che informa che "PSGallery" è un repository non attendibile.  Immettere "y" per continuare l'installazione.

        - `Publish-CloudPrinter`
        - Printer = nome della stampante condivisa. Questo nome deve corrispondere esattamente al nome della condivisione visualizzato nella scheda **condivisione** delle proprietà della stampante, aperta nel server di stampa.

        ![Proprietà stampante server di stampa](../media/hybrid-cloud-print/PrintServer-PrinterProperties.png)

        - Produttore = produttore della stampante.
        - Model = modello di stampante.
        - OrgLocation = stringa JSON che specifica la posizione della stampante, ad esempio

            `{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:Microsoft, depth:1}, {category:site, vs:Redmond, WA, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_number, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}`

        - SDDL = stringa SDDL che rappresenta le autorizzazioni per la stampante.
            - Accedere al server di stampa come amministratore, quindi eseguire il comando di PowerShell seguente sulla stampante che si desidera pubblicare: `(Get-Printer PrinterName -full).PermissionSDDL`.
            - Aggiungere **O:BA** come prefisso al risultato del comando precedente. Ad esempio Se la stringa restituita dal comando precedente è G:DUD: (A; OICI; FA;;; WD), quindi SDDL = O:BAG: DUD: (A; OICI; FA;;; WD).
        - DiscoveryEndpoint = accedere a portale di Azure e quindi ottenere la stringa dalle applicazioni aziendali > Mopria Discovery Service app > proxy di applicazione > URL esterno. Omettere l'oggetto finale/.
        - PrintServerEndpoint = accedere a portale di Azure e quindi ottenere la stringa dalle applicazioni aziendali > app Cloud Print di Enterprise > proxy di applicazione > URL esterno. Omettere l'oggetto finale/.
        - AzureClientId = ID applicazione dell'applicazione nativa registrata.
        - AzureTenantGuid = ID directory del tenant del Azure AD.
        - DiscoveryResourceId = URI ID applicazione dell'applicazione del servizio di individuazione Mopria.

    - È possibile immettere anche tutti i valori dei parametri obbligatori nella riga di comando. La sintassi è:

        `Publish-CloudPrinter -Printer <string> -Manufacturer <string> -Model <string> -OrgLocation <string> -Sddl <string> -DiscoveryEndpoint <string> -PrintServerEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        Comando di esempio:

        `Publish-CloudPrinter -Printer HcpTestPrinter -Manufacturer Manufacturer1 -Model Model1 -OrgLocation '{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:MyCompany, depth:1}, {category:site, vs:MyCity, State, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_name, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}' -Sddl O:BAG:DUD:(A;OICI;FA;;;WD) -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -PrintServerEndpoint https://enterprisecloudprint-contoso.msappproxy.net/ecp -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

    - Usare il comando seguente per verificare che la stampante sia pubblicata.

        `Publish-CloudPrinter -Query -DiscoveryEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        Comando di esempio:

        `Publish-CloudPrinter -Query -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

## <a name="verify-the-deployment"></a>Verificare la distribuzione

In un dispositivo Azure AD Unito in cui sono configurati i criteri MDM:
- Aprire un Web browser e passare a https://mopriadiscoveryservice-*nome-tenant*. msappproxy.NET/MCS/Services.
- Verrà visualizzato il testo JSON che descrive il set di funzionalità di questo endpoint.
- Passare a **impostazioni** > **dispositivi** > **Stampanti & scanner**.
    - Fare clic su **Aggiungi stampante o scanner**.
    - Dovrebbe essere visualizzata una ricerca di stampanti cloud (oppure è possibile cercare stampanti nell'organizzazione in un computer Windows 10 più recente).
    - Fare clic sul collegamento.
    - Fare clic sul collegamento selezionare un percorso di ricerca.
        - Verrà visualizzata la gerarchia del percorso del dispositivo.
    - Selezionare un percorso e fare clic su **OK** , quindi fare clic sul pulsante **Cerca** per trovare le stampanti.
    - Selezionare stampante e fare clic sul pulsante **Aggiungi dispositivo** .
    - Una volta completata l'installazione della stampante, stampare sulla stampante dall'app preferita.

> Nota: se si usa la stampante EcpPrintTest, è possibile trovare il file di output nel computer del server di stampa in C:\\ECPTestOutput\\EcpTestPrint. XPS location.

## <a name="troubleshooting"></a>Risoluzione dei problemi

Di seguito sono riportati i problemi comuni durante la distribuzione di HPC

|Errore |Procedura consigliata |
|------|------|
|Script di PowerShell CloudPrintDeploy non riuscito | <ul><li>Verificare che Windows Server disponga dell'aggiornamento più recente.</li><li>Se si usa Windows Server Update Services (WSUS), vedere [come rendere disponibili le funzionalità su richiesta e i Language Pack quando si usa WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).</li></ul> |
|L'installazione di SQLite non è riuscita. messaggio: è stato rilevato un ciclo di dipendenza per il pacchetto ' System. Data. SQLite ' | Install-Package System. Data. sqlite. Core-ProviderName NuGet-SkipDependencies<br>Install-Package System. Data. sqlite. EF6-ProviderName NuGet-SkipDependencies<br>Install-Package System. Data. sqlite. Linq-providering NuGet-SkipDependencies<br><br>Una volta scaricati correttamente i pacchetti, assicurarsi che siano tutti della stessa versione. In caso contrario, aggiungere il parametro-requiredversion ai comandi precedenti e impostarli in modo che abbiano la stessa versione. |
|Pubblicazione della stampante non riuscita | <ul><li>Per la pre-autenticazione pass-through, assicurarsi che l'utente che pubblica la stampante disponga delle autorizzazioni appropriate per il database di pubblicazione.</li><li>Per Azure AD pre-autenticazione, verificare che l'autenticazione di Windows sia abilitata in IIS. Vedere il passaggio 5,3. Provare prima di tutto la pre-autenticazione pass-through. Se la pre-autenticazione pass-through funziona, è probabile che il problema sia correlato al proxy di applicazione. Vedere [risolvere i problemi del proxy dell'applicazione e i messaggi di errore](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-troubleshoot). Si noti che il passare a passthrough Reimposta l'impostazione Single Sign-On; Ripetere il passaggio 5 per configurare Azure AD pre-autenticazione.</li></ul> |
|I processi di stampa resteranno in stato inviato allo stato della stampante | <ul><li>Verificare che TLS 1,2 sia abilitato nel server del connettore. Vedere l'articolo collegato nel passaggio 2,1.</li><li>Verificare che HTTP2 sia disabilitato nel server del connettore. Vedere l'articolo collegato nel passaggio 2,1.</li></ul> |

Di seguito sono riportate le posizioni dei log che consentono di risolvere i problemi

|Component |Percorso log |
|------|------|
|Client Windows 10 | <ul><li>Usare Visualizzatore eventi per visualizzare il log delle operazioni di Azure AD. Fare clic su **Start** e digitare Visualizzatore eventi. Passare a registri applicazioni e servizi > operazione Microsoft > Windows > AAD >.</li><li>Usare l'hub feedback per raccogliere i log. Vedere [inviare commenti e suggerimenti a Microsoft con l'app hub di feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)</li></ul> |
|Server connettore | Usare Visualizzatore eventi per visualizzare il log del proxy di applicazione. Fare clic su **Start** e digitare Visualizzatore eventi. Passare a registri applicazioni e servizi > Microsoft > AadApplicationProxy > Connector > amministratore. |
|Server di stampa | I log per l'app del servizio di individuazione Mopria e l'app Enterprise Cloud Print sono reperibili in C:\inetpub\logs\LogFiles\W3SVC1. |
