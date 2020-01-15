---
title: Ottimizzazione di IIS 10,0
description: Ottimizzazione delle prestazioni raccomandazioni per i server Web IIS 10,0 in Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 8b86bf779a4ea9d67f959dacf125a98a8e26a729
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947130"
---
# <a name="tuning-iis-100"></a>Ottimizzazione di IIS 10,0

Internet Information Services (IIS) 10,0 è incluso in Windows Server 2016. Usa un modello di processo simile a quello di IIS 8,5 e IIS 7,0. Un driver Web in modalità kernel (http. sys) riceve e instrada le richieste HTTP e soddisfa le richieste dalla relativa cache di risposta. I processi di lavoro si registrano per gli spazi sottospazi URL e http. sys instrada la richiesta al processo appropriato (o a un set di processi per i pool di applicazioni).

HTTP. sys è responsabile della gestione delle connessioni e della gestione delle richieste. La richiesta può essere servita dalla cache HTTP. sys o passata a un processo di lavoro per una gestione aggiuntiva. È possibile configurare più processi di lavoro, che forniscono isolamento a costi ridotti. Per ulteriori informazioni sul funzionamento della gestione delle richieste, vedere la figura seguente:

![gestione delle richieste in IIS 10,0](../../media/perftune-guide-iis-request-handling.png)

HTTP. sys include una cache di risposta. Quando una richiesta corrisponde a una voce nella cache di risposta, HTTP. sys invia la risposta della cache direttamente dalla modalità kernel. Alcune piattaforme di applicazioni Web, ad esempio ASP.NET, forniscono meccanismi per consentire la memorizzazione nella cache in modalità kernel di qualsiasi contenuto dinamico. Il gestore di file statici in IIS 10,0 memorizza automaticamente nella cache i file richiesti di frequente in http. sys.

Poiché in un server Web sono disponibili componenti in modalità kernel e modalità utente, entrambi i componenti devono essere ottimizzati per ottenere prestazioni ottimali. Per questo motivo, l'ottimizzazione di IIS 10,0 per un carico di lavoro specifico include la configurazione dei seguenti elementi:

-   HTTP. sys e la cache in modalità kernel associata

-   Processi di lavoro e IIS in modalità utente, inclusa la configurazione del pool di applicazioni

-   Determinati parametri di ottimizzazione che influiscono sulle prestazioni

Nelle sezioni seguenti viene illustrato come configurare gli aspetti in modalità kernel e in modalità utente di IIS 10,0.

## <a name="kernel-mode-settings"></a>Impostazioni della modalità kernel

Le impostazioni HTTP. sys correlate alle prestazioni rientrano in due ampie categorie: gestione della cache e connessione e gestione delle richieste. Tutte le impostazioni del registro di sistema vengono archiviate nella seguente voce del registro di sistema:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**Nota**  se il servizio HTTP è già in esecuzione, è necessario riavviarlo per rendere effettive le modifiche.

Â 

## <a name="cache-management-settings"></a>Impostazioni di gestione della cache

Un vantaggio fornito da HTTP. sys è una cache in modalità kernel. Se la risposta è nella cache in modalità kernel, è possibile soddisfare una richiesta HTTP interamente dalla modalità kernel, che riduce significativamente il costo della CPU per la gestione della richiesta. Tuttavia, la cache in modalità kernel di IIS 10,0 è basata sulla memoria fisica e il costo di una voce è la memoria occupata.

Una voce nella cache è utile solo quando viene usata. Tuttavia, la voce utilizza sempre la memoria fisica, indipendentemente dal fatto che la voce venga utilizzata o meno. È necessario valutare l'utilità di un elemento nella cache (il risparmio per poterlo servire dalla cache) e il relativo costo (la memoria fisica occupata) per tutta la durata della voce considerando le risorse disponibili (CPU e memoria fisica) e il carico di lavoro requisiti. HTTP. sys tenta di gestire solo gli elementi utili e attivamente accessibili nella cache, ma è possibile aumentare le prestazioni del server Web ottimizzando la cache HTTP. sys per carichi di lavoro specifici.

Di seguito sono riportate alcune impostazioni utili per la cache in modalità kernel HTTP. sys:

-   **UriEnableCache** Valore predefinito: 1

    Un valore diverso da zero Abilita la risposta in modalità kernel e la memorizzazione nella cache dei frammenti. Per la maggior parte dei carichi di lavoro, la cache deve rimanere abilitata. Provare a disabilitare la cache se si prevede una risposta molto bassa e la memorizzazione nella cache dei frammenti.

-   **UriMaxCacheMegabyteCount** Valore predefinito: 0

    Valore diverso da zero che specifica la memoria massima disponibile per la cache in modalità kernel. Il valore predefinito, 0, consente al sistema di regolare automaticamente la quantità di memoria disponibile per la cache.

    **Nota** Specificando le dimensioni viene impostato solo il valore massimo e il sistema potrebbe non consentire la crescita della cache fino alla dimensione massima configurata.

    Â 

