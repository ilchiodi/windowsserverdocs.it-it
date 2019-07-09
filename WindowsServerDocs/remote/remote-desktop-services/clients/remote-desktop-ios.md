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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66446699"
---
# <a name="get-started-with-remote-desktop-on-ios"></a>Introduzione a Desktop remoto in iOS

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Il client Desktop remoto per iOS consente di usare desktop, risorse e app di Windows da un dispositivo iOS (iPhone e iPad).

Segui le informazioni riportate di seguito per iniziare. In caso di dubbi, vedi le [domande frequenti](remote-desktop-client-faq.md).

> [!NOTE]
> - Se vuoi conoscere le nuove versioni per il client iOS, vedi [Novità per Desktop remoto in iOS](ios-whatsnew.md).
> - Il client iOS supporta i dispositivi che eseguono iOS 6.x e versioni successive.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Ottenere il client Desktop remoto e iniziare a usarlo

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Scaricare il client Desktop remoto dallo store iOS
Seguire questi passaggi per iniziare con Desktop remoto nel dispositivo iOS:

1. Scaricare il client di Desktop remoto Microsoft da [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configura il PC per accettare le connessioni remote](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Aggiungi una [connessione Desktop remoto](#add-a-remote-desktop-connection) o una [risorsa remota](#add-a-remote-resource). Una connessione consente di connettersi direttamente a un PC Windows, mentre una risorsa remota consente di usare un programma RemoteApp, un desktop basato su sessione o un desktop virtuale pubblicato in locale tramite connessioni RemoteApp e Desktop. Questa funzionalità è in genere disponibile negli ambienti aziendali.

### <a name="download-the-remote-desktop-ios-beta-client"></a>Scaricare il client Desktop remoto iOS beta
Nel dispositivo iOS segui [queste istruzioni](https://aka.ms/rdiosbeta) per scaricare il client Desktop remoto iOS beta.

### <a name="add-a-remote-desktop-connection"></a>Aggiungere una connessione Desktop remoto

Per creare una connessione Desktop remoto: 
1. Nella scelta dei Centro connessioni **+** , e quindi toccare **aggiungere computer o Server**.
2. Immettere le informazioni seguenti per la connessione desktop remoto:
   - **Nome PC** : il nome del computer. Puoi usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. È anche possibile aggiungere informazioni sulla porta al nome del computer (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Nome utente** : il nome utente da utilizzare per accedere al computer remoto. Puoi usare i formati seguenti: *user_name*, *domain\user_name* oppure <em>user_name@domain.com</em>. È inoltre possibile specificare se per la richiesta di un nome utente e una password.
3. È inoltre possibile impostare le opzioni aggiuntive seguenti:
   - **Nome descrittivo (facoltativo)** : un nome facile da ricordare per il computer si è connessi. È possibile utilizzare qualsiasi stringa, ma se non si specifica un nome descrittivo, viene visualizzato il nome del computer.
   - **Gateway (facoltativo)** -gateway Desktop remoto che si desidera utilizzare per connettersi a desktop virtuali, programmi RemoteApp e desktop basati su sessione su una rete aziendale interna. Ottenere le informazioni sul gateway, dall'amministratore di sistema.
   - **Suono** : selezionare il dispositivo da utilizzare per l'audio durante la sessione remota. È possibile scegliere di riprodurre un suono nei dispositivi locali, il dispositivo remoto oppure non funziona.
   - **Scambia i pulsanti del mouse** : ogni volta che un movimento del mouse viene inviato un comando con il pulsante sinistro del mouse, invia lo stesso comando con il pulsante destro del mouse. Ciò è necessario se il PC remoto è configurato per la modalità mouse per mancini.
   - **Modalità amministratore** -connettersi a una sessione di amministrazione in un server che esegue Windows Server 2003 o versione successiva.
4. Toccare **salvare**.

È necessario modificare queste impostazioni? Premere e tenere i desktop che si desidera modificare e quindi toccare l'icona di impostazioni. 

### <a name="add-a-remote-resource"></a>Aggiungere una risorsa remota
Risorse remote sono programmi RemoteApp, desktop basati su sessione e i desktop virtuali pubblicati tramite connessione RemoteApp e Desktop.

- L'URL visualizza il collegamento al server Accesso Web Desktop remoto che consente di accedere a connessioni RemoteApp e Desktop.
- Vengono elencate le connessioni RemoteApp e Desktop configurate.

Per aggiungere una risorsa remota:

1. Nella schermata connessione Center toccare **+** , e quindi toccare **aggiungere risorse Remote**. 
2. Immettere le informazioni per la risorsa remota:
   - **URL del feed** -l'URL del server Accesso Web desktop remoto. È inoltre possibile immettere l'account di posta elettronica aziendale in questo campo: in questo modo il client per cercare il Server di accesso Web desktop remoto associato l'indirizzo di posta elettronica.
   - **Nome utente** -il nome utente da utilizzare per il server Accesso Web desktop remoto si è connessi.
   - **Password** -la password da utilizzare per il server Accesso Web desktop remoto si è connessi.
3. Toccare **salvare**.

Verranno visualizzate nel Centro connessioni di risorse remote.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Connettersi a Gateway Desktop remoto per accedere alle risorse interne

Gateway Desktop remoto ti consente di connetterti a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. È possibile creare e gestire il gateway tramite il client Desktop remoto.

Per impostare un nuovo gateway:

1. Nel Centro connessioni, toccare **Impostazioni > gateway**. 
2. Toccare **gateway Desktop remoto aggiungere**.
3. Immettere le informazioni seguenti:
   - **Nome del server** : il nome del computer di cui si desidera utilizzare come gateway. Puoi usare il nome di un computer Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere informazioni sulla porta al nome del server (ad esempio: **RDGateway:443** oppure **10.0.0.1:443**).
   - **Nome utente** -il nome utente e la password da utilizzare per il gateway Desktop remoto si connette. È inoltre possibile selezionare **utilizzare le credenziali di connessione** da utilizzare il medesimo nome utente e la password come quelle utilizzate per la connessione desktop remoto.


## <a name="manage-your-user-accounts"></a>Gestire gli account utente 

Quando ci si connette a una risorsa remota o desktop, è possibile salvare gli account utente per selezionare nuovamente. È possibile gestire gli account utente utilizzando il client Desktop remoto.

Per creare un nuovo account utente:

1. Nel Centro connessioni, toccare **impostazioni**, e quindi toccare **nomi utente**.
2. Toccare **aggiungere Account utente**.
3. Immettere le informazioni seguenti:
   - **Nome utente** -il nome dell'utente da salvare per l'utilizzo con una connessione remota. Puoi immettere il nome utente in uno dei formati seguenti: user_name, domain\user_name o user_name@domain.com.
   - **Password** -la password per l'utente specificato. Ogni account utente che si desidera salvare da utilizzare per le connessioni remote deve avere una password associata.
4. Toccare **salvare**, e quindi toccare **impostazioni**.
5. Toccare **eseguita** per salvare la nuova configurazione.

Per eliminare un account utente:

1. Nel Centro connessioni, toccare **Impostazioni > utenti**.
2. Scorrere verso la riga da sinistra a destra per selezionare l'utente.
3. Toccare **eliminare**.



## <a name="navigate-the-remote-desktop-session"></a>Esplorare la sessione Desktop remoto
Quando avvii una sessione Desktop remoto, sono disponibili strumenti che puoi usare per esplorare la sessione.

### <a name="start-a-remote-desktop-connection"></a>Avviare una connessione Desktop remoto

1. Toccare la connessione desktop remoto per avviare la sessione di desktop remota. 
2. Se viene chiesto di verificare il certificato per il desktop remoto, toccare **Accept**. È possibile scegliere di accettare sempre scorrendo il  **non visualizzare più questo messaggio per le connessioni al computer** passare **ON**. 

### <a name="connection-bar"></a>Barra delle connessioni

Consente di barra di connessione di accedere ai controlli di spostamento aggiuntive. 

- **Controllo panoramica**: il controllo panoramica consente di ingrandire e spostare la schermata. Si noti che il controllo zoom è disponibile esclusivamente tramite tocco diretto.
   - Abilitare o disabilitare il controllo panoramica: tocca l'icona panoramica sulla barra delle connessioni per visualizzare il controllo panoramica e ingrandire la schermata. Toccare l'icona Zoom nella barra di connessione per nascondere il controllo e restituire la schermata per la risoluzione originale.
   - Usare il controllo panoramica: tieni premuto il controllo panoramica e quindi trascinalo nella direzione in cui vuoi spostare la schermata.
   - Spostare il controllo panoramica: tocca due volte e tieni premuto il controllo panoramica per spostarlo sullo schermo.
- **Nome connessione**: viene visualizzato il nome della connessione corrente. Toccare il nome della connessione per visualizzare la barra di selezione di sessione.
- **Tastiera**: tocca l'icona della tastiera per visualizzare o nascondere la tastiera. Il controllo dettaglio viene visualizzato automaticamente quando viene visualizzata la tastiera.
- **Spostare la barra delle connessioni**: tieni premuta la barra delle connessioni e quindi trascinala in una nuova posizione nella parte superiore della schermata.

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

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usare i movimenti tocco e le modalità del mouse in una sessione remota

Il client utilizza i movimenti tocco standard. È inoltre possibile utilizzare i movimenti tocco per replicare le azioni del mouse sul desktop remoto. Le modalità mouse disponibili sono definite nella tabella seguente.

> [!NOTE]
> Interazione con Windows 8 o versioni successive i movimenti di tocco nativo sono supportati in modalità tocco diretto. Per altre informazioni sui movimenti in Windows 8, vedi [Tocco: scorrimento rapido, tocco e altro](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

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

Il [client Desktop remoto iOS beta](https://aka.ms/rdiosbeta) supporta i mouse fisici Swiftpoint GT e ProPoint. Swiftpoint offre uno [sconto esclusivo](https://www.swiftpoint.com/microsoft/) sul modello GT per gli utenti del client iOS beta.

Il client iOS supporta attualmente solo mouse Swiftpoint. Fai riferimento alla pagina [Novità del client iOS](ios-whatsnew.md) e all'[App Store per iOS](https://aka.ms/rdios) per notizie sul supporto per altri dispositivi in futuro.

## <a name="use-a-keyboard-in-a-remote-session"></a>Usare una tastiera in una sessione remota

Puoi usare una tastiera su schermo o una tastiera fisica in una sessione remota.

Per le tastiere su schermo, usa il pulsante sul lato destro della barra al di sopra della tastiera per passare dalla tastiera standard alla tastiera aggiuntiva e viceversa.

Se è abilitato Bluetooth per il dispositivo iOS, il client rileva automaticamente la tastiera Bluetooth.

Tieni presente che, a causa delle limitazioni del sistema operativo, i tasti speciali quali CTRL, Opzione e Funzione non si comportano come previsto con una tastiera Bluetooth. I tasti seguenti funzionano:

- Tasti alfanumerici
- Tasti di direzione
- TAB: TAB funziona, ma MAIUSC+TAB non funziona
- HOME/Pos1: ALT+freccia SINISTRA = HOME
- FINE: ALT+freccia DESTRA = FINE
- PGSU: ALT+freccia SU = PGSU
- PGGIÙ: ALT+freccia GIÙ = PGGIÙ
- Seleziona tutto: COMANDO+A = CTRL+A (Seleziona tutto nella maggior parte dei programmi)
- Taglia: COMANDO+X = CTRL+X (Taglia nella maggior parte dei programmi)
- Copia: COMANDO+C = CTRL+C (Copia nella maggior parte dei programmi)
- Incolla: COMANDO+V = CTRL+V (Incolla nella maggior parte dei programmi)
- Simboli: ALT+tasti alfanumerici produce simboli diversi a seconda della lingua configurata

> [!TIP]
> Domande e commenti sono sempre Benvenuti. Tuttavia, NON inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, andare alla [forum su client di Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Avete suggerimenti funzionalità? Comunicaci nel [forum dedicato agli utenti di client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

