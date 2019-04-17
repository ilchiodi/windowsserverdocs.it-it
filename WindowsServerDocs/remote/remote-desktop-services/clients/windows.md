---
title: Introduzione a Desktop remoto in Windows
description: Impostazione passaggi per il client Desktop remoto per Windows di base.
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
ms.openlocfilehash: 1c06e2eca725a825ac0fa4c7b617a26d89c898ff
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297460"
---
# Introduzione a Desktop remoto in Windows

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

È possibile utilizzare il client Desktop remoto per Windows per lavorare con le app di Windows e i desktop in modalità remota da un altro dispositivo di Windows.

Usa le informazioni seguenti per informazioni di base. Assicurati di consulta le [domande frequenti](remote-desktop-client-faq.md) , se hai domande.

> [!NOTE]
> - Conoscere i nuovi rilasci per il client di Windows? Consulta [Novità per Desktop remoto in Windows?](windows-whatsnew.md)
> - È possibile eseguire il client in qualsiasi versione di Windows 10.

## Ottenere il client desktop remoto e iniziare a usarlo

Segui questi passaggi per iniziare con Desktop remoto nel tuo dispositivo Windows 10:

1. Scaricare il client Desktop remoto da [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configurare il PC per accettare le connessioni remote](remote-desktop-allow-access.md).
3. Aggiungere una connessione Desktop remoto o una risorsa remota. Usare una connessione di connettersi direttamente a un PC Windows e una risorsa remota per usare un programma RemoteApp, basata su sessione desktop o desktop virtuale pubblicata da parte dell'amministratore. 
4. Aggiungere elementi possono essere rapidamente per Desktop remoto.

### Aggiungere una connessione Desktop remoto

Per creare una connessione Desktop remoto:

