---
title: Configurare il client Web di Desktop remoto per gli utenti
description: Descrive come un amministratore può configurare il client web di Desktop remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 2cb819a7f91646c61b84c3ee70550af6033ba340
ms.sourcegitcommit: 491c94b25501732c4a4abda533cc62b8bd278ed2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099184"
---
# Configurare il client Web di Desktop remoto per gli utenti

Il client web di Desktop remoto consente agli utenti di accedere l'infrastruttura dell'organizzazione Desktop remoto tramite un browser compatibile. Sarà in grado di interagire con le app remote o desktop come farebbero con un PC locale indipendentemente da dove si trovano. Dopo aver configurato il client web di Desktop remoto, tutti gli utenti necessitano per iniziare a è l'URL in cui poter accedere il client, le proprie credenziali e un browser web supportati.

>[!IMPORTANT]
>Il client web non supporta attualmente l'utilizzo di Proxy applicazione di Azure e Proxy dell'applicazione Web non supporta affatto. Per informazioni dettagliate, vedere [Tramite Servizi Desktop remoto con servizi di proxy dell'applicazione](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) .

## Cosa è necessario configurare il client web

Prima di iniziare, tieni presente le seguenti operazioni::

* Assicurati che la [distribuzione di Desktop remoto](../rds-deploy-infrastructure.md) include un Gateway Desktop remoto, un gestore connessione desktop remoto e accesso Web desktop remoto in esecuzione su Windows Server 2016 o 2019.
* Assicurati che la distribuzione è configurata per [licenze di accesso per ogni singolo utente client](../rds-client-access-license.md) (CAL) invece di per ogni dispositivo, in caso contrario, che verranno utilizzate tutte le licenze.
* Installare l' [aggiornamento di Windows 10 KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) sul Gateway Desktop remoto. In un secondo momento aggiornamenti cumulativi potrebbe contiene già questo KB.
* Assicurati che i certificati attendibili pubblici sono configurati per i ruoli di Gateway Desktop remoto e accesso Web desktop remoto.
* Assicurati che tutti i computer che si connetteranno gli utenti siano in esecuzione una delle versioni del sistema operativo seguenti:
  * Windows10
  * Windows Server 2008 R2 o versione successiva

Gli utenti vedranno migliori prestazioni connessione a Windows Server 2016 (o versioni successive) e Windows 10 (versione 1611 o versione successiva).

>[!IMPORTANT]
>Se si utilizza il client web durante il periodo di anteprima e installato una versione precedente 1.0.0, è necessario disinstallare il client precedente prima di passare alla nuova versione. Se ricevi un errore che indica "il client web è stato installato tramite una versione precedente di RDWebClientManagement e deve essere rimossa prima di tutto prima di distribuire la nuova versione", segui questi passaggi:
>
>1. Apri un prompt di PowerShell con privilegi elevato.
>2. Eseguire **RDWebClientManagement modulo di disinstallazione** per disinstallare il modulo di nuovo.
>3. Chiudere e riaprire il prompt di PowerShell con privilegi elevati.
>4. Eseguire **version> \<old parametri-RequiredVersion RDWebClientManagement modulo di installazione per installare il modulo precedente.**
>5. Eseguire **RDWebClient di disinstallazione** per disinstallare il client web precedente.
>6. Eseguire **RDWebClientManagement modulo di disinstallazione** per disinstallare il modulo precedente.
>7. Chiudere e riaprire il prompt di PowerShell con privilegi elevati.
>8. Procedi con la procedura di installazione normale come indicato di seguito.

## Come pubblicare il client web di Desktop remoto

Per installare il client web per la prima volta, segui questi passaggi:

1. Nel server Gestore connessione desktop remoto, ottenere il certificato usato per le connessioni Desktop remoto e lo esportate come file con estensione cer. Copia il file con estensione cer dal Gestore connessione desktop remoto per il server che esegue il ruolo Web desktop remoto.
2. Nel server di accesso Web desktop remoto, Apri un prompt di PowerShell con privilegi elevato.
3. In Windows Server 2016, aggiorna il modulo PowerShellGet poiché la versione della posta in arrivo non supporta l'installazione del modulo di gestione client web. Per aggiornare PowerShellGet, eseguire il cmdlet seguente:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Dovrai riavviare PowerShell per l'aggiornamento può rendere effettive, in caso contrario, che il modulo potrebbe non funzionare.

4. Installare il modulo PowerShell di gestione client di web di Desktop remoto da PowerShell gallery con questo cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Dopo questa operazione, Esegui il cmdlet seguente per scaricare la versione più recente del client web di Desktop remoto:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. Successivamente, Esegui questo cmdlet con il valore racchiuso tra parentesi quadre sostituito con il percorso del file con estensione cer che hai copiato dal gestore desktop remoto:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Infine, Esegui questo cmdlet per pubblicare il client web di Desktop remoto:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Assicurati che il client web l'URL del client web accessibili con il nome del server, formattato come <https://server_FQDN/RDWeb/webclient/index.html>. È importante usare il nome del server che corrisponda al certificato pubblico accesso Web desktop remoto nell'URL (in genere il server FQDN).

    >[!NOTE]
    >Quando si esegue il cmdlet **Pubblica-RDWebClientPackage** , potresti vedere un avviso che indica le licenze CAL per ogni dispositivo che non sono supportati, anche se la distribuzione è configurata per licenze CAL per ogni utente. Se la distribuzione Usa licenze CAL per ogni singolo utente, è possibile ignorare questo avviso. Verrà visualizzata per essere certo grado di riconoscere la limitazione di configurazione.
8. Quando sei pronto agli utenti di accedere al client web, è sufficiente inviare loro l'URL del client web è stato creato.

>[!NOTE]
>Per visualizzare un elenco di tutti i cmdlet supportati per il modulo RDWebClientManagement, Esegui il seguente cmdlet di PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## Come aggiornare il client web di Desktop remoto

Quando è disponibile una nuova versione del client web di Desktop remoto, segui questi passaggi per la distribuzione degli aggiornamenti con il nuovo client:

1. Apri un prompt di PowerShell con privilegi elevato nel server di accesso Web desktop remoto ed Esegui il cmdlet seguente per scaricare la versione più recente del client web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Facoltativamente, puoi pubblicare il client per il test prima del rilascio ufficiale eseguendo questo cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    Il client deve essere visualizzato nell'URL test che corrisponde all'URL del client web (ad esempio, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Pubblicare il client per gli utenti eseguendo il cmdlet seguente:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Questo sostituirà il client per tutti gli utenti quando vengono riavviare la pagina web.

## Come disinstallare il client web di Desktop remoto

Per rimuovere tutte le tracce del client web, segui questi passaggi:

1. Nel server di accesso Web desktop remoto, Apri un prompt di PowerShell con privilegi elevato.
2. Annullare la pubblicazione i client di Test e produzione, disinstallare tutti i pacchetti locali e rimuovere le impostazioni client web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Disinstallare il modulo PowerShell di gestione client di web di Desktop remoto:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## Come installare il client web di Desktop remoto senza una connessione internet

Segui questi passaggi per distribuire il client web a un server di accesso Web desktop remoto che non dispone di una connessione internet.

> [!NOTE]
> L'installazione senza una connessione internet è disponibile nella versione 1.0.1 e di sopra del modulo RDWebClientManagement PowerShell.

> [!NOTE]
> Devi comunque un amministratore di PC con accesso a internet per scaricare i file necessari prima del trasferimento al server offline.

> [!NOTE]
> Il PC degli utenti finali richiede una connessione internet per il momento. Questo verrà risolto in una versione futura del client per fornire uno scenario offline completo.

### Da un dispositivo con accesso a internet

1. Apri un prompt di PowerShell.

2. Importa il modulo PowerShell di gestione client di web Desktop remoto da PowerShell gallery:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Scarica la versione più recente del client web di Desktop remoto per l'installazione su un altro dispositivo:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Scarica la versione più recente del modulo RDWebClientManagement PowerShell:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copia il contenuto della "C:\WebClient\" al server di accesso Web desktop remoto.

### Dal server di accesso Web desktop remoto

Segui le istruzioni in [come pubblicare il client web di Desktop remoto](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), sostituendo i passaggi 4 e 5 con il seguente.

4. Importa il modulo PowerShell di gestione client di web Desktop remoto dalla cartella locale:
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Distribuire la versione più recente del client web di Desktop remoto dalla cartella locale (sostituzione con il file zip appropriato):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## La connessione al gestore di desktop remoto senza Gateway Desktop remoto in Windows Server 2019
Questa sezione descrive come abilitare una connessione client web di un gestore di desktop remoto senza Gateway Desktop remoto in Windows Server 2019.

### La configurazione del server di desktop remoto

#### Segui questi passaggi se non è presente alcun certificato associato al server di desktop remoto

1. Apri **Server Manager** > **Servizi Desktop remoto**.

2. Nella sezione **Cenni preliminari sulla distribuzione** , seleziona il menu a discesa di **attività** .

3. Verrà aperta seleziona **Modificare le proprietà di distribuzione**, una nuova finestra intitolata **Proprietà di distribuzione** .

4. Nella finestra **Proprietà di distribuzione** , seleziona **i certificati** nel menu a sinistra.

5. Nell'elenco dei livelli di certificato, seleziona **Gestore connessione desktop remoto - abilitare Single Sign-On**. Hai due opzioni: (1) creare un nuovo certificato o (2) un certificato esistente.

#### Segui questi passaggi se è presente un certificato in precedenza associato al server di desktop remoto

1. Apri il certificato associato al gestore e copia il valore di **identificazione personale** .

2. Per associare il certificato alla porta sicura 3392, Apri una finestra di PowerShell con privilegi elevata ed Esegui il comando seguente, sostituendo **lt identificazione personale gt _"** con il valore copiato dal passaggio precedente:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Per controllare se il certificato è stato associato correttamente, Esegui il comando seguente:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Nell'elenco dei binding certificato SSL, assicurati che il certificato corretto viene associato alla porta 3392.

3. Apri il Registro di sistema (regedit) e nagivate a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e individua la chiave **WebSocketURI**. Il valore deve essere impostato su **https://+:3392/rdp/**.

### Configurazione di Host sessione Desktop remoto
Se il server Host sessione Desktop remoto è diverso dal server di desktop remoto, segui questi passaggi:

1. Creare un certificato per il computer Host sessione Desktop remoto, aprilo e copia il valore di **identificazione personale** .

2. Per associare il certificato alla porta sicura 3392, Apri una finestra di PowerShell con privilegi elevata ed Esegui il comando seguente, sostituendo **lt identificazione personale gt _"** con il valore copiato dal passaggio precedente:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Per controllare se il certificato è stato associato correttamente, Esegui il comando seguente:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Nell'elenco dei binding certificato SSL, assicurati che il certificato corretto viene associato alla porta 3392.

3. Apri il Registro di sistema (regedit) e nagivate a ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e individua la chiave **WebSocketURI**. Il valore deve essere impostato su **https://+:3392/rdp/**.

### Osservazioni generali

* Assicurati che il server Host sessione Desktop remoto sia desktop sono in esecuzione Windows Server 2019.

* Assicurati che pubblica attendibili i certificati sono configurati per server Host sessione Desktop remoto e di desktop remoto.
    > [!NOTE]
    > Se l'Host sessione Desktop remoto sia il server gestore desktop remoto condividono lo stesso computer, imposta solo il certificato del server gestore desktop remoto. Se il server Host sessione Desktop remoto e desktop Usa computer diversi, entrambi deve essere configurati con certificati univoci.

* Il **Nome alternativo del soggetto (SAN)** per ogni certificato deve essere impostata su del computer **Completamente dominio nome completo (FQDN)**. Il **Nome comune (CN)** deve corrispondere alla SAN per ogni certificato.

## Risoluzione dei problemi

Se un utente segnala eventuali problemi seguenti all'apertura del client web per la prima volta, le sezioni seguenti indicherà che cosa fare per risolverli.

### Cosa fare se il browser dell'utente viene visualizzato un avviso di sicurezza quando si tenta di accedere al client web

Il ruolo di accesso Web desktop remoto possa non essere con un certificato attendibile. Assicurati che il ruolo di accesso Web desktop remoto è configurato con un certificato attendibile pubblicamente.

Se il problema persiste, il nome del server nel web URL client potrebbe non corrispondere al nome fornito dal certificato di Web desktop remoto. Assicurati che l'URL Usa il nome FQDN del server che ospita il ruolo Web desktop remoto.

### Cosa fare se l'utente non può connettersi a una risorsa con il client web, anche se possono vedere gli elementi in tutte le risorse

Se l'utente segnala che non può connettersi con il client web anche se possono vedere le risorse elencate, verificare quanto segue:

* Il ruolo di Gateway Desktop remoto è configurato correttamente per l'uso di un certificato attendibile pubblico?
* Il server Gateway Desktop remoto avere installato gli aggiornamenti necessari? Assicurati che il server disponga di [aggiornare il KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) installato.

Se l'utente riceve un errore "è stata ricevuta certificato di autenticazione server imprevisto" messaggio quando tentano di connettersi, quindi viene visualizzato il messaggio identificazione personale del certificato. Cerca in Gestione certificati del server gestore desktop remoto con tale identificazione personale per trovare il certificato corretto. Verificare che il certificato è configurato per essere usato per il ruolo di desktop remoto nella pagina delle proprietà di distribuzione di Desktop remoto. Dopo verificando che il certificato non è scaduto, copiare il certificato nel formato di file con estensione cer al server di accesso Web desktop remoto ed eseguire il comando seguente nel server di accesso Web desktop remoto con il valore racchiuso tra parentesi quadre sostituito dal percorso del file del certificato:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### Diagnosticare i problemi con il log di console

Se non è possibile risolvere il problema in base alle istruzioni risoluzione dei problemi in questo articolo, puoi provare a diagnosticare l'origine del problema osservando la console di log nel browser. Il client web offre un metodo per la registrazione dell'attività di log del browser console durante l'uso del client web per facilitare la diagnosi di problemi.

* Seleziona i puntini di sospensione nell'angolo superiore destro e passare alla pagina **** nel menu a discesa.
* Selezionare il pulsante di **avviare la registrazione di** **informazioni sul supporto di acquisizione** .
* Eseguire le operazioni nel client web che ha generato il problema che si sta tentando di diagnosticare.
* Passa alla pagina **su** e seleziona **interrompere la registrazione**.
* Il browser scaricherà automaticamente un file con estensione txt intitolato **Logs.txt Console desktop remoto**. Questo file conterrà l'attività di log console completo generato durante la riproduzione il problema di destinazione.

È possibile accedere direttamente tramite il browser anche la console. La console in genere si trova sotto gli strumenti di sviluppo. Ad esempio, è possibile accedere il registro in Microsoft Edge, premendo il tasto **F12** o selezionando i puntini di sospensione, quindi passare a **ulteriori strumenti** > **Strumenti di sviluppo**.

## Ottenere assistenza con il client web

Se si è verificato un problema che non può essere risolto tramite le informazioni contenute in questo articolo, è possibile [inviare un'e-mail](mailto:rdwbclnt@microsoft.com) per segnalare. È anche possibile richiedere o votare nuove funzionalità al nostro [casella di suggerimento](https://aka.ms/rdwebfbk).