-   **UriMaxUriBytes** Valore predefinito: 262144 byte (256 KB)

    Dimensione massima di una voce nella cache in modalità kernel. Le risposte o i frammenti più grandi di questo non vengono memorizzati nella cache. Se si dispone di memoria sufficiente, provare ad aumentare il limite. Se la memoria è limitata e le voci di grandi dimensioni sono più piccole, potrebbe essere utile ridurre il limite.

-   **UriScavengerPeriod** Valore predefinito: 120 secondi

    La cache HTTP. sys viene periodicamente sottoposta a scansione da un Scavenger e le voci a cui non si accede tra le analisi dello scavenger vengono rimosse. Impostando il periodo del Scavenger su un valore elevato, viene ridotto il numero di analisi dello scavenger. Tuttavia, l'utilizzo della memoria cache può aumentare perché le voci meno recenti, a cui si accede meno di frequente, possono rimanere nella cache. L'impostazione del periodo troppo basso comporta l'analisi più frequente del Scavenger e può comportare troppi Scaricamenti e varianza della cache.

## <a name="request-and-connection-management-settings"></a>Impostazioni di gestione delle richieste e delle connessioni

In Windows Server 2016, HTTP. sys gestisce automaticamente le connessioni. Le seguenti impostazioni del registro di sistema non vengono più utilizzate:

-   **MaxConnections**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\MaxConnections
    ```

-   **IdleConnectionsHighMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsHighMark
    ```

-   **IdleConnectionsLowMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsLowMark
    ```

-   **IdleListTrimmerPeriod**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleListTrimmerPeriod
    ```

-   **RequestBufferLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\RequestBufferLookasideDepth
    ```

-   **InternalRequestLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\InternalRequestLookasideDepth
    ```


## <a name="user-mode-settings"></a>Impostazioni della modalità utente

Le impostazioni in questa sezione influiscono sul comportamento del processo di lavoro IISÂ 10,0. La maggior parte di queste impostazioni si trova nel file di configurazione XML seguente:

% SystemRoot%\\system32\\inetsrv\\config\\applicationHost. config

Utilizzare Appcmd. exe, la console di gestione IIS 10,0, i cmdlet di PowerShell per WebAdministration o IISAdministration per modificarli. La maggior parte delle impostazioni viene rilevata automaticamente e non richiede il riavvio dei processi di lavoro IIS 10,0 o del server applicazioni Web. Per ulteriori informazioni sul file applicationHost. config, vedere [Introduzione a ApplicationHost. config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig).


## <a name="ideal-cpu-setting-for-numa-hardware"></a>Impostazione della CPU ideale per hardware NUMA

A partire da Windows 2016, IIS 10,0 supporta l'assegnazione automatica della CPU ideale per i thread del pool di thread per migliorare le prestazioni e la scalabilità nell'hardware NUMA. Questa funzionalità è abilitata per impostazione predefinita e può essere configurata tramite la chiave del registro di sistema seguente:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

Se questa funzionalità è abilitata, gestione thread IIS consente di distribuire uniformemente i thread del pool di thread IIS tra tutte le CPU in tutti i nodi NUMA in base ai carichi correnti. In generale, è consigliabile lasciare invariata l'impostazione predefinita per l'hardware NUMA.

**Nota**  l'impostazione della CPU ideale è diversa dalle impostazioni di assegnazione del nodo NUMA del processo di lavoro (NumaNodeAssignment e numaNodeAffinityMode) introdotte in [Impostazioni CPU per un pool di applicazioni](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu). L'impostazione della CPU ideale influiscono sul modo in cui IIS distribuisce i thread del pool di thread, mentre le impostazioni di assegnazione del nodo NUMA del processo di lavoro determinano il nodo NUMA avviato da un processo di lavoro.

## <a name="user-mode-cache-behavior-settings"></a>Impostazioni del comportamento della cache in modalità utente

Questa sezione descrive le impostazioni che influiscono sul comportamento di memorizzazione nella cache in IISÂ 10,0. La cache in modalità utente viene implementata come un modulo che rimane in ascolto degli eventi di memorizzazione nella cache globale generati dalla pipeline integrata. Per disabilitare completamente la cache in modalità utente, rimuovere il modulo FileCacheModule (cachfile. dll) dall'elenco dei moduli installati nella sezione di configurazione System. webserver/globalModules in applicationHost. config.

