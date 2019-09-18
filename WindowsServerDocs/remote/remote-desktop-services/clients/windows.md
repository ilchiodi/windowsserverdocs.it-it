---
title: Introduzione al client Windows Store
description: Passaggi di installazione di base per il client Desktop remoto per Windows Store.
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
ms.openlocfilehash: 14df478c681074f8081190048fb5579d7fa9f653
ms.sourcegitcommit: 61767c405da44507bd3433967543644e760b20aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/14/2019
ms.locfileid: "70987319"
---
# <a name="get-started-with-the-windows-store-client"></a>Introduzione al client Windows Store

>Si applica a: Windows 10

Il client Desktop remoto per Windows consente di usare desktop e app di Windows in remoto da un altro dispositivo Windows.

Attieniti alle informazioni riportate di seguito per iniziare. In caso di dubbi, vedi le [domande frequenti](remote-desktop-client-faq.md).

> [!NOTE]
> - Se vuoi conoscere le nuove versioni per il client Windows Store, vedi [Novità del client Windows Store](windows-whatsnew.md).
> - Puoi eseguire il client in qualsiasi versione supportata di Windows 10.

## <a name="get-the-rd-client-and-start-using-it"></a>Ottenere il client Desktop remoto e iniziare a usarlo

Segui questa procedura per iniziare a usare Desktop remoto in un dispositivo Windows 10:

1. Scarica il client Desktop remoto da [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configura il PC per accettare le connessioni remote](remote-desktop-allow-access.md).
3. Aggiungi una connessione Desktop remoto o una risorsa remota. Una connessione consente di connettersi direttamente a un PC Windows, mentre una risorsa remota consente di usare un programma RemoteApp, un desktop basato su sessione o un desktop virtuale pubblicato dall'amministratore. 
4. Aggiungi gli elementi in modo da poter accedere rapidamente a Desktop remoto.

### <a name="add-a-remote-desktop-connection"></a>Aggiungere una connessione Desktop remoto

Per creare una connessione Desktop remoto:

