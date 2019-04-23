---
title: Tuning IIS 10.0
description: Per i server web IIS 10.0 in Windows Server 16 recommmendations l'ottimizzazione delle prestazioni
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 16ea5c857d99e8a69f528e81178911236341dbb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869852"
---
# <a name="tuning-iis-100"></a>Tuning IIS 10.0

Internet Information Services (IIS) 10.0 è incluso in Windows Server 2016. Usa un modello di processo simile a quella di IIS 7.0 e IIS 8.5. Un driver web in modalità kernel (http. sys) riceve e instrada le richieste HTTP e soddisfa le richieste dalla propria cache della risposta. Registrare i processi di lavoro per spazi secondari URL e HTTP. sys instrada la richiesta per il processo appropriato (o set di processi per i pool di applicazioni).

Http. sys è responsabile della gestione connessione e la gestione della richiesta. La richiesta può essere servita dalla cache HTTP. sys o passata a un processo di lavoro per la gestione aggiuntiva. Più processi di lavoro possono essere configurati, che offre isolamento a costi ridotti. Per altre informazioni su come richiedere funzionamento della gestione, vedere la figura seguente:

![richiesta di gestione in iis 10.0](../../media/perftune-guide-iis-request-handling.png)

Http. sys include una cache di risposta. Quando una richiesta corrisponde a una voce nella cache delle risposte, la risposta della cache HTTP. sys invia direttamente dalla modalità kernel. Alcune piattaforme di applicazioni web, ad esempio ASP.NET, forniscono meccanismi per attivare il contenuto dinamico da memorizzare nella cache nella cache in modalità kernel. Il gestore di file statici in IIS 10.0 memorizza automaticamente il file richiesti di frequente in HTTP. sys.

Poiché un server web ha i componenti in modalità kernel e modalità utente, entrambi i componenti devono essere ottimizzati per prestazioni ottimali. Pertanto, l'ottimizzazione di IIS 10.0 per un carico di lavoro specifico include la configurazione di quanto segue:

-   Http. sys e la cache associata in modalità kernel

-   Processi di lavoro e in modalità utente IIS, che include la configurazione di pool di applicazioni

-   Alcuni parametri di ottimizzazione che influiscono sulle prestazioni

Le sezioni seguenti illustrano come configurare gli aspetti in modalità kernel e modalità utente di IIS 10.0.

## <a name="kernel-mode-settings"></a>Impostazioni della modalità kernel

Impostazioni relative alle prestazioni di HTTP. sys rientrano in due ampie categorie: memorizza nella cache e la gestione connessione e alla richiesta. Tutte le impostazioni del Registro di sistema vengono archiviate con la voce del Registro di sistema seguente:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**Nota**   se è già in esecuzione il servizio HTTP, è necessario riavviarlo rendere effettive le modifiche.

Â 

## <a name="cache-management-settings"></a>Impostazioni di gestione della cache

Uno dei vantaggi che offre di HTTP. sys è una cache in modalità kernel. Se la risposta è nella cache in modalità kernel, è possibile soddisfare una richiesta HTTP interamente dalla modalità kernel, che consente di ridurre notevolmente il costo della CPU di gestisce la richiesta. Tuttavia, la cache in modalità kernel di IIS 10.0 è basata sulla memoria fisica e il costo di una voce è che occupa la memoria.

Una voce nella cache è utile solo quando viene usato. Tuttavia, la voce Usa sempre la memoria fisica, che viene viene usata la voce o meno. È necessario valutare l'utilità di un elemento nella cache (il risparmio garantito da essere in grado di servirla dalla cache) e relativo costo (la memoria fisica occupata) in base alla durata della voce da prendere in considerazione il carico di lavoro e le risorse disponibili (CPU e memoria fisica) requisiti. Http. sys prova a mantenere solo elementi attivamente a cui si accede, utili nella cache, ma è possono aumentare le prestazioni del server web, ottimizzando la cache HTTP. sys per determinati carichi di lavoro.

Di seguito sono alcune impostazioni utili per la cache in modalità kernel HTTP. sys:

-   **UriEnableCache** il valore predefinito: 1

    Un valore diverso da zero consente la risposta in modalità kernel e la memorizzazione nella cache di frammenti. Per la maggior parte dei carichi di lavoro, la cache deve rimanere abilitata. È consigliabile disabilitare la cache, se si prevede che una risposta molto bassa e un frammento di memorizzazione nella cache.

-   **UriMaxCacheMegabyteCount** il valore predefinito: 0

    Un valore diverso da zero che specifica la memoria massima disponibile per la cache in modalità kernel. Il valore predefinito, 0, consente al sistema di regolare automaticamente la quantità di memoria è disponibile nella cache.

    **Nota** specificando la dimensione imposta solo il valore massimo e il sistema potrebbe non consentire la cache di raggiungere la dimensione massima di set.

    Â 

