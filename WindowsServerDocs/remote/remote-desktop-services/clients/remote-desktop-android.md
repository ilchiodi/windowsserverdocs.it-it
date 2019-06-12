---
title: Introduzione a Desktop remoto in Android
description: Installazione di base i passaggi per il client Desktop remoto per Android.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b4b188eb8148b2f4e5c6672b07884af8fdcd0c60
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446746"
---
# <a name="get-started-with-remote-desktop-on-android"></a>Introduzione a Desktop remoto in Android

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Uso di desktop e App di Windows direttamente da un dispositivo Android, è possibile usare il client Desktop remoto per Android.

Usare le informazioni seguenti per iniziare. Assicurarsi di consultare il [domande frequenti su](remote-desktop-client-faq.md) se hai domande.

> [!NOTE]
> - Conoscere le nuove versioni per il client Android? Scopri [novità di Desktop remoto in Android?](android-whatsnew.md)
> È possibile eseguire il client Android 4.1 e dispositivi più recenti che nei dispositivi Chromebooks con 53 ChromeOS installato. Altre informazioni sulle applicazioni Android su Chrome [qui](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="get-the-rd-client-and-start-using-it"></a>Ottenere il client desktop remoto e iniziare a usarlo

Seguire questi passaggi per iniziare a usare Desktop remoto nel dispositivo Android:

1. Scaricare il client Desktop remoto dalla [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configurare il PC per accettare le connessioni remote](remote-desktop-allow-access.md).
3. Aggiungere una connessione Desktop remoto o una risorsa remota. Si usa una connessione per connettersi direttamente a un PC Windows e una risorsa remota. utilizzare un programma RemoteApp, desktop basati su sessione o desktop virtuali pubblicato locale. 
4. Creazione di un widget è possibile ottenere rapidamente a Desktop remoto.

> [!NOTE]
> Se si vuole distribuire nuove funzionalità in precedenza, è consigliabile scaricare nostri [Microsoft Remote Desktop Beta](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) applicazione dallo store Google Play. 

### <a name="add-a-remote-desktop-connection"></a>Aggiungere una connessione Desktop remoto

Per creare una connessione Desktop remoto:

1. Nella scelta dei Centro connessioni **+** , e quindi toccare **Desktop**.
2. Immettere le informazioni seguenti per il computer si desidera connettersi:
   - **Nome PC** : il nome del computer. Può trattarsi di un nome di computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del computer (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Nome utente** : il nome utente da utilizzare per accedere al computer remoto. È possibile usare i seguenti formati: *user_name*, *DOMINIO\nome_utente.* , o <em>user_name@domain.com</em>. È inoltre possibile specificare se per la richiesta di un nome utente e una password.
3. È inoltre possibile impostare le opzioni aggiuntive seguenti:
   - **Nome descrittivo** : un nome facile da ricordare per il computer si è connessi. È possibile utilizzare qualsiasi stringa, ma se non si specifica un nome descrittivo, viene visualizzato il nome del computer.
   - **Gateway** – gateway Desktop remoto che si desidera utilizzare per connettersi a desktop virtuali, programmi RemoteApp e desktop basati su sessione su una rete aziendale interna. Ottenere le informazioni sul gateway, dall'amministratore di sistema.
    È necessario configurare un Gateway Desktop remoto?
   - **Suono** : selezionare il dispositivo da utilizzare per l'audio durante la sessione remota. È possibile scegliere di riprodurre un suono nei dispositivi locali, il dispositivo remoto oppure non funziona.
   - **Personalizzare risoluzione dello schermo** -impostare una soluzione personalizzata per una connessione attivando questa impostazione. Quando viene applicato disattivato la risoluzione che sono definite nelle impostazioni globali dell'app.
   - **Scambia i pulsanti del mouse** : utilizzare questa opzione per invertire il puntatore del mouse a sinistra pulsante funzioni per il pulsante destro del mouse. (Ciò è particolarmente utile se sul computer remoto è configurato per un utente da ma si utilizza un mouse da destra.)
   - **Connettersi alla sessione di amministrazione** -usare questa opzione per connettersi a una sessione di console per amministrare un server di Windows.
   - **Reindirizzare l'archiviazione locale** – Collega l'archiviazione locale come un file system remoto nel computer remoto.
4. Toccare **salvare**.

È necessario modificare queste impostazioni? Toccare il menu di overflow ( **...** ) accanto al nome del desktop e quindi toccare **modificare**.

Se si desidera eliminare la connessione? Nuovamente, toccare il menu di overflow ( **...** ), quindi toccare **rimuovere**.

>[!TIP]
> Se viene visualizzato errore 0xf07 una password errata ("è stato possibile connettersi al computer remoto perché la password associata all'account utente è scaduto"), modificare la password e riprovare.

### <a name="add-a-remote-resource"></a>Aggiungere una risorsa remota.
Risorse remote sono programmi RemoteApp, desktop basati su sessione e i desktop virtuali pubblicati tramite connessione RemoteApp e Desktop.

Per aggiungere una risorsa remota:

1. Nella schermata connessione Center toccare **+** , e quindi toccare **Feed risorse Remote**. 
2. Immettere le informazioni per la risorsa remota:
   - **Messaggio di posta elettronica o URL** -l'URL del server Accesso Web desktop remoto. È inoltre possibile immettere l'account di posta elettronica aziendale in questo campo: in questo modo il client per cercare il Server di accesso Web desktop remoto associato l'indirizzo di posta elettronica.
   - **Nome utente** -il nome utente da utilizzare per il server Accesso Web desktop remoto si è connessi.
   - **Password** -la password da utilizzare per il server Accesso Web desktop remoto si è connessi.
3. Toccare **salvare**.

Verranno visualizzate nel Centro connessioni di risorse remote.


Per eliminare le risorse remote:

1. Nel Centro connessioni, toccare il menu di overflow ( **...** ) accanto alla risorsa remota.
2. Toccare **rimuovere**.
3. Confermare l'eliminazione.

### <a name="widgets--pin-a-saved-desktop-to-your-home-screen"></a>Widget – Pin un desktop salvato nella schermata iniziale

Le applicazioni Desktop remoto supportano le connessioni di blocco nella schermata iniziale utilizzando la funzionalità di widget Android. La procedura per aggiungere un widget dipende dal tipo di dispositivo Android in uso e al sistema operativo. Ecco il modo più comune per aggiungere un widget: 

1. Toccare **app** per avviare il menu di applicazioni.
2. Toccare **widget**.
3. Scorrere tramite i widget e cercare l'icona del Desktop remoto con la descrizione "Pin di Desktop remoto".
4. Tenendo premuta widget Desktop remoto e spostarlo sullo schermo principale.
5. Quando si rilascia l'icona, verrà visualizzato il desktop remoto salvato. Scegliere la connessione che si desidera salvare nella schermata iniziale.

Ora è possibile avviare la connessione desktop remoto direttamente dalla schermata iniziale per selezionarlo.

> [!NOTE]
> Se si rinomina la connessione di desktop nell'applicazione Desktop remoto, non si aggiornerà l'etichetta di questo bloccati desktop remoto.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Connettersi a un Gateway Desktop remoto per accedere alle risorse interne

Gateway Desktop remoto (Gateway Desktop remoto) consente di connettersi a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. È possibile creare e gestire il gateway tramite il client Desktop remoto.

Per impostare un nuovo gateway:

1. Nel Centro connessioni, toccare **Impostazioni > gateway**. Toccare **+** per aggiungere un nuovo gateway.
2. Immettere le informazioni seguenti:
   - **Nome del server** : il nome del computer di cui si desidera utilizzare come gateway. Può trattarsi di un nome di computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del server (ad esempio: **RDGateway:443** oppure **10.0.0.1: 443**).
   - **Nome utente** -il nome utente e la password da utilizzare per il Gateway Desktop remoto si connette. È inoltre possibile selezionare **utilizzare account utente desktop** per utilizzare le stesse credenziali utilizzate per la connessione desktop remoto.

## <a name="manage-your-user-accounts"></a>Gestire gli account utente

Quando ci si connette a una risorsa remota o desktop, è possibile salvare gli account utente per selezionare nuovamente. È inoltre possibile definire gli account utente nel client, anziché salvare i dati utente quando ci si connette a un desktop.

Per creare un nuovo account utente:

1. Nel Centro connessioni, toccare **impostazioni**, e quindi toccare **gli account utente**.
2. Toccare **+** per aggiungere un nuovo account utente.
3. Immettere le informazioni seguenti:
   - **Nome utente** -il nome dell'utente da salvare per l'utilizzo con una connessione remota. È possibile immettere il nome utente in uno dei seguenti formati: nome_utente, DOMINIO\nome_utente., o user_name@domain.com.
   - **Password** -la password per l'utente specificato. Ogni account utente che si desidera salvare da utilizzare per le connessioni remote deve avere una password associata.
4. Toccare **salvare**.

Per eliminare un account utente:

1. Nel Centro connessioni, toccare **Impostazioni > account utente**.
2. Toccare e tenere un account utente nell'elenco per selezionarlo. È possibile selezionare più utenti.
3. Toccare il Cestino per eliminare l'utente selezionato.

## <a name="navigate-the-remote-desktop-session"></a>Passare la sessione Desktop remoto
Quando si avvia una connessione desktop remoto, sono disponibili strumenti che è possibile utilizzare per passare la sessione.

### <a name="start-a-remote-desktop-connection"></a>Avviare una connessione Desktop remoto

1. Toccare la connessione Desktop remoto per avviare la sessione. 
2. Se viene chiesto di verificare il certificato per il desktop remoto, toccare **Connect**. È inoltre possibile selezionare **non visualizzare più questo messaggio per le connessioni al computer** sempre accettare il certificato.

### <a name="manage-global-app-settings"></a>Gestire le impostazioni dell'app globale

È possibile impostare le impostazioni globali seguenti nel client Android:

- **Mostra le anteprime Desktop** -consente di visualizzare un'anteprima del desktop nel Centro connessioni prima di connettersi a esso. Per impostazione predefinita, questo è impostato **su**.
- **Avvicinare le dita per eseguire lo Zoom** -consente di usare i movimenti zoom con avvicinamento delle dita. Se l'app in uso tramite Desktop remoto supporta Multi-touch (introdotta in Windows 8), disabilitare questa impostazione **disattivata**.
- **Guida per migliorare Desktop remoto** -invia dati anonimi a Microsoft. Usiamo i dati per migliorare il client. È possibile altre informazioni su come vengono gestiti questi dati anonimi, privati, vedere la [informativa sulla Privacy di Remote Desktop Client](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). Per impostazione predefinita, questa impostazione è **su**.
- **Visualizzare** -sono disponibili due impostazioni globali per la visualizzazione:
  - **Orientamento** -imposta l'orientamento preferito (orizzontale o verticale) per la sessione. 
    >[!NOTE]
    > Se ci si connette a un PC che eseguono Windows 8 o una versione precedente di Windows, la sessione non scala correttamente. La soluzione migliore consiste nel disconnettersi dal PC e quindi riconnettere l'orientamento da usare. Un'opzione migliore consiste nell'aggiornare il PC per almeno di Windows 8.1.

  - **Risoluzione** -imposta la risoluzione da usare per le connessioni desktop a livello globale. Se è già stata impostata una risoluzione personalizzata per una singola app o la connessione, questa impostazione non verrà modificato che.
    >[!NOTE]
    >Quando si modifica una delle impostazioni di visualizzazione, sono applicabili unicamente alle nuove connessioni da quel punto in. Per visualizzare le modifiche in una sessione di cui che si è già connessi per disconnettersi e quindi connettersi nuovamente.

### <a name="connection-bar"></a>Barra delle connessioni

Consente di barra di connessione di accedere ai controlli di spostamento aggiuntive. Per impostazione predefinita, la barra delle connessioni viene posizionata al centro nella parte superiore della schermata. Un doppio tocco e trascinare la barra verso sinistra o a destra per spostarlo.

- **Pan in controllo**: Il controllo zoom consente la schermata di ingrandimento e spostati. Si noti che il controllo zoom è disponibile esclusivamente tramite tocco diretto.
   - Abilitare / disabilitare il controllo dettaglio: Toccare l'icona Zoom nella barra di connessione per visualizzare il controllo di panoramica e zoom schermata. Toccare l'icona Zoom nella barra di connessione per nascondere il controllo e restituire la schermata per la risoluzione originale.
   - Usare il controllo dettaglio: Toccare e tenere premuto il controllo dettaglio e quindi trascinare nella direzione per spostare la schermata.
   - Spostare il controllo dettaglio: Doppie toccare e tenere premuto il controllo dettaglio per spostare il controllo sullo schermo.
- **Opzioni aggiuntive**: Toccare l'icona delle opzioni aggiuntive per visualizzare la selezione di sessione a barre e comando della barra (vedere sotto).
- **Tastiera**: Toccare l'icona di tastiera per visualizzare o nascondere la tastiera. Il controllo dettaglio viene visualizzato automaticamente quando viene visualizzata la tastiera.
- **Spostare la barra delle connessioni**: Toccare e tenere premuto la barra delle connessioni, quindi trascinare e rilasciare una nuova posizione nella parte superiore della schermata.


### <a name="command-bar"></a>Barra dei comandi

Toccare la barra delle connessioni per visualizzare la barra dei comandi sul lato destro dello schermo. È possibile passare tra le modalità del mouse (tocco diretto e il puntatore del Mouse). Utilizzare il pulsante home per tornare al centro di connessione nella barra dei comandi. In alternativa è possibile utilizzare il pulsante Indietro per la stessa azione. La sessione attiva non verrà disconnesso. 


### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Utilizzo diretto touch movimenti e le modalità del mouse in una sessione remota

Il client utilizza i movimenti tocco standard. È inoltre possibile utilizzare i movimenti tocco per replicare le azioni del mouse sul desktop remoto. Le modalità mouse disponibili sono definite nella tabella seguente.

> [!NOTE]
> Interazione con Windows 8 o versioni successive i movimenti di tocco nativo sono supportati in modalità tocco diretto. 

| Modalità mouse    | Operazione con il mouse      | Movimento                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Tocco diretto con  | A sinistra fare clic su           | scelta di un 1 dito                                                          |
| Tocco diretto con  | Fare clic          | 1 tap dito e tenere premuto                                                 |
| Puntatore del mouse | Zoom                 | Utilizzare le 2 DITA e avvicinare le dita per eseguire lo zoom avanti o spostare divide in due dita per eseguire lo zoom indietro. |
| Puntatore del mouse | A sinistra fare clic su           | scelta di un 1 dito                                                          |
| Puntatore del mouse | Fare clic e trascinare  | 1 dito doppie toccare e tenere premuto, quindi trascinare.                               |
| Puntatore del mouse | Fare clic          | scelta di un dito 2                                                          |
| Puntatore del mouse | Fare clic e trascinare | 2 il doppio tocco con un dito e tenere premuto, quindi trascinare.                               |
| Puntatore del mouse | Rotellina del mouse          | 2 dito toccare e tenere premuto, quindi trascinare verso l'alto o verso il basso                           |

> [!TIP]
> Domande e commenti sono sempre Benvenuti. Tuttavia, NON inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, andare alla [forum su client di Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Avete suggerimenti funzionalità? Comunicaci nel [forum dedicato agli utenti di client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
