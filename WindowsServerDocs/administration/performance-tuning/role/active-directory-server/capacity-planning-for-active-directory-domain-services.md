---
title: Pianificazione della capacità per servizi di dominio Active Directory
description: Descrizione dettagliata dei fattori da considerare durante la pianificazione della capacità per Active Directory Domain Services.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 5a9e2d39d4eedd1e8fdb4bfeaf267ad4cb4c596a
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799833"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Pianificazione della capacità per servizi di dominio Active Directory

In questo argomento è stata scritta in origine tramite Brumfield Ken, Senior Premier Field Engineer presso Microsoft e offre suggerimenti per la pianificazione della capacità per Active Directory Domain Services (AD DS).

## <a name="goals-of-capacity-planning"></a>Obiettivi di pianificazione della capacità

Pianificazione della capacità non è quello utilizzato per la risoluzione dei problemi degli eventi imprevisti delle prestazioni. Sono strettamente correlati, ma piuttosto diverso. Gli obiettivi di pianificazione della capacità sono:  

- Implementare e usare un ambiente correttamente 
- Ridurre al minimo i problemi di prestazioni tempo impiegato per la risoluzione dei problemi.  
  
La pianificazione della capacità, un'organizzazione può avere una destinazione di base del 40% di utilizzo del processore durante i periodi di picco per soddisfare i requisiti relativi alle prestazioni di client e gestire il tempo necessario per aggiornare l'hardware nel Data Center. Mentre, per ricevere una notifica degli eventi imprevisti di prestazioni anomale, una soglia di avviso di monitoraggio potrebbe essere impostata su 90% in un intervallo di 5 minuti.

La differenza è che quando viene continuamente superata una soglia di gestione della capacità (un evento singolo non rappresenta un problema), aggiunta di capacità (che è, aggiungendo processori più veloci o più) sarebbe una soluzione o il servizio di scalabilità tra più server sarebbe un soluzione. Soglie di avviso delle prestazioni indicano che esperienza client attualmente carente e sono necessari passaggi immediati per risolvere il problema.

Come un'analogia: gestione della capacità consiste nella prevenzione di un incidente auto (difensive guidando assicurandosi freni funzionino correttamente, e così via) mentre la risoluzione dei problemi di prestazioni è la polizia vigili del fuoco e il personale medico emergenza operazioni Dopo un incidente. Questo argomento vengono descritti "difensive guidando" basato su Active Directory.

Nel corso degli ultimi anni, Guida alla pianificazione della capacità per i sistemi di scalabilità verticale è stata modificata in modo significativo. Le modifiche seguenti nelle architetture di sistema hanno contestato presupposti fondamentali sulla progettazione e ridimensionamento di un servizio:

- nelle piattaforme server a 64 bit  
- Virtualizzazione  
- Maggiore attenzione al consumo di energia  
- Archiviazione su unità SSD  
- Scenari cloud  

Inoltre, l'approccio è passata da una capacità basata su server pianificazione esercizio a una capacità in base al servizio di pianificazione esercizio. Active Directory Domain Services (AD DS), un servizio distribuito avanzato che molti prodotti Microsoft e terze parti utilizzano come un back-end, diventa uno dei prodotti più importanti da pianificare in modo corretto garantire la capacità necessaria per l'esecuzione delle altre applicazioni.

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>Requisiti di base, per indicazioni sulla pianificazione della capacità

In questo articolo, sono previsti i requisiti di base seguenti:

