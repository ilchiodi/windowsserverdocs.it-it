---
title: Introduzione al client Android
description: Passaggi di configurazione di base per il client Desktop remoto per Android.
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
ms.date: 08/27/2019
ms.localizationpriority: medium
ms.openlocfilehash: 1099f1c8bb635ee1b4bb33c6483f128ee0ecdfb9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871237"
---
# <a name="get-started-with-the-android-client"></a>Introduzione al client Android

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Il client Desktop remoto per Android ti consente di usare desktop e app di Windows direttamente da un dispositivo Android.

Attieniti alle informazioni riportate di seguito per iniziare. In caso di dubbi, vedi le [domande frequenti](remote-desktop-client-faq.md).

> [!NOTE]
> - Vuoi informazioni sulle nuove versioni per il client Android? Vedi [Novità di Desktop remoto in Android](android-whatsnew.md).
> Puoi eseguire il client in Android 4.1 e nei dispositivi più recenti, nonché in dispositivi Chromebook con ChromeOS 53 installato. Altre informazioni sulle applicazioni Android in Chrome sono disponibili [qui](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="get-the-rd-client-and-start-using-it"></a>Ottenere il client Desktop remoto e iniziare a usarlo

Segui questa procedura per iniziare a usare Desktop remoto in un dispositivo Android:

1. Scarica il client Desktop remoto da [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configura il PC per accettare le connessioni remote](remote-desktop-allow-access.md).
3. Aggiungi una connessione Desktop remoto o una risorsa remota. Una connessione ti consente di connetterti direttamente a un PC Windows, mentre una risorsa remota ti permette di usare un programma RemoteApp, un desktop basato su sessione o un desktop virtuale pubblicato in locale. 
4. Crea un widget per poter accedere rapidamente a Desktop remoto.

> [!NOTE]
> Se vuoi provare le nuove funzionalità in anteprima, ti consigliamo di scaricare l'applicazione [Microsoft Remote Desktop Beta](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) da Google Play. 

### <a name="add-a-remote-desktop-connection"></a>Aggiungere una connessione Desktop remoto

Per creare una connessione Desktop remoto:

1. Nella scelta dei Centro connessioni **+** , e quindi toccare **Desktop**.
2. Immettere le informazioni seguenti per il computer si desidera connettersi:
   - **Nome PC** : il nome del computer. Puoi usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del computer (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Nome utente** : il nome utente da utilizzare per accedere al computer remoto. Puoi usare i formati seguenti: *user_name*, *domain\user_name* oppure <em>user_name@domain.com</em>. È inoltre possibile specificare se per la richiesta di un nome utente e una password.
3. È inoltre possibile impostare le opzioni aggiuntive seguenti:
   - **Nome descrittivo** : un nome facile da ricordare per il computer si è connessi. È possibile utilizzare qualsiasi stringa, ma se non si specifica un nome descrittivo, viene visualizzato il nome del computer.
   - **Gateway** – gateway Desktop remoto che si desidera utilizzare per connettersi a desktop virtuali, programmi RemoteApp e desktop basati su sessione su una rete aziendale interna. Ottenere le informazioni sul gateway, dall'amministratore di sistema.
    È necessario configurare un Gateway Desktop remoto?
   - **Suono** : selezionare il dispositivo da utilizzare per l'audio durante la sessione remota. È possibile scegliere di riprodurre un suono nei dispositivi locali, il dispositivo remoto oppure non funziona.
   - **Customize display resolution** (Personalizza risoluzione schermo): abilita questa impostazione per impostare una risoluzione personalizzata per una connessione. Quando l'opzione è disattivata, viene applicata la risoluzione definita nelle impostazioni globali dell'app.
   - **Scambia i pulsanti del mouse** : utilizzare questa opzione per invertire il puntatore del mouse a sinistra pulsante funzioni per il pulsante destro del mouse. (Ciò è particolarmente utile se sul computer remoto è configurato per un utente da ma si utilizza un mouse da destra.)
   - **Connettiti alla sessione amministrativa**: usa questa opzione per connetterti a una sessione console per l'amministrazione di un server Windows.
   - **Reindirizzare l'archiviazione locale** – Collega l'archiviazione locale come un file system remoto nel computer remoto.
4. Toccare **salvare**.

È necessario modificare queste impostazioni? Toccare il menu di overflow ( **...** ) accanto al nome del desktop e quindi toccare **modificare**.

Se si desidera eliminare la connessione? Nuovamente, toccare il menu di overflow ( **...** ), quindi toccare **rimuovere**.

>[!TIP]
> Se viene visualizzato l'errore 0xf07 relativo a una password errata ("La connessione al PC remoto non è riuscita perché la password associata all'account utente è scaduta"), modifica la password e riprova.

### <a name="add-a-remote-resource"></a>Aggiungere una risorsa remota
Risorse remote sono programmi RemoteApp, desktop basati su sessione e i desktop virtuali pubblicati tramite connessione RemoteApp e Desktop.

Per aggiungere una risorsa remota:

1. Nella schermata connessione Center toccare **+** , e quindi toccare **Feed risorse Remote**. 
2. Immetti le informazioni per la risorsa remota:
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
3. Scorri tra i widget e cerca l'icona di Desktop remoto con la descrizione "Pin Remote Desktop" (Blocca Desktop remoto).
4. Tenendo premuta widget Desktop remoto e spostarlo sullo schermo principale.
5. Rilasciando l'icona, visualizzerai i desktop remoti salvati. Scegliere la connessione che si desidera salvare nella schermata iniziale.

Ora è possibile avviare la connessione desktop remoto direttamente dalla schermata iniziale per selezionarlo.

> [!NOTE]
> Se si rinomina la connessione di desktop nell'applicazione Desktop remoto, non si aggiornerà l'etichetta di questo bloccati desktop remoto.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Stabilire la connessione a Gateway Desktop remoto per accedere alle risorse interne

Gateway Desktop remoto ti consente di connetterti a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. È possibile creare e gestire il gateway tramite il client Desktop remoto.

Per impostare un nuovo gateway:

1. Nel Centro connessioni, toccare **Impostazioni > gateway**. Toccare **+** per aggiungere un nuovo gateway.
2. Immettere le informazioni seguenti:
   - **Nome del server** : il nome del computer di cui si desidera utilizzare come gateway. Puoi usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere informazioni sulla porta al nome del server (ad esempio: **RDGateway:443** o **10.0.0.1:443**).
   - **Nome utente** -il nome utente e la password da utilizzare per il Gateway Desktop remoto si connette. È inoltre possibile selezionare **utilizzare account utente desktop** per utilizzare le stesse credenziali utilizzate per la connessione desktop remoto.

## <a name="manage-your-user-accounts"></a>Gestire gli account utente

Quando ci si connette a una risorsa remota o desktop, è possibile salvare gli account utente per selezionare nuovamente. È inoltre possibile definire gli account utente nel client, anziché salvare i dati utente quando ci si connette a un desktop.

Per creare un nuovo account utente:

1. Nel Centro connessioni, toccare **impostazioni**, e quindi toccare **gli account utente**.
2. Toccare **+** per aggiungere un nuovo account utente.
3. Immettere le informazioni seguenti:
   - **Nome utente** -il nome dell'utente da salvare per l'utilizzo con una connessione remota. Puoi immettere il nome utente in uno dei formati seguenti: user_name, domain\user_name o user_name@domain.com.
   - **Password** -la password per l'utente specificato. Ogni account utente che si desidera salvare da utilizzare per le connessioni remote deve avere una password associata.
4. Toccare **salvare**.

Per eliminare un account utente:

1. Nel Centro connessioni, toccare **Impostazioni > account utente**.
2. Toccare e tenere un account utente nell'elenco per selezionarlo. È possibile selezionare più utenti.
3. Toccare il Cestino per eliminare l'utente selezionato.

## <a name="navigate-the-remote-desktop-session"></a>Esplorare la sessione Desktop remoto
Quando si avvia una connessione desktop remoto, sono disponibili strumenti che è possibile utilizzare per passare la sessione.

### <a name="start-a-remote-desktop-connection"></a>Avviare una connessione Desktop remoto

1. Tocca la connessione Desktop remoto per avviare la sessione. 
2. Se viene chiesto di verificare il certificato per il desktop remoto, toccare **Connect**. Puoi anche selezionare **Non visualizzare più questo messaggio per le connessioni a questo computer** per accettare sempre il certificato.

### <a name="manage-global-app-settings"></a>Gestire le impostazioni delle app globali

Nel client Android puoi definire le impostazioni globali seguenti:

- **Mostra anteprime desktop**: consente di visualizzare un'anteprima di un desktop nel centro connessioni prima della connessione. Per impostazione predefinita, questa opzione è **attivata**.
- **Avvicina le dita per eseguire lo zoom**: consente di usare il movimento di avvicinamento delle dita per eseguire lo zoom. Se l'app in uso tramite Desktop remoto supporta il multitouch (introdotto in Windows 8), **disattiva** questa impostazione.
- **Info per migliorare il Desktop remoto**: invia dati anonimi a Microsoft. Questi dati vengono usati per migliorare il client. Per altre informazioni su come vengono trattati questi dati anonimi e privati, vedi l'[Informativa sulla privacy del client Desktop remoto](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). Per impostazione predefinita, questa opzione è **attivata**.
- **Schermo**: per lo schermo sono disponibili due impostazioni globali:
  - **Orientamento**: imposta l'orientamento preferito (orizzontale o verticale) per la sessione. 
    >[!NOTE]
    > Se ti connetti a un PC che esegue Windows 8 o una versione precedente di Windows, la sessione non verrà ridimensionata correttamente. La soluzione migliore consiste nel disconnetterti dal PC e quindi riconnetterti con l'orientamento desiderato. L'ideale sarebbe aggiornare il PC almeno a Windows 8.1.

  - **Risoluzione**: imposta la risoluzione da usare per le connessioni desktop a livello globale. Se hai già impostato una risoluzione personalizzata per una singola app o per la connessione, questa impostazione non modificherà tale risoluzione.
    >[!NOTE]
    >Quando modifichi una delle impostazioni di visualizzazione, le nuove impostazioni vengono applicate solo alle nuove connessioni stabilite. Per visualizzare la modifica in una sessione a cui hai già stabilito la connessione, disconnettiti e quindi connettiti di nuovo.

### <a name="connection-bar"></a>Barra delle connessioni

Consente di barra di connessione di accedere ai controlli di spostamento aggiuntive. Per impostazione predefinita, la barra delle connessioni viene posizionata al centro nella parte superiore della schermata. Un doppio tocco e trascinare la barra verso sinistra o a destra per spostarlo.

- **Controllo panoramica**: il controllo panoramica consente di ingrandire e spostare la schermata. Si noti che il controllo zoom è disponibile esclusivamente tramite tocco diretto.
   - Abilitare o disabilitare il controllo panoramica: tocca l'icona panoramica sulla barra delle connessioni per visualizzare il controllo panoramica e ingrandire la schermata. Toccare l'icona Zoom nella barra di connessione per nascondere il controllo e restituire la schermata per la risoluzione originale.
   - Usare il controllo panoramica: tieni premuto il controllo panoramica e quindi trascina nella direzione in cui vuoi spostare la schermata.
   - Spostare il controllo panoramica: tocca due volte e tieni premuto il controllo panoramica per spostarlo sullo schermo.
- **Opzioni aggiuntive**: tocca l'icona Opzioni aggiuntive per visualizzare la barra di selezione della sessione e la barra dei comandi (vedi sotto).
- **Tastiera**: tocca l'icona della tastiera per visualizzare o nascondere la tastiera. Il controllo dettaglio viene visualizzato automaticamente quando viene visualizzata la tastiera.
- **Spostare la barra delle connessioni**: tieni premuta la barra delle connessioni e quindi trascinala in una nuova posizione nella parte superiore dello schermo.


### <a name="command-bar"></a>Barra dei comandi

Toccare la barra delle connessioni per visualizzare la barra dei comandi sul lato destro dello schermo. È possibile passare tra le modalità del mouse (tocco diretto e il puntatore del Mouse). Utilizzare il pulsante home per tornare al centro di connessione nella barra dei comandi. In alternativa è possibile utilizzare il pulsante Indietro per la stessa azione. La sessione attiva non verrà disconnesso. 


### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usare i movimenti tocco diretto e le modalità del mouse in una sessione remota

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
