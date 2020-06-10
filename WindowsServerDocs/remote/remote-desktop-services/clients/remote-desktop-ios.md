---
title: Introduzione al client iOS
description: Informazioni su come configurare il client Desktop remoto per iOS
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: Heidilohr
manager: lizross
ms.author: helohr
date: 06/01/2020
ms.localizationpriority: medium
ms.openlocfilehash: 978c42b70f1c74a009640f7feda8dee7a82c9cff
ms.sourcegitcommit: 75b4cf49dd918ff98258dcae6e6e8d7825c9adec
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/02/2020
ms.locfileid: "84269229"
---
# <a name="get-started-with-the-ios-client"></a>Introduzione al client iOS

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Il client Desktop remoto per iOS consente di usare desktop, risorse e app di Windows da un dispositivo iOS (iPhone e iPad).

Segui le informazioni riportate di seguito per iniziare. In caso di dubbi, vedi le [domande frequenti](remote-desktop-client-faq.md).

> [!NOTE]
> - Se vuoi conoscere le nuove versioni per il client iOS, consulta la pagina [Novità di Desktop remoto per iOS](ios-whatsnew.md).
> - Il client iOS supporta i dispositivi che eseguono iOS 6.x e versioni successive.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Ottenere il client Desktop remoto e iniziare a usarlo

Questa sezione illustra come scaricare e configurare il client Desktop remoto per iOS.

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Scaricare il client Desktop remoto dallo store iOS

Per prima cosa è necessario scaricare il client e configurare il computer per la connessione alle risorse remote.

Per scaricare il client:

1. Scarica il client Desktop remoto Microsoft da [iOS App Store](https://aka.ms/rdios) o da [iTunes](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configura il PC per accettare le connessioni remote](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).

### <a name="add-a-pc"></a>Aggiungere un PC

Dopo aver scaricato il client e aver configurato il computer in uso in modo che accetti le connessioni remote, è necessario aggiungere un computer.

Per aggiungere un PC:

1. Nel Centro connessioni toccare **+** e quindi toccare **Aggiungere computer**.
2. Immettere le informazioni seguenti:
   - **Nome PC** : il nome del computer. È possibile usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del computer (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Nome utente**: il nome utente da usare per accedere al computer remoto. Puoi usare i formati seguenti: *user_name*, *domain\user_name* oppure `user_name@domain.com`. Puoi anche selezionare **Ask when required** (Chiedi se necessario) per visualizzare la richiesta di immettere un nome utente e una password se necessario.
3. È inoltre possibile impostare le opzioni aggiuntive seguenti:
   - **Nome descrittivo (facoltativo)** : un nome facile da ricordare per il PC a cui sei connesso. Puoi usare qualsiasi stringa, ma se non specifichi un nome descrittivo, al suo posto viene visualizzato il nome del PC.
   - **Gateway (facoltativo)** -gateway Desktop remoto che si desidera utilizzare per connettersi a desktop virtuali, programmi RemoteApp e desktop basati su sessione su una rete aziendale interna. Ottenere le informazioni sul gateway, dall'amministratore di sistema.
   - **Suono** : selezionare il dispositivo da utilizzare per l'audio durante la sessione remota. È possibile scegliere di riprodurre un suono nei dispositivi locali, il dispositivo remoto oppure non funziona.
   - **Scambia i pulsanti del mouse** : ogni volta che un movimento del mouse viene inviato un comando con il pulsante sinistro del mouse, invia lo stesso comando con il pulsante destro del mouse. Lo scambio dei pulsanti del mouse si rende necessario se il computer remoto è configurato per la modalità mouse per mancini.
   - **Modalità amministratore** -connettersi a una sessione di amministrazione in un server che esegue Windows Server 2003 o versione successiva.
   - **Appunti**: scegli se reindirizzare il testo e le immagini degli Appunti al PC.
   - **Archiviazione**: scegli se reindirizzare l'archiviazione al PC.
4. Toccare **salvare**.

È necessario modificare queste impostazioni? Tenere premuto il desktop che si desidera modificare e quindi toccare l'icona delle impostazioni.

### <a name="add-a-workspace"></a>Aggiungere un'area di lavoro

Per ottenere un elenco di risorse gestite a cui puoi accedere in iOS, aggiungi un'area di lavoro sottoscrivendo il feed fornito dall'amministratore.

Per aggiungere un'area di lavoro:

1. Nel centro connessioni tocca **+** e quindi **Aggiungi area di lavoro**.
2. Nel campo URL del feed immetti l'URL del feed da aggiungere. Può essere un URL o un indirizzo e-mail.
   - Nel caso di un URL, usa quello fornito dall'amministratore. Questo URL è in genere <https://rdweb.wvd.microsoft.com>.
   - Nel caso dell'e-mail, immetti il tuo indirizzo e-mail. Inserendo l’indirizzo e-mail si indica al client di cercare un URL associato a tale indirizzo, se l'amministratore ha configurato il server in questo modo.
3. Tocca **Next** (Avanti).
4. Specifica le credenziali quando richiesto.
   - Per **Nome utente**, specifica il nome utente di un account con l'autorizzazione per accedere alle risorse.
   - Per **Password** specifica la password dell'account.
   - È anche possibile che ti venga chiesto di fornire informazioni aggiuntive, a seconda delle impostazioni configurate dall'amministratore per l'autenticazione.
5. Toccare **salvare**.

Al termine, il Centro connessioni visualizza le risorse remote.

Una volta sottoscritto un feed, il relativo contenuto verrà aggiornato automaticamente a intervalli regolari. È possibile che vengano aggiunte, cambiate o rimosse risorse in base alle modifiche apportate dall'amministratore.

## <a name="manage-your-user-accounts"></a>Gestire gli account utente

Quando ti connetti a un PC o a un'area di lavoro, puoi salvare gli account utente da selezionare di nuovo.

Per creare un nuovo account utente:

1. In Connection Center (Centro connessioni) tocca **Settings** (Impostazioni) e quindi **User Accounts** (Account utente).
2. Toccare **aggiungere Account utente**.
3. Immettere le informazioni seguenti:
   - **Nome utente** -il nome dell'utente da salvare per l'utilizzo con una connessione remota. È possibile immettere il nome utente in uno dei formati seguenti: `user_name`, `domain\user_name` o `user_name@domain.com`.
   - **Password** -la password per l'utente specificato.
4. Toccare **salvare**.

Per eliminare un account utente:

1. In Connection Center (Centro connessioni) tocca **Settings** (Impostazioni) e quindi **User Accounts** (Account utente).
2. Seleziona l'account che vuoi eliminare.
3. Toccare **eliminare**.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Connettersi a un gateway Desktop remoto per accedere alle risorse interne

Gateway Desktop remoto ti consente di connetterti a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. È possibile creare e gestire il gateway tramite il client Desktop remoto.

Per impostare un nuovo gateway:

1. In Connection Center (Centro connessioni) tocca **Settings** > **Gateways** (Impostazioni>Gateway).
2. Tocca **Aggiungi gateway**.
3. Immettere le informazioni seguenti:
   - **Nome gateway**: il nome del computer da usare come gateway. È possibile usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere informazioni sulla porta al nome del server, ad esempio **RDGateway:443** o **10.0.0.1:443**.
   - **Nome utente**: il nome utente e la password da usare per il servizio Gateway Desktop remoto a cui ti connetti. È inoltre possibile selezionare **Utilizzare le credenziali di connessione** per usare lo stesso nome utente e la stessa password usati per la connessione al desktop remoto.

## <a name="navigate-the-remote-desktop-session"></a>Esplorare la sessione Desktop remoto

Questa sezione descrive gli strumenti che è possibile usare per esplorare la sessione di Desktop remoto.

### <a name="start-a-remote-desktop-connection"></a>Avviare una connessione Desktop remoto

1. Toccare la connessione desktop remoto per avviare la sessione di desktop remota.
2. Se viene chiesto di verificare il certificato per il desktop remoto, toccare **Accetta**. Per fare in modo che l’opzione Accetta sia l’impostazione predefinita, impostare **Non visualizzare più questo messaggio per le connessioni a questo computer** su **Abilitato**.

### <a name="connection-bar"></a>Barra delle connessioni

Consente di barra di connessione di accedere ai controlli di spostamento aggiuntive.

- **Controllo panoramica**: il controllo panoramica consente di ingrandire e spostare la schermata. Il controllo panoramica è disponibile solo usando il tocco diretto.
   - Per abilitare o disabilitare il controllo panoramica, toccare l'icona Zoom sulla barra di connessione per visualizzare il relativo controllo. Quando il controllo della panoramica è attivo, viene applicato lo zoom avanti nella schermata. Toccare di nuovo l'icona Zoom nella barra di connessione per nascondere il controllo e ripristinare la schermata alla risoluzione originale.
   - Per usare il controllo della panoramica, toccarlo e tenerlo premuto. Tenendo premuto, trascinare le dita nella direzione in cui si vuole spostare la schermata.
   - Per spostare il controllo della panoramica, effettuare un doppio tocco e tenere premuto il controllo per spostarlo nella schermata.
- **Nome connessione**: viene visualizzato il nome della connessione corrente. Toccare il nome della connessione per visualizzare la barra di selezione di sessione.
- **Tastiera**: tocca l'icona della tastiera per visualizzare o nascondere la tastiera. Il controllo dettaglio viene visualizzato automaticamente quando viene visualizzata la tastiera.
- **Spostare la barra delle connessioni**: Toccare e tenere premuta la barra di connessione. Tenendo premuto, trascinare la barra nella nuova posizione. Rilasciare la barra per collocarla nella nuova posizione.

### <a name="session-selection"></a>Selezione di sessione

È possibile avere più connessioni aperti per più computer contemporaneamente. Toccare la barra delle connessioni per visualizzare la barra di selezione di sessione sul lato sinistro della schermata. La barra di selezione sessione consente di visualizzare le connessioni aperte e passare tra loro.

Ecco cosa si può fare con la barra di selezione della sessione:

- Per passare da un'app all'altra in una sessione di risorse remote aperta, toccare il menu Expander e scegliere un'app dall'elenco.
- Toccare **Avvia nuova** per avviare una nuova sessione, quindi scegliere una sessione dall'elenco di quelle disponibili.
- Toccare l'icona **X** sul lato sinistro del riquadro della sessione per disconnettersi dalla sessione.

### <a name="command-bar"></a>Barra dei comandi

Barra dei comandi sostituito l'utilità barra a partire dalla versione 8.0.1. È possibile usare la barra dei comandi per scambiare le modalità del mouse e tornare al centro connessioni.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usare i movimenti tocco e le modalità del mouse in una sessione remota

Il client utilizza i movimenti tocco standard. È inoltre possibile utilizzare i movimenti tocco per replicare le azioni del mouse sul desktop remoto. Le modalità mouse disponibili sono definite nella tabella seguente.

> [!NOTE]
> In Windows 8 o versioni successive, i movimenti di tocco nativi sono supportati nella modalità tocco diretto. Per altre informazioni sui gesti in Windows 8, vedere [Tocco: scorrimento rapido, tocco e altro](https://windows.microsoft.com/windows-8/touch-swipe-tap-beyond).

| Modalità mouse    | Operazione con il mouse      | Movimento                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Tocco diretto con  | Clic con il pulsante sinistro           | Tocco con un dito                                               |
| Tocco diretto con  | Fare clic con il pulsante destro del mouse su          | Tocco e pressione con un dito                                      |
| Puntatore del mouse | Clic con il pulsante sinistro           | Tocco con un dito                                               |
| Puntatore del mouse | Clic con il pulsante sinistro e trascinamento  | Tocco e pressione con un dito, quindi trascinamento                   |
| Puntatore del mouse | Fare clic con il pulsante destro del mouse su          | Tocco con due dita                                              |
| Puntatore del mouse | Clic con il pulsante destro del mouse e trascinamento | Doppio tocco e pressione con due dita, quindi trascinamento                    |
| Puntatore del mouse | Rotellina del mouse          | Doppio tocco e pressione con due dita, quindi trascinamento verso l'alto o verso il basso                |
| Puntatore del mouse | Zoom                 | Pizzicamento con due dita per fare zoom avanti o allontanamento delle dita per fare zoom indietro |

## <a name="supported-input-devices"></a>Dispositivi di input supportati

Il client dispone del [supporto mouse bluetooth](https://support.apple.com/HT210546) per iOS 13 e iPadOS come funzionalità di accessibilità. È possibile usare i mouse Swiftpoint GT o ProPoint per una maggiore integrazione del mouse. Il client supporta anche le tastiere esterne compatibili con iOS e iPadOS. 

Per altre informazioni sul supporto dei dispositivi, vedi [Novità del client iOS](ios-whatsnew.md) e [iOS App Store](https://aka.ms/rdios).

> [!TIP]
> Swiftpoint offre uno [sconto esclusivo sul mouse ProPoint](https://www.swiftpoint.com/microsoft) per gli utenti client iOS.

## <a name="use-a-keyboard-in-a-remote-session"></a>Usare una tastiera in una sessione remota

Puoi usare una tastiera su schermo o una tastiera fisica in una sessione remota.

Per le tastiere su schermo, usa il pulsante sul lato destro della barra al di sopra della tastiera per passare dalla tastiera standard alla tastiera aggiuntiva e viceversa.

Se è abilitato Bluetooth per il dispositivo iOS, il client rileva automaticamente la tastiera Bluetooth.

Anche se alcune combinazioni di tasti potrebbero non funzionare come previsto in una sessione remota, molte combinazioni comuni dei tasti di Windows, ad esempio CTRL+C, CTRL+V e ALT+TAB, funzionano.

> [!TIP]
> Domande e commenti sono sempre Benvenuti. Tuttavia, se si pubblicano richieste di supporto o commenti e suggerimenti sul prodotto nella sezione dei commenti di questo articolo, non ci sarà possibile rispondere. Se si desidera ricevere assistenza o la risoluzione di un problema del client, è consigliabile visitare il [forum sui client Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winrdc) e avviare un nuovo thread. Per suggerimenti su una funzionalità, è possibile proporli usando il forum [per gli utenti client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