**System. webserver/Caching**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|Abilitato|Disabilita la cache IIS in modalità utente quando è impostata su **false**. Quando la percentuale di riscontri nella cache è molto ridotta, è possibile disabilitare completamente la cache per evitare il sovraccarico associato al percorso del codice della cache. La disabilitazione della cache in modalità utente non comporta la disabilitazione della cache in modalità kernel.|True|
|enableKernelCache|Disabilita la cache in modalità kernel quando è impostata su **false**.|True|
|maxCacheSize|Limita le dimensioni della cache in modalità utente di IIS alla dimensione specificata in megabyte. IIS regola il valore predefinito in base alla memoria disponibile. Scegliere con attenzione il valore in base alle dimensioni del set di file a cui si accede di frequente rispetto alla quantità di RAM o allo spazio degli indirizzi del processo IIS.|0|
|maxResponseSize|Memorizza nella cache i file fino alla dimensione specificata. Il valore effettivo dipende dal numero e dalle dimensioni dei file più grandi nel set di dati rispetto alla RAM disponibile. La memorizzazione nella cache di file di grandi dimensioni e spesso richiesti può ridurre l'utilizzo della CPU, l'accesso al disco e le latenze associate|262144|

## <a name="compression-behavior-settings"></a>Impostazioni del comportamento di compressione

IIS a partire da 7,0 comprime il contenuto statico per impostazione predefinita. Inoltre, la compressione del contenuto dinamico è abilitata per impostazione predefinita quando viene installato DynamicCompressionModule. La compressione riduce l'utilizzo della larghezza di banda ma aumenta l'utilizzo della CPU. Se possibile, il contenuto compresso viene memorizzato nella cache in modalità kernel. A partire da 8,5, IIS consente di controllare la compressione in modo indipendente per il contenuto statico e dinamico. Il contenuto statico si riferisce in genere al contenuto che non cambia, ad esempio i file GIF o HTM. Il contenuto dinamico viene in genere generato da script o codice sul server, ovvero pagine ASP.NET. È possibile personalizzare la classificazione di una determinata estensione come statica o dinamica.

Per disabilitare completamente la compressione, rimuovere StaticCompressionModule e DynamicCompressionModule dall'elenco dei moduli nella sezione System. webserver/globalModules in applicationHost. config.

**System. webserver/httpCompression**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|Abilita o Disabilita la compressione se l'utilizzo della CPU percentuale corrente supera o scende al di sotto dei limiti specificati.<br><br>A partire da IIS 7,0, la compressione viene disabilitata automaticamente se la CPU a stato costante aumenta al di sopra della soglia di disabilitazione. La compressione è abilitata se la CPU scende al di sotto della soglia di abilitazione.|50, 100, 50 e 90 rispettivamente|
|directory|Specifica la directory in cui le versioni compresse dei file statici vengono temporaneamente archiviate e memorizzate nella cache. Se si accede di frequente, provare a trasferire questa directory dall'unità di sistema.|%SystemDrive%\inetpub\temp\IIS file compressi temporanei|
|doDiskSpaceLimiting|Specifica se esiste un limite per la quantità di spazio su disco che tutti i file compressi possono occupare. I file compressi vengono archiviati nella directory di compressione specificata dall'attributo di **directory** .|True|
|maxDiskSpaceUsage|Specifica il numero di byte di spazio su disco che i file compressi possono occupare nella directory di compressione.<br><br>Questa impostazione potrebbe dover essere aumentata se la dimensione totale di tutto il contenuto compresso è troppo grande.|100 MB|

**System. webserver/urlCompression**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|doStaticCompression|Specifica se il contenuto statico è compresso.|True|
|doDynamicCompression|Specifica se il contenuto dinamico è compresso.|True|

**Nota** Per i server che eseguono IIS 10,0 con un utilizzo medio della CPU ridotto, è consigliabile abilitare la compressione per il contenuto dinamico, soprattutto se le risposte sono di grandi dimensioni. Questa operazione deve essere eseguita prima in un ambiente di testing per valutare l'effetto sull'utilizzo della CPU dalla linea di base.


### <a name="tuning-the-default-document-list"></a>Ottimizzazione dell'elenco dei documenti predefiniti

Il modulo documento predefinito gestisce le richieste HTTP per la radice di una directory e le converte in richieste per un file specifico, ad esempio default. htm o index. htm. In media, aroundÂ il 25% di tutte le richieste in Internet passa attraverso il percorso del documento predefinito. Questo problema varia significativamente per i singoli siti. Quando una richiesta HTTP non specifica un nome di file, il modulo del documento predefinito Cerca nell'elenco dei documenti predefiniti consentiti per ogni nome nella file system. Ciò può influire negativamente sulle prestazioni, soprattutto se il raggiungimento del contenuto richiede la round trip di rete o il tocco di un disco.

È possibile evitare il sovraccarico selezionando in modo selettivo i documenti predefiniti e riducendo o ordinando l'elenco di documenti. Per i siti Web che utilizzano un documento predefinito, è consigliabile ridurre l'elenco solo ai tipi di documento predefiniti utilizzati. Inoltre, ordinare l'elenco in modo che inizi con il nome del file di documento predefinito a cui si accede più di frequente.