-   **UriMaxUriBytes** il valore predefinito: 262.144 byte (256 KB)

    Le dimensioni massime di una voce nella cache in modalità kernel. Non vengono memorizzate nella cache le risposte o frammenti più grandi rispetto a questo. Se si dispone di sufficiente memoria, prendere in considerazione l'aumento del limite. Se la memoria è limitata e out piccoli riempiono le voci di grandi dimensioni, potrebbe essere utile per ridurre il limite.

-   **UriScavengerPeriod** il valore predefinito: 120 secondi

    La cache HTTP. sys è analizzata periodicamente da un analizzatore e vengono rimosse le voci non sono accessibili tra le analisi analizzatore. Impostazione del periodo di Analizzatore su un valore elevato riduce il numero di scansioni di analizzatore. Tuttavia, l'utilizzo della memoria cache potrebbe migliorare in quanto le voci meno recenti e meno di frequente possono rimanere nella cache. Impostazione del periodo troppo bassa fa in modo che le analisi analizzatore più frequenti e può comportare un numero eccessivo di scaricamenti e memorizzare nella cache della varianza.

## <a name="request-and-connection-management-settings"></a>Impostazioni di gestione connessione e di richiesta

In Windows Server 2016, HTTP. sys gestisce automaticamente le connessioni. Le impostazioni del Registro di sistema seguenti non sono più usate:

-   **maxConnections**

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


## <a name="user-mode-settings"></a>Impostazioni utente-modalità

Le impostazioni in questa sezione interessano il comportamento di processi di lavoro IISÂ 10.0. La maggior parte di queste impostazioni sono disponibili nel file di configurazione XML seguente:

%SystemRoot%\\system32\\inetsrv\\config\\applicationHost.config

Usare Appcmd.exe, la Console di gestione di IIS 10.0, WebAdministration o i cmdlet di PowerShell di amministrazione IIS per modificarli. La maggior parte delle impostazioni vengono rilevate automaticamente e non richiedono un riavvio del server applicazioni web o i processi di lavoro di IIS 10.0. Per altre informazioni sul file ApplicationHost. config, vedere [Introduzione a file ApplicationHost. config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig).


## <a name="ideal-cpu-setting-for-numa-hardware"></a>Impostazione della CPU ideali per l'hardware NUMA

A partire dal 2016 Windows, IIS 10.0 supporta l'assegnazione automatica del ideale della CPU per il pool di thread migliorare le prestazioni e scalabilità in componenti hardware NUMA. Questa funzionalità è abilitata per impostazione predefinita e può essere configurata tramite la chiave del Registro di sistema seguente:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

Con questa funzionalità è abilitata, il gestore di thread IIS rende sforzo per distribuire uniformemente i thread del pool IIS per tutte le CPU in tutti i nodi NUMA in base i carichi correnti. In generale, è consigliabile mantenere questa impostazione non modificato per l'hardware NUMA predefinita.

**Nota**   è diverso dal ruolo di lavoro processo assegnazione impostazioni dei nodi NUMA (numaNodeAssignment e numaNodeAffinityMode) introdotto in l'impostazione della CPU ideale [le impostazioni della CPU per un Pool di applicazioni](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu). Impostazione della CPU ideale influisce sul modo in cui IIS consente di distribuire pool di thread, mentre le impostazioni di assegnazione nodo NUMA processo di lavoro determinano su quale nodo NUMA viene avviato un processo di lavoro.

## <a name="user-mode-cache-behavior-settings"></a>Impostazioni di comportamento della cache in modalità utente

In questa sezione descrive le impostazioni che influiscono sul comportamento di memorizzazione nella cache in IISÂ 10.0. La cache in modalità utente viene implementata come un modulo che ascolta gli eventi di memorizzazione nella cache globali che vengono generati dalla pipeline integrata. Per disabilitare completamente la cache utente-modalità, rimuovere il modulo FileCacheModule (cachfile.dll) dall'elenco dei moduli installati nella sezione di configurazione system.webServer/globalModules in applicationHost. config.

**system.webServer/caching**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|Enabled|Disabilita la cache IIS in modalità utente quando impostata su **False**. Quando la cache raggiunge velocità è molto ridotto, che è possibile disabilitare la cache completamente per evitare il sovraccarico associato con il percorso del codice della cache. Disabilitare la cache in modalità utente non disabilita la cache in modalità kernel.|True|
|enableKernelCache|Disabilita la cache in modalità kernel quando impostata su **False**.|True|
|maxCacheSize|Limita la dimensione della cache in modalità utente IIS per le dimensioni specificate in megabyte. IIS viene modificata l'impostazione predefinita in base alla memoria disponibile. Scegliere i file rispetto alla quantità di RAM o lo spazio degli indirizzi di processo IIS accedere al valore con attenzione in base alla dimensione del set di frequente.|0|
|maxResponseSize|Memorizza nella cache i file fino alle dimensioni specificate. Il valore effettivo dipende dal numero e dimensione dei file più grande del set di dati rispetto alla memoria RAM disponibile. Memorizzazione nella cache di grandi dimensioni, possono ridurre i file richiesti di frequente utilizzo della CPU, l'accesso al disco e latenze associate.|262144|

