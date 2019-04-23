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
ms.openlocfilehash: e8c5da1960d0e3129b5520e65c2d5ecf45eef778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886952"
---
# <a name="get-started-with-remote-desktop-on-mac"></a>Introduzione a Desktop remoto su Mac

>Si applica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

È possibile usare il client Desktop remoto per Mac per lavorare con le app, risorse e desktop Windows dal computer Mac. Usare le informazioni seguenti per iniziare - e consultare il [domande frequenti su](remote-desktop-client-faq.md) se hai domande.

>[!Note]
> - Conoscere le nuove versioni per i client macOS? Scopri [novità di Desktop remoto su Mac?](mac-whatsnew.md)
> - Viene eseguito il client Mac nei computer che eseguono macOS 10.10 e versioni successive.
> - Le informazioni in questo articolo si applicano principalmente per la versione completa del client Mac - la versione disponibile in Mac App Store. Provare le nuove funzionalità, scaricare l'app di anteprima di seguito: [note sulla versione di client beta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## <a name="get-the-remote-desktop-client"></a>Ottenere il client Desktop remoto
Seguire questi passaggi per iniziare a usare Desktop remoto nel Mac:

1. Scaricare il client di Desktop remoto Microsoft dal [Mac App Store](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configurare il PC per accettare le connessioni remote](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). (Se si ignora questo passaggio, è possibile connettersi al PC.)
3. Aggiungere una connessione Desktop remoto o una risorsa remota. Si usa una connessione per connettersi direttamente a un PC Windows e una risorsa remota. utilizzare un programma RemoteApp, desktop basati su sessione o un desktop virtuale pubblicato locale utilizzando Connessione RemoteApp e Desktop. Questa funzionalità è in genere disponibile negli ambienti aziendali.

## <a name="what-about-the-mac-beta-client"></a>Per quanto riguarda il client per Mac versione beta?
Viene testato nuove funzionalità nel nostro canale di anteprima in HockeyApp. Necessario per l'estrazione? Passare a [Desktop remoto Microsoft per Mac](https://go.microsoft.com/fwlink/?LinkID=619698) e fare clic su **scaricare**. Non è necessario creare un account o l'accesso a HockeyApp per scaricare il client beta.

Se hai già il client, è possibile cercare gli aggiornamenti per assicurarsi di che aver la versione più recente. Nel client beta, fare clic su **Microsoft Remote Desktop Beta** nella parte superiore, quindi fare clic su **Controlla aggiornamenti**. 

## <a name="add-a-remote-desktop-connection"></a>Aggiungere una connessione Desktop remoto
Per creare una connessione desktop remoto:

1. Nel Centro connessioni, fare clic su **+**, quindi fare clic su **Desktop**.
2. Immettere le informazioni seguenti:
   - **Nome PC** -il nome del computer.
      - Può trattarsi di un nome di computer Windows (disponibili nel **sistema** impostazioni), un nome di dominio o un indirizzo IP.
      - È anche possibile aggiungere informazioni sulla porta alla fine di questo nome, ad esempio *MyDesktop:3389*.
   - **Account utente** -aggiungere l'account utente usato per accedere al computer remoto.
      - Per Active Directory (AD) aggiunti a un computer o account locali, usare uno dei seguenti formati: *user_name*, *DOMINIO\nome_utente.*, o *user_name@domain.com*.
      - Per Azure Active Directory (AAD) unita in join i computer, usare uno dei formati seguenti: *AzureAD\user_name* oppure *AzureAD\user_name@domain.com*.
      - È anche possibile scegliere se richiedere una password.
      - Quando si gestiscono più account utente con lo stesso nome utente, impostare un nome descrittivo per distinguere gli account.
      - Gestire gli account utente salvato nelle preferenze dell'app. 

