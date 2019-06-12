---
title: Introduzione a Desktop remoto in iOS
description: Informazioni su come configurare il client Desktop remoto per iOS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: lizap
manager: dongill
ms.author: elizapo
date: 01/13/2017
ms.localizationpriority: medium
ms.openlocfilehash: 71fe969de4d21f7fa3c134b0f80fc7f69e5b2da8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446699"
---
# <a name="get-started-with-remote-desktop-on-ios"></a>Introduzione a Desktop remoto in iOS

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

È possibile usare il client Desktop remoto per iOS per lavorare con le app, risorse e desktop Windows da un dispositivo iOS (iPhone e iPad).

Usare le informazioni seguenti per iniziare. Assicurarsi di consultare il [domande frequenti su](remote-desktop-client-faq.md) se hai domande.

> [!NOTE]
> - Conoscere le nuove versioni per il client iOS? Scopri [quali sono le novità per il Desktop remoto in iOS?](ios-whatsnew.md)
> - Il client di iOS supporta i dispositivi che eseguono iOS 6.x e versioni successive.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Ottenere il client Desktop remoto e iniziare a usarlo

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Scaricare il client Desktop remoto dallo store di iOS
Seguire questi passaggi per iniziare con Desktop remoto nel dispositivo iOS:

1. Scaricare il client di Desktop remoto Microsoft da [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configurare il PC per accettare le connessioni remote](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Aggiungere un [connessione Desktop remoto](#add-a-remote-desktop-connection) o una [risorsa remota](#add-a-remote-resource). Si usa una connessione per connettersi a un direttamente a un PC Windows e una risorsa remota. utilizzare un programma RemoteApp, desktop basati su sessione o un desktop virtuale pubblicato locale utilizzando Connessione RemoteApp e Desktop. Questa funzionalità è in genere disponibile negli ambienti aziendali.

### <a name="download-the-remote-desktop-ios-beta-client"></a>Scaricare il client di Desktop remoto iOS Beta
Nel dispositivo iOS, seguire [queste istruzioni](https://aka.ms/rdiosbeta) per scaricare il client di Desktop remoto iOS versione Beta.

### <a name="add-a-remote-desktop-connection"></a>Aggiungere una connessione Desktop remoto

Per creare una connessione desktop remoto: 
1. Nella scelta dei Centro connessioni **+** , e quindi toccare **aggiungere computer o Server**.
2. Immettere le informazioni seguenti per la connessione desktop remoto:
   - **Nome PC** : il nome del computer. Può trattarsi di un nome di computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del computer (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Nome utente** : il nome utente da utilizzare per accedere al computer remoto. È possibile usare i seguenti formati: *user_name*, *DOMINIO\nome_utente.* , o <em>user_name@domain.com</em>. È inoltre possibile specificare se per la richiesta di un nome utente e una password.
3. È inoltre possibile impostare le opzioni aggiuntive seguenti:
   - **Nome descrittivo (facoltativo)** : un nome facile da ricordare per il computer si è connessi. È possibile utilizzare qualsiasi stringa, ma se non si specifica un nome descrittivo, viene visualizzato il nome del computer.
   - **Gateway (facoltativo)** -gateway Desktop remoto che si desidera utilizzare per connettersi a desktop virtuali, programmi RemoteApp e desktop basati su sessione su una rete aziendale interna. Ottenere le informazioni sul gateway, dall'amministratore di sistema.
   - **Suono** : selezionare il dispositivo da utilizzare per l'audio durante la sessione remota. È possibile scegliere di riprodurre un suono nei dispositivi locali, il dispositivo remoto oppure non funziona.
   - **Scambia i pulsanti del mouse** : ogni volta che un movimento del mouse viene inviato un comando con il pulsante sinistro del mouse, invia lo stesso comando con il pulsante destro del mouse. Ciò è necessario se il computer remoto è configurato per la modalità mouse battitori.
   - **Modalità amministratore** -connettersi a una sessione di amministrazione in un server che esegue Windows Server 2003 o versione successiva.
4. Toccare **salvare**.

È necessario modificare queste impostazioni? Premere e tenere i desktop che si desidera modificare e quindi toccare l'icona di impostazioni. 

### <a name="add-a-remote-resource"></a>Aggiungere una risorsa remota.
Risorse remote sono programmi RemoteApp, desktop basati su sessione e i desktop virtuali pubblicati tramite connessione RemoteApp e Desktop.

- L'URL viene visualizzato il collegamento al server Accesso Web desktop remoto che consente di accedere a connessione RemoteApp e Desktop.
- Sono elencati i configurato Connessione RemoteApp e Desktop.

Per aggiungere una risorsa remota:

1. Nella schermata connessione Center toccare **+** , e quindi toccare **aggiungere risorse Remote**. 
2. Immettere le informazioni per la risorsa remota:
   - **URL del feed** -l'URL del server Accesso Web desktop remoto. È inoltre possibile immettere l'account di posta elettronica aziendale in questo campo: in questo modo il client per cercare il Server di accesso Web desktop remoto associato l'indirizzo di posta elettronica.
   - **Nome utente** -il nome utente da utilizzare per il server Accesso Web desktop remoto si è connessi.
   - **Password** -la password da utilizzare per il server Accesso Web desktop remoto si è connessi.
3. Toccare **salvare**.

Verranno visualizzate nel Centro connessioni di risorse remote.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Connettersi a un Gateway Desktop remoto per accedere alle risorse interne

Gateway Desktop remoto (Gateway Desktop remoto) consente di connettersi a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. È possibile creare e gestire il gateway tramite il client Desktop remoto.

Per impostare un nuovo gateway:

1. Nel Centro connessioni, toccare **Impostazioni > gateway**. 
2. Toccare **gateway Desktop remoto aggiungere**.
3. Immettere le informazioni seguenti:
   - **Nome del server** : il nome del computer di cui si desidera utilizzare come gateway. Può trattarsi di un nome di computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del server (ad esempio: **RDGateway:443** oppure **10.0.0.1: 443**).
   - **Nome utente** -il nome utente e la password da utilizzare per il gateway Desktop remoto si connette. È inoltre possibile selezionare **utilizzare le credenziali di connessione** da utilizzare il medesimo nome utente e la password come quelle utilizzate per la connessione desktop remoto.


## <a name="manage-your-user-accounts"></a>Gestire gli account utente 

Quando ci si connette a una risorsa remota o desktop, è possibile salvare gli account utente per selezionare nuovamente. È possibile gestire gli account utente utilizzando il client Desktop remoto.

Per creare un nuovo account utente:

1. Nel Centro connessioni, toccare **impostazioni**, e quindi toccare **nomi utente**.
2. Toccare **aggiungere Account utente**.
3. Immettere le informazioni seguenti:
   - **Nome utente** -il nome dell'utente da salvare per l'utilizzo con una connessione remota. È possibile immettere il nome utente in uno dei seguenti formati: nome_utente, DOMINIO\nome_utente., o user_name@domain.com.
   - **Password** -la password per l'utente specificato. Ogni account utente che si desidera salvare da utilizzare per le connessioni remote deve avere una password associata.
4. Toccare **salvare**, e quindi toccare **impostazioni**.
5. Toccare **eseguita** per salvare la nuova configurazione.

Per eliminare un account utente:

1. Nel Centro connessioni, toccare **Impostazioni > utenti**.
2. Scorrere verso la riga da sinistra a destra per selezionare l'utente.
3. Toccare **eliminare**.



## <a name="navigate-the-remote-desktop-session"></a>Passare la sessione Desktop remoto
Quando si avvia una sessione desktop remoto, sono disponibili alcuni strumenti che è possibile usare per passare la sessione.

### <a name="start-a-remote-desktop-connection"></a>Avviare una connessione Desktop remoto

1. Toccare la connessione desktop remoto per avviare la sessione di desktop remota. 
2. Se viene chiesto di verificare il certificato per il desktop remoto, toccare **Accept**. È possibile scegliere di accettare sempre scorrendo il  **non visualizzare più questo messaggio per le connessioni al computer** passare **ON**. 

### <a name="connection-bar"></a>Barra delle connessioni

Consente di barra di connessione di accedere ai controlli di spostamento aggiuntive. 

- **Pan in controllo**: Il controllo zoom consente la schermata di ingrandimento e spostati. Si noti che il controllo zoom è disponibile esclusivamente tramite tocco diretto.
   - Abilitare / disabilitare il controllo dettaglio: Toccare l'icona Zoom nella barra di connessione per visualizzare il controllo di panoramica e zoom schermata. Toccare l'icona Zoom nella barra di connessione per nascondere il controllo e restituire la schermata per la risoluzione originale.
   - Usare il controllo dettaglio: Toccare e tenere premuto il controllo dettaglio e quindi trascinare nella direzione per spostare la schermata.
   - Spostare il controllo dettaglio: Doppie toccare e tenere premuto il controllo dettaglio per spostare il controllo sullo schermo.
- **Nome della connessione**: Viene visualizzato il nome della connessione corrente. Toccare il nome della connessione per visualizzare la barra di selezione di sessione.
- **Tastiera**: Toccare l'icona di tastiera per visualizzare o nascondere la tastiera. Il controllo dettaglio viene visualizzato automaticamente quando viene visualizzata la tastiera.
- **Spostare la barra delle connessioni**: Toccare e tenere premuto la barra delle connessioni, quindi trascinare e rilasciare una nuova posizione nella parte superiore della schermata.

### <a name="session-selection"></a>Selezione di sessione
È possibile avere più connessioni aperti per più computer contemporaneamente. Toccare la barra delle connessioni per visualizzare la barra di selezione di sessione sul lato sinistro della schermata. La barra di selezione sessione consente di visualizzare le connessioni aperte e passare tra loro. 

- Spostarsi tra le applicazioni in una sessione aperta risorsa remota.

    Quando si è connessi a risorse remote, è possibile passare tra le applicazioni aperte all'interno di tale sessione, toccare il menu di espansione e scegliere dall'elenco di elementi disponibili.
- Avviare una nuova sessione

  È possibile avviare nuove applicazioni o le sessioni di desktop all'interno della connessione corrente: toccare **Avvia nuovo**, quindi scegliere dall'elenco di elementi disponibili.

- Disconnessione di una sessione

  Per disconnettere un tocco di sessione X sul lato sinistro del riquadro della sessione.

### <a name="command-bar"></a>Barra dei comandi

Barra dei comandi sostituito l'utilità barra a partire dalla versione 8.0.1. È possibile alternare le modalità di mouse e tornare al centro di connessione nella barra dei comandi.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usare i movimenti tocco e modalità di mouse in una sessione remota

Il client utilizza i movimenti tocco standard. È inoltre possibile utilizzare i movimenti tocco per replicare le azioni del mouse sul desktop remoto. Le modalità mouse disponibili sono definite nella tabella seguente.

> [!NOTE]
> Interazione con Windows 8 o versioni successive i movimenti di tocco nativo sono supportati in modalità tocco diretto. Per altre informazioni su Windows 8 vedere movimenti [Touch: Scorrere e toccare e oltre](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

| Modalità mouse    | Operazione con il mouse      | Movimento                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Tocco diretto con  | A sinistra fare clic su           | scelta di un 1 dito                                               |
| Tocco diretto con  | Fare clic          | 1 tap dito e tenere premuto                                      |
| Puntatore del mouse | A sinistra fare clic su           | scelta di un 1 dito                                               |
| Puntatore del mouse | Fare clic e trascinare  | 1 dito doppie toccare e tenere premuto, quindi trascinare.                    |
| Puntatore del mouse | Fare clic          | scelta di un dito 2                                               |
| Puntatore del mouse | Fare clic e trascinare | 2 il doppio tocco con un dito e tenere premuto, quindi trascinare.                    |
| Puntatore del mouse | Rotellina del mouse          | 2 dito toccare e tenere premuto, quindi trascinare verso l'alto o verso il basso                |
| Puntatore del mouse | Zoom                 | Zoom indietro / 2 dita per eseguire lo zoom avanti o distribuite 2 dita per eseguire lo zoom indietro |

## <a name="supported-input-devices"></a>Dispositivi di input supportati

Il [il client Desktop remoto iOS beta](https://aka.ms/rdiosbeta) supporta i topi Swiftpoint GT e ProPoint fisici. Swiftpoint viene offerta un' [sconto esclusivo](https://www.swiftpoint.com/microsoft/) sul totale complessivo per gli utenti di client beta di iOS.

Il client di iOS supporta attualmente solo Swiftpoint mice. Fare riferimento al [nuove funzionalità di client iOS](ios-whatsnew.md) pagina e il [iOS App Store](https://aka.ms/rdios) per notizie sul supporto per altri dispositivi in futuro.

## <a name="use-a-keyboard-in-a-remote-session"></a>Utilizzare una tastiera in una sessione remota

È possibile usare sullo schermo della tastiera o la tastiera fisica nella sessione remota.

Sullo schermo tastiere, usare il pulsante sul bordo destro della barra superiore della tastiera per spostarsi tra la tastiera standard e altra.

Se è abilitato Bluetooth per il dispositivo iOS, il client rileva automaticamente la tastiera Bluetooth.

Tenere presente che, a causa delle limitazioni del sistema operativo, tasti speciali, ad esempio Ctrl, opzione e la funzione non funzioneranno come previsto con una tastiera Bluetooth. Usare le chiavi seguenti:

- Tasti alfanumerici
- Tasti di direzione
- Scheda: Scheda funziona, ma Maiusc + Tab non funziona
- Home / Pos1: ALT + freccia sinistra = Home
- Fine: ALT + freccia destra = fine
- PGSU: ALT + freccia su = della pagina.
- Pagina giù: ALT + freccia giù = PGGIÙ
- Seleziona tutto: Comando + A = Ctrl + A (Select all nella maggior parte dei programmi)
- Taglia: Comando + X = Ctrl + X (Taglia nella maggior parte dei programmi)
- Copia: Comando + C = Ctrl + C (copia nella maggior parte dei programmi)
- Incolla: Comando + V = Ctrl + V (Incolla nella maggior parte dei programmi)
- Simboli: Tasti ALT + alfanumerico produrrà simboli diversi a seconda del linguaggio configurato

> [!TIP]
> Domande e commenti sono sempre Benvenuti. Tuttavia, NON inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, andare alla [forum su client di Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Avete suggerimenti funzionalità? Comunicaci nel [forum dedicato agli utenti di client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