## <a name="compression-behavior-settings"></a>Impostazioni di comportamento di compressione

Per impostazione predefinita, IIS a partire da 7.0 comprime contenuto statico. Inoltre, la compressione di contenuto dinamico è abilitata per impostazione predefinita quando viene installato il DynamicCompressionModule. La compressione riduce l'utilizzo della larghezza di banda ma aumenta l'utilizzo della CPU. Contenuto compresso è memorizzato nella cache in modalità kernel, se possibile. A partire da 8.5, IIS consente la compressione di essere controllata in modo indipendente per il contenuto statico e dinamico. Contenuti statici in genere fa riferimento al contenuto che non cambia, ad esempio i file GIF o HTM. Contenuto dinamico viene in genere generato dal codice nel server, vale a dire, le pagine ASP.NET o gli script. È possibile personalizzare la classificazione di qualsiasi particolare estensione come statico o dinamico.

Per disabilitare completamente la compressione, rimuovere StaticCompressionModule e DynamicCompressionModule dall'elenco dei moduli nella sezione system.webServer/globalModules in applicationHost. config.

**system.webServer/httpCompression**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|Abilita o disabilita la compressione se la percentuale di utilizzo della CPU corrente va di sopra o sotto di limiti specificati.<br><br>A partire da IIS 7.0, la compressione verrà disabilitata automaticamente se aumenta nello stato stazionario CPU di sopra della soglia di disabilitazione. La compressione è abilitata se della CPU scende sotto la soglia di attivazione.|50, 100, 50 e 90 rispettivamente|
|Directory|Specifica la directory in cui le versioni compresse dei file statici vengono temporaneamente archiviate e memorizzato nella cache. Prendere in considerazione lo spostamento della directory disattivato l'unità del sistema se si accede di frequente.|%SystemDrive%\inetpub\temp\IIS temporary Compressed Files|
|doDiskSpaceLimiting|Specifica se esiste un limite di spazio su disco possono occupare tutti i file compressi. File compressi vengono archiviati nella directory di compressione specificato per il **directory** attributo.|True|
|maxDiskSpaceUsage|Specifica il numero di byte di spazio su disco che i file compressi possono occupare nella directory di compressione.<br><br>Questa impostazione potrebbe essere necessario aumentare se la dimensione totale di tutto il contenuto compresso è troppo grande.|100 MB|

**system.webServer/urlCompression**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|doStaticCompression|Specifica se il contenuto statico è compresso.|True|
|doDynamicCompression|Specifica se il contenuto dinamico è compresso.|True|

**Nota** per i server che esegue IIS 10.0 con basso utilizzo medio della CPU, prendere in considerazione l'abilitazione della compressione di contenuto dinamico, in particolare se le risposte sono di grandi dimensioni. Questa operazione deve innanzitutto essere eseguita in un ambiente di testing per valutare l'effetto dell'uso della CPU dalla linea di base.


### <a name="tuning-the-default-document-list"></a>Elenco di documenti predefiniti di ottimizzazione

Il modulo di documento predefinita gestisce le richieste HTTP per la radice di una directory e li converte in richieste per un file specifico, ad esempio index. htm o default. htm. In Media, aroundÂ 25 percento di tutte le richieste Internet passano attraverso il percorso di documento predefinito. Il valore varia in modo significativo per i singoli siti. Quando una richiesta HTTP non specifica un nome di file, il modulo di documento predefinita cerca nell'elenco dei documenti predefiniti consentiti per ogni nome del file System. Ciò può influire negativamente sulle prestazioni, soprattutto se raggiunge il contenuto è necessario apportare una rete round trip o toccare un disco.

È possibile evitare il sovraccarico disattivando selettivamente documenti predefiniti e riducendo o ordinare l'elenco dei documenti. Per i siti Web che usano un documento predefinito, è necessario ridurre l'elenco per solo i tipi di documento predefiniti che vengono usati. Inoltre, ordinare l'elenco in modo che inizia con più di frequente nome file del documento predefinito a cui si accede.

È possibile impostare in modo selettivo il comportamento di documento predefinito sugli URL specifico tramite la personalizzazione della configurazione all'interno di un tag di posizione nel file ApplicationHost. config o inserendo un file Web. config direttamente nella directory del contenuto. Ciò consente un approccio ibrido, che abilita documenti predefiniti in cui sono gli unici necessari e imposta l'elenco per il file corretto assegnare un nome per ogni URL.

