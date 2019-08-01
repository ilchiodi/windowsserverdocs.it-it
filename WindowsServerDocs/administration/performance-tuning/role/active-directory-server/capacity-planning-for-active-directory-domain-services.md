---
title: Pianificazione della capacità per Active Directory Domain Services
description: Descrizione dettagliata dei fattori da prendere in considerazione durante la pianificazione della capacità per servizi di dominio Active Directory.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 5a9e2d39d4eedd1e8fdb4bfeaf267ad4cb4c596a
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/31/2019
ms.locfileid: "67799833"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Pianificazione della capacità per Active Directory Domain Services

Questo argomento è stato scritto in origine da Ken Brumfield, Senior Premier Field Engineer in Microsoft e fornisce indicazioni per la pianificazione della capacità per Active Directory Domain Services (AD DS).

## <a name="goals-of-capacity-planning"></a>Obiettivi della pianificazione della capacità

La pianificazione della capacità non è uguale alla risoluzione degli eventi imprevisti relativi alle prestazioni. Sono strettamente correlati, ma piuttosto diversi. Gli obiettivi della pianificazione della capacità sono:  

- Implementare e gestire correttamente un ambiente 
- Ridurre al minimo il tempo impiegato per la risoluzione dei problemi relativi alle prestazioni.  
  
Nella pianificazione della capacità, un'organizzazione potrebbe avere una destinazione di base del 40% di utilizzo del processore durante i periodi di picco, in modo da soddisfare i requisiti di prestazioni dei client e adattarsi al tempo necessario per l'aggiornamento dell'hardware nel Data Center. Per ricevere notifiche relative a eventi imprevisti di prestazioni anomali, è possibile che una soglia di avviso di monitoraggio venga impostata al 90% in un intervallo di 5 minuti.

La differenza consiste nel fatto che quando una soglia di gestione della capacità viene superata continuamente (un evento monouso non è un problema), l'aggiunta di capacità (ovvero l'aggiunta di processori più o più veloci) costituisce una soluzione o il ridimensionamento del servizio tra più server potrebbe essere un soluzione. Le soglie di avviso per le prestazioni indicano che l'esperienza del client è attualmente in sofferenza e sono necessari passaggi immediati per risolvere il problema.

Analogamente, la gestione della capacità riguarda la prevenzione di un incidente dell'auto (guida difensiva, la verifica del corretto funzionamento dei freni e così via), mentre la risoluzione dei problemi relativi alle prestazioni è il modo in cui la polizia, il reparto antincendio e i professionisti sanitari di emergenza Dopo un incidente. Si tratta di un tipo di "guida difensiva" Active Directory.

Negli ultimi anni, le linee guida per la pianificazione della capacità per i sistemi di scalabilità verticale sono state modificate in modo significativo. Le seguenti modifiche nelle architetture di sistema hanno richiesto presupposti fondamentali sulla progettazione e la scalabilità di un servizio:

- piattaforme server a 64 bit  
- Virtualizzazione  
- Maggiore attenzione al consumo di energia elettrica  
- Archiviazione SSD  
- Scenari cloud  

Inoltre, l'approccio si sposta da un esercizio di pianificazione della capacità basato su server a un esercizio di pianificazione della capacità basato su servizi. Active Directory Domain Services (AD DS), un servizio distribuito maturo che molti prodotti Microsoft e di terze parti usano come back-end, diventa uno dei prodotti più importanti da pianificare correttamente per garantire la capacità necessaria per l'esecuzione di altre applicazioni.

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>Requisiti di base per le linee guida sulla pianificazione della capacità

In questo articolo sono previsti i requisiti di base seguenti:

- I lettori hanno letto e hanno familiarità con le [linee guida per l'ottimizzazione delle prestazioni per Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- La piattaforma Windows Server è un'architettura basata su x64. Tuttavia, anche se l'ambiente di Active Directory è installato in Windows Server 2003 x86 (oltre la fine del ciclo di vita del supporto) e dispone di un albero delle informazioni di directory (DIT) che ha una dimensione inferiore a 1,5 GB e che può essere facilmente mantenuta in memoria, le linee guida da questa l'articolo è ancora applicabile.
- La pianificazione della capacità è un processo continuo ed è necessario rivedere regolarmente il modo in cui l'ambiente soddisfa le aspettative.
- L'ottimizzazione si verificherà su più cicli di vita hardware, in quanto i costi hardware cambiano. Ad esempio, la memoria diventa più economica, il costo per core diminuisce o il prezzo delle diverse opzioni di archiviazione cambia.
- Pianificare il periodo di picco della giornata. È consigliabile esaminarlo in intervalli di 30 minuti o ore. Qualsiasi valore maggiore può nascondere i picchi effettivi e qualsiasi valore inferiore potrebbe essere distorto da "picchi temporanei".
- Pianificare la crescita nel corso del ciclo di vita dell'hardware per l'azienda. Questo può includere una strategia di aggiornamento o di aggiunta di hardware in modo scaglionato oppure un aggiornamento completo ogni tre o cinque anni. Ogni richiederà una "ipotesi" come la quantità di carico su Active Directory aumenterà. I dati cronologici, se raccolti, contribuiranno a questa valutazione. 
- Pianificare la tolleranza di errore. Una volta ottenuta una stima *n* , pianificare gli scenari che *includono n* &ndash; 1 *, n* &ndash; 2, *n* &ndash; *x*.
  - Aggiungere altri server in base alle esigenze organizzative per assicurarsi che la perdita di uno o più server non superi le stime massime della capacità di picco.
  - Tenere inoltre presente che è necessario integrare il piano di crescita e il piano di tolleranza di errore. Se, ad esempio, è necessario un controller di dominio per supportare il carico, ma la stima è che il carico verrà raddoppiato nell'anno successivo e richiederà due controller di dominio totali, non sarà disponibile una capacità sufficiente per supportare la tolleranza di errore. La soluzione consiste nell'iniziare con tre controller di dominio. Uno può anche pianificare l'aggiunta del terzo controller di dominio dopo 3 o 6 mesi se i budget sono limitati.

    >[!NOTE]
    >L'aggiunta di applicazioni in grado di riconoscere Active Directory potrebbe avere un notevole effetto sul carico del controller di dominio, indipendentemente dal fatto che il carico provenga da client o server applicazioni.

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>Processo in tre passaggi per il ciclo di pianificazione della capacità

Nella pianificazione della capacità, prima di tutto decidere la qualità del servizio necessaria. Un data center principale supporta ad esempio un livello di concorrenza più elevato e richiede un'esperienza più coerente per gli utenti e le applicazioni che utilizzano, che richiedono maggiore attenzione alla ridondanza e alla riduzione dei colli di bottiglia di sistema e infrastruttura. Al contrario, una posizione satellite con pochi utenti non richiede lo stesso livello di concorrenza o tolleranza di errore. Per questo motivo, è possibile che l'ufficio satellite non debba prestare molta attenzione all'ottimizzazione dell'hardware e dell'infrastruttura sottostanti, il che può comportare risparmi sui costi. Tutte le indicazioni e le linee guida sono utili per ottenere prestazioni ottimali e possono essere ridotte in modo selettivo per gli scenari con requisiti meno complessi.

La domanda successiva è: virtualizzata o fisica? Dal punto di vista della pianificazione della capacità, non esiste una risposta corretta o errata; è disponibile un solo set di variabili diverso da usare. Gli scenari di virtualizzazione sono disponibili in una delle due opzioni seguenti:

- "Mapping diretto" con un Guest per host (dove la virtualizzazione esiste esclusivamente per astrarre l'hardware fisico dal server)
- "Host condiviso"

Gli scenari di test e produzione indicano che lo scenario "mapping diretto" può essere gestito in modo identico a un host fisico. "Host condiviso", tuttavia, introduce una serie di considerazioni più dettagliate in un secondo momento. Lo scenario "host condiviso" indica che i servizi di dominio Active Directory sono anche in competizione per le risorse e che per questa operazione sono presenti sanzioni e considerazioni sull'ottimizzazione.

Tenendo presenti queste considerazioni, il ciclo di pianificazione della capacità è un processo iterativo in tre passaggi:

1. Misurare l'ambiente esistente, determinare dove si trovano attualmente i colli di bottiglia del sistema e ottenere le nozioni di base dell'ambiente necessarie per pianificare la quantità di capacità necessaria.
1. Determinare l'hardware necessario in base ai criteri descritti nel passaggio 1.
1. Monitorare e verificare che l'infrastruttura implementata stia operando all'interno di specifiche. Alcuni dati raccolti in questo passaggio diventano la linea di base per il ciclo successivo di pianificazione della capacità.

### <a name="applying-the-process"></a>Applicazione del processo

Per ottimizzare le prestazioni, assicurarsi che i componenti principali siano selezionati correttamente e ottimizzati per i caricamenti dell'applicazione:

1. Memoria
1. Rete
1. Archiviazione
1. Processore
1. Accesso rete

I requisiti di archiviazione di base di servizi di dominio Active Directory e il comportamento generale del software client ben scritto consentono agli ambienti con un numero di 10.000 di 20.000 utenti di rinunciare a un notevole investimento nella pianificazione della capacità per quanto riguarda l'hardware fisico, come quasi tutti i server moderni il sistema di classe gestirà il carico. Detto questo, nella tabella seguente viene riepilogato come valutare un ambiente esistente per selezionare l'hardware appropriato. Ogni componente viene analizzato in dettaglio nelle sezioni successive per consentire agli amministratori di servizi di dominio Active Directory di valutare la propria infrastruttura usando le indicazioni di base e le entità specifiche dell'ambiente.

In generale:

- Tutte le dimensioni basate sui dati correnti saranno accurate solo per l'ambiente corrente.
- Per qualsiasi stima, si prevede che la domanda aumenti nel ciclo di vita dell'hardware.
- Consente di determinare se la capacità di oggi è più grande e di crescere nell'ambiente più ampio o di aumentare la capacità del ciclo di vita.
- Per la virtualizzazione, vengono applicate tutte le stesse entità e metodologie per la pianificazione della capacità, ad eccezione del fatto che l'overhead della virtualizzazione deve essere aggiunto a qualsiasi elemento correlato al dominio.
- La pianificazione della capacità, come qualsiasi elemento che tenta di prevedere, non è una scienza precisa. Non si prevede che le operazioni vengano calcolate perfettamente e con una precisione del 100%. Il materiale sussidiario è il Consiglio più snello. aggiungere la capacità per una maggiore sicurezza e verificare continuamente che l'ambiente rimanga nella destinazione.

### <a name="data-collection-summary-tables"></a>Tabelle di riepilogo raccolta dati

#### <a name="new-environment"></a>Nuovo ambiente

| Componente | Stime |
|-|-|
|Dimensioni di archiviazione/database|da 40 KB a 60 KB per ogni utente|
|RAM|Dimensioni del database<br />Raccomandazioni del sistema operativo di base<br />Applicazioni di terze parti|
|Rete|1 GB|
|CPU|1000 utenti simultanei per ogni core|

#### <a name="high-level-evaluation-criteria"></a>Criteri di valutazione di alto livello

| Componente | Criteri di valutazione | Considerazioni sulla pianificazione |
|-|-|-|
|Dimensioni di archiviazione/database|La sezione intitolata "per attivare la registrazione dello spazio su disco liberato dalla deframmentazione" nei [limiti di archiviazione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Prestazioni di archiviazione/database|<ul><li>"Disco logico ( *\<\>unità del database NTDS*) \Avg letture disco/sec," "disco logico ( *\<unità\>di database NTDS*) \Avg disco/sec," "disco logico ( *\<unità di database NTDS )\>* \Avg/sec disco "</li><li>"Disco logico ( *\<\>unità del database NTDS*) \ Reads/sec," "disco logico ( *\<unità\>del database NTDS*) \ scritture/sec," "disco logico ( *\<unità\>deldatabaseNTDS*) \ Trasferimenti/sec "</li></ul>|<ul><li>L'archiviazione presenta due aspetti da affrontare<ul><li>Spazio disponibile, che, con le dimensioni della data odierna basata su mandrino e archiviazione basata su SSD, è irrilevante per la maggior parte degli ambienti AD.</li> <li>Operazioni di i/o (input/output) disponibili: in molti ambienti questa operazione viene spesso trascurata. Tuttavia è importante valutare solo gli ambienti in cui non è disponibile RAM sufficiente per caricare l'intero database NTDS in memoria.</li></ul><li>L'archiviazione può essere un argomento complesso e deve coinvolgere le competenze del fornitore dell'hardware per il dimensionamento corretto. In particolare con scenari più complessi, ad esempio gli scenari SAN, NAS e iSCSI. Tuttavia, in generale, il costo per Gigabyte di archiviazione è spesso in opposizione diretta ai costi per IO:<ul><li>RAID 5 ha un costo inferiore per Gigabyte rispetto a RAID 1, ma RAID 1 ha un costo inferiore per i/o</li><li>I dischi rigidi basati su mandrino hanno un costo inferiore per Gigabyte, ma le SSD hanno un costo inferiore per i/o</li></ul><li>Dopo il riavvio del computer o del servizio Active Directory Domain Services, la cache Extensible Storage Engine (ESE) è vuota e le prestazioni saranno vincolate dal disco mentre la cache viene riscaldata.</li><li>Nella maggior parte degli ambienti AD è possibile leggere I/O intensivo in un modello casuale sui dischi, negando gran parte dei vantaggi della memorizzazione nella cache e delle strategie di ottimizzazione per la lettura.  Inoltre, AD ha una cache di dimensioni superiori in memoria rispetto alla maggior parte delle cache del sistema di archiviazione.</li></ul>
|RAM|<ul><li>Dimensioni del database</li><li>Raccomandazioni del sistema operativo di base</li><li>Applicazioni di terze parti</li></ul>|<ul><li>L'archiviazione è il componente più lento in un computer. Maggiore è il numero che può essere residente in RAM, minore è la necessità di passare al disco.</li><li>Assicurarsi che venga allocata una quantità di RAM sufficiente per archiviare il sistema operativo, gli agenti (antivirus, backup, monitoraggio), il database NTDS e la crescita nel tempo.</li><li>Per gli ambienti in cui la massimizzazione della quantità di RAM non è economicamente conveniente (ad esempio, percorsi satellite) o non fattibile (il DIT è troppo grande), fare riferimento alla sezione archiviazione per assicurarsi che le dimensioni dell'archiviazione siano corrette.</li></ul>|
|Rete|<ul><li>"Interfaccia di rete\*() \Byte ricevuti/sec"</li><li>"Interfaccia di rete\*() \Byte inviati/sec"|<ul><li>In generale, il traffico inviato da un controller di dominio supera molto il traffico inviato a un controller di dominio.</li><li>Poiché una connessione Ethernet comcambiata è full duplex, il traffico di rete in ingresso e in uscita deve essere ridimensionato in modo indipendente.</li><li>Il consolidamento del numero di controller di dominio aumenterà la quantità di larghezza di banda utilizzata per inviare risposte alle richieste client per ogni controller di dominio, ma sarà abbastanza vicino a lineare per il sito nel suo complesso.</li><li>Se si rimuovono i controller di dominio del percorso satellite, non dimenticare di aggiungere la larghezza di banda per il controller di dominio satellite nei controller di dominio dell'hub e di usarla per valutare la quantità di traffico WAN.</li></ul>|
|CPU|<ul><li>"Disco logico ( *\<\>unità del database NTDS*) \Avg letture disco/sec"</li><li>"Elaborazione (LSASS)\\% tempo processore"</li></ul>|<ul><li>Dopo aver eliminato l'archiviazione come collo di bottiglia, risolvere la quantità di potenza di calcolo necessaria.</li><li>Sebbene non sia perfettamente lineare, il numero di core del processore utilizzati in tutti i server all'interno di un ambito specifico, ad esempio un sito, può essere usato per misurare il numero di processori necessari per supportare il carico totale del client. Aggiungere il requisito minimo necessario per mantenere il livello di servizio corrente in tutti i sistemi all'interno dell'ambito.</li><li>Modifiche della velocità del processore, incluse le modifiche relative al risparmio energia, i numeri di impatto derivati dall'ambiente corrente. In genere, non è possibile valutare con precisione il modo in cui un processore da 2,5 GHz a un processore a 3 GHz ridurrà il numero di CPU necessarie.</li></ul>|
|Accesso rete|<ul><li>"Netlogon (\*) \Semaphore acquisisce"</li><li>"Netlogon (\*) \Semaphore timeout"</li><li>"Netlogon (\*) \Latenza tempo di attesa del semaforo"</li></ul>|<ul><li>Canale sicuro accesso rete/MaxConcurrentAPI influiscono solo sugli ambienti con autenticazione NTLM e/o convalida PAC. La convalida PAC è attiva per impostazione predefinita nelle versioni del sistema operativo precedenti a Windows Server 2008. Si tratta di un'impostazione client, di conseguenza i controller di dominio saranno interessati fino a quando non viene disattivato in tutti i sistemi client.</li><li>Gli ambienti con autenticazione con attendibilità significativa, che include i trust tra foreste, presentano maggiori rischi se non vengono ridimensionati correttamente.</li><li>I consolidamenti dei server aumenteranno la concorrenza dell'autenticazione tra trust.</li><li>È necessario gestire i picchi, ad esempio i failover del cluster, quando gli utenti eseguono di nuovo l'autenticazione in massa nel nuovo nodo del cluster.</li><li>Potrebbe essere necessario ottimizzare i singoli sistemi client (ad esempio un cluster).</li></ul>|

## <a name="planning"></a>Pianificazione

Per un lungo periodo di tempo, la raccomandazione della community per il dimensionamento di servizi di dominio Active Directory è stata quella di "inserire quantità di RAM pari alla dimensione del database". Nella maggior parte dei casi, questa raccomandazione è la maggior parte degli ambienti in cui è necessario preoccuparsi. Tuttavia, l'ecosistema che utilizza servizi di dominio Active Directory è diventato molto più grande, così come gli ambienti di servizi di dominio Active Directory, fin dall'introduzione in 1999. Anche se l'aumento della potenza di calcolo e il passaggio dalle architetture x86 alle architetture x64 hanno reso gli aspetti più sottili del dimensionamento per le prestazioni irrilevanti per un set più ampio di clienti che eseguono servizi di dominio Active Directory su hardware fisico, la crescita della virtualizzazione ha sono state riintrodotte le problematiche di ottimizzazione per un gruppo di destinatari maggiore di quello precedente.

Di seguito sono riportate le indicazioni su come determinare e pianificare le richieste di Active Directory come servizio, indipendentemente dal fatto che sia distribuito in un fisico, in una combinazione virtuale/fisica o in uno scenario puramente virtualizzato. In questo modo, la valutazione viene suddivisa in ognuno dei quattro componenti principali: archiviazione, memoria, rete e processore. In breve, per ottimizzare le prestazioni in servizi di dominio Active Directory, l'obiettivo è quello di avvicinarsi al più possibile il limite del processore.

## <a name="ram"></a>RAM

Semplicemente, maggiore è la possibilità di memorizzare nella cache la RAM, minore è la necessità di passare al disco. Per ottimizzare la scalabilità del server, la quantità minima di RAM deve corrispondere alla somma delle dimensioni correnti del database, della dimensione totale di SYSVOL, della quantità consigliata del sistema operativo e delle raccomandazioni del fornitore per gli agenti (antivirus, monitoraggio, backup e così via) ). È necessario aggiungere un importo aggiuntivo per adattarsi alla crescita nel corso della durata del server. Questa operazione sarà soggetta all'ambiente in base alle stime della crescita del database in base alle modifiche ambientali.

Per gli ambienti in cui la massimizzazione della quantità di RAM non è conveniente (ad esempio, percorsi satellite) o non fattibile (DIT è troppo grande), fare riferimento alla sezione archiviazione per assicurarsi che l'archiviazione sia progettata correttamente.

Un corollario che viene visualizzato nel contesto generale della memoria di ridimensionamento è il dimensionamento del file di paging. Nello stesso contesto di tutti gli altri elementi correlati alla memoria, l'obiettivo è ridurre al minimo il disco più lento. Quindi, la domanda dovrebbe essere "come dovrebbe essere dimensionato il file di paging". per "quanta RAM è necessaria per ridurre al minimo il paging?" La risposta alla seconda domanda è illustrata nella parte restante di questa sezione. In questo modo si lascia la maggior parte della discussione per dimensionare il file di paging nell'area di lavoro raccomandazioni generali del sistema operativo e la necessità di configurare il sistema per i dump della memoria, che non sono correlati alle prestazioni di servizi di dominio Active Directory.

### <a name="evaluating"></a>Valutazione

La quantità di RAM richiesta da un controller di dominio è in realtà un esercizio complesso per i motivi seguenti:

- Potenziale elevato errore quando si tenta di utilizzare un sistema esistente per misurare la quantità di RAM necessaria perché LSASS ridurrà in condizioni di utilizzo intensivo della memoria, riducendo artificialmente la necessità.
- Il fatto che un singolo controller di dominio deve memorizzare nella cache solo gli elementi "interessanti" per i client. Ciò significa che i dati che devono essere memorizzati nella cache in un controller di dominio in un sito con un solo server Exchange saranno molto diversi rispetto ai dati che devono essere memorizzati nella cache in un controller di dominio che autentica solo gli utenti.
- Il lavoro per valutare la RAM per ogni controller di dominio per ogni singolo caso è proibitivo e cambia man mano che l'ambiente cambia.
- I criteri alla base della raccomandazione contribuiranno a prendere decisioni informate: 
- Maggiore è la memorizzazione nella cache di RAM, minore è la necessità di passare al disco. 
- Lo spazio di archiviazione è di gran lunga il componente più lento di un computer. L'accesso ai dati in un supporto di archiviazione SSD e basato su mandrino è più lento dell'accesso ai dati in RAM.

Pertanto, per ottimizzare la scalabilità del server, la quantità minima di RAM è la somma delle dimensioni correnti del database, della dimensione totale di SYSVOL, della quantità consigliata del sistema operativo e delle raccomandazioni del fornitore per gli agenti (antivirus, monitoraggio, backup, e così via). Aggiungere ulteriori importi per supportare la crescita per la durata del server. Questa operazione sarà soggetta all'ambiente in base alle stime della crescita del database. Tuttavia, per le località satellite con un piccolo set di utenti finali, questi requisiti possono essere rilassati perché questi siti non dovranno memorizzare nella cache la maggior parte delle richieste.

Per gli ambienti in cui la massimizzazione della quantità di RAM non è economicamente conveniente (ad esempio, percorsi satellite) o non fattibile (il DIT è troppo grande), fare riferimento alla sezione archiviazione per assicurarsi che le dimensioni dell'archiviazione siano corrette.

> [!NOTE]
> Un corollario durante il ridimensionamento della memoria è il dimensionamento del file di paging. Poiché l'obiettivo è ridurre al minimo il disco più lento, la domanda va da "come deve essere ridimensionato il file di paging". per "quanta RAM è necessaria per ridurre al minimo il paging?" La risposta alla seconda domanda è illustrata nella parte restante di questa sezione. In questo modo si lascia la maggior parte della discussione per dimensionare il file di paging nell'area di lavoro raccomandazioni generali del sistema operativo e la necessità di configurare il sistema per i dump della memoria, che non sono correlati alle prestazioni di servizi di dominio Active Directory.

### <a name="virtualization-considerations-for-ram"></a>Considerazioni sulla virtualizzazione per RAM

Evitare il overcommit della memoria nell'host. L'obiettivo fondamentale di ottimizzare la quantità di RAM consiste nel ridurre al minimo la quantità di tempo impiegato per il disco. Negli scenari di virtualizzazione, il concetto di sovracommit della memoria è disponibile quando viene allocata una quantità maggiore di RAM ai guest, quindi esiste nel computer fisico. Questo non è un problema. Questo problema si verifica quando la memoria totale usata attivamente da tutti i guest supera la quantità di RAM nell'host e l'host sottostante avvia il paging. Le prestazioni diventano associate al disco nei casi in cui il controller di dominio passa a NTDS. dit per ottenere i dati oppure il controller di dominio passa al file di paging per ottenere i dati oppure l'host sta per ottenere i dati che il Guest ritiene sia in RAM.

### <a name="calculation-summary-example"></a>Esempio di riepilogo del calcolo

|Componente|Memoria stimata (esempio)|
|-|-|
|RAM consigliata del sistema operativo di base (Windows Server 2008)|2 GB|
|Attività interne LSASS|200 MB|
|Agente di monitoraggio|100 MB|
|Antivirus|100 MB|
|Database (catalogo globale)|8,5 GB???|
|Cuscino per l'esecuzione del backup, gli amministratori possono accedere senza alcun effetto|1 GB|
|Totale|12 GB|

**Consigliabile 16 GB**

Nel corso del tempo, è possibile fare in modo che un numero maggiore di dati venga aggiunto al database e che il server sarà in produzione da 3 a 5 anni. In base a una stima della crescita del 33%, 16 GB sarebbero una quantità ragionevole di RAM da inserire in un server fisico. In una macchina virtuale, data la facilità con cui è possibile modificare le impostazioni e la RAM può essere aggiunta alla macchina virtuale, a partire da 12 GB il piano di monitoraggio e aggiornamento in futuro è ragionevole.

## <a name="network"></a>Rete

### <a name="evaluating"></a>Valutazione
In questa sezione viene illustrata la valutazione delle richieste relative al traffico di replica, che è incentrato sul traffico che attraversa la rete WAN ed è trattato in modo approfondito nel [traffico di replica Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), rispetto alla valutazione della larghezza di banda totale e della rete capacità necessaria, inclusione di query client, applicazioni Criteri di gruppo e così via. Per gli ambienti esistenti, questo può essere raccolto usando i contatori delle prestazioni "Network\*Interface () \Byte received/sec" e "Network\*(Interface ) \Byte Sent/sec". Intervalli di campionamento per i contatori dell'interfaccia di rete in 15, 30 o 60 minuti. Un valore inferiore sarà in genere troppo volatile per le misurazioni valide; un valore maggiore consiste nel smussare le operazioni di lettura giornaliera.

> [!NOTE]
> Generalmente, la maggior parte del traffico di rete in un controller di dominio è in uscita perché il controller di dominio risponde alle query client. Questo è il motivo per cui è necessario concentrarsi sul traffico in uscita, sebbene sia consigliabile valutare ogni ambiente anche per il traffico in ingresso. Gli stessi approcci possono essere usati per gestire e verificare i requisiti del traffico di rete in ingresso. Per ulteriori informazioni, vedere l'articolo [della Knowledge Base 929851: L'intervallo di porte dinamiche predefinito per TCP/IP è stato modificato in Windows Vista e in Windows](http://support.microsoft.com/kb/929851)Server 2008.

### <a name="bandwidth-needs"></a>Esigenze di larghezza di banda

La pianificazione della scalabilità della rete riguarda due categorie distinte: la quantità di traffico e il carico della CPU dal traffico di rete. Ognuno di questi scenari è diretto rispetto ad altri argomenti in questo articolo.

Per valutare la quantità di traffico che deve essere supportata, esistono due categorie univoche di pianificazione della capacità per servizi di dominio Active Directory in termini di traffico di rete. Il primo è il traffico di replica che attraversa i controller di dominio e viene trattato accuratamente nel riferimento [Active Directory il traffico di replica](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) ed è ancora pertinente per le versioni correnti di servizi di dominio Active Directory. Il secondo è il traffico da client a server tra siti. Uno degli scenari più semplici da pianificare, il traffico all'insito riceve principalmente richieste di piccole dimensioni da parte dei client rispetto alle grandi quantità di dati restituiti ai client. 100 MB saranno in genere idonei in ambienti fino a 5.000 utenti per server, in un sito. È consigliabile usare una scheda di rete da 1 GB e il supporto di Receive-Side Scaling (RSS) per qualsiasi elemento superiore a 5.000 utenti. Per convalidare questo scenario, in particolare nel caso di scenari di consolidamento dei server, esaminare\*l'interfaccia di rete () \ byte/sec in tutti i controller di dominio in un sito, aggiungerli insieme e dividere il numero di destinazione dei controller di dominio per assicurarsi che è una capacità adeguata. Il modo più semplice per eseguire questa operazione consiste nell'utilizzare la visualizzazione "area in pila" in Monitoraggio affidabilità e performance monitor di Windows (precedentemente noto come PerfMon), assicurandosi che tutti i contatori vengano ridimensionati.

Si consideri l'esempio seguente, noto anche come un modo davvero complesso per verificare che la regola generale sia applicabile a un ambiente specifico. Vengono eseguiti i presupposti seguenti:

- L'obiettivo consiste nel ridurre il footprint al minor numero possibile di server. Idealmente, un server comporterà il carico e verrà distribuito un server aggiuntivo per la ridondanza (scenario *N* + 1). 
- In questo scenario, la scheda di rete corrente supporta solo 100 MB e si trova in un ambiente a commutazione.  
  L'utilizzo massimo della larghezza di banda di rete di destinazione è pari al 60% in uno scenario N (perdita di un controller di dominio).
- A ogni server sono connessi circa 10.000 client.

Informazioni ottenute dai dati nel grafico (interfaccia di rete (\*) \Byte inviati/sec):

1. Il giorno lavorativo inizia a dilagare circa 5:30 e si snoda a 7:00 PM.
1. Il periodo di picco più occupato è compreso tra 8:00 e 8:15, con più di 25 byte inviati al secondo sul controller di dominio più occupato.  
   > [!NOTE]
   > Tutti i dati sulle prestazioni sono cronologici. Il punto dati di picco a 8:15 indica il carico da 8:00 a 8:15.
1. Sono presenti picchi prima delle 4:00, con oltre 20 byte inviati al secondo sul controller di dominio più trafficato, che potrebbero indicare un carico da fusi orari diversi o attività di infrastruttura in background, ad esempio i backup. Poiché il picco alle 8:00 AM supera questa attività, non è pertinente.
1. Sono presenti cinque controller di dominio nel sito.
1. Il carico massimo è di circa 5,5 MB/s per controller di dominio, che rappresenta il 44% della connessione 100 MB. Usando questi dati, è possibile stimare che la larghezza di banda totale necessaria tra 8:00 AM e 8:15 AM sia 28 MB/s.
   >[!NOTE]
   >Prestare attenzione nel fatto che i contatori di interfaccia di rete inviati/ricevuti sono in byte e che la larghezza di banda di rete è misurata in bit. 100 MB &divide; 8 = 12,5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusioni

1. Questo ambiente corrente soddisfa il livello N + 1 di tolleranza di errore al 60% dell'utilizzo di destinazione. Se si utilizza un sistema offline, la larghezza di banda per server viene spostata da circa 5,5 MB/s (44%) fino a circa 7 MB/s (56%).
1. In base all'obiettivo indicato in precedenza di consolidare in un server, ciò supera l'utilizzo massimo della destinazione e, in teoria, il possibile utilizzo di una connessione 100 MB.
1. Con una connessione da 1 GB, questo rappresenterà il 22% della capacità totale.
1. In condizioni operative normali nello scenario *N* + 1, il carico client sarà distribuito in modo relativamente uniforme a circa 14 MB/s per server o l'11% della capacità totale.
1. Per garantire che la capacità sia adeguata durante la mancata disponibilità di un controller di dominio, le normali destinazioni operative per server sono circa il 30% di utilizzo della rete o 38 MB/s per server. Le destinazioni di failover sarebbero il 60% di utilizzo della rete o 72 MB/s per server.  
  
In breve, la distribuzione finale dei sistemi deve avere una scheda di rete da 1 GB ed essere connessa a un'infrastruttura di rete che supporterà il carico. Si noti inoltre che, data la quantità di traffico di rete generato, il carico della CPU dalle comunicazioni di rete può avere un impatto significativo e limitare la scalabilità massima di servizi di dominio Active Directory. Questo stesso processo può essere usato per stimare la quantità di comunicazione in ingresso verso il controller di dominio. Tuttavia, dato il predominamento del traffico in uscita rispetto al traffico in ingresso, si tratta di un esercizio accademico per la maggior parte degli ambienti. Verificare che il supporto hardware per RSS sia importante in ambienti con più di 5.000 utenti per server. Per gli scenari con traffico di rete elevato, il bilanciamento del carico di interrupt può costituire un collo di bottiglia. Questo può essere rilevato dal tempo di\*interrupt\% del processore () che viene distribuito in modo non uniforme tra le CPU. Le NIC abilitate per RSS possono attenuare questa limitazione e aumentare la scalabilità.

> [!NOTE]
> Un approccio simile può essere usato per stimare la capacità aggiuntiva necessaria durante il consolidamento dei data center o il ritiro di un controller di dominio in una posizione satellite. È sufficiente raccogliere il traffico in uscita e in ingresso ai client e che sarà la quantità di traffico che ora sarà presente nei collegamenti WAN.  
>  
> In alcuni casi, è possibile che si verifichi un traffico maggiore del previsto perché il traffico è più lento, ad esempio quando il controllo del certificato non riesce a soddisfare i timeout aggressivi sulla rete WAN. Per questo motivo, le dimensioni e l'utilizzo della rete WAN devono essere un processo iterativo e continuo.

### <a name="virtualization-considerations-for-network-bandwidth"></a>Considerazioni sulla virtualizzazione per la larghezza di banda di rete

È facile creare raccomandazioni per un server fisico: 1 GB per i server che supportano più di 5000 utenti. Quando più Guest iniziano a condividere un'infrastruttura del Commuter virtuale sottostante, è necessario prestare particolare attenzione per garantire che l'host disponga di una larghezza di banda di rete adeguata per supportare tutti gli utenti del sistema e quindi richiede il rigore aggiuntivo. Non si tratta di un'estensione che garantisce l'infrastruttura di rete nel computer host. Indipendentemente dal fatto che la rete sia compresa tra il controller di dominio in esecuzione come guest macchina virtuale in un host con il traffico di rete che passa attraverso un commutire virtuale o se è connesso direttamente a un Commuter fisico. Il Commuter virtuale è solo uno dei componenti in cui l'uplink deve supportare la quantità di dati trasmessi. Pertanto, la scheda di rete fisica dell'host fisico collegata al compartitore dovrebbe essere in grado di supportare il carico del controller di dominio e tutti gli altri guest che condividono il commutire virtuale connesso alla scheda di rete fisica.

### <a name="calculation-summary-example"></a>Esempio di riepilogo del calcolo

|Sistema|Larghezza di banda massima|
|-|-|
CONTROLLER DI DOMINIO 1|6,5 MB/s|
CONTROLLER DI DOMINIO 2|6,25 MB/s|
|DC 3|6,25 MB/s|
|DC 4|5,75 MB/s|
|DC 5|4,75 MB/s|
|Totale|28,5 MB/s|

**Consigliabile 72 MB/s** (28,5 MB/s diviso per 40%)

|Conteggio sistema/i di destinazione|Larghezza di banda totale (precedente)|
|-|-|
|2|28,5 MB/s|
|Comportamento normale risultante|28,5 &divide; 2 = 14,25 MB/s|

Come sempre, nel corso del tempo è possibile fare in modo che il carico del client aumenti e che la crescita venga pianificata nel modo più appropriato possibile. La quantità consigliata per la pianificazione di potrebbe consentire una crescita stimata nel traffico di rete del 50%.

## <a name="storage"></a>Archiviazione

La pianificazione dell'archiviazione costituisce due componenti:

- Capacità o dimensioni di archiviazione
- Prestazioni

Una notevole quantità di tempo e documentazione viene spesa per la pianificazione della capacità, lasciando le prestazioni spesso completamente trascurate. Con i costi hardware correnti, la maggior parte degli ambienti non è sufficientemente grande che uno di questi è effettivamente un problema e la raccomandazione di "inserire la quantità di RAM come le dimensioni del database" in genere copre il resto, sebbene possa essere eccessivo per le località satellite più grandi ambienti.

### <a name="sizing"></a>Ridimensionamento

#### <a name="evaluating-for-storage"></a>Valutazione per l'archiviazione

Rispetto a 13 anni fa, quando Active Directory è stato introdotto, un'ora in cui le unità da 4 GB e 9 GB erano le dimensioni di unità più comuni, il dimensionamento per Active Directory non è ancora una considerazione per tutti gli ambienti tranne quelli più grandi. Con le dimensioni minime del disco rigido disponibili nell'intervallo di 180 GB, l'intero sistema operativo, SYSVOL e NTDS. dit possono essere facilmente inclusi in un'unica unità. Di conseguenza, si consiglia di deprecare un notevole investimento in quest'area.

L'unica indicazione da considerare è garantire che il 110% delle dimensioni NTDS. dit sia disponibile per consentire la deframmentazione. Inoltre, le sistemazioni per la crescita del ciclo di vita dell'hardware devono essere apportate.

La prima e più importante considerazione è la valutazione delle dimensioni di NTDS. dit e SYSVOL. Queste misure consentiranno di dimensionare sia il disco fisso che l'allocazione di RAM. A causa del costo relativamente basso di questi componenti, non è necessario che la matematica sia rigorosa e precisa. Il contenuto relativo alla valutazione di questa operazione sia per gli ambienti esistenti che per quelli nuovi è disponibile nella serie di [archiviazione dati](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) . In particolare, fare riferimento agli articoli seguenti:

- **Per gli ambienti &ndash; esistenti** la sezione intitolata "per attivare la registrazione dello spazio su disco liberato dalla deframmentazione" nell'articolo [limiti di archiviazione](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **Per i nuovi &ndash; ambienti** , l'articolo ha intitolato [stime di crescita per Active Directory utenti e unità organizzative](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > Gli articoli sono basati sulle stime delle dimensioni dei dati effettuate al momento del rilascio di Active Directory in Windows 2000. Utilizzare le dimensioni degli oggetti che riflettono le dimensioni effettive degli oggetti nell'ambiente in uso.

Quando si esaminano gli ambienti esistenti con più domini, è possibile che si verifichino variazioni nelle dimensioni del database. Se il valore è true, usare il catalogo globale più piccolo (GC) e le dimensioni non GC.  

Le dimensioni del database possono variare tra le versioni del sistema operativo. I controller di dominio che eseguono sistemi operativi precedenti, ad esempio Windows Server 2003, hanno dimensioni di database inferiori rispetto a un controller di dominio che esegue un sistema operativo successivo, ad esempio Windows Server 2008 R2, soprattutto quando sono abilitate le funzionalità di Active Directory Cestino o Roaming delle credenziali.

> [!NOTE]  
  >
>- Per i nuovi ambienti, si noti che le stime nelle stime di crescita per Active Directory utenti e le unità organizzative indicano che 100.000 utenti (nello stesso dominio) utilizzano circa 450 MB di spazio. Si noti che gli attributi popolati possono avere un notevole effetto sull'importo totale. Gli attributi verranno popolati in molti oggetti sia da prodotti di terze parti che da prodotti Microsoft, tra cui Microsoft Exchange Server e Lync. È preferibile una valutazione basata sul portfolio dei prodotti nell'ambiente, ma l'esercizio di dettagli della matematica e dei test per stime precise per tutti gli ambienti, tranne quelli più grandi, potrebbe non rivelarsi particolarmente utile.
>- Verificare che il 110% delle dimensioni NTDS. dit sia disponibile come spazio libero per consentire la deframmentazione non in linea e pianificare la crescita in una durata hardware di tre o cinque anni. Dato il livello di archiviazione a basso costo, la stima dell'archiviazione al 300% delle dimensioni del DIT, perché l'allocazione delle risorse di archiviazione è sicura per adattarsi alla crescita e la potenziale necessità di deframmentazione offline.

#### <a name="virtualization-considerations-for-storage"></a>Considerazioni sulla virtualizzazione per l'archiviazione

In uno scenario in cui vengono allocati più file di disco rigido virtuale (VHD) in un singolo volume, usare un disco di dimensioni fisse di almeno il 210% (100% del DIT + 110% di spazio disponibile) la dimensione del DIT per assicurarsi che lo spazio riservato sia sufficiente.  

#### <a name="calculation-summary-example"></a>Esempio di riepilogo del calcolo

|Dati raccolti dalla fase di valutazione| |
|-|-|
|Dimensioni NTDS. dit|35 GB|
|Modificatore per consentire la deframmentazione non in linea|2.1|
|Archiviazione totale necessaria|73,5 GB|

> [!NOTE]
> Questa archiviazione necessaria è aggiunta allo spazio di archiviazione necessario per SYSVOL, il sistema operativo, il file di paging, i file temporanei, i dati locali memorizzati nella cache, ad esempio i file del programma di installazione, e le applicazioni.

### <a name="storage-performance"></a>Prestazioni di archiviazione

#### <a name="evaluating-performance-of-storage"></a>Valutazione delle prestazioni di archiviazione

Poiché il componente più lento in qualsiasi computer, l'archiviazione può avere il maggiore impatto negativo sull'esperienza client. Per gli ambienti sufficientemente grandi per i quali non sono realizzabili le indicazioni relative al dimensionamento della RAM, le conseguenze dell'informativa sulla pianificazione delle prestazioni possono essere devastanti.  Inoltre, le complessità e le varietà della tecnologia di archiviazione aumentano ulteriormente il rischio di errore, in quanto la pertinenza delle procedure consigliate di lunga durata di "put sistema operativo, log e database" in dischi fisici distinti è limitata negli scenari utili.  Ciò è dovuto al fatto che la procedura consigliata è basata sul presupposto che un "disco" sia un mandrino dedicato e che l'I/O possa essere isolato.  Questi presupposti che rendono questo vero non sono più rilevanti per l'introduzione di:

- Nuovi tipi di archiviazione e scenari di archiviazione virtualizzati e condivisi
- Assi condivisi in una rete di archiviazione (SAN)
- File VHD in una rete SAN o nell'archiviazione collegata alla rete
- Unità a stato solido
- Architetture di archiviazione a più livelli (ad esempio, memorizzazione nella cache del livello di archiviazione SSD con maggiore archiviazione basata su mandrino)

In breve, l'obiettivo finale di tutte le attività di archiviazione, indipendentemente dall'architettura e dalla progettazione dell'archiviazione sottostante, consiste nel garantire che siano disponibili la quantità necessaria di operazioni di input/output al secondo (IOPS) e che tali IOPS vengano eseguite entro un intervallo di tempo accettabile . Questa sezione illustra come valutare le richieste di servizi di dominio Active Directory per l'archiviazione sottostante per garantire la corretta progettazione delle soluzioni di archiviazione.  Data la variabilità delle tecnologie di archiviazione odierne, è preferibile collaborare con i fornitori di archiviazione per garantire IOPS adeguati.  Per gli scenari con archiviazione collegata localmente, fare riferimento all'appendice C per informazioni di base su come progettare scenari tradizionali di archiviazione locale.  Queste entità sono generalmente applicabili a livelli di archiviazione più complessi e consentono anche di dialogare con i fornitori che supportano le soluzioni di archiviazione back-end.

- Data la vasta gamma di opzioni di archiviazione disponibili, è consigliabile coinvolgere le competenze dei team o dei fornitori del supporto hardware per garantire che la soluzione specifica soddisfi le esigenze di servizi di dominio Active Directory. I numeri seguenti sono le informazioni che verrebbero fornite agli specialisti di archiviazione.

Per gli ambienti in cui il database è troppo grande per essere mantenuto nella RAM, utilizzare i contatori delle prestazioni per determinare il numero di operazioni di I/O che devono essere supportate:

- Disco logico (\*) \Avg disco/sec (ad esempio, se NTDS. dit è archiviato in D:/ , il percorso completo sarà disco logico (D:) \Avg disco/sec)
- Disco logico (\*) \Avg scritture disco/sec
- Disco logico (\*) \Avg disco/sec
- Disco logico (\*) \ letture/sec
- Disco logico (\*) \ scritture/sec
- Disco logico (\*) \ trasferimenti/sec

Questi devono essere campionati in intervalli di 15/30/60 minuti per il benchmarking delle richieste dell'ambiente corrente.

#### <a name="evaluating-the-results"></a>Valutazione dei risultati

> [!NOTE]
> Lo stato attivo si basa sulle letture del database poiché questo è in genere il componente più complesso, la stessa logica può essere applicata alle Scritture nel file di log sostituendo disco logico ( *\<NTDS log\>* ) \Avg Disk sec/Write e disco logico (*Log\>NTDS) \ scritture/sec):\<*
>  
> - Disco logico ( *\<NTDS\>* ) \Avg disco/sec indica se l'archiviazione corrente è adeguatamente dimensionata.  Se i risultati sono approssimativamente uguali al tempo di accesso al disco per il tipo di disco, disco logico ( *\<NTDS\>* ) \ Reads/sec è una misura valida.  Controllare le specifiche del produttore per l'archiviazione nel back-end, ma gli intervalli validi per disco logico ( *\<NTDS\>* ) \Avg disco/sec saranno approssimativamente:
>   - 7200 – da 9 a 12,5 millisecondi (MS)
>   - 10.000 – da 6 a 10 ms
>   - 15.000 – da 4 a 6 ms  
>   - Unità SSD – da 1 a 3 ms  
>   - >[!NOTE]
>     >Sono disponibili raccomandazioni che indicano che le prestazioni di archiviazione sono degradate in 15ms in 20ms (a seconda dell'origine).  La differenza tra i valori precedenti e le altre linee guida è che i valori precedenti corrispondono al normale intervallo operativo.  Gli altri suggerimenti sono indicazioni per la risoluzione dei problemi che consentono di identificare i casi in cui l'esperienza client peggiora significativamente e diventa evidente.  Per una spiegazione più approfondita, fare riferimento all'appendice C.
> - Disco logico ( *\<NTDS\>* ) \ Reads/sec è la quantità di i/O che viene eseguita.
>   - Se disco logico ( *\<NTDS\>* ) \Avg disco/sec rientra nell'intervallo ottimale per l'archiviazione back-end, disco logico ( *\<NTDS\>* ) \ Reads/sec può essere usato direttamente per ridimensionare l'archiviazione.
>   - Se disco logico ( *\<NTDS\>* ) \Avg disco/sec non rientra nell'intervallo ottimale per l'archiviazione back-end, è necessario eseguire operazioni di i/O aggiuntive in base alla formula seguente:
>     > (Disco logico ( *\<NTDS\>* ) \Avg disco/sec) &divide; (tempo di accesso al disco multimediale fisico &times; ) (disco logico *\<(\>NTDS*) \Avg letture disco/sec)

Considerazioni:

- Si noti che se il server è configurato con una quantità di RAM non ottimale, questi valori non saranno accurati ai fini della pianificazione.  Saranno erroneamente sul lato alto e potranno comunque essere usati come scenario peggiore.
- L'aggiunta/ottimizzazione di RAM in modo specifico comporta una riduzione della quantità di i/O di lettura (disco logico ( *\<NTDS\>* ) \ letture/sec.  Ciò significa che la soluzione di archiviazione potrebbe non essere affidabile come inizialmente calcolata.  Sfortunatamente, qualsiasi elemento più specifico di questa istruzione generale dipende dall'ambiente dal carico del client e non è possibile fornire indicazioni generali.  L'opzione migliore consiste nel modificare le dimensioni di archiviazione dopo aver ottimizzato la RAM.

#### <a name="virtualization-considerations-for-performance"></a>Considerazioni sulla virtualizzazione per le prestazioni

Analogamente a tutte le precedenti discussioni di virtualizzazione, la chiave consiste nel garantire che l'infrastruttura condivisa sottostante possa supportare il carico del controller di dominio più le altre risorse usando i supporti condivisi sottostanti e tutti i percorsi. Questo vale se un controller di dominio fisico condivide lo stesso supporto sottostante in un'infrastruttura SAN, NAS o iSCSI come altri server o applicazioni, indipendentemente dal fatto che sia un guest che usa l'accesso pass-through a un'infrastruttura SAN, NAS o iSCSI che condivide il supporti sottostanti o se il guest utilizza un file VHD che risiede in un supporto condiviso localmente o in un'infrastruttura SAN, NAS o iSCSI. L'esercizio di pianificazione è tutto per assicurarsi che i supporti sottostanti possano supportare il carico totale di tutti i consumer.

Dal punto di vista Guest, inoltre, poiché sono presenti percorsi di codice aggiuntivi che devono essere attraversati, si verifica un effetto sulle prestazioni per la necessità di passare attraverso un host per accedere a qualsiasi risorsa di archiviazione. Non sorprendentemente, il test delle prestazioni di archiviazione indica che la virtualizzazione ha un effetto sulla velocità effettiva soggetta all'utilizzo del processore del sistema host (vedere Appendice A: Criteri di ridimensionamento della CPU), che è ovviamente influenzato dalle risorse dell'host richiesto dal Guest. Ciò contribuisce alle considerazioni di virtualizzazione relative alle esigenze di elaborazione in uno scenario virtualizzato. vedere [considerazioni sulla virtualizzazione per l'elaborazione](#virtualization-considerations-for-processing).

Questa operazione più complessa è che sono disponibili diverse opzioni di archiviazione che hanno un notevole effetto sulle prestazioni. Come stima sicura durante la migrazione da computer fisico a virtuale, usare un moltiplicatore di 1,10 per adattarsi alle diverse opzioni di archiviazione per i guest virtualizzati in Hyper-V, ad esempio l'archiviazione pass-through, la scheda SCSI o l'IDE. Le modifiche da apportare quando si effettua il trasferimento tra diversi scenari di archiviazione sono irrilevanti per quanto riguarda l'archiviazione locale, SAN, NAS o iSCSI.

#### <a name="calculation-summary-example"></a>Esempio di riepilogo del calcolo

Determinazione della quantità di I/O necessaria per un sistema integro in condizioni operative normali:

- Disco logico ( *\<unità\>del database NTDS*) \ trasferimenti al secondo durante il periodo di picco di 15 minuti 
- Per determinare la quantità di I/O necessaria per l'archiviazione in cui viene superata la capacità dell'archiviazione sottostante:
  >*IOPS necessari* = (disco logico ( *\<unità\>del database NTDS*) \Avg disco/ &divide; &times; *\<\>sec/lettura media sec/lettura disco*) disco logico ( *\< Unità\>di database NTDS*) \ lettura/sec

|Contatore|Value|
|-|-|
|Disco logico effettivo ( *\<\>unità del database NTDS*) \Avg disco/sec|.02 secondi (20 millisecondi)|
|Disco logico di destinazione ( *\<unità\>del database NTDS*) \Avg disco/sec|.01 secondi|
|Moltiplicatore per la modifica nell'IO disponibile|0,02 &divide; 0,01 = 2|  
  
|Nome del valore|Value|
|-|-|
|Disco logico ( *\<unità\>del database NTDS*) \ trasferimenti/sec|400|
|Moltiplicatore per la modifica nell'IO disponibile|2|
|Totale IOPS necessari durante il periodo di picco|800|

Per determinare la frequenza con cui si desidera che la cache venga riscaldata:

- Determinare il tempo massimo accettabile per il riscaldamento della cache. Il tempo necessario per caricare l'intero database dal disco o per gli scenari in cui non è possibile caricare l'intero database nella RAM è il tempo massimo necessario per riempire la RAM.
- Determinare le dimensioni del database, esclusi gli spazi vuoti.  Per ulteriori informazioni, vedere [la pagina relativa alla valutazione per l'archiviazione](#evaluating-for-storage).  
- Dividere le dimensioni del database di 8 KB; si tratta del totale di IOs necessario per caricare il database.
- Dividere il totale di IOs per il numero di secondi nell'intervallo di tempo definito.

Si noti che la frequenza calcolata, con precisione, non sarà esatta perché le pagine caricate in precedenza vengono eliminate se ESE non è configurato per avere una dimensione della cache fissa e per impostazione predefinita in servizi di dominio Active Directory vengono utilizzate dimensioni della cache variabili.

|Punti dati da raccogliere|Valori
|-|-|
|Tempo massimo accettabile per il riscaldamento|10 minuti (600 secondi)
|Dimensioni del database|2 GB|  
  
|Passaggio di calcolo|Formula|Risultato|
|-|-|-|
|Calcolo delle dimensioni del database nelle pagine|(2 GB &times; 1024 &times; 1024) = *dimensioni del database in KB*|2\.097.152 KB|
|Calcolare il numero di pagine nel database|2\.097.152 KB &divide; 8 KB = *numero di pagine*|pagine di 262.144|
|Calcolare i IOPS necessari per il riscaldamento completo della cache|262.144 pagine &divide; 600 secondi = *IOPS necessari*|437 IOPS|

## <a name="processing"></a>Elaborazione

### <a name="evaluating-active-directory-processor-usage"></a>Valutazione dell'utilizzo del processore Active Directory

Per la maggior parte degli ambienti, dopo la corretta ottimizzazione di archiviazione, RAM e rete, come descritto nella sezione pianificazione, la gestione della quantità di capacità di elaborazione sarà il componente che merita la massima attenzione. La valutazione della capacità della CPU necessaria comporta due problemi:

- Indipendentemente dal fatto che le applicazioni nell'ambiente siano ben composte in un'infrastruttura di servizi condivisi, viene illustrato nella sezione intitolata "rilevamento di ricerche costose e inefficienti" nell'articolo creazione di Microsoft Active Applicazioni abilitate alla directory o migrazione da chiamate SAM di livello inferiore a chiamate LDAP.  
  
  Negli ambienti più grandi, il motivo è importante che le applicazioni con codice scarso possono incorrere in una volatilità al carico della CPU, "rubare" una quantità inordinata di tempo della CPU da altre applicazioni, incrementare artificialmente le esigenze di capacità e distribuire in modo non uniforme il carico Controller di dominio.  
- Poiché servizi di dominio Active Directory è un ambiente distribuito con una vasta gamma di potenziali client, la stima delle spese di un "singolo client" è soggetta all'ambiente a causa dei modelli di utilizzo e del tipo o della quantità di applicazioni che sfruttano i servizi di dominio Active Directory. In breve, in modo analogo alla sezione relativa alla rete, per un'applicabilità ampia, questo approccio è più adatto dal punto di vista della valutazione della capacità totale necessaria per l'ambiente.

Per gli ambienti esistenti, dato che il dimensionamento dell'archiviazione è stato discusso in precedenza, il presupposto è che l'archiviazione è ora dimensionata correttamente e pertanto i dati relativi al carico del processore sono validi. Per ripetere l'iterazione, è fondamentale assicurarsi che il collo di bottiglia nel sistema non sia le prestazioni dell'archiviazione. Quando si verifica un collo di bottiglia e il processore è in attesa, sono presenti stati inattivi che scompariranno una volta rimosso il collo di bottiglia.  Poiché gli Stati di attesa del processore vengono rimossi, per definizione, l'utilizzo della CPU aumenta perché non è più necessario attendere i dati. Quindi, raccogliere i contatori delle prestazioni "disco logico ( *\<unità\>del database NTDS*) \Avg letture disco/sec" e "processo\\(LSASS)% tempo processore". I dati in "Process (LSASS)\\% Processor Time" saranno artificialmente bassi se "disco logico ( *\<unità\>di database NTDS*) \Avg letture disco/sec" supera i 10-15 ms, che è una soglia generale utilizzata dal supporto tecnico Microsoft per la risoluzione dei problemi relativi alle prestazioni di archiviazione. Come prima, è consigliabile che gli intervalli di campionamento siano di 15, 30 o 60 minuti. Un valore inferiore sarà in genere troppo volatile per le misurazioni valide; un valore maggiore consiste nel smussare le operazioni di lettura giornaliera.

### <a name="introduction"></a>Introduzione

Per pianificare la pianificazione della capacità per i controller di dominio, la potenza di elaborazione richiede la massima attenzione e comprensione. Quando si dimensionano i sistemi per garantire le massime prestazioni, è sempre presente un componente che rappresenta il collo di bottiglia e in un controller di dominio di dimensioni appropriate questo sarà il processore.

Analogamente alla sezione relativa alla rete in cui la richiesta dell'ambiente viene verificata in base a un sito, è necessario eseguire la stessa operazione per la capacità di calcolo richiesta. A differenza della sezione relativa alla rete, in cui le tecnologie di rete disponibili superano la richiesta normale, prestare maggiore attenzione al dimensionamento della capacità della CPU.  Come qualsiasi ambiente con dimensioni anche moderate; un numero eccessivo di utenti simultanei può comportare un carico significativo sulla CPU.

Sfortunatamente, a causa della grande variabilità delle applicazioni client che sfruttano AD, una stima generale degli utenti per CPU è deplorevolmente inapplicabile a tutti gli ambienti. In particolare, le richieste di calcolo sono soggette al comportamento dell'utente e al profilo dell'applicazione. Ogni ambiente deve pertanto essere ridimensionato singolarmente.

#### <a name="target-site-behavior-profile"></a>Profilo del comportamento del sito di destinazione

Come indicato in precedenza, quando si pianifica la capacità per un intero sito, l'obiettivo è quello di definire un progetto con una progettazione della capacità di *N* + 1, in modo che l'errore di un sistema durante il periodo di picco consenta la continuazione del servizio con un livello di qualità ragionevole. Ciò significa che in uno scenario "*N*", il carico di tutte le caselle dovrebbe essere inferiore al 100% (migliore ancora, minore di 80%) durante i periodi di picco.

Inoltre, se le applicazioni e i client nel sito usano le procedure consigliate per individuare i controller di dominio (ovvero l'uso della [funzione DsGetDcName](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), i client devono essere distribuiti in modo relativamente uniforme con picchi temporanei secondari a causa di qualsiasi numero di fattori.

Nell'esempio seguente vengono eseguiti i presupposti seguenti:

- Ognuno dei cinque controller di dominio nel sito ha quattro CPU.
- L'utilizzo della CPU di destinazione totale durante l'orario di ufficio è del 40% in condizioni operative normali ("*n* + 1") e 60% in caso contrario ("*n*"). Durante le ore non lavorative, l'utilizzo della CPU di destinazione è pari al 80% perché il software di backup e altre operazioni di manutenzione dovrebbero utilizzare tutte le risorse disponibili.

![Grafico utilizzo CPU](media/capacity-planning-considerations-cpu-chart.png)

Analisi dei dati nel grafico (utilità del processore (_ Total)\% per ogni controller di dominio:

- Nella maggior parte dei casi, il carico è distribuito in modo relativamente uniforme, che corrisponde a quello previsto quando i client usano il localizzatore DC e hanno ricerche ben scritte. 
- Il 10% è costituito da un numero di picchi di cinque minuti, con alcune dimensioni pari al 20%. In generale, a meno che non causino il superamento della destinazione del piano di capacità, l'analisi di questi elementi non è utile.  
- Il periodo di picco per tutti i sistemi è compreso tra circa 8:00 AM e 9:15 AM. Con la transizione senza problemi da circa 5:00 a circa 5:00 PM, questo è in genere indicativo del ciclo aziendale. I picchi di utilizzo della CPU più casuali in uno scenario Box-by-box compreso tra 5:00 e 4:00 sono al di fuori dei problemi relativi alla pianificazione della capacità.

  >[!NOTE]
  >In un sistema ben gestito, i picchi possono essere il software di backup in esecuzione, le analisi antivirus complete del sistema, l'inventario hardware o software, la distribuzione di software o patch e così via. Poiché non rientrano nel ciclo aziendale di picco degli utenti, le destinazioni non vengono superate.

- Poiché ogni sistema è pari a circa il 40% e tutti i sistemi hanno lo stesso numero di CPU, in caso di errore o di disconnessione, i sistemi rimanenti vengono eseguiti con un valore stimato del 53% (il carico del 40% del sistema è suddiviso in modo uniforme e viene aggiunto al carico del sistema e del 40% esistente del sistema C). Per diversi motivi, questo presupposto lineare non è perfettamente accurato, ma fornisce un'accuratezza sufficiente per il misuratore.  

  **Scenario alternativo:** Due controller di dominio in esecuzione al 40%: Un controller di dominio ha esito negativo e la CPU stimata sul rimanente è stimata del 80%. Questo limite supera le soglie descritte in precedenza per il piano di capacità e inizia a limitare gravemente la quantità di spazio disponibile per il 10% del 20% nel profilo di carico precedente, il che significa che i picchi dovrebbero guidare il controller di dominio al 90% fino al 100% durante lo scenario "*N*" e de velocità di risposta limitata.

### <a name="calculating-cpu-demands"></a>Calcolo delle richieste della CPU

Il contatore oggetto\\prestazioni "tempo processore processo% processore" somma la quantità totale di tempo impiegato da tutti i thread di un'applicazione sulla CPU e divide in base alla quantità totale di tempo di sistema trascorso. Ciò è dovuto al fatto che un'applicazione multithread in un sistema con più CPU può superare il 100% del tempo di CPU e verrebbe interpretata in modo molto diverso rispetto alla "\\utilità processore% Processor Information". In pratica, il "processo (Lsass\\)% tempo processore" può essere visualizzato come il numero di CPU in esecuzione al 100% necessarie per supportare le richieste del processo. Il valore 200% indica che sono necessarie 2 CPU, ciascuna al 100%, per supportare il caricamento completo di servizi di dominio Active Directory. Anche se una CPU in esecuzione al 100% di capacità è la più conveniente dal punto di vista del denaro speso per le CPU e il consumo di energia e energia, per diversi motivi descritti in Appendice A, una maggiore velocità di risposta in un sistema multithread si verifica quando il sistema è non in esecuzione al 100%.

Per supportare picchi temporanei nel carico del client, è consigliabile usare una CPU con un periodo di picco compreso tra il 40% e il 60% della capacità del sistema. Con l'esempio precedente, ciò significa che tra 3,33 (60% target) e 5 (40% target) CPU è necessario per il caricamento di servizi di dominio Active Directory (processo LSASS). È necessario aggiungere capacità aggiuntiva in base alle esigenze del sistema operativo di base e di altri agenti necessari, ad esempio antivirus, backup, monitoraggio e così via. Sebbene l'impatto degli agenti debba essere valutato in base all'ambiente, è possibile stimare un valore compreso tra 5% e 10% di una singola CPU. Nell'esempio corrente, questo suggerisce che tra le CPU 3,43 (60% target) e 5,1 (40% target) sono necessarie durante i periodi di picco.

Il modo più semplice per eseguire questa operazione consiste nell'utilizzare la visualizzazione "area in pila" in Monitoraggio affidabilità e performance monitor (PerfMon), assicurandosi che tutti i contatori vengano ridimensionati.

Presupposti:

- L'obiettivo consiste nel ridurre il footprint al minor numero possibile di server. Idealmente, un server comporterebbe il carico e un server aggiuntivo aggiunto per la ridondanza (scenario *N* + 1).

![Grafico del tempo del processore per il processo LSASS (su tutti i processori)](media/capacity-planning-considerations-proc-time-chart.png)

Informazioni ottenute dai dati nel grafico (elaborazione (LSASS)\\% tempo processore):

- Il giorno lavorativo inizia a dilagare circa 7:00 e diminuisce alle 5:00.
- Il periodo di picco più occupato è compreso tra 9:30 e 11:00. 
  > [!NOTE]
  > Tutti i dati sulle prestazioni sono cronologici. Il punto dati di picco a 9:15 indica il carico da 9:00 a 9:15.
- Sono presenti picchi prima delle 7:00, che potrebbero indicare un carico da fusi orari diversi o attività di infrastruttura in background, ad esempio i backup. Poiché il picco alle 9:30 AM supera questa attività, non è pertinente.
- Nel sito sono presenti tre controller di dominio.

Al massimo carico, Lsass utilizza circa il 485% di una CPU o 4,85 CPU in esecuzione al 100%. In base alla matematica precedente, questo significa che il sito necessita di circa 12,25 CPU per servizi di dominio Active Directory. Aggiungere i suggerimenti precedenti dal 5% al 10% per i processi in background e questo significa che la sostituzione del server oggi richiede circa 12,30 a 12,35 CPU per supportare lo stesso carico. Una stima ambientale per la crescita è ora necessaria per il factoring.

### <a name="when-to-tune-ldap-weights"></a>Quando ottimizzare i pesi LDAP

Esistono diversi scenari in cui è necessario prendere in considerazione l'ottimizzazione di [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) . Nel contesto della pianificazione della capacità, questa operazione viene eseguita quando il caricamento dell'applicazione o dell'utente non è bilanciato in modo uniforme oppure i sistemi sottostanti non sono bilanciati in modo uniforme in termini di funzionalità. I motivi per evitare la pianificazione della capacità esulano dall'ambito di questo articolo.

Esistono due motivi comuni per ottimizzare i pesi LDAP:

- L'emulatore PDC è un esempio che interessa tutti gli ambienti per i quali il comportamento di caricamento dell'applicazione o dell'utente non viene distribuito in modo uniforme. Poiché determinati strumenti e azioni hanno come destinazione l'emulatore PDC, ad esempio gli strumenti di gestione Criteri di gruppo, i secondi tentativi in caso di errori di autenticazione, creazione di trust e così via, le risorse della CPU nell'emulatore PDC possono essere richieste molto più che altrove in il sito.
  - Questa operazione è utile solo se è presente una differenza evidente nell'utilizzo della CPU per ridurre il carico sull'emulatore PDC e aumentare il carico di altri controller di dominio consente una distribuzione più uniforme del carico.
  - In questo caso, impostare LDAPSrvWeight tra 50 e 75 per l'emulatore PDC.
- Server con conteggi diversi di CPU (e velocità) in un sito.  Si immagini, ad esempio, che ci siano 2 8 Server Core e 1 4-core server.  L'ultimo server ha metà dei processori degli altri due server.  Questo significa che un carico client ben distribuito aumenterà approssimativamente il carico medio della CPU nella casella a quattro core a circa due volte le caselle 8 core.
  - Ad esempio, le caselle 2 8-core verrebbero eseguite al 40% e la casella a quattro core verrebbe eseguita al 80%.
  - Si consideri anche l'effetto della perdita di box 1 8-core in questo scenario, in particolare il fatto che la casella quattro core verrebbe sovraccaricata.

#### <a name="example-1---pdc"></a>Esempio 1: PDC

| |Utilizzo con impostazioni predefinite|Nuovo LdapSrvWeight|Nuovo utilizzo stimato|
|-|-|-|-|
|DC 1 (emulatore PDC)|53%|57|40%|
|CONTROLLER DI DOMINIO 2|33%|100|40%|
|DC 3|33%|100|40%|

Il problema è che se il ruolo emulatore PDC viene trasferito o sequestrato, in particolare a un altro controller di dominio nel sito, si verifica un notevole aumento del nuovo emulatore PDC.

Utilizzando l'esempio del profilo di [comportamento del sito di destinazione](#target-site-behavior-profile), si presuppone che tutti e tre i controller di dominio nel sito dispongano di quattro CPU. Cosa dovrebbe accadere, in condizioni normali, se uno dei controller di dominio aveva otto CPU? Sono presenti due controller di dominio con utilizzo del 40% e uno con utilizzo del 20%. Anche se non si tratta di un problema, è possibile bilanciare meglio il carico. Per ottenere questo risultato, sfruttare i pesi LDAP.  Uno scenario di esempio è:

#### <a name="example-2---differing-cpu-counts"></a>Esempio 2: conteggi diversi della CPU

| |Utilità processore\\informazioni %processore(_totale&nbsp;)<br />Utilizzo con impostazioni predefinite|Nuovo LdapSrvWeight|Nuovo utilizzo stimato|
|-|-|-|-|
|4-DC CPU 1|40|100|30|
|4-DC CPU 2|40|100|30|
|8-CPU DC 3|20|200|30|

Tuttavia, prestare attenzione a questi scenari. Come si può notare in precedenza, i calcoli matematici sembrano molto interessanti e graziosi su carta. Tuttavia, in questo articolo, la pianificazione di uno scenario "*N* + 1" è di importanza fondamentale. L'effetto di un controller di dominio in modalità offline deve essere calcolato per ogni scenario. Nello scenario immediatamente precedente, in cui la distribuzione del carico è pari, per garantire un carico del 60% durante uno scenario "*N*", con il carico bilanciato in modo uniforme tra tutti i server, la distribuzione sarà ottimale perché i rapporti rimarranno coerenti. Esaminando lo scenario di ottimizzazione dell'emulatore PDC e in generale qualsiasi scenario in cui il carico di utenti o applicazioni non è bilanciato, l'effetto è molto diverso:

| |Utilizzo ottimizzato|Nuovo LdapSrvWeight|Nuovo utilizzo stimato|
|-|-|-|-|
|DC 1 (emulatore PDC)|40%|85|47%|
|CONTROLLER DI DOMINIO 2|40%|100|53%|
|DC 3|40%|100|53%|

### <a name="virtualization-considerations-for-processing"></a>Considerazioni sulla virtualizzazione per l'elaborazione

Ci sono due livelli di pianificazione della capacità che devono essere eseguiti in un ambiente virtualizzato. A livello di host, in modo analogo all'identificazione del ciclo aziendale descritto in precedenza per l'elaborazione del controller di dominio, è necessario identificare le soglie durante il periodo di picco. Poiché le entità sottostanti sono le stesse per un computer host che pianifica i thread Guest sulla CPU come per ottenere i thread di servizi di dominio Active Directory sulla CPU in un computer fisico, è consigliabile usare lo stesso obiettivo del 40% al 60% nell'host sottostante. Al livello successivo, il livello guest, dal momento che le entità della pianificazione dei thread non sono state modificate, l'obiettivo all'interno del Guest rimane nell'intervallo da 40% a 60%.

In uno scenario con mapping diretto, un Guest per host, è necessario aggiungere tutte le pianificazioni di capacità a questo punto ai requisiti (RAM, disco, rete) del sistema operativo host sottostante. In uno scenario di host condiviso, il test indica che è presente un effetto del 10% sull'efficienza dei processori sottostanti. Ciò significa che se un sito richiede 10 CPU a una destinazione pari al 40%, la quantità consigliata di CPU virtuali da allocare tra tutti gli "*N*" Guest sarà 11. In un sito con una distribuzione mista di server fisici e server virtuali, il modificatore si applica solo alle macchine virtuali. Se ad esempio un sito presenta uno scenario "*N* + 1", un server con mapping fisico o diretto con 10 CPU equivale a un guest con 11 CPU in un host, con 11 CPU riservate per il controller di dominio.

Durante l'analisi e il calcolo delle quantità di CPU necessarie per supportare il carico di servizi di dominio Active Directory, il numero di CPU che eseguono il mapping a ciò che è possibile acquistare in termini di hardware fisico non è necessariamente mappato in modo corretto. La virtualizzazione elimina la necessità di eseguire l'arrotondamento. La virtualizzazione riduce lo sforzo necessario per aggiungere capacità di calcolo a un sito, data la facilità con cui è possibile aggiungere una CPU a una macchina virtuale. Non elimina la necessità di valutare accuratamente la potenza di calcolo necessaria in modo che l'hardware sottostante sia disponibile quando è necessario aggiungere altre CPU ai guest.  Come sempre, ricordarsi di pianificare e monitorare la crescita della domanda.

### <a name="calculation-summary-example"></a>Esempio di riepilogo del calcolo

|Sistema|Picco CPU|
|-|-|-|
|CONTROLLER DI DOMINIO 1|120%|
|CONTROLLER DI DOMINIO 2|147%|
|DC 3|218%|
|CPU totale utilizzata|485%|  
  
|Conteggio sistema/i di destinazione|Larghezza di banda totale (precedente)|
|-|-|
|CPU necessarie al 40% di destinazione|4,85 &divide; . 4 = 12,25|

Ripetuta a causa dell'importanza di questo punto, *ricordarsi di pianificare la crescita*. Supponendo che la crescita del 50% nei tre anni successivi, questo ambiente necessiterà di &times; 18,375 CPU (12,25 1,5) al contrassegno di tre anni. Un piano alternativo può essere esaminato dopo il primo anno e aggiungere capacità aggiuntiva in base alle esigenze.

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>Carico di autenticazione client tra trust per NTLM

#### <a name="evaluating-cross-trust-client-authentication-load"></a>Valutazione del carico di autenticazione client tra trust

Molti ambienti possono avere uno o più domini connessi da un trust. Una richiesta di autenticazione per un'identità in un altro dominio che non utilizza l'autenticazione Kerberos deve attraversare una relazione di trust utilizzando il canale sicuro del controller di dominio a un altro controller di dominio nel dominio di destinazione o nel dominio successivo nel percorso nel dominio di destinazione. Il numero di chiamate simultanee che usano il canale sicuro che un controller di dominio può apportare a un controller di dominio in un dominio trusted è controllato da un'impostazione nota come **MaxConcurrentApi**. Per i controller di dominio, verificare che il canale protetto sia in grado di gestire la quantità di carico eseguita da uno dei due approcci seguenti: ottimizzazione di **MaxConcurrentApi** o, all'interno di una foresta, creazione di trust di collegamento. Per misurare il volume di traffico attraverso un singolo Trust, fare riferimento a [come eseguire l'ottimizzazione delle prestazioni per l'autenticazione NTLM tramite l'impostazione MaxConcurrentApi](https://support.microsoft.com/kb/2688798).

Durante la raccolta dei dati, questo, come per tutti gli altri scenari, deve essere raccolto durante i periodi di picco del giorno in cui i dati risultano utili.

> [!NOTE]
> Gli scenari di interforesta e tra foreste possono causare l'attraversamento di più trust da parte dell'autenticazione ed è necessario ottimizzare ogni fase.

#### <a name="planning"></a>Pianificazione

Per impostazione predefinita, esistono diverse applicazioni che utilizzano l'autenticazione NTLM oppure possono essere utilizzate in uno scenario di configurazione specifico. I server applicazioni aumentano la capacità e il servizio aumentando il numero di client attivi. C'è anche una tendenza che i client mantengono aperte le sessioni per un periodo di tempo limitato e si riconnettono a intervalli regolari, ad esempio la sincronizzazione pull della posta elettronica. Un altro esempio comune di carico NTLM elevato è il server proxy Web che richiede l'autenticazione per l'accesso a Internet.

Queste applicazioni possono causare un carico significativo per l'autenticazione NTLM, che può influire significativamente sul controller di dominio, specialmente quando gli utenti e le risorse si trovano in domini diversi.

Esistono diversi approcci alla gestione del carico di attendibilità incrociato, che in pratica vengono utilizzati in combinazione anziché in uno scenario esclusivo o/o. Le opzioni possibili sono:

- Ridurre l'autenticazione client tra attendibilità individuando i servizi utilizzati da un utente nello stesso dominio in cui risiede l'utente.
- Aumentare il numero di canali protetti disponibili. Questo è rilevante per il traffico tra foreste e tra foreste e sono noti come trust di collegamento.
- Ottimizzare le impostazioni predefinite per **MaxConcurrentApi**.

Per l'ottimizzazione di **MaxConcurrentApi** in un server esistente, l'equazione è la seguente:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires*  +  *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

Per ulteriori informazioni, vedere [l'articolo della Knowledge 2688798: Come eseguire l'ottimizzazione delle prestazioni per l'autenticazione NTLM tramite l'impostazione](http://support.microsoft.com/kb/2688798)MaxConcurrentApi.

## <a name="virtualization-considerations"></a>Considerazioni sulla virtualizzazione

None, si tratta di un'impostazione di ottimizzazione del sistema operativo.

### <a name="calculation-summary-example"></a>Esempio di riepilogo del calcolo

|Tipo di dati|Value|
|-|-|
|Il semaforo acquisisce (minimo)|6\.161|
|Il semaforo acquisisce (massimo)|6\.762|
|Timeout del semaforo|0|
|Tempo di attesa medio del semaforo|0,012|
|Durata raccolta (secondi)|1:11 minuti (71 secondi)|
|Formula (da KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0,012/|
|Valore minimo per **MaxConcurrentApi**|((6762 &ndash; 6161) + 0) &times; 0,012 &divide; 71 =. 101|

Per questo sistema per questo periodo di tempo, i valori predefiniti sono accettabili.

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>Monitoraggio della conformità agli obiettivi della pianificazione della capacità

In questo articolo è stato illustrato che la pianificazione e la scalabilità vanno verso le destinazioni di utilizzo. Di seguito è riportato un grafico di riepilogo delle soglie consigliate che è necessario monitorare per assicurarsi che i sistemi funzionino entro le soglie di capacità appropriate. Tenere presente che non si tratta di soglie per le prestazioni, ma le soglie per la pianificazione della capacità. Un server che opera in eccesso di queste soglie funziona, ma è il momento di iniziare a convalidare il comportamento di tutte le applicazioni. In caso affermativo, è il momento di iniziare a valutare gli aggiornamenti hardware o altre modifiche di configurazione.

|Category|Contatore delle prestazioni|Intervallo/campionamento|Destinazione|Avviso|
|-|-|-|-|-|
|Processore|Informazioni sul processore (_\\Total)% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 o versioni precedenti)|Memoria\mbyte MB|< 100 MB|N/D|< 100 MB|
|RAM (Windows Server 2012)|Durata media cache standby Memory\Long-Term|30 minuti|Deve essere testato|Deve essere testato|
|Rete|Interfaccia di rete\*() \Byte inviati/sec<br /><br />Interfaccia di rete\*() \Byte ricevuti/sec|30 minuti|40%|60%|
|Archiviazione|Disco logico ( *\<unità\>del database NTDS*) \Avg letture disco/sec<br /><br />Disco logico ( *\<unità\>del database NTDS*) \Avg scritture disco/sec|60 min|10 ms|15 ms|
|Servizi AD|Netlogon (\*) \Latenza tempo di attesa del semaforo|60 min|0|1 secondo|

## <a name="appendix-a-cpu-sizing-criteria"></a>Appendice A: Criteri di ridimensionamento CPU

### <a name="definitions"></a>Definizioni

**Processore (microprocessore):** componente che legge ed esegue le istruzioni di programma  

**CPU:** Unità di elaborazione centrale  

**Processore multicore:** più CPU nello stesso circuito integrato  

**CPU** multipla: più CPU, non nello stesso circuito integrato  

**Processore logico:** un motore di calcolo logico dal punto di vista del sistema operativo  

Sono inclusi Hyper-Threading, un core nel processore multicore o un processore a core singolo.  

Poiché i sistemi server moderni hanno più processori, più processori multicore e Hyper-Threading, queste informazioni sono generalizzate per coprire entrambi gli scenari. Il termine processore logico verrà quindi utilizzato per rappresentare il sistema operativo e la prospettiva dell'applicazione dei motori di elaborazione disponibili.

### <a name="thread-level-parallelism"></a>Parallelismo a livello di thread

Ogni thread è un'attività indipendente, perché ogni thread ha un proprio stack e istruzioni. Poiché servizi di dominio Active Directory è multithread e il numero di thread disponibili può essere ottimizzato usando la [modalità di visualizzazione e impostazione dei criteri LDAP in Active Directory tramite Ntdsutil. exe](http://support.microsoft.com/kb/315071), la scalabilità è ottimale in più processori logici.

### <a name="data-level-parallelism"></a>Parallelismo a livello di dati

Questa operazione comporta la condivisione dei dati tra più thread all'interno di un processo (solo nel caso del processo di servizi di dominio Active Directory) e in più thread in più processi (in generale). Per quanto riguarda l'eccessiva semplificazione del caso, significa che tutte le modifiche ai dati vengono riflesse in tutti i thread in esecuzione in tutti i diversi livelli di cache (L1, L2, L3) in tutti i core che eseguono i thread, nonché l'aggiornamento della memoria condivisa. Le prestazioni possono peggiorare durante le operazioni di scrittura, mentre tutte le varie posizioni di memoria sono coerenti prima che l'elaborazione delle istruzioni possa continuare.

### <a name="cpu-speed-vs-multiple-core-considerations"></a>Confronto tra la velocità della CPU e le considerazioni su più core

La regola empirica generale è costituita da processori logici più veloci che riducono la durata necessaria per elaborare una serie di istruzioni, mentre un numero maggiore di processori logici significa che è possibile eseguire più attività contemporaneamente. Queste regole vengono suddivise perché gli scenari diventano intrinsecamente più complessi con considerazioni sul recupero dei dati dalla memoria condivisa, in attesa del parallelismo a livello di dati e dal sovraccarico della gestione di più thread. Questo è anche il motivo per cui la scalabilità nei sistemi multicore non è lineare.

In queste considerazioni si considerino le analogie seguenti: pensare a un'autostrada, in cui ogni thread è costituito da una singola macchina, ogni corsia è un core e il limite di velocità è la velocità di clock.

1. Se è presente una sola auto nell'autostrada, non è importante se sono presenti due corsie o 12 corsie. Questa vettura verrà rilasciata solo con la massima rapidità che il limite di velocità consentirà.
1. Si supponga che i dati necessari per il thread non siano immediatamente disponibili. L'analogia è che un segmento della strada viene arrestato. Se è presente una sola auto nell'autostrada, non importa quale sia il limite di velocità fino alla riapertura della corsia (i dati vengono recuperati dalla memoria).
1. Man mano che aumenta il numero di automobili, aumenta il sovraccarico per gestire il numero di automobili. Confrontare l'esperienza di guida e la quantità di attenzione necessaria quando la strada è praticamente vuota (ad esempio, la sera tardi) rispetto a quando il traffico è intenso (ad esempio, a metà del pomeriggio, ma non all'ora di punta). Inoltre, si consideri la quantità di attenzione necessaria per la guida su un'autostrada a due corsia, in cui è presente solo un'altra corsia in cui si preoccupano le attività dei driver, rispetto a un'autostrada a sei corsia, dove è necessario preoccuparsi degli altri driver.
   > [!NOTE]
   > L'analogia sullo scenario relativo all'ora di Rush viene estesa nella sezione successiva: Tempo di risposta/il modo in cui la disponibilità del sistema influisca sulle prestazioni.

Di conseguenza, le specifiche relative a processori più o più veloci diventano estremamente soggettive per il comportamento dell'applicazione, che nel caso di servizi di dominio Active Directory è molto specifica dell'ambiente e persino varia da server a server all'interno di un ambiente. Questo è il motivo per cui i riferimenti precedenti nell'articolo non investono molto in modo eccessivamente accurato e nei calcoli viene incluso un margine di sicurezza. Quando si prendono decisioni di acquisto basate sul budget, è consigliabile eseguire prima di tutto l'ottimizzazione dell'utilizzo dei processori al 40% (o il numero desiderato per l'ambiente) prima di valutare l'acquisto di processori più veloci. La maggiore sincronizzazione tra più processori riduce il vero vantaggio di un maggior numero di processori dalla progressione lineare&times; (2 il numero di processori fornisce una&times; potenza di calcolo aggiuntiva inferiore a 2).

> [!NOTE]
> La legge di Amdahl e la legge di Gustafson sono i concetti pertinenti.

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>Tempo di risposta/come la disponibilità del sistema influisca sulle prestazioni

La teoria dell'accodamento è lo studio matematico delle righe in attesa (code). Nella teoria di Accodamento, la legge di utilizzo è rappresentata dall'equazione:

*U* k = *B* &divide; *T*

Dove *U* k è la percentuale di utilizzo, *B* è la quantità di tempo occupata e *T* è il tempo totale osservato dal sistema. Tradotta nel contesto di Windows, ciò significa che il numero di thread di intervallo 100-nanosecondi (NS) in uno stato di esecuzione è diviso per il numero di intervalli 100-ns disponibili nell'intervallo di tempo specificato. Si tratta esattamente della formula per il calcolo di% Processor Utility ( [oggetto processore](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) di riferimento e [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

La teoria di Accodamento fornisce anche la formula: *N* = *u* k &divide; (1 &ndash; *u* k) per stimare il numero di elementi in attesa in base all'utilizzo (*n* è la lunghezza della coda). La creazione di grafici in base a tutti gli intervalli di utilizzo fornisce le stime seguenti per quanto tempo la coda da ottenere nel processore è a qualsiasi carico di CPU.

![Lunghezza coda](media/capacity-planning-considerations-queue-length.png)

Si osserva che dopo il carico della CPU del 50%, in media è sempre presente un'attesa di un altro elemento nella coda, con un incremento notevolmente rapido dopo circa il 70% di utilizzo della CPU.

Tornando all'analogia di guida usata in precedenza in questa sezione:

- I tempi di attività di "mid-afternoon", ipoteticamente, rientrano nell'intervallo compreso tra 40 e 70%. Il traffico è sufficiente, in modo tale che la possibilità di scegliere una corsia non sia stata soggetta a restrizioni e che la probabilità di un altro driver sia nel modo più elevato, non richiede il livello di sforzo necessario per "trovare" un gap sicuro tra le altre automobili in viaggio.
- Si noterà che mentre il traffico si avvicina all'ora di punta, il sistema stradale si avvicina alla capacità del 100%. La modifica delle corsie può diventare molto difficile perché le automobili sono così vicine, che devono essere sottoposti a una maggiore attenzione.

Questo è il motivo per cui le medie a lungo termine per la capacità stimate in modo prudenziale al 40% consentono una stanza principale per i picchi anomali del carico, a prescindere dal fatto che i picchi transitori (ad esempio le query hardcoded eseguite per alcuni minuti) o i picchi anomali nel carico generale (la mattina di il primo giorno dopo un lungo weekend.

L'istruzione precedente considera la percentuale di calcolo del tempo del processore identica alla normativa di utilizzo è una semplificazione della semplicità del lettore generale. Per quelli più rigorosamente matematici:  
- Conversione del [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = il numero di intervalli di 100-NS per il thread "inattivo" trascorre sul processore logico. Modifica della variabile "*X*" nel calcolo [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *T* = numero totale di intervalli di 100-NS in un intervallo di tempo specificato. Modifica della variabile "*Y*" nel calcolo [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) .
  - *U* k = percentuale di utilizzo del processore logico per "thread inattivo" o% tempo di inattività.  
- Uso della matematica:
  - *U* k = 1 –% tempo processore
  - % Tempo processore = 1 – *U* k
  - % Tempo processore = 1 – *B* / *T*
  - % Tempo processore = 1 – *X1* – *x0* / *Y1* – *y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>Applicazione dei concetti alla pianificazione della capacità

La matematica precedente può determinare il numero di processori logici necessari in un sistema sembra estremamente complesso. Questo è il motivo per cui l'approccio al dimensionamento dei sistemi è mirato alla determinazione dell'utilizzo massimo della destinazione in base al carico corrente e al calcolo del numero di processori logici necessari per ottenere tale valore. Inoltre, anche se le velocità del processore logico avranno un impatto significativo sulle prestazioni, l'efficienza della cache, i requisiti di coerenza della memoria, la pianificazione e la sincronizzazione dei thread e il carico del client non bilanciato avrà un impatto significativo sui prestazioni che variano in base al server. Con il costo relativamente economico della potenza di calcolo, il tentativo di analizzare e determinare il numero perfetto di CPU necessarie diventa un esercizio accademico superiore a quello che fornisce valore aziendale.

Il 40% non è un requisito difficile e rapido. si tratta di un inizio ragionevole. Diversi consumer di Active Directory richiedono diversi livelli di velocità di risposta. Potrebbero verificarsi scenari in cui gli ambienti possono essere eseguiti al 80% o al 90% come una media sostenuta, in quanto i tempi di attesa maggiori per l'accesso al processore non influiscano in modo significativo sulle prestazioni del client. È importante ripetere l'iterazione che nel sistema sono presenti molte aree molto più lente rispetto al processore logico nel sistema, incluso l'accesso alla RAM, l'accesso al disco e la trasmissione della risposta sulla rete. Tutti questi elementi devono essere ottimizzati insieme. Esempi:

- L'aggiunta di più processori a un sistema che esegue il 90% che è associato a un disco probabilmente non consentirà di migliorare significativamente le prestazioni. Un'analisi più approfondita del sistema probabilmente identificherà che ci sono molti thread che non sono ancora in esecuzione nel processore perché sono in attesa del completamento di operazioni di I/O.
- La risoluzione dei problemi legati al disco implica potenzialmente che i thread che in precedenza impiegavano molto tempo in uno stato di attesa non saranno più in uno stato di attesa per I/O e vi sarà maggiore concorrenza per il tempo di CPU, ovvero l'utilizzo del 90% nei precedenti L'esempio passerà al 100% (perché non può andare più in alto). Entrambi i componenti devono essere ottimizzati in combinazione.
  > [!NOTE]
  > Le informazioni sul processore (\\*)% l'utilità processore può superare il 100% con sistemi con modalità "Turbo".  Questo è il punto in cui la CPU supera la velocità di elaborazione nominale per brevi periodi.  Documentazione di riferimento per i produttori della CPU e descrizione del contatore per informazioni più dettagliate.  

La discussione di considerazioni sull'utilizzo completo del sistema introduce anche i controller di dominio di conversazione come guest virtualizzati. Il [tempo di risposta/il modo in cui la disponibilità del sistema influisca sulle prestazioni](#response-timehow-the-system-busyness-impacts-performance) si applica sia all'host che al Guest in uno scenario virtualizzato. Questo è il motivo per cui in un host con un solo Guest, un controller di dominio (e in genere qualsiasi sistema) ha quasi le stesse prestazioni dell'hardware fisico. L'aggiunta di altri utenti Guest agli host aumenta l'utilizzo dell'host sottostante, aumentando in tal modo i tempi di attesa per ottenere l'accesso ai processori, come illustrato in precedenza. In breve, l'utilizzo del processore logico deve essere gestito sia nell'host che nei livelli Guest.

Estendendo le analogie precedenti, lasciando l'autostrada come hardware fisico, la macchina virtuale guest verrà analoga a un bus (un bus Express che passa direttamente alla destinazione che il rider vuole). Immaginate i quattro scenari seguenti:

- È disattivata, un rider si trova su un bus quasi vuoto e il bus si trova su una strada anche quasi vuota. Poiché non c'è traffico da sostenere, il rider ha una semplice corsa e raggiunge la stessa velocità del pilota. I tempi di viaggio del Rider sono comunque limitati dal limite di velocità.
- È fuori orario, quindi il bus è quasi vuoto, ma la maggior parte delle corsie sulla strada sono chiuse, quindi l'autostrada è ancora congestionata. Il rider si trova su un bus quasi vuoto su una strada congestionata. Sebbene il rider non disponga di una grande concorrenza nel bus per la posizione in cui si trovano, il tempo totale di viaggio è comunque determinato dal resto del traffico al di fuori di.
- Si tratta di un'ora di punta, in modo che l'autostrada e il bus siano congestionati. Non solo il viaggio dura più a lungo, ma il bus è un incubo perché gli utenti sono spalla a spalla e l'autostrada non è molto migliore. L'aggiunta di più bus (processori logici al Guest) non significa che possono adattarsi alla strada più facilmente o che la corsa verrà abbreviata.
- Lo scenario finale, sebbene possa allungare l'analogia, è il punto in cui il bus è pieno, ma la strada non è congestionata. Mentre il rider continuerà a raggiungere e spegnere il bus, il viaggio sarà efficace dopo che il bus è in viaggio. Questo è l'unico scenario in cui l'aggiunta di altri bus (processori logici al Guest) consente di migliorare le prestazioni del Guest.

Da questa posizione è relativamente semplice estrapolare un numero di scenari compreso tra lo 0% e lo stato utilizzato dal 100% e lo stato utilizzato dallo 0% e 100% del bus con diversi gradi di influenza......

L'applicazione delle entità di livello superiore alla CPU del 40% come destinazione ragionevole per l'host e il Guest è un inizio ragionevole per lo stesso ragionamento precedente, ovvero la quantità di Accodamento.

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>Appendice B: Considerazioni sulle diverse velocità del processore e sull'effetto del risparmio energia del processore sulle velocità del processore

In tutte le sezioni della selezione del processore, il presupposto è che il processore venga eseguito al 100% della velocità di clock al momento della raccolta dei dati e che i sistemi di sostituzione avranno gli stessi processori di velocità. Sebbene entrambe le ipotesi siano false, in particolare con Windows Server 2008 R2 e versioni successive, in cui la combinazione per il risparmio di energia predefinita è **bilanciata**, la metodologia è ancora considerata l'approccio conservativo. Sebbene il potenziale tasso di errore possa aumentare, aumenta solo il margine di sicurezza Man mano che aumentano le velocità del processore.

- Ad esempio, in uno scenario in cui sono richieste 11,25 CPU, se i processori venivano eseguiti a metà velocità al momento della raccolta dei dati, la stima più accurata &divide; potrebbe essere 5,125 2.
- È impossibile garantire che la doppia velocità di clock raddoppi la quantità di elaborazione che si verifica per un determinato periodo di tempo. Questo è dovuto al fatto che la quantità di tempo impiegato dai processori in attesa su RAM o altri componenti di sistema potrebbe rimanere invariata. Il risultato finale è che i processori più veloci potrebbero dedicare una percentuale maggiore del tempo di inattività durante l'attesa del recupero dei dati. Anche in questo caso, si consiglia di usare il denominatore comune più basso, di essere conservatore ed evitare di provare a calcolare un livello di accuratezza potenzialmente falsa, supponendo un confronto lineare tra le velocità del processore.

In alternativa, se le velocità del processore nell'hardware di sostituzione sono inferiori all'hardware corrente, è possibile aumentare la stima dei processori necessari per un importo proporzionale. Viene calcolato, ad esempio, che sono necessari 10 processori per sostenere il carico in un sito e che i processori correnti sono in esecuzione a 3,3 GHz e i processori di sostituzione verranno eseguiti a 2,6 GHz. si tratta di una riduzione della velocità del 21%. In questo caso, 12 processori sarebbero la quantità consigliata.

Ciò premesso, questa variabilità non comporta la modifica delle destinazioni di utilizzo del processore di gestione della capacità. Poiché le velocità di clock del processore verranno modificate in modo dinamico in base al carico richiesto, l'esecuzione del sistema con carichi più elevati genererà uno scenario in cui la CPU impiega più tempo in uno stato di velocità di clock superiore, in modo che l'obiettivo finale sia al 40% di utilizzo in un 100% stato della velocità di clock al picco. Un valore inferiore a quello che genera risparmio energia in quanto le velocità della CPU saranno limitate durante gli scenari di picco.

> [!NOTE]
> Un'opzione consiste nel disattivare il risparmio energia sui processori (impostando la combinazione per il risparmio di energia su **prestazioni elevate**) durante la raccolta dei dati. Questo darebbe una rappresentazione più accurata dell'utilizzo della CPU nel server di destinazione.

Per regolare le stime per i diversi processori, è stato usato per la sicurezza, esclusi gli altri colli di bottiglia del sistema indicati in precedenza, per presupporre che il raddoppio della velocità del processore raddoppiasse la quantità di elaborazione che poteva essere eseguita.  Attualmente, l'architettura interna dei processori è molto diversa tra i processori, che un modo più sicuro per misurare gli effetti dell'utilizzo di processori diversi rispetto ai dati provenienti da è quello di sfruttare il benchmark SPECint_rate2006 dalla valutazione delle prestazioni standard Corporation.

1. Trovare i punteggi di SPECint_rate2006 per il processore in uso e il piano da usare.
    1. Nel sito Web di standard Performance Evaluation Corporation selezionare **risultati**, evidenziare **CPU2006** e selezionare **Cerca tutti i risultati di SPECint_rate2006**.
    1. In **richiesta semplice** immettere i criteri di ricerca per il processore di destinazione, ad esempio il **processore corrisponde a E5-2630 (baselinetarget)** e il **processore corrisponde a E5-2650 (baseline)** .
    1. Trovare la configurazione del server e del processore da usare (o qualcosa di simile, se non è disponibile una corrispondenza esatta) e prendere nota del valore nelle colonne **result** e **# Cores** .
1. Per determinare se il modificatore usa l'equazione seguente:
   >((*Valore score della piattaforma di destinazione per core* &times; ) *(MHz per core della piattaforma di base*)) &divide; ((*valore di base per punteggio per core*) &times; (*MHz per core della piattaforma di destinazione*)  

    Usando l'esempio precedente:
   >(35,83 &times; 2000) &divide; (33,75 &times; 2300) = 0,92
1. Moltiplicare il numero stimato di processori per il modificatore.  Nel caso precedente, per passare dal processore E5-2650 al processore E5-2630, moltiplicare i processori calcolati &times; 11,25 CPU 0,92 = 10,35 necessari.

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>Appendice C: Nozioni fondamentali sul sistema operativo che interagiscono con l'archiviazione

I concetti di teoria di Accodamento delineati in [tempi di risposta/come la disponibilità del sistema influisca sulle prestazioni](#response-timehow-the-system-busyness-impacts-performance) sono applicabili anche all'archiviazione. Avere una certa familiarità con il modo in cui il sistema operativo gestisce l'I/O è necessario per applicare questi concetti. Nel sistema operativo Microsoft Windows, viene creata una coda per il mantenimento delle richieste di I/O per ogni disco fisico. Tuttavia, deve essere creato un chiarimento sul disco fisico. I controller di matrice e i SANs presentano aggregazioni di assi al sistema operativo come singoli dischi fisici. Inoltre, i controller di matrice e i San possono aggregare più dischi in un unico set di matrici e quindi suddividere il set di matrici in più "partizioni", che a sua volta vengono presentate al sistema operativo come più dischi fisici (Ref. figure).

![Blocca assi](media/capacity-planning-considerations-block-spindles.png)  

In questa figura i due assi vengono sottoposti a mirroring e suddivisi in aree logiche per l'archiviazione dei dati (data 1 e data 2). Queste aree logiche vengono visualizzate dal sistema operativo come dischi fisici distinti.

Sebbene ciò possa essere molto confuso, in questa appendice viene utilizzata la terminologia seguente per identificare le diverse entità:

- **Mandrino:** il dispositivo fisicamente installato nel server.
- **Array:** raccolta di assi aggregati per controller.
- **Partizione di matrice:** partizionamento della matrice aggregata
- **Lun:** matrice usata per fare riferimento a sans
- **Disco-** Il sistema operativo osservato come un singolo disco fisico.
- **Partition:** partizionamento logico di ciò che il sistema operativo percepisce come un disco fisico.

### <a name="operating-system-architecture-considerations"></a>Considerazioni sull'architettura del sistema operativo

Il sistema operativo crea una coda di I/O FIFO (First in/First out) per ogni disco osservato; Questo disco potrebbe rappresentare un mandrino, una matrice o una partizione di matrice. Dal punto di vista del sistema operativo, per quanto riguarda la gestione delle operazioni di I/O, le code più attive sono migliori. Quando viene serializzata una coda FIFO, significa che tutti i/o emessi nel sottosistema di archiviazione devono essere elaborati nell'ordine in cui è arrivata la richiesta. Correlando ogni disco rilevato dal sistema operativo con un mandrino/Array, il sistema operativo gestisce ora una coda di I/O per ogni set di dischi univoco, eliminando così la contesa per le risorse di I/o limitate tra dischi e isolando la richiesta di I/O a una singola disco. Come eccezione, Windows Server 2008 introduce il concetto di assegnazione di priorità di I/O e le applicazioni progettate per usare la priorità "bassa" rientrino in questo ordine normale e riprendono il posto. Le applicazioni non codificate in modo specifico per sfruttare la priorità "bassa" per impostazione predefinita "normale".

### <a name="introducing-simple-storage-subsystems"></a>Introduzione ai sottosistemi di archiviazione semplici

A partire da un semplice esempio (un singolo disco rigido all'interno di un computer) verrà assegnata un'analisi componente per componente. Suddividendo questa attività nei principali componenti del sottosistema di archiviazione, il sistema è costituito da:

- **1 –** 10.000 rpm Ultra Fast SCSI HD (Ultra Fast SCSI ha una velocità di trasferimento di 20 MB/s)
- **1:** Bus SCSI (cavo)
- **1:** Scheda SCSI Ultra veloce
- **1 –** bus PCI da 33 MHz a 32 bit

Una volta identificati i componenti, è possibile calcolare la quantità di dati che possono transitare dal sistema o la quantità di operazioni di I/O che è possibile gestire. Si noti che la quantità di I/O e la quantità di dati che è possibile transitare nel sistema sono correlati, ma non uguali. Questa correlazione dipende dal fatto che l'I/O del disco sia casuale o sequenziale e che le dimensioni del blocco. Tutti i dati vengono scritti nel disco come un blocco, ma applicazioni diverse che usano dimensioni dei blocchi diverse. In base a componenti:

- **Disco rigido:** Il disco rigido 10.000-RPM medio ha un tempo di ricerca di 7 millisecondi e un tempo di accesso di 3 ms. Il tempo di ricerca è la quantità media di tempo impiegato dall'intestazione di lettura/scrittura per spostarsi in una posizione sul piatto. Tempo di accesso è la quantità media di tempo impiegato per leggere o scrivere i dati su disco, una volta che la testa si trova nella posizione corretta. Il tempo medio per la lettura di un blocco di dati univoco in un disco a 10.000 RPM costituisce pertanto una ricerca e un accesso, per un totale di circa 10 ms (o .010 secondi) per ogni blocco di dati.

  Quando ogni accesso al disco richiede lo spostamento dell'intestazione in una nuova posizione sul disco, il comportamento di lettura/scrittura viene definito "casuale". Pertanto, quando tutte le operazioni di I/O sono casuali, un disco a 10.000 RPM può gestire circa 100 I/O al secondo (IOPS) (la formula è 1000 ms al secondo diviso 10 ms per I/O o 1000/10 = 100 IOPS).

  In alternativa, quando si verificano tutte le I/O dai settori adiacenti nell'HD, questa operazione viene definita I/O sequenziale. L'I/O sequenziale non ha tempo di ricerca perché, quando il primo I/O è completo, la testa di lettura/scrittura si trova all'inizio della posizione in cui il blocco di dati successivo viene archiviato nell'HD. Un HD a 10.000 RPM è quindi in grado di gestire circa 333 operazioni di I/O al secondo (1000 ms al secondo divise per 3 ms per I/O).

  >[!NOTE]
  >Questo esempio non riflette la cache del disco, in cui vengono in genere conservati i dati di un cilindro. In questo caso, i 10 MS sono necessari per la prima operazione di I/O e il disco legge l'intero cilindro. Tutte le altre le I/O sequenziali sono soddisfatte dalla cache. Di conseguenza, le cache in disco potrebbero migliorare le prestazioni di I/O sequenziali.
  
  Fino a questo punto, la velocità di trasferimento del disco rigido è stata irrilevante. Se il disco rigido è di 20 MB/s Ultra-Wide o Ultra3 160 MB/s, la quantità effettiva di IOPS può essere gestita da 10.000-RPM HD è ~ 100 casuale o ~ 300 I/O sequenziali. Quando le dimensioni dei blocchi cambiano in base all'applicazione che scrive nell'unità, la quantità di dati di cui è stato eseguito il pull per I/O è diversa. Se, ad esempio, le dimensioni del blocco sono pari a 8 KB, le operazioni di I/O 100 leggeranno o scriveranno sul disco rigido un totale di 800 KB. Tuttavia, se le dimensioni del blocco sono pari a 32 KB, 100 I/O in lettura/scrittura 3.200 KB (3,2 MB) sul disco rigido. Fino a quando la velocità di trasferimento SCSI supera la quantità totale di dati trasferiti, il recupero di un'unità della velocità di trasferimento "più veloce" non riuscirà. Per il confronto, vedere le tabelle seguenti.

  | |7200 RPM 9ms Seek, accesso 4ms|10.000 RPM 7ms Seek, accesso 3ms|15.000 RPM 4ms Seek, accesso 2 ms
  |-|-|-|-|
  |I/O casuale|80|100|150|
  |I/O sequenziale|250|300|500|  
  
  |unità RPM 10.000|Dimensioni blocco da 8 KB (Active Directory Jet)|
  |-|-|
  |I/O casuale|800 KB/s|
  |I/O sequenziale|2400 KB/s|

- **Backplane SCSI (bus):** Conoscere il modo in cui il cavo della barra multifunzione (bus), o in questo scenario, influisca sulla velocità effettiva del sottosistema di archiviazione dipende dalla conoscenza delle dimensioni del blocco. Essenzialmente, la domanda è la quantità di I/O in grado di gestire il bus se l'i/O è in blocchi da 8 KB. In questo scenario il bus SCSI è 20 MB/s o 20480 KB/s. 20480 KB/s diviso per i blocchi da 8 KB restituisce un massimo di circa 2500 IOPS supportato dal bus SCSI.

  >[!NOTE]
  >Le figure nella tabella seguente rappresentano un esempio. La maggior parte dei dispositivi di archiviazione collegati attualmente usa PCI Express, che fornisce una velocità effettiva molto più elevata.  
  
  |I/O supportati dalla dimensione del bus SCSI per blocco|Dimensioni blocco 2 KB|Dimensioni blocco da 8 KB (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10.000|2\.500|
  |40 MB/s|20.000|5\.000|
  |128 MB/s|65.536|16.384|
  |320 MB/s|160.000|40.000|

  Come può essere determinato da questo grafico, nello scenario presentato, indipendentemente dall'uso, il bus non sarà mai un collo di bottiglia, in quanto il numero massimo di mandrini è 100 I/O, ben al di sotto delle soglie sopra riportate.

  >[!NOTE]
  >Si presuppone che il bus SCSI sia efficiente al 100%.
  
- **Scheda SCSI-** Per determinare la quantità di I/O che questo può gestire, è necessario controllare le specifiche del produttore. L'indirizzamento delle richieste di I/O al dispositivo appropriato richiede l'elaborazione di alcuni tipi, quindi la quantità di I/O che può essere gestita dipende dal processore della scheda SCSI (o controller di Array).

  In questo esempio, verrà eseguito il presupposto che è possibile gestire 1.000 I/O.

- **Bus PCI-** Si tratta di un componente spesso trascurato. In questo esempio, questo non sarà il collo di bottiglia. Tuttavia, man mano che i sistemi si ridimensionano, può diventare un collo di bottiglia. Per riferimento, un bus PCI a 32 bit che opera in 33Mhz può in teoria trasferire 133 MB/s di dati. Di seguito è riportata l'equazione:  
  > 32 bits &divide; a 8 bit per &times; byte 33 MHz = 133 MB/s.  

  Si noti che è il limite teorico; in realtà viene raggiunto solo il 50% del valore massimo, anche se in determinati scenari di espansione è possibile ottenere l'efficienza del 75% per brevi periodi.

  Un bus PCI 66MHz a 64 bit può supportare un massimo teorico di (64 &divide; bit 8 bit per &times; byte 66 MHz) = 528 MB/sec. Inoltre, qualsiasi altro dispositivo (ad esempio, la scheda di rete, il secondo controller SCSI e così via) ridurrà la larghezza di banda disponibile Poiché la larghezza di banda è condivisa e i dispositivi si contendono le risorse limitate.

Dopo l'analisi dei componenti del sottosistema di archiviazione, l'asse rappresenta il fattore di limitazione della quantità di I/O che può essere richiesto e, di conseguenza, la quantità di dati che possono transitare nel sistema. In particolare, in uno scenario di servizi di dominio Active Directory sono 100 I/O casuali al secondo in incrementi di 8 KB, per un totale di 800 KB al secondo durante l'accesso al database Jet. In alternativa, la velocità effettiva massima per un mandrino allocato esclusivamente ai file di log subisce le limitazioni seguenti: 300 I/O sequenziali al secondo in incrementi di 8 KB, per un totale di 2400 KB (2,4 MB) al secondo.

A questo punto, dopo aver analizzato una semplice configurazione, nella tabella seguente viene illustrata la posizione in cui si verificherà il collo di bottiglia quando i componenti del sottosistema di archiviazione vengono modificati o aggiunti.

|Note|Analisi colli di bottiglia|Disco|Bus|Scheda|Bus PCI|
|-|-|-|-|-|-|
|Si tratta della configurazione del controller di dominio dopo l'aggiunta di un secondo disco. La configurazione del disco rappresenta il collo di bottiglia a 800 KB/s.|Aggiungi 1 disco (totale = 2)<br /><br />I/O è casuale<br /><br />Dimensioni blocco 4 KB<br /><br />10.000 RPM HD|totale 200 I/o<br />800 KB/s totale.| | | |
|Dopo l'aggiunta di 7 dischi, la configurazione del disco rappresenta ancora il collo di bottiglia a 3200 KB/s.|**Aggiungi 7 dischi (Totale = 8)**  <br /><br />I/O è casuale<br /><br />Dimensioni blocco 4 KB<br /><br />10.000 RPM HD|800 I/o totale.<br />3200 KB/s totali| | | |
|Dopo aver modificato I/O su sequenziale, la scheda di rete diventa il collo di bottiglia perché è limitata a 1000 IOPS.|Aggiungi 7 dischi (Totale = 8)<br /><br />**I/O sequenziale**<br /><br />Dimensioni blocco 4 KB<br /><br />10.000 RPM HD| | |2400 I/O al secondo possono essere letti/scritti su disco, il controller è limitato a 1000 IOPS| |
|Dopo aver sostituito la scheda di rete con una scheda SCSI che supporta 10.000 IOPS, il collo di bottiglia torna alla configurazione del disco.|Aggiungi 7 dischi (Totale = 8)<br /><br />I/O è casuale<br /><br />Dimensioni blocco 4 KB<br /><br />10.000 RPM HD<br /><br />**Aggiornare la scheda SCSI (ora supporta 10.000 I/O)**|800 I/o totale.<br />3\.200 KB/s totali| | | |
|Dopo aver aumentato le dimensioni del blocco a 32 KB, il bus diventa il collo di bottiglia perché supporta solo 20 MB/s.|Aggiungi 7 dischi (Totale = 8)<br /><br />I/O è casuale<br /><br />**Dimensioni blocco di 32 KB**<br /><br />10.000 RPM HD| |800 I/o totale. 25.600 KB/s (25 MB/s) possono essere letti/scritti su disco.<br /><br />Il bus supporta solo 20 MB/s| | |
|Dopo l'aggiornamento del bus e l'aggiunta di altri dischi, il disco rimane il collo di bottiglia.|**Aggiungi 13 dischi (Totale = 14)**<br /><br />Aggiungere la seconda scheda SCSI con 14 dischi<br /><br />I/O è casuale<br /><br />Dimensioni blocco 4 KB<br /><br />10.000 RPM HD<br /><br />**Aggiornamento a 320 MB/s bus SCSI**|2800 I/o<br /><br />11.200 KB/s (10,9 MB/s)| | | |
|Dopo aver modificato I/O su sequenziale, il disco rimane il collo di bottiglia.|Aggiungi 13 dischi (Totale = 14)<br /><br />Aggiungere la seconda scheda SCSI con 14 dischi<br /><br />**I/O sequenziale**<br /><br />Dimensioni blocco 4 KB<br /><br />10.000 RPM HD<br /><br />Aggiornamento a 320 MB/s bus SCSI|8\.400 I/o<br /><br />33.600 KB\s<br /><br />(32,8 MB\s)| | | |
|Dopo l'aggiunta di dischi rigidi più veloci, il disco rimane il collo di bottiglia.|Aggiungi 13 dischi (Totale = 14)<br /><br />Aggiungere la seconda scheda SCSI con 14 dischi<br /><br />I/O sequenziale<br /><br />Dimensioni blocco 4 KB<br /><br />**15.000 RPM HD**<br /><br />Aggiornamento a 320 MB/s bus SCSI|14.000 I/o<br /><br />56.000 KB/s<br /><br />(54,7 MB/s)| | | |
|Dopo aver aumentato le dimensioni del blocco a 32 KB, il bus PCI diventa il collo di bottiglia.|Aggiungi 13 dischi (Totale = 14)<br /><br />Aggiungere la seconda scheda SCSI con 14 dischi<br /><br />I/O sequenziale<br /><br />**Dimensioni blocco di 32 KB**<br /><br />15.000 RPM HD<br /><br />Aggiornamento a 320 MB/s bus SCSI| | | |14.000 I/o<br /><br />448.000 KB/s<br /><br />(437 MB/s) è il limite di lettura/scrittura per l'asse.<br /><br />Il bus PCI supporta un numero massimo teorico di 133 MB/s (il livello di efficienza del 75% è ottimale).|

### <a name="introducing-raid"></a>Introduzione a RAID

La natura di un sottosistema di archiviazione non viene modificata in modo significativo quando viene introdotto un controller di Array; sostituisce semplicemente la scheda SCSI nei calcoli. Ciò che cambia è il costo di lettura e scrittura dei dati sul disco quando si usano i diversi livelli di matrice (ad esempio RAID 0, RAID 1 o RAID 5).

In RAID 0, i dati vengono sottoposti a striping su tutti i dischi del set RAID. Ciò significa che durante un'operazione di lettura o scrittura, una parte dei dati viene estratta o inserita in ogni disco, aumentando la quantità di dati che possono transitare nel sistema durante lo stesso periodo di tempo. Quindi, in un secondo, in ogni mandrino (presupponendo di nuovo le unità da 10.000 RPM), è possibile eseguire le operazioni di I/O 100. La quantità totale di I/O che può essere supportata è N mandrini volte 100 I/O al secondo per mandrino (produce 100 * N I/O al secondo).

![Unità logica d:](media/capacity-planning-considerations-logical-d-drive.png)

In RAID 1 i dati vengono rispecchiati (duplicati) in una coppia di assi per la ridondanza. Pertanto, quando viene eseguita un'operazione di I/O di lettura, i dati possono essere letti da entrambi gli assi nel set. In questo modo, la capacità di I/O viene resa disponibile da entrambi i dischi durante un'operazione di lettura. Si noti che le operazioni di scrittura non ottengono un vantaggio in termini di prestazioni in un RAID 1. Questo perché gli stessi dati devono essere scritti in entrambe le unità per la ridondanza. Sebbene non ricerchi più tempo, poiché la scrittura dei dati si verifica simultaneamente su entrambi gli assi, perché entrambi gli assi sono occupati nella duplicazione dei dati, un'operazione di I/O di scrittura in sostanza impedisce che si verifichino due operazioni di lettura. Pertanto, ogni I/O di scrittura costa due I/O di lettura. È possibile creare una formula da tali informazioni per determinare il numero totale di operazioni di I/O che si verificano:  

> *Lettura i/o* + 2 &times; *scrittura*  = totale di i/o *su disco utilizzati*  

Quando il rapporto tra le letture e le Scritture e il numero di assi è noto, l'equazione seguente può essere derivata dall'equazione precedente per identificare il numero massimo di I/O che può essere supportato dalla matrice:  

> *IOPS massimo per mandrino* &times;2 mandrini &times; [( *%legge*  +  *%scrive*) &divide; ( *%legge* + 2 &times; *%scrive*)] = *Total IOPS*

RAID 1 + 0, si comporta esattamente come RAID 1 per quanto riguarda le spese di lettura e scrittura. Tuttavia, l'i/O viene ora sottoposto a striping in ogni set con mirroring. Se  

> *IOPS massimo per mandrino* &times;2 mandrini &times; [( *%legge*  +  *%scrive*) &divide; ( *%legge* + 2 &times; *%scrive*)] = *Total IOPS*  

in un set RAID 1, quando una molteplicità (*N*) di set RAID 1 viene sottoposta a striping, le operazioni di i/o totali &times; che possono essere elaborate diventano N i/o per set RAID 1:  

> *N* &times; {*IOPS massimo per mandrino* &times;2 mandrini &times; [( *%Legge*  +  *%Scrive*) &divide; ( *%Legge* + 2 &times; *%Scrive*)] } = *Totale IOPS*

In RAID 5, a volte definito RAID *n* + 1, i dati vengono sottoposti a striping in *n* assi e le informazioni sulla parità vengono scritte nell'albero "+ 1". Tuttavia, RAID 5 è molto più costoso quando si eseguono operazioni di I/O in scrittura rispetto a RAID 1 o 1 + 0. RAID 5 esegue il processo seguente ogni volta che un I/O di scrittura viene inviato all'array:

1. Leggere i dati precedenti
1. Leggi la parità precedente
1. Scrivere i nuovi dati
1. Scrivere la nuova parità

Poiché ogni richiesta di I/O di scrittura inviata al controller di array dal sistema operativo richiede il completamento di quattro operazioni di I/O, le richieste di scrittura inviate durano quattro volte per il completamento come singolo I/O di lettura. Per derivare una formula per tradurre le richieste I/O dal punto di vista del sistema operativo a quello sperimentato dagli assi:  

> *Leggere I/O* + 4 &times; *Escrive I/O*  =  *Total I/O*  

Analogamente, in un set RAID 1, quando il rapporto tra letture e scritture e il numero di assi è noto, l'equazione seguente può essere derivata dall'equazione precedente per identificare il numero massimo di I/O che può essere supportato dalla matrice (si noti che il numero totale di mandrini non include e "unità" persa alla parità):  

> *IOPS per mandrino* &times; (*Mandrino* – 1) &times; [( *%Legge*  +  *%Scrive*) &divide; ( *%Legge* + 4 &times; *%Scrive*)] = *Totale IOPS*

### <a name="introducing-sans"></a>Introduzione a SANs

Espandendo la complessità del sottosistema di archiviazione, quando una SAN viene introdotta nell'ambiente, i principi di base delineati non cambiano, ma è necessario tenere conto del comportamento di I/O per tutti i sistemi connessi alla SAN. Uno dei principali vantaggi derivanti dall'utilizzo di una rete SAN è una quantità aggiuntiva di ridondanza sull'archiviazione collegata internamente o esternamente. la pianificazione della capacità deve ora prendere in considerazione le esigenze di tolleranza di errore. Inoltre, vengono introdotti altri componenti che devono essere valutati. Suddivisione di una SAN nelle parti componenti:

- Unità disco rigido SCSI o Fibre Channel
- Backplane canale unità di archiviazione
- Unità di archiviazione
- Modulo controller di archiviazione
- Opzioni SAN
- HBA (s)
- Bus PCI

Quando si progetta un sistema per la ridondanza, vengono inclusi componenti aggiuntivi per consentire il rischio di errori. Quando si pianifica la capacità, è molto importante escludere il componente ridondante dalle risorse disponibili. Se, ad esempio, la SAN ha due moduli controller, la capacità di I/O di un modulo controller è tutto da usare per la velocità effettiva di I/O totale disponibile per il sistema. Ciò è dovuto al fatto che se si verifica un errore in un controller, l'intero carico di I/O richiesto da tutti i sistemi connessi dovrà essere elaborato dal controller rimanente. Quando viene eseguita la pianificazione della capacità per i periodi di picco di utilizzo, i componenti ridondanti non devono essere inseriti nelle risorse disponibili e l'utilizzo del picco pianificato non deve superare il 80% di saturazione del sistema (per gestire i picchi o il sistema anomalo) comportamento). Analogamente, l'opzione SAN ridondante, l'unità di archiviazione e gli assi non devono essere conteggiati nei calcoli di I/O.

Quando si analizza il comportamento del disco rigido SCSI o Fibre Channel, il metodo di analisi del comportamento, come descritto in precedenza, non cambia. Sebbene esistano alcuni vantaggi e svantaggi per ogni protocollo, il fattore di limitazione in base al disco è la limitazione meccanica del disco rigido.

L'analisi del canale nell'unità di archiviazione è esattamente identica al calcolo delle risorse disponibili sul bus SCSI o alla larghezza di banda, ad esempio 20 MB/s, divisa per dimensione del blocco (ad esempio 8 KB). Il punto in cui si devia dall'esempio precedente semplice consiste nell'aggregazione di più canali. Se, ad esempio, sono presenti 6 canali, ognuno dei quali supporta 20 MB/s di velocità massima di trasferimento, la quantità totale di I/O e il trasferimento dei dati disponibili è 100 MB/s (questo è corretto, non è 120 MB/s). Anche in questo caso, la tolleranza di errore è un lettore principale in questo calcolo, in caso di perdita di un intero canale, il sistema viene lasciato solo con 5 canali funzionanti. Pertanto, per garantire che continui a soddisfare le aspettative di prestazioni in caso di errore, la velocità effettiva totale per tutti i canali di archiviazione non deve superare 100 MB/s (questo presuppone che il carico e la tolleranza di errore siano distribuiti uniformemente tra tutti i canali). La trasformazione in un profilo di I/O dipende dal comportamento dell'applicazione. Nel caso di Active Directory Jet i/o, questo sarebbe correlato a circa 12.500 di i/o al secondo (100 MB/s &divide; 8 KB per I/o).

Successivamente, è necessario ottenere le specifiche del produttore per i moduli controller per comprendere la velocità effettiva che ciascun modulo è in grado di supportare. In questo esempio, la rete SAN dispone di due moduli controller che supportano ogni 7.500 I/O. La velocità effettiva totale del sistema può essere di 15.000 IOPS se non si desidera la ridondanza. Per calcolare la velocità effettiva massima in caso di errore, la limitazione è la velocità effettiva di un controller o 7.500 IOPS. Questa soglia è ben al di sotto dei 12.500 IOPS (presupponendo la dimensione massima del blocco di 4 KB) che può essere supportata da tutti i canali di archiviazione e pertanto è attualmente il collo di bottiglia nell'analisi. Sempre a scopo di pianificazione, l'i/O massimo desiderato da pianificare sarebbe 10.400 I/O.

Quando i dati terminano il modulo controller, viene transitata una connessione Fibre Channel classificata a 1 GB/s (o 1 Gigabit al secondo). Per correlare questo oggetto con le altre metriche, 1 GB/s diventa 128 MB/s (1 GB/s &divide; 8 bit/byte). Poiché questo supera la larghezza di banda totale in tutti i canali nell'unità di archiviazione (100 MB/s), non si verifica un collo di bottiglia del sistema. Inoltre, poiché si tratta solo di uno dei due canali (la connessione aggiuntiva di 1 GB/s Fibre Channel per la ridondanza), se una connessione ha esito negativo, la connessione rimanente ha ancora una capacità sufficiente per gestire tutto il trasferimento dei dati richiesto.

In route al server, i dati probabilmente transiteranno un commutire SAN. Dato che il comportatore SAN deve elaborare la richiesta di I/O in ingresso e in uscita dalla porta appropriata, l'opzione avrà un limite alla quantità di I/O che può essere gestita. Tuttavia, le specifiche dei produttori saranno necessarie per determinare il limite. Se, ad esempio, sono disponibili due opzioni e ogni opzione è in grado di gestire 10.000 IOPS, la velocità effettiva totale sarà di 20.000 IOPS. Anche in questo caso, la tolleranza di errore costituisce un problema, in caso di errore di un'opzione, la velocità effettiva totale del sistema sarà di 10.000 IOPS. Poiché è preferibile non superare il 80% di utilizzo durante il normale funzionamento, l'utilizzo di non più di 8000 I/O dovrebbe essere la destinazione.

Infine, la scheda HBA installata nel server avrebbe anche un limite alla quantità di I/O che è in grado di gestire. In genere, una seconda scheda HBA viene installata per la ridondanza, ma proprio come con l'opzione SAN, quando si calcola il numero massimo di I/O che può essere gestito, la velocità effettiva totale di *N* &ndash; 1 HBA è la scalabilità massima del sistema.

### <a name="caching-considerations"></a>Considerazioni sulla memorizzazione nella cache

Le cache sono uno dei componenti che possono influire in modo significativo sulle prestazioni complessive in qualsiasi punto del sistema di archiviazione. L'analisi dettagliata degli algoritmi di Caching esula dall'ambito di questo articolo. Tuttavia, alcune istruzioni di base sulla memorizzazione nella cache su sottosistemi disco valgono l'illuminazione:

- La memorizzazione nella cache consente di migliorare le operazioni di I/O in scrittura sequenziali, in quanto consente di memorizzare nel buffer molte operazioni di scrittura più piccole in blocchi I/O più grandi e di defase per l'archiviazione in un minor numero di dimensioni di blocco In questo modo si ridurrà l'i/o totale casuale e il totale di I/O sequenziale, garantendo così una maggiore disponibilità delle risorse per altri I/O.
- La memorizzazione nella cache non migliora la velocità effettiva di I/O di scrittura del sottosistema di archiviazione. Consente solo la memorizzazione nel buffer delle Scritture fino a quando non sono disponibili gli assi per eseguire il commit dei dati. Quando tutti gli i/O disponibili degli assi nel sottosistema di archiviazione sono saturi per periodi prolungati, la cache verrà colmata. Per svuotare la cache, è necessario assegnare tempo sufficiente tra i picchi o gli assi aggiuntivi, in modo da fornire un numero sufficiente di I/O per consentire lo scaricamento della cache.

  Le cache di dimensioni maggiori consentono solo la memorizzazione nel buffer di più dati. Ciò significa che è possibile gestire periodi più lunghi di saturazione.

  In un sottosistema di archiviazione normalmente funzionante, il sistema operativo proverà a migliorare le prestazioni di scrittura in quanto i dati devono essere scritti nella cache. Una volta che i supporti sottostanti sono saturi con I/O, la cache riempie e le prestazioni di scrittura torneranno alla velocità del disco.

- Quando si esegue la memorizzazione nella cache dell'I/O di lettura, lo scenario in cui la cache è più vantaggiosa è quando i dati vengono archiviati in sequenza sul disco e la cache può essere Read-Ahead (si presuppone che il settore successivo contenga i dati che verranno richiesti successivamente).
- Quando l'I/O di lettura è casuale, è improbabile che la memorizzazione nella cache del controller dell'unità fornisca alcun miglioramento alla quantità di dati che possono essere letti dal disco. Qualsiasi miglioramento è inesistente se la dimensione della cache basata su applicazioni o del sistema operativo è superiore alla dimensione della cache basata su hardware.

  Nel caso di Active Directory, la cache è limitata solo dalla quantità di RAM.

### <a name="ssd-considerations"></a>Considerazioni sull'unità SSD

Le unità SSD sono un animale completamente diverso rispetto ai dischi rigidi basati su mandrino. Tuttavia, i due criteri chiave rimangono: "Quanti IOPS può gestire?" e "Qual è la latenza per questi IOPS?" Rispetto ai dischi rigidi basati su mandrino, le unità SSD possono gestire volumi più elevati di I/O e possono avere latenze inferiori. In generale e al momento della stesura di questo articolo, mentre le unità SSD sono ancora costose in un confronto con costi per Gigabyte, sono molto convenienti in termini di costo per I/O e meritano una considerazione significativa in termini di prestazioni di archiviazione.

Considerazioni:

- Sia le operazioni di IOPS che le latenze sono molto soggettive rispetto alle progettazioni del produttore e, in alcuni casi, sono state osservate come prestazioni più ridotte rispetto alle tecnologie basate su mandrino. In breve, è più importante verificare e convalidare le specifiche del produttore unità per unità e non presupporre alcuna generalità.
- I tipi di IOPS possono avere numeri molto diversi a seconda che si tratti di lettura o scrittura. I servizi di servizi di dominio Active Directory, in generale, che prevalentemente sono basati su lettura, saranno meno interessati rispetto ad altri scenari di applicazione.
- "Resistenza alla scrittura": questo è il concetto che le celle SSD verranno esaurite. Diversi produttori affrontano questa sfida in modo diverso. Almeno per l'unità di database, il profilo i/O di lettura prevalentemente consente di sminuire il significato di questa preoccupazione poiché i dati non sono altamente volatili.

### <a name="summary"></a>Riepilogo

Un modo per prendere in considerazione l'archiviazione è la raffigurazione del plumbing familiare. Si supponga che il valore di IOPS dei supporti in cui sono archiviati i dati sia lo svuotamento principale della casa. Quando questo è intasato (ad esempio, le radici nella pipe) o limitato (è compresso o troppo piccolo), tutti i sink della famiglia eseguono il backup quando viene usata una quantità eccessiva di acqua (troppi Guest). Questo è perfettamente analogo a un ambiente condiviso in cui uno o più sistemi sfruttano lo spazio di archiviazione condiviso in un SAN/NAS/iSCSI con lo stesso supporto sottostante. È possibile adottare diversi approcci per risolvere i diversi scenari:

- Uno svuotamento compresso o sottodimensionato richiede una sostituzione e una correzione completa della scalabilità. Si tratta di un aspetto simile all'aggiunta di nuovi componenti hardware o alla ridistribuzione dei sistemi mediante lo spazio di archiviazione condiviso nell'infrastruttura.
- Una pipe "intasata" indica in genere l'identificazione di uno o più problemi e la rimozione di tali problemi. In uno scenario di archiviazione potrebbero essere presenti backup a livello di sistema o di archiviazione, analisi antivirus sincronizzate in tutti i server e il software di deframmentazione sincronizzato in esecuzione durante i periodi di picco.

In qualsiasi progettazione di plumbing, più feed di scarichi nello svuotamento principale. Se si arresta uno di questi vuoti o un punto di giunzione, viene eseguito il backup solo degli elementi dietro il punto di giunzione. In uno scenario di archiviazione, potrebbe trattarsi di un commutatore di overload (scenario SAN/NAS/iSCSI), di problemi di compatibilità del driver (combinazione di driver/firmware HBA/Storport. sys errato) o di backup/antivirus/deframmentazione. Per determinare se la "pipe" di archiviazione è sufficientemente grande, è necessario misurare IOPS e dimensioni di I/O. In ogni congiunzione si aggiungono insieme per garantire un "diametro di pipe" adeguato.

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>Appendice D-discussione sulla risoluzione dei problemi di archiviazione-ambienti in cui la disponibilità di RAM pari almeno alla dimensione del database non è un'opzione valida

È utile comprendere il motivo per cui queste indicazioni sono disponibili, in modo che le modifiche alla tecnologia di archiviazione possano essere gestite. Questi consigli sono disponibili per due motivi. Il primo è l'isolamento di IO, in modo che i problemi di prestazioni (ovvero il paging) nell'asse del sistema operativo non influiscano sulle prestazioni del database e dei profili di I/O. Il secondo è che i file di log per servizi di dominio Active Directory e la maggior parte dei database sono di natura sequenziale, mentre le cache e i dischi rigidi basati su assi hanno un notevole vantaggio a livello di prestazioni quando vengono usati con I/O sequenziali rispetto ai modelli di I/O più casuali del sistema operativo e quasi modelli di I/O puramente casuali dell'unità del database di servizi di dominio Active Directory. Isolando l'i/O sequenziale in un'unità fisica separata, è possibile aumentare la velocità effettiva. La sfida presentata dalle opzioni di archiviazione di oggi è che i presupposti fondamentali alla base di queste raccomandazioni non sono più vere. In molti scenari di archiviazione virtualizzati, ad esempio i file di immagine iSCSI, SAN, NAS e disco virtuale, il supporto di archiviazione sottostante è condiviso tra più host, negando in tal modo completamente l'"isolamento degli i/O" e gli aspetti "ottimizzazione di I/O sequenziale". Questi scenari aggiungono infatti un ulteriore livello di complessità in quanto gli altri host che accedono ai supporti condivisi possono ridurre la velocità di risposta al controller di dominio.

Per la pianificazione delle prestazioni di archiviazione, è necessario considerare tre categorie: stato della cache a freddo, stato della cache riscaldata e backup/ripristino. Lo stato della cache a freddo si verifica in scenari come quando il controller di dominio viene riavviato inizialmente o il servizio Active Directory viene riavviato e non sono presenti dati Active Directory nella RAM. Lo stato della cache a caldo è il punto in cui il controller di dominio è in uno stato stabile e il database viene memorizzato nella cache. È importante tenere presente che i profili delle prestazioni sono molto diversi e la quantità di RAM sufficiente per memorizzare nella cache l'intero database non consente le prestazioni quando la cache è a freddo. Uno può considerare la progettazione delle prestazioni per questi due scenari con la seguente analogia, il riscaldamento della cache fredda è uno "Sprint" e l'esecuzione di un server con una cache a caldo è una "maratona".

Per lo scenario di cache a freddo e a caldo della cache, la domanda diventa la velocità con cui lo spazio di archiviazione può spostare i dati dal disco alla memoria. Il riscaldamento della cache è uno scenario in cui, nel tempo, le prestazioni migliorano il riutilizzo dei dati, la percentuale di riscontri nella cache e la frequenza con cui la necessità di passare al disco diminuisce. Di conseguenza, l'impatto negativo sulle prestazioni del passaggio al disco diminuisce. Eventuali peggioramenti delle prestazioni sono solo temporanei durante l'attesa che la cache si scaldi e raggiunga le dimensioni massime consentite dal sistema. La conversazione può essere semplificata in base alla velocità con cui i dati possono essere disattivati dal disco ed è una misura semplice delle operazioni di i/o al secondo disponibili per Active Directory, che è soggetta a IOPS disponibili nell'archivio sottostante. Dal punto di vista della pianificazione, poiché il riscaldamento degli scenari di cache e di backup/ripristino avviene in modo eccezionale, in genere si verificano fuori orario e sono soggettivi al carico del controller di dominio, le raccomandazioni generali non esistono, tranne nel caso in cui queste attività siano pianificate per le ore non di punta.

Servizi di dominio Active Directory, nella maggior parte degli scenari, è prevalentemente di lettura i/o, in genere un rapporto del 90% di lettura/10% di scrittura. L'I/O di lettura spesso tende a essere il collo di bottiglia per l'esperienza utente e con l'I/O di scrittura causa un peggioramento delle prestazioni di scrittura. Poiché le operazioni di I/O per NTDS. dit sono prevalentemente casuali, le cache tendono a fornire un vantaggio minimo per leggere i/o, rendendolo molto più importante configurare correttamente l'archiviazione per il profilo di I/O di lettura.

Per le normali condizioni operative, l'obiettivo della pianificazione dell'archiviazione è ridurre al minimo i tempi di attesa per la restituzione da disco di una richiesta di servizi di dominio Active Directory. Ciò significa essenzialmente che il numero di I/O in attesa e in sospeso è inferiore o equivalente al numero di percorsi sul disco. Esistono diversi modi per misurare questo problema. In uno scenario di monitoraggio delle prestazioni, l'indicazione generale è che la disco logico ( *\<\>unità del database NTDS*) \Avg disco/sec è inferiore a 20 ms. La soglia operativa desiderata deve essere molto inferiore, preferibilmente quanto più vicina alla velocità dello spazio di archiviazione possibile, nell'intervallo da 2 a 6 millisecondi (da 002 a .006 secondo), a seconda del tipo di archiviazione.

Esempio:

![Grafico della latenza di archiviazione](media/capacity-planning-considerations-storage-latency.png)

Analisi del grafico:

- **Ovale verde a sinistra:** La latenza rimane coerente a 10 ms. Il carico aumenta da 800 IOPS a 2400 IOPS. Questo è il piano assoluto per la velocità con cui una richiesta di I/O può essere elaborata dall'archiviazione sottostante. Questa operazione è soggetta alle specifiche della soluzione di archiviazione.
- **Ovale Bordeaux a destra:** La velocità effettiva rimane piana dall'uscita del cerchio verde fino alla fine della raccolta dei dati, mentre la latenza continua ad aumentare. In questo modo viene dimostrato che quando i volumi della richiesta superano i limiti fisici dell'archiviazione sottostante, più a lungo le richieste si siedono nella coda in attesa di essere inviate al sottosistema di archiviazione.

Applicazione di queste informazioni:

- **Conseguenze per un utente che esegue una query sull'appartenenza di un gruppo di grandi dimensioni:** Si supponga che questa operazione richieda la lettura di 1 MB di dati dal disco, la quantità di I/O e il tempo impiegato per la valutazione come segue:
  - Active Directory le pagine del database hanno una dimensione di 8 KB. 
  - È necessario leggere almeno 128 pagine dal disco. 
  - Supponendo che non siano presenti elementi memorizzati nella cache, al piano (10 ms) verranno importati almeno 1,28 secondi per caricare i dati dal disco per restituirli al client. A 20 ms, in cui la velocità effettiva di archiviazione è durata a partire dal limite massimo e rappresenta anche il valore massimo consigliato, per ottenere i dati dal disco per poter essere restituiti all'utente finale verranno importati 2,5 secondi.  
- **A quale velocità verrà scaldata la cache –** Presumendo che il carico del client aumenti la velocità effettiva in questo esempio di archiviazione, la cache riscalderà a una velocità di 2400 &times; IOPS 8 KB per i/o. Oppure circa 20 MB al secondo, caricando circa 1 GB di database in RAM ogni 53 secondi.

> [!NOTE]
> È normale che per brevi periodi di osservazione le latenze si arrampichi quando i componenti leggono o scrivono in modo aggressivo sul disco, ad esempio quando viene eseguito il backup del sistema o quando servizi di dominio Active Directory è in esecuzione Garbage Collection. Per gestire questi eventi periodici, è necessario fornire una maggiore parte dei calcoli. L'obiettivo è fornire una velocità effettiva sufficiente per gestire questi scenari senza influisca sulla funzione normale.

Come si può notare, esiste un limite fisico basato sulla progettazione dell'archiviazione per la velocità con cui la cache potrebbe essere calda. Ciò che verrà scaldato nella cache sono le richieste client in ingresso fino alla frequenza con cui l'archiviazione sottostante è in grado di fornire. L'esecuzione di script per "pre-warm" nella cache durante le ore di picco offrirà la competizione per il carico basato su richieste client reali. Ciò può influire negativamente sul recapito dei dati necessari ai client perché, per impostazione predefinita, genera una competizione per le scarse risorse disco, perché i tentativi artificiali di riscaldamento della cache caricherà i dati che non sono rilevanti per i client che contattano il controller di dominio.
