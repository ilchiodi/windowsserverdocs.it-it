---
title: Per iniziare con Desktop remoto su Mac
description: Scopri come configurare il client Desktop remoto per Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 10/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: e8c5da1960d0e3129b5520e65c2d5ecf45eef778
ms.sourcegitcommit: 6dc14d4315793132488494b5543ee83e3f562f09
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2018
ms.locfileid: "4555718"
---
# Per iniziare con Desktop remoto su Mac

>Si applica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

È possibile utilizzare il client Desktop remoto per Mac per lavorare con le app Windows, le risorse e i desktop dal tuo computer Mac. Usa le informazioni seguenti per iniziare a - e consulta le [domande frequenti su](remote-desktop-client-faq.md) se hai domande.

>[!Note]
> - I nuovi rilasci per il client macOS saperlo? Consulta [Novità per Desktop remoto su Mac?](mac-whatsnew.md)
> - Il client Mac viene eseguita nel computer che eseguono macOS 10.10 e versioni successive.
> - Le informazioni contenute in questo articolo si applicano principalmente per la versione completa del client Mac - la versione disponibile in AppStore il Mac. Testare le nuove funzionalità scaricando la nostra app anteprima qui: [note sulla versione client beta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## Ottenere il client Desktop remoto
Segui questi passaggi per iniziare con Desktop remoto su Mac:

1. Scaricare il client Desktop remoto Microsoft da [App Mac nello Store](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configurare il PC per accettare le connessioni remote](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). (Se si ignora questo passaggio, è possibile connettersi al PC.)
3. Aggiungere una connessione Desktop remoto o una risorsa remota. Usare una connessione di connettersi direttamente a un PC Windows e una risorsa remota di usare un programma RemoteApp, basata su sessione desktop o un desktop virtuale pubblicata in locale con connessione RemoteApp e Desktop. Questa funzionalità è in genere disponibile negli ambienti aziendali.

## Cosa accade al client beta Mac?
Abbiamo cessato sta valutando nuove funzionalità nel nostro canale anteprima su HockeyApp. Vuoi Scoprilo? Vai a [Microsoft Desktop remoto per Mac](https://go.microsoft.com/fwlink/?LinkID=619698) e fai clic su **Scarica**. Non devi creare un account o un'insegna in HockeyApp per scaricare il client beta.

Se hai già il client, puoi controllare la disponibilità di aggiornamenti in modo da garantire la versione più recente. Nel client beta, fai clic su **Versione Beta di Desktop remoto Microsoft** nella parte superiore e quindi fai clic su **Verifica disponibilità aggiornamenti**. 

## Aggiungere una connessione Desktop remoto
Per creare una connessione desktop remoto:

1. Nel centro di connessione, fai clic su **+**, quindi fai clic su **Desktop**.
2. Immetti le informazioni seguenti:
   - **Nome del PC** - il nome del computer.
      - Può trattarsi di un nome computer Windows (trovata nelle impostazioni di **sistema** ), un nome di dominio o un indirizzo IP.
      - Puoi anche aggiungere le informazioni sulla porta alla fine di questo nome, come *MyDesktop:3389*.
   - **Account utente** - Aggiungi l'account utente che usi per accedere al PC remoto.
      - Per Active Directory (AD) aggiunti a un computer o account locale, usa uno di questi formati: *nome_utente*, *DOMINIO\nome_utente.* o *user_name@domain.com*.
      - Per Azure Active Directory (AAD) aggiunti a un computer, usa uno di questi formati: *AzureAD\user_name* o *Azuread\prova@contoso.onmicrosoft.com user_name@domain.com*.
      - Puoi anche scegliere se Richiedi una password.
      - Durante la gestione di più account utente con lo stesso nome utente, imposta un nome descrittivo per distinguere gli account.
      - Gestire gli account utente salvate nelle preferenze dell'app. 

3. Puoi anche impostare le impostazioni facoltative per la connessione:
   - Impostare un nome descrittivo 
   - Aggiungere un Gateway
   - Impostare l'output audio
   - Pulsanti del mouse di scambio
   - Abilitare la modalità di amministratore
   - Reindirizzamento cartelle locali in una sessione remota
   - Stampanti locali in avanti
   - Le smart card in avanti
4. Fai clic su **Salva**.

Per avviare la connessione, semplicemente doppio clic. Lo stesso vale per le risorse remote.

### Esportare e importare le connessioni
Puoi esportare una definizione di connessione desktop remoto e usarli in un altro dispositivo. Funzionamento dei desktop remoti vengono salvati in separato. File RDP.

1. In Centro di connessione, fai doppio clic sul desktop remoto.
2. Fai clic su **Esporta**.
3. Passa alla posizione in cui si desidera salvare il desktop remoto. File RDP.
4. Scegli **OK**.

Utilizzare la procedura seguente per importare un desktop remoto. File RDP.

1. Nella barra del menu, fai clic su **File > Import**.
2. Individua il. File RDP.
3. Fai clic su **Apri**.

## Aggiungere una risorsa remota
Risorse remote sono programmi RemoteApp, basata su sessione desktop e desktop virtuali pubblicato con connessione RemoteApp e Desktop.

- L'URL visualizzato il collegamento al server di accesso Web desktop remoto che consente di accedere a connessione RemoteApp e Desktop.
- Sono elencate le configurato RemoteApp e le connessioni Desktop.

Per aggiungere una risorsa remota:

1. Nel clic di connessione Center **+**, quindi fai clic su **Aggiungi risorse Remote**. 
2. Immettere le informazioni per la risorsa remota:
   - **URL del feed** - l'URL del server Accesso Web desktop remoto. È inoltre possibile immettere l'account di posta elettronica aziendali in questo campo – in questo modo il client per cercare il Server di accesso Web desktop remoto associato l'indirizzo di posta elettronica.
   - **Nome utente** - il nome utente da utilizzare per il server di accesso Web desktop remoto a che si connette.
   - **Password** : password da usare per il server di accesso Web desktop remoto a che si connette.
3. Fai clic su **Salva**.


Le risorse remote verranno visualizzate nel centro di connessione.


## Connettersi a un Gateway Desktop remoto per accedere alle risorse interne

Un Gateway Desktop remoto (Gateway Desktop remoto) consente di connettersi a un computer remoto in una rete aziendale da qualsiasi posizione su Internet. Puoi creare e gestire il gateway nelle preferenze dell'app o durante l'installazione di una nuova connessione desktop.

Per configurare un nuovo gateway nelle preferenze:

1. Nel centro di connessione, fai clic su **Preferenze > gateway**. 
2. Fai clic sul **+** pulsante nella parte inferiore della tabella immettere le informazioni seguenti:
  - **Nome del server** : il nome del computer che vuoi usare come gateway. Può trattarsi di un nome computer Windows, un nome di dominio Internet o un indirizzo IP. Puoi anche aggiungere le informazioni sulla porta per il nome del server (ad esempio: **RDGateway:443** o **10.0.0.1:443**).
  - **Nome utente** - il nome utente e password da usare per il gateway Desktop remoto a che si connette. Puoi anche selezionare **usare connessione credenziali** per usare lo stesso nome utente e password come quelli usati per la connessione desktop remota.


## Gestire gli account utente

Quando ti Connetti a un desktop o remoto risorse, è possibile salvare gli account utente di selezionare da nuovamente. Puoi gestire gli account utente con il client Desktop remoto.

Per creare un nuovo account utente:

1. Nel centro di connessione, fai clic su **Impostazioni** > **gli account**.
2. Fai clic su **Aggiungi Account utente**.
3. Immetti le informazioni seguenti:
   - **Nome utente** - il nome dell'utente di salvare per l'uso con una connessione remota. Puoi immettere il nome utente in una qualsiasi delle seguenti formati: nome_utente, DOMINIO\nome_utente., o user_name@domain.com.
   - **Password** : la password per l'utente specificato. Ogni account utente che si desidera salvare per usare per le connessioni remote deve avere una password è associata.
   - **Nome descrittivo** - se si utilizza lo stesso account utente con password differenti, imposta un nome descrittivo per distinguere gli account utente.
4. Tocca **salvare**e quindi toccare **le impostazioni**.

## Personalizzare la risoluzione dello schermo
È possibile specificare la risoluzione dello schermo per la sessione desktop remoto.

1. Nel centro di connessione, fai clic su **Preferenze**.
2. Fai clic su **risoluzione**. 
3. Fai clic su **+**.
4. Immetti una risoluzione altezza e larghezza e quindi fai clic su **OK.**

Per eliminare la risoluzione, selezionalo e quindi fai clic su **-**.

**Visualizza hanno spazi separati** Se si esegue Mac OS X 10.9 e disabilitato **schermi hanno spazi separati** in Maverick (**Preferenze di sistema >: controllo della missione**), è necessario configurare questa impostazione nel client desktop remoto con l'opzione stesso.

### Reindirizzamento delle unità per le risorse remote
Il reindirizzamento delle unità è supportato per le risorse remote, in modo che sia possibile salvare file creati con un'applicazione remota in locale per il Mac. Le cartelle reindirizzate sono sempre la home directory visualizzata come un'unità di rete nella sessione remota.

> [!NOTE]
> Per poter usare questa funzionalità, l'amministratore deve impostare le impostazioni appropriate nel server.


## Usare una tastiera in una sessione remota

Layout di tastiera Mac diversa dal layout di tastiera di Windows. 

- Il tasto di comando sulla tastiera Mac è uguale al tasto Windows.
- Per eseguire azioni che utilizzano il pulsante di comando su Mac, devi usare il pulsante di controllo di Windows (ad esempio: Copia = Ctrl + C).
- I tasti funzione possono essere attivati nella sessione, inoltre premendo il tasto FN (ad esempio: MAIUSC+F1 FN).
- Il tasto Alt a destra della barra spaziatrice sulla tastiera Mac è uguale al tasto Alt Alt Gr/destra in Windows.

Per impostazione predefinita, la sessione remota userà le stesse impostazioni internazionali della tastiera come il sistema operativo è in esecuzione del client. (Se Mac è in esecuzione un'en-us del sistema operativo, che verrà usato per anche le sessioni remote. Se le impostazioni locali della tastiera del sistema operativo non viene usata, controlla la tastiera impostazione nel PC remoto e modifica l'impostazione manualmente. Vedi le [Domande frequenti sul Client Desktop remoto](remote-desktop-client-faq.md) per ulteriori informazioni sulle tastiere e impostazioni locali.


## Supporto per Desktop remoto gateway collegabile autenticazione e autorizzazione

Windows Server 2012 R2 ha introdotto il supporto per un nuovo metodo di autenticazione, Gateway Desktop remoto collegabile autenticazione e autorizzazione, che offre una maggiore flessibilità per routine di autenticazione personalizzato. Puoi ora questo modello di autenticazione con il client Mac. 

> [!IMPORTANT]
> Modelli di autenticazione e autorizzazione personalizzati prima di Windows 8.1 non sono supportati, anche se vengono illustrati nell'articolo sopra essi.

Per ulteriori informazioni su questa funzionalità, consulta [http://aka.ms/paa-sample](http://aka.ms/paa-sample).


> [!TIP]
> Domande e i commenti sono sempre iniziale. Tuttavia, non è possibile inviare una richiesta di risoluzione dei problemi utilizzando la funzionalità di commento alla fine di questo articolo. Al contrario, Vai al [forum sui client Desktop remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e avviare un nuovo thread. Hai un elenco dei suggerimenti funzionalità? Facci sapere nel [forum vocali utente client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