Per disabilitare completamente il documento predefinito, rimuovere DefaultDocumentModule dall'elenco dei moduli nella sezione system.webServer/globalModules in applicationHost. config.

**system.webServer/defaultDocument**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|enabled|Specifica che i documenti predefiniti sono abilitati.|True|
|&lt;file&gt; elemento|Specifica i nomi di file che sono configurati come documenti predefiniti.|L'elenco predefinito è default. htm, default. ASP, index. htm, index. HTML, IISSTART. htm e default. aspx.|

## <a name="central-binary-logging"></a>Registrazione binaria centrale

Quando la sessione del server include numerosi gruppi di URL sotto di esso, il processo di creazione di centinaia di file di log formattato per singoli gruppi di URL e la scrittura dei dati di log in un disco può consumare rapidamente utili risorse di CPU e memoria, creando così le prestazioni e problemi di scalabilità. Registrazione centralizzata binaria riduce al minimo la quantità di risorse di sistema utilizzate per la registrazione, mentre allo stesso tempo che fornisce dati di log dettagliati per le organizzazioni che lo richiedono. Analisi dei log in formato binario richiede l'utilizzo di uno strumento di post-elaborazione.

È possibile abilitare la registrazione binaria centrale impostando l'attributo centralLogFileMode a CentralBinary e impostare il **abilitate** dell'attributo **True**. Provare a spostare la posizione del file di log centrale dalla partizione di sistema e in un'unità di registrazione dedicata per evitare conflitti tra le attività di sistema e le attività di registrazione.

**system.applicationHost/log**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|centralLogFileMode|Specifica la modalità di registrazione per un server. Modificare questo valore CentralBinary per abilitare la registrazione binaria centrale.|Sito|

**system.applicationHost/log/centralBinaryLogFile**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|enabled|Specifica se è abilitata la registrazione binaria centrale.|False|
|Directory|Specifica la directory in cui vengono scritte voci di log.|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>Se applicazione e il sito

Le impostazioni seguenti si riferiscono ai se pool e il sito dell'applicazione.

**system.applicationHost/applicationPools/applicationPoolDefaults**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|queueLength|Indica a http. sys quante richieste vengono accodate per un pool di applicazioni prima che le richieste successive vengono rifiutate. Quando viene superato il valore di questa proprietà, IIS rifiuta le richieste successive con un errore 503.<br><br>È possibile aumentare questo per le applicazioni che comunicano con gli archivi dati di back-end con latenza elevata, se si osservano 503 errori.|1000|
|enable32BitAppOnWin64|Se è True, consente a un'applicazione a 32 bit per l'esecuzione in un computer che ha un processore a 64 bit.<br><br>Provare ad abilitare la modalità a 32 bit se il consumo di memoria rappresenta un problema. Poiché sono più piccole dimensioni (istruzione) e dimensioni del puntatore, applicazioni a 32 bit usano meno memoria rispetto alle applicazioni a 64 bit. Lo svantaggio di esecuzione di applicazioni a 32 bit in un computer a 64 bit è che lo spazio di indirizzi in modalità utente è limitato a 4 GB.|False|

**system.applicationHost/sites/VirtualDirectoryDefault**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|allowSubDirConfig|Specifica se IIS è simile per i file Web. config nella directory contenuto inferiore rispetto all'oggetto corrente a livello (True) o non cerca i file Web. config nella directory contenuto inferiore a quello corrente (False). Per imporre una limitazione di semplice, che consente di configurare solo nella directory virtuali, 10.0 IISÂ può sapere che, a meno che  **/ &lt;nome&gt;htm** è una directory virtuale, non dovrebbe essere un file di configurazione. Ignorare le operazioni aggiuntive del file può migliorare significativamente le prestazioni dei siti Web che dispone di un set di dimensioni molto grande di contenuto statico a cui si accede in modo casuale.|True|

## <a name="managing-iis-100-modules"></a>Gestione dei moduli di IIS 10.0

IIS 10.0 è stata suddivisa in più, i moduli utente estendibile per supportare una struttura modulare. Questa fattorizzazione è a basso costo. Per ogni modulo pipeline integrata deve chiamare il modulo per tutti gli eventi rilevanti per il modulo. Ciò accade indipendentemente dal fatto che il modulo deve eseguire una lavoro. È possibile risparmiare sui cicli della CPU e memoria tramite la rimozione di tutti i moduli che non sono rilevanti per un particolare sito Web.

Un server web che è ottimizzato per semplici file statici può includere solo i moduli di cinque seguenti: UriCacheModule HttpCacheModule, StaticFileModule, AnonymousAuthenticationModule e HttpLoggingModule.