3. È anche possibile impostare queste impostazioni facoltative per la connessione:
   - Impostare un nome descrittivo 
   - Aggiungere un Gateway
   - Impostare l'output audio
   - Scambia i pulsanti del mouse
   - Abilitare la modalità di amministrazione
   - Reindirizzamento cartelle locali in una sessione remota
   - Inoltro delle stampanti locali
   - Inoltro Smart card
4. Fare clic su **Salva**.

Per avviare la connessione, semplicemente fare doppio clic. Lo stesso vale per le risorse remote.

### <a name="export-and-import-connections"></a>Esportare e importare connessioni
È possibile esportare una definizione della connessione desktop remoto e usarla in un altro dispositivo. Desktop remoto vengono salvati in separato. File RDP.

1. In Centro connessioni, fare clic sul desktop remoto.
2. Fare clic su **esportare**.
3. Passare al percorso in cui si desidera salvare il desktop remoto. File con estensione RDP.
4. Fare clic su **OK**.

Utilizzare la procedura seguente per importare un desktop remoto. File con estensione RDP.

1. Nella barra dei menu, fare clic su **File > Importa**.
2. Individuare il. File con estensione RDP.
3. Fare clic su **Apri**.

## <a name="add-a-remote-resource"></a>Aggiungere una risorsa remota.
Risorse remote sono programmi RemoteApp, desktop basati su sessione e i desktop virtuali pubblicati tramite connessione RemoteApp e Desktop.

- L'URL viene visualizzato il collegamento al server Accesso Web desktop remoto che consente di accedere a connessione RemoteApp e Desktop.
- Sono elencati i configurato Connessione RemoteApp e Desktop.

Per aggiungere una risorsa remota:

1. Nella fare clic su Centro connessioni **+**, quindi fare clic su **aggiungere risorse Remote**. 
2. Immettere le informazioni per la risorsa remota:
   - **URL del feed** -l'URL del server Accesso Web desktop remoto. È inoltre possibile immettere l'account di posta elettronica aziendale in questo campo: in questo modo il client per cercare il Server di accesso Web desktop remoto associato l'indirizzo di posta elettronica.
   - **Nome utente** -il nome utente da utilizzare per il server Accesso Web desktop remoto si è connessi.
   - **Password** -la password da utilizzare per il server Accesso Web desktop remoto si è connessi.
3. Fare clic su **Salva**.


Verranno visualizzate nel Centro connessioni di risorse remote.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Connettersi a un Gateway Desktop remoto per accedere alle risorse interne

Gateway Desktop remoto (Gateway Desktop remoto) consente di connettersi a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. È possibile creare e gestire i gateway nelle preferenze dell'app o durante la configurazione di una nuova connessione di desktop.

Per configurare un nuovo gateway nelle preferenze:

1. Nel Centro connessioni, fare clic su **Preferenze > gateway**. 
2. Scegliere il **+** nella parte inferiore della tabella immettere le informazioni seguenti:
  - **Nome del server** : il nome del computer di cui si desidera utilizzare come gateway. Può trattarsi di un nome di computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del server (ad esempio: **RDGateway:443** oppure **10.0.0.1: 443**).
  - **Nome utente** -il nome utente e la password da utilizzare per il gateway Desktop remoto si connette. È inoltre possibile selezionare **utilizzare le credenziali di connessione** da utilizzare il medesimo nome utente e la password come quelle utilizzate per la connessione desktop remoto.


## <a name="manage-your-user-accounts"></a>Gestire gli account utente

Quando ci si connette a una risorsa remota o desktop, è possibile salvare gli account utente per selezionare nuovamente. È possibile gestire gli account utente utilizzando il client Desktop remoto.

Per creare un nuovo account utente:

1. Nel Centro connessioni, fare clic su **le impostazioni** > **account**.
2. Fare clic su **aggiungere Account utente**.
3. Immettere le informazioni seguenti:
   - **Nome utente** -il nome dell'utente da salvare per l'utilizzo con una connessione remota. È possibile immettere il nome utente in uno dei seguenti formati: nome_utente, DOMINIO\nome_utente., o user_name@domain.com.
   - **Password** -la password per l'utente specificato. Ogni account utente che si desidera salvare da utilizzare per le connessioni remote deve avere una password associata.
   - **Nome descrittivo** : se si usa lo stesso account utente con password diverse, impostare un nome descrittivo per distinguere gli account utente.
