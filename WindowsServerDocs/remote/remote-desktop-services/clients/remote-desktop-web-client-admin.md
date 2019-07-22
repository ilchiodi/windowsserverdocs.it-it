---
title: Configurare il client Web Desktop remoto per gli utenti
description: Descrive come un amministratore può configurare il client Web Desktop remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 02c7098c8e3f93ce315e7d9a881613a03924e78b
ms.sourcegitcommit: 286e3181ebd2cb9d7dc7fe651858a4e0d61d153f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2019
ms.locfileid: "68300692"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>Configurare il client Web Desktop remoto per gli utenti

Il client Web Desktop remoto consente agli utenti di accedere all'infrastruttura di Desktop remoto della tua organizzazione tramite un Web browser compatibile. Saranno quindi in grado di interagire con app o desktop remoti come quando usano un PC locale, ovunque si trovano. Dopo che avrai configurato il client Web Desktop remoto, per iniziare gli utenti avranno bisogno solo dell'URL con cui possono accedere al client, le loro credenziali e un Web browser supportato.

>[!IMPORTANT]
>Il client Web attualmente non supporta l'uso di Azure Application Proxy né il Proxy applicazione Web. Per i dettagli, vedi [Uso di Servizi Desktop remoto con servizi proxy dell'applicazione](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services).

## <a name="what-youll-need-to-set-up-the-web-client"></a>Condizioni necessarie per configurare il client Web

Prima di iniziare, tieni presente quanto segue:

* Assicurati che la [distribuzione Desktop remoto](../rds-deploy-infrastructure.md) abbia un Gateway Desktop remoto, un Gestore connessione Desktop remoto e un server Accesso Web Desktop remoto in esecuzione in Windows Server 2016 o 2019.
* Assicurati che la distribuzione sia configurata per le [licenze CAL Per Utente](../rds-client-access-license.md) anziché Per Dispositivo, altrimenti verranno usate tutte le licenze.
* Installa l'[aggiornamento KB4025334 di Windows 10](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) nel Gateway Desktop remoto. Gli aggiornamenti cumulativi successivi potrebbero già includere tale aggiornamento KB.
* Assicurati che i certificati attendibili pubblici siano configurati per i ruoli Gateway Desktop remoto e Accesso Web Desktop remoto.
* Assicurati che i computer a cui si connetteranno gli utenti eseguano una delle versioni di sistema operativo seguenti:
  * Windows 10
  * Windows Server 2008R2 o versioni successive

Gli utenti riscontreranno prestazioni migliori se si connettono a Windows Server 2016 (o versioni successive) e a Windows 10 (versione 1611 o versioni successive).

>[!IMPORTANT]
>Se hai usato il client Web durante il periodo di anteprima e installato una versione precedente la 1.0.0, devi disinstallare il client precedente prima di passare alla nuova versione. Se viene visualizzato un errore per segnalare che il client Web è stato installato usando una versione meno recente di RDWebClientManagement e che deve essere rimosso prima di distribuire la nuova versione, segui questa procedura:
>
>1. Apri un prompt di PowerShell con privilegi elevati.
>2. Esegui **Uninstall-Module RDWebClientManagement** per disinstallare il nuovo modulo.
>3. Chiudi e riapri il prompt di PowerShell con privilegi elevati.
>4. Esegui **Install-Module RDWebClientManagement -RequiredVersion \<versione precedente> per installare il modulo precedente.**
>5. Esegui **Uninstall-RDWebClient** per disinstallare il client Web precedente.
>6. Esegui **Uninstall-Module RDWebClientManagement** per disinstallare il modulo precedente.
>7. Chiudi e riapri il prompt di PowerShell con privilegi elevati.
>8. Prosegui con la normale procedura di installazione, come descritto di seguito.

## <a name="how-to-publish-the-remote-desktop-web-client"></a>Come pubblicare il client Web Desktop remoto

Per installare il client Web per la prima volta, segui questa procedura:

1. Nel server Gestore connessione Desktop remoto ottieni il certificato usato per le connessioni Desktop remoto ed esportalo come file con estensione cer. Copia tale file dal Gestore connessione Desktop remoto al server che esegue il ruolo Web Desktop remoto.
2. Nel server Accesso Web Desktop remoto apri un prompt di PowerShell con privilegi elevati.
3. In Windows Server 2016 aggiorna il modulo PowerShellGet perché la versione fornita non supporta l'installazione del modulo di gestione del client Web. Per aggiornare PowerShellGet, esegui il cmdlet seguente:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Dovrai riavviare PowerShell affinché l'aggiornamento diventi effettivo, altrimenti il modulo potrebbe non funzionare.