È possibile impostare selettivamente il comportamento predefinito del documento su URL specifici personalizzando la configurazione all'interno di un tag location in applicationHost. config oppure inserendo un file Web. config direttamente nella directory del contenuto. Questo consente un approccio ibrido, che Abilita i documenti predefiniti solo quando sono necessari e imposta l'elenco sul nome file corretto per ogni URL.

Per disabilitare completamente i documenti predefiniti, rimuovere DefaultDocumentModule dall'elenco dei moduli nella sezione System. webserver/globalModules in applicationHost. config.

**System. webserver/defaultDocument**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|enabled|Specifica che i documenti predefiniti sono abilitati.|True|
|elemento&gt; &lt;file|Specifica i nomi di file che sono configurati come documenti predefiniti.|L'elenco predefinito è default. htm, default. asp, index. htm, index. html, Iisstart. htm e default. aspx.|

## <a name="central-binary-logging"></a>Registrazione binaria centrale

Quando la sessione del server include numerosi gruppi di URL, il processo di creazione di centinaia di file di log formattati per i singoli gruppi di URL e la scrittura dei dati di log su un disco può utilizzare rapidamente risorse di CPU e memoria, creando in tal modo prestazioni e problemi di scalabilità. La registrazione binaria centralizzata riduce al minimo la quantità di risorse di sistema utilizzate per la registrazione, fornendo allo stesso tempo dati di log dettagliati per le organizzazioni che lo richiedono. Per l'analisi dei log in formato binario è necessario uno strumento di post-elaborazione.

È possibile abilitare la registrazione binaria centrale impostando l'attributo centralLogFileMode su CentralBinary e impostando l'attributo **Enabled** su **true**. Provare a trasferire il percorso del file di log centrale dalla partizione di sistema e su un'unità di registrazione dedicata per evitare conflitti tra le attività del sistema e le attività di registrazione.

**System. applicationHost/log**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|centralLogFileMode|Specifica la modalità di registrazione per un server. Modificare questo valore in CentralBinary per abilitare la registrazione binaria centrale.|Sito|

**System. applicationHost/log/centralBinaryLogFile**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|enabled|Specifica se è abilitata la registrazione binaria centrale.|False|
|directory|Specifica la directory in cui vengono scritte le voci di log.|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>Ottimizzazione di applicazioni e siti

Le impostazioni seguenti sono correlate al pool di applicazioni e alle ottimizzazioni del sito.

**System. applicationHost/applicationPools/applicationPoolDefaults**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|queueLength|Indica a HTTP. sys il numero di richieste accodate per un pool di applicazioni prima che le richieste future vengano rifiutate. Quando viene superato il valore per questa proprietà, IIS rifiuta le richieste successive con un errore 503.<br><br>Si consiglia di aumentare questo aspetto per le applicazioni che comunicano con gli archivi dati back-end a latenza elevata se vengono osservati 503 errori.|1000|
|enable32BitAppOnWin64|Se impostato su true, consente l'esecuzione di un'applicazione a 32 bit in un computer che dispone di un processore a 64 bit.<br><br>Se l'utilizzo della memoria rappresenta un problema, provare ad abilitare la modalità a 32 bit. Poiché le dimensioni del puntatore e le dimensioni dell'istruzione sono inferiori, le applicazioni a 32 bit utilizzano meno memoria rispetto alle applicazioni a 64 bit. Lo svantaggio dell'esecuzione di applicazioni a 32 bit in un computer a 64 bit è che lo spazio degli indirizzi in modalità utente è limitato a 4 GB.|False|

**System. applicationHost/sites/VirtualDirectoryDefault**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|allowSubDirConfig|Specifica se IIS cerca i file Web. config nelle directory del contenuto inferiori al livello corrente (true) o non cerca i file Web. config nelle directory del contenuto inferiori al livello corrente (false). Imponendo una semplice limitazione, che consente la configurazione solo nelle directory virtuali, IISÂ 10,0 sa che, a meno che **/&lt;nome&gt;. htm** sia una directory virtuale, non dovrebbe cercare un file di configurazione. Ignorare le operazioni aggiuntive sui file può migliorare significativamente le prestazioni dei siti Web con un set molto ampio di contenuto statico a cui si accede in modo casuale.|True|

## <a name="managing-iis-100-modules"></a>Gestione dei moduli di IIS 10,0

IIS 10,0 è stato sottoposto a factoring in più moduli estendibili dall'utente per supportare una struttura modulare. Questa factoring ha un costo ridotto. Per ogni modulo la pipeline integrata deve chiamare il modulo per ogni evento pertinente per il modulo. Ciò avviene indipendentemente dal fatto che il modulo debba eseguire alcuna operazione. È possibile conservare i cicli della CPU e la memoria rimuovendo tutti i moduli che non sono rilevanti per un particolare sito Web.