4. Toccare **salvare**, e quindi toccare **impostazioni**.

## <a name="customize-your-display-resolution"></a>Personalizzare la risoluzione dello schermo
È possibile specificare la risoluzione dello schermo per la sessione di desktop remoto.

1. Nel Centro connessioni, fare clic su **Preferenze**.
2. Fare clic su **risoluzione**. 
3. Fare clic su **+**.
4. Immettere una risoluzione altezza e la larghezza e quindi fare clic su **OK.**

Per eliminare la risoluzione, selezionarlo e quindi fare clic su **-**.

**Consente di visualizzare deve contenere spazi separati** se è in esecuzione Mac OS X 10.9 e disabilitati **consente di visualizzare deve contenere spazi separati** in Mavericks (**preferenze di sistema > Mission Control**), è necessario configurare Questa impostazione del client di desktop remoto con la stessa opzione.

### <a name="drive-redirection-for-remote-resources"></a>Reindirizzamento delle unità per le risorse remote
Il reindirizzamento delle unità è supportata per le risorse remote, in modo che sia possibile salvare i file creati con un'applicazione remota localmente al Mac. La cartella reindirizzata è sempre la home directory visualizzata come un'unità di rete nella sessione remota.

> [!NOTE]
> Per usare questa funzionalità, l'amministratore deve impostare le impostazioni appropriate nel server.


## <a name="use-a-keyboard-in-a-remote-session"></a>Utilizzare una tastiera in una sessione remota

Layout di tastiera Mac differiscono dal layout di tastiera di Windows. 

- Il tasto di comando sulla tastiera Mac è uguale alla chiave di Windows.
- Per eseguire azioni che usano il pulsante di comando nel computer Mac, è necessario usare il pulsante di controllo in Windows (ad esempio: Copia = Ctrl + C).
- I tasti funzione possono essere attivati nella sessione facendo inoltre premendo il tasto FN (ad esempio: FN + F1).
- Il tasto Alt a destra della barra spaziatrice sulla tastiera Mac è uguale al tasto Alt destro Gr Alt in Windows.

Per impostazione predefinita, la sessione remota utilizzerà le stesse impostazioni locali della tastiera come sistema operativo su cui viene eseguito il client. (Se il Mac sia in esecuzione un en-us del sistema operativo, che verrà usato per le sessioni remote anche. Se le impostazioni locali del sistema operativo della tastiera non viene utilizzata, controllare la tastiera impostazione nel computer remoto e modificare l'impostazione manualmente. Vedere le [domande frequenti sul Client Desktop remoto](remote-desktop-client-faq.md) per altre informazioni su tastiere e le impostazioni locali.


## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>Supporto per l'autenticazione del plug-in gateway Desktop remoto e l'autorizzazione

Windows Server 2012 R2 introdotto il supporto per un nuovo metodo di autenticazione, autenticazione plug-in Gateway Desktop remoto e l'autorizzazione, che offre maggiore flessibilità per le routine di autenticazione personalizzato. È ora possibile questo modello di autenticazione con il client Mac. 

> [!IMPORTANT]
> Modelli di autenticazione e autorizzazione personalizzati prima di Windows 8.1 non sono supportati, anche se nell'articolo precedente vengono illustrate tali.

Per altre informazioni su questa funzionalità, consultare [ http://aka.ms/paa-sample ](http://aka.ms/paa-sample).


> [!TIP]
> Domande e commenti sono sempre Benvenuti. Tuttavia, NON inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, andare alla [forum su client di Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Avete suggerimenti funzionalità? Comunicaci nel [forum dedicato agli utenti di client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

