---
title: Introduzione a Desktop remoto su Mac
description: Informazioni su come configurare il client Desktop remoto per Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 10/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 17df3ca3b88404a2775790d7a4a8206b7aa5befa
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546351"
---
# <a name="get-started-with-remote-desktop-on-mac"></a>Introduzione a Desktop remoto su Mac

>Si applica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Puoi eseguire il client Desktop remoto per Mac per utilizzare desktop, risorse e app di Windows dal computer Mac. Per iniziare, usa le informazioni seguenti e, se hai dubbi, consulta le [domande frequenti](remote-desktop-client-faq.md).

>[!NOTE]
> - Se vuoi conoscere le nuove versioni per il client macOS, vedi [Novità di Desktop remoto in Mac](mac-whatsnew.md).
> - Il client Mac viene eseguito in computer con macOS 10.10 e versioni successive.
> - Le informazioni in questo articolo si applicano principalmente alla versione completa del client Mac, ovvero la versione disponibile in Mac App Store. Prova le nuove funzionalità scaricando da qui l'app di anteprima: [note sulla versione beta del client](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## <a name="get-the-remote-desktop-client"></a>Ottenere il client Desktop remoto
Per iniziare a usare Desktop remoto nel computer Mac, segui questi passaggi:

1. Scaricare il client di Desktop remoto Microsoft dal [Mac App Store](https://itunes.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configura il PC per accettare le connessioni remote](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). Se ignori questo passaggio, non puoi connetterti al PC.
3. Aggiungi una connessione Desktop remoto o una risorsa remota. Una connessione consente di connettersi direttamente a un PC Windows, mentre una risorsa remota consente di usare un programma RemoteApp, un desktop basato su sessione o un desktop virtuale pubblicato in locale usando connessioni RemoteApp e Desktop. Questa funzionalità è in genere disponibile negli ambienti aziendali.

## <a name="what-about-the-mac-beta-client"></a>Informazioni sulla versione beta del client Mac
Stiamo testando nuove funzionalità nel canale di anteprima in HockeyApp. Per dare un'occhiata, passa a [Microsoft Remote Desktop for Mac](https://go.microsoft.com/fwlink/?LinkID=619698) e fai clic su **Download** (Scarica). Non devi creare un account o accedere a HockeyApp per scaricare la versione beta del client.

Se hai già il client, puoi verificare la disponibilità di aggiornamenti per assicurarti di usare la versione più recente. Nel client beta, fare clic su **Microsoft Remote Desktop Beta** nella parte superiore, quindi fare clic su **Controlla aggiornamenti**. 

## <a name="add-a-remote-desktop-connection"></a>Aggiungere una connessione Desktop remoto
Per creare una connessione Desktop remoto:

1. In Connection Center (Centro connessioni) fai clic su **+** e quindi su **Desktop**.
2. Immettere le informazioni seguenti:
   - **PC name** (Nome PC): nome del computer.
      - Puoi usare il nome di un computer Windows (individuabile nelle impostazioni di **sistema**), un nome di dominio o un indirizzo IP.
      - Puoi anche aggiungere informazioni sulla porta alla fine del nome, ad esempio *MyDesktop:3389*.
   - **User Account** (Account utente): account utente usato per accedere al PC remoto.
     - Per gli account locali o i computer aggiunti ad Active Directory (AD), usa uno dei formati seguenti: *user_name*, *domain\user_name* oppure <em>user_name@domain.com</em>.
     - Per i computer aggiunti ad Azure Active Directory (AAD), usa uno dei formati seguenti: *AzureAD\user_name* oppure <em>AzureAD\user_name@domain.com</em>.
     - Puoi anche scegliere se richiedere una password.
     - Quando gestisci più account utente con lo stesso nome utente, imposta un nome descrittivo per distinguere gli account.
     - Gestisci gli account utente salvati nelle preferenze dell'app. 

3. Puoi anche definire queste impostazioni facoltative per la connessione:
   - Impostare un nome descrittivo 
   - Aggiungere un gateway
   - Impostare l'output audio
   - Scambiare i pulsanti del mouse
   - Abilitare la modalità di amministrazione
   - Reindirizzare le cartelle locali in una sessione remota
   - Inoltrare le stampanti locali
   - Inoltrare le smart card
4. Fare clic su **Save**.

Per avviare la connessione, fai semplicemente doppio clic su di essa. Usa la stessa modalità di selezione per le risorse remote.

### <a name="export-and-import-connections"></a>Esportare e importare connessioni
Puoi esportare una definizione della connessione Desktop remoto e usarla in un altro dispositivo. Desktop remoto vengono salvati in separato. File RDP.

1. In Centro connessioni, fare clic sul desktop remoto.
2. Fare clic su **esportare**.
3. Passare al percorso in cui si desidera salvare il desktop remoto. File con estensione RDP.
4. Fare clic su **OK**.

Utilizzare la procedura seguente per importare un desktop remoto. File con estensione RDP.

1. Nella barra dei menu fai clic su **File** > **Import** (File > Importa).
2. Individuare il. File con estensione RDP.
3. Fare clic su **Apri**.

## <a name="add-a-remote-resource"></a>Aggiungere una risorsa remota
Risorse remote sono programmi RemoteApp, desktop basati su sessione e i desktop virtuali pubblicati tramite connessione RemoteApp e Desktop.

- L'URL visualizza il collegamento al server Accesso Web Desktop remoto che consente di accedere a connessioni RemoteApp e Desktop.
- Vengono elencate le connessioni RemoteApp e Desktop configurate.

Per aggiungere una risorsa remota:

1. In Connection Center (Centro connessioni) fai clic su **+** e quindi su **Add Remote Resources** (Aggiungi risorse remote). 
2. Immettere le informazioni per la risorsa remota:
   - **URL del feed** -l'URL del server Accesso Web desktop remoto. È inoltre possibile immettere l'account di posta elettronica aziendale in questo campo: in questo modo il client per cercare il Server di accesso Web desktop remoto associato l'indirizzo di posta elettronica.
   - **Nome utente** -il nome utente da utilizzare per il server Accesso Web desktop remoto si è connessi.
   - **Password** -la password da utilizzare per il server Accesso Web desktop remoto si è connessi.
3. Fare clic su **Save**.


Verranno visualizzate nel Centro connessioni di risorse remote.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Connettersi a un gateway Desktop remoto per accedere alle risorse interne

Un gateway Desktop remoto consente di stabilire la connessione a un computer remoto in una rete aziendale da qualsiasi posizione in Internet. Puoi creare e gestire i gateway nelle preferenze dell'app o durante la configurazione di una nuova connessione desktop.

Per impostare un nuovo gateway nelle preferenze:

1. In Connection Center (Centro connessioni) fai clic su **Preferences > Gateway** (Preferenze > Gateway). 
2. Seleziona il pulsante **+** nella parte inferiore della tabella e immetti le informazioni seguenti:
   - **Nome del server** : il nome del computer di cui si desidera utilizzare come gateway. Puoi usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere informazioni sulla porta al nome del server (ad esempio: **RDGateway:443** oppure **10.0.0.1:443**).
   - **Nome utente** -il nome utente e la password da utilizzare per il gateway Desktop remoto si connette. È inoltre possibile selezionare **utilizzare le credenziali di connessione** da utilizzare il medesimo nome utente e la password come quelle utilizzate per la connessione desktop remoto.


## <a name="manage-your-user-accounts"></a>Gestire gli account utente

Quando ci si connette a una risorsa remota o desktop, è possibile salvare gli account utente per selezionare nuovamente. È possibile gestire gli account utente utilizzando il client Desktop remoto.

Per creare un nuovo account utente:

1. In Connection Center (Centro connessioni) fai clic su **Settings** > **Accounts** (Impostazioni > Account).
2. Fai clic su **Add User Account** (Aggiungi account utente).
3. Immettere le informazioni seguenti:
   - **Nome utente** -il nome dell'utente da salvare per l'utilizzo con una connessione remota. Puoi immettere il nome utente in uno dei formati seguenti: user_name, domain\user_name o user_name@domain.com.
   - **Password** -la password per l'utente specificato. Ogni account utente che si desidera salvare da utilizzare per le connessioni remote deve avere una password associata.
   - **Friendly Name** (Nome descrittivo): se stai usando lo stesso account utente con password diverse, imposta un nome descrittivo per distinguere gli account utente.
4. Toccare **salvare**, e quindi toccare **impostazioni**.

## <a name="customize-your-display-resolution"></a>Personalizzare la risoluzione dello schermo
È possibile specificare la risoluzione dello schermo per la sessione di desktop remoto.

1. Nel Centro connessioni, fare clic su **Preferenze**.
2. Fare clic su **risoluzione**. 
3. Fare clic su **+** .
4. Immettere una risoluzione altezza e la larghezza e quindi fare clic su **OK.**

Per eliminare la risoluzione, selezionarlo e quindi fare clic su **-** .

**Displays have separate spaces** (Schermi con spazi separati). Se usi Mac OS X 10.9 e hai disabilitato l'opzione **Displays have separate spaces** (Schermi con spazi separati) in Mavericks (**System Preferences > Mission Control** (Preferenze di sistema > Controllo missione) devi configurare questa impostazione nel client Desktop remoto con la stessa opzione.

### <a name="drive-redirection-for-remote-resources"></a>Reindirizzamento delle unità per le risorse remote
Il reindirizzamento delle unità è supportato per le risorse remote e consente di salvare localmente nel computer Mac i file creati con un'applicazione remota. La cartella reindirizzata è sempre la home directory visualizzata come un'unità di rete nella sessione remota.

> [!NOTE]
> Per usare questa funzionalità, l'amministratore deve definire le impostazioni appropriate nel server.


## <a name="use-a-keyboard-in-a-remote-session"></a>Usare una tastiera in una sessione remota

I layout di tastiera Mac differiscono dai layout di tastiera Windows. 

- Il tasto COMANDO sulla tastiera Mac corrisponde al tasto WINDOWS.
- Per eseguire le azioni che usano il tasto COMANDO in Mac, devi usare il tasto CTRL di Windows (ad esempio: Copia = CTRL+C).
- I tasti funzione possono essere attivati durante la sessione premendo anche il tasto FN (ad esempio: FN+F1).
- Il tasto ALT a destra della barra spaziatrice sulla tastiera Mac corrisponde al tasto ALTGR/ALT di destra in Windows.

Per impostazione predefinita, la sessione remota userà le stesse impostazioni locali della tastiera del sistema operativo su cui viene eseguito il client. Se il Mac esegue un sistema operativo in lingua inglese, questo stesso sistema operativo verrà usato per le sessioni remote. Se le impostazioni locali della tastiera del sistema operativo non vengono usate, controlla l'impostazione della tastiera nel computer remoto e modifica l'impostazione manualmente. Per altre informazioni sulle tastiere e le impostazioni locali, vedi [Domande frequenti sul client Desktop remoto](remote-desktop-client-faq.md).


## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>Supporto per l'autenticazione del plug-in gateway Desktop remoto e l'autorizzazione

Windows Server 2012 R2 introdotto il supporto per un nuovo metodo di autenticazione, autenticazione plug-in Gateway Desktop remoto e l'autorizzazione, che offre maggiore flessibilità per le routine di autenticazione personalizzato. È ora possibile questo modello di autenticazione con il client Mac. 

> [!IMPORTANT]
> Modelli di autenticazione e autorizzazione personalizzati prima di Windows 8.1 non sono supportati, anche se nell'articolo precedente vengono illustrate tali.

Per altre informazioni su questa funzionalità, consulta [http://aka.ms/paa-sample](http://aka.ms/paa-sample).


> [!TIP]
> Domande e commenti sono sempre Benvenuti. Tuttavia, NON inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, andare alla [forum su client di Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Avete suggerimenti funzionalità? Comunicaci nel [forum dedicato agli utenti di client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