Un server Web ottimizzato per semplici file statici può includere solo i cinque moduli seguenti: UriCacheModule, HttpCacheModule, StaticFileModule, AnonymousAuthenticationModule e HttpLoggingModule.

Per rimuovere i moduli da applicationHost. config, rimuovere tutti i riferimenti al modulo dalle sezioni System. webServer/handlers e System. webserver/Modules, oltre a rimuovere la dichiarazione del modulo in System. webserver/globalModules.

## <a name="classic-asp-settings"></a>Impostazioni ASP classiche

Il costo principale dell'elaborazione di una richiesta ASP classica prevede l'inizializzazione di un motore di script, la compilazione dello script ASP richiesto in un modello ASP e l'esecuzione del modello nel motore di script. Sebbene il costo di esecuzione del modello dipenda dalla complessità dello script ASP richiesto, il modulo ASP classico di IIS può memorizzare nella cache i motori di script in memoria e i modelli di cache sia nella memoria che nel disco (solo se si verifica un overflow della cache dei modelli in memoria) per migliorare le prestazioni in Scenari associati alla CPU.

Le impostazioni seguenti vengono usate per configurare la cache del modello ASP classico e la cache del motore di script e non influiscono sulle impostazioni di ASP.NET.

**System. webserver/ASP/cache**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|diskTemplateCacheDirectory|Nome della directory utilizzata da ASP per archiviare i modelli compilati quando si verifica un overflow della cache in memoria.<br><br>Raccomandazione: impostare su una directory che non viene utilizzata molto spesso, ad esempio un'unità non condivisa con il sistema operativo, il registro IIS o altro contenuto di uso frequente.|Modelli compilati%SystemDrive%\inetpub\temp\ASP|
|maxDiskTemplateCacheFiles|Specifica il numero massimo di modelli ASP compilati che possono essere memorizzati nella cache su disco.<br><br>Raccomandazione: impostare sul valore massimo di 0x7FFFFFFF.|2000|
|scriptFileCacheSize|Questo attributo specifica il numero massimo di modelli ASP compilati che possono essere memorizzati nella cache in memoria.<br><br>Raccomandazione: impostare almeno il numero di script ASP richiesti di frequente serviti da un pool di applicazioni. Se possibile, impostare su tutti i modelli ASP dei limiti di memoria consentiti.|500|
|scriptEngineCacheMax|Specifica il numero massimo di motori di script che manterranno memorizzati nella cache.<br><br>Raccomandazione: impostare almeno il numero di script ASP richiesti di frequente serviti da un pool di applicazioni. Se possibile, impostare il numero di motori di script consentiti dal limite di memoria.|250|

**System. webserver/ASP/limits**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|processorThreadMax|Specifica il numero massimo di thread di lavoro per processore che possono essere creati da ASP. Aumentare se l'impostazione corrente non è sufficiente per gestire il carico, che può causare errori quando vengono gestite richieste o causare un utilizzo insufficiente delle risorse della CPU.|25|

**System. webserver/ASP/ComPlus**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|executeInMta|Impostare su **true** se vengono rilevati errori o errori quando IIS sta servendo il contenuto ASP. Questa situazione può verificarsi, ad esempio, quando si ospitano più siti isolati in cui ogni sito viene eseguito con il proprio processo di lavoro. Gli errori vengono in genere segnalati da COM+ nell'Visualizzatore eventi. Questa impostazione Abilita il modello di Apartment a thread multipli in ASP.|False|


## <a name="aspnet-concurrency-setting"></a>Impostazione della concorrenza ASP.NET

### <a name="aspnet-35"></a>ASP.NET 3.5
Per impostazione predefinita, ASP.NET limita la concorrenza delle richieste per ridurre l'utilizzo della memoria dello stato stabile nel server. Le applicazioni con concorrenza elevata potrebbero dover modificare alcune impostazioni per migliorare le prestazioni complessive. È possibile modificare questa impostazione nel file Aspnet. config:

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

L'impostazione seguente è utile per usare completamente le risorse in un sistema:

-   **maxConcurrentRequestPerCpu** Valore predefinito: 5000

    Questa impostazione limita il numero massimo di richieste ASP.NET eseguite simultaneamente in un sistema. Il valore predefinito è conservativo per ridurre l'utilizzo di memoria da parte delle applicazioni ASP.NET. Si consiglia di aumentare questo limite nei sistemi che eseguono applicazioni che eseguono operazioni di I/O lunghe e sincrone. In caso contrario, gli utenti possono riscontrare una latenza elevata a causa di errori di accodamento o di richiesta dovuti al superamento dei limiti della coda in un carico elevato quando viene usata l'impostazione predefinita.