4. Installa il modulo PowerShell di gestione del client Web Desktop remoto da PowerShell Gallery con questo cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. A questo punto esegui il cmdlet seguente per scaricare la versione più recente del client Web Desktop remoto:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. Esegui quindi questo cmdlet con il valore tra parentesi angolari sostituito dal percorso del file con estensione cer che hai copiato dal Gestore Desktop remoto:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Esegui infine questo cmdlet per pubblicare il client Web Desktop remoto:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Assicurati di poter accedere al client Web nel relativo URL con il nome server formattato come <https://server_FQDN/RDWeb/webclient/index.html>. È importante usare il nome server corrispondente al certificato pubblico di Accesso Web Desktop remoto nell'URL (in genere l'FQDN del server).

    >[!NOTE]
    >Durante l'esecuzione del cmdlet **Publish-RDWebClientPackage**, può essere visualizzato un avviso per indicare che le licenze CAL Per Dispositivo non sono supportate, anche se la distribuzione è configurata per l'uso di licenze CAL Per Utente. Se la distribuzione usa licenze CAL Per Utente, puoi ignorare questo avviso. Viene visualizzato per segnalarti la limitazione di configurazione.
8. Quando sei pronto per consentire agli utenti di accedere al client Web, ti è sufficiente inviare loro l'URL del client Web che hai creato.

>[!NOTE]
>Per visualizzare un elenco di tutti i cmdlet supportati per il modulo RDWebClientManagement, esegui il cmdlet seguente in PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>Come aggiornare il client Web Desktop remoto

Quando è disponibile una nuova versione del client Web Desktop remoto, segui questa procedura per aggiornare la distribuzione con il nuovo client:

1. Apri un prompt di PowerShell con privilegi elevati nel server Accesso Web Desktop remoto ed esegui il cmdlet seguente per scaricare l'ultima versione disponibile del client Web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Facoltativamente, puoi pubblicare il client per testarlo prima che venga rilasciata la versione ufficiale eseguendo questo cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    Il client deve risultare nell'URL di prova corrispondente al tuo URL del client Web (ad esempio, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Pubblica il client per gli utenti eseguendo il cmdlet seguente:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    In questo modo il client verrà sostituito per tutti gli utenti quando questi riavviano la pagina Web.

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>Come disinstallare il client Web Desktop remoto

Per rimuovere tutte le tracce del client Web, segui questa procedura:

1. Nel server Accesso Web Desktop remoto apri un prompt di PowerShell con privilegi elevati.
2. Annulla la pubblicazione dei client di prova e di produzione, disinstalla tutti i pacchetti locali e rimuovi le impostazioni del client Web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Disinstalla il modulo PowerShell di gestione del client Web Desktop remoto:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>Come installare il client Web Desktop remoto senza una connessione Internet

Segui questa procedura per distribuire il client Web a un server Accesso Web Desktop remoto che non dispone di una connessione Internet.

> [!NOTE]
> L'installazione senza una connessione Internet è disponibile nella versione 1.0.1 e nelle versioni successive del modulo di PowerShell RDWebClientManagement.

> [!NOTE]
> Devi comunque disporre di un PC di amministrazione con accesso a Internet per scaricare i file necessari prima di trasferirli al server offline.

> [!NOTE]
> Per il momento, il PC dell'utente finale necessita di una connessione Internet. Questo problema verrà risolto in una versione futura del client per offrire uno scenario offline completo.

### <a name="from-a-device-with-internet-access"></a>Da un dispositivo con accesso a Internet

1. Apri un prompt di PowerShell.

2. Importa il modulo PowerShell di gestione del client Web Desktop remoto da PowerShell Gallery:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Scarica la versione più recente del client Web Desktop remoto per installarlo in un dispositivo diverso:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Scarica la versione più recente del modulo di PowerShell RDWebClientManagement:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copia il contenuto di "C:\WebClient\" nel server Accesso Web Desktop remoto.

### <a name="from-the-rd-web-access-server"></a>Dal server Accesso Web Desktop remoto

Segui le istruzioni contenute in [Come pubblicare il client Web Desktop remoto](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), sostituendo i passaggi 4 e 5 con i seguenti.

4. Importa il modulo PowerShell di gestione del client Web Desktop remoto dalla cartella locale:
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Distribuisci la versione più recente del client Web Desktop remoto dalla cartella locale (sostituisci con il file con estensione zip appropriato):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>Connessione al Gestore Desktop remoto senza un Gateway Desktop remoto in Windows Server 2019
Questa sezione illustra come abilitare una connessione client Web a un Gestore Desktop remoto senza un Gateway Desktop remoto in Windows Server 2019.

### <a name="setting-up-the-rd-broker-server"></a>Configurazione del server Gestore Desktop remoto

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>Segui questa procedura se non sono presenti certificati associati al server Gestore Desktop remoto

1. Apri **Server Manager** > **Servizi Desktop remoto**.

2. Nella sezione **Panoramica della distribuzione** seleziona il menu a discesa **Attività**.

3. Scegli **Edit Deployment Properties** (Modifica proprietà distribuzione). Verrà visualizzata una nuova finestra denominata **Proprietà distribuzione**.

4. Nella finestra **Proprietà distribuzione** scegli **Certificati** dal menu a sinistra.

5. Dall'elenco dei livelli di certificato seleziona **Gestore connessione Desktop remoto - Abilita Single Sign-On**. Hai a disposizione due opzioni: (1) creare un nuovo certificato o (2) usare un certificato esistente.

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>Segui questa procedura se è presente un certificato precedentemente associato al server Gestore Desktop remoto

1. Apri il certificato associato al Gestore e copia il valore **Thumbprint**.

2. Per associare questo certificato alla porta sicura 3392, apri una finestra di PowerShell con privilegi elevati ed esegui il comando seguente, sostituendo **"< thumbprint >"** con il valore copiato dal passaggio precedente:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Per verificare se il certificato è stato associato correttamente, esegui il comando seguente:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Nell'elenco delle associazioni ai certificati SSL, assicurati che il certificato corretto sia associato alla porta 3392.

3. Apri il Registro di sistema di Windows (regedit), passa a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e individua la chiave **WebSocketURI**. Il valore deve essere impostato su <strong>https://+:3392/rdp/</strong>.

### <a name="setting-up-the-rd-session-host"></a>Configurazione dell'Host sessione Desktop remoto
Segui questa procedura se il server Host sessione Desktop remoto è diverso dal server Gestore Desktop remoto:

1. Crea un certificato per il computer Host sessione Desktop remoto, aprilo e copia il valore **Identificazione personale**.

2. Per associare questo certificato alla porta sicura 3392, apri una finestra di PowerShell con privilegi elevati ed esegui il comando seguente, sostituendo **"< thumbprint >"** con il valore copiato dal passaggio precedente:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Per verificare se il certificato è stato associato correttamente, esegui il comando seguente:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Nell'elenco delle associazioni ai certificati SSL, assicurati che il certificato corretto sia associato alla porta 3392.

3. Apri il Registro di sistema di Windows (regedit), passa a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e individua la chiave **WebSocketURI**. Il valore deve essere impostato su <https://+:3392/rdp/>.

### <a name="general-observations"></a>Osservazioni generali

* Assicurati che sia l'Host sessione Desktop remoto sia il server Gestore Desktop remoto eseguano Windows Server 2019.

* Assicurati che i certificati attendibili pubblici siano configurati sia per l'Host sessione Desktop remoto che per il server Gestore Desktop remoto.
    > [!NOTE]
    > Se l'Host sessione Desktop remoto e il server Gestore Desktop remoto condividono lo stesso computer, imposta solo il certificato del server Gestore Desktop remoto. Se l'Host sessione Desktop remoto e il server Gestore Desktop remoto usano computer diversi, devono essere configurati entrambi con certificati univoci.

* Il **nome alternativo del soggetto** per ogni certificato deve essere impostato sul **nome di dominio completo (FQDN)** del computer. Il **nome comune (CN)** deve corrispondere al nome alternativo del soggetto per ogni certificato.

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>Come preconfigurare le impostazioni per gli utenti del client Web Desktop remoto
Questa sezione illustrerà come usare PowerShell per configurare le impostazioni per la distribuzione del client Web Desktop remoto. Questi cmdlet di PowerShell controllano la possibilità da parte di un utente di modificare le impostazioni in base alle considerazioni sulla sicurezza o al flusso di lavoro desiderato dell'organizzazione. Le impostazioni seguenti si trovano tutte nel pannello laterale **Impostazioni** del client Web. 

### <a name="suppress-telemetry"></a>Eliminare la telemetria
Per impostazione predefinita, gli utenti possono scegliere di abilitare o disabilitare la raccolta dei dati di telemetria inviati a Microsoft. Per informazioni sui dati di telemetria raccolti da Microsoft, vedi l'informativa sulla privacy facendo clic sul collegamento nel pannello laterale **Informazioni su**.

Come amministratore, puoi scegliere di eliminare la raccolta della telemetria per la distribuzione usando il cmdlet di PowerShell seguente:

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "SuppressTelemetry" $true
   ```

Per impostazione predefinita, l'utente può scegliere di abilitare o disabilitare la telemetria. Il valore booleano **$false** corrisponderà al comportamento predefinito del client. Il valore booleano **$true** disabilita la telemetria e impedisce all'utente di abilitarla.

### <a name="remote-resource-launch-method"></a>Metodo di avvio delle risorse remote
Per impostazione predefinita, gli utenti possono scegliere di avviare le risorse remote (1) nel browser o (2) scaricando un file con estensione rdp da gestire con un altro client installato nel loro computer. Come amministratore, puoi scegliere di limitare il metodo di avvio delle risorse remote per la distribuzione con il comando di Powershell seguente:

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser" ($true|$false)
   ```
 Per impostazione predefinita, l'utente può selezionare uno dei due metodi di avvio. Il valore booleano **$true** forzerà l'utente ad avviare le risorse nel browser. Il valore booleano **$false** forzerà l'utente ad avviare le risorse scaricando un file con estensione rdp da gestire con un client RDP installato in locale.

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>Ripristinare le impostazioni predefinite delle configurazioni RDWebClientDeploymentSetting
Per ripristinare la configurazione predefinita di un'impostazione del client Web a livello di distribuzione, esegui il cmdlet di PowerShell seguente e usa il parametro --Name per specificare l'impostazione che vuoi ripristinare:
   ```PowerShell
    Reset-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser"
    Reset-RDWebClientDeploymentSetting -Name "SuppressTelemetry"
   ```

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se un utente rileva uno dei problemi seguenti quando apre il client Web per la prima volta, potrai risolvere il problema seguendo le istruzioni contenute nelle sezioni seguenti.

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>Come procedere se il browser dell'utente visualizza un avviso di sicurezza quando tenta di accedere al client Web

È possibile che il ruolo Accesso Web Desktop remoto non stia usando un certificato attendibile. Assicurati che tale ruolo sia configurato con un certificato attendibile pubblicamente.

Se il problema persiste, è possibile che il nome server nell'URL del client Web non corrisponda al nome fornito dal certificato Web Desktop remoto. Assicurati che l'URL usi l'FQDN del server che ospita il ruolo Web Desktop remoto.

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>Come procedere se l'utente non riesce a connettersi a una risorsa con il client Web anche se può visualizzare le voci in Tutte le risorse

Se l'utente segnala di non potersi connettere al client Web anche se può vedere elencate le risorse, verifica quanto segue:

* Il ruolo Gateway Desktop remoto è configurato correttamente per l'uso di un certificato pubblico attendibile?
* Nel server Gateway Desktop remoto sono installati gli aggiornamenti necessari? Assicurati che nel server sia installato l'[aggiornamento KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334).

Se l'utente tenta di connettersi e viene visualizzato un messaggio di errore per segnalare che "è stato ricevuto un certificato di autenticazione server imprevisto", il messaggio mostrerà l'identificazione personale del certificato. Cerca il gestore di certificati del server Gestore Desktop remoto usando tale identificazione personale per trovare il certificato corretto. Verifica che il certificato sia configurato per essere usato per il ruolo Gestore Desktop remoto nella pagina delle proprietà della distribuzione Desktop remoto. Dopo aver controllato che il certificato non sia scaduto, copialo sotto forma di file con estensione cer nel server Accesso Web Desktop remoto ed esegui il comando seguente nel server Accesso Web Desktop remoto con il valore tra parentesi angolari sostituito dal percorso del file del certificato:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>Diagnosticare i problemi con il log della console

Se non riesci a risolvere il problema seguendo le istruzioni per la risoluzione dei problemi contenute in questo articolo, puoi provare a diagnosticare da solo l'origine del problema esaminando il log della console nel browser. Il client Web fornisce un metodo per registrare l'attività di log della console del browser mentre si usa il client Web per agevolare la diagnosi dei problemi.

* Fai clic sui puntini di sospensione nell'angolo superiore destro e passa alla pagina **Informazioni su** dal menu a discesa.
* In **Capture support information** (Acquisisci informazioni supporto) fai clic sul pulsante **Avvia registrazione**.
* Esegui nel client Web le operazioni che hanno generato il problema che stai tentando di diagnosticare.
* Passa alla pagina **Informazioni su** e seleziona **Interrompi registrazione**.
* Il browser scaricherà automaticamente un file con estensione txt denominato **RD Console Logs.txt**. Questo file conterrà l'intera attività di log della console generata durante la riproduzione del problema in questione.

È anche possibile accedere alla console direttamente tramite il browser. La console in genere è disponibile negli strumenti di sviluppo. Ad esempio, puoi accedere al log in Microsoft Edge premendo il tasto **F12** oppure facendo clic sui puntini di sospensione e passando ad **Altri strumenti** > **Strumenti di sviluppo**.

## <a name="get-help-with-the-web-client"></a>Ottenere assistenza per il client Web

Se hai rilevato un problema che non si risolve seguendo le informazioni fornite in questo articolo, puoi segnalarcelo [via posta elettronica](mailto:rdwbclnt@microsoft.com). Puoi anche richiedere o votare per nuove funzionalità nella [casella riservata ai suggerimenti](https://aka.ms/rdwebfbk).
