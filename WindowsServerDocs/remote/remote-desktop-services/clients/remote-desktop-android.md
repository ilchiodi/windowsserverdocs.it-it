---
title: Introduzione a Desktop remoto in Android
description: Basic consente di impostare i passaggi per il client Desktop remoto per Android.
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
ms.openlocfilehash: 42b4b4ffb73bd9d5d1397d32bd36c41d7e404dd7
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297430"
---
# Introduzione a Desktop remoto in Android

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

È possibile utilizzare il client Desktop remoto per Android per lavorare con i desktop e le app di Windows direttamente dal tuo dispositivo Android.

Usa le informazioni seguenti per informazioni di base. Assicurati di consulta le [domande frequenti](remote-desktop-client-faq.md) , se hai domande.

> [!NOTE]
> - Le nuove versioni del client Android di saperlo? Consulta [Novità per Desktop remoto in Android?](android-whatsnew.md)
> È possibile eseguire il client su 4.1 Android e i dispositivi più recenti nonché su Chromebook con 53 ChromeOS installato. Altre informazioni sulle applicazioni Android in Chrome [qui](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## Ottenere il client desktop remoto e iniziare a usarlo

Segui questi passaggi per iniziare con Desktop remoto nel dispositivo Android:

1. Scaricare il client Desktop remoto da [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configurare il PC per accettare le connessioni remote](remote-desktop-allow-access.md).
3. Aggiungere una connessione Desktop remoto o una risorsa remota. Usare una connessione di connettersi direttamente a un PC Windows e una risorsa remota per usare un programma RemoteApp, basata su sessione desktop o desktop virtuale pubblicata in locale. 
4. Creare un widget può essere rapidamente per Desktop remoto.

> [!NOTE]
> Se si vuole della versione di anteprima nuove funzionalità in precedenza, ti consigliamo di scaricare l'applicazione della [Versione Beta di Desktop remoto Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) dall'archivio di Google Play. 

### Aggiungere una connessione Desktop remoto

Per creare una connessione Desktop remoto:

1. Nel tocco di connessione Center **+** e quindi tocca **Desktop**.
2. Immetti le informazioni seguenti per il computer che si desidera connettersi:
  - **Nome del PC** : il nome del computer. Può trattarsi di un nome di computer di Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere le informazioni sulla porta per il nome del PC (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
  - **Nome utente** : il nome utente da usare per accedere al PC remoto. Puoi usare i formati seguenti: *nome_utente*, *DOMINIO\nome_utente.* o *user_name@domain.com*. È inoltre possibile specificare se di richiedere un nome utente e password.
3. Puoi anche impostare le opzioni aggiuntive seguenti:
  - **Nome descrittivo** : un nome per il PC si connette a facile da ricordare. È possibile utilizzare qualsiasi stringa, ma se non specifichi un nome descrittivo, viene visualizzato il nome del PC.
  - **Gateway** -gateway di Desktop remoto che vuoi usare per connettersi a desktop virtuali, i programmi RemoteApp e desktop basati su sessione in una rete aziendale interna. Ottenere le informazioni sul gateway dall'amministratore di sistema.
    È necessario configurare un Gateway Desktop remoto?
  - **Audio** : selezionare il dispositivo da utilizzare per l'audio durante la sessione remota. Scegliere di riprodurre un suono nei dispositivi locali, il dispositivo remoto, o affatto.
  - **Personalizza la risoluzione** - impostare una risoluzione personalizzata per una connessione attivando questa impostazione. Quando viene applicato disattivare la risoluzione di cui hai definito in impostazioni globali dell'app.
  - **Pulsanti del mouse scambio** : Usa questa opzione per il mouse a sinistra di scambio funzioni dei pulsanti per il pulsante destro del mouse. (Questo è particolarmente utile se il PC remoto è configurato per un utente sinistrorso, ma usare un mouse destrorso.)
  - **Connetti all'amministratore sessione** - Usa questa opzione per connettersi a una sessione della console per amministrare i Windows server.
  - **Reindirizzare l'archiviazione locale** – consente di montare l'archiviazione locale come un file system remoto nel PC remoto.
4. Tocca **salvare**.

Devi modificare queste impostazioni? Tocca il menu di overflow (**...**) accanto al nome del desktop e quindi tocca **modificare**.

Vuoi eliminare la connessione? Anche in questo caso, tocca il menu di overflow (**...**) e quindi tocca **rimuovere**.

>[!TIP]
> Se ricevi l'errore 0xf07 password errate ("che non si connettono al PC remoto perché la password associata all'account utente è scaduto"), modificare la password e riprova.

### Aggiungere una risorsa remota
Risorse remote sono programmi RemoteApp, basata su sessione desktop e desktop virtuali pubblicato con connessione RemoteApp e Desktop.

Per aggiungere una risorsa remota:

1. Nella schermata di connessione Center, tocca **+** e quindi tocca **Feed risorse Remote**. 
2. Immettere le informazioni per la risorsa remota:
   - **Messaggio di posta elettronica o URL** - l'URL del server Accesso Web desktop remoto. È inoltre possibile immettere l'account di posta elettronica aziendali in questo campo: in questo modo il client per cercare il Server di accesso Web desktop remoto associato l'indirizzo di posta elettronica.
   - **Nome utente** - il nome utente da utilizzare per il server di accesso Web desktop remoto a che si connette.
   - **Password** : password da usare per il server di accesso Web desktop remoto a che si connette.
3. Tocca **salvare**.

Le risorse remote verranno visualizzate al centro di connessione.


Per eliminare le risorse remote:

1. Al centro di connessione, tocca il menu di overflow (**...**) accanto alla risorsa remota.
2. Tocca **rimuovere**.
3. Confermare l'eliminazione.

### Widget-aggiungere un desktop salvato alla schermata home

Le applicazioni Desktop remoto supportano le connessioni associazione alla schermata home utilizzando la funzionalità widget Android. Il modo di aggiungere un widget dipende dal tipo di dispositivo Android che usi e il sistema operativo. Ecco il modo più comune per aggiungere un widget: 

1. Toccare **le app** per avviare il menu di App.
2. Tocca **widget**.
3. Scorrere rapidamente tramite i widget e Cerca l'icona di Desktop remoto con la descrizione "Pin di Desktop remoto".
4. Tocca e tieni premuto tale widget Desktop remoto e spostarlo alla schermata iniziale.
5. Quando si rilascia l'icona, vedrai il funzionamento dei desktop remoti salvata. Scegliere la connessione che si desidera salvare alla schermata home.

Ora puoi iniziare la connessione desktop remoto direttamente dalla tua schermata home toccando.

> [!NOTE]
> Se si rinomina la connessione desktop nell'applicazione Desktop remoto, l'etichetta di questo aggiunte desktop remoto non sarà aggiornati.

## Connettersi a un Gateway Desktop remoto per accedere alle risorse interne

Un Gateway Desktop remoto (Gateway Desktop remoto) consente di connettersi a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. Puoi creare e gestire il gateway tramite il client Desktop remoto.

Per configurare un nuovo gateway:

1. Al centro di connessione, toccare **Impostazioni gt _ gateway**. Tocca **+** per aggiungere un nuovo gateway.
2. Immetti le informazioni seguenti:
  - **Nome del server** : il nome del computer che vuoi usare come gateway. Può trattarsi di un nome di computer di Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere le informazioni sulla porta per il nome del server (ad esempio: **RDGateway:443** o **10.0.0.1:443**).
  - **Nome utente** - il nome utente e password da usare per il Gateway Desktop remoto si connette a. Puoi anche selezionare **usare account utente desktop** per usare le stesse credenziali come quelli usati per la connessione desktop remota.

## Gestire gli account utente

Quando ti Connetti a un desktop o remoto risorse, è possibile salvare gli account utente di selezionare da nuovamente. È anche possibile definire gli account utente nel client, anziché il salvataggio dei dati utente quando ci si connette a un desktop.

Per creare un nuovo account utente:

1. Al centro di connessione, toccare **le impostazioni**e quindi toccare **gli account utente**.
2. Tocca **+** per aggiungere un nuovo account utente.
3. Immetti le informazioni seguenti:
   - **Nome utente** - il nome dell'utente di salvare per l'uso con una connessione remota. Puoi immettere il nome utente in una qualsiasi delle seguenti formati: nome_utente, DOMINIO\nome_utente., o user_name@domain.com.
   - **Password** : la password per l'utente specificato. Ogni account utente che si desidera salvare per usare per le connessioni remote deve avere una password è associata.
4. Tocca **salvare**.

Per eliminare un account utente:

1. Al centro di connessione, toccare **gli account utente gt _ impostazioni**.
2. Tocca e tieni premuto un account utente nell'elenco per selezionarlo. Puoi selezionare più utenti.
3. Tocca il Cestino per eliminare l'utente selezionato.

## Passa la sessione Desktop remoto
Quando si avvia una connessione desktop remoto, sono disponibili strumenti disponibili che è possibile utilizzare per passare la sessione.

### Avviare una connessione Desktop remoto

1. Tocca la connessione Desktop remoto per avviare la sessione. 
2. Se viene richiesto di verificare il certificato per il desktop remoto, toccare **Connect**. Puoi anche selezionare sempre accetta il certificato **non Richiedi nuovamente per le connessioni a questo computer** .

### Gestire le impostazioni dell'app globale

Puoi impostare le seguenti impostazioni globale nel client Android:

- **Visualizzare le anteprime Desktop** - ti consente di visualizzare un'anteprima di un desktop al centro della connessione prima di connettersi a esso. Per impostazione predefinita, questo è impostato su **on**.
- **Avvicinamento delle dita per eseguire lo Zoom** - consente di usare i movimenti di avvicinamento delle dita di zoom. Se l'app usi tramite Desktop remoto supporta Multi-touch (introdotto in Windows 8), attivare questa impostazione **disattivata**.
- **Contribuire a migliorare Desktop remoto** - dati anonimi Invia a Microsoft. Questi dati vengono utilizzati per migliorare il client. Per altre informazioni su come trattiamo questi dati anonimi, privati, vedere l' [Informativa sulla Privacy di Client Desktop remoto](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). Per impostazione predefinita, questa impostazione è **attivata**.
- **Visualizzare** - ci sono due impostazioni globali dello schermo:
   - **Orientamento** - imposta l'orientamento preferita (orizzontale o verticale) per la sessione. 
   >[!NOTE]
   > Se si connette a un PC che eseguono Windows 8 o una versione precedente di Windows, la sessione non vengono ridimensionate correttamente. Il metodo migliore consiste nel scollegare dal PC e poi ricollegare nell'orientamento che vuoi usare. Un'opzione migliore consiste nell'aggiornare almeno il PC per Windows 8.1.

   - **Risoluzione** - imposta la risoluzione si desidera utilizzare per le connessioni desktop a livello globale. Se hai già impostato una risoluzione personalizzata per un'app singola o una connessione, questa impostazione non cambierà che.
   >[!NOTE]
   >Quando modifichi una delle impostazioni di visualizzazione, essi si applicano solo a nuove connessioni da quel punto su. Per visualizzare la modifica in una sessione di che sei già connesso per scollegare e quindi connettersi nuovamente.

### Barra di connessione

Consente di barra di connessione che accedere ai controlli di spostamento aggiuntivi. Per impostazione predefinita, la barra di connessione viene posizionata centrale nella parte superiore dello schermo. Doppio tocco e trascinare la barra a sinistra o a destra per spostarlo.

- **Controllo di panning**: il controllo pan Abilita schermo per essere ingrandita e spostate. Tieni presente che è disponibile tramite tocco diretto solo controllo pan.
   - Abilitare / disabilitare il controllo pan: toccare l'icona di panoramica nella barra di connessione per visualizzare il controllo di panoramica e zoom lo schermo. Toccare l'icona di panoramica nella barra di connessione nuovamente per nascondere il controllo e restituire lo schermo alla sua risoluzione originale.
   - Usare il controllo pan: tocco e attesa della panoramica di controllo e quindi trascinare nella direzione che si desidera spostare lo schermo.
   - Spostare il controllo pan: Usa il doppio tocco e contenere il controllo di zoom per spostare il controllo sullo schermo.
- **Opzioni aggiuntive**: toccare l'icona di opzioni aggiuntive per visualizzare la selezione di sessione barra e comando della barra (vedi di seguito).
- **Tastiera**: toccare l'icona della tastiera per visualizzare o nascondere la tastiera. Il controllo zoom viene visualizzato automaticamente quando viene visualizzata la tastiera.
- **Spostare la barra di connessione**: tocco e la barra di connessione, pressione prolungata e quindi trascina e rilascia in una nuova posizione nella parte superiore dello schermo.


### Barra dei comandi

Toccare la barra di connessione per visualizzare la barra dei comandi sul lato destro dello schermo. Puoi passare tra le modalità mouse (tocco diretto e del puntatore del Mouse). Usa il pulsante home per tornare al Centro connessione della barra dei comandi. In alternativa è possibile utilizzare il pulsante Indietro per la stessa azione. La sessione attiva non verrà disconnesso. 


### Utilizzo diretto movimenti di tocco e le modalità mouse in una sessione remota

Il client Usa movimenti di tocco standard. Puoi anche usare movimenti di tocco per replicare le azioni del mouse sul desktop remoto. La modalità mouse disponibili sono definite nella tabella seguente.

> [!NOTE]
> Interazione con Windows 8 o versione successiva i movimenti di tocco native sono supportati in modalità tocco diretto. 

| Modalità mouse    | Operazione con il mouse      | Movimento                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Tocco diretto  | Pulsante sinistro           | tocco con 1 dita                                                          |
| Tocco diretto  | Fare clic con il pulsante destro del mouse          | 1 tocco delle dita e pressione prolungata                                                 |
| Puntatore del mouse | Zoom                 | Usare 2 DITA e avvicinamento delle dita per eseguire lo zoom avanti o spostare tra di loro dita per lo zoom indietro. |
| Puntatore del mouse | Pulsante sinistro           | tocco con 1 dita                                                          |
| Puntatore del mouse | Fare clic con il tasto sinistro del mouse e trascinare  | tocca due volte 1 DITA e pressione prolungata, quindi trascinare.                               |
| Puntatore del mouse | Fare clic con il pulsante destro del mouse          | tocca il dito 2                                                          |
| Puntatore del mouse | Fare clic con il tasto destro del mouse e trascinare | 2 doppio tocco DITA e pressione prolungata, quindi trascinare.                               |
| Puntatore del mouse | Rotellina del mouse          | 2 dito tocca e tieni premuto, quindi trascinare verso l'alto o verso il basso                           |

> [!TIP]
> Domande e i commenti sono sempre iniziale. Tuttavia, non è possibile inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, Vai al [forum di client Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Hai un suggerimento di funzionalità? Facci sapere nel [forum di voce utente client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