### <a name="aspnet-46"></a>ASP.NET 4.6
Oltre all'impostazione maxConcurrentRequestPerCpu, ASP.NET 4,7 fornisce anche impostazioni per migliorare le prestazioni delle applicazioni che si basano molto sull'operazione asincrona. È possibile modificare l'impostazione nel file Aspnet. config.

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit** Valore predefinito: 90 la richiesta asincrona presenta alcuni problemi di scalabilità quando un carico enorme (oltre le funzionalità hardware) viene inserito in tale scenario. Il problema è dovuto alla natura dell'allocazione negli scenari asincroni. In queste condizioni, l'allocazione viene eseguita all'avvio dell'operazione asincrona e viene utilizzata quando viene completata. A questo punto, Itâs molto possibile che gli oggetti siano stati spostati in generazione 1 o 2 da GC. In tal caso, l'aumento del carico indicherà un aumento del numero di richieste al secondo (RPS) fino a un punto. Una volta passato questo punto, il tempo trascorso in GC inizierà a diventare un problema e il RPS inizierà a ridursi, con un effetto di scalabilità negativo. Per risolvere il problema, quando l'utilizzo della CPU supera l'impostazione di percentCpuLimit, le richieste verranno inviate alla coda nativa di ASP.NET.
-   **percentCpuLimitMinActiveRequestPerCpu** Valore predefinito: la limitazione della CPU 100 (impostazione percentCpuLimit) non è basata sul numero di richieste ma sul costo. Di conseguenza, potrebbero essere presenti solo alcune richieste che richiedono un elevato utilizzo di CPU, causando un backup nella coda nativa senza alcun modo per svuotarlo dalle richieste in ingresso. Per risolvere questo problme, è possibile usare percentCpuLimitMinActiveRequestPerCpu per assicurarsi che venga servito un numero minimo di richieste prima di avviare la limitazione delle richieste.

## <a name="worker-process-and-recycling-options"></a>Opzioni per il processo di lavoro e il riciclo

È possibile configurare le opzioni per il riciclo dei processi di lavoro IIS e fornire soluzioni pratiche a situazioni o eventi acuti senza richiedere interventi o reimpostare un servizio o un computer. Tali situazioni ed eventi includono perdite di memoria, aumento del carico di memoria o processi di lavoro non rispondenti o inattivi. In condizioni normali, le opzioni di riciclo potrebbero non essere necessarie e il riciclo può essere disattivato o il sistema può essere configurato per il riciclo molto raramente.

È possibile abilitare il riciclo dei processi per un'applicazione specifica aggiungendo attributi all'elemento **Recycling/PeriodicRestart** . L'evento di riciclo può essere attivato da diversi eventi, tra cui l'utilizzo della memoria, un numero fisso di richieste e un periodo di tempo fisso. Quando un processo di lavoro viene riciclato, le richieste in coda e in esecuzione vengono svuotate e viene avviato contemporaneamente un nuovo processo per soddisfare le nuove richieste. L'elemento **Recycling/PeriodicRestart** è per applicazione, il che significa che ogni attributo della tabella seguente viene partizionato per ogni singola applicazione.

**System. applicationHost/applicationPools/ApplicationPoolDefaults/Recycling/periodicRestart**

|Attributo|Descrizione|Valore predefinito|
|--- |--- |--- |
|memory|Abilitare il riciclo dei processi se il consumo di memoria virtuale supera il limite specificato, espressa in kilobyte. Si tratta di un'impostazione utile per computer a 32 bit con uno spazio di indirizzi piccolo e 2 GB. Questo consente di evitare richieste non riuscite a causa di errori di memoria insufficiente.|0|
|privateMemory|Abilitare il riciclo del processo se le allocazioni di memoria privata superano il limite specificato, espressa in kilobyte.|0|
|requests|Abilitare il riciclo del processo dopo un determinato numero di richieste.|0|
|ora|Abilitare il riciclo del processo dopo un periodo di tempo specificato.|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>Ottimizzazione della pagina di elaborazione dinamica del processo di lavoro

A partire da Windows Server 2012 R2, IIS offre la possibilità di configurare un processo di lavoro da sospendere dopo che è rimasto inattivo per un periodo di tempo (oltre all'opzione di terminazione, esistente da IIS 7).

Lo scopo principale del processo di lavoro inattivo per le funzionalità di terminazione del processo di lavoro e di interruzione del processo di lavoro è quello di preservare l'utilizzo della memoria nel server, poiché un sito può utilizzare una grande quantità di memoria anche se è semplicemente in attesa. A seconda della tecnologia usata nel sito (contenuto statico rispetto a ASP.NET e altri Framework), la memoria usata può essere da circa 10 MB a centinaia di MB e ciò significa che se il server è configurato con molti siti, è necessario capire le impostazioni più valide per i siti è possibile migliorare notevolmente le prestazioni dei siti attivi e sospesi.

Prima di esaminare le specifiche, è necessario tenere presente che, se non sono presenti vincoli di memoria, è consigliabile semplicemente impostare i siti in modo che non vengano mai sospesi o terminati. Dopo tutto, thereâs un valore minimo durante la terminazione di un processo di lavoro se è l'unico nel computer.

**Si noti**  nel caso in cui il sito esegua codice instabile, ad esempio codice con una perdita di memoria o in altro modo instabile, l'impostazione del sito su termina in caso di inattività può essere un'alternativa rapida e sporca per correggere il bug del codice. Questo non è un elemento da consigliare, ma in un crunch può essere preferibile usare questa funzionalità come meccanismo di pulizia, mentre una soluzione più permanente si trova nel lavoro.\]