Per rimuovere i moduli da applicationHost. config, rimuovere tutti i riferimenti al modulo dalle sezioni system.webServer/handlers e WebServer/Modules oltre a rimuovere la dichiarazione di modulo in system.webServer/globalModules.

## <a name="classic-asp-settings"></a>Impostazioni ASP classiche

Il costo principale di elaborazione di una richiesta ASP classica include l'inizializzazione di un motore di script, la compilazione di script ASP richiesto in un modello ASP e l'esecuzione del modello sul motore di script. Mentre il costo di esecuzione del modello varia a seconda della complessità dello script ASP richiesto, modulo ASP classico IIS può memorizzare nella cache i motori di script nei modelli in memoria e della cache in memoria e disco (solo se causa l'overflow cache dei modelli in memoria) per migliorare le prestazioni in Scenari basate sulla CPU.

Le impostazioni seguenti vengono usate per configurare la cache dei modelli ASP classica e cache del motore di script e non influiscono le impostazioni di ASP.NET.

**system.webServer/asp/cache**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|diskTemplateCacheDirectory|Il nome della directory utilizzata da ASP per archiviare i modelli compilati quando la cache in memoria causa un overflow.<br><br>Consiglio: Impostato su una directory che non è ampiamente usato, ad esempio, un'unità che non viene condivisa con il sistema operativo, log IIS, o altro contenuto si accede frequentemente.|%SystemDrive%\inetpub\temp\ASP compilati i modelli|
|maxDiskTemplateCacheFiles|Specifica il numero massimo di modelli ASP compilati che possono essere memorizzati nella cache su disco.<br><br>Consiglio: Impostare il valore massimo di 0x7FFFFFFF.|2000|
|scriptFileCacheSize|Questo attributo specifica il numero massimo di modelli ASP compilati che possono essere memorizzati nella cache in memoria.<br><br>Consiglio: Impostata su almeno al numero come numero di script ASP più richieste servite da un pool di applicazioni. Se possibile, impostare il numero di modelli ASP consentiti dai limiti di memoria.|500|
|scriptEngineCacheMax|Specifica il numero massimo di motori di script che verranno mantenuti nella memoria cache.<br><br>Consiglio: Impostata su almeno al numero come numero di script ASP più richieste servite da un pool di applicazioni. Se possibile, impostare su tutti i motori di script in base al limite di memoria disponibile.|250|

**system.webServer/asp/limits**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|processorThreadMax|Specifica il numero massimo di thread di lavoro per processore che possono creare ASP. Aumenta se l'impostazione corrente è sufficiente per gestire il carico, che può causare correggerà sull'utilizzo delle risorse di CPU o causare errori quando gestisce le richieste.|25|

**system.webServer/asp/comPlus**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|executeInMta|Impostare su **True** se vengono rilevati errori o gli errori mentre IIS è contenuto ASP. Ciò può verificarsi, ad esempio, quando si ospitano più siti isolati in cui ogni sito viene eseguito con un proprio processo di lavoro. In genere, gli errori vengono segnalati da COM+ nel Visualizzatore eventi. Questa impostazione consente al modello di apartment a thread multipli in ASP.|False|


## <a name="aspnet-concurrency-setting"></a>Impostazione di concorrenza di ASP.NET

### <a name="aspnet-35"></a>ASP.NET 3.5
Per impostazione predefinita, ASP.NET limita la concorrenza di richiesta per ridurre il consumo di memoria nello stato stazionario sul server. Le applicazioni di concorrenza elevata potrebbe essere necessario modificare alcune impostazioni per migliorare le prestazioni complessive. È possibile modificare questa impostazione nel file ASPNET config:

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

La seguente impostazione è utile per usare tutte le risorse in un sistema:

-   **maxConcurrentRequestPerCpu** il valore predefinito: 5000

    Questa impostazione limita il numero massimo di richieste ASP.NET in esecuzione contemporanea su un sistema. Il valore predefinito è conservativo per ridurre il consumo di memoria delle applicazioni ASP.NET. Provare ad aumentare questo limite sui sistemi che eseguono le applicazioni che eseguono operazioni dei / o sincrone, lungo. In caso contrario, gli utenti possono riscontrare una latenza elevata a causa di errori di accodamento o la richiesta a causa del superamento dei limiti della coda in un carico elevato quando si usa l'impostazione predefinita.

### <a name="aspnet-46"></a>ASP.NET 4.6
Oltre all'impostazione maxConcurrentRequestPerCpu, ASP.NET 4.7 include inoltre impostazioni per migliorare le prestazioni nelle applicazioni di cui fa particolare affidamento operazione asincrona. L'impostazione può essere modificata nel file ASPNET config.

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit** il valore predefinito: 90 richiesta asincrona ha alcuni problemi di scalabilità quando viene inserito un carico di grandi dimensioni (oltre le funzionalità hardware) in tale scenario. Il problema è dovuto alla natura dell'allocazione in scenari asincroni. In queste condizioni, allocazione verrà eseguito quando viene avviato l'operazione asincrona, e che verranno usato quando viene completato. Da quel momento, s sensazione molto possibili gli oggetti sono stati spostati dal GC generazione 1 o 2. In questo caso, aumenta il carico mostrerà aumento su richiesta al secondo (rps) fino a un punto. Una volta che viene passato a quel punto, il tempo impiegato in Garbage Collection inizierà a diventare un problema e le richieste al secondo inizieranno a dip, avere un effetto negativo di ridimensionamento. Per risolvere il problema, quando l'utilizzo della cpu supera percentCpuLimit impostazione, le richieste verranno inviate alla coda nativa di ASP.NET.
-   **percentCpuLimitMinActiveRequestPerCpu** Default value: Limitazione CPU 100 (percentCpuLimit impostazione) non si basa sul numero di richieste, ma sul costo richiesto sono. Di conseguenza, potrebbero esserci alcune richieste intensivo della CPU può causare un backup, la coda nativa con alcun modo su un valore vuoto, a parte le richieste in ingresso. Per risolvere questo problme, percentCpuLimitMinActiveRequestPerCpu è utilizzabile per garantire che un numero minimo di richieste viene eseguito prima di limitazione delle richieste viene attivata.

## <a name="worker-process-and-recycling-options"></a>Processo di lavoro e il riciclo di opzioni

È possibile configurare le opzioni per il riciclo dei processi di lavoro IIS e fornire soluzioni pratiche a situazioni acute o eventi senza richiedere intervento o la reimpostazione di un servizio o un computer. Tali situazioni e gli eventi includono le perdite di memoria, aumentare il carico di memoria o i processi di lavoro inattivo o non risponde. In condizioni normali, il riciclo di opzioni potrebbe non essere necessario e riciclo può essere disattivato o il sistema può essere configurato per riciclare molto raramente.

È possibile abilitare il riciclo dei processi per una particolare applicazione mediante l'aggiunta di attributi per il **riciclo/periodicRestart** elemento. L'evento di riciclo può essere attivato da eventi diversi tra cui utilizzo della memoria, un numero fisso di richieste e un periodo di tempo fisso. Quando un processo di lavoro viene riciclato, le richieste in coda e in esecuzione vengono svuotate e contemporaneamente viene avviato un nuovo processo per soddisfare nuove richieste. Il **riciclo/periodicRestart** elemento è per ogni applicazione, vale a dire che ogni attributo nella tabella seguente viene partizionata in base all'applicazione.

**system.applicationHost/applicationPools/ApplicationPoolDefaults/recycling/periodicRestart**

|Attributo|Descrizione|Impostazione predefinita|
|--- |--- |--- |
|memoria|Attivare se il consumo di memoria virtuale supera il limite specificato nel kilobyte il riciclo dei processi. Si tratta di un'impostazione utile per i computer a 32 bit che presentano una piccola, spazio degli indirizzi di 2 GB. Consente di evitare le richieste non riuscite a causa di errori di memoria insufficiente.|0|
|privateMemory|Se le allocazioni di memoria privata superano il limite specificato in kilobyte, abilitare il riciclo dei processi.|0|
|requests|Abilitare il riciclo dei processi dopo un certo numero di richieste.|0|
|ora|Abilitare il riciclo dei processi dopo un periodo di tempo specificato.|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>Ottimizzazione dinamica dei processi di paging

A partire da Windows Server 2012 R2, IIS offre l'opzione di configurazione di processo di lavoro per sospendere una volta divenuti inattivi per un periodo di tempo (oltre all'opzione di terminazione, che è stato introdotto con IIS 7).