1. Nel centro per i connessione tocca **+ Aggiungi**e quindi tocca **Desktop**.
2. Immetti le informazioni seguenti per il computer che si desidera connettersi:
  - **Nome del PC** : il nome del computer. Può trattarsi di un nome di computer di Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere le informazioni sulla porta per il nome del PC (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
  - **Account utente** : l'account utente da utilizzare per accedere al PC remoto. Tocca **+** per aggiungere un nuovo account o selezionare un account esistente. Puoi usare i formati seguenti per il nome utente: *nome_utente*, *DOMINIO\nome_utente.* o *user_name@domain.com*. È inoltre possibile specificare se richiedere un nome utente e password durante la connessione selezionando **Chiedi ogni volta**.
3. Puoi anche impostare le opzioni aggiuntive il tocco di **mostrare altre**:
  - **Nome visualizzato** : un nome per il PC si connette a facile da ricordare. È possibile utilizzare qualsiasi stringa, ma se non specifichi un nome descrittivo, viene visualizzato il nome del PC.
  - **Gruppo** : specificare un gruppo per renderlo più facile trovare le connessioni in un secondo momento. Puoi aggiungere un nuovo gruppo toccando **+** oppure selezionare uno dall'elenco.
  - **Gateway** -gateway di Desktop remoto che vuoi usare per connettersi a desktop virtuali, i programmi RemoteApp e desktop basati su sessione in una rete aziendale interna. Ottenere le informazioni sul gateway dall'amministratore di sistema.
  - **Connetti all'amministratore sessione** - Usa questa opzione per connettersi a una sessione della console per amministrare i Windows server.
  - **Pulsanti del mouse scambio** : Usa questa opzione per il mouse a sinistra di scambio funzioni dei pulsanti per il pulsante destro del mouse. (Questo è particolarmente utile se il PC remoto è configurato per un utente sinistrorso, ma usare un mouse destrorso.)
  - **Impostare la risoluzione di sessione remota:** -seleziona la risoluzione che vuoi usare nella sessione. **Scegli per me** imposterà la risoluzione in base alla dimensione del client.
  - **Modificare le dimensioni dello schermo:** : quando si seleziona ad alta risoluzione statica per la sessione, hai la possibilità di visualizzare gli elementi sullo schermo più grande per migliorare la leggibilità. Nota: Questo avviene solo quando ci si connette a Windows 8.1 o versioni successive.
  - **Aggiornare la sessione remota di risoluzione sul ridimensionamento** – quando è abilitata, il client verrà aggiornata in modo dinamico la risoluzione di sessione in base alla dimensione del client. Nota: Questo avviene solo quando ci si connette a Windows 8.1 o versioni successive.
  - **Negli Appunti** – quando è abilitata, consente di copiare il testo e immagini tra il PC remoto.
  - **La riproduzione audio** , selezionare il dispositivo da utilizzare per l'audio durante la sessione remota. Scegliere di riprodurre un suono nei dispositivi locali, il PC remoto, o affatto.
  - **Registrazione audio** – quando è abilitata, puoi utilizzare un microfono locale con le applicazioni nel PC remoto.
4. Tocca **salvare**.

Devi modificare queste impostazioni? Tocca il menu di overflow (**...**) accanto al nome del desktop e quindi tocca **modificare**.

Vuoi eliminare la connessione? Anche in questo caso, tocca il menu di overflow (**...**) e quindi tocca **rimuovere**.

### Aggiungere una risorsa remota
Risorse remote sono programmi RemoteApp, basata su sessione desktop e desktop virtuali pubblicato da parte dell'amministratore tramite Servizi Desktop remoto.

Per aggiungere una risorsa remota:

1. Nella schermata di connessione Center, toccare **+ Aggiungi**e quindi tocca **risorse Remote**. 
2. Immetti l' **URL del Feed** forniti dal tuo amministratore e tocca **trovare feed**.
3. Quando richiesto, specificare le credenziali da utilizzare per la sottoscrizione il feed.

Le risorse remote verranno visualizzate al centro di connessione.


Per eliminare le risorse remote:

1. Al centro di connessione, tocca il menu di overflow (**...**) accanto alla risorsa remota.
2. Tocca **rimuovere**.

### Aggiungere un desktop salvato il menu Start

Per aggiungere una connessione al menu Start, tocca il menu di overflow (**...**) accanto al nome del desktop e quindi toccare **Aggiungi a Start**.

Ora puoi iniziare la connessione desktop remoto direttamente dal menu Start toccando.

## Connettersi a un Gateway Desktop remoto per accedere alle risorse interne

Un Gateway Desktop remoto (Gateway Desktop remoto) consente di connettersi a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. Puoi creare e gestire il gateway tramite il client Desktop remoto.

Per configurare un nuovo gateway:

1. Al centro di connessione, toccare **Impostazioni**.
2. Accanto a Gateway, tocca **+** per aggiungere un nuovo gateway. Nota: Un gateway possa essere aggiunti anche quando Aggiungi una nuova connessione.
3. Immetti le informazioni seguenti:
  - **Nome del server** : il nome del computer che vuoi usare come gateway. Può trattarsi di un nome di computer di Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere le informazioni sulla porta per il nome del server (ad esempio: **RDGateway:443** o **10.0.0.1:443**).
  - **Account utente** - selezionare o aggiungere un account utente da utilizzare con il Gateway Desktop remoto si connette a. Puoi anche selezionare **usare account utente desktop** per usare le stesse credenziali come quelli usati per la connessione desktop remota.
4. Tocca **salvare**.  

## Impostazioni dell'app globale

È possibile impostare le seguenti impostazioni globale nel client toccando **Impostazioni**:

GESTIRE GLI ELEMENTI
- **Account utente** - consente di aggiunta, modificare ed eliminare gli account utente salvati nel client. Questo è un buon metodo per aggiornare la password di un account, dopo avere modificato.
- **Gateway** - consente di aggiunta, modificare ed eliminare i server gateway salvati nel client.
- **Gruppo** - consente di aggiunta, modificare ed eliminare gruppi salvati nel client. Queste consentono a facilmente le connessioni di gruppo.

IMPOSTAZIONI DELLA SESSIONE
- **Avviare le connessioni a schermo intero** , quando è abilitata, ogni volta che viene avviata una connessione, il client userà l'intero schermo del monitor corrente.
- **Avviare ogni connessione in una nuova finestra** , quando è abilitata, ogni connessione viene avviata in una finestra separata, consentendoti di posizionarli su schermi diversi e passare tra di essi usando la barra delle applicazioni.
- **Quando il ridimensionamento dell'app:** -consente di controllare cosa accade quando la finestra di client viene ridimensionata. Valori predefiniti per **estendere il contenuto, mantenendo le proporzioni**.
- **Usare i comandi della tastiera con:** -Iniziamo specificare in cui vengono usati i comandi della tastiera, ad esempio *ALT + TAB* o *vincere* . Il valore predefinito è di inviarle solo alla sessione quando la connessione è in scren completo.
- **Impedire la schermata il timeout di** - consente di mantenere lo schermo del timeout quando è attiva una sessione. Ciò è utile quando la connessione non richiede alcuna interazione per lunghi periodi di tempo.

IMPOSTAZIONI DELL'APP
- **Visualizzare le anteprime Desktop** - ti consente di visualizzare un'anteprima di un desktop al centro della connessione prima di connettersi a esso. Per impostazione predefinita, questo è impostato su **on**.
- **Migliorare la Desktop remoto** - dati anonimi Invia a Microsoft. Questi dati vengono utilizzati per migliorare il client. Per ulteriori informazioni sul modo in cui trattiamo questi dati anonimi e privati, vedere l' [Informativa sulla Privacy di Microsoft](https://privacy.microsoft.com/en-us/privacystatement). Per impostazione predefinita, questa impostazione è **attivata**.

### Gestire gli account utente

Quando ti Connetti a un desktop o remoto risorse, è possibile salvare gli account utente di selezionare da nuovamente. È anche possibile definire gli account utente nel client, anziché il salvataggio dei dati utente quando ci si connette a un desktop.

Per creare un nuovo account utente:

1. Al centro di connessione, toccare **Impostazioni**.
2. Uno accanto all'account utente, tocca **+** per aggiungere un nuovo account utente.
3. Immetti le informazioni seguenti:
   - **Nome utente** : il nome dell'utente di salvare per l'uso con una connessione remota. Puoi immettere il nome utente in una qualsiasi delle seguenti formati: nome_utente, DOMINIO\nome_utente., o user_name@domain.com.
   - **Password** : la password per l'utente specificato. Questo può essere lasciato vuoto venga richiesta una password durante la connessione.
4. Tocca **salvare**.

Per eliminare un account utente:

1. Al centro di connessione, toccare **Impostazioni**.
2. Seleziona l'account da eliminare nell'elenco di account utente.
3. Uno accanto all'account utente, toccare l'icona di modifica.
4. Nella parte inferiore di eliminare l'account utente, tocca **rimuovere questo account** .
5. Puoi anche modificare l'account utente e toccare **Salva**.

## Passa la sessione Desktop remoto
Quando si avvia una connessione desktop remoto, sono disponibili strumenti disponibili che è possibile utilizzare per passare la sessione.

### Avviare una connessione Desktop remoto

1. Tocca la connessione Desktop remoto per avviare la sessione.
2. Se non hai salvato le credenziali per la connessione, ti verrà chiesto di fornire un **nome utente** e **Password**.
3. Se viene richiesto di verificare il certificato per il desktop remoto, leggi le informazioni e assicurati che sia un PC attendibile prima toccando **Connect**. Puoi anche selezionare Accetta sempre questo certificato **non chiedere informazioni su questo nuovo certificato** .

### Barra di connessione

Consente di barra di connessione che accedere ai controlli di spostamento aggiuntivi. Per impostazione predefinita, la barra di connessione viene posizionata centrale nella parte superiore dello schermo. Tocca e trascinare la barra verso sinistra o a destra per spostare la.

- **Controllo di panning** - il controllo pan Abilita schermo per essere ingrandita e spostate. Tieni presente che il controllo pan è disponibile solo nei dispositivi abilitati per il tocco e usando la modalità tocco diretto.
   - Abilitare / disabilitare il controllo pan: toccare l'icona di panoramica nella barra di connessione per visualizzare il controllo di panoramica e zoom lo schermo. Toccare l'icona di panoramica nella barra di connessione nuovamente per nascondere il controllo e restituire lo schermo alla sua risoluzione originale.
   - Utilizzare il controllo pan - tocco premuto il controllo zoom e quindi trascinare nella direzione che si desidera spostare lo schermo.
   - Spostare il controllo pan - doppio tocco e contenere il controllo di zoom per spostare il controllo sullo schermo.
- **Opzioni aggiuntive** - toccare l'icona di opzioni aggiuntive per visualizzare la selezione di sessione barra e comando della barra (vedi di seguito).
- **Tastiera** , tocco, l'icona della tastiera per visualizzare o nascondere la tastiera su schermo. Il controllo zoom viene visualizzato automaticamente quando viene visualizzata la tastiera.

### Barra dei comandi

Tocca **...** sulla barra di connessione per visualizzare la barra dei comandi sul lato destro dello schermo.

- **Home** - Usa il pulsante Home per tornare al Centro connessione della barra dei comandi.
  - In alternativa è possibile utilizzare il pulsante Indietro per la stessa azione.
  - La sessione attiva non verrà disconnesso.
  - Ciò consente di avviare le connessioni aggiuntive.
- **Disconnetti** - Usa il pulsante di disconnessione per terminare la connessione.
  - L'App rimarrà attivo, purché la sessione è terminata non nel PC remoto.
- **A schermo intero** - entra o esce dalla modalità a schermo intero.
- **Tocco e Mouse** : È possibile alternare le modalità mouse (tocco diretto e del puntatore del Mouse).

### Utilizzo diretto movimenti di tocco e le modalità mouse in una sessione remota

Due modalità mouse sono disponibili per interagire con la sessione.
- **Tocco diretto**: passa tutti i contatti di tocco alla sessione deve essere interpretato da remoto.
  - Usato nello stesso modo che usi Windows dotati di touchscreen.
- **Puntatore del mouse**: Trasforma il touchscreen locali in un touchpad di grandi dimensioni che consente di spostare il puntatore del mouse nella sessione.
  - Usato nello stesso modo che usi Winodws con un touchpad.

> [!NOTE]
> Interazione con Windows 8 o versione successiva i movimenti di tocco native sono supportati in modalità tocco diretto. 

| Modalità mouse    | Operazione con il mouse      | Movimento                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Tocco diretto  | Pulsante sinistro           | tocco con 1 dita                                                          |
| Tocco diretto  | Fare clic con il pulsante destro del mouse          | 1 tocco delle dita e pressione prolungata                                                 |
| Puntatore del mouse | Pulsante sinistro           | tocco con 1 dita                                                          |
| Puntatore del mouse | Fare clic con il tasto sinistro del mouse e trascinare  | tocca due volte 1 DITA e pressione prolungata, quindi trascinare.                               |
| Puntatore del mouse | Fare clic con il pulsante destro del mouse          | tocca il dito 2                                                          |
| Puntatore del mouse | Fare clic con il tasto destro del mouse e trascinare | 2 doppio tocco DITA e pressione prolungata, quindi trascinare.                               |
| Puntatore del mouse | Rotellina del mouse          | 2 dito tocca e tieni premuto, quindi trascinare verso l'alto o verso il basso                           |
| Puntatore del mouse | Zoom                 | Usare 2 DITA e avvicinamento delle dita per eseguire lo zoom avanti o spostare tra di loro dita per lo zoom indietro. |

> [!TIP]
> Domande e i commenti sono sempre iniziale. Tuttavia, non è possibile inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, Vai al [forum di client Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Hai un suggerimento di funzionalità? Facci sapere tramite l' [Hub di Feedback](feedback-hub://?tabid=2&contextid=605).