Â 

Un altro fattore da considerare è che se il sito usa una grande quantità di memoria, il processo di sospensione stesso riceve un pedaggio, perché il computer deve scrivere i dati usati dal processo di lavoro su disco. Se il processo di lavoro usa un blocco di memoria di grandi dimensioni, la sospensione potrebbe essere più costosa del costo di dover attendere l'avvio del backup.

Per sfruttare al meglio la funzionalità di sospensione del processo di lavoro, è necessario esaminare i siti in ogni pool di applicazioni e decidere quale deve essere sospeso, che deve essere terminato e che deve essere attivo per un periodo illimitato. Per ogni azione e ogni sito, è necessario determinare il periodo di timeout ideale.

Idealmente, i siti che verranno configurati per la sospensione o la terminazione sono quelli che hanno visitatori ogni giorno, ma non sono sufficienti per garantirne l'attivazione continua. Si tratta in genere di siti con circa 20 visitatori univoci al giorno o meno. È possibile analizzare i modelli di traffico usando i file di log del sito e calcolare il traffico giornaliero medio.

Tenere presente che, una volta che un utente specifico si connette al sito, in genere rimane invariato per almeno un periodo di tempo, effettuando richieste aggiuntive e quindi il conteggio delle richieste giornaliere potrebbe non riflettere accuratamente i modelli di traffico reali. Per ottenere una lettura più accurata, è anche possibile usare uno strumento, ad esempio Microsoft Excel, per calcolare il tempo medio tra le richieste. Ad esempio:

||URL della richiesta|Tempo richiesta|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/SourceSilverLight/GeosourcewebService/Service. asmx|10:23|0:11|
|6|/SourceSilverLight/geosource. Web/GeoSearchServer... ¦.|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la... ¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap... ¦.|12:51|0:01|
|9|/rest/Services/DynamicService/Silverlight_basemap... ¦.|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache. As...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache. js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

Tuttavia, la parte più difficile è determinare l'impostazione da applicare per avere senso. In questo caso, il sito riceve una serie di richieste da parte degli utenti e la tabella precedente mostra che un totale di 4 sessioni univoche si sono verificate in un periodo di 4 ore. Con le impostazioni predefinite per la sospensione del processo di lavoro del pool di applicazioni, il sito verrebbe terminato dopo il timeout predefinito di 20 minuti, il che significa che ognuno di questi utenti avrebbe riscontrato il ciclo di rotazione del sito. Questo lo rende un candidato ideale per la sospensione del processo di lavoro, perché per la maggior parte del tempo il sito è inattivo e pertanto sospendere le risorse e consentire agli utenti di raggiungere il sito quasi immediatamente.