Lo scopo principale del processo di lavoro inattivi pagina istanze e funzionalità di terminazione del processo di lavoro inattivo è per conservare l'utilizzo della memoria nel server, poiché un sito può utilizzare una grande quantità di memoria, anche se appena si trova, in attesa. A seconda della tecnologia usata nel sito (Visual Studio ASP.NET di Visual Studio contenuto statico altri framework), la memoria utilizzata può essere in qualsiasi punto da circa 10 MB a centinaia di MB e questo significa che se il server è configurato con molti siti, capire le impostazioni più efficace per i siti possono migliorare notevolmente le prestazioni dei siti attivi e sospesi.

Prima di analizzare le specifiche, è necessario tenere presente che se non sono presenti vincoli di memoria, probabilmente è meglio impostare semplicemente i siti di sospensione o terminazione mai. Dopo tutto, s thereâ scarso valore in un ruolo di lavoro di terminazione processo se è l'unico esistente per la macchina.

**Nota**   nel caso in cui il sito esegue codice instabile, ad esempio di codice con una perdita di memoria, o instabile in caso contrario, l'impostazione del sito per terminare in inattiva può essere un'alternativa rapide e la correzione del bug del codice. Si tratta di un elemento che verrebbe e invita, ma in un masticare, potrebbe essere preferibile usare questa funzionalità come meccanismo di pulizia mentre una soluzione definitiva è previsto il.\]

