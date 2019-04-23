---
title: Introduzione a Desktop remoto in Windows
description: Installazione di base i passaggi per il client Desktop remoto per Windows.
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
ms.date: 05/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: 20879507e13ac7da566c95db7b59d88e0b5d8ce8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873572"
---
# <a name="get-started-with-remote-desktop-on-windows"></a>Introduzione a Desktop remoto in Windows

>Si applica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

È possibile usare il client Desktop remoto per Windows per lavorare con le app di Windows e i desktop in remoto da un altro dispositivo di Windows.

Usare le informazioni seguenti per iniziare. Assicurarsi di consultare il [domande frequenti su](remote-desktop-client-faq.md) se hai domande.

> [!NOTE]
> - Conoscere le nuove versioni per il client di Windows? Scopri [novità di Desktop remoto in Windows?](windows-whatsnew.md)
> - È possibile eseguire il client in qualsiasi versione di Windows 10.

## <a name="get-the-rd-client-and-start-using-it"></a>Ottenere il client desktop remoto e iniziare a usarlo

Seguire questi passaggi per iniziare a usare Desktop remoto nel dispositivo Windows 10:

1. Scaricare il client Desktop remoto dalla [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configurare il PC per accettare le connessioni remote](remote-desktop-allow-access.md).
3. Aggiungere una connessione Desktop remoto o una risorsa remota. Si usa una connessione per connettersi direttamente a un PC Windows e una risorsa remota. utilizzare un programma RemoteApp, desktop basati su sessione o desktop virtuale pubblicati dall'amministratore. 
4. Aggiungere elementi, quindi è possibile ottenere rapidamente a Desktop remoto.

### <a name="add-a-remote-desktop-connection"></a>Aggiungere una connessione Desktop remoto

Per creare una connessione Desktop remoto:

1. Nella scelta dei Centro connessioni **+ Aggiungi**, quindi toccare **Desktop**.
2. Immettere le informazioni seguenti per il computer si desidera connettersi:
  - **Nome PC** : il nome del computer. Può trattarsi di un nome di computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del computer (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
  - **Account utente** : l'account utente da utilizzare per accedere al computer remoto. Toccare **+** per aggiungere un nuovo account oppure selezionare un account esistente. È possibile usare i formati seguenti per il nome utente: *user_name*, *DOMINIO\nome_utente.*, o *user_name@domain.com*. È inoltre possibile specificare se per la richiesta di un nome utente e password durante la connessione selezionando **Chiedi conferma ogni volta**.
3. È anche possibile impostare opzioni aggiuntive toccando **Show more**:
  - **Nome visualizzato** : un nome facile da ricordare per il computer si connette a. È possibile utilizzare qualsiasi stringa, ma se non si specifica un nome descrittivo, viene visualizzato il nome del computer.
  - **Gruppo** : specificare un gruppo per renderlo più semplice trovare le connessioni in un secondo momento. È possibile aggiungere un nuovo gruppo toccando **+** o selezionarne uno dall'elenco.
  - **Gateway** – gateway Desktop remoto che si desidera utilizzare per connettersi a desktop virtuali, programmi RemoteApp e desktop basati su sessione su una rete aziendale interna. Ottenere le informazioni sul gateway, dall'amministratore di sistema.
  - **Connettersi alla sessione di amministrazione** -usare questa opzione per connettersi a una sessione di console per amministrare un server di Windows.
  - **Scambia i pulsanti del mouse** : utilizzare questa opzione per invertire il puntatore del mouse a sinistra pulsante funzioni per il pulsante destro del mouse. (Ciò è particolarmente utile se sul computer remoto è configurato per un utente da ma si utilizza un mouse da destra.)
  - **Impostare la risoluzione della sessione remota:** – selezionare la soluzione da usare nella sessione. **Scegliere per me** imposterà la risoluzione in base alla dimensione del client.
  - **Modificare le dimensioni dello schermo:** : quando si seleziona ad alta risoluzione statica per la sessione, è disponibile l'opzione per consentire agli elementi sullo schermo di dimensioni maggiori per migliorare la leggibilità. Nota: Ciò si applica solo quando ci si connette a Windows 8.1 o versioni successive.
  - **Aggiornare la risoluzione della sessione remota in caso di ridimensionamento** – quando abilitato, il client verrà aggiornato in modo dinamico la risoluzione di sessione in base alla dimensione del client. Nota: Ciò si applica solo quando ci si connette a Windows 8.1 o versioni successive.
  - **Appunti** : quando abilitata, consente di copiare il testo e immagini in/da computer remoto.
  - **Riproduzione audio** : selezionare il dispositivo da usare per l'audio durante la sessione remota. È possibile scegliere di riprodurre un suono nei dispositivi locali, il computer remoto, o niente affatto.
  - **Registrazione audio** : quando abilitata, consente di usare un microfono locale con le applicazioni nel computer remoto.
4. Toccare **salvare**.

È necessario modificare queste impostazioni? Toccare il menu di overflow (**...** ) accanto al nome del desktop e quindi toccare **modifica**.

Se si desidera eliminare la connessione? Nuovamente, toccare il menu di overflow (**...** ) e quindi toccare **rimuovere**.

### <a name="add-a-remote-resource"></a>Aggiungere una risorsa remota.
Risorse remote sono programmi RemoteApp, desktop basati su sessione e desktop virtuali pubblicati dall'amministratore tramite Servizi Desktop remoto.

Per aggiungere una risorsa remota:

1. Nella schermata connessione Center toccare **+ Aggiungi**, quindi toccare **risorse Remote**. 
2. Immettere il **URL Feed** fornito dall'amministratore, quindi toccare **trovare feed**.
3. Quando richiesto, fornire le credenziali da utilizzare per la sottoscrizione al feed.

Verranno visualizzate nel Centro connessioni di risorse remote.


Per eliminare le risorse remote:

1. Nel Centro connessioni, toccare il menu di overflow (**...**) accanto alla risorsa remota.
2. Toccare **rimuovere**.

### <a name="pin-a-saved-desktop-to-your-start-menu"></a>PIN un desktop salvato dal menu Start

Per aggiungere una connessione a dal menu Start, toccare il menu di overflow (**...** ) accanto al nome del desktop e quindi toccare **Aggiungi a Start**.

È ora possibile avviare la connessione desktop remoto direttamente dal menu di avvio per selezionarlo.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Connettersi a un Gateway Desktop remoto per accedere alle risorse interne

Gateway Desktop remoto (Gateway Desktop remoto) consente di connettersi a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. È possibile creare e gestire il gateway tramite il client Desktop remoto.

Per impostare un nuovo gateway:

1. Nel Centro connessioni, toccare **impostazioni**.
2. Accanto a Gateway, toccare **+** per aggiungere un nuovo gateway. Nota: È possibile aggiungere un gateway anche quando si aggiunge una nuova connessione.
3. Immettere le informazioni seguenti:
  - **Nome del server** : il nome del computer di cui si desidera utilizzare come gateway. Può trattarsi di un nome di computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del server (ad esempio: **RDGateway:443** oppure **10.0.0.1: 443**).
  - **Account utente** : selezionare o aggiungere un account utente da usare con il Gateway Desktop remoto si è connessi. È inoltre possibile selezionare **utilizzare account utente desktop** per utilizzare le stesse credenziali utilizzate per la connessione desktop remoto.
4. Toccare **salvare**.  

## <a name="global-app-settings"></a>Impostazioni dell'app globale

È possibile impostare le impostazioni globali seguenti nel client toccando **impostazioni**:

GLI ELEMENTI GESTITI
- **Account utente** : consente di aggiungere, modificare ed eliminare gli account utente salvati nel client. Questo è un buon metodo per aggiornare la password per un account dopo la modifica.
- **Gateway** : consente di aggiungere, modificare ed eliminare i server gateway salvati nel client.
- **Gruppo** : consente di aggiungere, modificare ed eliminare i gruppi salvati nel client. Queste classi consentono di facilmente le connessioni del gruppo.

IMPOSTAZIONI SESSIONE
- **Avviare le connessioni in modalità schermo intero** : quando abilitato, ogni volta che viene avviata una connessione, il client userà l'intero schermo del monitor corrente.
- **Avviare ogni connessione in una nuova finestra** -se abilitato, ogni connessione viene avviata in una finestra separata, che consente di inserire tali elementi su monitor diversi e passare tra loro usando la barra delle applicazioni.
- **Quando si ridimensiona l'app:** -consente di controllare cosa accade quando si ridimensiona la finestra del client. Per impostazione predefinita **Stretch al contenuto, mantenendo le proporzioni**.
- **Usare i comandi da tastiera con:** -consente di specificare quale comandi da tastiera *vincere* oppure *ALT + TAB* vengono usati. Il valore predefinito è solo di inviarle alla sessione quando la connessione è in schermo intero.
- **Evitare il timeout di schermo** -consente di impedire la schermata di timeout quando è attiva una sessione. Ciò risulta utile quando la connessione non richiede alcuna interazione per lunghi periodi di tempo.

IMPOSTAZIONI DELL'APP
- **Mostra le anteprime Desktop** -consente di visualizzare un'anteprima del desktop nel Centro connessioni prima di connettersi a esso. Per impostazione predefinita, questo è impostato **su**.
- **Contribuire al miglioramento di Desktop remoto** -invia dati anonimi a Microsoft. Usiamo i dati per migliorare il client. Per altre informazioni su come vengono gestiti questi dati anonimi, privati, vedere la [informativa sulla Privacy Microsoft](https://privacy.microsoft.com/en-us/privacystatement). Per impostazione predefinita, questa impostazione è **su**.

### <a name="manage-your-user-accounts"></a>Gestire gli account utente

Quando ci si connette a una risorsa remota o desktop, è possibile salvare gli account utente per selezionare nuovamente. È inoltre possibile definire gli account utente nel client, anziché salvare i dati utente quando ci si connette a un desktop.

Per creare un nuovo account utente:

1. Nel Centro connessioni, toccare **impostazioni**.
2. Accanto a account utente, toccare **+** per aggiungere un nuovo account utente.
3. Immettere le informazioni seguenti:
   - **Nome utente** -il nome dell'utente da salvare per l'uso con una connessione remota. È possibile immettere il nome utente in uno dei seguenti formati: nome_utente, DOMINIO\nome_utente., o user_name@domain.com.
   - **Password** -la password per l'utente specificato. Ciò può essere lasciata vuota per ricevere la richiesta di immettere una password durante la connessione.
4. Toccare **salvare**.

Per eliminare un account utente:

1. Nel Centro connessioni, toccare **impostazioni**.
2. Selezionare l'account da eliminare dall'elenco con account utente.
3. Accanto a account utente, toccare l'icona di modifica.
4. Toccare **rimuovere questo account** nella parte inferiore per eliminare l'account utente.
5. È anche possibile modificare l'account utente e toccare **salvare**.

## <a name="navigate-the-remote-desktop-session"></a>Passare la sessione Desktop remoto
Quando si avvia una connessione desktop remoto, sono disponibili strumenti che è possibile utilizzare per passare la sessione.

### <a name="start-a-remote-desktop-connection"></a>Avviare una connessione Desktop remoto

1. Toccare la connessione Desktop remoto per avviare la sessione.
2. Se è ancora stato salvato le credenziali per la connessione, verrà richiesto di fornire una **nomeutente** e **Password**.
3. Se viene richiesto di verificare il certificato per il desktop remoto, esaminare le informazioni e assicurarsi che si tratti di un PC attendibili prima toccando **Connect**. È anche possibile selezionare **non chiedere più su questo certificato** sempre accettare questo certificato.

### <a name="connection-bar"></a>Barra delle connessioni

Consente di barra di connessione di accedere ai controlli di spostamento aggiuntive. Per impostazione predefinita, la barra delle connessioni viene posizionata al centro nella parte superiore della schermata. Toccare e trascinare la barra a sinistra o destra per spostarlo.

- **Panoramica di controllo** -il controllo zoom consente la schermata di ingrandimento e spostati. Si noti che controllo zoom è disponibile solo in dispositivi abilitati per il tocco e usando la modalità tocco diretto.
   - Abilitare / disabilitare il controllo dettaglio: Toccare l'icona Zoom nella barra di connessione per visualizzare il controllo di panoramica e zoom schermata. Toccare l'icona Zoom nella barra di connessione per nascondere il controllo e restituire la schermata per la risoluzione originale.
   - Usare il controllo dettaglio: toccare e tenere premuto il controllo dettaglio e quindi trascinare nella direzione per spostare la schermata.
   - Spostare il controllo dettaglio: doppio tocco e mantenere il controllo dettaglio per spostare il controllo sullo schermo.
- **Opzioni aggiuntive** -toccare l'icona Opzioni aggiuntive per visualizzare la selezione di sessione a barre e comando della barra (vedere sotto).
- **Tastiera** -toccare l'icona di tastiera per visualizzare o nascondere la tastiera su schermo. Il controllo dettaglio viene visualizzato automaticamente quando viene visualizzata la tastiera.

### <a name="command-bar"></a>Barra dei comandi

Toccare il **...**  sulla barra di connessione per visualizzare la barra dei comandi sul lato destro dello schermo.

- **Home** -usare il pulsante Home per tornare al centro di connessione nella barra dei comandi.
  - In alternativa è possibile utilizzare il pulsante Indietro per la stessa azione.
  - La sessione attiva non verrà disconnesso.
  - In questo modo è possibile avviare connessioni aggiuntive.
- **Disconnetti** -usare il pulsante Disconnetti per terminare la connessione.
  - L'App rimarrà attiva finché non termina la sessione nel computer remoto.
- **Schermo intero** - entra o esce dalla modalità schermo intero.
- **Touch e Mouse** -è possibile spostarsi tra le modalità mouse (tocco diretto e il puntatore del Mouse).

### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Utilizzo diretto touch movimenti e le modalità del mouse in una sessione remota

Due modalità mouse disponibili interagire con la sessione.
- **Tocco diretto**: Passa tutti i contatti tocco per la sessione deve essere interpretato in modalità remota.
  - Usato nello stesso modo che si utilizzerebbe Windows con touch screen.
- **Puntatore del mouse**: Trasforma il touchscreen locale in un grande touchpad che consente di spostare il puntatore del mouse nella sessione.
  - Usato nello stesso modo che si utilizzerebbe Winodws con il touchpad.

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
> Domande e commenti sono sempre Benvenuti. Tuttavia, NON inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, andare alla [forum su client di Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Avete suggerimenti funzionalità? Comunicaci usando il [Hub di Feedback](feedback-hub://?tabid=2&contextid=605).