Una nota finale e molto importante è che le prestazioni del disco sono fondamentali per questa funzionalità. Poiché la sospensione e il processo di riattivazione comportano la scrittura e la lettura di grandi quantità di dati sul disco rigido, è consigliabile usare un disco veloce per questo. Le unità SSD (Solid State Drive) sono ideali ed è consigliabile assicurarsi che il file di paging di Windows sia archiviato (se il sistema operativo non è installato nell'unità SSD, configurare il sistema operativo per spostarvi il file di paging).

Indipendentemente dal fatto che si usi o meno un'unità SSD, è consigliabile anche correggere le dimensioni del file di paging in modo da adattarle alla scrittura dei dati di paging senza il ridimensionamento dei file. Il ridimensionamento del file di paging potrebbe verificarsi quando il sistema operativo deve archiviare i dati nel file di paging perché, per impostazione predefinita, Windows è configurato per regolarne automaticamente le dimensioni in base alle esigenze. Impostando le dimensioni su uno fisso, è possibile impedire il ridimensionamento e migliorare notevolmente le prestazioni.

Per configurare una dimensione del file di paging prefissa, è necessario calcolare le dimensioni ideali, che dipendono dal numero di siti che verranno sospesi e dalla quantità di memoria utilizzata. Se la media è di 200 MB per un processo di lavoro attivo e sono presenti 500 siti sui server che verranno sospesi, il file di paging deve essere almeno (200 \* 500) MB per le dimensioni di base del file di paging, quindi in base + 100 GB nell'esempio.

**Nota** Quando i siti vengono sospesi, utilizzeranno circa 6 MB ciascuno, quindi, in questo caso, l'utilizzo della memoria se tutti i siti sono sospesi sono circa 3 GB. In realtà, tuttavia, youâre probabilmente non verranno mai sospesi tutti nello stesso momento.

 
## <a name="transport-layer-security-tuning-parameters"></a>Parametri di ottimizzazione Transport Layer Security

L'uso di Transport Layer Security (TLS) impone un costo aggiuntivo della CPU. Il componente più costoso di TLS è il costo di stabilire uno stabilimento di sessione perché comporta un handshake completo. Anche la riconnessione, la crittografia e la decrittografia si aggiungono al costo. Per migliorare le prestazioni di TLS, eseguire le operazioni seguenti:

-   Abilitare keep-alive HTTP per le sessioni TLS. In questo modo si eliminano i costi di creazione della sessione.

-   Riutilizzare le sessioni quando appropriato, soprattutto con il traffico non Keep-Alive.

-   Applicare selettivamente la crittografia solo a pagine o parti del sito che lo richiedono, anziché all'intero sito.

**Nota**
-   Le chiavi più grandi offrono maggiore sicurezza, ma usano anche più tempo della CPU.

-   È possibile che non sia necessario crittografare tutti i componenti. Tuttavia, la combinazione di HTTP normale e HTTPS può comportare un avviso popup che non tutto il contenuto della pagina è protetto.

 
## <a name="internet-server-application-programming-interface-isapi"></a>ISAPI (Internet Server Application Programming Interface)

Non sono necessari parametri di ottimizzazione speciali per le applicazioni ISAPI. Se si scrive un'estensione ISAPI privata, assicurarsi che sia scritta per le prestazioni e l'uso delle risorse.

## <a name="managed-code-tuning-guidelines"></a>Linee guida per l'ottimizzazione del codice gestito

Il modello di pipeline integrato in IIS 10,0 offre un elevato livello di flessibilità ed estensibilità. I moduli personalizzati implementati in codice nativo o gestito possono essere inseriti nella pipeline oppure possono sostituire i moduli esistenti. Sebbene questo modello di estendibilità offra praticità e semplicità, è necessario prestare attenzione prima di inserire nuovi moduli gestiti che consentono di associare eventi globali. L'aggiunta di un modulo gestito globale significa che tutte le richieste, incluse le richieste di file statiche, devono toccare il codice gestito. I moduli personalizzati sono soggetti ad eventi come Garbage Collection. Inoltre, i moduli personalizzati aggiungono un notevole costo della CPU a causa del marshalling dei dati tra il codice nativo e quello gestito. Se possibile, è necessario impostare la precondizione su managedHandler per il modulo gestito.

Per migliorare le prestazioni di avvio a freddo, assicurarsi di precompilare il sito Web ASP.NET o sfruttare la funzionalità di inizializzazione dell'applicazione IIS per scaldare l'applicazione.

Se lo stato della sessione non è necessario, assicurarsi di disattivarlo per ogni pagina.

Se sono presenti molte operazioni di I/O, provare a usare la versione asincrona delle API pertinenti che offre una maggiore scalabilità.

Inoltre, l'utilizzo corretto della cache di output aumenterà anche le prestazioni del sito Web.

Quando si eseguono più host che contengono script ASP.NET in modalità isolata (un pool di applicazioni per sito), monitorare l'utilizzo della memoria. Verificare che il server disponga di RAM sufficiente per il numero previsto di pool di applicazioni in esecuzione simultanea. Si consiglia di usare più domini applicazione anziché più processi isolati.


## <a name="other-issues-that-affect-iis-performance"></a>Altri problemi che influiscono sulle prestazioni di IIS

I problemi seguenti possono influire sulle prestazioni di IIS:

-   Installazione di filtri che non sono compatibili con la cache

    L'installazione di un filtro che non è compatibile con la cache HTTP comporta la disabilitazione completa della memorizzazione nella cache, con conseguente riduzione delle prestazioni. I filtri ISAPI scritti prima di IISÂ 6,0 possono causare questo comportamento.

-   Richieste di Common Gateway Interface (CGI)

    Per motivi di prestazioni, non è consigliabile usare le applicazioni CGI per gestire le richieste con IIS. Spesso la creazione e l'eliminazione di processi CGI comporta un sovraccarico significativo. Alternative migliori includono l'uso di FastCGI, script dell'applicazione ISAPI e script ASP o ASP.NET. Per ognuna di queste opzioni è disponibile l'isolamento.

## <a name="see-also"></a>Vedi anche
- [Ottimizzazione delle prestazioni del server Web](index.md) 
- [Ottimizzazione di HTTP 1.1/2](http-performance.md)