Â 

Un altro fattore da considerare è che se il sito utilizza una grande quantità di memoria, quindi lo stesso processo di sospensione accetta un numero a pagamento, perché il computer deve scrivere i dati usati dal processo di lavoro su disco. Se il processo di lavoro utilizza una grande quantità di memoria, quindi la sospensione, potrebbe essere più costosa rispetto al costo di dover attendere che venga avviato il backup.

Per rendere il meglio della funzionalità di sospensione di processo di lavoro, è necessario rivedere i siti in ciascun pool di applicazioni e decidere quale deve essere sospeso, che deve essere interrotto e che deve essere attiva per un periodo illimitato. Per ogni sito e ogni azione, è necessario determinare il periodo di timeout ideale.

In teoria, i siti che si desidera configurare per la sospensione o terminazione sono quelli che dispongono di visitatori ogni giorno, ma non sono sufficienti a giustificare mantenendola active continuamente. Si tratta in genere siti con circa 20 visitatori univoci di un giorno o meno. È possibile analizzare i modelli di traffico usando i file di log del sito e calcolare il Media traffico giornaliero.

Tenere presente che dopo un determinato utente si connette al sito, verrà in genere rimangono su di esso per almeno un po' di tempo, effettua richieste aggiuntive e quindi è sufficiente il conteggio giornaliero delle richieste potrebbe non riflettere i modelli di traffico reale. Per ottenere la lettura di un più accurata, è anche possibile usare uno strumento, ad esempio Microsoft Excel, per la quale calcolare il tempo medio tra le richieste. Ad esempio: 

||URL della richiesta|Medio richiesta|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/ SourceSilverLight/GeosourcewebService/Service.asmx|10:23|0:11|
|6|/ SourceSilverLight/Geosource.web/GeoSearchServer...¦.|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la...¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap...¦.|12:51|0:01|
|9|/ rest/Services/DynamicService/Silverlight_basemap... ¦.|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache.as...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache.js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

La parte difficile, tuttavia, è scoprire quale impostazione da applicare per comprendere. In questo caso, il sito recupera una serie di richieste dagli utenti e la precedente tabella mostra che si è verificato un totale di sessioni univoche 4 in un periodo di 4 ore. Con le impostazioni predefinite per la sospensione di processo di lavoro del pool di applicazioni, il sito verrà terminato dopo il timeout predefinito di 20 minuti, il che significa ciascuno di questi utenti potrebbe verificarsi il ciclo di spin-up del sito. Questo rende un candidato ideale per la sospensione di processo di lavoro, perché per la maggior parte dei casi, il sito è inattivo, quindi la sospensione, verrebbe conservare le risorse e consentire agli utenti di raggiungere il sito quasi istantaneamente.

