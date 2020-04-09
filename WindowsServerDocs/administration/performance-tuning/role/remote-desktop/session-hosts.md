---
title: Ottimizzazione delle prestazioni Desktop remoto host della sessione
description: Linee guida per l'ottimizzazione delle prestazioni per Desktop remoto host sessione
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: hammadbu; vladmis; denisgun
author: phstee
ms.date: 10/22/2019
ms.openlocfilehash: 3227bfe3bf21343ca9b7e85a07f550b4684a2fb7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851714"
---
# <a name="performance-tuning-remote-desktop-session-hosts"></a>Ottimizzazione delle prestazioni Desktop remoto host della sessione


In questo argomento viene illustrato come selezionare host sessione Desktop remoto hardware (host sessione Desktop remoto), ottimizzare l'host e ottimizzare le applicazioni.

**Contenuto dell'argomento:**

-   [Selezione dell'hardware appropriato per le prestazioni](#selecting-the-proper-hardware-for-performance)

-   [Ottimizzazione delle applicazioni per host sessione Desktop remoto](#tuning-applications-for-remote-desktop-session-host)

-   [Parametri di ottimizzazione host sessione Desktop remoto](#remote-desktop-session-host-tuning-parameters)

## <a name="selecting-the-proper-hardware-for-performance"></a>Selezione dell'hardware appropriato per le prestazioni


Per la distribuzione di un server Host sessione Desktop remoto, la scelta dell'hardware è regolata dal set di applicazioni e dal modo in cui vengono utilizzate dagli utenti. I fattori chiave che influiscono sul numero di utenti e la loro esperienza sono CPU, memoria, disco e grafica. In questa sezione sono contenute linee guida aggiuntive specifiche per i server Host sessione Desktop remoto e sono correlate principalmente all'ambiente multiutente dei server Host sessione Desktop remoto.

### <a name="cpu-configuration"></a>Configurazione CPU

La configurazione della CPU è determinata dal punto di vista concettuale moltiplicando la CPU necessaria per supportare una sessione per il numero di sessioni supportate dal sistema, mantenendo al tempo stesso una zona buffer per gestire i picchi temporanei. Più processori logici possono contribuire a ridurre le situazioni anomale di congestione della CPU, che in genere sono causate da pochi thread iperattivi contenuti in un numero simile di processori logici.

Quindi, più processori logici in un sistema, minore è il margine del cuscino che deve essere incorporato nella stima di utilizzo della CPU, che comporta una percentuale maggiore di carico attivo per CPU. Un fattore importante da ricordare è che raddoppiare il numero di CPU non raddoppia la capacità della CPU.

### <a name="memory-configuration"></a>Configurazione della memoria

La configurazione della memoria dipende dalle applicazioni utilizzate dagli utenti; Tuttavia, la quantità di memoria necessaria può essere stimata usando la formula seguente: TotalMem = OSMem + SessionMem \* NS

OSMem è la quantità di memoria necessaria per l'esecuzione del sistema operativo (ad esempio immagini binarie di sistema, strutture di dati e così via), SessionMem è la quantità di processi di memoria in esecuzione in una sessione e NS è il numero di sessioni attive di destinazione. La quantità di memoria necessaria per una sessione è determinata principalmente dal set di riferimenti alla memoria privata per le applicazioni e i processi di sistema in esecuzione all'interno della sessione. Il codice condiviso o le pagine di dati hanno un effetto ridotto perché nel sistema è presente una sola copia.

Una osservazione interessante (presupponendo che il sistema di dischi che sta eseguendo il backup del file di paging non cambi) è che il numero di sessioni attive simultanee che il sistema prevede di supportare è maggiore di quello che l'allocazione di memoria per sessione deve essere. Se la quantità di memoria allocata per sessione non è aumentata, il numero di errori di pagina generati dalle sessioni attive aumenta con il numero di sessioni. Questi errori sovraccaricano infine il sottosistema di I/O. Aumentando la quantità di memoria allocata per sessione, la probabilità che si verificano errori di pagina diminuisce, consentendo di ridurre la frequenza complessiva degli errori di pagina.

### <a name="disk-configuration"></a>Configurazione del disco

L'archiviazione è uno degli aspetti più trascurati quando si configurano i server Host sessione Desktop remoto e può essere la limitazione più comune nei sistemi distribuiti nel campo.

L'attività del disco generata in un tipico server Host sessione Desktop remoto influiscono sulle aree seguenti:

-   File di sistema e file binari dell'applicazione

-   File di paging

-   Profili utente e dati utente

Idealmente, è consigliabile eseguire il backup di queste aree da dispositivi di archiviazione distinti. L'uso di configurazioni RAID con striping o altri tipi di archiviazione a prestazioni elevate migliora ulteriormente le prestazioni. Si consiglia vivamente di usare gli adattatori di archiviazione con memorizzazione nella cache di scrittura con supporto di batteria. I controller con memorizzazione nella cache di scrittura su disco offrono un supporto migliorato per le operazioni di scrittura sincrona. Poiché tutti gli utenti dispongono di un hive separato, le operazioni di scrittura sincrona sono molto più comuni in un server Host sessione Desktop remoto. Gli hive del registro di sistema vengono salvati periodicamente su disco mediante operazioni di scrittura sincrona. Per abilitare queste ottimizzazioni, dalla console Gestione disco aprire la finestra di dialogo **Proprietà** per il disco di destinazione e, nella scheda **criteri** , selezionare la casella di controllo **Abilita memorizzazione nella cache sul disco** e **Disattiva lo svuotamento del buffer di scrittura nella cache di Windows** nelle caselle di controllo del dispositivo.

### <a name="network-configuration"></a>Configurazione di rete

L'utilizzo della rete per un server Host sessione Desktop remoto include due categorie principali:

-   L'utilizzo del traffico di connessione host sessione Desktop remoto è determinato quasi esclusivamente dai modelli di disegno presentati dalle applicazioni in esecuzione nelle sessioni e dal traffico di I/O dei dispositivi reindirizzati.

    Ad esempio, le applicazioni che gestiscono l'elaborazione del testo e l'input di dati utilizzano la larghezza di banda di circa 10 a 100 kilobit al secondo, mentre la riproduzione grafica e video avanzata provoca un notevole aumento dell'utilizzo della larghezza di banda

-   Connessioni back-end come profili mobili, accesso alle applicazioni per condivisioni file, server di database, server di posta elettronica e server HTTP.

    Il volume e il profilo del traffico di rete sono specifici per ogni distribuzione.

## <a name="tuning-applications-for-remote-desktop-session-host"></a>Ottimizzazione delle applicazioni per host sessione Desktop remoto


La maggior parte dell'utilizzo della CPU in un server Host sessione Desktop remoto è determinata dalle app. Le applicazioni desktop sono in genere ottimizzate per la velocità di risposta, con l'obiettivo di ridurre al minimo il tempo impiegato da un'applicazione per rispondere a una richiesta dell'utente. Tuttavia, in un ambiente server, è ugualmente importante ridurre al minimo la quantità totale di utilizzo della CPU necessaria per completare un'azione, in modo da evitare effetti negativi sulle altre sessioni.

Quando si configurano le app da usare in un server Host sessione Desktop remoto, considerare i suggerimenti seguenti:

-   Ridurre al minimo l'elaborazione del ciclo di inattività in background

    Esempi tipici sono la disabilitazione della grammatica e del controllo ortografico in background, l'indicizzazione dei dati per la ricerca e i salvataggi in background.

-   Ridurre al minimo la frequenza con cui un'app esegue un controllo o un aggiornamento dello stato.

    La disabilitazione di tali comportamenti o l'aumento dell'intervallo tra le iterazioni di polling e la generazione di timer comporta un notevole vantaggio nell'utilizzo della CPU, perché l'effetto di tali attività viene rapidamente amplificato per molte sessioni attive. Esempi tipici sono le icone di stato della connessione e gli aggiornamenti delle informazioni sulla barra di stato.

-   Ridurre la contesa delle risorse tra le app riducendo la frequenza di sincronizzazione.

    Esempi di tali risorse includono chiavi del registro di sistema e file di configurazione. Esempi di componenti e funzionalità dell'applicazione sono gli indicatori di stato (ad esempio le notifiche della Shell), l'indicizzazione in background o il monitoraggio delle modifiche e la sincronizzazione offline.

-   Disabilitare i processi non necessari registrati per iniziare con l'accesso dell'utente o un avvio della sessione.

    Questi processi possono contribuire significativamente al costo di utilizzo della CPU quando si crea una nuova sessione utente, che in genere è un processo a elevato utilizzo di CPU e che può essere molto costosa negli scenari di mattina. Usare MsConfig. exe o MsInfo32. exe per ottenere un elenco di processi avviati all'accesso dell'utente. Per informazioni più dettagliate, è possibile usare [Autoruns per Windows](https://technet.microsoft.com/sysinternals/bb963902.aspx).

Per l'utilizzo della memoria, è necessario considerare quanto segue:

-   Verificare che le DLL caricate da un'app non vengano rilocate.

    -   È possibile verificare le dll rilocate selezionando elabora visualizzazione DLL, come illustrato nella figura seguente, usando [Esplora processi](https://technet.microsoft.com/sysinternals/bb896653.aspx).

    -   Qui è possibile notare che la funzionalità y. dll è stata rilocata perché x. dll ha già occupato l'indirizzo di base predefinito e ASLR non è stato abilitato

        ![dll rilocate](../../media/perftune-guide-relocated-dlls.png)

        Se le dll vengono rilocate, è Impossibile condividere il proprio codice tra le sessioni, che aumenta significativamente il footprint di una sessione. Questo è uno dei problemi di prestazioni più comuni relativi alla memoria in un server Host sessione Desktop remoto.

-   Per le applicazioni Common Language Runtime (CLR), usare il generatore di immagini native (Ngen. exe) per aumentare la condivisione della pagina e ridurre il sovraccarico della CPU.

    Quando possibile, applicare tecniche simili ad altri motori di esecuzione simili.

## <a name="remote-desktop-session-host-tuning-parameters"></a>Parametri di ottimizzazione host sessione Desktop remoto


### <a name="page-file"></a>File di paging

Dimensioni del file di paging insufficienti possono causare errori di allocazione della memoria nelle app o nei componenti di sistema. Per monitorare la quantità di memoria virtuale di cui è stato eseguito il commit nel sistema, è possibile utilizzare il contatore delle prestazioni byte da memoria a commit.

### <a name="antivirus"></a>Antivirus

L'installazione del software antivirus in un server Host sessione Desktop remoto influisce significativamente sulle prestazioni complessive del sistema, soprattutto sull'utilizzo della CPU. Si consiglia vivamente di escludere dall'elenco Active Monitoring tutte le cartelle che contengono i file temporanei, in particolare quelle che i servizi e altri componenti di sistema generano.

### <a name="task-scheduler"></a>Utilità di pianificazione

Utilità di pianificazione consente di esaminare l'elenco delle attività pianificate per eventi diversi. Per un server Host sessione Desktop remoto, è utile concentrarsi specificamente sulle attività configurate per l'esecuzione in modalità di inattività, all'accesso dell'utente o alla connessione e disconnessione della sessione. A causa delle specifiche della distribuzione, molte di queste attività potrebbero non essere necessarie.

### <a name="desktop-notification-icons"></a>Icone di notifica sul desktop

Le icone di notifica sul desktop possono avere meccanismi di aggiornamento piuttosto costosi. È necessario disabilitare le notifiche rimuovendo il componente che li registra dall'elenco di avvio o modificando la configurazione nelle app e nei componenti di sistema per disabilitarli. È possibile usare le **Icone di personalizzazione delle notifiche** per esaminare l'elenco di notifiche disponibili sul server.

### <a name="remote-desktop-protocol-data-compression"></a>Compressione dei dati Remote Desktop Protocol

Remote Desktop Protocol la compressione può essere configurata utilizzando Criteri di gruppo in **Configurazione Computer** &gt; **modelli amministrativi** &gt; **componenti** di Windows **Remote Desktop Session Host** **&gt; Servizi Desktop remoto &gt; host sessione Desktop remoto** &gt; **ambiente sessione remota** &gt; **configurare la compressione per i dati RemoteFX**. Sono possibili tre valori:

-   **Ottimizzato per l'utilizzo di meno memoria** Utilizza la quantità minima di memoria per sessione, ma presenta il rapporto di compressione più basso e pertanto il consumo di larghezza di banda più elevato.

-   **Bilancia la memoria e la larghezza di banda della rete** Riduzione dell'utilizzo della larghezza di banda, aumentando il consumo di memoria (circa 200 KB per sessione).

-   **Ottimizzato per l'utilizzo della larghezza di banda di rete inferiore** Consente inoltre di ridurre l'utilizzo della larghezza di banda di rete a un costo di circa 2 MB per sessione. Se si vuole usare questa impostazione, è necessario valutare il numero massimo di sessioni ed eseguire il test a tale livello con questa impostazione prima di collocare il server in produzione.

È anche possibile scegliere di non usare un algoritmo di compressione Remote Desktop Protocol, quindi è consigliabile usarlo con un dispositivo hardware progettato per ottimizzare il traffico di rete. Anche se si sceglie di non usare un algoritmo di compressione, alcuni dati grafici verranno compressi.

### <a name="device-redirection"></a>Reindirizzamento del dispositivo

Il reindirizzamento del dispositivo può essere configurato usando Criteri di gruppo in **Configurazione Computer** &gt; **modelli amministrativi** &gt; **componenti di Windows** &gt; **Servizi Desktop remoto &gt; host sessione Desktop remoto** **&gt; il** **Reindirizzamento di dispositivi e risorse** oppure usando la casella proprietà **raccolta di sessioni** in Server Manager.

In genere, il reindirizzamento dei dispositivi aumenta la quantità di connessioni del server Host sessione Desktop remoto della larghezza di banda usata perché i dati vengono scambiati tra i dispositivi nei computer client e i processi in esecuzione nella sessione del server. L'entità dell'aumento è una funzione della frequenza delle operazioni eseguite dalle applicazioni in esecuzione nel server sui dispositivi reindirizzati.

Il reindirizzamento della stampante e il reindirizzamento dei dispositivi Plug and Play aumentano anche l'utilizzo della CPU all'accesso. È possibile reindirizzare le stampanti in due modi:

-   Corrispondenza del reindirizzamento basato su driver della stampante quando è necessario installare un driver per la stampante nel server. Questo metodo è stato utilizzato nelle versioni precedenti di Windows Server.

-   Introdotta in Windows Server 2008, il reindirizzamento dei driver della stampante Easy Print utilizza un driver della stampante comune per tutte le stampanti.

È consigliabile usare il metodo Easy Print perché causa un minor utilizzo della CPU per l'installazione della stampante al momento della connessione. Il metodo driver corrispondente causa un maggiore utilizzo della CPU, perché richiede il caricamento di driver diversi da parte del servizio spooler. Per l'utilizzo della larghezza di banda, la stampa semplice causa un aumento dell'utilizzo della larghezza di banda di rete, ma non abbastanza significativo per compensare i vantaggi di prestazioni, gestibilità e affidabilità.

Il reindirizzamento audio causa un flusso costante di traffico di rete. Il reindirizzamento audio consente inoltre agli utenti di eseguire app multimediali che in genere hanno un utilizzo elevato della CPU.

### <a name="client-experience-settings"></a>Impostazioni esperienza client

Per impostazione predefinita, Connessione Desktop remoto (RDC) sceglie automaticamente l'impostazione dell'esperienza corretta in base all'idoneità della connessione di rete tra il server e i computer client. È consigliabile che la configurazione RDC rimanga in modo da **rilevare automaticamente la qualità della connessione**.

Per gli utenti avanzati, RDC fornisce il controllo su una gamma di impostazioni che influenzano le prestazioni della larghezza di banda di rete per la connessione Servizi Desktop remoto. È possibile accedere alle impostazioni seguenti utilizzando la scheda **esperienza** in connessione Desktop remoto o come impostazioni nel file RDP.

Quando ci si connette a qualsiasi computer, vengono applicate le impostazioni seguenti:

-   **Disabilita sfondo** (Disabilita sfondo: i: 0) non Mostra lo sfondo del desktop sulle connessioni reindirizzate. Questa impostazione può ridurre significativamente l'utilizzo della larghezza di banda se lo sfondo del desktop è costituito da un'immagine o da altri contenuti con costi significativi per il disegno.

-   **Cache bitmap** (bitmapcachepersistenable: i: 1) quando questa impostazione è abilitata, viene creata una cache sul lato client delle bitmap di cui viene eseguito il rendering nella sessione. Fornisce un miglioramento significativo sull'utilizzo della larghezza di banda ed è sempre necessario abilitarlo (a meno che non siano presenti altre considerazioni sulla sicurezza).

-   **Mostra contenuto delle finestre durante il trascinamento** (disabilitazione del trascinamento della finestra completa: i: 1) quando questa impostazione è disabilitata, riduce la larghezza di banda visualizzando solo la cornice della finestra anziché tutto il contenuto quando la finestra viene trascinata.

-   **Animazione di menu e finestre** (Disattiva menu Animas: i: 1 e Disabilita impostazione cursore: i: 1): quando queste impostazioni sono disabilitate, riduce la larghezza di banda disabilitando l'animazione nei menu (ad esempio dissolvenza) e i cursori.

-   **Smussatura dei caratteri** (Consenti smussatura dei caratteri: i: 0) controlla il supporto per il rendering del tipo di carattere ClearType. Quando ci si connette a computer che eseguono Windows 8 o Windows Server 2012 e versioni successive, l'abilitazione o la disabilitazione di questa impostazione non ha un impatto significativo sull'utilizzo della larghezza di banda. Tuttavia, per i computer che eseguono versioni precedenti a Windows 7 e Windows 2008 R2, l'abilitazione di questa impostazione influiscono significativamente sull'utilizzo della larghezza di banda di rete.

Le impostazioni seguenti si applicano solo quando ci si connette a computer che eseguono Windows 7 e versioni precedenti del sistema operativo:

-   **Composizione del desktop** Questa impostazione è supportata solo per una sessione remota in un computer che esegue Windows 7 o Windows Server 2008 R2.

-   **Stili di visualizzazione** (Disabilita i temi: i: 1) quando questa impostazione è disabilitata, riduce la larghezza di banda semplificando i disegni di tema che usano il tema classico.

Utilizzando la scheda **esperienza** in connessione Desktop remoto, è possibile scegliere la velocità di connessione per influenzare le prestazioni della larghezza di banda di rete. Di seguito sono elencate le opzioni disponibili per configurare la velocità di connessione:

-   **Rileva automaticamente la qualità della connessione** (tipo di connessione: i: 7) quando questa impostazione è abilitata, connessione Desktop remoto sceglie automaticamente le impostazioni che comporteranno un'esperienza utente ottimale basata sulla qualità della connessione. Questa configurazione è consigliata quando ci si connette a computer che eseguono Windows 8 o Windows Server 2012 e versioni successive.

-   **Modem (56 Kbps)** (tipo di connessione: i: 1) questa impostazione Abilita la memorizzazione nella cache persistente delle bitmap.

-   **Banda larga a bassa velocità (256 Kbps-2 Mbps)** (tipo di connessione: i: 2) questa impostazione Abilita la memorizzazione nella cache persistente e gli stili di visualizzazione.

-   **Cellulare/satellite (2Mbps-16 Mbps con latenza elevata)** (tipo di connessione: i: 3) questa impostazione consente la composizione del desktop, la memorizzazione nella cache permanente della bitmap, gli stili di visualizzazione e lo sfondo del desktop.

-   **Banda larga ad alta velocità (2 Mbps-10 Mbps)** (tipo di connessione: i: 4) questa impostazione consente la composizione del desktop, Mostra il contenuto delle finestre durante il trascinamento, l'animazione di menu e finestre, la memorizzazione nella cache persistente della bitmap, gli stili di visualizzazione e lo sfondo del desktop.

-   **WAN (10 Mbps o superiore con latenza elevata)** (tipo di connessione: i: 5) questa impostazione consente la composizione del desktop, Mostra il contenuto delle finestre durante il trascinamento, l'animazione di menu e finestre, la memorizzazione nella cache permanente della bitmap, gli stili visivi e lo sfondo del desktop.

-   **LAN (10 Mbps o superiore)** (tipo di connessione: i: 6) questa impostazione consente la composizione del desktop, Mostra il contenuto delle finestre durante il trascinamento, l'animazione di menu e finestre, la memorizzazione nella cache permanente della bitmap, i temi e lo sfondo del desktop.

### <a name="desktop-size"></a>Dimensioni del desktop

Le dimensioni del desktop per le sessioni remote possono essere controllate tramite la scheda Visualizza in Connessione Desktop remoto o tramite il file di configurazione RDP (desktopwidth: i: 1152 e desktopheight: i: 864). Maggiori sono le dimensioni del desktop, maggiore è il consumo di memoria e larghezza di banda associato a tale sessione. Le dimensioni massime del desktop correnti sono 4096 x 2048.
