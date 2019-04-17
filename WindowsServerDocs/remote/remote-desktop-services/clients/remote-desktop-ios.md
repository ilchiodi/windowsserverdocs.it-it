---
title: Introduzione a Desktop remoto in iOS
description: Scopri come configurare il client Desktop remoto per iOS
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
ms.openlocfilehash: aab84dde3ded2a3d3f17f4bb318302321c444606
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297230"
---
# Introduzione a Desktop remoto in iOS

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

È possibile utilizzare il client Desktop remoto per iOS per lavorare con le app di Windows, le risorse e desktop dal tuo dispositivo iOS (iPhone e iPads).

Usa le informazioni seguenti per informazioni di base. Assicurati di consulta le [domande frequenti](remote-desktop-client-faq.md) , se hai domande.

> [!NOTE]
> - Conoscere i nuovi rilasci per il client iOS? Consulta [quali sono le novità per Desktop remoto in iOS?](ios-whatsnew.md)
> - Il client iOS supporta i dispositivi che eseguono iOS 6. x e versioni successive.

## Ottenere il client Desktop remoto e iniziare a usarlo

### Scaricare il client Desktop remoto da store iOS
Segui questi passaggi per iniziare con Desktop remoto nel tuo dispositivo iOS:

1. Scaricare il client Desktop remoto Microsoft da [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configurare il PC per accettare le connessioni remote](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Aggiungere una [connessione Desktop remoto](#add-a-remote-desktop-connection) o una [risorsa remota](#add-a-remote-resource). Usare una connessione di connettersi a un direttamente a un PC Windows e una risorsa remota per usare un programma RemoteApp, basata su sessione desktop o un desktop virtuale pubblicata in locale con connessione RemoteApp e Desktop. Questa funzionalità viene in genere disponibile negli ambienti aziendali.

### Scaricare il client Beta iOS di Desktop remoto
Nel tuo dispositivo iOS, segui [queste istruzioni](https://aka.ms/rdiosbeta) per scaricare il client di Desktop remoto iOS Beta.

### Aggiungere una connessione Desktop remoto

Per creare una connessione desktop remoto: 
1. Nel tocco di connessione Center **+** e quindi tocca **Aggiungi PC o un Server**.
2. Immetti le informazioni seguenti per la connessione desktop remota:
  - **Nome del PC** : il nome del computer. Può trattarsi di un nome di computer di Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere le informazioni sulla porta per il nome del PC (ad esempio, **MyDesktop:3389** o **10.0.0.1:3389**).
  - **Nome utente** : il nome utente da usare per accedere al PC remoto. Puoi usare i formati seguenti: *nome_utente*, *DOMINIO\nome_utente.* o *user_name@domain.com*. È inoltre possibile specificare se di richiedere un nome utente e password.
3. Puoi anche impostare le opzioni aggiuntive seguenti:
  - **Nome descrittivo (facoltativo)** : un nome per il PC si connette a facile da ricordare. È possibile utilizzare qualsiasi stringa, ma se non specifichi un nome descrittivo, viene visualizzato il nome del PC.
  - **Gateway (facoltativo)** – gateway di Desktop remoto che vuoi usare per connettersi a desktop virtuali, i programmi RemoteApp e desktop basati su sessione in una rete aziendale interna. Ottenere le informazioni sul gateway dall'amministratore di sistema.
  - **Audio** : selezionare il dispositivo da utilizzare per l'audio durante la sessione remota. Scegliere di riprodurre un suono nei dispositivi locali, il dispositivo remoto, o affatto.
  - Lo **scambio dei pulsanti del mouse** : ogni volta che un movimento del mouse invia un comando con il pulsante sinistro del mouse, invia lo stesso comando con il pulsante destro del mouse invece. Questo è necessario se il PC remoto è configurato per la modalità mouse sinistrorso.
  - **Modalità di amministratore** - connettersi a una sessione di amministrazione in un server che esegue Windows Server 2003 o versione successiva.
4. Tocca **salvare**.

Devi modificare queste impostazioni? Premi e tieni premuto il desktop che si desidera modificare e quindi toccare l'icona delle impostazioni. 

### Aggiungere una risorsa remota
Risorse remote sono programmi RemoteApp, basata su sessione desktop e desktop virtuali pubblicato con connessione RemoteApp e Desktop.

- L'URL visualizzato il collegamento al server di accesso Web desktop remoto che consente di accedere a connessione RemoteApp e Desktop.
- Sono elencate le configurato RemoteApp e le connessioni Desktop.

Per aggiungere una risorsa remota:

1. Nella schermata di connessione Center, tocca **+** e quindi tocca **Aggiungi risorse Remote**. 
2. Immettere le informazioni per la risorsa remota:
   - **URL Feed** - l'URL del server Accesso Web desktop remoto. È inoltre possibile immettere l'account di posta elettronica aziendali in questo campo: in questo modo il client per cercare il Server di accesso Web desktop remoto associato l'indirizzo di posta elettronica.
   - **Nome utente** - il nome utente da utilizzare per il server di accesso Web desktop remoto a che si connette.
   - **Password** : password da usare per il server di accesso Web desktop remoto a che si connette.
3. Tocca **salvare**.

Le risorse remote verranno visualizzate al centro di connessione.


## Connettersi a un Gateway Desktop remoto per accedere alle risorse interne

Un Gateway Desktop remoto (Gateway Desktop remoto) consente di connettersi a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. Puoi creare e gestire il gateway tramite il client Desktop remoto.

Per configurare un nuovo gateway:

1. Al centro di connessione, toccare **Impostazioni gt _ gateway**. 
2. Tocca **gateway Desktop remoto aggiungere**.
3. Immetti le informazioni seguenti:
  - **Nome del server** : il nome del computer che vuoi usare come gateway. Può trattarsi di un nome di computer di Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere le informazioni sulla porta per il nome del server (ad esempio: **RDGateway:443** o **10.0.0.1:443**).
  - **Nome utente** - il nome utente e password da usare per il gateway Desktop remoto a che si connette. Puoi anche selezionare **usare connessione credenziali** per usare lo stesso nome di utente e password come quelli usati per la connessione desktop remota.


## Gestire gli account utente 

Quando ti Connetti a un desktop o remoto risorse, è possibile salvare gli account utente di selezionare da nuovamente. Puoi gestire gli account utente con il client Desktop remoto.

Per creare un nuovo account utente:

1. Al centro di connessione, toccare **le impostazioni**e quindi tocca **Nomi utente**.
2. Tocca **aggiungere Account utente**.
3. Immetti le informazioni seguenti:
   - **Nome utente** - il nome dell'utente di salvare per l'uso con una connessione remota. Puoi immettere il nome utente in una qualsiasi delle seguenti formati: nome_utente, DOMINIO\nome_utente., o user_name@domain.com.
   - **Password** : la password per l'utente specificato. Ogni account utente che si desidera salvare per usare per le connessioni remote deve avere una password è associata.
4. Tocca **salvare**e quindi toccare **le impostazioni**.
5. Toccare **fatto** per salvare la nuova configurazione.

Per eliminare un account utente:

1. Al centro di connessione, toccare **Impostazioni gt _ nomi utente**.
2. Scorri rapidamente la riga da destra a sinistra per selezionare l'utente.
3. Tocca **eliminare**.



## Passa la sessione Desktop remoto
Quando si avvia una sessione desktop remoto, sono disponibili strumenti disponibili che è possibile utilizzare per passare la sessione.

### Avvia una connessione Desktop remoto

1. Tocca la connessione desktop remoto per avviare la sessione desktop remoto. 
2. Se viene richiesto di verificare il certificato per il desktop remoto, toccare **accetta**. Scegliere di accettare sempre scorrere l'interruttore **non Richiedi nuovamente per le connessioni a questo computer** su **ON**. 

### Barra di connessione

Consente di barra di connessione che accedere ai controlli di spostamento aggiuntivi. 

- **Controllo di panning**: il controllo pan Abilita schermo per essere ingrandita e spostate. Tieni presente che è disponibile tramite tocco diretto solo controllo pan.
   - Abilitare / disabilitare il controllo pan: toccare l'icona di panoramica nella barra di connessione per visualizzare il controllo di panoramica e zoom lo schermo. Toccare l'icona di panoramica nella barra di connessione nuovamente per nascondere il controllo e restituire lo schermo alla sua risoluzione originale.
   - Usare il controllo pan: tocco e attesa della panoramica di controllo e quindi trascinare nella direzione che si desidera spostare lo schermo.
   - Spostare il controllo pan: Usa il doppio tocco e contenere il controllo di zoom per spostare il controllo sullo schermo.
- **Nome della connessione**: il nome della connessione corrente viene visualizzato. Tocca il nome della connessione per visualizzare la barra di selezione della sessione.
- **Tastiera**: toccare l'icona della tastiera per visualizzare o nascondere la tastiera. Il controllo zoom viene visualizzato automaticamente quando viene visualizzata la tastiera.
- **Spostare la barra di connessione**: tocco e la barra di connessione, pressione prolungata e quindi trascina e rilascia in una nuova posizione nella parte superiore dello schermo.

### Selezione di sessione
Si possono avere più connessioni Apri ai PC diverse nello stesso momento. Toccare la barra di connessione per visualizzare la barra di selezione sessione sul lato sinistro dello schermo. La barra di selezione sessione consente di visualizzare le connessioni aperte e passare tra loro. 

- Spostarsi tra le App in una sessione di risorse remote aperte.

    Quando si è connessi a risorse remote, puoi passare tra le applicazioni aperte all'interno di tale sessione toccando il menu di espansione e scegliendo dall'elenco degli elementi disponibili.
- Avviare una nuova sessione

  È possibile avviare nuove applicazioni o sessioni desktop all'interno della connessione corrente: tocca **Avviare nuovo**e quindi scegli l'elenco degli elementi disponibili.

- Disconnessione una sessione

  Per disconnettere un tocco sessione X sul lato sinistro del riquadro della sessione.

### Barra dei comandi

La barra dei comandi sostituito l'utilità della barra partire dalla versione 8.0.1. Puoi passare tra le modalità mouse e tornare al Centro connessione della barra dei comandi.

## Usare movimenti di tocco e le modalità mouse in una sessione remota

Il client Usa movimenti di tocco standard. Puoi anche usare movimenti di tocco per replicare le azioni del mouse sul desktop remoto. La modalità mouse disponibili sono definite nella tabella seguente.

> [!NOTE]
> Interazione con Windows 8 o versione successiva i movimenti di tocco native sono supportati in modalità tocco diretto. Per ulteriori informazioni su Windows 8 vedere movimenti [Touch: scorrimento rapido, tocco e oltre](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

| Modalità mouse    | Operazione con il mouse      | Movimento                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Tocco diretto  | Pulsante sinistro           | tocco con 1 dita                                               |
| Tocco diretto  | Fare clic con il pulsante destro del mouse          | 1 tocco delle dita e pressione prolungata                                      |
| Puntatore del mouse | Pulsante sinistro           | tocco con 1 dita                                               |
| Puntatore del mouse | Fare clic con il tasto sinistro del mouse e trascinare  | tocca due volte 1 DITA e pressione prolungata, quindi trascinare.                    |
| Puntatore del mouse | Fare clic con il pulsante destro del mouse          | tocca il dito 2                                               |
| Puntatore del mouse | Fare clic con il tasto destro del mouse e trascinare | 2 doppio tocco DITA e pressione prolungata, quindi trascinare.                    |
| Puntatore del mouse | Rotellina del mouse          | 2 dito tocca e tieni premuto, quindi trascinare verso l'alto o verso il basso                |
| Puntatore del mouse | Zoom                 | Avvicinamento delle 2 dita per eseguire lo zoom avanti o diffondersi 2 dita per lo zoom indietro |

## Dispositivi di input supportati

Il [client di Desktop remoto iOS beta](https://aka.ms/rdiosbeta) supporta i mouse fisici Swiftpoint GT e ProPoint. Swiftpoint offre un [sconto esclusivo](https://www.swiftpoint.com/microsoft/) su GT per gli utenti di client beta iOS.

Il client iOS attualmente supporta solo Swiftpoint mouse. Fai riferimento alla pagina [What ' s new in client iOS](ios-whatsnew.md) e [App iOS nello Store](https://aka.ms/rdios) per notizie sul supporto per altri dispositivi in futuro.

## Usare una tastiera in una sessione remota

Puoi usare sullo schermo della tastiera o tastiera fisica nella sessione remota.

Per sullo schermo tastiere, Usa il pulsante a destra della barra sopra la tastiera per spostarsi tra la tastiera standard e altre.

Se Bluetooth è abilitata per il tuo dispositivo iOS, il client rileva automaticamente la tastiera Bluetooth.

Tieni presente che, a causa dei limiti del sistema operativo, tasti speciali, ad esempio Ctrl, opzione e funzione non funziona come previsto con una tastiera Bluetooth. Le seguenti chiavi di lavoro:

- Tasti alfanumerici
- Tasti di direzione
- Scheda: Il tasto di tabulazione, ma Maiusc + Tab non funziona
- Home / Pos1: Alt + freccia sinistra = Home
- Fine: Alt + freccia destra = fine
- PGSU: Alt + freccia = PGSU
- PGGIÙ: Alt + freccia giù = PGGIÙ
- Seleziona tutto: Comando + A = Ctrl + A (Seleziona tutto nella maggior parte dei programmi)
- Taglio: Comando + X = Ctrl + X (taglio nella maggior parte dei programmi)
- Copia: Comando + C = Ctrl + C (copia nella maggior parte dei programmi)
- Incolla: Comando + V = Ctrl + V (Incolla nella maggior parte dei programmi)
- Simboli: Tasti Alt + alfanumerico produrrà simboli diversi a seconda della lingua configurato

> [!TIP]
> Domande e i commenti sono sempre iniziale. Tuttavia, non è possibile inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, Vai al [forum di client Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Hai un suggerimento di funzionalità? Facci sapere nel [forum di voce utente client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