- I lettori di aver letto e acquisito familiarità con [Performance Tuning linee guida per Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- La piattaforma Windows Server è x64 basati su architettura. Ma anche se l'ambiente Active Directory viene installato in Windows Server 2003 x86 (a questo punto oltre la fine del ciclo di vita di supporto) e ha una struttura di informazioni di directory (DIT) vale a dire meno 1,5 GB di dimensioni e che possono facilmente essere conservati in memoria, le linee guida da questo oggetto articolo sono comunque applicabili.
- Pianificazione della capacità è un processo continuativo eseguito e si devono esaminare periodicamente l'efficacia l'ambiente soddisfa le aspettative.
- Ottimizzazione svolgeranno in più cicli di vita dell'hardware, come la modifica dei costi dell'hardware. Ad esempio, della memoria diventa più economica, si riduce il costo per ogni core o il prezzo dell'archiviazione diverse opzioni di modifica.
- Piano per il periodo di occupato di picco del giorno. È consigliabile a questo aspetto in intervalli di 30 minuti o ore. Un valore maggiore può nascondere picchi effettivi e qualsiasi valore che inferiore è distorto da "picchi temporanei."
- Pianificare la crescita nel corso del ciclo di vita dell'hardware per le aziende. Può trattarsi di una strategia di aggiornamento o l'aggiunta di hardware in un processo sfalsato o un aggiornamento completo di ogni tre e cinque anni. Ognuno richiede una "stima" come aumenterà notevolmente il carico su Active Directory. I dati cronologici, se raccolti, potrà stimare con questa valutazione. 
- Pianificare la tolleranza di errore. Una volta una stima *N* è derivato, in piano per gli scenari che includono *N* &ndash; 1, *N* &ndash; 2 *N* &ndash; *x*.
  - Aggiungere server aggiuntivi in base alle esigenze dell'organizzazione per garantire che la perdita di un singolo o più server non superi le stime di capacità massima.
  - Si consideri anche che il piano di aumento delle dimensioni e il piano di tolleranza di errore devono essere integrate. Ad esempio, se un controller di dominio è necessario per supportare il carico, ma la stima è che il carico verrà raddoppiato nell'anno successivo e richiedono due controller di dominio totale, non sarà disponibile una capacità sufficiente per supportare la tolleranza di errore. La soluzione, è possibile iniziare con tre controller di dominio. Uno è stato inoltre prevede di aggiungere il controller di dominio di terzo dopo 3 o 6 mesi se budget sono ridotti.

    >[!NOTE]
    >Aggiunta di applicazioni compatibili con Active Directory potrebbe avere un impatto significativo sul carico controller di dominio, se il carico provengono da applicazioni server o client.

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>Processo in tre fasi per il ciclo di pianificazione della capacità

Pianificazione della capacità, innanzitutto, decidere quali qualità del servizio è necessaria. Ad esempio, un Data Center principale supporta un livello superiore di concorrenza e richiede esperienza più coerente per gli utenti e utilizzo di applicazioni, che richiede maggiore attenzione alla ridondanza e riducendo al minimo i colli di bottiglia del sistema e dell'infrastruttura. Al contrario, un percorso satellite con un numero limitato di utenti non è necessario lo stesso livello di concorrenza o la tolleranza di errore. Di conseguenza, la sede periferica potrebbe non essere necessario molta attenzione all'ottimizzazione dell'hardware e infrastruttura, con conseguente risparmio sui costi sottostante. Tutte le raccomandazioni e indicazioni qui contenute sono per ottenere prestazioni ottimali e può essere flessibile in modo selettivo per gli scenari con requisiti inferiori.

La domanda successiva è: virtualizzato o fisico? Da una prospettiva di pianificazione della capacità, non vi è alcuna risposta giusta o sbagliata. è solo un diverso set di variabili con cui operare. Scenari di virtualizzazione sono disponibili verso il basso su una delle due opzioni:

- "Mapping diretto" con un guest per ogni host (in cui la virtualizzazione esiste esclusivamente per astrarre l'hardware fisico dal server)
- "Host condiviso"

Scenari di test e produzione indicano che lo scenario "mapping diretto" può essere gestito in modo identico a un host fisico. "Shared host", tuttavia, introduce una serie di considerazioni venga specificata in modo più dettagliato più avanti. Lo scenario di "host condiviso" significa che Active Directory Domain Services anche entra in competizione per le risorse e sono presenti multe e considerazioni sull'ottimizzazione per eseguire questa operazione.

Tenendo presenti queste considerazioni, la capacità del ciclo di pianificazione è un processo iterativo costituito da tre passaggi:

1. Misurare l'ambiente esistente, determinare dove i colli di bottiglia di sistema attualmente e ottenere ambientali nozioni di base necessarie per pianificare la quantità di capacità necessari.
1. Determinare l'hardware necessario in base ai criteri indicati nel passaggio 1.
1. Monitorare e convalidare che l'infrastruttura, come implementato sta operando all'interno di specifiche. Alcuni dati raccolti in questo passaggio diventano la linea di base per il successivo ciclo di pianificazione della capacità.

### <a name="applying-the-process"></a>Il processo di applicazione

Per ottimizzare le prestazioni, verificare che questi componenti principali sono selezionati correttamente e ottimizzati per l'applicazione viene caricata:

1. Memoria
1. Rete
1. Archiviazione
1. Processore
1. Accesso rete

I requisiti di archiviazione di base di Active Directory Domain Services e il comportamento generale del software client ben scritto consentono gli ambienti con un massimo di 10.000 e 20.000 agli utenti di rinunciare investimento ingente in per quanto riguarda l'hardware fisico, come quasi tutti i server moderne di pianificazione della capacità sistema di classe gestirà il carico. Ciò premesso, la tabella seguente riepiloga le modalità di valutazione di un ambiente esistente per poter selezionare l'hardware appropriato. Ogni componente viene analizzato in dettaglio nelle sezioni successive per consentire agli amministratori di Active Directory Domain Services di valutare la propria infrastruttura con indicazioni di base ed entità specifiche dell'ambiente.

In generale:

- Qualsiasi ridimensionamento basato sui dati correnti saranno solo accurato per l'ambiente corrente.
- Per eventuali stime, prevedere la domanda incremento nel ciclo di vita dell'hardware.
- Determinare se a P20 oggi e crescere nell'ambiente di dimensioni maggiori o aggiungere capacità al ciclo di vita.
- Per la virtualizzazione, le stesse entità e le metodologie di pianificazione della capacità valide, ad eccezione del fatto che l'overhead della virtualizzazione deve essere aggiunto a qualsiasi elemento dominio correlato.
- Pianificazione della capacità, quali tutto ciò che tenta di prevedere, non è una scienza accurata. Non si prevede di elementi da calcolare perfettamente e con un'accuratezza di 100%. Le indicazioni fornite in questo caso sono consigliabile più snelli; componente aggiuntivo di capacità per garantire una maggiore sicurezza e convalidare continuamente che l'ambiente rimane nella destinazione.

### <a name="data-collection-summary-tables"></a>Tabelle di riepilogo raccolta dei dati

#### <a name="new-environment"></a>Nuovo ambiente

| Componente | Stime |
|-|-|
|Dimensioni di archiviazione/Database|Da 40 KB a 60 KB per ogni utente|
|RAM|Dimensioni del database<br />Consigli di base del sistema operativo<br />Applicazioni di terze parti|
|Rete|1 GB|
|CPU|1000 utenti simultanei per ogni core|

#### <a name="high-level-evaluation-criteria"></a>Criteri di valutazione di alto livello

| Componente | Criteri di valutazione | Considerazioni sulla pianificazione |
|-|-|-|
|Dimensioni di archiviazione/Database|La sezione intitolata "per attivare la registrazione di spazio su disco che viene liberato dalla deframmentazione" in [i limiti di archiviazione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Archiviazione / le prestazioni del Database|<ul><li>"Disco logico ( *\<unità di Database NTDS\>* ) \Avg letture disco/sec," "disco logico ( *\<unità di Database NTDS\>* ) \Avg scritture disco/sec," " Disco logico ( *\<unità di Database NTDS\>* ) \Avg trasferimenti disco/sec "</li><li>"Disco logico ( *\<unità di Database NTDS\>* ) \Reads/sec," "disco logico ( *\<unità di Database NTDS\>* ) \Writes/sec," "disco logico (  *\<Unità di Database NTDS\>* ) \Transfers/sec "</li></ul>|<ul><li>Archiviazione dispone di due problemi all'indirizzo<ul><li>Spazio disponibile su cui con le dimensioni delle attuali mandrino basati su unità SSD archiviazione basata su è irrilevante per la maggior parte degli ambienti di Active Directory.</li> <li>Operazioni di Input/Output (i/o) disponibile In molti ambienti, questo è spesso trascurato. Ma è importante valutare solo agli ambienti in cui è presente non RAM sufficiente per caricare l'intero Database NTDS in memoria.</li></ul><li>Archiviazione può essere un argomento complesso e dovrebbe coinvolgere le competenze di fornitore di hardware per il dimensionamento corretto. In particolare con scenari più complessi, ad esempio gli scenari di iSCSI SAN e NAS. Tuttavia, in generale, costo per Gigabyte di spazio di archiviazione è spesso in opposizione diretta dei costi per i/o:<ul><li>RAID 5 offre costi più bassi per Gigabyte rispetto a Raid 1, ma Raid 1 ha un costo inferiore per i/o</li><li>Unità disco rigido basato su spindle hanno costi più bassi per Gigabyte, ma unità SSD hanno un costo inferiore per i/o</li></ul><li>Dopo il riavvio del computer o il servizio Active Directory Domain Services, la cache del motore ESE (Extensible Storage) è vuota e le prestazioni risulteranno disco associato, mentre le cache warms.</li><li>Nella maggior parte degli ambienti Active Directory è a elevato utilizzo i/o in un modello casuale ai dischi, la negazione gran parte del vantaggio della memorizzazione nella cache di lettura e lettura le strategie di ottimizzazione.  Puoi anche AD ha una cache in memoria modo maggiore rispetto a memorizza nella cache di sistema di archiviazione la maggior parte delle.</li></ul>
|RAM|<ul><li>Dimensioni del database</li><li>Consigli di base del sistema operativo</li><li>Applicazioni di terze parti</li></ul>|<ul><li>L'archiviazione è il componente più lento in un computer. Più che può essere residenti in memoria, meno, è necessario accedere al disco.</li><li>Riservare una quantità di RAM sufficiente per archiviare il sistema operativo, agenti (antivirus, backup, il monitoraggio), Database NTDS e la crescita nel tempo.</li><li>Per gli ambienti in cui ottimizzare la quantità di RAM non costo effettivo (ad esempio, una località) o non è realizzabile (DIT è troppo grande), informazioni di riferimento alle dimensioni della sezione di archiviazione per assicurarsi che l'archiviazione.</li></ul>|
|Rete|<ul><li>"Interfaccia di rete (\*) \Bytes ricevuti/sec"</li><li>"Interfaccia di rete (\*) \Bytes inviati/sec"|<ul><li>In generale, il traffico inviato da un controller di dominio molto supera il traffico inviato a un controller di dominio.</li><li>Poiché una connessione Ethernet commutata è full duplex, il traffico di rete in ingresso e in uscita è necessario ridimensionare in modo indipendente.</li><li>Consolidamento del numero di controller di dominio aumenterà la quantità di larghezza di banda usata per l'invio di risposte richieste client per ogni controller di dominio, ma sarà abbastanza lineare per il sito nel suo complesso.</li><li>Se la rimozione di percorso satellite i controller di dominio, non dimenticare di aggiungere la larghezza di banda per il controller di dominio satellite nell'hub di controller di dominio, nonché da utilizzare per valutare quanto il traffico WAN sarà.</li></ul>|
|CPU"Disco logico ( *\<unità di Database NTDS\>* ) \Avg letture disco/sec"descripti"Process(lsass)\\' tempo processore"tors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>Dopo l'eliminazione di archiviazione come un collo di bottiglia, risolvere la quantità di potenza di calcolo necessita.</li><li>Anche se non è perfettamente lineare, numero di core del processore utilizzata tra tutti i server in un ambito specifico (ad esempio, un sito) è utilizzabile per misurare il numero di processori è necessario per supportare il carico di lavoro totale. Aggiungere il minimo necessario per mantenere il livello di servizio corrente in tutti i sistemi all'interno dell'ambito.</li><li>Le modifiche in velocità del processore, incluso il risparmio energia relative modifiche, i valori di impatto derivati dall'ambiente corrente. In genere, non è possibile valutare con precisione come passando da un processore 2.5 GHz a un processore a 3GHz ridurrà il numero di CPU necessita.</li></ul>|
|Accesso rete|<ul><li>"Accesso rete (\*) acquisisce \Semaphore"</li><li>"Accesso rete (\*) \Semaphore timeout"</li><li>"Accesso rete (\*) semaforo \Average contengono ora"</li></ul>|<ul><li>Net Logon Secure Channel/MaxConcurrentAPI interessa solo gli ambienti con autenticazioni NTLM e/o convalida PAC. Convalida PAC è attivata per impostazione predefinita nelle versioni di sistema operativo precedente a Windows Server 2008. Si tratta di un'impostazione client, vi è alcun impatto i controller di dominio fino a quando non è disattivata in tutti i sistemi client.</li><li>Ambienti con l'autenticazione, trust tra significativi che include relazioni di trust tra foreste, hanno maggiore rischio in caso contrario dimensionati in modo corretto.</li><li>Consolidamento dei server aumenta la concorrenza di autenticazione cross-trust.</li><li>Picchi necessario essere tenuto in considerazione, ad esempio cluster hanno esito negativo-over di valori, come gli utenti la riautenticazione concatenarle per il nuovo nodo del cluster.</li><li>I sistemi client singolo (ad esempio, un cluster) potrebbe essere necessario anche l'ottimizzazione.</li></ul>|

## <a name="planning"></a>Pianificazione

Per un lungo periodo di tempo, indicazione della community per il ridimensionamento di Active Directory Domain Services è stato quello di "put in più RAM come le dimensioni del database". Nella maggior parte, che Consiglio è che la maggior parte degli ambienti necessario preoccuparsi. Ma l'ecosistema di utilizzo di Active Directory Domain Services è diventato molto più grande, in quanto gli ambienti Active Directory Domain Services, rispetto alla sua introduzione nel 1999. Sebbene l'aumento di potenza di calcolo e l'opzione da x86 architetture x64 architetture ha reso gli aspetti molto più elusivo di ridimensionamento per le prestazioni non rilevanti per un set più ampio di clienti che eseguono Active Directory in hardware fisico, l'aumento delle dimensioni della virtualizzazione ha reintrodotto i problemi di ottimizzazione per un pubblico più ampio rispetto a prima.

Le indicazioni seguenti sono pertanto su come determinare e prevedere per soddisfare le esigenze di Active Directory come servizio indipendentemente dal fatto che venga distribuita in fisico, una combinazione di virtuali/computer fisici o in uno scenario puramente virtualizzato. Di conseguenza, si interromperà verso il basso della ognuno dei quattro componenti principali di valutazione: processore, memoria, rete e archiviazione. In breve, per ottimizzare le prestazioni in Active Directory Domain Services, l'obiettivo è ottenere il più vicino possibile limite del processore.

## <a name="ram"></a>RAM

Semplicemente, più che possono essere memorizzati nella cache nella memoria RAM, minore è necessario accedere al disco. Per ottimizzare la scalabilità del server la quantità minima di RAM deve essere la somma delle dimensioni del database corrente, le dimensioni totali di SYSVOL, il sistema operativo consigliata quantità e le indicazioni del fornitore per gli agenti (antivirus, monitoraggio, backup e così via ). Per un periodo aggiuntivo deve essere aggiunti per supportare la crescita nel corso della durata del server. Questo sarà ambientalistica soggettivo sulla base delle stime di crescita del database in base alle modifiche dell'ambiente.

Per gli ambienti in cui ottimizzare la quantità di RAM non costo effettivo (ad esempio, una località) o non è realizzabile (DIT è troppo grande), informazioni di riferimento la sezione di archiviazione per assicurarsi che l'archiviazione sia progettata correttamente.

Corollario che viene visualizzato nel contesto generale in memoria di ridimensionamento è il ridimensionamento del file della pagina. Nello stesso contesto di tutti gli altri elementi correlati della memoria, l'obiettivo è ridurre al minimo sarà il molto più lentamente del disco. In questo modo la domanda deve essere compreso tra, "come il file di paging di ridimensionamento?" per "Quantità di RAM è necessario per ridurre al minimo il paging?" La risposta alla domanda quest'ultima è descritto nella parte restante di questa sezione. In questo modo la maggior parte della discussione per ridimensionare il file di paging all'area di autenticazione dei consigli generali del sistema operativo e la necessità di configurare il sistema per i dump di memoria, che non sono correlati alle prestazioni di Active Directory Domain Services.

### <a name="evaluating"></a>La valutazione

La quantità di RAM che è un controller di dominio (DC) è effettivamente molto complessa per questi motivi:

- Un'alta probabilità di errore durante il tentativo di utilizzare un sistema esistente per valutare la quantità di RAM è necessaria perché LSASS annullerà in condizioni di pressione della memoria, artificialmente deflazionando la necessità.
- Il fatto soggettivo che un singolo controller di dominio deve solo memorizzare nella cache "interessante" ai propri client. Ciò significa che i dati che devono essere memorizzati nella cache in un controller di dominio in un sito con solo un server di Exchange sarà molto diversi rispetto ai dati che deve essere memorizzato nella cache in un controller di dominio solo l'autenticazione degli utenti.
- La manodopera per valutare RAM per ogni controller di dominio in un caso per caso è troppo elevata e viene modificato quando viene modificato l'ambiente.
- I criteri dietro indicazione consentiranno di prendere decisioni informate: 
- Più che possono essere memorizzati nella cache nella memoria RAM, meno, è necessario accedere al disco. 
- Archiviazione è di gran lunga il componente più lento di un computer. Accesso ai dati su basa spindle e unità SSD supporti di archiviazione sono nell'ordine di 1.000.000 di volte più lenti rispetto all'accesso ai dati nella memoria RAM.

Di conseguenza, per ottimizzare la scalabilità del server, la quantità minima di RAM è la somma delle dimensioni del database corrente, le dimensioni totali di SYSVOL, il sistema operativo consigliata quantità e le indicazioni del fornitore per gli agenti (antivirus, monitoraggio, backup e e così via). Aggiungere importi aggiuntivi per supportare la crescita nel corso della durata del server. Questo sarà ambientalistica soggettivo sulla base delle stime di crescita del database. Tuttavia, per i percorsi di satellite con un set ridotto di utenti finali, questi requisiti è possibile ridurre questi siti non saranno necessario alla cache tanto per la maggior parte delle richieste di servizio.

Per gli ambienti in cui ottimizzare la quantità di RAM non costo effettivo (ad esempio, una località) o non è realizzabile (DIT è troppo grande), informazioni di riferimento alle dimensioni della sezione di archiviazione per assicurarsi che l'archiviazione.

> [!NOTE]
> Corollario durante il ridimensionamento della memoria è il ridimensionamento del file della pagina. Poiché l'obiettivo è ridurre al minimo sarà il molto più lentamente dischi, la domanda va da "come il file di paging di ridimensionamento?" per "Quantità di RAM è necessario per ridurre al minimo il paging?" La risposta alla domanda quest'ultima è descritto nella parte restante di questa sezione. In questo modo la maggior parte della discussione per ridimensionare il file di paging all'area di autenticazione dei consigli generali del sistema operativo e la necessità di configurare il sistema per i dump di memoria, che non sono correlati alle prestazioni di Active Directory Domain Services.

### <a name="virtualization-considerations-for-ram"></a>Considerazioni sulla virtualizzazione di RAM

Evitare di commit in eccesso della memoria sull'host. L'obiettivo fondamentale alla base di ottimizzare la quantità di RAM è ridurre al minimo la quantità di tempo per passare al disco. In scenari di virtualizzazione, il concetto di commit in eccesso della memoria esistente in cui viene allocata altra RAM per i guest quindi presente nel computer fisico. In questo e in se stesso non costituisce un problema. Diventa un problema quando la memoria totale utilizzata attivamente da tutti i guest supera la quantità di RAM nell'host e l'host sottostante avvia il paging. Prestazioni diventano vincolati al disco nei casi in cui il controller di dominio sta per il database Ntds. dit per ottenere i dati, il controller di dominio sta per il file di paging per ottenere i dati o l'host sta per disco per ottenere i dati che il guest considera nella RAM.

### <a name="calculation-summary-example"></a>Esempio di riepilogo di calcolo

|Componente|Memoria stimata (ad esempio)|
|-|-|
|Sistema operativo di base consigliati RAM (Windows Server 2008)|2 GB|
|Attività interne LSASS|200 MB|
|Agente di monitoraggio|100 MB|
|Antivirus|100 MB|
|Database (catalogo globale)|8,5 GB si è sicuri???|
|Cuscinetto per il backup per l'esecuzione, gli amministratori di accedere senza impatto|1 GB|
|Totale|12 GB|

**Consigliato: 16 GB**

Nel corso del tempo, è possibile effettuare il presupposto che verranno aggiunti ulteriori dati al database e probabilmente sarà il server nell'ambiente di produzione da 3 a 5 anni. Basato su una stima della crescita del 33%, 16 GB sarebbe una quantità ragionevole di RAM per inserire in un server fisico. In una macchina virtuale, data la semplicità con cui le impostazioni possono essere modificate e RAM può essere aggiunti alla macchina virtuale, iniziando in corrispondenza di 12 GB con il piano di monitoraggio e aggiornamento in futuro è ragionevole.

## <a name="network"></a>Rete

### <a name="evaluating"></a>La valutazione
In questa sezione è minore sui modi per valutare le esigenze riguardanti il traffico di replica, che è incentrato sul traffico che attraversa la rete WAN e accuratamente verrà trattato negli [il traffico di replica di Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), piuttosto che sulla valutazione totale capacità di larghezza di banda e di rete necessaria, con strumenti per le query dei client, le applicazioni di criteri di gruppo e così via. Per gli ambienti esistenti, si possono essere raccolti usando i contatori delle prestazioni "interfaccia di rete (\*) \Bytes ricevuti/sec" e "interfaccia di rete (\*) \Bytes inviati/sec." Intervalli di campionamento per i contatori di interfaccia di rete in 15, 30 o 60 minuti. Un valore minore sarà in genere troppo volatile per le misurazioni valida. un valore superiore verrà smussare legge giornalieri eccessivamente.

> [!NOTE]
> In genere, la maggior parte del traffico di rete su un controller di dominio è in uscita perché il controller di dominio risponde alle query dei client. Questo è il motivo per lo stato attivo sul traffico in uscita, anche se è consigliabile valutare anche ogni ambiente per il traffico in ingresso. Lo stesso approccio è utilizzabile per risolvere e verificare i requisiti di traffico di rete in ingresso. Per altre informazioni, vedere l'articolo della Knowledge Base [929851: Intervallo di porte dinamiche predefinite per TCP/IP è stato modificato in Windows Vista e Windows Server 2008](http://support.microsoft.com/kb/929851).

### <a name="bandwidth-needs"></a>Esigenze di larghezza di banda

La pianificazione di scalabilità di rete comprende due categorie distinte: la quantità di traffico e la CPU caricare dal traffico di rete. Ognuno di questi scenari è semplice rispetto ad alcuni degli altri argomenti in questo articolo.

Per valutare la quantità di traffico devono essere supportate, esistono due categorie univoche della pianificazione della capacità per Active Directory Domain Services in termini di traffico di rete. Il primo è il traffico di replica che consente di scorrere tra i controller di dominio ed è descritta completamente nel riferimento [il traffico di replica di Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) e sono ancora rilevanti per le versioni correnti di Active Directory Domain Services. Il secondo è il traffico client-server all'interno del sito. Uno degli scenari più semplici per pianificare, il traffico all'interno del sito prevalentemente riceve delle piccole richieste dai client rispetto al grandi quantità di dati inviati ai client. 100 MB sarà in genere adeguati per ambienti di un massimo di 5.000 utenti per ogni server, in un sito. Supporto con una scheda di rete da 1 GB e Receive-Side Scaling (RSS) è consigliato per tutti gli elementi 5.000 utenti. Per convalidare questo scenario, in particolare nel caso di scenari di consolidamento dei server, esaminare l'interfaccia di rete (\*) \Bytes/sec tra tutti i controller di dominio in un sito, sommarli e dividere il numero di controller di dominio per assicurarsi che non esiste è una capacità adeguata. Il modo più semplice per eseguire questa operazione è usare la visualizzazione "Area in pila" in Windows affidabilità e Performance Monitor (precedentemente noto come Perfmon), assicurandosi che tutti i contatori vengono ridimensionati lo stesso.

Si consideri l'esempio seguente (noto anche come un davvero, davvero complesso modo per convalidare che la regola generale è applicabile a uno specifico ambiente). I presupposti seguenti:

- L'obiettivo consiste nel ridurre il footprint ai server il minor numero possibile. In teoria, un unico server includeranno il carico e un ulteriore server di distribuzione per garantire la ridondanza (*N* 1 scenario +). 
- In questo scenario, la scheda di rete corrente supporta solo 100 MB e si trova in un ambiente disattivato.  
  L'utilizzo della larghezza di banda di rete di destinazione massima è 60% in uno scenario N (perdita di un controller di dominio).
- Ogni server dispone di circa 10.000 client connessi a esso.

Informazioni ottenute dai dati nel grafico (interfaccia di rete (\*) \Bytes inviati/sec):

1. L'orario lavorativo inizia a imparare circa 5:30 e venti verso il basso in 19:00:00.
1. Il periodo più occupato di picco è compreso 8:00 e 8 5:15 di Mattina, con più di 25 byte inviati/sec nel controller di dominio più attivi.  
   > [!NOTE]
   > Tutti i dati sulle prestazioni sono cronologici. Pertanto, il punto dati massimo 8:15 indica il caricamento dal 8.00 a 8:15.
1. Sono presenti picchi prima 4:00 AM, con più di 20 byte inviati/sec nel controller di dominio più attivi, che potrebbe indicare uno carico da fusi orari diversi o in background di attività di infrastruttura, ad esempio i backup. Poiché il picco a ore 8:00 supera questa attività, quindi non è rilevante.
1. Esistono cinque i controller di dominio nel sito.
1. Il carico massimo è circa 5.5 MB/s per ogni controller di dominio, che rappresenta il 44% della connessione di 100 MB. Usando questi dati, è possibile prevedere che la larghezza di banda totale necessaria tra ore 8:00 e 8 5:15 di Mattina è 28 MB/s.
   >[!NOTE]
   >Prestare attenzione al fatto che i contatori di invio/ricezione di interfaccia di rete sono espressi in byte e della larghezza di banda di rete viene misurata in bit. 100 MB &divide; 8 = 12,5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusioni:

1. In questo ambiente corrente soddisfa i N + 1 e livello di tolleranza di errore al 60% di utilizzo di destinazione. Portare offline un sistema sposterà la larghezza di banda per ogni server da circa 5.5 MB/s (% con 44) a circa 7 MB/s (% 56).
1. In base l'obiettivo di consolidare in un server indicato in precedenza, questo entrambi supera l'utilizzo di destinazione massimo e teoricamente il possibile utilizzo di una connessione di 100 MB.
1. Con una connessione da 1 GB ciò rappresenterà 22% della capacità totale.
1. Nel normale funzionamento le condizioni nel *N* + 1 scenario, carico di lavoro client verrà distribuita in modo relativamente uniforme a circa 14 MB/s per ogni server o 11% della capacità totale.
1. Per assicurarsi che la capacità è sufficiente durante l'indisponibilità di un controller di dominio, le destinazioni operative normale per ogni server sarà di circa 30% di rete di utilizzo o 38 MB/s per ogni server. Destinazione del failover sarà 60% di utilizzo di rete o 72 MB/s per ogni server.  
  
In breve, la distribuzione finale dei sistemi deve avere una scheda di rete da 1 GB e di essere connessa a un'infrastruttura di rete in grado di supportare tale carico. Un'ulteriore nota è che dato la quantità di traffico di rete generato, il carico della CPU da comunicazioni di rete può avere un impatto significativo e limitare la scalabilità massima di Active Directory Domain Services. Lo stesso processo utilizzabile per stimare la quantità di comunicazioni in ingresso nel controller di dominio. Ma dato il predominanza del traffico in uscita rispetto al traffico in ingresso, è un esercizio per la maggior parte degli ambienti accademico. Per garantire il supporto hardware per RSS è importante in ambienti con più di 5.000 utenti per ogni server. Per gli scenari con elevato traffico di rete, bilanciamento del carico di interruzione può essere un collo di bottiglia. Questa condizione può essere rilevata dal processore (\*)\% il tempo di Interrupt non sono distribuiti tra le CPU. RSS è abilitato schede di rete può ridurre questo limite e aumentare la scalabilità.

> [!NOTE]
> Un approccio simile può essere usato per stimare la capacità aggiuntiva necessaria prima del consolidamento di data center, o il ritiro di un controller di dominio in una posizione di satellite. È sufficiente raccogliere il traffico in ingresso e in uscita per i client e che sarà la quantità di traffico che sarà presente sui collegamenti WAN.  
>  
> In alcuni casi, si potrebbe riscontrare una maggiore quantità di traffico al previsto poiché il traffico è più lento, ad esempio quando verifica certificati non riesce a soddisfare i timeout aggressivi tramite la rete WAN. Per questo motivo, ridimensionamento WAN e utilizzo deve essere un processo iterativo e continuo.

### <a name="virtualization-considerations-for-network-bandwidth"></a>Considerazioni sulla virtualizzazione per la larghezza di banda di rete

È facile per le raccomandazioni per un server fisico: 1 GB per i server che supportano maggiore di 5.000 utenti. Dopo che più utenti guest inizia a condividere un'infrastruttura di commutazione virtuale sottostante, è necessario assicurarsi che l'host dispone di larghezza di banda sufficiente per supportare tutti i guest nel sistema e richiede pertanto il rigore aggiuntivo ulteriore attenzione. Si tratta nient'altro che un'estensione di garantire l'infrastruttura di rete alla macchina host. Si tratta indipendentemente dal fatto che indica se la rete non include il controller di dominio in esecuzione come guest macchina virtuale in un host con il traffico di rete, superamento di un commutatore virtuale oppure se connesso direttamente a uno switch fisico. Il commutatore virtuale è solo uno dei componenti più in uplink deve supportare la quantità di dati trasmessi. In questo modo la scheda di rete fisica host fisico collegata al commutatore deve essere in grado di supportare il carico di controller di dominio e tutte le altri utenti guest di condivisione del commutatore virtuale connesso alla scheda di rete fisica.

### <a name="calculation-summary-example"></a>Esempio di riepilogo di calcolo

|Sistema|Larghezza di banda massima|
|-|-|
DC 1|6.5 MB/s|
DC 2|6,25 MB/s|
|DC 3|6,25 MB/s|
|CONTROLLER DI DOMINIO 4|5.75 MB/s|
|CONTROLLER DI DOMINIO 5|4,75 MB/s|
|Totale|28.5 MB/s|

**Consigliato: MB/s 72** (MB/s 28,5 diviso da 40%)

|Numero di sistemi di destinazione|Larghezza di banda totale (precedente)|
|-|-|
|2|28.5 MB/s|
|Causando un comportamento normale|28.5 &divide; 2 = 14.25 MB/sec|

Come sempre, nel corso del tempo il presupposto può accadere che aumenta il carico di client e questa crescita dovrebbe essere pianificata per le possibili. La quantità consigliata per pianificare consentirebbe per un incremento stimato del traffico di rete del 50%.

## <a name="storage"></a>Archiviazione

Pianificazione di archiviazione si intende per due componenti:

- Capacità o dimensioni di archiviazione
- Prestazioni

Viene impiegata una grande quantità di tempo e la documentazione per pianificazione della capacità, lasciando le prestazioni completamente spesso trascurata. Con i costi hardware corrente, la maggior parte degli ambienti non sono grandi sufficientemente che uno di questi è effettivamente un problema e l'indicazione di "inserire nella quantità di RAM come le dimensioni del database" riguarda in genere il resto, anche se potrebbe essere eccessivo per percorsi satellite in dimensioni maggiori ambienti.

### <a name="sizing"></a>Ridimensionamento

#### <a name="evaluating-for-storage"></a>La valutazione per l'archiviazione

Rispetto a 13 anni fa quando è stato introdotto Active Directory, una volta quando le unità di 9 GB e 4 GB erano le dimensioni delle unità più comuni, ridimensionamento per Active Directory non è in realtà presi in considerazione per tutti, ma gli ambienti più grandi. Con il più piccolo disponibile sul disco rigido dimensioni nell'intervallo di 180 GB, l'intero sistema operativo, SYSVOL e NTDS. dit possono essere inserita in una specifica unità. Di conseguenza, si consiglia di deprecare investimento ingente in quest'area.

L'unica indicazione da tenere in considerazione è per assicurarsi che sia disponibile per consentire la deframmentazione 110% delle dimensioni del DIT. Inoltre, è opportuno sistemazioni speciali per la crescita durante il ciclo di vita dell'hardware.

Il prima e più importante considerazione è la valutazione le dimensioni di database Ntds. dit e SYSVOL sarà. Queste misurazioni porterà nel disco di dimensioni fissata e allocazione di memoria RAM. A causa di () costo relativamente basso di questi componenti, la matematica non devono essere rigorosa e precisa. Contenuti su come valutare per ambienti nuovi ed esistenti sono reperibili nel [archiviazione di dati](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) serie di articoli. In particolare, vedere gli articoli seguenti:

- **Per gli ambienti esistenti &ndash;**  nella sezione intitolata "per attivare la registrazione di spazio su disco che viene liberato dalla deframmentazione" nell'articolo [limiti di archiviazione](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **Per i nuovi ambienti &ndash;**  l'articolo intitolato [le previsioni di crescita per utenti di Active Directory e unità organizzative](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > Gli articoli sono basati sulla stima delle dimensioni dei dati effettuata al momento del rilascio di Active Directory in Windows 2000. Usare dimensioni di oggetti che riflettono l'effettiva dimensione degli oggetti nell'ambiente in uso.

Quando si esaminano gli ambienti esistenti con più domini, potrebbero esserci variazioni nel database di dimensioni. In tal caso, usare il più piccolo catalogo globale (GC) e le dimensioni non GC.  

Le dimensioni del database possono variare tra le versioni del sistema operativo. Controller di dominio che eseguono sistemi operativi precedenti, ad esempio Windows Server 2003 ha una dimensione di database più piccola rispetto a un controller di dominio che esegue un sistema operativo successivo, ad esempio Windows Server 2008 R2, in particolare quando le funzionalità Cestino di questo tipo Active Directory Bin o il Roaming delle credenziali siano abilitati.

> [!NOTE]  
  >
>- Per gli ambienti di nuovo, si noti che le stime in crescita stime per gli utenti di Active Directory e unità organizzative indicano che gli 100.000 utenti (nello stesso dominio) utilizzano circa 450 MB di spazio. Si noti che gli attributi popolati possono avere un impatto notevole sull'importo totale. Gli attributi verranno compilati nel numero di oggetti da terze parti e i prodotti Microsoft, tra cui Microsoft Exchange Server e Lync. È preferibile una valutazione basata sul portfolio di prodotti nell'ambiente, ma l'esercizio di che riporta in dettaglio out i calcoli matematici e test per le stime precise per tutti, ma gli ambienti più grandi potrebbe non essere effettivamente la pena di tempo e lavoro.
>- Assicurarsi che la deframmentazione 110% delle dimensioni sono disponibile come lo spazio disponibile per consentire l'uso offline di NTDS. dit e pianificare la crescita su una durata di hardware da tre a cinque anni. Specificato come archiviazione è, la stima di archiviazione al 300% le dimensioni del DIT, come l'allocazione della memoria è sicuro per adattarsi alla crescita e la potenziale necessità di offline defrag.

#### <a name="virtualization-considerations-for-storage"></a>Considerazioni sulla virtualizzazione per l'archiviazione

In uno scenario in cui sono allocati più file disco rigido virtuale (VHD) in un singolo volume usare fisse dimensioni disco pari ad almeno % 210 (100% del DIT + 110% di spazio libero) le dimensioni del DIT per garantire che vi sia sufficiente spazio riservato.  

#### <a name="calculation-summary-example"></a>Esempio di riepilogo di calcolo

|Dati raccolti dalla fase di valutazione| |
|-|-|
|NTDS.dit size|35 GB|
|Deframmentazione di modificatore per consentire offline|2.1|
|Spazio di archiviazione totale necessario|73.5 GB|

> [!NOTE]
> Questa risorsa di archiviazione necessita è oltre l'archiviazione necessaria per SYSVOL, sistema operativo, file di paging, i file temporanei, i dati memorizzati nella cache locali (ad esempio i file di programma di installazione) e le applicazioni.

### <a name="storage-performance"></a>Prestazioni di archiviazione

#### <a name="evaluating-performance-of-storage"></a>La valutazione delle prestazioni di archiviazione

Come il componente più lento all'interno di qualsiasi computer, l'archiviazione può avere influiranno negativamente sull'esperienza client. Per questi ambienti di grandi dimensioni sufficienti per il quale i consigli di ridimensionamento di RAM non sono applicabili, le conseguenze di trascurare la pianificazione di archiviazione per le prestazioni possono essere devastante.  Inoltre, la complessità e i diversi tipi di tecnologia di archiviazione ulteriormente aumentano il rischio di errori in quanto la rilevanza dei lungo discusso di procedure consigliate "put sistema operativo, log e database" nei dischi fisici separati è limitata in scenari utili relative.  Questo avviene perché la procedura consigliata lungo discusso si basa sul presupposto che un "disco" è una stringa dedicata e ciò è consentito essere isolato dei / o.  Questa ipotesi che rendono possibile questa true non sono più rilevanti con l'introduzione di:

- Nuovi tipi di archiviazione e gli scenari di archiviazione condivisi e virtualizzato
- Condiviso assi in un provider di servizi Internet (SAN, Storage Area Network)
- File VHD in una rete SAN o NAS
- Unità a stato solido
- Architetture di archiviazione a livelli (ad esempio livello di archiviazione SSD archiviazione mandrino basati su dimensioni maggiori di memorizzazione nella cache)

In breve, al fine di tutti gli sforzi delle prestazioni di archiviazione, indipendentemente dalla progettazione e architettura di archiviazione sottostanti è per garantire la quantità necessaria di operazioni di Input/Output al secondo (IOPS) sono disponibili e che tali operazioni IOPS eseguite entro un intervallo di tempo accettabile . Questa sezione descrive come valutare le richieste di Active Directory Domain Services dell'archiviazione sottostante per garantire soluzioni di archiviazione sono progettate in modo corretto.  Data la variabilità delle tecnologie di archiviazione di oggi, è consigliabile collaborare con i fornitori di archiviazione per assicurarsi che IOPS adeguata.  Per gli scenari con archivio collegato locale, fare riferimento a appendice C per le nozioni di base di come progettare gli scenari di archiviazione locale tradizionale.  Questa entità sono generalmente applicabili ai livelli di archiviazione più complessi e risulta utile anche nella finestra di dialogo con i fornitori di soluzioni di archiviazione back-end di supporto.

- Dato l'ampia gamma di opzioni di archiviazione disponibili, è consigliabile coinvolgere l'esperienza del team di supporto di hardware o i fornitori per garantire che la soluzione specifica soddisfi le esigenze di Active Directory Domain Services. I numeri seguenti sono le informazioni che verrebbe fornite per gli specialisti di archiviazione.

Per gli ambienti in cui il database è troppo grande per essere mantenuta nella RAM, usare i contatori delle prestazioni per determinare quanto devono essere supportati i/o:

- Disco logico (\*) \Avg letture disco/sec (ad esempio, se NTDS. dit è archiviato nell'unità d / unità, il percorso completo dovrebbe essere \Avg LogicalDisk d: letture disco/sec)
- Disco logico (\*) \Avg scritture disco/sec
- Disco logico (\*) \Avg trasferimenti disco/sec
- Disco logico (\*) \Reads/sec
- LogicalDisk(\*)\Writes/sec
- Disco logico (\*) \Transfers/sec

Questi devono essere campionati in intervalli di 15 o 30/60 minuti effettuare un benchmark le esigenze dell'ambiente corrente.

#### <a name="evaluating-the-results"></a>Valutazione dei risultati

> [!NOTE]
> Lo stato attivo è su operazioni di lettura dal database poiché si tratta in genere il componente più complesse, la stessa logica può essere applicata per operazioni di scrittura nel file di log sostituendo il disco logico ( *\<NTDS Log\>* ) \Avg scritture disco/sec disco logico e ( *\<NTDS Log\>* ) \Writes/sec):
>  
> - Disco logico ( *\<NTDS\>* ) \Avg letture disco/sec indica se lo spazio di archiviazione corrente viene ridimensionato in modo adeguato.  Se i risultati sono all'incirca uguali al tempo di accesso il disco per il tipo di disco, disco logico ( *\<NTDS\>* ) \Reads/sec è una misura valida.  Verificare le specifiche del produttore per l'archiviazione nel back-end, ma gli intervalli di buona per disco logico ( *\<NTDS\>* ) \Avg letture disco/sec approssimativamente sarebbe:
>   - 7200 – 9 a 12,5 millisecondi (ms)
>   - 10.000 – da 6 a 10 ms
>   - 15.000 – 4 a 6 ms  
>   - SSD: 1 a 3 ms  
>   - >[!NOTE]
>     >Le raccomandazioni presenti che informa che le prestazioni di archiviazione sono danneggiata a 15 ms a 20 ms (a seconda di origine).  La differenza tra i valori precedenti e l'altro materiale sussidiario è che i valori sopra riportati sono l'intervallo di funzionamento normale.  Le altre raccomandazioni sono di risoluzione dei problemi di materiale sussidiario per identificare quando esperienza client comporta una riduzione e diventa evidente in modo significativo.  Riferimento appendice C per una spiegazione più approfondita.
> - Disco logico ( *\<NTDS\>* ) \Reads/sec è la quantità dei / o che deve essere eseguita.
>   - Se disco logico ( *\<NTDS\>* ) \Avg letture disco/sec è compreso nell'intervallo ottima per l'archiviazione back-end, disco logico ( *\<NTDS\>* ) \Reads/ sec utilizzabile direttamente per ridimensionare la risorsa di archiviazione.
>   - Se disco logico ( *\<NTDS\>* ) \Avg letture disco/sec non è presente all'interno dell'intervallo ottima per l'archiviazione back-end, i/o aggiuntivi è necessaria in base alla formula seguente:
>     > (Disco logico ( *\<NTDS\>* ) \Avg letture disco/sec) &divide; (ora di accesso supporto fisico su disco) &times; (disco logico ( *\<NTDS\>* ) \Avg letture disco/sec)

Considerazioni:

- Si noti che se il server è configurato con una quantità ottimale di RAM, questi valori saranno imprecisi ai fini della pianificazione.  Si sarà erroneamente sul lato superiore e può ancora essere utilizzati come peggiore delle eventualità.
- In particolare la RAM aggiunta/ottimizzazione determinerà una diminuzione nella quantità dei / o letti (disco logico ( *\<NTDS\>* ) \Reads/Sec.  Questo significa che la soluzione di archiviazione non sarà necessario essere così solido come inizialmente calcolato.  Sfortunatamente, qualsiasi elemento più specifico rispetto a questa istruzione generale per l'ambiente dipende dal carico di lavoro e linee guida generali non può essere fornito.  La soluzione migliore consiste nel modificare dimensioni di archiviazione dopo l'ottimizzazione della RAM.

#### <a name="virtualization-considerations-for-performance"></a>Considerazioni sulla virtualizzazione per le prestazioni

Analogamente a tutte le discussioni di virtualizzazione precedenti, la chiave qui è per garantire che l'infrastruttura condivisa sottostante può supportare il carico del controller di dominio e le altre risorse tramite l'oggetto sottostante condiviso media e tutti i percorsi a esso. Ciò vale se un controller di dominio fisico sta condividendo lo stesso supporto sottostante in una rete SAN, NAS o infrastruttura iSCSI come altri server o applicazioni, sia che si tratti di un utente guest usando il pass-tramite l'accesso a un'infrastruttura SAN o NAS iSCSI che condivide il sottostante supporti, o se il guest utilizza un file di disco rigido virtuale che si trova nel file multimediali condivisi in locale o un'infrastruttura SAN o NAS iSCSI. L'esercizio di pianificazione è completamente incentrato sul rendere assicurarsi che il contenuto multimediale sottostante può supportare il carico totale di tutti i consumer.

Inoltre, dalla prospettiva del guest, quanti sono i percorsi di codice aggiuntive che devono essere attraversati, vi è un impatto sulle prestazioni a dover passare attraverso un host per accedere a qualsiasi archivio. Non sorprende che test delle prestazioni di archiviazione indica che la virtualizzazione ha un impatto sulla velocità effettiva che è soggettiva per l'utilizzo del processore del sistema host (vedere l'appendice a: Criteri di ridimensionamento della CPU), che ovviamente è influenzata dalle risorse dell'host richieste dai guest. Ciò contribuisce alla virtualizzazione prendere in considerazione le esigenze di elaborazione in uno scenario virtualizzato (vedere [considerazioni sulla virtualizzazione per l'elaborazione](#virtualization-considerations-for-processing)).

Rendendola più complessa è che esistono diverse opzioni di archiviazione diversi disponibili che tutti hanno un impatto sulle prestazioni diverse. Come stima sicuro la migrazione da fisica a virtuale, utilizzare un moltiplicatore di 1.10 per regolare per diverse opzioni di archiviazione per i guest virtualizzati su Hyper-V, ad esempio pass-through archiviazione, una scheda SCSI o IDE. Le modifiche che devono essere effettuate durante il trasferimento tra gli scenari di archiviazione diversi sono irrilevanti per quanto riguarda se lo spazio di archiviazione è locale, SAN, NAS o iSCSI.

#### <a name="calculation-summary-example"></a>Esempio di riepilogo di calcolo

Rilevamento della quantità dei / o necessari per un sistema integro in condizioni operative normali:

- Disco logico ( *\<unità di Database NTDS\>* ) \Transfers/sec durante il periodo di picco 15 minuti periodo 
- Per determinare la quantità dei / o necessari per l'archiviazione in cui viene superata la capacità dell'archiviazione sottostante:
  >*Necessarie operazioni IOPS* = (disco logico ( *\<unità di Database NTDS\>* ) \Avg letture disco/sec &divide; *\<destinazione sec/lettura disco Media\>* ) &times; Disco logico ( *\<unità di Database NTDS\>* ) \Read/sec

|Contatore|Value|
|-|-|
|Disco logico effettivo ( *\<unità di Database NTDS\>* ) \Avg trasferimenti disco/sec|secondi.02 (20 millisecondi)|
|Disco logico di destinazione ( *\<unità di Database NTDS\>* ) \Avg trasferimenti disco/sec|pari a 0,1 secondi|
|Moltiplicatore per la modifica dei / o disponibili|0.02 &divide; 0.01 = 2|  
  
|Nome del valore|Value|
|-|-|
|Disco logico ( *\<unità di Database NTDS\>* ) \Transfers/sec|400|
|Moltiplicatore per la modifica dei / o disponibili|2|
|Numero totale di IOPS necessari durante il periodo di punta|800|

Per determinare la frequenza in corrispondenza del quale si desidera essere preparata la cache:

- Determinare il tempo massimo accettabile per preparare la cache. È entrambi l'intervallo di tempo che deve intraprendere per caricare l'intero database dal disco, o per scenari in cui l'intero database non è possibile caricare nella memoria RAM, questo sarà il tempo massimo per riempire RAM.
- Determinare le dimensioni del database, esclusi gli spazi vuoti.  Per altre informazioni, vedere [la valutazione per l'archiviazione](#evaluating-for-storage).  
- Dividere la dimensione del database da 8 KB. che sarà IOs totale necessario per caricare il database.
- Dividere la total IOs per il numero di secondi nell'intervallo di tempo definito.

Si noti che la velocità di calcolo, mentre accurate, non sarà esatta perché caricato in precedenza le pagine vengono rimosse se ESE non è configurato per avere una dimensione della cache predefinito e Active Directory Domain Services per impostazione predefinita Usa le dimensioni della cache di variabili.

|Punti dati da raccogliere|Valori
|-|-|
|Tempo massimo accettabile a caldo|10 minuti (600 secondi)
|Dimensioni del database|2 GB|  
  
|Passaggio di calcolo|Formula|Risultato|
|-|-|-|
|Calcolare le dimensioni del database nelle pagine|(2 GB &times; 1024 &times; 1024) = *dimensioni del database espressa in KB*|2\.097.152 KB|
|Calcolare il numero di pagine di database|KB 2.097.152 &divide; 8 KB = *numero di pagine*|262.144 pagine|
|Calcola IOPS necessarie per preparare completamente la cache|262.144 pagine &divide; 600 secondi = *IOPS necessario*|437 IOPS|

## <a name="processing"></a>Elaborazione

### <a name="evaluating-active-directory-processor-usage"></a>La valutazione dell'utilizzo del processore di Active Directory

Per la maggior parte degli ambienti, dopo l'archiviazione, RAM e rete vengono ottimizzati correttamente come descritto nella sezione pianificazione, gestire la quantità di capacità di elaborazione sarà il componente che merita particolare attenzione. La valutazione della capacità della CPU necessita sono disponibili due sfide:

- O meno le applicazioni nell'ambiente sono in corso ben progettate in un'infrastruttura di servizi condivisi ed è illustrata nella sezione intitolata "Rilevamento costosa e inefficiente ricerche" nell'articolo Creazione più efficiente Microsoft attive La directory abilitata le applicazioni o la migrazione da SAM legacy chiama alle chiamate LDAP.  
  
  In ambienti più grandi, il motivo che è importante è che le applicazioni in modo inadeguato codificate possono unità volatilità in carico della CPU, "ruba" una quantità eccessiva di tempo di CPU da altre applicazioni, unità artificialmente le esigenze di capacità e distribuire il carico in modo non uniforme i controller di dominio.  
- Come Active Directory Domain Services è un ambiente distribuito con un'ampia varietà di potenziali clienti, stimare il costo di un "client singolo" è soggettiva ambientalistica a causa di modelli di utilizzo e il tipo o la quantità delle applicazioni che sfruttano Active Directory Domain Services. In breve, molto simile alla sezione rete, per l'applicabilità ampia, questo è meglio che hanno quasi raggiunto dalla prospettiva di valutare la capacità totale necessaria nell'ambiente.

Per gli ambienti esistenti, come il ridimensionamento di archiviazione è stato illustrato in precedenza, presuppone che archiviazione ora viene ridimensionata in modo corretto e pertanto i dati riguardanti il carico del processore sono validi. Come già detto, è fondamentale per garantire che il collo di bottiglia nel sistema non sia le prestazioni della risorsa di archiviazione. Quando è presente un collo di bottiglia e il processore è in attesa, sono stati di inattività che scompare dopo la rimozione di un collo di bottiglia.  Come vengono rimossi gli stati di attesa del processore, per definizione, utilizzo della CPU aumenta perché non ha più in attesa di dati. Di conseguenza, raccogliere i contatori delle prestazioni "disco logico ( *\<unità di Database NTDS\>* ) \Avg letture disco/sec" e "Process(lsass)\\% Processor Time". I dati in "Process(lsass)\\% Processor Time" sarà artificialmente basso se "disco logico ( *\<unità di Database NTDS\>* ) \Avg letture disco/sec" supera da 10 a 15 ms, ovvero un limite generale che Il supporto tecnico Microsoft Usa per la risoluzione dei problemi di prestazioni correlati all'archiviazione. Come in precedenza, è consigliabile che gli intervalli di campionamento sia 15, 30 o 60 minuti. Un valore minore sarà in genere troppo volatile per le misurazioni valida. un valore superiore verrà smussare legge giornalieri eccessivamente.

### <a name="introduction"></a>Introduzione

Per pianificare la pianificazione della capacità per i controller di dominio, la potenza di elaborazione richiede maggiori attenzioni e conoscenza. Quando si ridimensionano i sistemi per garantire prestazioni ottimali, è sempre presente un componente che costituisce il collo di bottiglia e in un Controller di dominio ridimensionati correttamente questo sarà il processore.

Come per la sezione rete in cui la richiesta dell'ambiente viene esaminata in modo da sito a sito, lo stesso deve essere eseguito per la capacità di calcolo richiesta. Diversamente dalla sezione rete, in cui le tecnologie di rete disponibili superano a questo momento la domanda normale, prestare maggiore attenzione alle capacità di CPU di ridimensionamento.  Come tutti gli ambienti di dimensioni moderate anche; tutto il resto alcune migliaia di utenti simultanea possa inserire carico significativo sulla CPU.

Purtroppo, a causa di un enorme variabilità delle applicazioni client che sfruttano Active Directory, una stima generale degli utenti per CPU è purtroppo non applicabile a tutti gli ambienti. In particolare, le richieste di calcolo sono soggetti a profilo di comportamento e l'applicazione utente. Pertanto, ogni ambiente deve essere dimensionato singolarmente.

#### <a name="target-site-behavior-profile"></a>Profilo di comportamento del sito di destinazione

Come accennato in precedenza, quando si pianifica la capacità per un intero sito, l'obiettivo è come destinazione una progettazione con un' *N* + progettazione 1 capacità, ad esempio che un errore di un sistema durante il periodo di picco consentirà di prosecuzione di servizio in un ragionevole livello di qualità. Ciò significa che in un "*N*" scenario di carico in tutte le caselle deve essere inferiore al 100% (meglio ancora, minore dell'80%) durante i periodi di picco.

Inoltre, se le applicazioni e i client del sito Usa le procedure consigliate per l'individuazione dei controller di dominio (vale a dire usando il [DsGetDcName funzione](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), i client devono essere distribuiti in modo relativamente uniforme con minor picchi temporanei a causa di un numero qualsiasi di fattori.

Nell'esempio seguente, i presupposti seguenti:

- Ognuno dei cinque controller di dominio nel sito dispone di quattro di CPU.
- Destinazione totale della CPU durante le ore lavorative è 40% in condizioni operative normali ("*N* + 1") e in caso contrario il 60% ("*N*"). Durante la non di ufficio, l'utilizzo della CPU di destinazione è 80% perché il software di backup e altre operazioni di manutenzione dovrebbero usare tutte le risorse disponibili.

![Grafico utilizzo CPU](media/capacity-planning-considerations-cpu-chart.png)

Analisi dei dati nel grafico (processore Information(_Total)\% utilità processore) per ognuno dei controller di dominio:

- Nella maggior parte, il carico è relativamente distribuito uniformemente ovvero proprie aspettative quando i client usare DC locator e avranno sia scritta correttamente le ricerche. 
- Esistono una serie di picchi di cinque minuti del 10%, con alcune più ampio del 20%. In generale, a meno che non provocano la destinazione del piano di capacità superamento, esaminare questi non possono essere utili.  
- Il periodo di picco per tutti i sistemi è tra circa 8:00 e alle 09:00: 15. Con la transizione senza problemi da circa 5:00 tramite circa alle 17:00, si tratta in genere indicativa del ciclo aziendale. I picchi di utilizzo della CPU in uno scenario di finestra-da-box tra alle 17:00 e 4:00 AM più casuali sarebbe di fuori di problemi di pianificazione della capacità.

  >[!NOTE]
  >In un sistema ben gestito, vengono detti picchi potrebbe essere in esecuzione un software di backup, le analisi antivirus completa del sistema, inventario hardware o software, la distribuzione di software o patch e così via. Poiché si trovano all'esterno del ciclo di picco utente aziendale, le destinazioni non vengono superate.

- Come ogni sistema è circa il 40% e tutti i sistemi hanno gli stessi numeri di CPU, è necessario uno esito negativo o essere portato offline, i sistemi rimanenti verranno eseguiti un % 53 stimato (40% carico del sistema D è in modo uniforme suddividere e aggiunta al carico del 40% esistente dell'oggetto di sistema e di sistema C). Per una serie di motivi, questo presupposto lineare non è perfettamente accurata, ma fornisce abbastanza accuratezza per misurare.  

  **Scenario alternativo:** due controller di dominio in esecuzione al 40%: Ha esito negativo di un dominio controller, quello rimanente stimata CPU sarebbe un stimato di 80%. Questo molto supera le soglie descritte in precedenza per la pianificazione della capacità e inizia anche a limitare in modo significativo la quantità di spazio head per il 10% e 20% visualizzate nel profilo di load precedente, il che significa che i picchi comporterà il controller di dominio al 90% al 100% durante la "*N*"scenario e sicuramente una riduzione della velocità di risposta.

### <a name="calculating-cpu-demands"></a>Il calcolo del carico della CPU

Il "processo\\% Processor Time" contatore dell'oggetto delle prestazioni calcola la somma di quantità totale di tempo che tutti i thread di un'applicazione di spesa sulla CPU e all'ora viene diviso per l'importo totale del sistema ha superato. Di conseguenza sono che un'applicazione a thread multipli in un sistema con più CPU può superare il 100% tempo di CPU e verrebbe interpretata in modo molto diverso rispetto a "informazioni sul processore\\% utilità processore". In pratica il "Process(lsass)\\% tempo processore" può essere considerato come il numero di CPU in esecuzione al 100% che sono necessari per supportare le richieste del processo. Il valore 200% significa che 2 CPU, ognuno con al 100%, sono necessari per supportare il carico completo di dominio Active Directory. Sebbene sia una CPU in esecuzione al 100% della capacità più convenienti dalla prospettiva del denaro speso su CPU e il consumo di energia e, per una serie di motivi dettagliate nell'appendice A, maggiore velocità di risposta in un sistema multi-thread si verifica quando il sistema non è in esecuzione al 100%.

Per rispondere ai picchi temporanei nel carico di lavoro client, è consigliabile una CPU di periodo di picco di tra 40% e il 60% della capacità del sistema di destinazione. Usa l'esempio precedente, ciò significa che tra 3.33 (60% di destinazione) e 5 (40% di destinazione) CPU sarebbero necessari per il caricamento (processo lsass) di Active Directory Domain Services. Capacità aggiuntive devono essere aggiunti secondo le esigenze del sistema operativo di base e altri agenti necessari (ad esempio l'antivirus, backup, monitoraggio e così via). Sebbene l'impatto degli agenti deve essere valutata in base a una per ogni ambiente, è possibile effettuare una stima della tra 5 e 10% di una singola CPU. Nell'esempio corrente, in tale sarebbe consigliabile che tra 3.43 (60% di destinazione) e 5.1 (40% di destinazione) CPU sono necessarie durante i periodi di picco.

Il modo più semplice per eseguire questa operazione è usare la visualizzazione "Area in pila" in Windows affidabilità e Performance Monitor (perfmon), assicurandosi che tutti i contatori vengono ridimensionati lo stesso.

Presupposti:

- Obiettivo è ridurre l'impatto per i server il minor numero possibile. In teoria, un server ipotetico il carico e un server aggiuntivo aggiunto per la ridondanza (*N* 1 scenario +).

![Grafico del tempo processore per processo lsass (su tutti i processori)](media/capacity-planning-considerations-proc-time-chart.png)

Informazioni ottenute dai dati nel grafico (Process(lsass)\\% tempo processore):

- Il giorno lavorativo rispetto alle 17:00 e inizia a imparare circa 7:00.
- Il periodo più occupato di picco è compreso 9:30 e ore 11:00. 
  > [!NOTE]
  > Tutti i dati sulle prestazioni sono cronologici. Il punto dati massimo 9:15 indica il caricamento da ore 9.00 su 9:15.
- Sono presenti picchi prima 7:00 questo tipo potrebbe indicare carico da fusi orari diversi né attività di infrastruttura in background, ad esempio i backup. Perché il picco a 9:30 supera questa attività, non è rilevante.
- Esistono tre controller di dominio nel sito.

Al massimo carico, lsass utilizza circa 485% di una CPU o 4.85 CPU in esecuzione al 100%. In base ai calcoli matematici in precedenza, questo significa che il sito deve circa 12,25 CPU per Active Directory Domain Services. Componente aggiuntivo di suggerimenti precedenti del 5% al 10% per i processi in background e questo significa sostituire subito il server sarà necessario circa 12.30 a 1.235 CPU per supportare lo stesso carico. Una stima dell'ambiente per la crescita ora deve essere stato eseguito il factoring.

### <a name="when-to-tune-ldap-weights"></a>Quando ottimizzare i pesi LDAP

Esistono diversi scenari in cui Ottimizzazione [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) da considerare. All'interno del contesto di pianificazione della capacità, ciò può essere effettuato quando i carichi utente o dell'applicazione non sono bilanciati o sistemi sottostanti non sono bilanciati in modo uniforme in termini di funzionalità. Motivi per eseguire questa operazione di là la pianificazione della capacità non sono compresi nell'ambito di questo articolo.

Esistono due motivi comuni per ottimizzare i pesi LDAP:

- L'emulatore PDC è un esempio che influisce su tutti gli ambienti per la quale utente o applicazione comportamento di caricamento non è distribuito uniformemente. Poiché alcuni strumenti e le azioni come destinazione l'emulatore PDC, ad esempio di strumenti di gestione di criteri di gruppo, secondo tentativi nel caso di errori di autenticazione, stabilire una relazione di trust e così via, le risorse della CPU nell'emulatore PDC potrebbero essere molto maggiore rispetto a richieste in un punto il sito.
  - È utile solo ottimizzare questo se è presente una differenza notevole utilizzo della CPU per ridurre il carico sull'emulatore PDC e aumentare il carico in altri controller di dominio consentirà una distribuzione più uniforme del carico.
  - In questo caso, è possibile impostare LDAPSrvWeight compreso tra 50 e 75 per l'emulatore PDC.
- Server con conteggi di diversi di CPU (e velocità) in un sito.  Ad esempio sono presenti due server otto core e uno di quattro core.  L'ultimo server è la metà dei processori degli altri due server.  Ciò significa che un carico distribuito anche client comporta un aumento di carico medio della CPU nella finestra di quattro core per circa due volte tale valore delle finestre di otto core.
  - Ad esempio, le due scatole otto core potrebbero essere in esecuzione al 40% e la casella di quattro core potrebbe essere in esecuzione all'80%.
  - Inoltre, prendere in considerazione l'impatto della perdita di una casella di otto core in questo scenario, in particolare il fatto che la casella di quattro core potrebbe ora essere sottoposti a overload.

#### <a name="example-1---pdc"></a>Esempio 1: PDC

| |Utilizzo con le impostazioni predefinite|Nuovo LdapSrvWeight|Utilizzo del nuovo stimato|
|-|-|-|-|
|Controller di dominio 1 (l'emulatore PDC)|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

Catch qui è che se il ruolo emulatore PDC viene trasferito o riassegnato, in particolare in un altro controller di dominio nel sito sia un incremento significativo sul nuovo emulatore PDC.

Usando l'esempio dalla sezione [profilo di comportamento del sito di destinazione](#target-site-behavior-profile), è stato effettuato un presupposto che tutte le tre controller di dominio nel sito disponeva di quattro CPU. L'azione da eseguire, in condizioni normali, se uno dei controller di dominio dispone di otto CPU? Vi sono due controller di dominio al 40% di utilizzo e uno per 20% di utilizzo. Sebbene non sia errato, ha la possibilità di bilanciare il carico leggermente migliore. Sfruttare i pesi LDAP per eseguire questa operazione.  Uno scenario di esempio sarebbe:

#### <a name="example-2---differing-cpu-counts"></a>Esempio 2: CPU che differisce conta

| |Informazioni sul processore\\ %&nbsp;Utility(_Total) processore<br />Utilizzo con le impostazioni predefinite|Nuovo LdapSrvWeight|Utilizzo del nuovo stimato|
|-|-|-|-|
|CONTROLLER DI DOMINIO DI 4 CPU 1|40|100|30%|
|CPU 4 CONTROLLER DI DOMINIO 2|40|100|30%|
|CONTROLLER DI DOMINIO DI 8 CPU 3|20|200|30%|

Prestare molta attenzione con questi scenari tuttavia. Come si può notare in precedenza, i calcoli matematici Cerca davvero interessante e vera e propria su carta. Ma in questo articolo, la pianificazione per un "*N* + scenario 1" è di fondamentale importanza. L'impatto di un controller di dominio passerà offline deve essere calcolato per ogni scenario. Nello scenario immediatamente precedente in cui la distribuzione del carico è pari, per garantire un 60% caricare durante un "*N*" scenario, quando il carico è bilanciato in modo uniforme tra tutti i server, la distribuzione andrà bene come rimangono i rapporti coerente. Cerca nell'emulatore PDC scenario di ottimizzazione e in generale qualsiasi scenario in cui il carico utente o l'applicazione è sbilanciato, l'effetto è molto diversa:

| |Utilizzo ottimizzato|Nuovo LdapSrvWeight|Utilizzo del nuovo stimato|
|-|-|-|-|
|Controller di dominio 1 (l'emulatore PDC)|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### <a name="virtualization-considerations-for-processing"></a>Considerazioni sulla virtualizzazione per l'elaborazione

Esistono due livelli di pianificazione della capacità che devono essere eseguite in un ambiente virtualizzato. A livello di host, simile all'identificazione del ciclo aziendale descritto per il controller di dominio di elaborazione in precedenza, è necessario identificare i valori di soglia nel periodo di picco. Poiché le entità sottostante sono gli stessi per un computer host pianificazione dei thread guest sulla CPU per il recupero di thread di Active Directory Domain Services sulla CPU in un computer fisico, lo stesso risultato dal 40 al 60% dell'host sottostante sono consigliati. Al livello successivo, il livello di guest, poiché le entità di pianificazione dei thread non sono cambiati, l'obiettivo entro i guest rimangono nell'intervallo di 40 al 60%.

In uno scenario con mapping diretto, un guest per ogni host, tutti la pianificazione della capacità eseguita fino a questo punto deve essere aggiunta ai requisiti del sistema operativo host sottostante (RAM, disco, rete). In uno scenario di host condiviso, indicano che vi è impatto del 10% sull'efficienza dei processori sottostanti. Ciò significa che se un sito deve 10 CPU a una destinazione del 40%, la quantità di CPU virtuali per allocare in tutti i "*N*" gli ospiti sarebbe uguale a 11. In un sito con una distribuzione mista dei server fisici e virtuali, il modificatore si applica solo alle macchine virtuali. Se, ad esempio, un sito ha un "*N* + 1" scenario, un server fisico o il mapping diretto con 10 CPU sarebbe sull'equivalente al uno guest con 11 CPU in un host con 11 CPU riservate per il controller di dominio.

Durante l'analisi e calcolo della quantità di CPU necessari per supportare il carico di Active Directory Domain Services, i numeri di CPU, eseguire il mapping a ciò che può essere acquistata in hardware fisico di termini non sono associati correttamente. Virtualizzazione Elimina la necessità di arrotondare per eccesso. La virtualizzazione riduce lo sforzo necessario per aggiungere capacità di calcolo in un sito, assegnato la facilità con cui una CPU può essere aggiunti a una macchina virtuale. Non elimina la necessità di valutare in modo accurato la potenza di calcolo necessaria in modo che l'hardware sottostante è disponibile quando CPU aggiuntive devono essere aggiunti al guest.  Come sempre, ricordarsi di pianificare e monitorare la crescita della domanda.

### <a name="calculation-summary-example"></a>Esempio di riepilogo di calcolo

|Sistema|Picco CPU|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|CPU totale in uso|485%|  
  
|Numero di sistemi di destinazione|Larghezza di banda totale (precedente)|
|-|-|
|CPU al 40% destinazione necessaria|4.85 &divide; .4 = 12.25|

A causa dell'importanza di questo punto, ripetere *ricordarsi di pianificare la crescita*. Supponendo che il 50% di crescita nei prossimi tre anni, in questo ambiente sarà necessario 18.375 CPU (a 12,25 &times; 1.5) in corrispondenza del contrassegno di tre anni. Un piano alternativo, è possibile esaminare dopo il primo anno e componente aggiuntivo di capacità aggiuntiva in base alle esigenze.

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>Carico di autenticazione il client cross-trust per NTLM

#### <a name="evaluating-cross-trust-client-authentication-load"></a>La valutazione di carico di autenticazione il client cross-trust

Molti ambienti possono avere uno o più domini collegati da una relazione di trust. È necessario attraversare un trust tramite il canale sicuro del controller di dominio in un altro controller di dominio nel dominio di destinazione o il dominio successivo nel percorso di una richiesta di autenticazione per un'identità in un altro dominio che non usa l'autenticazione Kerberos per il dominio di destinazione. Il numero di chiamate simultanee, usando un canale protetto che un controller di dominio è possibile apportare a un controller di dominio in un dominio trusted è controllato dall'impostazione di una nota come **MaxConcurrentAPI**. Per i controller di dominio, assicurando che il canale sicuro consente di gestire la quantità di carico viene eseguita da uno dei due approcci: ottimizzazione **MaxConcurrentAPI** o, in una foresta, la creazione di trust di collegamento. Per misurare il volume di traffico in un trust singoli, fare riferimento a [come ottimizzare le prestazioni per l'autenticazione NTLM tramite l'impostazione MaxConcurrentApi](https://support.microsoft.com/kb/2688798).

Durante la raccolta dati, come con tutti gli altri scenari deve essere raccolti durante i periodi di occupato di picco del giorno per i dati siano utili.

> [!NOTE]
> Intraforest e scenari tra insiemi di strutture possono causare l'autenticazione attraversare più relazioni di trust e ogni fase devono essere ottimizzati.

#### <a name="planning"></a>Pianificazione

Esistono una serie di applicazioni che usano l'autenticazione NTLM per impostazione predefinita, o usarla in un determinato scenario di configurazione. I server applicazioni aumentano di capacità e un numero crescente di client attivi del servizio. È inoltre disponibile una tendenza che i client tenere aperto sessioni per un periodo di tempo limitato e piuttosto riconnettersi a intervalli regolari (ad esempio sincronizzazione della posta elettronica pull). Un altro esempio comune per un carico elevato NTLM è server proxy web che richiedono l'autenticazione per l'accesso a Internet.

Queste applicazioni possono causare un carico significativo per l'autenticazione NTLM, che è possibile inserire un sovraccarico significativo nei controller di dominio, soprattutto quando gli utenti e le risorse si trovano in domini diversi.

Esistono diversi approcci disponibili per la gestione delle attendibilità tra carico, che in pratica vengono usati in combinazione, anziché in esclusiva di / o uno scenario. Le opzioni possibili sono:

- Ridurre l'autenticazione client cross-trust individuando i servizi che un utente utilizza nello stesso dominio residente in cui l'utente.
- Aumentare il numero di proteggere i canali disponibili. Questo è rilevante per il traffico tra foreste e intraforest e sono note come i trust di collegamento.
- Ottimizzare le impostazioni predefinite per **MaxConcurrentAPI**.

Per l'ottimizzazione **MaxConcurrentAPI** in un server esistente, l'equazione è:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

Per altre informazioni, vedere [2688798 articolo della Knowledge Base: Come ottimizzare le prestazioni per l'autenticazione NTLM tramite l'impostazione MaxConcurrentApi](http://support.microsoft.com/kb/2688798).

## <a name="virtualization-considerations"></a>Considerazioni sulla virtualizzazione

None, questo è un sistema operativo impostazione dell'ottimizzazione.

### <a name="calculation-summary-example"></a>Esempio di riepilogo di calcolo

|Tipo di dati|Value|
|-|-|
|Semaforo acquisisce (minimo)|6,161|
|Semaforo acquisisce (massimo)|6,762|
|Timeout di semaforo|0|
|Tempo di attesa semaforo medio|0.012|
|Durata raccolta (secondi)|1:11 minuti (71 secondi)|
|Formula (da 2688798 KB)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Valore minimo per **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

Per questo sistema per questo periodo di tempo, i valori predefiniti sono accettabili.

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>Monitoraggio per la conformità con gli obiettivi di pianificazione della capacità

In questo articolo, si è stato illustrato che la pianificazione e la scalabilità proseguire le destinazioni di utilizzo. Ecco un grafico di riepilogo delle soglie consigliate che devono essere monitorati per assicurarsi che i sistemi sono operativi all'interno delle soglie di una capacità adeguata. Tenere presente che queste non sono le soglie delle prestazioni, ma le soglie di pianificazione della capacità. Un server operativo che superano queste soglie funzionerà, ma è possibile iniziare la convalida che tutte le applicazioni sono funzionava bene. Se detto applicazioni funzionano bene, è possibile iniziare la valutazione degli aggiornamenti hardware o altre modifiche alla configurazione.

|Category|Contatore delle prestazioni|Intervallo/campionamento|Destinazione|Avviso|
|-|-|-|-|-|
|Processore|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 or earlier)|Memory\Available MB|< 100 MB|N/A|< 100 MB|
|RAM (Windows Server 2012)|Memory\Long-Term Average Standby Cache Lifetime(s)|30 min|Must be tested|Must be tested|
|Network|Network Interface(\*)\Bytes Sent/sec<br /><br />Network Interface(\*)\Bytes Received/sec|30 min|40%|60%|
|Storage|LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read<br /><br />LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write|60 min|10 ms|15 ms|
|AD Services|Netlogon(\*)\Average Semaphore Hold Time|60 min|0|1 second|

## Appendix A: CPU sizing criteria

### Definitions

**Processor (microprocessor) –** a component that reads and executes program instructions  

**CPU –** Central Processing Unit  

**Multi-Core processor –** multiple CPUs on the same integrated circuit  

**Multi-CPU –** multiple CPUs, not on the same integrated circuit  

**Logical Processor –** one logical computing engine from the perspective of the operating system  

This includes hyper-threaded, one core on multi-core processor, or a single core processor.  

As today’s server systems have multiple processors, multiple multi-core processors, and hyper-threading, this information is generalized to cover both scenarios. As such, the term logical processor will be used as it represents the operating system and application perspective of the available computing engines.

### Thread-level parallelism

Each thread is an independent task, as each thread has its own stack and instructions. Because AD DS is multi-threaded and the number of available threads can be tuned by using [How to view and set LDAP policy in Active Directory by using Ntdsutil.exe](http://support.microsoft.com/kb/315071), it scales well across multiple logical processors.

### Data-level parallelism

This involves sharing data across multiple threads within one process (in the case of the AD DS process alone) and across multiple threads in multiple processes (in general). With concern to over-simplifying the case, this means that any changes to data are reflected to all running threads in all the various levels of cache (L1, L2, L3) across all cores running said threads as well as updating shared memory. Performance can degrade during write operations while all the various memory locations are brought consistent before instruction processing can continue.

### CPU speed vs. multiple-core considerations

The general rule of thumb is faster logical processors reduce the duration it takes to process a series of instructions, while more logical processors means that more tasks can be run at the same time. These rules of thumb break down as the scenarios become inherently more complex with considerations of fetching data from shared-memory, waiting on data-level parallelism, and the overhead of managing multiple threads. This is also why scalability in multi-core systems is not linear.

Consider the following analogies in these considerations: think of a highway, with each thread being an individual car, each lane being a core, and the speed limit being the clock speed.

1. If there is only one car on the highway, it doesn’t matter if there are two lanes or 12 lanes. That car is only going to go as fast as the speed limit will allow.
1. Assume that the data the thread needs is not immediately available. The analogy would be that a segment of road is shutdown. If there is only one car on the highway, it doesn’t matter what the speed limit is until the lane is reopened (data is fetched from memory).
1. As the number of cars increase, the overhead to manage the number of cars increases. Compare the experience of driving and the amount of attention necessary when the road is practically empty (such as late evening) versus when the traffic is heavy (such as mid-afternoon, but not rush hour). Also, consider the amount of attention necessary when driving on a two-lane highway, where there is only one other lane to worry about what the drivers are doing, versus a six-lane highway where one has to worry about what a lot of other drivers are doing.
   > [!NOTE]
   > The analogy about the rush hour scenario is extended in the next section: Response Time/How the System Busyness Impacts Performance.

As a result, specifics about more or faster processors become highly subjective to application behavior, which in the case of AD DS is very environmentally specific and even varies from server to server within an environment. This is why the references earlier in the article do not invest heavily in being overly precise, and a margin of safety is included in the calculations. When making budget-driven purchasing decisions, it is recommended that optimizing usage of the processors at 40% (or the desired number for the environment) occurs first, before considering buying faster processors. The increased synchronization across more processors reduces the true benefit of more processors from the linear progression (2&times; the number of processors provides less than 2&times; available additional compute power).

> [!NOTE]
> Amdahl’s Law and Gustafson’s Law are the relevant concepts here.

### Response time/How the system busyness impacts performance

Queuing theory is the mathematical study of waiting lines (queues). In queuing theory, the Utilization Law is represented by the equation:

*U* k = *B* &divide; *T*

Where *U* k is the utilization percentage, *B* is the amount of time busy, and *T* is the total time the system was observed. Translated into the context of Windows, this means the number of 100-nanosecond (ns) interval threads that are in a Running state divided by how many 100-ns intervals were available in given time interval. This is exactly the formula for calculating % Processor Utility (reference [Processor Object](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) and [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

Queuing theory also provides the formula: *N* = *U* k &divide; (1 &ndash; *U* k) to estimate the number of waiting items based on utilization ( *N* is the length of the queue). Charting this over all utilization intervals provides the following estimates to how long the queue to get on the processor is at any given CPU load.

![Queue length](media/capacity-planning-considerations-queue-length.png)

It is observed that after 50% CPU load, on average there is always a wait of one other item in the queue, with a noticeably rapid increase after about 70% CPU utilization.

Returning to the driving analogy used earlier in this section:

- The busy times of “mid-afternoon” would, hypothetically, fall somewhere into the 40% to 70% range. There is enough traffic such that one’s ability to pick any lane is not majorly restricted, and the chance of another driver being in the way, while high, does not require the level of effort to “find” a safe gap between other cars on the road.
- One will notice that as traffic approaches rush hour, the road system approaches 100% capacity. Changing lanes can become very challenging because cars are so close together that increased caution must be exercised to do so.

This is why the long term averages for capacity conservatively estimated at 40% allows for head room for abnormal spikes in load, whether said spikes transitory (such as poorly coded queries that run for a few minutes) or abnormal bursts in general load (the morning of the first day after a long weekend).

The above statement regards % Processor Time calculation being the same as the Utilization Law is a bit of a simplification for the ease of the general reader. For those more mathematically rigorous:  
- Translating the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = The number of 100-ns intervals “Idle” thread spends on the logical processor. The change in the “*X*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation
  - *T* = the total number of 100-ns intervals in a given time range. The change in the “*Y*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation.
  - *U* k = The utilization percentage of the logical processor by the “Idle Thread” or % Idle Time.  
- Working out the math:
  - *U* k = 1 – %Processor Time
  - %Processor Time = 1 – *U* k
  - %Processor Time = 1 – *B* / *T*
  - %Processor Time = 1 – *X1* – *X0* / *Y1* – *Y0*

### Applying the concepts to capacity planning

The preceding math may make determinations about the number of logical processors needed in a system seem overwhelmingly complex. This is why the approach to sizing the systems is focused on determining maximum target utilization based on current load and calculating the number of logical processors required to get there. Additionally, while logical processor speeds will have a significant impact on performance, cache efficiencies, memory coherence requirements, thread scheduling and synchronization, and imperfectly balanced client load will all have significant impacts on performance that will vary on a server-by-server basis. With the relatively cheap cost of compute power, attempting to analyze and determine the perfect number of CPUs needed becomes more an academic exercise than it does provide business value.

Forty percent is not a hard and fast requirement, it is a reasonable start. Various consumers of Active Directory require various levels of responsiveness. There may be scenarios where environments can run at 80% or 90% utilization as a sustained average, as the increased wait times for access to the processor will not noticeably impact client performance. It is important to re-iterate that there are many areas in the system that are much slower than the logical processor in the system, including access to RAM, access to disk, and transmitting the response over the network. All of these items need to be tuned in conjunction. Examples:

- Adding more processors to a system running 90% that is disk-bound is probably not going to significantly improve performance. Deeper analysis of the system will probably identify that there are a lot of threads that are not even getting on the processor because they are waiting on I/O to complete.
- Resolving the disk-bound issues potentially means that threads that were previously spending a lot of time in a waiting state will no longer be in a waiting state for I/O and there will be more competition for CPU time, meaning that the 90% utilization in the previous example will go to 100% (because it can not go higher). Both components need to be tuned in conjunction.
  > [!NOTE]
  > Processor Information(*)\\% Processor Utility can exceed 100% with systems that have a "Turbo" mode.  This is where the CPU exceeds the rated processor speed for short periods.  Reference CPU manufacturers documentation and description of the counter for greater insight.  

Discussing whole system utilization considerations also brings into the conversation domain controllers as virtualized guests. [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) applies to both the host and the guest in a virtualized scenario. This is why in a host with only one guest, a domain controller (and generally any system) has near the same performance it does on physical hardware. Adding additional guests to the hosts increases the utilization of the underlying host, thereby increasing the wait times to get access to the processors as explained previously. In short, logical processor utilization needs to be managed at both the host and at the guest levels.

Extending the previous analogies, leaving the highway as the physical hardware, the guest VM will be analogized with a bus (an express bus that goes straight to the destination the rider wants). Imagine the following four scenarios:

- It is off hours, a rider gets on a bus that is nearly empty, and the bus gets on a road that is also nearly empty. As there is no traffic to contend with, the rider has a nice easy ride and gets there just as fast as if the rider had driven instead. The rider’s travel times are still constrained by the speed limit.
- It is off hours so the bus is nearly empty but most of the lanes on the road are closed, so the highway is still congested. The rider is on an almost-empty bus on a congested road. While the rider does not have a lot of competition in the bus for where to sit, the total trip time is still dictated by the rest of the traffic outside.
- It is rush hour so the highway and the bus are congested. Not only does the trip take longer, but getting on and off the bus is a nightmare because people are shoulder to shoulder and the highway is not much better. Adding more busses (logical processors to the guest) does not mean they can fit on the road any more easily, or that the trip will be shortened.
- The final scenario, though it may be stretching the analogy a little, is where the bus is full, but the road is not congested. While the rider will still have trouble getting on and off the bus, the trip will be efficient after the bus is on the road. This is the only scenario where adding more busses (logical processors to the guest) will improve guest performance.

From there it is relatively easy to extrapolate that there are a number of scenarios in between the 0%-utilized and the 100%-utilized state of the road and the 0%- and 100%-utilized state of the bus that have varying degrees of impact.

Applying the principals above of 40% CPU as reasonable target for the host as well as the guest is a reasonable start for the same reasoning as above, the amount of queuing.

## Appendix B: Considerations regarding different processor speeds, and the effect of processor power management on processor speeds

Throughout the sections on processor selection the assumption is made that the processor is running at 100% of clock speed the entire time the data is being collected and that the replacement systems will have the same speed processors. Despite both assumptions in practice being false, particularly with Windows Server 2008 R2 and later, where the default power plan is **Balanced**, the methodology still stands as it is the conservative approach. While the potential error rate may increase, it only increases the margin of safety as processor speeds increase.

- For example, in a scenario where 11.25 CPUs are demanded, if the processors were running at half speed when the data was collected, the more accurate estimate might be 5.125 &divide; 2.
- It is impossible to guarantee that doubling the clock speeds would double the amount of processing that happens for a given time period. This is due to the fact the amount of time that the processors spend waiting on RAM or other system components could stay the same. The net effect is that the faster processors might spend a greater percentage of the time idle while waiting on data to be fetched. Again, it is recommended to stick with the lowest common denominator, being conservative, and avoid trying to calculate a potentially false level of accuracy by assuming a linear comparison between processor speeds.

Alternatively, if processor speeds in replacement hardware are lower than current hardware, it would be safe to increase the estimate of processors needed by a proportionate amount. For example, it is calculated that 10 processors are needed to sustain the load in a site, and the current processors are running at 3.3 Ghz and replacement processors will run at 2.6 Ghz, this is a 21% decrease in speed. In this case, 12 processors would be the recommended amount.

That said, this variability would not change the Capacity Management processor utilization targets. As processor clock speeds will be adjusted dynamically based on the load demanded, running the system under higher loads will generate a scenario where the CPU spends more time in a higher clock speed state, making the ultimate goal to be at 40% utilization in a 100% clock speed state at peak. Anything less than that will generate power savings as CPU speeds will be throttled back during off peak scenarios.

> [!NOTE]
> An option would be to turn off power management on the processors (setting the power plan to **High Performance**) while data is collected. That would give a more accurate representation of the CPU consumption on the target server.

To adjust estimates for different processors, it used to be safe, excluding other system bottlenecks outlined above, to assume that doubling processor speeds doubled the amount of processing that could be performed.  Today, the internal architecture of processors is different enough between processors, that a safer way to gauge the effects of using different processors than data was taken from is to leverage the SPECint_rate2006 benchmark from Standard Performance Evaluation Corporation.

1. Find the SPECint_rate2006 scores for the processor that are in use and that plan to be used.
    1. On the website of the Standard Performance Evaluation Corporation, select **Results**, highlight **CPU2006**, and select **Search all SPECint_rate2006 results**.
    1. Under **Simple Request**, enter the search criteria for the target processor, for example **Processor Matches E5-2630 (baselinetarget)** and **Processor Matches E5-2650 (baseline)**.
    1. Find the server and processor configuration to be used (or something close, if an exact match is not available) and note the value in the **Result** and **# Cores** columns.
1. To determine the modifier use the following equation:
   >((*Target platform per-core score value*) &times; (*MHz per-core of baseline platform*)) &divide; ((*Baseline per-core score value*) &times; (*MHz per-core of target platform*))  

    Using the above example:
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Multiply the estimated number of processors by the modifier.  In the above case to go from the E5-2650 processor to the E5-2630 processor multiply the calculated 11.25 CPUs &times; 0.92 = 10.35 processors needed.

## Appendix C: Fundamentals regarding the operating system interacting with storage

The queuing theory concepts outlined in [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) are also applicable to storage. Having a familiarity of how the operating system handles I/O is necessary to apply these concepts. In the Microsoft Windows operating system, a queue to hold the I/O requests is created for each physical disk. However, a clarification on physical disk needs to be made. Array controllers and SANs present aggregations of spindles to the operating system as single physical disks. Additionally, array controllers and SANs can aggregate multiple disks into one array set and then split this array set into multiple “partitions”, which is in turn presented to the operating system as multiple physical disks (ref. figure).

![Block spindles](media/capacity-planning-considerations-block-spindles.png)  

In this figure the two spindles are mirrored and split into logical areas for data storage (Data 1 and Data 2). These logical areas are viewed by the operating system as separate physical disks.

Although this can be highly confusing, the following terminology is used throughout this appendix to identify the different entities:

- **Spindle –** the device that is physically installed in the server.
- **Array –** a collection of spindles aggregated by controller.
- **Array partition –** a partitioning of the aggregated array
- **LUN –** an array, used when referring to SANs
- **Disk –** What the operating system observes to be a single physical disk.
- **Partition –** a logical partitioning of what the operating system perceives as a physical disk.

### Operating system architecture considerations

The operating system creates a First In/First Out (FIFO) I/O queue for each disk that is observed; this disk may be representing a spindle, an array, or an array partition. From the operating system perspective, with regard to handling I/O, the more active queues the better. As a FIFO queue is serialized, meaning that all I/Os issued to the storage subsystem must be processed in the order the request arrived. By correlating each disk observed by the operating system with a spindle/array, the operating system now maintains an I/O queue for each unique set of disks, thereby eliminating contention for scarce I/O resources across disks and isolating I/O demand to a single disk. As an exception, Windows Server 2008 introduces the concept of I/O prioritization, and applications designed to use the “Low” priority fall out of this normal order and take a back seat. Applications not specifically coded to leverage the “Low” priority default to “Normal.”

### Introducing simple storage subsystems

Starting with a simple example (a single hard drive inside a computer) a component-by-component analysis will be given. Breaking this down into the major storage subsystem components, the system consists of:

- **1 –** 10,000 RPM Ultra Fast SCSI HD (Ultra Fast SCSI has a 20 MB/s transfer rate)
- **1 –** SCSI Bus (the cable)
- **1 –** Ultra Fast SCSI Adapter
- **1 –** 32-bit 33 MHz PCI bus

Once the components are identified, an idea of how much data can transit the system, or how much I/O can be handled, can be calculated. Note that the amount of I/O and quantity of data that can transit the system is correlated, but not the same. This correlation depends on whether the disk I/O is random or sequential and the block size. (All data is written to the disk as a block, but different applications using different block sizes.) On a component-by-component basis:

- **The hard drive –** The average 10,000-RPM hard drive has a 7-millisecond (ms) seek time and a 3 ms access time. Seek time is the average amount of time it takes the read/write head to move to a location on the platter. Access time is the average amount of time it takes to read or write the data to disk, once the head is in the correct location. Thus, the average time for reading a unique block of data in a 10,000-RPM HD constitutes a seek and an access, for a total of approximately 10 ms (or .010 seconds) per block of data.

  When every disk access requires movement of the head to a new location on the disk, the read/write behavior is referred to as “random.” Thus, when all I/O is random, a 10,000-RPM HD can handle approximately 100 I/O per second (IOPS) (the formula is 1000 ms per second divided by 10 ms per I/O or 1000/10=100 IOPS).

  Alternatively, when all I/O occurs from adjacent sectors on the HD, this is referred to as sequential I/O. Sequential I/O has no seek time because when the first I/O is complete, the read/write head is at the start of where the next block of data is stored on the HD. Thus a 10,000-RPM HD is capable of handling approximately 333 I/O per second (1000 ms per second divided by 3 ms per I/O).

  >[!NOTE]
  >This example does not reflect the disk cache, where the data of one cylinder is typically kept. In this case, the 10 ms are needed on the first I/O and the disk reads the whole cylinder. All other sequential I/O is satisfied from the cache. As a result, in-disk caches might improve sequential I/O performance.
  
  So far, the transfer rate of the hard drive has been irrelevant. Whether the hard drive is 20 MB/s Ultra Wide or an Ultra3 160 MB/s, the actual amount of IOPS the can be handled by the 10,000-RPM HD is ~100 random or ~300 sequential I/O. As block sizes change based on the application writing to the drive, the amount of data that is pulled per I/O is different. For example, if the block size is 8 KB, 100 I/O operations will read from or write to the hard drive a total of 800 KB. However, if the block size is 32 KB, 100 I/O will read/write 3,200 KB (3.2 MB) to the hard drive. As long as the SCSI transfer rate is in excess of the total amount of data transferred, getting a “faster” transfer rate drive will gain nothing. See the following tables for comparison.

  | |7200 RPM 9ms seek, 4ms access|10,000 RPM 7ms seek, 3ms access|15,000 RPM 4ms seek, 2ms access
  |-|-|-|-|
  |Random I/O|80|100|150|
  |Sequential I/O|250|300|500|  
  
  |10,000 RPM drive|8 KB block size (Active Directory Jet)|
  |-|-|
  |Random I/O|800 KB/s|
  |Sequential I/O|2400 KB/s|

- **SCSI backplane (bus) –** Understanding how the “SCSI backplane (bus)”, or in this scenario the ribbon cable, impacts throughput of the storage subsystem depends on knowledge of the block size. Essentially the question would be, how much I/O can the bus handle if the I/O is in 8 KB blocks? In this scenario, the SCSI bus is 20 MB/s, or 20480 KB/s. 20480 KB/s divided by 8 KB blocks yields a maximum of approximately 2500 IOPS supported by the SCSI bus.

  >[!NOTE]
  >The figures in the following table represent an example. Most attached storage devices currently use PCI Express, which provides much higher throughput.  
  
  |I/O supported by SCSI bus per block size|2 KB block size|8 KB block size (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10,000|2,500|
  |40 MB/s|20,000|5,000|
  |128 MB/s|65,536|16,384|
  |320 MB/s|160,000|40,000|

  As can be determined from this chart, in the scenario presented, no matter what the use, the bus will never be a bottleneck, as the spindle maximum is 100 I/O, well below any of the above thresholds.

  >[!NOTE]
  >This assumes that the SCSI bus is 100% efficient.
  
- **SCSI adapter –** For determining the amount of I/O that this can handle, the manufacturer’s specifications need to be checked. Directing I/O requests to the appropriate device requires processing of some sort, thus the amount of I/O that can be handled is dependent on the SCSI adapter (or array controller) processor.

  In this example, the assumption that 1,000 I/O can be handled will be made.

- **PCI bus –** This is an often overlooked component. In this example, this will not be the bottleneck; however as systems scale up, it can become a bottleneck. For reference, a 32 bit PCI bus operating at 33Mhz can in theory transfer 133 MB/s of data. Following is the equation:  
  > 32 bits &divide; 8 bits per byte &times; 33 MHz = 133 MB/s.  

  Note that is the theoretical limit; in reality only about 50% of the maximum is actually reached, although in certain burst scenarios, 75% efficiency can be obtained for short periods.

  A 66Mhz 64-bit PCI bus can support a theoretical maximum of (64 bits &divide; 8 bits per byte &times; 66 Mhz) = 528 MB/sec. Additionally, any other device (such as the network adapter, second SCSI controller, and so on) will reduce the bandwidth available as the bandwidth is shared and the devices will contend for the limited resources.

After analysis of the components of this storage subsystem, the spindle is the limiting factor in the amount of I/O that can be requested, and consequently the amount of data that can transit the system. Specifically, in an AD DS scenario, this is 100 random I/O per second in 8 KB increments, for a total of 800 KB per second when accessing the Jet database. Alternatively, the maximum throughput for a spindle that is exclusively allocated to log files would suffer the following limitations: 300 sequential I/O per second in 8 KB increments, for a total of 2400 KB (2.4 MB) per second.

Now, having analyzed a simple configuration, the following table demonstrates where the bottleneck will occur as components in the storage subsystem are changed or added.

|Notes|Bottleneck analysis|Disk|Bus|Adapter|PCI bus|
|-|-|-|-|-|-|
|This is the domain controller configuration after adding a second disk. The disk configuration represents the bottleneck at 800 KB/s.|Add 1 disk (Total=2)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|200 I/Os total<br />800 KB/s total.| | | |
|After adding 7 disks, the disk configuration still represents the bottleneck at 3200 KB/s.|**Add 7 disks (Total=8)**  <br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|800 I/Os total.<br />3200 KB/s total| | | |
|After changing I/O to sequential, the network adapter becomes the bottleneck because it is limited to 1000 IOPS.|Add 7 disks (Total=8)<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD| | |2400 I/O sec can be read/written to disk, controller limited to 1000 IOPS| |
|After replacing the network adapter with a SCSI adapter that supports 10,000 IOPS, the bottleneck returns to the disk configuration.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade SCSI adapter (now supports 10,000 I/O)**|800 I/Os total.<br />3,200 KB/s total| | | |
|After increasing the block size to 32 KB, the bus becomes the bottleneck because it only supports 20 MB/s.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />**32 KB block size**<br /><br />10,000 RPM HD| |800 I/Os total. 25,600 KB/s (25 MB/s) can be read/written to disk.<br /><br />The bus only supports 20 MB/s| | |
|After upgrading the bus and adding more disks, the disk remains the bottleneck.|**Add 13 disks (Total=14)**<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade to 320 MB/s SCSI bus**|2800 I/Os<br /><br />11,200 KB/s (10.9 MB/s)| | | |
|After changing I/O to sequential, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI Adapter with 14 disks<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus|8,400 I/Os<br /><br />33,600 KB\s<br /><br />(32.8 MB\s)| | | |
|After adding faster hard drives, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />4 KB block size<br /><br />**15,000 RPM HD**<br /><br />Upgrade to 320 MB/s SCSI bus|14,000 I/Os<br /><br />56,000 KB/s<br /><br />(54.7 MB/s)| | | |
|After increasing the block size to 32 KB, the PCI bus becomes the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />**32 KB block size**<br /><br />15,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus| | | |14,000 I/Os<br /><br />448,000 KB/s<br /><br />(437 MB/s) is the read/write limit to the spindle.<br /><br />The PCI bus supports a theoretical maximum of 133 MB/s (75% efficient at best).|

### Introducing RAID

The nature of a storage subsystem does not change dramatically when an array controller is introduced; it just replaces the SCSI adapter in the calculations. What does change is the cost of reading and writing data to the disk when using the various array levels (such as RAID 0, RAID 1, or RAID 5).

In RAID 0, the data is striped across all the disks in the RAID set. This means that during a read or a write operation, a portion of the data is pulled from or pushed to each disk, increasing the amount of data that can transit the system during the same time period. Thus, in one second, on each spindle (again assuming 10,000-RPM drives), 100 I/O operations can be performed. The total amount of I/O that can be supported is N spindles times 100 I/O per second per spindle (yields 100*N I/O per second).

![Logical d: drive](media/capacity-planning-considerations-logical-d-drive.png)

In RAID 1, the data is mirrored (duplicated) across a pair of spindles for redundancy. Thus, when a read I/O operation is performed, data can be read from both of the spindles in the set. This effectively makes the I/O capacity from both disks available during a read operation. The caveat is that write operations gain no performance advantage in a RAID 1. This is because the same data needs to be written to both drives for the sake of redundancy. Though it does not take any longer, as the write of data occurs concurrently on both spindles, because both spindles are occupied duplicating the data, a write I/O operation in essence prevents two read operations from occurring. Thus, every write I/O costs two read I/O. A formula can be created from that information to determine the total number of I/O operations that are occurring:  

> *Read I/O* + 2 &times; *Write I/O* = *Total available disk I/O consumed*  

When the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array:  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total IOPS*

RAID 1+ 0, behaves exactly the same as RAID 1 regarding the expense of reading and writing. However, the I/O is now striped across each mirrored set. If  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total I/O*  

in a RAID 1 set, when a multiplicity (*N*) of RAID 1 sets are striped, the Total I/O that can be processed becomes N &times; I/O per RAID 1 set:  

> *N* &times; {*Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] } = *Total IOPS*

In RAID 5, sometimes referred to as *N* + 1 RAID, the data is striped across *N* spindles and parity information is written to the “+ 1” spindle. However, RAID 5 is much more expensive when performing a write I/O than RAID 1 or 1 + 0. RAID 5 performs the following process every time a write I/O is submitted to the array:

1. Read the old data
1. Read the old parity
1. Write the new data
1. Write the new parity

As every write I/O request that is submitted to the array controller by the operating system requires four I/O operations to complete, write requests submitted take four times as long to complete as a single read I/O. To derive a formula to translate I/O requests from the operating system perspective to that experienced by the spindles:  

> *Read I/O* + 4 &times; *Write I/O* = *Total I/O*  

Similarly in a RAID 1 set, when the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array (Note that total number of spindles does not include the “drive” lost to parity):  

> *IOPS per spindle* &times; (*Spindles* – 1) &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 4 &times; *%Writes*)] = *Total IOPS*

### Introducing SANs

Expanding the complexity of the storage subsystem, when a SAN is introduced into the environment, the basic principles outlined do not change, however I/O behavior for all of the systems connected to the SAN needs to be taken into account. As one of the major advantages in using a SAN is an additional amount of redundancy over internally or externally attached storage, capacity planning now needs to take into account fault tolerance needs. Also, more components are introduced that need to be evaluated. Breaking a SAN down into the component parts:

- SCSI or Fibre Channel hard drive
- Storage unit channel backplane
- Storage units
- Storage controller module
- SAN switch(es)
- HBA(s)
- The PCI bus

When designing any system for redundancy, additional components are included to accommodate the potential of failure. It is very important, when capacity planning, to exclude the redundant component from available resources. For example, if the SAN has two controller modules, the I/O capacity of one controller module is all that should be used for total I/O throughput available to the system. This is due to the fact that if one controller fails, the entire I/O load demanded by all connected systems will need to be processed by the remaining controller. As all capacity planning is done for peak usage periods, redundant components should not be factored into the available resources and planned peak utilization should not exceed 80% saturation of the system (in order to accommodate bursts or anomalous system behavior). Similarly, the redundant SAN switch, storage unit, and spindles should not be factored into the I/O calculations.

When analyzing the behavior of the SCSI or Fibre Channel hard drive, the method of analyzing the behavior as outlined previously does not change. Although there are certain advantages and disadvantages to each protocol, the limiting factor on a per disk basis is the mechanical limitation of the hard drive.

Analyzing the channel on the storage unit is exactly the same as calculating the resources available on the SCSI bus, or bandwidth (such as 20 MB/s) divided by block size (such as 8 KB). Where this deviates from the simple previous example is in the aggregation of multiple channels. For example, if there are 6 channels, each supporting 20 MB/s maximum transfer rate, the total amount of I/O and data transfer that is available is 100 MB/s (this is correct, it is not 120 MB/s). Again, fault tolerance is a major player in this calculation, in the event of the loss of an entire channel, the system is only left with 5 functioning channels. Thus, to ensure continuing to meet performance expectations in the event of failure, total throughput for all of the storage channels should not exceed 100 MB/s (this assumes load and fault tolerance is evenly distributed across all channels). Turning this into an I/O profile is dependent on the behavior of the application. In the case of Active Directory Jet I/O, this would correlate to approximately 12,500 I/O per second (100 MB/s &divide; 8 KB per I/O).

Next, obtaining the manufacturer’s specifications for the controller modules is required in order to gain an understanding of the throughput each module can support. In this example, the SAN has two controller modules that support 7,500 I/O each. The total throughput of the system may be 15,000 IOPS if redundancy is not desired. In calculating maximum throughput in the case of failure, the limitation is the throughput of one controller, or 7,500 IOPS. This threshold is well below the 12,500 IOPS (assuming 4 KB block size) maximum that can be supported by all of the storage channels, and thus, is currently the bottleneck in the analysis. Still for planning purposes, the desired maximum I/O to be planned for would be 10,400 I/O.

When the data exits the controller module, it transits a Fibre Channel connection rated at 1 GB/s (or 1 Gigabit per second). To correlate this with the other metrics, 1 GB/s turns into 128 MB/s (1 GB/s &divide; 8 bits/byte). As this is in excess of the total bandwidth across all channels in the storage unit (100 MB/s), this will not bottleneck the system. Additionally, as this is only one of the two channels (the additional 1 GB/s Fibre Channel connection being for redundancy), if one connection fails, the remaining connection still has enough capacity to handle all the data transfer demanded.

En route to the server, the data will most likely transit a SAN switch. As the SAN switch has to process the incoming I/O request and forward it out the appropriate port, the switch will have a limit to the amount of I/O that can be handled, however, manufacturers specifications will be required to determine what that limit is. For example, if there are two switches and each switch can handle 10,000 IOPS, the total throughput will be 20,000 IOPS. Again, fault tolerance being a concern, if one switch fails, the total throughput of the system will be 10,000 IOPS. As it is desired not to exceed 80% utilization in normal operation, using no more than 8000 I/O should be the target.

Finally, the HBA installed in the server would also have a limit to the amount of I/O that it can handle. Usually, a second HBA is installed for redundancy, but just like with the SAN switch, when calculating maximum I/O that can be handled, the total throughput of *N* &ndash; 1 HBAs is what the maximum scalability of the system is.

### Caching considerations

Caches are one of the components that can significantly impact the overall performance at any point in the storage system. Detailed analysis about caching algorithms is beyond the scope of this article; however, some basic statements about caching on disk subsystems are worth illuminating:

- Caching does improved sustained sequential write I/O as it can buffer many smaller write operations into larger I/O blocks and de-stage to storage in fewer, but larger block sizes. This will reduce total random I/O and total sequential I/O, thus providing more resource availability for other I/O.
- Caching does not improve sustained write I/O throughput of the storage subsystem. It only allows for the writes to be buffered until the spindles are available to commit the data. When all the available I/O of the spindles in the storage subsystem is saturated for long periods, the cache will eventually fill up. In order to empty the cache, enough time between bursts, or extra spindles, need to be allotted in order to provide enough I/O to allow the cache to flush.

  Larger caches only allow for more data to be buffered. This means longer periods of saturation can be accommodated.

  In a normally operating storage subsystem, the operating system will experience improved write performance as the data only needs to be written to cache. Once the underlying media is saturated with I/O, the cache will fill and write performance will return to disk speed.

- When caching read I/O, the scenario where the cache is most advantageous is when the data is stored sequentially on the disk, and the cache can read-ahead (it makes the assumption that the next sector contains the data that will be requested next).
- When read I/O is random, caching at the drive controller is unlikely to provide any enhancement to the amount of data that can be read from the disk. Any enhancement is non-existent if the operating system or application-based cache size is greater than the hardware-based cache size.

  In the case of Active Directory, the cache is only limited by the amount of RAM.

### SSD considerations

SSDs are a completely different animal than spindle-based hard disks. Yet the two key criteria remain: “How many IOPS can it handle?” and “What is the latency for those IOPS?” In comparison to spindle-based hard disks, SSDs can handle higher volumes of I/O and can have lower latencies. In general and as of this writing, while SSDs are still expensive in a cost-per-Gigabyte comparison, they are very cheap in terms of cost-per-I/O and deserve significant consideration in terms of storage performance.

Considerations:

- Both IOPS and latencies are very subjective to the manufacturer designs and in some cases have been observed to be poorer performing than spindle based technologies. In short, it is more important to review and validate the manufacturer specs drive by drive and not assume any generalities.
- IOPS types can have very different numbers depending on whether it is read or write. AD DS services, in general, being predominantly read-based, will be less affected than some other application scenarios.
- “Write endurance” – this is the concept that SSD cells will eventually wear out. Various manufacturers deal with this challenge different fashions. At least for the database drive, the predominantly read I/O profile allows for downplaying the significance of this concern as the data is not highly volatile.

### Summary

One way to think about storage is picturing household plumbing. Imagine the IOPS of the media that the data is stored on is the household main drain. When this is clogged (such as roots in the pipe) or limited (it is collapsed or too small), all the sinks in the household back up when too much water is being used (too many guests). This is perfectly analogous to a shared environment where one or more systems are leveraging shared storage on an SAN/NAS/iSCSI with the same underlying media. Different approaches can be taken to resolve the different scenarios:

- A collapsed or undersized drain requires a full scale replacement and fix. This would be similar to adding in new hardware or redistributing the systems using the shared storage throughout the infrastructure.
- A “clogged” pipe usually means identification of one or more offending problems and removal of those problems. In a storage scenario this could be storage or system level backups, synchronized antivirus scans across all servers, and synchronized defragmentation software running during peak periods.

In any plumbing design, multiple drains feed into the main drain. If anything stops up one of those drains or a junction point, only the things behind that junction point back up. In a storage scenario, this could be an overloaded switch (SAN/NAS/iSCSI scenario), driver compatibility issues (wrong driver/HBA Firmware/storport.sys combination), or backup/antivirus/defragmentation. To determine if the storage “pipe” is big enough, IOPS and I/O size needs to be measured. At each joint add them together to ensure adequate “pipe diameter.”

## Appendix D - Discussion on storage troubleshooting - Environments where providing at least as much RAM as the database size is not a viable option

It is helpful to understand why these recommendations exist so that the changes in storage technology can be accommodated. These recommendations exist for two reasons. The first is isolation of IO, such that performance issues (that is, paging) on the operating system spindle do not impact performance of the database and I/O profiles. The second is that log files for AD DS (and most databases) are sequential in nature, and spindle-based hard drives and caches have a huge performance benefit when used with sequential I/O as compared to the more random I/O patterns of the operating system and almost purely random I/O patterns of the AD DS database drive. By isolating the sequential I/O to a separate physical drive, throughput can be increased. The challenge presented by today’s storage options is that the fundamental assumptions behind these recommendations are no longer true. In many virtualized storage scenarios, such as iSCSI, SAN, NAS, and Virtual Disk image files, the underlying storage media is shared across multiple hosts, thus completely negating both the “isolation of IO” and the “sequential I/O optimization” aspects. In fact these scenarios add an additional layer of complexity in that other hosts accessing the shared media can degrade responsiveness to the domain controller.

In planning storage performance, there are three categories to consider: cold cache state, warmed cache state, and backup/restore. The cold cache state occurs in scenarios such as when the domain controller is initially rebooted or the Active Directory service is restarted and there is no Active Directory data in RAM. Warm cache state is where the domain controller is in a steady state and the database is cached. These are important to note as they will drive very different performance profiles, and having enough RAM to cache the entire database does not help performance when the cache is cold. One can consider performance design for these two scenarios with the following analogy, warming the cold cache is a “sprint” and running a server with a warm cache is a “marathon.”

For both the cold cache and warm cache scenario, the question becomes how fast the storage can move the data from disk into memory. Warming the cache is a scenario where, over time, performance improves as more queries reuse data, the cache hit rate increases, and the frequency of needing to go to disk decreases. As a result the adverse performance impact of going to disk decreases. Any degradation in performance is only transient while waiting for the cache to warm and grow to the maximum, system-dependent allowed size. The conversation can be simplified to how quickly the data can be gotten off of disk, and is a simple measure of the IOPS available to Active Directory, which is subjective to IOPS available from the underlying storage. From a planning perspective, because warming the cache and backup/restore scenarios happen on an exceptional basis, normally occur off hours, and are subjective to the load of the DC, general recommendations do not exist except in that these activities be scheduled for non-peak hours.

AD DS, in most scenarios, is predominantly read IO, usually a ratio of 90% read/10% write. Read I/O often tends to be the bottleneck for user experience, and with write IO, causes write performance to degrade. As I/O to the NTDS.dit is predominantly random, caches tend to provide minimal benefit to read IO, making it that much more important to configure the storage for read I/O profile correctly.

For normal operating conditions, the storage planning goal is minimize the wait times for a request from AD DS to be returned from disk. This essentially means that the number of outstanding and pending I/O is less than or equivalent to the number of pathways to the disk. There are a variety of ways to measure this. In a performance monitoring scenario, the general recommendation is that LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read be less than 20 ms. The desired operating threshold must be much lower, preferably as close to the speed of the storage as possible, in the 2 to 6 millisecond (.002 to .006 second) range depending on the type of storage.

Example:

![Storage latency chart](media/capacity-planning-considerations-storage-latency.png)

Analyzing the chart:

- **Green oval on the left –** The latency remains consistent at 10 ms. The load increases from 800 IOPS to 2400 IOPS. This is the absolute floor to how quickly an I/O request can be processed by the underlying storage. This is subject to the specifics of the storage solution.
- **Burgundy oval on the right –** The throughput remains flat from the exit of the green circle through to the end of the data collection while the latency continues to increase. This is demonstrating that when the request volumes exceed the physical limitations of the underlying storage, the longer the requests spend sitting in the queue waiting to be sent out to the storage subsystem.

Applying this knowledge:

- **Impact to a user querying membership of a large group –** Assume this requires reading 1 MB of data from the disk, the amount of I/O and how long it takes can be evaluated as follows:
  - Active Directory database pages are 8 KB in size. 
  - A minimum of 128 pages need to be read in from disk. 
  - Assuming nothing is cached, at the floor (10 ms) this is going to take a minimum 1.28 seconds to load the data from disk in order to return it to the client. At 20 ms, where the throughput on storage has long since maxed out and is also the recommended maximum, it will take 2.5 seconds to get the data from disk in order to return it to the end user.  
- **At what rate will the cache be warmed –** Making the assumption that the client load is going to maximize the throughput on this storage example, the cache will warm at a rate of 2400 IOPS &times; 8 KB per IO. Or, approximately 20 MB/s per second, loading about 1 GB of database into RAM every 53 seconds.

> [!NOTE]
> It is normal for short periods to observe the latencies climb when components aggressively read or write to disk, such as when the system is being backed up or when AD DS is running garbage collection. Additional head room on top of the calculations should be provided to accommodate these periodic events. The goal being to provide enough throughput to accommodate these scenarios without impacting normal function.

As can be seen, there is a physical limit based on the storage design to how quickly the cache can possibly warm. What will warm the cache are incoming client requests up to the rate that the underlying storage can provide. Running scripts to “pre-warm” the cache during peak hours will provide competition to load driven by real client requests. That can adversely affect delivering data that clients need first because, by design, it will generate competition for scarce disk resources as artificial attempts to warm the cache will load data that is not relevant to the clients contacting the DC.Processore Information(_Total)\\' utilità di processore Domain Services
description: Detailed discussion of the factors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>After eliminating storage as a bottleneck, address the amount of compute power needed.</li><li>While not perfectly linear, the number of processor cores consumed across all servers within a specific scope (such as a site) can be used to gauge how many processors are necessary to support the total client load. Add the minimum necessary to maintain the current level of service across all the systems within the scope.</li><li>Changes in processor speed, including power management related changes, impact numbers derived from the current environment. Generally, it is impossible to precisely evaluate how going from a 2.5 GHz processor to a 3 GHz processor will reduce the number of CPUs needed.</li></ul>|
|NetLogon|<ul><li>“Netlogon(\*)\Semaphore Acquires”</li><li>“Netlogon(\*)\Semaphore Timeouts”</li><li>“Netlogon(\*)\Average Semaphore Hold Time”</li></ul>|<ul><li>Net Logon Secure Channel/MaxConcurrentAPI only affects environments with NTLM authentications and/or PAC Validation. PAC Validation is on by default in operating system versions before Windows Server 2008. This is a client setting, so the DCs will be impacted until this is turned off on all client systems.</li><li>Environments with significant cross trust authentication, which includes intra-forest trusts, have greater risk if not sized properly.</li><li>Server consolidations will increase concurrency of cross-trust authentication.</li><li>Surges need to be accommodated, such as cluster fail-overs, as users re-authenticate en masse to the new cluster node.</li><li>Individual client systems (such as a cluster) might need tuning too.</li></ul>|

## Planning

For a long time, the community’s recommendation for sizing AD DS has been to “put in as much RAM as the database size.” For the most part, that recommendation is all that most environments needed to be concerned about. But the ecosystem consuming AD DS has gotten much bigger, as have the AD DS environments themselves, since its introduction in 1999. Although the increase in compute power and the switch from x86 architectures to x64 architectures has made the subtler aspects of sizing for performance irrelevant to a larger set of customers running AD DS on physical hardware, the growth of virtualization has reintroduced the tuning concerns to a larger audience than before.

The following guidance is thus about how to determine and plan for the demands of Active Directory as a service regardless of whether it is deployed in a physical, a virtual/physical mix, or a purely virtualized scenario. As such, we will break down the evaluation to each of the four main components: storage, memory, network, and processor. In short, in order to maximize performance on AD DS, the goal is to get as close to processor bound as possible.

## RAM

Simply, the more that can be cached in RAM, the less it is necessary to go to disk. To maximize the scalability of the server the minimum amount of RAM should be the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). An additional amount should be added to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth based on environmental changes.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage  section to ensure that storage is properly designed.

A corollary that comes up in the general context in sizing memory is sizing of the page file. In the same context as everything else memory related, the goal is to minimize going to the much slower disk. Thus the question should go from, “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Evaluating

The amount of RAM that a domain controller (DC) needs is actually a complex exercise for these reasons:

- High potential for error when trying to use an existing system to gauge how much RAM is needed as LSASS will trim under memory pressure conditions, artificially deflating the need.
- The subjective fact that an individual DC only needs to cache what is “interesting” to its clients. This means that the data that needs to be cached on a DC in a site with only an Exchange server will be very different than the data that needs to be cached on a DC that only authenticates users.
- The labor to evaluate RAM for each DC on a case-by-case basis is prohibitive and changes as the environment changes.
- The criteria behind the recommendation will help to make informed decisions: 
- The more that can be cached in RAM, the less it is necessary to go to disk. 
- Storage is by far the slowest component of a computer. Access to data on spindle-based and SSD storage media is on the order of 1,000,000x slower than access to data in RAM.

Thus, in order to maximize the scalability of the server, the minimum amount of RAM is the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). Add additional amounts to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth. However, for satellite locations with a small set of end users, these requirements can be relaxed as these sites will not need to cache as much to service most of the requests.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.

> [!NOTE]
> A corollary while sizing memory is sizing of the page file. Because the goal is to minimize going to the much slower disk, the question goes from “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Virtualization considerations for RAM

Avoid memory over-commit at the host. The fundamental goal behind optimizing the amount of RAM is to minimize the amount of time spent going to disk. In virtualization scenarios, the concept of memory over-commit exists where more RAM is allocated to the guests then exists on the physical machine. This in and of itself is not a problem. It becomes a problem when the total memory actively used by all the guests exceeds the amount of RAM on the host and the underlying host starts paging. Performance becomes disk-bound in cases where the domain controller is going to the NTDS.dit to get data, or the domain controller is going to the page file to get data, or the host is going to disk to get data that the guest thinks is in RAM.

### Calculation summary example

|Component|Estimated memory (example)|
|-|-|
|Base operating system recommended RAM (Windows Server 2008)|2 GB|
|LSASS internal tasks|200 MB|
|Monitoring agent|100 MB|
|Antivirus|100 MB|
|Database (Global Catalog)|8.5 GB are you sure ???|
|Cushion for backup to run, administrators to log on without impact|1 GB|
|Total|12 GB|

**Recommended: 16 GB**

Over time, the assumption can be made that more data will be added to the database and the server will probably be in production for 3 to 5 years. Based on an estimate of growth of 33%, 16 GB would be a reasonable amount of RAM to put in a physical server. In a virtual machine, given the ease with which settings can be modified and RAM can be added to the VM, starting at 12 GB with the plan to monitor and upgrade in the future is reasonable.

## Network

### Evaluating
This section is less about evaluating the demands regarding replication traffic, which is focused on traffic traversing the WAN and is thoroughly covered in [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), than it is about evaluating total bandwidth and network capacity needed, inclusive of client queries, Group Policy applications, and so on. For existing environments, this can be collected by using performance counters “Network Interface(\*)\Bytes Received/sec,” and “Network Interface(\*)\Bytes Sent/sec.” Sample intervals for Network Interface counters in either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

> [!NOTE]
> Generally, the majority of network traffic on a DC is outbound as the DC responds to client queries. This is the reason for the focus on outbound traffic, though it is recommended to evaluate each environment for inbound traffic also. The same approaches can be used to address and review inbound network traffic requirements. For more information, see Knowledge Base article [929851: The default dynamic port range for TCP/IP has changed in Windows Vista and in Windows Server 2008](http://support.microsoft.com/kb/929851).

### Bandwidth needs

Planning for network scalability covers two distinct categories: the amount of traffic and the CPU load from the network traffic. Each of these scenarios is straight-forward compared to some of the other topics in this article.

In evaluating how much traffic must be supported, there are two unique categories of capacity planning for AD DS in terms of network traffic. The first is replication traffic that traverses between domain controllers and is covered thoroughly in the reference [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) and is still relevant to current versions of AD DS. The second is the intrasite client-to-server traffic. One of the simpler scenarios to plan for, intrasite traffic predominantly receives small requests from clients relative to the large amounts of data sent back to the clients. 100 MB will generally be adequate in environments up to 5,000 users per server, in a site. Using a 1 GB network adapter and Receive Side Scaling (RSS) support is recommended for anything above 5,000 users. To validate this scenario, particularly in the case of server consolidation scenarios, look at Network Interface(\*)\Bytes/sec across all the DCs in a site, add them together, and divide by the target number of domain controllers to ensure that there is adequate capacity. The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (formerly known as Perfmon), making sure all of the counters are scaled the same.

Consider the following example (also known as, a really, really complex way to validate that the general rule is applicable to a specific environment). The following assumptions are made:

- The goal is to reduce the footprint to as few servers as possible. Ideally, one server will carry the load and an additional server is deployed for redundancy (*N* + 1 scenario). 
- In this scenario, the current network adapter supports only 100 MB and is in a switched environment.  
  The maximum target network bandwidth utilization is 60% in an N scenario (loss of a DC).
- Each server has about 10,000 clients connected to it.

Knowledge gained from the data in the chart (Network Interface(\*)\Bytes Sent/sec):

1. The business day starts ramping up around 5:30 and winds down at 7:00 PM.
1. The peak busiest period is from 8:00 AM to 8:15 AM, with greater than 25 Bytes sent/sec on the busiest DC.  
   > [!NOTE]
   > All performance data is historical. So the peak data point at 8:15 indicates the load from 8:00 to 8:15.
1. There are spikes before 4:00 AM, with more than 20 Bytes sent/sec on the busiest DC, which could indicate either load from different time zones or background infrastructure activity, such as backups. Since the peak at 8:00 AM exceeds this activity, it is not relevant.
1. There are five Domain Controllers in the site.
1. The max load is about 5.5 MB/s per DC, which represents 44% of the 100 MB connection. Using this data, it can be estimated that the total bandwidth needed between 8:00 AM and 8:15 AM is 28 MB/s.
   >[!NOTE]
   >Be careful with the fact that Network Interface sent/receive counters are in bytes and network bandwidth is measured in bits. 100 MB &divide; 8 = 12.5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusions:

1. This current environment does meet the N+1 level of fault tolerance at 60% target utilization. Taking one system offline will shift the bandwidth per server from about 5.5 MB/s (44%) to about 7 MB/s (56%).
1. Based on the previously stated goal of consolidating to one server, this both exceeds the maximum target utilization and theoretically the possible utilization of a 100 MB connection.
1. With a 1 GB connection this will represent 22% of the total capacity.
1. Under normal operating conditions in the *N* + 1 scenario, client load will be relatively evenly distributed at about 14 MB/s per server or 11% of total capacity.
1. To ensure that capacity is adequate during unavailability of a DC, the normal operating targets per server would be about 30% network utilization or 38 MB/s per server. Failover targets would be 60% network utilization or 72 MB/s per server.  
  
In short, the final deployment of systems must have a 1 GB network adapter and be connected to a network infrastructure that will support said load. A further note is that given the amount of network traffic generated, the CPU load from network communications can have a significant impact and limit the maximum scalability of AD DS. This same process can be used to estimate the amount of inbound communication to the DC. But given the predominance of outbound traffic relative to inbound traffic, it is an academic exercise for most environments. Ensuring hardware support for RSS is important in environments with greater than 5,000 users per server. For scenarios with high network traffic, balancing of interrupt load can be a bottleneck. This can be detected by Processor(\*)\% Interrupt Time being unevenly distributed across CPUs. RSS enabled NICs can mitigate this limitation and increase scalability.

> [!NOTE]
> A similar approach can be used to estimate the additional capacity necessary when consolidating data centers, or retiring a domain controller in a satellite location. Simply collect the outbound and inbound traffic to clients and that will be the amount of traffic that will now be present on the WAN links.  
>  
> In some cases, you might experience more traffic than expected because traffic is slower, such as when certificate checking fails to meet aggressive time-outs on the WAN. For this reason, WAN sizing and utilization should be an iterative, ongoing process.

### Virtualization considerations for network bandwidth

It is easy to make recommendations for a physical server: 1 GB for servers supporting greater than 5000 users. Once multiple guests start sharing an underlying virtual switch infrastructure, additional attention is necessary to ensure that the host has adequate network bandwidth to support all the guests on the system, and thus requires the additional rigor. This is nothing more than an extension of ensuring the network infrastructure into the host machine. This is regardless whether the network is inclusive of the domain controller running as a virtual machine guest on a host with the network traffic going over a virtual switch, or whether connected directly to a physical switch. The virtual switch is just one more component where the uplink needs to support the amount of data being transmitted. Thus the physical host physical network adapter linked to the switch should be able to support the DC load plus all other guests sharing the virtual switch connected to the physical network adapter.

### Calculation summary example

|System|Peak bandwidth|
|-|-|
DC 1|6.5 MB/s|
DC 2|6.25 MB/s|
|DC 3|6.25 MB/s|
|DC 4|5.75 MB/s|
|DC 5|4.75 MB/s|
|Total|28.5 MB/s|

**Recommended: 72 MB/s** (28.5 MB/s divided by 40%)

|Target system(s) count|Total bandwidth (from above)|
|-|-|
|2|28.5 MB/s|
|Resulting normal behavior|28.5 &divide; 2 = 14.25 MB/s|

As always, over time the assumption can be made that client load will increase and this growth should be planned for as best as possible. The recommended amount to plan for would allow for an estimated growth in network traffic of 50%.

## Storage

Planning storage constitutes two components:

- Capacity, or storage size
- Performance

A great amount of time and documentation is spent on planning capacity, leaving performance often completely overlooked. With current hardware costs, most environments are not large enough that either of these is actually a concern, and the recommendation to “put in as much RAM as the database size” usually covers the rest, though it may be overkill for satellite locations in larger environments.

### Sizing

#### Evaluating for storage

Compared to 13 years ago when Active Directory was introduced, a time when 4 GB and 9 GB drives were the most common drive sizes, sizing for Active Directory is not even a consideration for all but the largest environments. With the smallest available hard drive sizes in the 180 GB range, the entire operating system, SYSVOL, and NTDS.dit can easily fit on one drive. As such, it is recommended to deprecate heavy investment in this area.

The only recommendation for consideration is to ensure that 110% of the NTDS.dit size is available in order to enable defrag. Additionally, accommodations for growth over the life of the hardware should be made.

The first and most important consideration is evaluating how large the NTDS.dit and SYSVOL will be. These measurements will lead into sizing both fixed disk and RAM allocation. Due to the (relatively) low cost of these components, the math does not need to be rigorous and precise. Content about how to evaluate this for both existing and new environments can be found in the [Data Storage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) series of articles. Specifically, refer to the following articles:

- **For existing environments &ndash;** The section titled “To activate logging of disk space that is freed by defragmentation” in the article [Storage Limits](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **For new environments &ndash;** The article titled [Growth Estimates for Active Directory Users and Organizational Units](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > The articles are based on data size estimates made at the time of the release of Active Directory in Windows 2000. Use object sizes that reflect the actual size of objects in your environment.

When reviewing existing environments with multiple domains, there may be variations in database sizes. Where this is true, use the smallest global catalog (GC) and non-GC sizes.  

The database size can vary between operating system versions. DCs that run earlier operating systems such as Windows Server 2003 has a smaller database size than a DC that runs a later operating system such as Windows Server 2008 R2, especially when features such Active Directory Recycle Bin or Credential Roaming are enabled.

> [!NOTE]  
  >
>- For new environments, notice that the estimates in Growth Estimates for Active Directory Users and Organizational Units indicate that 100,000 users (in the same domain) consume about 450 MB of space. Please note that the attributes populated can have a huge impact on the total amount. Attributes will be populated on many objects by both third-party and Microsoft products, including Microsoft Exchange Server and Lync. An evaluation based on the portfolio of the products in the environment is preferred, but the exercise of detailing out the math and testing for precise estimates for all but the largest environments may not actually be worth significant time and effort.
>- Ensure that 110% of the NTDS.dit size is available as free space in order to enable offline defrag, and plan for growth over a three to five year hardware lifespan. Given how cheap storage is, estimating storage at 300% the size of the DIT as storage allocation is safe to accommodate growth and the potential need for offline defrag.

#### Virtualization considerations for storage

In a scenario where multiple Virtual Hard Disk (VHD) files are being allocated on a single volume use a fixed sized disk of at least 210% (100% of the DIT + 110% free space) the size of the DIT to ensure that there is adequate space reserved.  

#### Calculation summary example

|Data collected from evaluation phase| |
|-|-|
|NTDS.dit size|35 GB|
|Modifier to allow for offline defrag|2.1|
|Total storage needed|73.5 GB|

> [!NOTE]
> This storage needed is in addition to the storage needed for SYSVOL, operating system, page file, temporary files, local cached data (such as installer files), and applications.

### Storage performance

#### Evaluating performance of storage

As the slowest component within any computer, storage can have the biggest adverse impact on client experience. For those environments large enough for which the RAM sizing recommendations are not feasible, the consequences of overlooking planning storage for performance can be devastating.  Also, the complexities and varieties of storage technology further increase the risk of failure as the relevance of long standing best practices of “put operating system, logs, and database” on separate physical disks is limited in it’s useful scenarios.  This is because the long standing best practice is based on the assumption that is that a “disk” is a dedicated spindle and this allowed I/O to be isolated.  This assumptions that make this true are no longer relevant with the introduction of:

- New storage types and virtualized and shared storage scenarios
- Shared spindles on a Storage Area Network (SAN)
- VHD file on a SAN or network-attached storage
- Solid State Drives
- Tiered storage architectures (i.e. SSD storage tier caching larger spindle based storage)

In short, the end goal of all storage performance efforts, regardless of underlying storage architecture and design, is to ensure the needed amount of Input/Output Operations per Second (IOPS) are available and that those IOPS happen within an acceptable time frame. This section covers how to evaluate what AD DS demands of the underlying storage in order to ensure storage solutions are properly designed.  Given the variability of today’s storage technologies, it is best to work with the storage vendors to ensure adequate IOPS.  For those scenarios with locally attached storage, reference the Appendix C for the basics in how to design traditional local storage scenarios.  This principals are generally applicable to more complex storage tiers and will also help in dialog with the vendors supporting backend storage solutions.

- Given the wide breadth of storage options available, it is recommended to engage the expertise of hardware support teams or vendors to ensure that the specific solution meets the needs of AD DS. The following numbers are the information that would be provided to the storage specialists.

For environments where the database is too large to be held in RAM, use the performance counters to determine how much I/O needs to be supported:

- LogicalDisk(\*)\Avg Disk sec/Read (for example, if NTDS.dit is stored on the D:/ drive, the full path would be LogicalDisk(D:)\Avg Disk sec/Read)
- LogicalDisk(\*)\Avg Disk sec/Write
- LogicalDisk(\*)\Avg Disk sec/Transfer
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

These should be sampled in 15/30/60 minute intervals to benchmark the demands of the current environment.

#### Evaluating the results

> [!NOTE]
> The focus is on reads from the database as this is usually the most demanding component, the same logic can be applied to writes to the log file by substituting LogicalDisk(*\<NTDS Log\>*)\Avg Disk sec/Write and LogicalDisk(*\<NTDS Log\>*)\Writes/sec):
>  
> - LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read indicates whether or not the current storage is adequately sized.  If the results are roughly equal to the Disk Access Time for the disk type, LogicalDisk(*\<NTDS\>*)\Reads/sec is a valid measure.  Check the manufacturer specifications for the storage on the back end, but good ranges for LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read would roughly be:
>   - 7200 – 9 to 12.5 milliseconds (ms)
>   - 10,000 – 6 to 10 ms
>   - 15,000 – 4 to 6 ms  
>   - SSD – 1 to 3 ms  
>   - >[!NOTE]
>     >Recommendations exist stating that storage performance is degraded at 15ms to 20ms (depending on source).  The difference between the above values and the other guidance is that the above values are the normal operating range.  The other recommendations are troubleshooting guidance to identify when client experience significantly degrades and becomes noticeable.  Reference Appendix C for a deeper explanation.
> - LogicalDisk(*\<NTDS\>*)\Reads/sec is the amount of I/O that is being performed.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is within the optimal range for the backend storage, LogicalDisk(*\<NTDS\>*)\Reads/sec can be used directly to size the storage.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is not within the optimal range for the backend storage, additional I/O is needed according to the following formula:
>     > (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read) &divide; (Physical Media Disk Access Time) &times; (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read)

Considerations:

- Note that if the server is configured with a sub-optimal amount of RAM, these values will be inaccurate for planning purposes.  They will be erroneously on the high side and can still be used as a worst case scenario.
- Adding/Optimizing RAM specifically will drive a decrease in the amount of read I/O (LogicalDisk(*\<NTDS\>*)\Reads/Sec.  This means the storage solution may not have to be as robust as initially calculated.  Unfortunately, anything more specific than this general statement is environmentally dependent on client load and general guidance cannot be provided.  The best option is to adjust storage sizing after optimizing RAM.

#### Virtualization considerations for performance

Similar to all of the preceding virtualization discussions, the key here is to ensure that the underlying shared infrastructure can support the DC load plus the other resources using the underlying shared media and all pathways to it. This is true whether a physical domain controller is sharing the same underlying media on a SAN, NAS, or iSCSI infrastructure as other servers or applications, whether it is a guest using pass through access to a SAN, NAS, or iSCSI infrastructure that shares the underlying media, or if the guest is using a VHD file that resides on shared media locally or a SAN, NAS, or iSCSI infrastructure. The planning exercise is all about making sure that the underlying media can support the total load of all consumers.

Also, from a guest perspective, as there are additional code paths that must be traversed, there is a performance impact to having to go through a host to access any storage. Not surprisingly, storage performance testing indicates that the virtualizing has an impact on throughput that is subjective to the processor utilization of the host system (see Appendix A: CPU Sizing Criteria), which is obviously influenced by the resources of the host demanded by the guest. This contributes to the virtualization considerations regarding processing needs in a virtualized scenario (see [Virtualization considerations for processing](#virtualization-considerations-for-processing)).

Making this more complex is that there are a variety of different storage options that are available that all have different performance impacts. As a safe estimate when migrating from physical to virtual, use a multiplier of 1.10 to adjust for different storage options for virtualized guests on Hyper-V, such as pass-through storage, SCSI Adapter, or IDE. The adjustments that need to be made when transferring between the different storage scenarios are irrelevant as to whether the storage is local, SAN, NAS, or iSCSI.

#### Calculation summary example

Determining the amount of I/O needed for a healthy system under normal operating conditions:

- LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec during the peak period 15 minute period 
- To determine the amount of I/O needed for storage where the capacity of the underlying storage is exceeded:
  >*Needed IOPS* = (LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read &divide; *\<Target Avg Disk sec/Read\>*) &times; LogicalDisk(*\<NTDS Database Drive\>*)\Read/sec

|Counter|Value|
|-|-|
|Actual LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.02 seconds (20 milliseconds)|
|Target LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.01 seconds|
|Multiplier for change in available IO|0.02 &divide; 0.01 = 2|  
  
|Value name|Value|
|-|-|
|LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec|400|
|Multiplier for change in available IO|2|
|Total IOPS needed during peak period|800|

To determine the rate at which the cache is desired to be warmed:

- Determine the maximum acceptable time to warm the cache. It is either the amount of time that it should take to load the entire database from disk, or for scenarios where the entire database cannot be loaded in RAM, this would be the maximum time to fill RAM.
- Determine the size of the database, excluding white space.  For more information, see [Evaluating for storage](#evaluating-for-storage).  
- Divide the database size by 8 KB; that will be the total IOs necessary to load the database.
- Divide the total IOs by the number of seconds in the defined time frame.

Note that the rate calculated, while accurate, will not be exact because previously loaded pages are evicted if ESE is not configured to have a fixed cache size, and AD DS by default uses variable cache size.

|Data points to collect|Values
|-|-|
|Maximum acceptable time to warm|10 minutes (600 seconds)
|Database size|2 GB|  
  
|Calculation step|Formula|Result|
|-|-|-|
|Calculate size of database in pages|(2 GB &times; 1024 &times; 1024) = *Size of database in KB*|2,097,152 KB|
|Calculate number of pages in database|2,097,152 KB &divide; 8 KB = *Number of pages*|262,144 pages|
|Calculate IOPS necessary to fully warm the cache|262,144 pages &divide; 600 seconds = *IOPS needed*|437 IOPS|

## Processing

### Evaluating Active Directory processor usage

For most environments, after storage, RAM, and networking are properly tuned as described in the Planning section, managing the amount of processing capacity will be the component that deserves the most attention. There are two challenges in evaluating CPU capacity needed:

- Whether or not the applications in the environment are being well-behaved in a shared services infrastructure, and is discussed in the section titled “Tracking Expensive and Inefficient Searches” in the article Creating More Efficient Microsoft Active Directory-Enabled Applications or migrating away from down-level SAM calls to LDAP calls.  
  
  In larger environments, the reason this is important is that poorly coded applications can drive volatility in CPU load, “steal” an inordinate amount of CPU time from other applications, artificially drive up capacity needs, and unevenly distribute load against the DCs.  
- As AD DS is a distributed environment with a large variety of potential clients, estimating the expense of a “single client” is environmentally subjective due to usage patterns and the type or quantity of applications leveraging AD DS. In short, much like the networking section, for broad applicability, this is better approached from the perspective of evaluating the total capacity needed in the environment.

For existing environments, as storage sizing was discussed previously, the assumption is made that storage is now properly sized and thus the data regarding processor load is valid. To reiterate, it is critical to ensure that the bottleneck in the system is not the performance of the storage. When a bottleneck exists and the processor is waiting, there are idle states that will go away once the bottleneck is removed.  As processor wait states are removed, by definition, CPU utilization increases as it no longer has to wait on the data. Thus, collect performance counters “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” and “Process(lsass)\\% Processor Time”. The data in “Process(lsass)\\% Processor Time” will be artificially low if “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” exceeds 10 to 15 ms, which is a general threshold that Microsoft support uses for troubleshooting storage-related performance issues. As before, it is recommended that sample intervals be either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

### Introduction

In order to plan capacity planning for domain controllers, processing power requires the most attention and understanding. When sizing systems to ensure maximum performance, there is always a component that is the bottleneck and in a properly sized Domain Controller this will be the processor.

Similar to the networking section where the demand of the environment is reviewed on a site-by-site basis, the same must be done for the compute capacity demanded. Unlike the networking section, where the available networking technologies far exceed the normal demand, pay more attention to sizing CPU capacity.  As any environment of even moderate size; anything over a few thousand concurrent users can put significant load on the CPU.

Unfortunately, due to the huge variability of client applications that leverage AD, a general estimate of users per CPU is woefully inapplicable to all environments. Specifically, the compute demands are subject to user behavior and application profile. Therefore, each environment needs to be individually sized.

#### Target site behavior profile

As mentioned previously, when planning capacity for an entire site, the goal is to target a design with an *N* + 1 capacity design, such that failure of one system during the peak period will allow for continuation of service at a reasonable level of quality. That means that in an “*N*” scenario, load across all the boxes should be less than 100% (better yet, less than 80%) during the peak periods.

Additionally, if the applications and clients in the site are using best practices for locating domain controllers (that is, using the [DsGetDcName function](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), the clients should be relatively evenly distributed with minor transient spikes due to any number of factors.

In the next example, the following assumptions are made:

- Each of the five DCs in the site has four of CPUs.
- Total target CPU usage during business hours is 40% under normal operating conditions (“*N* + 1”) and 60% otherwise (“*N*”). During non-business hours, the target CPU usage is 80% because backup software and other maintenance are expected to consume all available resources.

![CPU usage chart](media/capacity-planning-considerations-cpu-chart.png)

Analyzing the data in the chart (Processor Information(_Total)\% Processor Utility) for each of the DCs:

- For the most part, the load is relatively evenly distributed which is what would be expected when clients use DC locator and have well written searches. 
- There are a number of five-minute spikes of 10%, with some as large as 20%. Generally, unless they cause the capacity plan target to be exceeded, investigating these is not worthwhile.  
- The peak period for all systems is between about 8:00 AM and 9:15 AM. With the smooth transition from about 5:00 AM through about 5:00 PM, this is generally indicative of the business cycle. The more randomized spikes of CPU usage on a box-by-box scenario between 5:00 PM and 4:00 AM would be outside of the capacity planning concerns.

  >[!NOTE]
  >On a well-managed system, said spikes are might be backup software running, full system antivirus scans, hardware or software inventory, software or patch deployment, and so on. Because they fall outside the peak user business cycle, the targets are not exceeded.

- As each system is about 40% and all systems have the same numbers of CPUs, should one fail or be taken offline, the remaining systems would run at an estimated 53% (System D's 40% load is evenly split and added to System A's and System C’s existing 40% load). For a number of reasons, this linear assumption is NOT perfectly accurate, but provides enough accuracy to gauge.  

  **Alternate scenario –** Two domain controllers running at 40%: One domain controller fails, estimated CPU on the remaining one would be an estimated 80%. This far exceeds the thresholds outlined above for capacity plan and also starts to severely limit the amount of head room for the 10% to 20% seen in the load profile above, which means that the spikes would drive the DC to 90% to 100% during the “*N*” scenario and definitely degrade responsiveness.

### Calculating CPU demands

The “Process\\% Processor Time” performance object counter sums the total amount of time that all of the threads of an application spend on the CPU and divides by the total amount of system time that has passed. The effect of this is that a multi-threaded application on a multi-CPU system can exceed 100% CPU time, and would be interpreted VERY differently than “Processor Information\\% Processor Utility”. In practice the “Process(lsass)\\% Processor Time” can be viewed as the count of CPUs running at 100% that are necessary to support the process’s demands. A value of 200% means that 2 CPUs, each at 100%, are needed to support the full AD DS load. Although a CPU running at 100% capacity is the most cost efficient from the perspective of money spent on CPUs and power and energy consumption, for a number of reasons detailed in Appendix A, better responsiveness on a multi-threaded system occurs when the system is not running at 100%.

To accommodate transient spikes in client load, it is recommended to target a peak period CPU of between 40% and 60% of system capacity. Working with the example above, that would mean that between 3.33 (60% target) and 5 (40% target) CPUs would be needed for the AD DS (lsass process) load. Additional capacity should be added in according to the demands of the base operating system and other agents required (such as antivirus, backup, monitoring, and so on). Although the impact of agents needs to be evaluated on a per environment basis, an estimate of between 5% and 10% of a single CPU can be made. In the current example, this would suggest that between 3.43 (60% target) and 5.1 (40% target) CPUs are necessary during peak periods.

The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (perfmon), making sure all of the counters are scaled the same.

Assumptions:

- Goal is to reduce footprint to as few servers as possible. Ideally, one server would carry the load and an additional server added for redundancy (*N* + 1 scenario).

![Processor time chart for lsass process (over all processors)](media/capacity-planning-considerations-proc-time-chart.png)

Knowledge gained from the data in the chart (Process(lsass)\\% Processor Time):

- The business day starts ramping up around 7:00 and decreases at 5:00 PM.
- The peak busiest period is from 9:30 AM to 11:00 AM. 
  > [!NOTE]
  > All performance data is historical. The peak data point at 9:15 indicates the load from 9:00 to 9:15.
- There are spikes before 7:00 AM which could indicate either load from different time zones or background infrastructure activity, such as backups. Because the peak at 9:30 AM exceeds this activity, it is not relevant.
- There are three domain controllers in the site.

At maximum load, lsass consumes about 485% of one CPU, or 4.85 CPUs running at 100%. As per the math earlier, this means the site needs about 12.25 CPUs for AD DS. Add in the above suggestions of 5% to 10% for background processes and that means replacing the server today would need approximately 12.30 to 12.35 CPUs to support the same load. An environmental estimate for growth now needs to be factored in.

### When to tune LDAP weights

There are several scenarios where tuning [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) should be considered. Within the context of capacity planning, this would be done when the application or user loads are not evenly balanced, or the underlying systems are not evenly balanced in terms of capability. Reasons to do so beyond capacity planning are outside of the scope of this article.

There are two common reasons to tune LDAP Weights:

- The PDC emulator is an example that affects every environment for which user or application load behavior is not evenly distributed. As certain tools and actions target the PDC emulator, such as the Group Policy management tools, second attempts in the case of authentication failures, trust establishment, and so on, CPU resources on the PDC emulator may be more heavily demanded than elsewhere in the site.
  - It is only useful to tune this if there is a noticeable difference in CPU utilization in order  to reduce the load on the PDC emulator and increase the load on other domain controllers will allow a more even distribution of load.
  - In this case, set LDAPSrvWeight between 50 and 75 for the PDC emulator.
- Servers with differing counts of CPUs (and speeds) in a site.  For example, say there are two eight-core servers and one four-core server.  The last server has half the processors of the other two servers.  This means that a well distributed client load will increase the average CPU load on the four-core box to roughly twice that of the eight-core boxes.
  - For example, the two eight-core boxes would be running at 40% and the four-core box would be running at 80%.
  - Also, consider the impact of loss of one eight-core box in this scenario, specifically the fact that the four-core box would now be overloaded.

#### Example 1 - PDC

| |Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

The catch here is that if the PDC emulator role is transferred or seized, particularly to another domain controller in the site, there will be a dramatic increase on the new PDC emulator.

Using the example from the section [Target site behavior profile](#target-site-behavior-profile), an assumption was made that all three domain controllers in the site had four CPUs. What should happen, under normal conditions, if one of the domain controllers had eight CPUs? There would be two domain controllers at 40% utilization and one at 20% utilization. While this is not bad, there is an opportunity to balance the load a little bit better. Leverage LDAP weights to accomplish this.  An example scenario would be:

#### Example 2 - Differing CPU counts

| |Processor Information\\ %&nbsp;Processor Utility(_Total)<br />Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|4-CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8-CPU DC 3|20|200|30%|

Be very careful with these scenarios though. As can be seen above, the math looks really nice and pretty on paper. But throughout this article, planning for an “*N* + 1” scenario is of paramount importance. The impact of one DC going offline must be calculated for every scenario. In the immediately preceding scenario where the load distribution is even, in order to ensure a 60% load during an “*N*” scenario, with the load balanced evenly across all servers, the distribution will be fine as the ratios stay consistent. Looking at the PDC emulator tuning scenario, and in general any scenario where user or application load is unbalanced, the effect is very different:

| |Tuned Utilization|New LdapSrvWeight|Estimated New Utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### Virtualization considerations for processing

There are two layers of capacity planning that need to be done in a virtualized environment. At the host level, similar to the identification of the business cycle outlined for the domain controller processing previously, thresholds during the peak period need to be identified. Because the underlying principals are the same for a host machine scheduling guest threads on the CPU as for getting AD DS threads on the CPU on a physical machine, the same goal of 40% to 60% on the underlying host are recommended. At the next layer, the guest layer, since the principals of thread scheduling have not changed, the goal within the guest remains in the 40% to 60% range.

In a direct mapped scenario, one guest per host, all the capacity planning done to this point needs to be added to the requirements (RAM, disk, network) of the underlying host operating system. In a shared host scenario, testing indicates that there is 10% impact on the efficiency of the underlying processors. That means if a site needs 10 CPUs at a target of 40%, the recommended amount of virtual CPUs to allocate across all the “*N*” guests would be 11. In a site with a mixed distribution of physical servers and virtual servers, the modifier only applies to the VMs. For example, if a site has an “*N* + 1” scenario, one physical or direct-mapped server with 10 CPUs would be about equivalent to one guest with 11 CPUs on a host, with 11 CPUs reserved for the domain controller.

Throughout the analysis and calculation of the CPU quantities necessary to support AD DS load, the numbers of CPUs that map to what can be purchased in terms physical hardware do not necessarily map cleanly. Virtualization eliminates the need to round up. Virtualization decreases the effort necessary to add compute capacity to a site, given the ease with which a CPU can be added to a VM. It does not eliminate the need to accurately evaluate the compute power needed so that the underlying hardware is available when additional CPUs need to be added to the guests.  As always, remember to plan and monitor for growth in demand.

### Calculation summary example

|System|Peak CPU|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|Total CPU being used|485%|  
  
|Target system(s) count|Total bandwidth (from above)|
|-|-|
|CPUs needed at 40% target|4.85 &divide; .4 = 12.25|

Repeating due to the importance of this point, *remember to plan for growth*. Assuming 50% growth over the next three years, this environment will need 18.375 CPUs (12.25 &times; 1.5) at the three-year mark. An alternate plan would be to review after the first year and add in additional capacity as needed.

### Cross-trust client authentication load for NTLM

#### Evaluating cross-trust client authentication load

Many environments may have one or more domains connected by a trust. An authentication request for an identity in another domain that does not use Kerberos authentication needs to traverse a trust using the domain controller’s secure channel to another domain controller either in the destination domain or the next domain in the path to the destination domain. The number of concurrent calls using the secure channel that a domain controller can make to a domain controller in a trusted domain is controlled by a setting known as **MaxConcurrentAPI**. For domain controllers, ensuring that the secure channel can handle the amount of load is accomplished by one of two approaches: tuning **MaxConcurrentAPI** or, within a forest, creating shortcut trusts. To gauge the volume of traffic across an individual trust, refer to [How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](https://support.microsoft.com/kb/2688798).

During data collection, this, as with all the other scenarios, must be collected during the peak busy periods of the day for the data to be useful.

> [!NOTE]
> Intraforest and interforest scenarios may cause the authentication to traverse multiple trusts and each stage would need to be tuned.

#### Planning

There are a number of applications that use NTLM authentication by default, or use it in a certain configuration scenario. Application servers grow in capacity and service an increasing number of active clients. There is also a trend that clients keep sessions open for a limited time and rather reconnect on a regular basis (such as email pull sync). Another common example for high NTLM load is web proxy servers that require authentication for Internet access.

These applications can cause a significant load for NTLM authentication, which can put significant stress on the DCs, especially when users and resources are in different domains.

There are multiple approaches to managing cross-trust load, which in practice are used in conjunction rather than in an exclusive either/or scenario. The possible options are:

- Reduce cross-trust client authentication by locating the services that a user consumes in the same domain that the user is resident in.
- Increase the number of secure-channels available. This is relevant to intraforest and cross-forest traffic and are known as shortcut trusts.
- Tune the default settings for **MaxConcurrentAPI**.

For tuning **MaxConcurrentAPI** on an existing server, the equation is:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

For more information, see [KB article 2688798: How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](http://support.microsoft.com/kb/2688798).

## Virtualization considerations

None, this is an operating system tuning setting.

### Calculation summary example

|Data type|Value|
|-|-|
|Semaphore Acquires (Minimum)|6,161|
|Semaphore Acquires (Maximum)|6,762|
|Semaphore Timeouts|0|
|Average Semaphore Hold Time|0.012|
|Collection Duration (seconds)|1:11 minutes (71 seconds)|
|Formula (from KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Minimum value for **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

For this system for this time period, the default values are acceptable.

## Monitoring for compliance with capacity planning goals

Throughout this article, it has been discussed that planning and scaling go towards utilization targets. Here is a summary chart of the recommended thresholds that must be monitored to ensure the systems are operating within adequate capacity thresholds. Keep in mind that these are not performance thresholds, but capacity planning thresholds. A server operating in excess of these thresholds will work, but is time to start validating that all the applications are well behaved. If said applications are well behaved, it is time to start evaluating hardware upgrades or other configuration changes.

|Category|Performance counter|Interval/Sampling|Target|Warning|
|-|-|-|-|-|
|Processor|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 o versioni precedenti)|Memoria\MByte MB|< 100 MB|N/D|< 100 MB|
|RAM (Windows Server 2012)|A termine Memory\Long Media Lifetime(s) Cache di Standby|30 min|Devono essere testati|Devono essere testati|
|Rete|Interfaccia di rete (\*) \Bytes inviati/sec<br /><br />Interfaccia di rete (\*) \Bytes ricevuti/sec|30 min|40%|60%|
|Archiviazione|Disco logico ( *\<unità di Database NTDS\>* ) \Avg letture disco/sec<br /><br />Disco logico ( *\<unità di Database NTDS\>* ) \Avg scritture disco/sec|60 min|10 ms|15 ms|
|Servizi di Active Directory|Il servizio Accesso rete (\*) semaforo \Average contengono ora|60 min|0|1 secondo|

## <a name="appendix-a-cpu-sizing-criteria"></a>Appendice A: Criterio di ridimensionamento della CPU

### <a name="definitions"></a>Definizioni

**Processore (microprocessore) –** un componente che legge ed esegue le istruzioni di programma  

**CPU-** CPU  

**Processori multicore –** più CPU collegati allo stesso circuito integrata  

**Con più CPU –** più CPU, non collegati allo stesso circuito integrata  

**Processore logico –** un motore di calcolo logico dalla prospettiva del sistema operativo  

Ciò include l'hyperthreading, uno dei core su processori multicore o un processore a core singolo.  

I sistemi server attuali dispone di più processori, più processori multicore e hyper-threading, queste informazioni sono generalizzate per coprire entrambi gli scenari. Di conseguenza, verrà utilizzato il processore logico termine perché rappresenta il sistema operativo e un punto di vista dell'applicazione dei motori di elaborazione disponibili.

### <a name="thread-level-parallelism"></a>Parallelismo a livello di thread

Ogni thread è un'attività indipendente, poiché ogni thread dispone di un proprio stack e le istruzioni. Poiché Active Directory Domain Services è a thread multipli e il numero di thread disponibili può essere ottimizzato tramite [come visualizzare e impostare criteri LDAP in Active Directory usando Ntdsutil.exe](http://support.microsoft.com/kb/315071), la scalabilità tra più processori logici.

### <a name="data-level-parallelism"></a>Parallelismo a livello di dati

Ciò comporta la condivisione dei dati tra più thread all'interno di un processo (nel caso solo il processo di AD DS) e tra più thread in più processi (in genere). Con attenzione per eccesso semplificando il caso, ciò significa che qualsiasi modifica ai dati viene applicate a tutti i thread in esecuzione in tutti i diversi livelli di cache (L1, L2, L3) per tutti i core detto thread in esecuzione, nonché l'aggiornamento di memoria condivisa. Le prestazioni possono ridursi durante le operazioni di scrittura, mentre tutti i vari percorsi di memoria saranno riportati coerente prima di poter continuare l'elaborazione di istruzione.

### <a name="cpu-speed-vs-multiple-core-considerations"></a>Velocità della CPU e più componenti principali considerazioni

La regola empirica generale è più veloce processori logici ridurre il tempo impiegato per elaborare una serie di istruzioni, durante il mezzo di processori logici che più attività possono essere eseguite nello stesso momento. Queste regole generali suddividere, come gli scenari di aumento della complessità intrinsecamente con le considerazioni relative a recupero dei dati dalla memoria condivisa, in attesa di parallelismo a livello di dati e l'overhead di gestione di più thread. Questo è anche il motivo per cui non è lineare scalabilità nei sistemi multicore.

Si consideri la seguente analogie in queste considerazioni: si tratta di un'autostrada, con ogni thread in corso di un'automobile singoli, ogni corsia in corso di un core e il limite di velocità in corso la velocità di clock.

1. Se è presente solo un'automobile in autostrada, non importa se sono presenti due corsie o 12 corsie. Tale macchina verrà solo passare la stessa velocità consentito dal limite di velocità.
1. Si supponga che i dati che ha bisogno il thread non sono immediatamente disponibili. L'analogia sarebbe che un segmento di strada viene arrestato. Se è presente solo un'automobile in autostrada, non è importante che cos'è il limite di velocità fino a quando non viene riaperta la corsia (i dati vengono recuperati dalla memoria).
1. Aumenta il numero di automobili, aumenta il sovraccarico per gestire il numero di automobili. Confrontare l'esperienza di Guida e la quantità di attenzione necessaria quando la strada è praticamente vuoti, ad esempio sera, rispetto a quando il traffico è elevato (ad esempio pomeriggio intermedio, ma non ore). Inoltre, considerare la quantità di attenzione necessaria quando si Guida su un'autostrada due-corsia, in cui è presente un solo altra corsia preoccuparsi di cosa fanno i driver, rispetto a un'autostrada sei-corsia in cui è necessario preoccuparsi di quali numerosi altri driver stanno facendo.
   > [!NOTE]
   > Nella sezione successiva viene esteso l'analogia sullo scenario ore: Tempo di risposta o come il sistema Busyness influisce sulle prestazioni.

Di conseguenza, le specifiche sulle più veloce o più processori diventano estremamente soggettive al comportamento dell'applicazione, che nel caso di Active Directory Domain Services è molto per l'ambiente specifico e persino varia da server a server all'interno di un ambiente. Ecco perché i riferimenti in precedenza nell'articolo non investire molto in corso eccessivamente preciso e un margine di sicurezza è incluso nei calcoli. Quando si effettuano le decisioni di acquisto basato su budget, è consigliabile ottimizzare l'utilizzo dei processori a 40% (o il numero desiderato per l'ambiente) si verifica in primo luogo, prima che considerando l'acquisto di processori più veloci. La sincronizzazione tra più processori maggiore riduce il vero vantaggio del più processori dalla progressione lineare (2&times; il numero di processori fornisce meno di 2&times; potenza di calcolo aggiuntive disponibili).

> [!NOTE]
> Legge di Amdahl e dalle disposizioni postulato sono i concetti pertinenti.

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>Tempo di risposta/come busyness il sistema influisce sulle prestazioni

Teoria accodamento è lo Studio matematico delle linee di attesa (Code). In teoria Accodamento messaggi, il diritto di utilizzo è rappresentato dall'equazione:

*U* k = *B* &divide; *T*

In cui *U* k è la percentuale di utilizzo *B* è la quantità di tempo occupato, e *T* è il tempo totale è stato rilevato il sistema. Tradotto in contesto di Windows, ciò significa il numero di 100 nanosecondi (ns) i thread di intervallo che sono nello stato di esecuzione diviso per il numero di intervalli di 100 ns era disponibile nelle dato intervallo di tempo. Questo è esattamente la formula per calcolare % Processor Utility (riferimento [oggetto processore](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) e [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

Teoria Accodamento messaggi offre inoltre la formula: *N* = *U* k &divide; (1 &ndash; *U* k) per stimare il numero di elementi in attesa in base all'utilizzo ( *N* è la lunghezza della coda). In questo grafico fra tutti gli intervalli di utilizzo fornisce le stime riportate di seguito per quanto tempo coda da cui recuperare il processore è in un determinato carico della CPU.

![Lunghezza coda](media/capacity-planning-considerations-queue-length.png)

Si è osservato che dopo il caricamento di 50% della CPU, in Media ci sia sempre un'attesa di un altro elemento nella coda, con una velocità significativamente rapido aumentano dopo circa il 70% l'utilizzo della CPU.

Restituzione di per la stessa analogia trainante usata in precedenza in questa sezione:

- I tempi di occupato delle "pomeriggio intermedio", ipoteticamente, rientra in una posizione nell'intervallo del 40% al 70%. Vi è traffico sufficiente che di uno possibilità di selezionare qualsiasi corsia non risulta essere majorly limitato e la probabilità di un altro driver riguardano il modo, durante l'alta, non richiede il livello di impegno per "trova" un gap sicuro tra altri automobili in viaggio.
- Una nota che quando il traffico si avvicina ore, il sistema road si avvicina a 100% della capacità. Modificare le corsie può diventare molto difficile perché automobili sono così vicina insieme che occorre prestare maggiore attenzione a questo scopo.

È per questo motivo calcola la media a lungo termine per consentite dalla capacità stimata del 40% di spazio head per picchi anomali nel carico, che ha dichiarato picchi transitorio (ad esempio, in modo inadeguato codificata query in esecuzione per alcuni minuti) o anomali in genere burst (al mattino di carico il primo giorno dopo un lungo fine settimana).

L'istruzione precedente viene gestito in corso lo stesso come il diritto di utilizzo è un po' di semplificare il lavoro per la semplicità del lettore generale calcolo di % tempo processore. Per tali più matematicamente rigorosi:  
- Conversione di [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = il numero di intervalli di 100 ns "Idle" thread trascorre sul processore logico. La modifica di "*X*" di una variabile nel [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calcolo
  - *T* = numero totale di intervalli di 100 ns in un intervallo di tempo specificato. La modifica di "*Y*" di una variabile nel [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calcolo.
  - *U* k = la percentuale di utilizzo del processore logico per il "Thread inattivo" o % tempo di inattività.  
- Procedendo i calcoli matematici:
  - *U* k = 1-% tempo processore
  - % Tempo processore = 1 – *U* k
  - %Processor Time = 1 – *B* / *T*
  - % Tempo processore = 1 – *X1* – *X0* / *Y1* – *Y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>Applicare i concetti relativi alla pianificazione della capacità

I calcoli matematici precedenti potrebbero prevedere la determinazione sul numero di processori logici necessari in un sistema sembrano piuttosto complessa. Ecco perché l'approccio per i sistemi di ridimensionamento è incentrata sulla determinazione di utilizzo di destinazione massima in base al corrente di carico e di calcolo del numero di processori logici necessarie per accedervi. Inoltre, velocità del processore logico avrà un impatto significativo sulle prestazioni, efficienza della cache, i requisiti di coerenza a livello di memoria, pianificazione dei thread e sincronizzazione, mentre client modo perfetto con bilanciamento carico tutti avranno un impatto significativo prestazioni che variano in base da un server. Con il costo relativamente conveniente potenza di calcolo, il tentativo di analizzare e determinare il numero ideale di CPU necessita diventa più accademico esercizio fornire valore aziendale.

40% non è un requisito ferree, è una start ragionevole. Diversi consumer di Active Directory richiedono diversi livelli di velocità di risposta. Possono esistere scenari in cui eseguire ambienti a 80 o 90% di utilizzo come media velocità, come i tempi di attesa aumento per l'accesso per il processore non influirà in modo significativo delle prestazioni dei client. È importante reiterare che esistono molte aree del sistema che sono molto più lento rispetto al processore logico nel sistema, compresi gli accessi alla memoria RAM, accedere al disco e la trasmissione della risposta sulla rete. Tutti questi elementi devono essere ottimizzati in combinazione. Esempi:

- Aggiunta di più processori a un sistema che esegue il 90% che è associato a dischi probabilmente non verrà migliorare significativamente le prestazioni. Un'analisi più approfondita del sistema identificherà probabilmente che sono presenti numerosi thread che sono non coinvolto nel processore perché sono in attesa i/o per il completamento.
- Risolvere i problemi associati a disco potenzialmente significa che i thread che sono stati precedentemente un notevole dispendio di tempo nello stato di attesa non saranno più nello stato di attesa i/o e vi saranno più elevato per il tempo di CPU, vale a dire che il 90% di utilizzo nella precedente esempio passerà al 100% (perché non può passare superiore). Entrambi i componenti devono essere ottimizzati in combinazione.
  > [!NOTE]
  > Processore Information(*)\\% utilità processore può superare il 100% con i sistemi che hanno una modalità "Turbo".  Si tratta in cui la CPU supera la velocità del processore più votati per brevi periodi.  Documentazione di riferimento della CPU di produttori e una descrizione del contatore per maggiori informazioni.  

La discussione considerazioni sull'utilizzo di tutto il sistema offre anche al controller di dominio conversazione come Guest virtualizzati. [Tempo di risposta/come busyness il sistema influisce sulle prestazioni](#response-timehow-the-system-busyness-impacts-performance) si applica a host e guest in uno scenario virtualizzato. È per questo motivo in un host con guest solo uno, un controller di dominio (e in genere qualsiasi sistema) ha quasi le stesse prestazioni che avviene su hardware fisico. Aggiunta di utenti guest aggiuntive agli host aumenta l'utilizzo dell'host sottostante, aumentando in tal modo i tempi di attesa per ottenere l'accesso ai processori come spiegato in precedenza. In breve, utilizzo del processore logico deve essere gestito sia sull'host e a livello di guest.

Estendere i precedente analogie, lasciando autostrada come l'hardware fisico, il guest macchina virtuale verrà analogized con bus (bus express che passa direttamente alla destinazione di rider vuole). Si supponga di quattro scenari seguenti:

- Si trova fuori orario di lavoro, un motociclista ottiene in un bus che è quasi vuoto e il bus Ottiene su una strada che anche è quasi vuota. Poiché non esiste nessun traffico da affrontare con, sono altrettanto rapido come se il rider aveva driven invece il rider ha una giostra facile nice. Tempi di viaggio di rider ancora sono vincolate dal limite di velocità.
- Gli orari di minore in modo che il bus è quasi vuoto ma la maggior parte delle corsie in viaggio vengono chiusi, si tratta ancora nel caso di congestione autostrada. Il rider è un bus quasi vuoto su una strada congestionata. Mentre il rider non ha molta concorrenza nel bus di dove si trovano, il tempo totale di andata e ritorno ancora dipende dal resto del traffico all'esterno.
- Si tratta di ore, pertanto sono congestionati autostrada e il bus. Non solo la corsa richiede più tempo, ma ottenere e disattivare i bus è un incubo perché le persone sono shoulder a shoulder e autostrada non è molto meglio. Aggiunta di più bus (processori logici per il guest) non significa ma possono essere collocati in viaggio uno più facilmente, o che verrà ridotto il viaggio.
- Nello scenario finale, anche se la stessa analogia del potrà essere allungamento un po', nel caso in cui il bus di è completo, ma non è la strada di congestione. Sebbene il rider sarà comunque necessario durante il recupero e disattivare i bus, l'ottimizzazione dei viaggi sarà efficiente dopo che il bus di è in viaggio. Questo è l'unico scenario in cui l'aggiunta di più bus (processori logici per il guest) migliorerà le prestazioni di guest.

Da qui è relativamente semplice estrapolare questo esempio sono presenti numerosi scenari tra il 0% sottoutilizzate e lo stato sottoutilizzate 100% del viaggio e lo stato 0% e 100%-sottoutilizzate del bus che presentano vari livelli di impatto.

Applicando le entità di sopra del 40% della CPU come destinazione ragionevole per l'host, nonché il guest è iniziare ragionevole per la stessa soluzione come in precedenza, la quantità di Accodamento messaggi.

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>Appendice B: Considerazioni relative alle velocità di processore differente e l'effetto di risparmio energia dei processori in velocità processore

In tutte le sezioni sulla selezione del processore viene presupposto che il processore è in esecuzione al 100% della velocità di clock l'intera ora che sono stati raccolti i dati e che i sistemi di sostituzione avrà lo stessa processori di velocità. Nonostante entrambe supposizioni in pratica da false, in particolare con Windows Server 2008 R2 e versioni successive, dove è il risparmio di energia predefinita **bilanciato**, la metodologia è l'acronimo comunque perché è l'approccio conservativo. Anche se può aumentare il tasso di errore potenziali, può solo aumentare il margine di sicurezza come aumento della velocità del processore.

- In uno scenario in cui vengono richiesti 11.25 CPU, se i processori in esecuzione alla velocità della metà quando sono stati raccolti i dati, ad esempio, la stima più accurata potrebbe essere 5.125 &divide; 2.
- È possibile garantire che raddoppiare la velocità di clock sarebbe doppio della quantità di elaborazione che si verifica per un determinato periodo di tempo. Ciò è dovuto al fatto che la quantità di tempo in cui i processori spend in attesa di RAM o altri componenti di sistema può rimanere invariato. Il risultato finale è che i processori più veloci potrebbero impiegare una maggiore percentuale del tempo di inattività durante l'attesa dei dati da recuperare. Anche in questo caso, è consigliabile il minimo comune denominatore, essere prudenti, bisogna rispettarla ed evitare tenta di calcolare un potenzialmente false a livello di accuratezza, presupponendo un confronto lineare tra velocità del processore.

In alternativa, se la velocità del processore in componenti hardware sostitutivi è inferiore a hardware corrente, sarebbe possibile aumentare la stima dei processori necessari per una quantità proporzionata. Ad esempio, viene calcolato che sono necessari 10 processori per sostenere il carico in un sito e i processori correnti eseguono GHz 3.3 e processori sostitutivo verranno eseguito a 2,6 Ghz, si tratta di una diminuzione 21% nella velocità. In questo caso, 12 processori sarebbe la quantità consigliata.

Ciò premesso, questa variabilità sarebbe non modifica gli obiettivi di utilizzo del processore di Capacity Management. Come la velocità di clock del processore verrà regolata dinamicamente in base al carico richiesto, in esecuzione il sistema sottoposto a carichi più elevati genererà uno scenario in cui la CPU usa più tempo in uno stato di velocità di clock superiore, rendendo l'obiettivo finale al 40% di utilizzo in 100% stato di velocità di clock picco di domanda. Un valore minore di quello genererà il risparmio di energia della velocità della CPU sarà limitata nuovamente durante la disattivazione gli scenari di picco.

> [!NOTE]
> Sarebbe un'opzione per disattivare il risparmio di energia sui processori (l'impostazione di risparmio di energia su **ad alte prestazioni**) mentre vengono raccolti i dati. Che produrrebbe una rappresentazione accurata del consumo di CPU nel server di destinazione.

Per regolare le stime relative a processori diversi, usato come sicure, esclusione di altri colli di bottiglia del sistema descritti in precedenza, presupporre che le doppie velocità processore raddoppiato la quantità di elaborazione che può essere eseguita.  Attualmente, l'architettura interna dei processori è sufficientemente diversa tra processori, ovvero un modo più sicuro per misurare gli effetti dell'uso di processori diversi rispetto a cui sono stati acquisiti da sfruttare il benchmark SPECint_rate2006 dalla versione di valutazione delle prestazioni Standard Corporation.

1. Trovare i punteggi SPECint_rate2006 per il processore che sono in uso e che prevedono di essere utilizzato.
    1. Nel sito Web della società valutazione delle prestazioni Standard, selezionare **risultati**, evidenziare **CPU2006**e selezionare **cerca tutti i risultati SPECint_rate2006**.
    1. Sotto **semplice richiesta**, immettere i criteri di ricerca per il processore di destinazione, ad esempio **corrispondenze di processori E5-2630 (baselinetarget)** e **processore corrispondenze E5-2650(base)** .
    1. Trovare la configurazione di server e del processore da usare (o qualcosa di chiusura, se non è disponibile una corrispondenza esatta) e notare il valore di **risultato** e **& core** colonne.
1. Per determinare il modificatore di usare l'equazione seguente:
   >((*Valore di punteggio per ogni core della piattaforma di destinazione*) &times; (*MHz per core della piattaforma di base*)) &divide; ((*linea di base di valore di punteggio per ogni core*) &times; (*MHz per core della piattaforma di destinazione*))  

    Usando l'esempio precedente:
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Moltiplicare il numero stimato di processori per il modificatore.  In questo caso per passare dalla finestra di E5-2650 processore del processore 2630 E5 moltiplicare le 11.25 CPU calcolate &times; 0.92 = 10,35 processori necessiti.

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>Appendice C: Nozioni di base riguardanti il sistema operativo l'interazione con archiviazione

I concetti teoria dell'accodamento descritti in [tempi di risposta/come busyness il sistema influisce sulle prestazioni](#response-timehow-the-system-busyness-impacts-performance) sono applicabili anche ad archiviazione. Avere una conoscenza del modo in cui l'handle del sistema operativo, i/o è necessario applicare questi concetti. Nel sistema operativo Microsoft Windows, viene creata una coda per contenere le richieste dei / o per ogni disco fisico. Tuttavia, un chiarimento su disco fisico deve essere inviata. Controller di array e nomi alternativi del soggetto presentare aggregazioni di assi per il sistema operativo come singolo dischi fisici. Inoltre, i controller di array e nomi alternativi del soggetto può aggregare più dischi in una matrice set e quindi suddividere la matrice impostata in più "partizioni", che a sua volta viene presentato al sistema operativo come più dischi fisici (figura ref).

![Bloccare gli assi](media/capacity-planning-considerations-block-spindles.png)  

In questa figura le due assi sono il mirroring e suddiviso in aree logiche per l'archiviazione dei dati (Data 1 e 2 di dati). Queste aree logiche vengono visualizzate dal sistema operativo come dischi fisici separati.

Anche se ciò può generare confusione elevata, la terminologia seguente viene utilizzata in questa appendice per identificare le diverse entità:

- **Spindle –** il dispositivo fisico installato nel server.
- **Array:** una raccolta di assi aggregati dal controller.
- **Matrice di partizione:** un partizionamento della matrice di aggregate
- **LUN-** una matrice, usata quando si fa riferimento a nomi alternativi del soggetto
- **Disco –** cosa osserva il sistema operativo da un singolo disco fisico.
- **Partizione –** un partizionamento logico di ciò che percepisce come un'unità disco del sistema operativo.

### <a name="operating-system-architecture-considerations"></a>Considerazioni sull'architettura del sistema operativo

Il sistema operativo crea una coda First In/First Out (FIFO) i/o per ogni disco che viene osservato; Questo disco potrebbe essere che rappresenta un asse, una matrice o una partizione di matrice. Dal punto di vista di sistema operativo, per quanto riguarda la gestione dei / o, più attivo mette in coda è preferibile. Come verrà serializzata una coda FIFO, vale a dire che tutti i/o emessi al sottosistema di archiviazione devono essere elaborati nell'ordine di arrivo della richiesta. Grazie alla correlazione tra ogni disco osservato dal sistema operativo con un asse o una matrice, il sistema operativo gestisce ora una coda dei / o per ogni set univoco di dischi, in tal modo eliminando la contesa tra risorse dei / o insufficienti su dischi e isolare richiesta dei / o a un singolo disco. Come un'eccezione, Windows Server 2008 introduce il concetto di priorità i/o e applicazioni progettate per utilizzare la priorità "Bassa" rientrano in questo ordine normale e sedersi back. Le applicazioni non codificate per sfruttare l'impostazione predefinita priorità "Bassa" a "Normal".

### <a name="introducing-simple-storage-subsystems"></a>Introduzione a sottosistemi di archiviazione semplice

A partire da un semplice esempio (un singolo disco rigido all'interno di un computer) verrà fornita un'analisi da singoli componenti. Scomporre questo nei componenti del sottosistema di archiviazione principale, il sistema include:

- **1 –** 10.000 RPM Ultra rapida SCSI HD (Ultra rapida SCSI ha una velocità di trasferimento 20 MB/s)
- **1 –** Bus SCSI (il cavo)
- **1 –** adattatore SCSI ultra rapida
- **1 –** bus PCI 33 MHz a 32 bit

Una volta identificati i componenti, un'idea della quantità di dati possono far passare il sistema o quanto i/o possono essere gestiti, può essere calcolata. Si noti che la quantità dei / o e quantità di dati che possono far passare il sistema sia correlati, ma non uguali. Questa correlazione dipende dal fatto che il disco i/o casuali o sequenziali e la dimensione del blocco. (Tutti i dati vengono scritti sul disco come blocco, ma diverse applicazioni usando diversi blocchi). In modo da singoli componenti:

- **Il disco rigido,** sul disco rigido di 10.000 RPM Media sia un 7 millisecondi (ms) di ricerca ora e un'ora di accesso 3 ms. Cercare ora è la quantità media di tempo necessaria la testina di lettura/scrittura a passare a una posizione nella base. Ora di accesso è la quantità media di tempo che necessario per leggere o scrivere i dati su disco, una volta che l'intestazione è nella posizione corretta. Di conseguenza, il tempo medio per la lettura di un blocco univoco dei dati in un HD 10.000 RPM costituisce un accesso, per un totale di circa 10 ms (o.010 secondi) e una ricerca per ogni blocco di dati.

  Quando ogni accesso al disco richiede lo spostamento di testa in una nuova posizione sul disco, il comportamento di lettura/scrittura è detta "casuali". Di conseguenza, quando tutti i/o casuale, un HD 10.000 RPM può gestire circa 100 i/o al secondo (IOPS) (la formula è 1000 ms al secondo diviso per 10 ms per ogni i/o o 1000/10 = 100 IOPS).

  In alternativa, quando tutti i/o viene eseguita da settori adiacenti sul disco rigido, questa viene definita come i/o sequenziali. I/o sequenziali non ha alcun tempo di ricerca perché una volta completata il / o prima, la testina di lettura/scrittura è all'inizio in cui è archiviato il successivo blocco di dati sul disco rigido. In questo modo è in grado di gestire circa 333 un HD 10.000 RPM i/o al secondo (1000 ms al secondo diviso per 3 ms per ogni i/o).

  >[!NOTE]
  >In questo esempio non riflette la cache del disco, in cui i dati di un cilindro vengono in genere conservati. In questo caso, sono necessarie il / o prima di 10 ms e il disco legge l'intero cilindro. Tutti gli altri i/o sequenziali viene soddisfatta dalla cache. Di conseguenza, le cache nel disco potrebbero migliorare le prestazioni dei / o sequenziale.
  
  Fino ad ora, la velocità di trasferimento del disco rigido è stato irrilevante. Indica se il disco rigido è 20 MB/s Ultra-Wide o un Ultra3 160 MB/s, la quantità effettiva di IOPS può essere gestita da 10.000 RPM HD è ~ 100 casuale o i/o sequenziali di ~ 300. Come modificare le dimensioni di blocco in base all'applicazione la scrittura nell'unità, la quantità di dati che viene effettuato il pull per i/o è diversa. Ad esempio, se la dimensione del blocco è 8 KB, 100 operazioni dei / o verranno leggere o scrivere sul disco rigido un totale di 800 KB. Tuttavia, se la dimensione del blocco è 32 KB, 100 i/o verranno lettura/scrittura 3.200 KB (3,2 MB) nel disco rigido. Fino a quando la velocità di trasferimento SCSI è che supererà la quantità totale di dati trasferiti, ottenere un trasferimento "veloce" unità di velocità otterranno nulla. Vedere le tabelle seguenti per il confronto.

  | |Accedere a 4ms 7200 RPM 9ms seek,|Accedere a 10.000 RPM 7 ms seek, 3 MS|Accedere a 15.000 RPM 4ms seek, 2 ms
  |-|-|-|-|
  |/ O casuali|80|100|150|
  |/ O sequenziali|250|300|500|  
  
  |Unità di 10.000 RPM|Dimensione del blocco di 8 KB (Active Directory Jet)|
  |-|-|
  |/ O casuali|800 KB/s|
  |/ O sequenziali|2400 KB/s|

- **Backplane SCSI (bus) –** Understanding come "SCSI backplane (bus)", o in questo scenario il cavo sulla barra multifunzione, velocità effettiva di impatto del sottosistema di archiviazione dipende dalla conoscenza della dimensione del blocco. Essenzialmente la domanda sarebbe, quanto più i/o può l'handle del bus se il / o in blocchi da 8 KB? In questo scenario, il bus SCSI è 20 MB/s o 20480 KB/s. KB/s 20480 diviso per 8 KB blocchi producono un massimo di circa 2500 IOPS supportata dal bus SCSI.

  >[!NOTE]
  >Le cifre nella tabella seguente rappresentano un esempio. Più dispositivi di archiviazione collegati usano attualmente PCI Express, che offre velocità effettiva notevolmente superiore.  
  
  |/ O supportata dal bus SCSI per ogni dimensione del blocco|Dimensione del blocco 2 KB|Dimensione del blocco di 8 KB (AD Jet) (SQL Server 7.0 o SQL Server 2000)
  |-|-|-|
  |20 MB/s|10,000|2,500|
  |40 MB/s|20,000|5,000|
  |128 MB/s|65,536|16,384|
  |320 MB/s|160,000|40,000|

  Come può essere determinato da questo grafico, nello scenario presentato, indipendentemente da quali l'utilizzo, il bus non saranno mai un collo di bottiglia, come l'asse massimo è 100 i/o, ben di sotto di una delle soglie precedente.

  >[!NOTE]
  >Ciò presuppone che il bus SCSI è efficace al 100%.
  
- **Scheda SCSI –** per determinare la quantità dei / o che si possono gestire, devono essere controllati le specifiche del produttore. Indirizzamento delle richieste dei / o per il dispositivo appropriato richiede l'elaborazione di qualche tipo, di conseguenza la quantità dei / o che possono essere gestiti dipenda da una scheda SCSI (o controller dell'array) processore.

  In questo esempio si presuppone che può 1.000 dei / o essere gestita verrà effettuata.

- **Bus PCI-** si tratta di un componente spesso trascurato. In questo esempio, questa non sarà il collo di bottiglia; Quando i sistemi aumenta, tuttavia può diventare un collo di bottiglia. Per riferimento, un bus PCI a 32 bit che funziona a 33Mhz possibile Transfer teoria 133 MB/s dei dati. Seguito è riportata l'equazione:  
  > 32 bit &divide; a 8 bit per byte &times; 33 MHz = 133 MB/s.  

  Si noti che è il limite teorico; in realtà solo circa il 50% rispetto al numero massimo viene effettivamente raggiunto, anche se in determinati scenari di burst, è possibile ottenere l'efficienza di 75% per brevi periodi.

  66 bus PCI Mhz a 64 bit può supportare un massimo teorico di (64 bit &divide; a 8 bit per byte &times; 66 Mhz) = 528 MB/sec. inoltre, qualsiasi altro dispositivo (ad esempio la scheda di rete, secondo controller SCSI e così via) consente di ridurre la larghezza di banda disponibile come la larghezza di banda viene condivisa e i dispositivi verranno si contendono le risorse limitate.

Dopo l'analisi dei componenti di questo sottosistema di archiviazione, l'asse è il fattore limitante nella quantità dei / o che possono essere richieste e, di conseguenza, la quantità di dati che possono far passare il sistema. In particolare, in uno scenario di Active Directory Domain Services, si tratta 100 i/o casuale al secondo in incrementi di 8 KB, per un totale di 800 KB al secondo quando l'accesso al database Jet. In alternativa, la velocità effettiva massima per un asse che viene allocata esclusivamente per i file di log potrebbe risentire le limitazioni seguenti: 300 sequenziale i/o al secondo in incrementi di 8 KB, per un totale di 2400 KB, ovvero 2,4 MB al secondo.

A questo punto, con l'analisi una semplice configurazione, nella tabella seguente viene illustrato dove si verificherà il collo di bottiglia come componenti del sottosistema di archiviazione vengono modificati o aggiunti.

|Note|Analisi del collo di bottiglia|Disco|Bus|Scheda|Bus PCI|
|-|-|-|-|-|-|
|Questa è la configurazione di controller di dominio dopo aver aggiunto un secondo disco. La configurazione del disco rappresenta un collo di bottiglia a 800 KB/s.|Aggiungere 1 disco (totale = 2)<br /><br />I/o è casuale<br /><br />Dimensione del blocco di 4 KB<br /><br />HD 10.000 RPM|200 i/o totale<br />800 totale KB/s.| | | |
|Dopo l'aggiunta di 7 dischi, la configurazione del disco rappresenta comunque il collo di bottiglia a 3200 KB/s.|**Aggiungere 7 dischi (totale = 8)**  <br /><br />I/o è casuale<br /><br />Dimensione del blocco di 4 KB<br /><br />HD 10.000 RPM|800 i/o totale.<br />Totale KB/s 3200| | | |
|Dopo aver modificato i/o a sequenziale, la scheda di rete diventa il collo di bottiglia perché è limitata a 1000 IOPS.|Aggiungere 7 dischi (totale = 8)<br /><br />**I/o è sequenziale**<br /><br />Dimensione del blocco di 4 KB<br /><br />HD 10.000 RPM| | |Sec i/o 2400 possono essere letti/scritti su disco, limitato a 1000 IOPS controller| |
|Dopo aver sostituito la scheda di rete con una scheda SCSI che supporta 10.000 IOPS, il collo di bottiglia restituisce la configurazione del disco.|Aggiungere 7 dischi (totale = 8)<br /><br />I/o è casuale<br /><br />Dimensione del blocco di 4 KB<br /><br />HD 10.000 RPM<br /><br />**Eseguire l'aggiornamento di una scheda SCSI (ora i/o supporta 10.000)**|800 i/o totale.<br />3\.200 totale KB/s| | | |
|Dopo avere aumentato le dimensioni del blocco a 32 KB, il bus diventa il collo di bottiglia perché supporta solo 20 MB/s.|Aggiungere 7 dischi (totale = 8)<br /><br />I/o è casuale<br /><br />**Dimensione del blocco di 32 KB**<br /><br />HD 10.000 RPM| |800 i/o totale. 25.600 KB/s (25 MB/s) possono essere letti/scritti su disco.<br /><br />Il bus supporta solo i 20 MB/s| | |
|Dopo l'aggiornamento al bus e aggiunta di altri dischi, il disco rimane il collo di bottiglia.|**Aggiungere 13 dischi (totali = 14)**<br /><br />Aggiungere una seconda scheda SCSI con 14 dischi<br /><br />I/o è casuale<br /><br />Dimensione del blocco di 4 KB<br /><br />HD 10.000 RPM<br /><br />**Eseguire l'aggiornamento a 320 bus SCSI MB/s**|/ O 2800<br /><br />KB/s 11,200 (10.9 MB/s)| | | |
|Dopo aver modificato i/o a sequenziali, il disco rimane il collo di bottiglia.|Aggiungere 13 dischi (totali = 14)<br /><br />Aggiungere una seconda scheda SCSI con 14 dischi<br /><br />**I/o è sequenziale**<br /><br />Dimensione del blocco di 4 KB<br /><br />HD 10.000 RPM<br /><br />Eseguire l'aggiornamento a 320 bus SCSI MB/s|I/8,400 o<br /><br />33.600 KB\s<br /><br />(32,8 MB\s)| | | |
|Dopo l'aggiunta di dischi rigidi più veloci, il disco rimane il collo di bottiglia.|Aggiungere 13 dischi (totali = 14)<br /><br />Aggiungere una seconda scheda SCSI con 14 dischi<br /><br />I/o è sequenziale<br /><br />Dimensione del blocco di 4 KB<br /><br />**DA 15.000 RPM HD**<br /><br />Eseguire l'aggiornamento a 320 bus SCSI MB/s|I/14.000 o<br /><br />KB/s 56.000<br /><br />(54.7 MB/s)| | | |
|Dopo avere aumentato le dimensioni del blocco a 32 KB, il bus PCI diventa il collo di bottiglia.|Aggiungere 13 dischi (totali = 14)<br /><br />Aggiungere una seconda scheda SCSI con 14 dischi<br /><br />I/o è sequenziale<br /><br />**Dimensione del blocco di 32 KB**<br /><br />DA 15.000 RPM HD<br /><br />Eseguire l'aggiornamento a 320 bus SCSI MB/s| | | |I/14.000 o<br /><br />KB/s 448,000<br /><br />(437 MB/s) è il limite di lettura/scrittura per l'asse.<br /><br />Il bus PCI supporta un massimo teorico di 133 MB/s (75% efficiente ottimali).|

### <a name="introducing-raid"></a>Introduzione a RAID

La natura di un sottosistema di archiviazione non influisce in modo significativo quando viene introdotto un controller dell'array; semplicemente sostituita la scheda SCSI nei calcoli. Che cosa comporta la modifica è il costo della lettura e scrittura dei dati sul disco quando si usano i vari livelli di matrice (ad esempio RAID 0 o RAID 1 o RAID 5).

Come RAID 0, i dati vengono suddivisi in tutti i dischi RAID impostati. Ciò significa che durante un'operazione di lettura o di un'operazione di scrittura, una parte dei dati viene effettuato il pull da o il push a ogni disco, aumentando la quantità di dati che possono far passare il sistema nello stesso periodo di tempo. Di conseguenza, in un secondo, su ogni asse (anche in questo caso presupponendo che le unità di 10.000 RPM), 100 operazioni dei / o possono essere eseguite. La quantità totale dei / o che possono essere supportati è N spindle 100 volte i/o al secondo per ogni asse (produce 100 * N i/o al secondo).

![Unità logiche d:](media/capacity-planning-considerations-logical-d-drive.png)

In RAID 1, i dati viene eseguito il mirroring (duplicati) attraverso una coppia di assi per la ridondanza. Di conseguenza, quando viene eseguita un'operazione dei / o lettura, i dati possono essere letti da entrambi gli assi nel set. Ciò rende effettivamente la capacità dei / o da entrambi i dischi disponibili durante un'operazione di lettura. Il problema è guadagno di operazioni che non scrivono alcun vantaggio per le prestazioni in un RAID 1. Questo avviene perché gli stessi dati devono essere scritto per entrambe le unità ai fini di ridondanza. Benché non richiedere più, quando la scrittura dei dati verifica contemporaneamente in entrambi assi, perché entrambi assi sono occupate duplicare i dati, un'operazione dei / o scrittura impedisce essenzialmente due operazioni di lettura che si verifichi. Di conseguenza, i costi dei / o ogni scrittura due lettura i/o. È possibile creare una formula da tali informazioni per determinare il numero totale di operazioni dei / o che si sono verificati:  

> *I/o di lettura* + 2 &times; *i/o scrivere* = *totale i/o disponibile su disco utilizzato*  

Quando il rapporto di letture e scritture e il numero di spindle sono noti, l'equazione seguente può essere derivata dall'equazione del precedente per identificare il / o massima che può essere supportato dall'array di:  

> *Numero massimo di IOPS per ogni spindle* &times; 2 spindle &times; [( *% letture* +  *% scritture*) &divide; ( *% letture* + 2 &times; *% scritture*)] = *Total IOPS*

RAID 1 + 0, si comporta esattamente come RAID 1 riguardanti l'inconveniente di lettura e scrittura. Tuttavia, il / o ora con striping tra ogni set con mirroring. Se  

> *Numero massimo di IOPS per ogni spindle* &times; 2 spindle &times; [( *% letture* +  *% scritture*) &divide; ( *% letture* + 2 &times; *% scritture*)] = *totale i/o*  

in un set RAID 1, quando una molteplicità (*N*) di RAID 1 set di striping, diventa il / o totale che può essere elaborato N &times; i/o per ogni set di RAID 1:  

> *N* &times; {*numero massimo di IOPS per ogni asse* &times; 2 spindle &times; [( *% letture* +  *% scritture*) &divide; ( *% Letture* + 2 &times; *% scritture*)]} = *Total IOPS*

RAID 5, talvolta detta *N* + 1 RAID, i dati vengono archiviati con striping tra *N* parità e spindle le informazioni vengono scritte di spindle "+ 1". RAID 5, tuttavia, è molto più costoso durante l'esecuzione di un / o di scrittura rispetto a RAID 1 o 1 + 0. Il sistema RAID 5 esegue il processo seguente ogni volta che viene inviato un / o scrittura nella matrice:

1. Leggere i dati vecchi
1. Leggere la parità vecchia
1. Scrivere i nuovi dati
1. Scrivere il nuovo parità

Come tutte le richieste i/o scrittura viene inviata dal sistema operativo per il controller dell'array richiede quattro operazioni dei / o per il completamento, le richieste di scrittura inviate richiedono quadruplo del tempo per completare una singola lettura i/o. Per derivare una formula per convertire le richieste dei / o dal punto di vista di sistema operativo a cui si è verificata da stringhe:  

> *I/o di lettura* + 4 &times; *i/o di scrittura* = *totale i/o*  

Allo stesso modo in un set RAID 1, quando il rapporto di letture e scritture e il numero di spindle è noto, l'equazione seguente può essere derivata dall'equazione sopra riportato per identificare il / o massima che può essere supportato dalla matrice (si noti che non include il numero totale di assi e "unità" perse a parità):  

> *Numero di IOPS per spindle* &times; (*spindle* – 1) &times; [( *% letture* +  *% scritture*) &divide; ( *% Letture* + 4 &times; *% scritture*)] = *Total IOPS*

### <a name="introducing-sans"></a>Introduzione a nomi alternativi del soggetto

Espansione della complessità del sottosistema di archiviazione, quando una rete SAN viene introdotto nell'ambiente, i principi di base descritti non cambiano, tuttavia il comportamento dei / o per tutti i sistemi connessi alla rete SAN deve essere preso in considerazione. Uno dei principali vantaggi nell'uso della SAN è una quantità di ridondanza aggiuntiva rispetto all'archiviazione collegati internamente o esternamente, ora la pianificazione della capacità deve prendere in account esigenze di tolleranza d'errore. Inoltre, vengono introdotti altri componenti che devono essere valutati. Suddividendo una SAN nelle parti:

- Unità disco rigido SCSI o Fibre Channel
- Backplane del canale unità di archiviazione
- Unità di archiviazione
- Modulo controller di archiviazione
- Interruttori SAN
- HBA
- Il bus PCI

Quando si progetta un sistema per la ridondanza, componenti aggiuntivi sono inclusi per contenere il rischio di errore. È molto importante, quando pianificazione della capacità, per escludere il componente ridondante dalle risorse disponibili. Ad esempio, se la rete SAN è due moduli controller, la capacità dei / o del modulo di un controller è tutto ciò che deve essere utilizzato per totale velocità effettiva dei / o disponibile per il sistema. Ciò è dovuto al fatto che se un controller non riesce, sarà necessario l'intero carico dei / o richiesta da tutti i sistemi connessi da elaborare tramite il controller rimanente. Come la pianificazione della capacità tutto avviene per periodi di utilizzo, ai componenti ridondanti dovrebbero non essere sottoposte a factoring nelle risorse disponibili e pianificato picchi di utilizzo non devono superare i % 80 saturazione del sistema (per soddisfare i picchi o anomalo del sistema comportamento). Analogamente, il commutatore, unità di archiviazione e spindle ridondanti SAN non devono essere incluso nei calcoli dei / o.

Quando l'analisi del comportamento del disco rigido SCSI o Fibre Channel, il metodo di analisi del comportamento, come descritto in precedenza non cambia. Anche se sono presenti alcuni vantaggi e svantaggi di ogni protocollo, il fattore di limitazione in base al disco è la limitazione meccanica del disco rigido.

Il canale nell'unità di archiviazione è analizzare esattamente gli stessi del calcolo le risorse disponibili sul bus SCSI o larghezza di banda (ad esempio 20 MB/s) diviso per le dimensioni di blocco (ad esempio 8 KB). In cui questo si differenzia dall'esempio precedente semplice è l'aggregazione di più canali. Ad esempio, se sono presenti 6 canali, ognuno dei quali supporta velocità di trasferimento massimo di 20 MB/s, la quantità totale dei / o e il trasferimento dati disponibile è 100 MB/s (questo è corretto, è non 120 MB/s). Anche in questo caso, la tolleranza di errore è un lettore principali in questo calcolo, in caso di perdita di un intero canale, il sistema è left solo con 5 canali funzionanti. Di conseguenza, per assicurarsi di continuare a soddisfare le aspettative delle prestazioni in caso di errore, velocità effettiva totale per tutti i canali di archiviazione non deve superare 100 MB/s (ciò presuppone carico e tolleranza di errore è distribuita uniformemente tra tutti i canali). Questa trasformazione in un profilo dei / o si basa sul comportamento dell'applicazione. Nel caso di Active Directory Jet i/o, ciò verrebbe correlato a circa 12.500 i/o al secondo (100 MB/s &divide; 8 KB per i/o).

Successivamente, ottenere le specifiche del produttore per i moduli controller è necessario per acquisire familiarità con la velocità effettiva di che ogni modulo può supportare. In questo esempio, la rete SAN è che i/o supporto 7.500 brevetti ogni due moduli del controller. La velocità effettiva totale del sistema potrebbe essere 15.000 IOPS se non si desidera la ridondanza. Per il calcolo della velocità effettiva massima in caso di errore, la limitazione è la velocità effettiva di un controller o 7.500 IOPS. Questa soglia è ben di sotto il numero massimo IOPS (presupponendo che le dimensioni di blocchi di 4 KB) 12.500 che può essere supportati da tutti i canali di archiviazione e di conseguenza, è attualmente il collo di bottiglia nell'analisi. Ancora per ai fini della pianificazione, il / o massimo desiderato pianificare di sarebbe 10,400 i/o.

Quando i dati viene chiuso il modulo controller, passa una connessione Fibre Channel da 1 GB/s (o da 1 Gigabit al secondo). Per correlare questo con altri criteri di misurazione, 1 GB/s si trasforma in 128 MB/s (1 GB/s &divide; Mbps)*(8). Poiché questo è superiore ai limiti di larghezza di banda totale in tutti i canali in unità di archiviazione (100 MB/s), questo verrà non creare colli di bottiglia del sistema. Inoltre, poiché questa è solo uno dei due canali (di 1 GB/s Fibre Channel connessione aggiuntiva in corso per la ridondanza), se una connessione non riesce, la connessione rimanente dispone ancora di una capacità sufficiente per gestire tutto il trasferimento dei dati richiesto.

Indirizzamento al server, i dati verrà probabilmente transito un commutatore di rete SAN. Poiché il commutatore di rete SAN deve elaborare la richiesta dei / o in ingresso e in avanti attraverso la porta appropriata, il commutatore avrà un limite alla quantità dei / o che possono essere gestiti, tuttavia, le specifiche di produttori sarà necessarie per determinare quali tale limite è. Ad esempio, se sono presenti due opzioni e ogni switch può gestire 10.000 IOPS, velocità effettiva totale sarà 20.000 IOPS. Anche in questo caso, tolleranza di errore in fase di un problema, se un commutatore ha esito negativo, la velocità effettiva totale del sistema verrà 10.000 IOPS. Come si desidera non supera l'80% di utilizzo in condizioni normali, utilizzando non più di 8000 i/o deve essere la destinazione.

Infine, la scheda bus host installata nel server avrebbe anche un limite alla quantità dei / o che è possibile gestire. In genere, una scheda HBA secondo viene installato per la ridondanza, ma esattamente come con il commutatore di rete SAN, durante il calcolo dei / o massima che può essere gestita, la velocità effettiva totale della *N* &ndash; HBA 1 è ciò che è la massima scalabilità del sistema.

### <a name="caching-considerations"></a>Considerazioni sulla memorizzazione nella cache

Le cache sono uno dei componenti che possono influire significativamente sulle prestazioni complessive in qualsiasi momento nel sistema di archiviazione. Analisi dettagliata sugli algoritmi di memorizzazione nella cache non rientra nell'ambito di questo articolo. Tuttavia, alcune istruzioni di base sulla memorizzazione nella cache in sottosistemi di dischi sono opportuno che illumina:

- La memorizzazione nella cache i/o scrittura sequenziale sostenuta migliorata esegue perché consente di molte operazioni di scrittura più piccole in blocchi più grandi dei / o del buffer e destaging nell'archiviazione in dimensioni del blocco di un numero inferiore, ma di dimensioni maggiori. Ciò ridurrà totale i/o casuali e totale i/o sequenziali, assicurando la disponibilità di altre risorse per gli altri i/o.
- La memorizzazione nella cache non migliorare la velocità effettiva dei / o scrittura sostenuta del sottosistema di archiviazione. Consente solo per le operazioni di scrittura essere memorizzati nel buffer fino a quando non le assi sono disponibili per eseguire il commit dei dati. Quando tutti i/o disponibile di spindle nel sottosistema di archiviazione è satura per lunghi periodi di tempo, la cache alla fine si riempirà completamente. Per vuotare la cache, un tempo sufficiente tra i picchi o spindle aggiuntivi, devi essere allocato per offrire sufficiente i/o per consentire alla cache di scaricamento.

  Le cache più grande consentono solo la maggiore quantità di dati memorizzato nel buffer. Ciò significa che possono essere accolte periodi più lunghi di saturazione.

  In un sottosistema di archiviazione funziona normalmente, il sistema operativo si verificheranno ottenere prestazioni migliori quando i dati solo da scrivere nella cache. Dopo che il contenuto multimediale sottostante è satura con i/o, riempirà la cache e le prestazioni di scrittura verranno restituito alla velocità del disco.

- Quando la memorizzazione nella cache di lettura i/o, lo scenario in cui la cache è più vantaggiosa è quando i dati vengono archiviati in modo sequenziale sul disco e della cache possono read-ahead (rende il presupposto che il settore successivo contiene i dati che verranno richiesto successivamente).
- Durante la lettura i/o è casuale, la memorizzazione nella cache al controller dell'unità è improbabile fornire qualsiasi funzionalità avanzata per la quantità di dati che possono essere letti dal disco. Qualsiasi miglioramento è inesistente se il sistema operativo o le dimensioni di cache basati sull'applicazione sono maggiore delle dimensioni di cache basata su hardware.

  Nel caso di Active Directory, la cache è limitata solo dalla quantità di RAM.

### <a name="ssd-considerations"></a>Considerazioni su unità SSD

SSDs sono qualcosa di completamente diverso rispetto ai dischi rigidi basati su spindle. Restano tuttavia i due criteri di chiave: "Il numero di IOPS di gestire?" e "Qual è la latenza per le operazioni IOPS?" Rispetto ai dischi rigidi basati su spindle, SSDs può gestire volumi più elevati dei / o e può avere latenze più basse. In generale e al momento della stesura di questo articolo, sebbene siano ancora costosi in un confronto di costo per Gigabyte unità SSD, sono poco costose in termini di costo per ogni lettura dei / O e meritano una considerazione significativi in termini di prestazioni di archiviazione.

Considerazioni:

- IOPS sia le latenze sono molto soggettive per le progettazioni di produttore e in alcuni casi sono state osservate a prestazioni inferiori rispetto a mandrino basati su tecnologie. In breve, è più importante esaminare e convalidare le specifiche del produttore dall'unità e non presupporre che eventuali disposizioni generali.
- Tipi di operazioni IOPS possono avere numeri molto diversi a seconda se è di lettura o scrittura. Active Directory Domain services, in generale, in corso prevalentemente basati su lettura, sarà meno interessate più alcuni altri scenari di applicazione.
- "Scrittura resistenza" – questo è il concetto che le celle SSD verranno infine wear out. Diversi produttori affrontare questa sfida modi diversi. Almeno per l'unità di database, il profilo dei / o lettura prevalentemente consente di passare l'importanza di questo problema, come i dati non sono estremamente volatili.

### <a name="summary"></a>Riepilogo

Un modo per pensare di archiviazione è vengono disegnati plumbing componenti del nucleo familiare. Si supponga il numero di IOPS del supporto che i dati vengono archiviati in sia esaurire le risorse principali componenti del nucleo familiare. Quando questo viene ostruito (ad esempio le radici nella pipe) o limitato (è compresso o troppo piccolo), tutti i sink in famiglia eseguire il backup quando è in corso una quantità eccessiva water utilizzato (numero eccessivo di guest). Questo comportamento è perfettamente analogo a un ambiente condiviso in cui uno o più sistemi stanno sfruttando l'archiviazione condivisa su una rete SAN/NAS/iSCSI con lo stesso supporto sottostante. È possono eseguire diversi approcci per risolvere i diversi scenari:

- Lo svuotamento dimensioni insufficienti o compresso richiede una sostituzione completa di scalabilità e la correzione. Questo dovrebbe essere simile all'aggiunta nel nuovo hardware o la ridistribuzione dei sistemi che utilizzano l'archiviazione condivisa in tutta l'infrastruttura.
- Una barra verticale "ostruita" significa in genere identificazione di uno o più problemi che causa l'errore e la rimozione di tali problemi. In uno scenario di archiviazione può essere archiviazione di backup o di sistema a livello, sincronizzato le analisi antivirus in tutti i server e in esecuzione software deframmentazione sincronizzato durante periodi di massima attività.

In tutte le strutture di plumbing di svuotamento più feed in esaurire le risorse principali. Se qualsiasi elemento smette di uno di tali svuotamento o un punto di giunzione, solo gli elementi protetti da tale giunzione punto eseguire il backup. In uno scenario di archiviazione, potrebbe trattarsi di un cambio di overload (scenario con SAN/NAS/iSCSI), problemi di compatibilità dei driver (errata del driver/HBA Firmware/storport.sys combinazione) o backup/antivirus/la deframmentazione. Per determinare se lo spazio di archiviazione "pipe" è sufficientemente grande, devono essere misurata dimensioni i/o e IOPS. Per ogni giunto aggiungerli contribuiscono a garantire adeguata "diametro pipe".

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>Appendice D - discussione sulla risoluzione dei problemi di archiviazione - ambienti in cui fornire almeno la maggior parte di RAM perché le dimensioni del database non sono un'opzione praticabile

È utile comprendere il motivo per cui queste indicazioni presenti in modo che le modifiche della tecnologia di archiviazione possono essere risolto. Queste indicazioni sono disponibili per due motivi. Il primo è l'isolamento dei / o, in modo che i problemi di prestazioni (, effettua il paging) su spindle il sistema operativo non sulle prestazioni del database e i profili dei / o. Il secondo è che i file di log per Active Directory Domain Services (e la maggior parte dei database) sono sequenziali e memorizza nella cache e unità disco rigido basato su spindle dispone di un vantaggio notevole delle prestazioni quando usato con i/o sequenziali rispetto ai modelli dei / o più casuali del sistema operativo e quasi unità di database puramente casuali modelli dei / o di dominio di Active Directory. Isolando il / o sequenziali in un'unità fisica separata, è possibile aumentare la velocità effettiva. Il problema presentato da opzioni di archiviazione attuali è che i presupposti fondamentali alla base di queste indicazioni non sono più true. In molti virtualizzare gli scenari di archiviazione, ad esempio iSCSI, i file di immagine disco virtuale SAN e NAS, la media è condiviso tra più host di archiviazione sottostante, pertanto completamente la negazione di "isolamento dei / o" sia gli aspetti di "Ottimizzazione dei / o sequenziale". In effetti questi scenari di aggiungere un ulteriore livello di complessità in quanto degli altri host l'accesso ai file multimediali condivisi può influire negativamente la velocità di risposta al controller di dominio.

Nella pianificazione delle prestazioni di archiviazione, sono disponibili tre categorie da considerare: stato della cache a freddo, stato riscaldato cache e backup/ripristino. Lo stato della cache a freddo si verifica negli scenari, ad esempio quando il controller di dominio viene inizialmente riavviato o viene riavviato il servizio Active Directory e non sono presenti dati di Active Directory nella RAM. Stato della cache a caldo è dove il controller di dominio è in uno stato stabile e il database viene memorizzato nella cache. Questi sono importanti da notare è determineranno i profili di prestazioni molto diversi e con quantità di RAM sufficiente per memorizzare nella cache l'intero database non di migliorare le prestazioni quando la cache a freddo. È possibile considerare la progettazione delle prestazioni per questi due scenari con la stessa analogia del seguente, riscaldamento della cache a freddo è uno "sprint" e l'esecuzione di un server con una cache a caldo è "marathon".

Per le cache a freddo e scenario relativo alla cache a caldo, la domanda diventa la velocità con cui lo spazio di archiviazione può spostare i dati dal disco in memoria. Preparazione della cache è uno scenario in cui, nel corso del tempo le prestazioni migliorano quanto più query riutilizzare i dati, la cache hit aumenti e riduce la frequenza di dover accedere al disco. Di conseguenza, si riduce l'impatto negativo sulle prestazioni di andare al disco. Ogni riduzione delle prestazioni è solo temporaneo durante l'attesa per la cache a caldo e raggiungere la massima, dipendente da sistema dimensione massima consentita. La conversazione può essere semplificata per la velocità possono aver ottenuti i dati di fuori del disco ed è una misura semplice del valore di IOPS disponibili per Active Directory, che è soggettiva per IOPS disponibile dalla risorsa di archiviazione sottostante. Da una prospettiva di pianificazione, poiché il riscaldamento gli scenari di cache e backup/ripristino si verificano in via eccezionale, in genere si verificano gli orari di minore e sono soggettivo per il carico del controller di dominio, consigli generali non esistono, ad eccezione in quanto queste attività pianificata per le ore non di punta.

Servizi di dominio Active Directory, nella maggior parte degli scenari, è prevalentemente di lettura i/o, in genere un rapporto di scrittura read/10% 90%. I/o lettura spesso tende a diventare il collo di bottiglia per l'esperienza utente e con i/o scrittura, le cause scrivere una riduzione delle prestazioni. Come i/o per il database Ntds. dit è principalmente casuale, le cache tendono a fornire vantaggi minimi per la lettura dei / o, rendendo che sempre più importante per configurare l'archiviazione per leggere i/o profilo correttamente.

Per le condizioni operative normali, l'obiettivo di pianificazione di archiviazione è ridurre al minimo i tempi di attesa di una richiesta da Active Directory Domain Services deve essere restituito dal disco. Questo significa essenzialmente che il numero di in attesa e in sospeso i/o è minore o uguale al numero di percorsi su disco. Esistono diversi modi per misurare questa. In una scenario di monitoraggio delle prestazioni, la raccomandazione generale è il disco logico ( *\<unità di Database NTDS\>* ) \Avg letture disco/sec essere inferiore a 20 ms. La soglia operativa desiderata deve essere molto inferiore, preferibilmente come vicino la velocità della risorsa di archiviazione possibili, nell'intervallo da 2 a 6 millisecondi (.002 a.006 secondo) a seconda del tipo di archiviazione.

Esempio:

![Grafico latenza archiviazione](media/capacity-planning-considerations-storage-latency.png)

L'analisi del grafico:

- **Ovale di colore verde a sinistra:** rimanga coerenza a 10 ms di latenza. Il carico aumenta da 800 IOPS a 2400 IOPS. Questo è il limite minimo assoluto di rapidità con cui può essere elaborata una richiesta dei / o dall'archivio sottostante. Ciò è soggetto a specifiche della soluzione di archiviazione.
- **Forma ovale bordeaux che indica a destra, ovvero** la velocità effettiva rimane invariata dall'uscita del cerchio verde tramite alla fine della raccolta di dati mentre la latenza in continua crescita. Ciò è dimostrare che, quando i volumi di richieste superano i limiti fisici dell'archiviazione sottostante, maggiore sarà il tempo le richieste di impiegare che si trova nella coda in attesa di essere inviati al sottosistema di archiviazione.

Applicando queste informazioni:

- **Impatto di un'appartenenza di query utente di un gruppo di grandi dimensioni –** presuppongono ciò richiede la lettura di 1 MB di dati dal disco, la quantità dei / o e il tempo necessario può essere valutata come indicato di seguito:
  - Pagine di database Active Directory sono 8 KB di dimensioni. 
  - Almeno 128 pagine dovrà essere letta dal disco. 
  - Supponendo che nulla è memorizzato nella cache, il tetto minimo (10 ms) verranno visualizzate richiedono almeno 1,28 secondi per caricare i dati dal disco per restituirlo al client. Richiederà a 20 ms, in cui la velocità effettiva in archiviazione dispone di tempo raggiunge il limite massimo ed è anche il numero massimo consigliato, 2,5 secondi per ottenere i dati dal disco per restituirlo all'utente finale.  
- **Con quale frequenza verrà preparata la cache –** presupponendo che il carico di lavoro sta per ottimizzare la velocità effettiva in questo esempio di archiviazione, la cache verrà warm con una frequenza di IOPS 2400 &times; 8 KB per i/o. In alternativa, circa 20 MB al secondo per secondo, il caricamento circa 1 GB di database nella RAM ogni 53 secondi.

> [!NOTE]
> È norma per brevi periodi osservare le latenze salire quando i componenti di lettura o scrittura su disco, ad esempio quando il sistema sottoposti a backup o quando Active Directory Domain Services è in esecuzione operazioni di garbage collection in modo aggressivo. Per gestire questi eventi periodici, è necessario specificare altre chat room head sopra i calcoli. L'obiettivo principale è fornire velocità effettiva sufficiente per supportare questi scenari senza influire sulla funzione normale.

Come si può notare, è previsto un limite fisico in base alla struttura di archiviazione per la velocità della cache può eventualmente warm. Che cosa verrà warm cache sono in arrivo delle richieste client fino a velocità in grado di fornire l'archiviazione sottostante. Esecuzione degli script "pre-warm" cache durante le ore di picco fornirà la concorrenza di carico basato su dalle richieste client reali. Che può influenzare negativamente la distribuzione dei dati che i client devono innanzitutto perché, per impostazione predefinita, la concorrenza per le risorse del disco insufficienti verrà generato come artificiali tentativi per preparare la cache verranno caricati i dati che non sono rilevanti per i client di contattare il controller di dominio.