1. Nel Centro connessioni tocca **+ Aggiungi** e quindi **Desktop**.
2. Immettere le informazioni seguenti per il computer si desidera connettersi:
   - **Nome PC** : il nome del computer. Puoi usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del computer (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Account utente**: l'account utente da usare per accedere al PC remoto. Tocca **+** per aggiungere un nuovo account oppure seleziona un account esistente. Puoi usare i formati seguenti per il nome utente: *user_name*, *domain\user_name* oppure <em>user_name@domain.com</em>. Puoi anche specificare se visualizzare la richiesta di un nome utente e di una password durante la connessione selezionando **Chiedi conferma ogni volta**.
3. Puoi anche impostare opzioni aggiuntive toccando **Mostra dettagli**:
   - **Nome visualizzato**: un nome facile da ricordare per il computer a cui vuoi connetterti. È possibile utilizzare qualsiasi stringa, ma se non si specifica un nome descrittivo, viene visualizzato il nome del computer.
   - **Gruppo**: specifica un gruppo per trovare più facilmente le connessioni in un secondo momento. Puoi aggiungere un nuovo gruppo toccando **+** oppure puoi selezionarne uno dall'elenco.
   - **Gateway** – gateway Desktop remoto che si desidera utilizzare per connettersi a desktop virtuali, programmi RemoteApp e desktop basati su sessione su una rete aziendale interna. Ottenere le informazioni sul gateway, dall'amministratore di sistema.
   - **Connettiti alla sessione amministrativa**: usa questa opzione per connetterti a una sessione console per l'amministrazione di un server Windows.
   - **Scambia i pulsanti del mouse** : utilizzare questa opzione per invertire il puntatore del mouse a sinistra pulsante funzioni per il pulsante destro del mouse. (Ciò è particolarmente utile se sul computer remoto è configurato per un utente da ma si utilizza un mouse da destra.)
   - **Set my remote session resolution to** (Imposta risoluzione sessione remota su): seleziona la risoluzione da usare nella sessione. **Choose for me** (Scegli per me) imposterà la risoluzione in base alla dimensione del client.
   - **Modifica le dimensioni della visualizzazione**: se selezioni un valore alto di risoluzione statica per la sessione, puoi ingrandire la visualizzazione degli elementi sullo schermo per migliorare la leggibilità. Nota: è applicabile solo in caso di connessione a Windows 8.1 o versioni successive.
   - **Update the remote session resolution on resize** (Aggiorna risoluzione sessione remota al ridimensionamento): se questa opzione è abilitata, il client aggiornerà in modo dinamico la risoluzione della sessione in base alla dimensione del client. Nota: è applicabile solo in caso di connessione a Windows 8.1 o versioni successive.
   - **Appunti**: se questa opzione è abilitata, puoi copiare testo e immagini in e da PC remoto.
   - **Riproduzione audio**: seleziona il dispositivo da usare per l'audio durante la sessione remota. Puoi scegliere di riprodurre il suono nei dispositivi locali, nel PC remoto oppure di non riprodurlo affatto.
   - **Registrazione audio**: se questa opzione è abilitata, puoi usare un microfono locale con le applicazioni nel PC remoto.
4. Toccare **salvare**.

È necessario modificare queste impostazioni? Tocca il menu extra ( **...** ) accanto al nome del desktop e quindi **Modifica**.

Se si desidera eliminare la connessione? Tocca di nuovo il menu extra ( **...** ) e quindi **Rimuovi**.

### <a name="add-a-remote-resource"></a>Aggiungere una risorsa remota

Le risorse remote sono programmi RemoteApp, desktop basati su sessione e desktop virtuali pubblicati tramite Servizi Desktop remoto.

Per aggiungere una risorsa remota:

1. Nella schermata del Centro connessioni tocca **+ Aggiungi** e quindi **Risorse remote**.
2. Immetti il valore di **URL feed** fornito dall'amministratore e quindi tocca **Find feeds** (Trova feed).
3. Quando richiesto, fornisci le credenziali da usare per la sottoscrizione al feed.

Verranno visualizzate nel Centro connessioni di risorse remote.

Per eliminare le risorse remote:

1. Nel Centro connessioni, toccare il menu di overflow ( **...** ) accanto alla risorsa remota.
2. Toccare **rimuovere**.

### <a name="pin-a-saved-desktop-to-your-start-menu"></a>Aggiungere un desktop salvato al menu Start

Per aggiungere una connessione al menu Start, tocca il menu extra ( **...** ) accanto al nome del desktop e quindi **Aggiungi a Start**.

Ora puoi avviare la connessione desktop remoto direttamente dal menu Start tramite tocco.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Connettersi a Gateway Desktop remoto per accedere alle risorse interne

Gateway Desktop remoto ti consente di connetterti a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. È possibile creare e gestire il gateway tramite il client Desktop remoto.

Per impostare un nuovo gateway:

1. Nel Centro connessioni tocca **Impostazioni**.
2. Tocca **+** accanto a Gateway per aggiungere un nuovo gateway. Nota: un gateway può essere aggiunto anche quando viene aggiunta una nuova connessione.
3. Immettere le informazioni seguenti:
   - **Nome del server** : il nome del computer di cui si desidera utilizzare come gateway. Puoi usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere informazioni sulla porta al nome del server (ad esempio: **RDGateway:443** oppure **10.0.0.1:443**).
   - **Account utente**: seleziona o aggiungi un account utente da usare con l'istanza di Gateway Desktop remoto a cui vuoi connetterti. È inoltre possibile selezionare **utilizzare account utente desktop** per utilizzare le stesse credenziali utilizzate per la connessione desktop remoto.
4. Toccare **salvare**.  

## <a name="global-app-settings"></a>Impostazioni app globali

Puoi impostare le impostazioni globali seguenti nel client toccando **Impostazioni**:

ELEMENTI GESTITI

- **Account utente**: consente di aggiungere, modificare ed eliminare gli account utente salvati nel client. Questo è un buon metodo per aggiornare la password per un account dopo la modifica.
- **Gateway**: consente di aggiungere, modificare ed eliminare i server gateway salvati nel client.
- **Gruppo**: consente di aggiungere, modificare ed eliminare i gruppi salvati nel client. I gruppi consentono di raggruppare facilmente le connessioni.

IMPOSTAZIONI SESSIONE

- **Start connections in full screen** (Avvia connessioni a schermo intero): se questa opzione è abilitata, ogni volta che viene avviata una connessione, il client userà lo schermo intero del monitor corrente.
- **Start each connection in a new window** (Avvia ogni connessione in una nuova finestra): se questa opzione è abilitata, ogni connessione verrà avviata in una finestra separata, consentendo di posizionare tali elementi su monitor diversi e passare da uno all'altro usando la barra delle applicazioni.
- **When resizing the app** (Al ridimensionamento dell'app): consente di controllare cosa accade quando si ridimensiona la finestra del client. Per impostazione predefinita, questa opzione viene impostata su **Stretch the content, preserving aspect ratio** (Estendi il contenuto mantenendo le proporzioni).
- **Use keyboard commands with** (Usa comandi tastiera con): consente di specificare dove usare i comandi da tastiera *WIN* o *ALT+TAB*. Per impostazione predefinita, vengono inviati alla sessione solo quando la connessione è a schermo intero.
- **Prevent the screen from timing out** (Impedisci timeout schermata): impedisce il timeout della schermata quando è attiva una sessione. Ciò risulta utile se la connessione non richiede alcuna interazione per lunghi periodi di tempo.

IMPOSTAZIONI APP

- **Show Desktop Previews** (Mostra anteprime desktop): consente di visualizzare un'anteprima di un desktop nel Centro connessioni prima della connessione. Per impostazione predefinita, questa opzione è **abilitata**.
- **Help improve Remote Desktop** (Contribuisci al miglioramento di Desktop remoto): invia dati anonimi a Microsoft. Questi dati vengono usati per migliorare il client. Per altre informazioni su come vengono gestiti questi dati anonimi, privati, vedi l'[Informativa sulla privacy Microsoft](https://privacy.microsoft.com/en-us/privacystatement). Per impostazione predefinita, questa opzione è **abilitata**.

### <a name="manage-your-user-accounts"></a>Gestire gli account utente

Quando ci si connette a una risorsa remota o desktop, è possibile salvare gli account utente per selezionare nuovamente. È inoltre possibile definire gli account utente nel client, anziché salvare i dati utente quando ci si connette a un desktop.

Per creare un nuovo account utente:

1. Nel Centro connessioni tocca **Impostazioni**.
2. Accanto ad Account utente tocca **+** per aggiungere un nuovo account utente.
3. Immettere le informazioni seguenti:
   - **Nome utente**: il nome dell'utente da salvare per l'uso con una connessione remota. Puoi immettere il nome utente in uno dei formati seguenti: user_name, domain\user_name o user_name@domain.com.
   - **Password** -la password per l'utente specificato. Può essere lasciata vuota per ricevere la richiesta di immettere una password durante la connessione.
4. Toccare **salvare**.

Per eliminare un account utente:

1. Nel Centro connessioni tocca **Impostazioni**.
2. Seleziona l'account da eliminare dall'elenco in Account utente.
3. Accanto ad Account utente tocca l'icona di modifica.
4. Tocca **Remove this account** (Rimuovi questo account) nella parte inferiore per eliminare l'account utente.
5. Puoi anche modificare l'account utente e toccare **Salva**.

## <a name="navigate-the-remote-desktop-session"></a>Esplorare la sessione Desktop remoto
Quando si avvia una connessione desktop remoto, sono disponibili strumenti che è possibile utilizzare per passare la sessione.

### <a name="start-a-remote-desktop-connection"></a>Avviare una connessione Desktop remoto

1. Tocca la connessione Desktop remoto per avviare la sessione.
2. Se non hai salvato le credenziali per la connessione, verrà chiesto di specificare un valore per **Nome utente** e **Password**.
3. Se viene chiesto di verificare il certificato per il desktop remoto, esamina le informazioni e assicurati che si tratti di un PC attendibile prima di toccare **Connetti**. Puoi anche selezionare **Don't ask about this certificate again** (Non chiedere di nuovo per questo certificato) per accettare sempre questo certificato.

### <a name="connection-bar"></a>Barra delle connessioni

Consente di barra di connessione di accedere ai controlli di spostamento aggiuntive. Per impostazione predefinita, la barra delle connessioni viene posizionata al centro nella parte superiore della schermata. Tocca e trascina la barra verso sinistra o destra per spostarla.

- **Controllo panoramica**: il controllo panoramica consente di ingrandire e spostare la schermata. Questo controllo è disponibile solo in dispositivi abilitati per il tocco e che usano la modalità tocco diretto.
   - Abilitare o disabilitare il controllo panoramica: tocca l'icona panoramica sulla barra delle connessioni per visualizzare il controllo panoramica e ingrandire la schermata. Toccare l'icona Zoom nella barra di connessione per nascondere il controllo e restituire la schermata per la risoluzione originale.
   - Usare il controllo panoramica: tieni premuto il controllo panoramica e quindi trascinalo nella direzione in cui vuoi spostare la schermata.
   - Spostare il controllo panoramica: tocca due volte e tieni premuto il controllo panoramica per spostarlo sullo schermo.
- **Opzioni aggiuntive**: tocca l'icona Opzioni aggiuntive per visualizzare la barra di selezione della sessione e la barra dei comandi (vedi sotto).
- **Tastiera**: tocca l'icona della tastiera per visualizzare o nascondere la tastiera su schermo. Il controllo dettaglio viene visualizzato automaticamente quando viene visualizzata la tastiera.

### <a name="command-bar"></a>Barra dei comandi

Tocca **...** sulla barra delle connessioni per visualizzare la barra dei comandi sul lato destro dello schermo.

- **Home**: usa il pulsante Home per tornare al Centro connessioni dalla barra dei comandi.
  - In alternativa è possibile utilizzare il pulsante Indietro per la stessa azione.
  - La sessione attiva non verrà disconnesso.
  - In questo modo è possibile avviare connessioni aggiuntive.
- **Disconnetti**: usa il pulsante Disconnetti per terminare la connessione.
  - Le app rimarranno attive finché non termina la sessione nel PC remoto.
- **Schermo intero**: attiva o disattiva la modalità schermo intero.
- **Tocco/Mouse**: puoi alternare le modalità del mouse (tocco diretto e puntatore del mouse).

### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usare i movimenti tocco diretto e le modalità del mouse in una sessione remota

Sono disponibili due modalità del mouse per interagire con la sessione.

- **Tocco diretto**: passa tutti i contatti tocco alla sessione per l'interpretazione in modalità remota.
  - Viene usata nello stesso modo in cui si usa Windows con un touchscreen.
- **Puntatore del mouse**: trasforma il touchscreen locale in un grande touchpad consentendo di spostare un puntatore del mouse nella sessione.
  - Viene usata nello stesso modo in cui si usa Windows con un touchpad.

> [!NOTE]
> Interazione con Windows 8 o versioni successive i movimenti di tocco nativo sono supportati in modalità tocco diretto.

| Modalità mouse    | Operazione con il mouse      | Movimento                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Tocco diretto con  | A sinistra fare clic su           | scelta di un 1 dito                                                          |
| Tocco diretto con  | Fare clic          | 1 tap dito e tenere premuto                                                 |
| Puntatore del mouse | A sinistra fare clic su           | scelta di un 1 dito                                                          |
| Puntatore del mouse | Fare clic e trascinare  | 1 dito doppie toccare e tenere premuto, quindi trascinare.                               |
| Puntatore del mouse | Fare clic          | scelta di un dito 2                                                          |
| Puntatore del mouse | Fare clic e trascinare | 2 il doppio tocco con un dito e tenere premuto, quindi trascinare.                               |
| Puntatore del mouse | Rotellina del mouse          | 2 dito toccare e tenere premuto, quindi trascinare verso l'alto o verso il basso                           |
| Puntatore del mouse | Zoom                 | Utilizzare le 2 DITA e avvicinare le dita per eseguire lo zoom avanti o spostare divide in due dita per eseguire lo zoom indietro. |

> [!TIP]
> Domande e commenti sono sempre Benvenuti. Tuttavia, NON inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, andare alla [forum su client di Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Avete suggerimenti funzionalità? Usa l'[Hub di Feedback](feedback-hub://?tabid=2&contextid=605) per inviarci le tue impressioni.
