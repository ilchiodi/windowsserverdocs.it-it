---
title: Host sessione Desktop remoto l'ottimizzazione delle prestazioni
description: Ottimizzazione delle prestazioni le linee guida per host sessione Desktop remoto
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e45d1abb545ad46e654c811a0347c589bd12adf0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863242"
---
# <a name="performance-tuning-remote-desktop-session-hosts"></a>Host sessione Desktop remoto l'ottimizzazione delle prestazioni


In questo argomento viene illustrato come selezionare l'hardware Host sessione Desktop remoto (Host sessione Desktop remoto), ottimizzare l'host e l'ottimizzazione delle applicazioni.

**In questo argomento:**

-   [Selezionare l'hardware corretto per le prestazioni](#hw)

-   [Ottimizzazione delle applicazioni per Host sessione Desktop remoto](#apps)

-   [Parametri di regolazione Host sessione Desktop remoto](#host)

## <a href="" id="hw"></a>Selezionare l'hardware corretto per le prestazioni


Per una distribuzione di server Host sessione Desktop remoto, la scelta dell'hardware è disciplinata dal set di applicazioni e come utenti le usano. I fattori chiave che influisce sul numero di utenti e la loro esperienza sono CPU, memoria, disco e grafica. In questa sezione contiene le ulteriori linee guida specifiche per i server Host sessione Desktop remoto e viene per lo più correlata all'ambiente con più utenti del server Host sessione Desktop remoto.

### <a name="cpu-configuration"></a>Configurazione CPU

Configurazione della CPU è concettualmente determinata moltiplicando la CPU necessaria per supportare una sessione per il numero di sessioni che il sistema deve supportare, pur mantenendo una zona buffer per gestire picchi temporanei. Più processori logici consente di ridurre le situazioni anomale CPU congestione, che sono in genere causate da alcuni thread iperattivo contenuti in un numero simile di processori logici.

Di conseguenza, i processori logici in un sistema, minore il margine di cuscinetto che deve essere compilato per la stima dell'utilizzo della CPU, che comporta più grande percentuale di carico attive per ogni CPU. Un fattore importante da ricordare è che raddoppiando il numero di CPU non non raddoppiare la capacità della CPU.

### <a name="memory-configuration"></a>Configurazione della memoria

Configurazione della memoria è dipendente da applicazioni che gli utenti utilizzino; Tuttavia, la quantità di memoria necessaria può essere stimata utilizzando la formula seguente: TotalMem = OSMem + SessionMem \* NS

OSMem è quanta memoria che del sistema operativo necessaria per l'esecuzione (ad esempio le immagini binarie del sistema, le strutture di dati e così via), SessionMem è quanto richiedono i processi di memoria in esecuzione in una sessione e NS è il numero di sessioni attive. La quantità di memoria necessaria per una sessione è principalmente determinata dal riferimento di memoria privata impostati per le applicazioni e i processi di sistema che sono in esecuzione all'interno della sessione. Le pagine condivise di codice o dati hanno un impatto minimo perché è presente nel sistema di sola copia.

Un'osservazione interessa (presupponendo che il sistema che esegue il backup del file di paging del disco rimane invariato) è che maggiore è il numero di sessioni attive simultanee del sistema prevede di supportare, maggiori saranno le dimensioni dell'allocazione della memoria per ogni sessione deve essere. Se la quantità di memoria allocata per ogni sessione non viene aumentata, il numero di errori di pagina che le sessioni attive genera aumenta con il numero di sessioni. Questi errori alla fine sovraccaricare il sottosistema dei / o. Se si aumenta la quantità di memoria allocata per ogni sessione, riduce la probabilità di dover sostenere errori di pagina, che consente di ridurre il tasso complessivo di errori di pagina.

### <a name="disk-configuration"></a>Configurazione del disco

L'archiviazione è uno degli aspetti più sottovalutati quando si configurano i server Host sessione Desktop remoto e può essere la limitazione più comune in sistemi distribuiti nel campo.

L'attività del disco che viene generato in un server Host sessione Desktop remoto tipico influisce sulle aree seguenti:

-   File di sistema e file binari dell'applicazione

-   File di paging

-   Profili utente mobili e dati utente

In teoria, queste aree devono eseguire il backup per i dispositivi di archiviazione distinti. Usando le configurazioni RAID con striping o altri tipi di spazio di archiviazione ad alte prestazioni migliora le prestazioni. È consigliabile usare gli adattatori di archiviazione con la cache in scrittura a batteria. Controller con la memorizzazione nella cache di scrittura disco offrire supporto migliorato per le operazioni di scrittura sincrona. Poiché tutti gli utenti hanno un hive separato, operazioni di scrittura sincrona sono notevolmente più comune in un server Host sessione Desktop remoto. Gli hive del Registro di sistema periodicamente vengono salvati su disco mediante operazioni di scrittura sincrona. Per abilitare queste ottimizzazioni, dalla console di Gestione disco, aprire il **delle proprietà** per il disco di destinazione e, nella finestra di dialogo il **criteri** scheda, seleziona il **Abilita memorizzazione nella cache di scrittura il disco** e **disattivare lo svuotamento buffer di cache di scrittura di Windows** sulle caselle di controllo del dispositivo.

### <a name="network-configuration"></a>Configurazione di rete

Uso della rete per un server Host sessione Desktop remoto include due categorie principali:

-   Utilizzo del traffico connessione Host sessione Desktop remoto dipende quasi esclusivamente per i modelli di disegni che si verificano dalle applicazioni in esecuzione all'interno di sessioni e il traffico dei / o reindirizzato dispositivi.

    Ad esempio, le applicazioni che gestiscono l'elaborazione del testo e i dati di input utilizzano larghezza di banda di 10 e 100 kilobit per secondo, mentre la riproduzione di video e migliorare la grafica causano aumenti significativi nell'utilizzo della larghezza di banda.

-   Connessioni back-end, ad esempio i profili, applicazione l'accesso a condivisioni file, i server di database, server di posta elettronica e i server HTTP.

    Il volume e il profilo di traffico di rete è specifico per ogni distribuzione.

## <a href="" id="apps"></a>Ottimizzazione delle applicazioni per Host sessione Desktop remoto


La maggior parte dell'utilizzo della CPU in un server Host sessione Desktop remoto è basato sulle app. Le app desktop sono in genere ottimizzate verso la velocità di risposta con l'obiettivo di ridurre al minimo il tempo impiegato da un'applicazione di rispondere a una richiesta dell'utente. Tuttavia in un ambiente server, è ugualmente importante per ridurre al minimo la quantità totale di utilizzo della CPU necessario per completare un'azione per evitare di influire negativamente sulle altre sessioni.

Quando si configurano le app che devono essere utilizzati in un server Host sessione Desktop remoto, prendere in considerazione i suggerimenti seguenti:

-   Ridurre al minimo l'elaborazione di cicli inattivi in background

    Esempi tipici sono la disabilitazione di controllo in background grammaticale e ortografico, l'indicizzazione per la ricerca dei dati e lo salva in background.

-   Ridurre al minimo la frequenza con cui un'app esegue un aggiornamento o controllo dello stato di.

    Disabilitare tali comportamenti o aumentando l'intervallo tra iterazioni di polling e timer di attivazione in modo significativo vantaggio dell'utilizzo della CPU perché l'effetto di queste attività è rapidamente amplificata per numero di sessioni attive. Esempi tipici sono icone di stato di connessione e gli aggiornamenti di stato della barra informazioni.

-   Ridurre al minimo la contesa di risorse tra le app, riducendo la frequenza di sincronizzazione.

    Esempi di tali risorse includono le chiavi del Registro di sistema e file di configurazione. Esempi di funzionalità e i componenti dell'applicazione sono l'indicatore di stato (ad esempio le notifiche di shell), l'indicizzazione in background o il monitoraggio delle modifiche e la sincronizzazione offline.

-   Disabilitare i processi non necessari che sono registrati per iniziare con accesso dell'utente o un avvio della sessione.

    Questi processi possono contribuire in modo significativo al costo di utilizzo della CPU quando si crea una nuova sessione utente, in genere è un processo intensivo della CPU, e può essere molto costoso in scenari di mattina. Utilizzare MsConfig.exe o MsInfo32.exe per ottenere un elenco dei processi vengono avviati all'accesso dell'utente. Per altre informazioni, è possibile usare [Autoruns per Windows](https://technet.microsoft.com/sysinternals/bb963902.aspx).

Per il consumo di memoria, è necessario considerare quanto segue:

-   Verificare che non vengono riallocate DLL caricate da un'app.

    -   Rilocate DLL può essere verificata selezionando Visualizza processo DLL, come illustrato nella figura seguente, usando [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653.aspx).

    -   Qui possiamo vedere che y è stato riposizionato perché x.dll già occupato relativo indirizzo di base predefinita e non è stato abilitato ASLR

        ![DLL rilocate](../../media/perftune-guide-relocated-dlls.png)

        Se le DLL vengono rilocate, non è possibile condividere codice tra le sessioni, aumentando notevolmente il footprint di una sessione. Questo è uno dei più comuni problemi di prestazioni correlati alla memoria in un server Host sessione Desktop remoto.

-   Per applicazioni di common language runtime (CLR), usare il generatore di immagini Native (Ngen.exe) per aumentare la condivisione delle pagine e ridurre l'overhead della CPU.

    Quando possibile, applica tecniche simili ad altri motori di esecuzione analoghi.

## <a href="" id="host"></a>Parametri di regolazione Host sessione Desktop remoto


### <a name="page-file"></a>File di paging

Insufficienti del file di paging può causare i errori di allocazione di memoria nelle App o componenti del sistema. È possibile utilizzare il contatore delle prestazioni memoria-a byte vincolati a monitorare la quantità memoria virtuale riservata nel sistema.

### <a name="antivirus"></a>Antivirus

Installazione del software antivirus in un server Host sessione Desktop remoto notevolmente influisce sulle prestazioni generali del sistema, in particolare l'utilizzo della CPU. È consigliabile che nell'elenco di monitoraggio attivo è escludere tutte le cartelle che contengono i file temporanei, in particolare quelli che servizi e altri componenti di sistema generano.

### <a name="task-scheduler"></a>Utilità di pianificazione

Utilità di pianificazione consente di esaminare l'elenco di attività pianificate per diversi eventi. Per un server Host sessione Desktop remoto, è utile per lo stato attivo in modo specifico per le attività che sono configurati per eseguire su inattivo, all'accesso dell'utente o sessione connettersi e disconnettersi. A causa di informazioni specifiche della distribuzione, molte di queste attività potrebbe non essere necessari.

### <a name="desktop-notification-icons"></a>Icone di notifica sul desktop

Icone di notifica sul desktop possono avere meccanismi di aggiornamento piuttosto dispendiosa. È consigliabile disabilitare le notifiche tramite la rimozione del componente che vengono registrati dall'elenco di avvio o modificando la configurazione nelle applicazioni e componenti di sistema per disabilitarli. È possibile usare **personalizzare le notifiche di icone** per esaminare l'elenco delle notifiche disponibili nel server.

### <a name="remotefx-data-compression"></a>Compressione dei dati di RemoteFX

La compressione di Microsoft RemoteFX può essere configurata tramite criteri di gruppo nello **configurazione Computer &gt; modelli amministrativi &gt; i componenti di Windows &gt; Remote Desktop Services &gt; remoto Host sessione desktop &gt; Remote Environment di sessione &gt; configurare la compressione per i dati di RemoteFX**. Sono possibili tre valori:

-   **Ottimizzato per usare meno memoria** consuma la quantità minima di memoria per ogni sessione, ma è rapporto di compressione più basso e pertanto il consumo di larghezza di banda più elevato.

-   **Consente di bilanciare la larghezza di banda di rete e memoria** ridotto il consumo di larghezza di banda aumentando leggermente il consumo di memoria (circa 200 KB per ogni sessione).

-   **Con ottimizzazione per la minore larghezza di banda di rete usare** riduce ulteriormente l'utilizzo della larghezza di banda di rete a un costo di circa 2 MB per ogni sessione. Se si desidera usare questa impostazione, occorre valutare il numero massimo di sessioni e di test per tale livello con questa impostazione prima di inserire il server nell'ambiente di produzione.

È anche possibile scegliere di non usare un algoritmo di compressione di RemoteFX. Scelta di non usare un algoritmo di compressione RemoteFX userà più larghezza di banda di rete ed è consigliabile solo se si usa un dispositivo hardware che è progettato per ottimizzare il traffico di rete. Anche se si sceglie di non utilizzare un algoritmo di compressione RemoteFX, alcuni dati di grafica verranno compresso.

### <a name="device-redirection"></a>Reindirizzamento del dispositivo

Il reindirizzamento del dispositivo può essere configurato tramite criteri di gruppo nello **configurazione Computer &gt; modelli amministrativi &gt; i componenti di Windows &gt; Remote Desktop Services &gt; Desktop remoto Host sessione &gt; dispositivo e il reindirizzamento della risorsa** oppure usando la **insieme di sessioni** finestra di dialogo in Server Manager.

In genere, il reindirizzamento della periferica aumenta quanto più l'uso di connessioni server Host sessione Desktop remoto della larghezza di banda di rete perché i dati vengono scambiati tra i dispositivi nei computer client e i processi in esecuzione nella sessione del server. L'entità dell'aumento è una funzione della frequenza delle operazioni eseguite dalle applicazioni in esecuzione nel server per i dispositivi reindirizzati.

Il reindirizzamento della stampante e pronta per il reindirizzamento della periferica aumenta anche l'utilizzo di CPU al momento dell'accesso. È possibile reindirizzare le stampanti in due modi:

-   Corrispondenza basato sul driver il reindirizzamento della stampante quando un driver della stampante deve essere installato nel server. Nelle versioni precedenti di Windows Server viene utilizzato questo metodo.

-   Introdotta in Windows Server 2008, il reindirizzamento di driver della stampante Easy Print Usa un driver della stampante comune per tutte le stampanti.

Il metodo Easy Print è consigliabile perché causa minore utilizzo della CPU per l'installazione delle stampanti al momento della connessione. Il metodo di driver corrispondenti provoca un aumento dell'utilizzo della CPU perché richiede il servizio spooler di caricare i driver diversi. Per informazioni sull'utilizzo della larghezza di banda, Easy Print fa sì che utilizzo della larghezza di banda di rete leggermente maggiore, ma non abbastanza significativa da compensare gli altri vantaggi di prestazioni, gestibilità e affidabilità.

Il reindirizzamento audio fa sì che un flusso del traffico di rete. Il reindirizzamento audio consente inoltre agli utenti di eseguire App multimediale in genere con utilizzo elevato di CPU.

### <a name="client-experience-settings"></a>Impostazioni di esperienza client

Per impostazione predefinita, la connessione di Desktop remoto (RDC) sceglie automaticamente il diritto esperienza impostazione in base all'idoneità della connessione di rete tra i computer client e server. È consigliabile che la configurazione di RDC rimangono nella **rileva automaticamente la qualità della connessione**.

Per gli utenti esperti, la tecnologia RDC consente di controllare una gamma di impostazioni che influenzano le prestazioni della larghezza di banda di rete per la connessione di Servizi Desktop remoto. Le impostazioni seguenti è possibile accedere usando il **esperienza** scheda Connessione Desktop remoto o come impostazioni nel file RDP.

Quando ci si connette a qualsiasi computer, si applicano le impostazioni seguenti:

-   **Disable wallpaper=i:<0** (disabilita lo sfondo: i:0#.w|Contoso) non viene visualizzato lo sfondo del desktop per le connessioni reindirizzate. Questa impostazione può ridurre notevolmente utilizzo della larghezza di banda se lo sfondo del desktop è costituito da un'immagine o un altro contenuto con i notevoli costi correlati per il disegno.

-   **Cache bitmap** (Bitmapcachepersistenable:i:1) quando questa impostazione è abilitata, viene creata una cache lato client della bitmap a cui viene eseguito il rendering della sessione. Fornisce un miglioramento significativo sull'utilizzo della larghezza di banda e deve sempre essere abilitato (a meno che non esistono altre considerazioni sulla sicurezza).

-   **Visualizzare il contenuto di windows durante il trascinamento** (disabilitare la finestra completo. trascinare: allowfontsmoothing:i:1) quando questa impostazione è disabilitata, riduce la larghezza di banda visualizzando solo il frame della finestra anziché tutto il contenuto quando la finestra viene trascinata.

-   **Animazione menu e finestre** (Disable menu anims:i:1 e Disable cursor impostazione: allowfontsmoothing:i:1): Quando queste impostazioni sono disabilitate, riduce la larghezza di banda disabilitando l'animazione dei menu (ad esempio, dissolvenza) e i cursori.

-   **Caratteri smussati** (Allow font smoothing=i:<0: i:0#.w|Contoso) supporto per il rendering dei caratteri ClearType controlli. Quando ci si connette ai computer che eseguono Windows 8 o Windows Server 2012 e versioni successive, abilitazione o disabilitazione di questa impostazione non ha un impatto significativo sull'utilizzo della larghezza di banda. Tuttavia, per i computer che eseguono versioni precedenti a Windows 7 e Windows 2008 R2, si abilita questa impostazione influisce sul consumo di larghezza di banda di rete in modo significativo.

Le impostazioni seguenti si applicano solo quando ci si connette ai computer che eseguono Windows 7 e versioni precedenti del sistema operativo:

-   **Composizione del desktop** questa impostazione è supportata solo per una sessione remota per un computer che esegue Windows 7 o Windows Server 2008 R2.

-   **Stili visivi** (disabilitare i temi: allowfontsmoothing:i:1) quando questa impostazione è disabilitata, riduce la larghezza di banda mediante la semplificazione disegni tema che utilizzano il tema Classic.

Tramite il **esperienza** scheda all'interno di connessione Desktop remoto, è possibile scegliere la velocità della connessione per influenzare le prestazioni della larghezza di banda di rete. Di seguito sono elencate le opzioni disponibili configurare la velocità della connessione:

-   **Rilevare automaticamente la qualità della connessione** (tipo di connessione: i:7) quando questa impostazione è abilitata, connessione Desktop remoto sceglie automaticamente impostazioni che determineranno l'esperienza utente ottimale in base la qualità della connessione. (Questa configurazione è consigliata quando ci si connette ai computer che eseguono Windows 8 o Windows Server 2012 e versioni successive).

-   **Modem (56 Kbps)** (tipo di connessione: allowfontsmoothing:i:1) questa impostazione abilita la memorizzazione nella cache di bitmap permanente.

-   **Bassa velocità a banda larga (256 Kbps - 2 Mbps)** (tipo di connessione: i:2) questa impostazione abilita gli stili di memorizzazione nella cache e visual bitmap permanente.

-   **Rete cellulare o Satellite (2 Mbps - 16 Mbps con una latenza elevata)** (tipo di connessione: i:3) questa impostazione consente di composizione del desktop, la memorizzazione nella cache persistente bitmap, gli stili di visualizzazione e sfondo del desktop.

-   **Connessione a banda larga ad alta velocità (2 Mbps-10 Mbps)** (tipo di connessione: i:4) questa impostazione consente la composizione del desktop, visualizzare il contenuto di windows durante il trascinamento, animazione menu e finestre, la memorizzazione nella cache persistente bitmap, gli stili di visualizzazione e sfondo del desktop.

-   **WAN (10 Mbps o superiore con una latenza elevata)** (tipo di connessione: i:5) questa impostazione consente la composizione del desktop, visualizzare il contenuto di windows durante il trascinamento, animazione menu e finestre, la memorizzazione nella cache persistente bitmap, gli stili di visualizzazione e sfondo del desktop.

-   **LAN (10 Mbps o superiore)** (tipo di connessione: i:6) questa impostazione consente la composizione del desktop, visualizzare il contenuto di windows durante il trascinamento, animazione menu e finestre, la memorizzazione nella cache persistente bitmap, temi e sfondo del desktop.

### <a name="desktop-size"></a>Dimensioni del desktop

Dimensioni del desktop per sessioni remote possono essere controllate utilizzando la scheda di visualizzazione in connessione Desktop remoto oppure usando il file di configurazione di RDP (desktopwidth:i:1152 e desktopheight:i:864). Maggiore è la dimensione del desktop, maggiore di memoria e larghezza di banda consumo associata a tale sessione. Le dimensioni correnti di desktop massima sono 4096 x 2048.
