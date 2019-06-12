---
title: Configurare il client Web di Desktop remoto per gli utenti
description: Viene descritto come un amministratore può configurare il client di web Desktop remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 45164e9eca0873c82148aa3b7baa179a3f626dd7
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804969"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>Configurare il client Web di Desktop remoto per gli utenti

Il client di web Desktop remoto consente agli utenti di accedere a infrastruttura di Desktop remoto dell'organizzazione tramite un web browser compatibile con. Si sarà in grado di interagire con le app remote o desktop come si farebbe con un PC locale ovunque si trovino. Dopo aver impostato il client di web Desktop remoto, tutti gli utenti necessari per iniziare a è l'URL in cui accedono i client, le proprie credenziali e un web browser supportato.

>[!IMPORTANT]
>Il client del web non supporta attualmente Usa il Proxy di applicazione di Azure e non supporta Proxy applicazione Web affatto. Visualizzare [tramite Servizi Desktop remoto con servizi proxy dell'applicazione](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) per informazioni dettagliate.

## <a name="what-youll-need-to-set-up-the-web-client"></a>Che cosa è necessario configurare il client web

Prima di iniziare, tenere presente quanto segue:

* Assicurarsi che il [distribuzione di Desktop remoto](../rds-deploy-infrastructure.md) dispone di un Gateway Desktop remoto, un gestore connessione desktop remoto e accesso Web desktop remoto in esecuzione in Windows Server 2016 o 2019.
* Assicurarsi che la distribuzione è configurata per [licenze CAL per utente](../rds-client-access-license.md) (CAL) anziché per ogni dispositivo, in caso contrario, tutte le licenze verranno utilizzate.
* Installare il [aggiornamento di Windows 10 KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) su Gateway Desktop remoto. In un secondo momento gli aggiornamenti cumulativi potrebbe contiene già questa Knowledge Base.
* Assicurarsi che i certificati pubblici attendibili sono configurati per i ruoli del servizio Gateway Desktop remoto e accesso Web desktop remoto.
* Assicurarsi che tutti gli utenti si connetteranno per i computer siano in esecuzione una delle versioni del sistema operativo seguenti:
  * Windows 10
  * Windows Server 2008 R2 o versione successiva

Gli utenti visualizzeranno migliori prestazioni, ci si connette a Windows Server 2016 (o versione successiva) e Windows 10 (versione 1611 o versione successiva).

>[!IMPORTANT]
>Se si utilizza il client web durante il periodo di anteprima e installata una versione precedente alla 1.0.0, è prima di tutto necessario disinstallare il client precedente prima di passare alla nuova versione. Se si riceve un errore con la dicitura "il client web è stato installato con una versione precedente di RDWebClientManagement e deve essere rimosso prima di distribuire la nuova versione", seguire questa procedura:
>
>1. Aprire un prompt di PowerShell con privilegi elevato.
>2. Eseguire **Uninstall-Module RDWebClientManagement** per disinstallare il nuovo modulo.
>3. Chiudere e riaprire il prompt di PowerShell con privilegi elevati.
>4. Eseguire **RDWebClientManagement Install-Module - RequiredVersion \<vecchia versione > per installare il modulo precedente.**
>5. Eseguire **Disinstalla RDWebClient** per disinstallare il client web precedente.
>6. Eseguire **Uninstall-Module RDWebClientManagement** per disinstallare il modulo precedente.
>7. Chiudere e riaprire il prompt di PowerShell con privilegi elevati.
>8. Procedere come indicato di seguito con i passaggi di installazione normale.

## <a name="how-to-publish-the-remote-desktop-web-client"></a>Come pubblicare il client di web Desktop remoto

Per installare il client web per la prima volta, seguire questa procedura:

1. Nel server Gestore connessione desktop remoto, ottenere il certificato utilizzato per le connessioni Desktop remoto ed esportarlo come file con estensione cer. Copiare il file con estensione cer da Gestore connessione desktop remoto nel server che esegue il ruolo Web desktop remoto.
2. Nel server Accesso Web desktop remoto, aprire un prompt di PowerShell con privilegi elevato.
3. In Windows Server 2016, aggiornare il modulo PowerShellGet, poiché la versione della posta in arrivo non supporta l'installazione del modulo di gestione di client web. Per aggiornare PowerShellGet, eseguire il cmdlet seguente:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >È necessario riavviare PowerShell, l'aggiornamento per rendere effettive, in caso contrario, che il modulo potrebbe non funzionare.

4. Installare il modulo PowerShell di gestione di Desktop remoto web client da PowerShell gallery con questo cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Successivamente, eseguire il cmdlet seguente per scaricare la versione più recente del client web Desktop remoto:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. Successivamente, eseguire questo cmdlet con il valore tra parentesi quadre sostituito con il percorso del file con estensione cer che è stato copiato dal Broker desktop remoto:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Infine, eseguire questo cmdlet per pubblicare il client di web Desktop remoto:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Assicurarsi che il client web presso l'URL di client web è possibile accedere con il nome del server, nel formato <https://server_FQDN/RDWeb/webclient/index.html>. È importante usare il nome del server corrispondente al certificato pubblico di accesso Web desktop remoto nell'URL (in genere il server FQDN).

    >[!NOTE]
    >Quando si esegue la **Publish-RDWebClientPackage** cmdlet, è possibile notare un avviso per segnalare le licenze CAL per dispositivo che non sono supportati, anche se la distribuzione è configurata per le licenze CAL per utente. Se la distribuzione Usa licenze CAL per utente, è possibile ignorare questo avviso. Verrà visualizzata per assicurarsi che si verifica il limite di configurazione.
8. Quando si è pronti per gli utenti accedere al client web, semplicemente inviare l'URL del client web è stato creato.

>[!NOTE]
>Per visualizzare un elenco di tutti i cmdlet supportati per il modulo RDWebClientManagement, eseguire il cmdlet seguente in PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>Come aggiornare il client di web Desktop remoto

Quando è disponibile una nuova versione del client web Desktop remoto, seguire questi passaggi per aggiornare la distribuzione con il nuovo client:

1. Aprire un prompt di PowerShell con privilegi elevati nel server di accesso Web desktop remoto ed eseguire il cmdlet seguente per scaricare la versione più recente del client web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Facoltativamente, è possibile pubblicare il client per i test prima del rilascio ufficiale, eseguire questo cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    Il client deve essere visualizzato nell'URL di test che corrisponde all'URL di client web (ad esempio, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Pubblicare il client per gli utenti, eseguire il cmdlet seguente:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Il client per tutti gli utenti verranno sostituite quando si riavvia la pagina web.

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>Come disinstallare il client di web Desktop remoto

Per rimuovere tutte le tracce del client web, seguire questa procedura:

1. Nel server Accesso Web desktop remoto, aprire un prompt di PowerShell con privilegi elevato.
2. Annullare la pubblicazione il client di Test e produzione, disinstallare tutti i pacchetti locali e rimuovere le impostazioni del client web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Disinstallare il modulo PowerShell di gestione client di web di Desktop remoto:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>Come installare il client di web Desktop remoto senza una connessione internet

Seguire questi passaggi per distribuire il client web a un server Accesso Web desktop remoto che non ha una connessione a internet.

> [!NOTE]
> Installazione senza una connessione internet non è disponibile nella versione 1.0.1 e versioni successive del modulo RDWebClientManagement PowerShell.

> [!NOTE]
> È comunque necessario un amministratore PC con l'accesso a internet per scaricare i file necessari prima di trasferirli nel server offline.

> [!NOTE]
> I PC degli utenti finali richiede una connessione internet per il momento. Questo verrà risolto in una versione futura del client per fornire uno scenario offline completo.

### <a name="from-a-device-with-internet-access"></a>Da un dispositivo con accesso a internet

1. Aprire un prompt di PowerShell.

2. Importa il modulo PowerShell di gestione client di web Desktop remoto da PowerShell gallery:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Scaricare la versione più recente del client web Desktop remoto per l'installazione in un altro dispositivo:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Scaricare la versione più recente del modulo RDWebClientManagement PowerShell:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copiare il contenuto di "C:\WebClient\" al server Accesso Web desktop remoto.

### <a name="from-the-rd-web-access-server"></a>Dal server di accesso Web desktop remoto

Seguire le istruzioni riportate sotto [come pubblicare il client di web Desktop remoto](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), sostituendo i passaggi 4 e 5 con il codice seguente.

4. Importa il modulo PowerShell di gestione client di web Desktop remoto dalla cartella locale:
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Distribuire la versione più recente del client web Desktop remoto dalla cartella locale (sostituire con il file zip appropriato):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>La connessione a desktop remoto senza Gateway Desktop remoto in Windows Server 2019
Questa sezione descrive come abilitare una connessione di client web a un gestore di desktop remoto senza un Gateway Desktop remoto in Windows Server 2019.

### <a name="setting-up-the-rd-broker-server"></a>Configurazione del server Broker di desktop remoto

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>Seguire questa procedura se non è presente un certificato associato al server di desktop remoto

1. Aprire **Server Manager** > **Servizi Desktop remoto**.

2. Nelle **Cenni preliminari sulla distribuzione** selezionare la **attività** menu a discesa.

3. Selezionare **modificare le proprietà di distribuzione**, una nuova finestra intitolata **delle proprietà di distribuzione** verrà aperto.

4. Nel **le proprietà di distribuzione** finestra, seleziona **certificati** nel menu a sinistra.

5. Nell'elenco dei livelli di certificato, selezionare **Gestore connessione desktop remoto - Attiva Single Sign-On**. Sono disponibili due opzioni: (1) creare un nuovo certificato o (2) un certificato esistente.

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>Seguire questa procedura se è presente un certificato precedentemente associato al server di desktop remoto

1. Aprire il certificato associato per il Broker e copiare il **identificazione personale** valore.

2. Per associare il certificato alla porta sicura 3392, aprire una finestra di PowerShell con privilegi elevata ed eseguire il seguente comando, sostituendo **"< identificazione personale >"** con il valore copiato nel passaggio precedente:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Per controllare se il certificato è stato associato correttamente, eseguire il comando seguente:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Nell'elenco di associazioni certificato SSL, assicurarsi che il certificato corretto è associato alla porta 3392.

3. Aprire il Registro di sistema di Windows (regedit) e a nagivate ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e individuare la chiave **WebSocketURI**. Il valore deve essere impostato su <strong>https://+:3392/rdp/</strong>.

### <a name="setting-up-the-rd-session-host"></a>Configurazione di Host sessione Desktop remoto
Il server Host sessione Desktop remoto è diverso dal server Broker di desktop remoto, seguire questa procedura:

1. Creare un certificato per la macchina Host sessione Desktop remoto, aprire e copiare il **identificazione personale** valore.

2. Per associare il certificato alla porta sicura 3392, aprire una finestra di PowerShell con privilegi elevata ed eseguire il seguente comando, sostituendo **"< identificazione personale >"** con il valore copiato nel passaggio precedente:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Per controllare se il certificato è stato associato correttamente, eseguire il comando seguente:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Nell'elenco di associazioni certificato SSL, assicurarsi che il certificato corretto è associato alla porta 3392.

3. Aprire il Registro di sistema di Windows (regedit) e a nagivate ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e individuare la chiave **WebSocketURI**. Il valore deve essere impostato su <https://+:3392/rdp/>.

### <a name="general-observations"></a>Osservazioni generali

* Assicurarsi che i server Host sessione Desktop remoto e di desktop remoto eseguano Windows Server 2019.

* Verificare che tali pubblici attendibili i certificati sono configurati per il server Host sessione Desktop remoto sia il desktop remoto.
    > [!NOTE]
    > Se l'Host sessione Desktop remoto sia sul server di desktop remoto condividono lo stesso computer, impostare solo il certificato del server Broker di desktop remoto. Se il server Host sessione Desktop remoto e Broker di desktop remoto utilizza computer diversi, entrambi deve essere configurati con certificati univoci.

* Il **nome alternativo soggetto (SAN)** di ogni certificato deve essere impostato per la macchina **completamente dominio nome completo (FQDN)** . Il **nome comune (CN)** deve corrispondere la rete SAN per ogni certificato.

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>Come pre-configurare le impostazioni per gli utenti di client web Desktop remoto
In questa sezione indicano come usare PowerShell per configurare le impostazioni per la distribuzione del client web Desktop remoto. Questi cmdlet di PowerShell controllo capacità di un utente di modificare le impostazioni di base a problemi di sicurezza dell'organizzazione o del flusso di lavoro è studiata. Le impostazioni seguenti si trovano nel **impostazioni** pannello laterale del client web. 

### <a name="suppress-telemetry"></a>Eliminare i dati di telemetria
Per impostazione predefinita, gli utenti possono scegliere di abilitare o disabilitare la raccolta di dati di telemetria vengono inviati a Microsoft. Per informazioni sui dati di telemetria raccolti da Microsoft, consultare l'informativa sulla Privacy tramite il collegamento nel **sulle** pannello laterale.

Come amministratore, è possibile scegliere di eliminare la raccolta dati di telemetria per la distribuzione usando il cmdlet di PowerShell seguente:

   ```PowerShell
    Set-RDWebClientDeploymentSetting -SuppressTelemetry $true
   ```

Per impostazione predefinita, l'utente può selezionare per abilitare o disabilitare la telemetria. Valore booleano **$false** corrisponderanno il comportamento di client predefinito. Valore booleano **$true** disabilita i dati di telemetria e impedisce all'utente di abilitazione della telemetria.

### <a name="remote-resource-launch-method"></a>Metodo di avvio risorsa remota
Per impostazione predefinita, gli utenti possono scegliere di avviare le risorse remote (1) nel browser o (2), scaricare un file con estensione rdp da gestire con un altro client installato nel proprio computer. Come amministratore, è possibile scegliere di limitare il metodo di avvio risorsa remota per la distribuzione con il comando Powershell seguente:

   ```PowerShell
    Set-RDWebClientDeploymentSetting -LaunchResourceInBrowser ($true|$false)
   ```
 Per impostazione predefinita, l'utente può selezionare uno dei due metodi di avvio. Valore booleano **$true** imporrà all'utente di avviare le risorse nel browser. Valore booleano **$false** forzerà l'utente per avviare le risorse scaricando un file RDP per la gestione con un client RDP installato localmente.

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>Reimpostare le configurazioni RDWebClientDeploymentSetting impostazioni predefinite
Per reimpostare tutte le impostazioni client a livello di distribuzione web per le configurazioni predefinite, eseguire il cmdlet di PowerShell seguente:

   ```PowerShell
    Reset-RDWebClientDeploymentSetting 
   ```

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se un utente segnala i problemi seguenti quando si apre il client web per la prima volta, le sezioni seguenti indicherà quali operazioni eseguire per correggerli.

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>Cosa fare se il browser dell'utente viene visualizzato un avviso di sicurezza quando si prova ad accedere al client web

Il ruolo Accesso Web desktop remoto non in uso da un certificato attendibile. Assicurarsi che il ruolo Accesso Web desktop remoto è configurato con un certificato attendibile pubblicamente.

Se il problema persiste, il nome del server del Web URL client potrebbero non corrispondere al nome fornito per il certificato di Web desktop remoto. Assicurarsi che l'URL Usa il nome FQDN del server che ospita il ruolo Web desktop remoto.

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>Cosa fare se l'utente non può connettersi a una risorsa con il client web, anche se è possibile visualizzare gli elementi in tutte le risorse

Se l'utente che non possono connettersi con il client web anche se possono vedere le risorse elencate, verificare quanto segue:

* Il ruolo Gateway Desktop remoto è configurato correttamente per usare un certificato pubblico attendibile?
* Il server Gateway Desktop remoto ha gli aggiornamenti richiesti installati? Assicurarsi che il server abbia [KB4025334 aggiornamento](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) installato.

Se l'utente riceve un errore "è stato ricevuto il certificato di autenticazione server imprevisto" del messaggio quando provano a connettersi, quindi il messaggio verrà visualizzato l'identificazione personale del certificato. Cerca il gestore di certificati del server Broker di desktop remoto utilizzando tale identificazione digitale per trovare il certificato appropriato. Verificare che il certificato sia configurato per essere utilizzato per il ruolo del Broker di desktop remoto nella pagina delle proprietà di distribuzione di Desktop remoto. Dopo assicurandosi che il certificato non è ancora scaduto, copiare il certificato nel formato file CER per il server Accesso Web desktop remoto ed eseguire il comando seguente nel server di accesso Web desktop remoto con il valore tra parentesi quadre sostituito dal percorso del file del certificato:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>Diagnosticare i problemi con il log della console

Se è possibile risolvere il problema di base alle istruzioni sulla risoluzione dei problemi in questo articolo, è possibile provare a diagnosticare l'origine del problema osservando la console di accesso nel browser. Il client web fornisce un metodo per registrare l'attività di log di console del browser quando si usa il client web per facilitare la diagnosi di problemi.

* Selezionare i puntini di sospensione nell'angolo superiore destro e passare per il **sulle** pagina nel menu a discesa.
* Sotto **le informazioni di supporto di acquisizione** selezionare la **avviare la registrazione** pulsante.
* Eseguire le operazioni nel client web che ha generato il problema che si sta tentando di diagnosi.
* Passare il **sulle** pagina e selezionare **Arresta registrazione**.
* Il browser scaricherà automaticamente un file con estensione txt intitolato **desktop remoto Console Logs**. Questo file conterrà l'attività di log di console completo generato durante la riproduzione del problema di destinazione.

La console è inoltre accessibile direttamente tramite il browser. La console è in genere si trova sotto gli strumenti di sviluppo. Ad esempio, è possibile accedere il log in Microsoft Edge premendo la **F12** importanti o selezionando i puntini di sospensione, quindi passando alla **più strumenti** > **glistrumentidisviluppo**.

## <a name="get-help-with-the-web-client"></a>Ottieni assistenza per il client web

Se si è verificato un problema che non può essere risolto tramite le informazioni contenute in questo articolo, è possibile [messaggio di posta elettronica](mailto:rdwbclnt@microsoft.com) per segnalarla. È anche possibile richiedere o votare per nuove funzionalità al nostro [finestra di suggerimento](https://aka.ms/rdwebfbk).
