---
title: Introduzione al client Android
description: Informazioni generali sul client Android.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 03/12/2020
ms.localizationpriority: medium
ms.openlocfilehash: d4ef08107c816aebf6563e57e5e76b12b793d472
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323423"
---
# <a name="get-started-with-the-android-client"></a>Introduzione al client Android

>Si applica a: Android 4.1 e versioni successive

Il client Desktop remoto per Android ti consente di usare desktop e app di Windows direttamente da un dispositivo Android o da un Chromebook che supporta Google Play Store.

Segui le informazioni riportate di seguito per iniziare. In caso di dubbi, vedi le [domande frequenti](remote-desktop-client-faq.md).

> [!NOTE]
> - Vuoi informazioni sulle nuove versioni per il client Android? Vedi [Novità del client Android](android-whatsnew.md).
> - Il client Android supporta i dispositivi che eseguono Android 4.1 e versioni successive, nonché Chromebook con ChromeOS 53 e versioni successive. Altre informazioni sulle applicazioni Android in Chrome sono disponibili [qui](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="set-up-the-remote-desktop-client-for-android"></a>Configurare il client Desktop remoto per Android

### <a name="download-the-remote-desktop-client-from-the-google-play-store"></a>Scarica il client Desktop remoto da Google Play Store.

Di seguito viene illustrato come configurare il client Desktop remoto in un dispositivo Android:

1. Scarica il client Desktop remoto Microsoft da [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android).
2. Avvia il **client Desktop remoto** dall'elenco di app.
3. Aggiungi una [connessione Desktop remoto](#add-a-remote-desktop-connection) o [risorse remote](#add-remote-resources). Una connessione ti consente di connetterti direttamente a un PC Windows, mentre le risorse remote ti consentono di accedere alle app e ai desktop pubblicati da un amministratore.

> [!NOTE]
> Se vuoi testare le nuove funzionalità prima del rilascio, ti consigliamo di scaricare il client [Microsoft Remote Desktop Beta](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) da Google Play Store.

### <a name="add-a-remote-desktop-connection"></a>Aggiungere una connessione Desktop remoto

Se non hai già provveduto, [configura il PC in modo che accetti le connessioni remote](remote-desktop-allow-access.md).

Per creare una connessione Desktop remoto:

1. Nel Centro connessioni tocca **+** e quindi **Desktop**.
2. Immetti il nome del PC remoto in **Nome PC**. Puoi usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere al nome del PC informazioni relative alla porta, ad esempio MyDesktop:3389 o 10.0.0.1:3389. Questo è l'unico campo obbligatorio.
3. Seleziona il valore di **Nome utente** usato per accedere al PC remoto.
   - Seleziona **Immetti ogni volta** per fare in modo che il client chieda le credenziali ogni volta che ti connetti al PC remoto.
   - Seleziona **Add User Account** (Aggiungi account utente) per salvare un account usato di frequente, evitando così di dover immettere le credenziali ogni volta che esegui l'accesso. Per altri dettagli, vedi [Gestire gli account utente](#manage-your-user-accounts).
4. Puoi anche toccare **Show additional options** (Mostra opzioni aggiuntive) per impostare i parametri facoltativi seguenti:
   - In **Nome descrittivo** puoi immettere un nome facile da ricordare per il PC a cui vuoi connetterti. Se non specifichi un nome descrittivo, verrà visualizzato il nome del PC.
   - **Gateway** corrisponde al gateway Desktop remoto che userai per connetterti a un computer da una rete esterna. Per altre informazioni, contatta l'amministratore di sistema.
   - **Sound** (Audio) consente di selezionare il dispositivo usato dalla sessione remota per l'audio. Puoi scegliere di riprodurre l'audio nel dispositivo locale, nel dispositivo remoto oppure di non riprodurlo affatto.
   - **Customize display resolution** (Personalizza risoluzione schermo) consente di impostare la risoluzione per la sessione remota. Se questa opzione è disattivata, verrà usata la risoluzione specificata nelle impostazioni globali.
   - **Scambia pulsanti del mouse** consente di scambiare i comandi inviati tramite movimenti del mouse verso destra e sinistra. Ideale per gli utenti mancini.
   - **Connettiti alla sessione amministratore** ti consente di connetterti a una sessione di amministrazione nel PC remoto.
   - **Redirect local storage** (Reindirizza archiviazione locale) consente il reindirizzamento dell'archiviazione locale. Questa opzione è disabilitata per impostazione predefinita.
5. Al termine, tocca **Salva**.

È necessario modificare queste impostazioni? Tocca il menu **Altre opzioni** ( **...** ) accanto al nome del desktop e quindi **Modifica**.

Vuoi rimuovere la connessione? Tocca di nuovo il menu **Altre opzioni** ( **...** ) e quindi **Rimuovi**.

>[!TIP]
> Se viene visualizzato l'errore 0xf07 relativo a una password errata ("La connessione al PC remoto non è riuscita perché la password associata all'account utente è scaduta"), modifica la password e riprova.

### <a name="add-remote-resources"></a>Aggiungere risorse remote

Le risorse remote sono programmi RemoteApp, desktop basati su sessione e desktop virtuali pubblicati dall'amministratore. Il client Android supporta le risorse pubblicate dalle distribuzioni di **Servizi Desktop remoto** e **Desktop virtuale Windows**. Per aggiungere risorse remote:

1. In Connection Center (Centro connessioni) tocca **+** e quindi **Remote Resource Feed** (Feed risorse remote).
2. Immetti un valore in **Feed URL** (URL del feed). Può essere un URL o un indirizzo e-mail:
   - Il valore di **URL** corrisponde al server di Accesso Web Desktop remoto fornito dall'amministratore. Se accedi alle risorse da Desktop virtuale Windows, puoi usare `https://rdweb.wvd.microsoft.com`.
   - Se prevedi di usare **Email**, immetti l'indirizzo e-mail in questo campo. In questo modo indichi al client di cercare un server di Accesso Web Desktop remoto associato all'indirizzo e-mail, se configurato dall'amministratore.
3. Tocca **Next** (Avanti).
4. Immetti le informazioni di accesso, quando richiesto. Queste informazioni possono variare in base alla distribuzione e possono includere:
   - **Nome utente**: il nome dell'utente autorizzato ad accedere alle risorse.
   - **Password**: la password associata al nome utente.
   - **Additional factor** (Fattore aggiuntivo): il fattore che potrebbe essere richiesto a seconda della modalità di autenticazione configurata dall'amministratore.
5. Al termine, tocca **Salva**.

Verranno visualizzate nel Centro connessioni di risorse remote.

Per Rimuovere le risorse remote:

1. Nel Centro connessioni, toccare il menu di overflow ( **...** ) accanto alla risorsa remota.
2. Toccare **rimuovere**.
3. Conferma la rimozione.

### <a name="use-a-widget-to-pin-a-saved-desktop-to-your-home-screen"></a>Usare un widget per aggiungere un desktop salvato alla schermata iniziale

Il client Desktop remoto supporta l'aggiunta di connessioni alla schermata iniziale tramite la funzionalità widget di Android. La procedura per aggiungere un widget dipende dal tipo di dispositivo Android in uso e al sistema operativo. Ecco il modo più comune per aggiungere un widget:

1. Tocca **App** per avviare il menu delle app.
2. Tocca **Widget**.
3. Scorri tra i widget e cerca l'icona Desktop remoto con la descrizione seguente: "Pin Remote Desktop" (Aggiungi Desktop remoto).
4. Tenendo premuta widget Desktop remoto e spostarlo sullo schermo principale.
5. Rilasciando l'icona, visualizzerai i desktop remoti salvati. Scegliere la connessione che si desidera salvare nella schermata iniziale.

Ora è possibile avviare la connessione desktop remoto direttamente dalla schermata iniziale per selezionarlo.

> [!NOTE]
> Se rinomini la connessione desktop nel client Desktop remoto, l'etichetta aggiunta non verrà aggiornata.

## <a name="manage-general-app-settings"></a>Gestire le impostazioni delle app generali

Per modificare le impostazioni generali dell'app, vai a Connection Center (Centro connessioni), tocca **Impostazioni** e quindi **Generale**.

Puoi definire le impostazioni generali seguenti:

- **Show Desktop Previews** (Mostra anteprime desktop): ti consente di visualizzare un'anteprima di un desktop nel Centro connessioni prima della connessione. Questa opzione è attivata per impostazione predefinita.
- **Pinch to zoom remote session** (Avvicina le dita per eseguire lo zoom della sessione remota): ti consente di usare i movimenti di avvicinamento delle dita per fare lo zoom. Se l'app che usi tramite Desktop remoto supporta il multitouch (introdotto in Windows 8), disabilita questa funzionalità.
- Abilita **Use scancode input when available** (Usa input codice tasto quando disponibile) se l'app remota non risponde correttamente all'input da tastiera inviato come codice tasto. Se questa opzione è disabilitata, l'input viene inviato come Unicode.
- **Help improve Remote Desktop** (Contribuisci al miglioramento di Desktop remoto): invia dati anonimi a Microsoft relativi al modo con cui usi Desktop remoto per Android. Questi dati vengono usati per migliorare il client. Per altre informazioni sull'informativa sulla privacy e sui tipi di dati raccolti, vedi l'[Informativa sulla privacy di Microsoft](https://privacy.microsoft.com/privacystatement). Questa opzione è attivata per impostazione predefinita.

## <a name="manage-display-settings"></a>Gestire le impostazioni dello schermo

Per modificare le impostazioni dello schermo, tocca **Impostazioni** e quindi **Schermo** nel Centro connessioni.

Puoi definire le impostazioni dello schermo seguenti:

- **Orientamento**: consente di impostare l'orientamento preferito (orizzontale o verticale) per la sessione.
  
  >[!NOTE]
  > Se ti connetti a un PC che esegue Windows 8 o una versione precedente, la sessione non verrà ridimensionata correttamente se cambia l'orientamento del dispositivo. Per un corretto ridimensionamento del client, disconnettiti dal PC e quindi riconnettiti con l'orientamento desiderato. Puoi garantire un corretto ridimensionamento anche usando un PC con Windows 10.

- **Risoluzione**: consente di impostare la risoluzione remota da usare per le connessioni desktop a livello globale. Se hai già impostato una risoluzione personalizzata per una singola connessione, questa impostazione non modificherà tale risoluzione.
  
  >[!NOTE]
  >Quando modifichi le impostazioni dello schermo, tali modifiche si applicano solo alle nuove connessioni effettuate dopo la modifica dell'impostazione. Per applicare le modifiche alla sessione a cui sei attualmente connesso, aggiorna la sessione disconnettendoti e riconnettendoti.

## <a name="manage-your-rd-gateways"></a>Gestire Gateway Desktop remoto

Gateway Desktop remoto ti consente di connetterti a un computer remoto in una rete privata da qualsiasi posizione su Internet. È possibile creare e gestire il gateway tramite il client Desktop remoto.

Per configurare un nuovo Gateway Desktop remoto:

1. Nel Centro connessioni tocca **Impostazioni** e quindi **Gateways** (Gateway).
2. Toccare **+** per aggiungere un nuovo gateway.
3. Immettere le informazioni seguenti:
   - Immetti il nome del computer da usare come gateway in **Nome server**. Puoi usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere informazioni sulla porta al nome del server (ad esempio: RDGateway:443 o 10.0.0.1:443).
   - Seleziona l'**account utente** che userai per accedere a Gateway Desktop remoto.
     - Seleziona **Use desktop user account** (Usa account utente desktop) per usare le stesse credenziali specificate per il PC remoto.
     - Seleziona **Add User Account** (Aggiungi account utente) per salvare un account usato di frequente, evitando così di dover immettere le credenziali ogni volta che esegui l'accesso. Per altre informazioni, vedi [Gestire gli account utente](#manage-your-user-accounts).

Per eliminare un Gateway Desktop remoto:

1. Nel Centro connessioni tocca **Impostazioni** e quindi **Gateways** (Gateway).
2. Toccare e tenere un gateway nell'elenco per selezionarlo. Puoi selezionare più gateway contemporaneamente.
3. Toccare il Cestino per eliminare il gateway selezionato.

## <a name="manage-your-user-accounts"></a>Gestire gli account utente

Puoi salvare gli account utente da usare ogni volta che ti connetti a un desktop remoto o a risorse remote.

Per salvare un account utente:

1. Nel Centro connessioni, toccare **impostazioni**, e quindi toccare **gli account utente**.
2. Toccare **+** per aggiungere un nuovo account utente.
3. Immettere le informazioni seguenti:
   - **Nome utente**: il nome utente da salvare per l'uso con una connessione remota. Puoi immettere il nome utente in uno dei formati seguenti: user_name, domain\user_name o user_name@domain.com.
   - **Password**: la password per l'utente specificato. Ogni account utente che si desidera salvare da utilizzare per le connessioni remote deve avere una password associata.
4. Al termine, tocca **Salva**.

Per eliminare un account utente salvato:

1. Nel Centro connessioni, toccare **impostazioni**, e quindi toccare **gli account utente**.
2. Toccare e tenere un account utente nell'elenco per selezionarlo. Puoi selezionare più utenti contemporaneamente.
3. Toccare il Cestino per eliminare l'utente selezionato.

## <a name="navigate-the-remote-desktop-session"></a>Esplorare la sessione Desktop remoto

Ecco una breve introduzione su come aprire ed esplorare la sessione di Desktop remoto.

### <a name="start-a-remote-desktop-connection"></a>Avviare una connessione Desktop remoto

1. Tocca **il nome della connessione Desktop remoto** per avviare la sessione.
2. Se viene chiesto di verificare il certificato per il desktop remoto, tocca **Connetti**. Puoi anche selezionare **Non visualizzare più questo messaggio per le connessioni a questo computer** per accettare il certificato per impostazione predefinita.

### <a name="connection-bar"></a>Barra delle connessioni

Consente di barra di connessione di accedere ai controlli di spostamento aggiuntive. Per impostazione predefinita, la barra delle connessioni viene posizionata al centro nella parte superiore della schermata. Trascina la barra verso sinistra o destra per spostarla.

- **Controllo panoramica**: il controllo panoramica consente di ingrandire e spostare la schermata. Il controllo panoramica è disponibile esclusivamente tramite tocco diretto.
  - Per mostrare il controllo panoramica, tocca l'icona corrispondente sulla barra delle connessioni per visualizzare il controllo panoramica e ingrandire la schermata. Tocca di nuovo l'icona panoramica per nascondere il controllo e ripristinare le dimensioni originali della schermata.
  - Per usare il controllo panoramica, tienilo premuto e quindi trascinalo nella direzione in cui vuoi spostare la schermata.
  - Per spostare il controllo panoramica, effettua un doppio tocco su di esso e tienilo premuto per spostare il controllo sullo schermo.
- **Opzioni aggiuntive**: tocca l'icona Opzioni aggiuntive per visualizzare la barra di selezione della sessione e la barra dei comandi.
- **Tastiera**: tocca l'icona della tastiera per visualizzare o nascondere la tastiera. Il controllo dettaglio viene visualizzato automaticamente quando viene visualizzata la tastiera.

### <a name="session-selection-bar"></a>Barra di selezione di sessione

È possibile avere più connessioni aperti per più computer contemporaneamente. Toccare la barra delle connessioni per visualizzare la barra di selezione di sessione sul lato sinistro della schermata. La barra di selezione di sessione ti consente di visualizzare le connessioni aperte e di passare da una all'altra.

Quando sei connesso a risorse remote, puoi passare da un'app all'altra all'interno di tale sessione toccando il menu di espansione ( **>** ) e scegliendo un elemento dall'elenco di elementi disponibili.

Per avviare una nuova sessione nell'ambito della connessione corrente, tocca **Avvia nuova** e quindi scegli un elemento dall'elenco degli elementi disponibili.

Per disconnettere una sessione, tocca **X** sul lato sinistro del riquadro della sessione.

### <a name="command-bar"></a>Barra dei comandi

Toccare la barra delle connessioni per visualizzare la barra dei comandi sul lato destro dello schermo. Sulla barra dei comandi puoi passare da una modalità del mouse all'altra (tocco diretto e puntatore del mouse) o toccare il pulsante Home per tornare al Centro connessioni. Puoi anche toccare il pulsante Indietro per tornare al Centro connessioni. Se torni al Centro connessioni, non verrà disconnessa la sessione attiva.

### <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usare i movimenti tocco e le modalità del mouse in una sessione remota

Il client utilizza i movimenti tocco standard. È inoltre possibile utilizzare i movimenti tocco per replicare le azioni del mouse sul desktop remoto. La tabella seguente illustra i movimenti corrispondenti alle azioni del mouse in ogni modalità del mouse.

> [!NOTE]
> I movimenti di tocco nativo sono supportati in modalità tocco diretto in Windows 8 o versioni successive.

| Modalità mouse    | Azione del mouse         | Movimento                                                                 |
|---------------|----------------------|-------------------------------------------------------------------------|
| Tocco diretto con  | Clic con il pulsante sinistro           | Tocco con un dito                                                     |
| Tocco diretto con  | Fare clic con il pulsante destro del mouse su          | Tocco e pressione con un dito, quindi rilascio                              |
| Puntatore del mouse | Zoom                 | Avvicinamento di due dita per fare zoom indietro o allontanamento delle dita per fare zoom avanti |
| Puntatore del mouse | Clic con il pulsante sinistro           | Tocco con un dito                                                     |
| Puntatore del mouse | Clic con il pulsante sinistro e trascinamento  | Doppio tocco e pressione con un dito, quindi trascinamento                          |
| Puntatore del mouse | Fare clic con il pulsante destro del mouse su          | Tocco con due dita                                                    |
| Puntatore del mouse | Clic con il pulsante destro del mouse e trascinamento | Doppio tocco e pressione con due dita, quindi trascinamento                         |
| Puntatore del mouse | Rotellina del mouse          | Pressione con due dita, quindi trascinamento verso l'alto o verso il basso                     |

## <a name="join-the-beta-channel"></a>Accedere al canale Beta

Se vuoi accedere alle funzionalità più recenti prima di chiunque altro o vuoi identificare i problemi prima che vengano rilasciate nuove versioni, il canale Beta è la scelta ottimale. Il canale Beta è anche lo strumento ideale per consentire agli amministratori aziendali di convalidare nuove versioni del client Android per gli utenti nel relativo ambiente.

Per accedere al canale Beta, fornisci semplicemente il consenso per accedere alle versioni di anteprima e scarica il client. Riceverai le versioni di anteprima direttamente da Google Play Store.

[Accedi al canale Beta](https://play.google.com/apps/testing/com.microsoft.rdc.androidx)