Una nota finale e molto importante proposito è che le prestazioni del disco sono fondamentale per questa funzionalità. Poiché il processo di riattivazione e sospensione implicano la scrittura e lettura di grandi quantità di dati sul disco rigido, è consigliabile usare un disco veloci per questo oggetto. Le unità a stato solido (SSD) sono ideale e altamente consigliata per questo e assicurarsi che il file di paging di Windows è archiviato al suo interno (se il sistema operativo stesso non è installato sull'unità SSD, configurare il sistema operativo per spostare il file di paging).

Se si usa un'unità SSD o No, è inoltre consigliabile risolvere le dimensioni del file della pagina per supportare la scrittura dei dati di pagina in uscita ad esso senza alcun ridimensionamento file. Ridimensionamento di file di paging potrebbe verificarsi quando il sistema operativo deve archiviare dati nel file della pagina, poiché per impostazione predefinita, Windows è configurato per la regolazione automatica delle dimensioni in base necessario. Impostando le dimensioni di uno predefinito, è possibile evitare il ridimensionamento e migliorare notevolmente le prestazioni.

Per configurare un file di paging pre-fisso, è necessario calcolare le dimensioni ideali, che dipende da quanti siti è possibile sospendere e quantità di memoria utilizzate. Se la media è di 200 MB per un processo di lavoro attivo e si dispone di 500 siti nei server che sarà possibile sospendere, quindi il file di paging deve essere almeno (200 \* 500) MB rispetto alla dimensione base del file di paging (quindi, di base + 100 GB in questo esempio).

**Nota** quando i siti vengono sospese, vengono utilizzati circa 6 MB ciascuno, pertanto in questo caso, utilizzo della memoria se vengono sospesi tutti i siti sarebbe circa 3 GB. In realtà, però, troverai re mai passare in modo che vengano sospesi nello stesso momento.

 
## <a name="transport-layer-security-tuning-parameters"></a>Transport Layer Security regolazione dei parametri

L'uso di Transport Layer Security (TLS) impone aggiuntivi della CPU. Il componente più costoso di TLS è il costo della creazione di una definizione di sessione dal momento che implica un handshake completo. La riconnessione, la crittografia e decrittografia anche aggiungere al costo. Per migliorare le prestazioni TLS, eseguire le operazioni seguenti:

-   Abilitare i keep-alive HTTP per le sessioni TLS. Ciò consente di eliminare i costi di stabilire una sessione.

-   Riutilizzare le sessioni quando appropriato, in particolare con il traffico non-keep-alive.

-   In modo selettivo Applica crittografia solo alle pagine o parti del sito che serve, anziché all'intero sito.

**Nota**
-   Chiavi di dimensioni maggiori garantiscono una maggiore sicurezza, ma usano anche più tempo della CPU.

-   Tutti i componenti potrebbe essere necessario non crittografati. Combinazione plain HTTP e HTTPS, tuttavia, potrebbe causare una finestra popup di avviso che non tutto il contenuto della pagina sia sicuro.

 
## <a name="internet-server-application-programming-interface-isapi"></a>Internet Server Application Programming Interface (ISAPI)

Nessun parametro di ottimizzazione speciale è necessari per le applicazioni ISAPI. Se si scrive un'estensione ISAPI privata, assicurarsi che viene scritto per le prestazioni e utilizzo delle risorse.

## <a name="managed-code-tuning-guidelines"></a>Linee guida sull'ottimizzazione di codice gestito

Il modello di pipeline integrata in IIS 10.0 consente un elevato livello di flessibilità ed estensibilità. Moduli personalizzati implementati in codice nativo o gestito possono essere inseriti nella pipeline, o possono sostituire i moduli esistenti. Sebbene questo modello di estendibilità offre praticità e semplicità, è necessario prestare attenzione prima di inserire nuovi moduli gestiti che si collegano in eventi globali. Aggiunta di che un modulo gestito di globale significa che tutte le richieste, incluse le richieste di file statici, devono accedere a codice gestito. Moduli personalizzati sono soggetti a eventi, ad esempio operazioni di garbage collection. Inoltre, i moduli personalizzati aggiungere significativa della CPU a causa di marshalling dei dati tra codice gestito e nativo. Se possibile, è necessario impostare precondizione su managedHandler per il modulo gestito.

Per ottenere migliorate prestazioni di avvio a freddo, assicurarsi di che aver precompliato la funzionalità di inizializzazione dell'applicazione di IIS sito web oppure usare ASP.NET per il riscaldamento all'applicazione.

Se lo stato della sessione non è necessaria, assicurarsi che si disattivarla per ogni pagina.

Se sono presenti operazioni associate ai / o molti, provare a usare la versione asincrona di API rilevanti che offrono una maggiore scalabilità.

Anche le Cache di Output in modo corretto verrà anche migliorare le prestazioni del sito web.

Quando si eseguono più host che contengono script ASP.NET in modalità isolata (un pool di applicazioni per ogni sito), monitorare l'utilizzo della memoria. Assicurarsi che il server disponga di sufficiente RAM per il numero previsto di in esecuzione simultanea di pool di applicazioni. È consigliabile usare più domini dell'applicazione invece di più processi isolati.


## <a name="other-issues-that-affect-iis-performance"></a>Altri problemi che influiscono sui IIS prestazioni

I problemi seguenti possono influire sulle prestazioni di IIS:

-   Installazione dei filtri che non sono compatibili con cache

    L'installazione di un filtro che non supporta le HTTP-cache fa in modo che IIS disabilitare completamente la memorizzazione nella cache, determinando una riduzione delle prestazioni. Filtri ISAPI che sono stati scritti prima IISÂ 6.0 possono causare il problema.

-   Common Gateway Interface (CGI) richieste

    Per motivi di prestazioni, l'uso di applicazioni CGI per rispondere alle richieste non è consigliata con IIS. Creazione ed eliminazione dei processi CGI frequente comporta un sovraccarico significativo. Alternative migliori includono l'uso di FastCGI, script di applicazione ISAPI e ASP o ASP.NET. L'isolamento è disponibile per ognuna di queste opzioni.

# <a name="see-also"></a>Vedere anche
- [Web ottimizzazione delle prestazioni Server](index.md) 
- [HTTP 1.1/2 ottimizzazione](http-performance.md)
