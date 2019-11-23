---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Funzionamento del servizio Ora di Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 2bf4a887218cd51e9c10954a75bbc1ba2112647f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405150"
---
# <a name="how-the-windows-time-service-works"></a>Funzionamento del servizio Ora di Windows

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versione successiva

**Contenuto della sezione**  
  
-   [Architettura del servizio ora di Windows](#windows-time-service-architecture)  
  
-   [Protocolli Tempo di servizio ora di Windows](#windows-time-service-time-protocols)  
  
-   [Processi e interazioni del servizio ora di Windows](#windows-time-service-processes-and-interactions)  
  
-   [Porte di rete usate dal servizio ora di Windows](#network-ports-used-by-windows-time-service)  
  
> [!NOTE]  
> In questo argomento viene illustrato solo come funziona il servizio ora di Windows (W32Time). Per informazioni su come configurare il servizio ora di Windows, vedere [configurazione dei sistemi per un'accuratezza elevata](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> In Windows Server 2003 e Microsoft Windows 2000 Server, il servizio directory è denominato servizio directory Active Directory. In Windows Server 2008 e versioni successive, il servizio directory è denominato servizi di dominio Active Directory (AD DS). Nel resto di questo argomento si fa riferimento ad AD DS, ma le informazioni sono applicabili anche ad Active Directory.  
  
Sebbene il servizio ora di Windows non è un'implementazione esatta del protocollo NTP (Network Time), Usa la suite di algoritmi complessa definiti nelle specifiche NTP per garantire che gli orologi dei computer in rete siano più accurati possibile. In teoria, tutti gli orologi dei computer in un dominio di Active Directory vengono sincronizzati con l'ora di un computer autorevole. Molti fattori possono influire sulla sincronizzazione dell'ora in una rete. I seguenti fattori spesso influiscono sull'accuratezza della sincronizzazione in Active Directory:  
  
-   Condizioni della rete  
  
-   L'accuratezza dell'orologio hardware del computer  
  
-   La quantità di risorse della CPU e della rete disponibili per il servizio ora di Windows  
  
> [!IMPORTANT]  
> Prima di Windows Server 2016, il servizio W32Time non è stato progettato per soddisfare le esigenze dell'applicazione in termini di tempo.  Tuttavia, gli aggiornamenti a Windows Server 2016 consentono ora di implementare una soluzione per l'accuratezza del 1 MS nel dominio.  Per ulteriori informazioni, vedere l' [ora esatta](accurate-time.md) e [il limite del supporto di Windows 2016 per configurare il servizio ora di Windows per ambienti ad alta precisione](support-boundary.md) .  
  
Per impostazione predefinita, i computer che sincronizzano la loro durata o che non fanno parte di un dominio vengono configurati per la sincronizzazione con time.windows.com.  Pertanto, non è possibile garantire l'accuratezza del tempo nei computer con connessioni di rete intermittenti o non disponibili.  
  
Un insieme di strutture Active Directory presenta una gerarchia di sincronizzazione di tempo predeterminato. Il servizio ora di Windows sincronizza il tempo tra i computer all'interno della gerarchia, con gli orologi di riferimento più accurati nella parte superiore. Se più di un'origine ora è configurata in un computer, ora di Windows utilizza algoritmi NTP per selezionare l'origine dell'ora migliore dalle origini configurate in base alle capacità del computer per la sincronizzazione con tale origine dell'ora. Il servizio ora di Windows non supporta la sincronizzazione di rete da broadcast o multicast peer. Per ulteriori informazioni su queste funzionalità NTP, vedere RFC 1305 nel Database IETF RFC.  
  
Ogni computer che esegue il servizio ora di Windows utilizza il servizio per gestire l'ora più accurata. Per impostazione predefinita, i computer membri di un dominio agiscono come client di ora, pertanto nella maggior parte dei casi non è necessario configurare il servizio ora di Windows. Tuttavia, il servizio ora di Windows può essere configurato per richiedere tempo da un'origine ora di riferimento designata e può anche fornire tempo ai client.
  
Il livello a cui l'ora del computer sia precisa viene chiamato un strato. L'origine dell'ora più accurata in una rete (ad esempio un orologio hardware) occupa il livello più basso strato o uno strato. L'origine esatta del tempo viene chiamata un orologio di riferimento. Un server NTP che acquisisce il tempo di direttamente da un orologio riferimento occupa un strato di un livello superiore a quello dell'orologio del riferimento. Risorse di acquisire l'ora del server NTP sono due passaggi per l'orologio di riferimento e pertanto occupano un strato di due superiore l'origine dell'ora più accurati possibile e così via. Come numero strato di un computer aumenta, l'ora del clock di sistema potrebbe diventare meno accurato. Pertanto, il livello strato di qualsiasi computer è un indicatore in quale misura tale computer viene sincronizzato con l'origine dell'ora più accurato.  
  
Quando il gestore W32Time riceve esempi, utilizza algoritmi speciali in NTP per determinare quale degli esempi di tempo risulta più appropriato per l'utilizzo. Il servizio ora utilizza anche un altro set di algoritmi per determinare quale delle origini ora configurata è il più accurato. Quando il servizio ora ha determinato quali time è migliore, in base ai criteri precedenti, modifica la frequenza di clock locale per consentire la convergenza verso l'ora corretta. Se la differenza tra l'orologio locale e l'esempio di ora esatta selezionato (detto anche il tempo di inclinazione) è troppo grande per correggere regolando la frequenza di clock locale, il servizio ora imposta l'orologio locale sull'ora corretta. Questa regolazione della frequenza di clock o modifica dell'ora dell'orologio diretto è noto come disciplina di orologio.  
  
## <a name="windows-time-service-architecture"></a>Architettura del servizio Ora di Windows  
Il servizio ora di Windows include i componenti seguenti:  
  
-   Gestione controllo servizi  
  
-   Gestore del servizio ora di Windows  
  
-   Disciplina di orologio  
  
-   Provider servizi orari  
  
Nella figura seguente è illustrata l'architettura del servizio ora di Windows.  
  
**Architettura del servizio ora di Windows**  
  
![Ora di Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
Gestione controllo servizi è responsabile dell'avvio e arresto del servizio ora di Windows. Gestione del servizio ora di Windows è responsabile dell'avvio l'azione del provider servizi orari NTP incluso con il sistema operativo. Gestione del servizio ora di Windows controlla tutte le funzioni del servizio ora di Windows e l'unione di tutti gli esempi di tempo. Oltre a fornire informazioni sullo stato corrente del sistema, ad esempio l'origine dell'ora corrente o l'ultima volta che l'orologio di sistema è stato aggiornato, gestione del servizio ora di Windows è anche responsabile della creazione di eventi nel registro eventi.  
  
Il processo di sincronizzazione ora prevede i passaggi seguenti:  
  
-   Provider di input richiesta e ricevere esempi ora da origini dell'ora NTP configurate.  
  
-   Questi esempi vengono quindi passati al tempo Gestione servizi di Windows, che raccoglie tutti gli esempi e li passa il sottocomponente disciplina di orologio.  
  
-   Il sottocomponente disciplina orologio applica gli algoritmi NTP che comporta la selezione dell'esempio tempo migliore.  
  
-   Il sottocomponente disciplina orologio consente di regolare l'ora dell'orologio di sistema per l'ora più accurata regolare la frequenza di clock o modificando direttamente l'ora.  
  
Se un computer è stato designato come un server, può inviare il tempo a qualsiasi computer che richiede la sincronizzazione dell'ora in qualsiasi punto del processo.  
  
## <a name="windows-time-service-time-protocols"></a>Protocolli ora servizio ora di Windows  

Protocolli ora determinano in quale misura due computer orologi vengono sincronizzati. Un protocollo di tempo è responsabile di determinare le migliori informazioni disponibili per l'ora e convergenti gli orologi per garantire il mantenimento di un'ora coerenza nei sistemi separati.  
  
Il servizio ora di Windows utilizza il protocollo NTP (Network Time) per sincronizzare l'ora in una rete. NTP è un protocollo di tempo Internet che include gli algoritmi di disciplina necessari per la sincronizzazione degli orologi. NTP è un protocollo di tempo più accurato rispetto il Simple Network Time Protocol (SNTP) che viene utilizzato in alcune versioni di Windows. tuttavia W32Time continua a supportare SNTP per abilitare la compatibilità con computer che eseguono servizi basati su SNTP tempo, ad esempio Windows 2000.  
  
### <a name="network-time-protocol"></a>Protocollo di rete del tempo  
Protocollo NTP (Network Time) il valore predefinito è ora il protocollo di sincronizzazione utilizzato dal servizio ora di Windows nel sistema operativo. NTP è un protocollo di tempo a tolleranza di errore e altamente scalabile ed è il protocollo utilizzato più spesso per sincronizzare gli orologi dei computer tramite un riferimento ora designato.  
  
Sincronizzazione dell'ora NTP viene eseguita in un periodo di tempo e comporta il trasferimento di NTP pacchetti in rete. Pacchetti NTP contengono timestamp, che includono un campione ora sia il client e il server che fanno parte della sincronizzazione temporale.  
  
NTP si basa su un orologio di riferimento per definire l'ora più accurata da utilizzare e Sincronizza tutti gli orologi di una rete di orologio tale riferimento. NTP utilizza Coordinated Universal Time (UTC) come standard universale per ora corrente. UTC è indipendente dei fusi orari e Abilita NTP essere utilizzati ovunque nel mondo indipendentemente dalle impostazioni di fuso orario.  
  
#### <a name="ntp-algorithms"></a>Algoritmi NTP  
NTP include due algoritmi, un algoritmo di filtro orologio e un algoritmo di selezione di orologio, per agevolare il servizio ora di Windows per determinare la migliore esempio time. L'algoritmo di filtraggio orologio è progettato per esplorare gli esempi di tempo che vengono ricevuti da origini dell'ora richiesto e determinano il migliore esempi da ogni origine. L'algoritmo di selezione dell'orologio determina quindi server di più preciso sulla rete. Queste informazioni vengono quindi passate all'algoritmo di disciplina di orologio, che utilizza le informazioni raccolte per correggere l'orologio locale del computer, durante la compensazione degli errori a causa dell'imprecisione orologio computer e la latenza di rete.  
  
Gli algoritmi NTP sono più accurati in condizioni di carichi di rete e server light-medio. Come con qualsiasi algoritmo che tiene conto di tempo di transito in rete, gli algoritmi NTP potrebbero scarse in condizioni di congestione della rete estremi. Per ulteriori informazioni sugli algoritmi NTP, vedere RFC 1305 nel Database IETF RFC.  
  
#### <a name="ntp-time-provider"></a>Provider servizi orari NTP  
Il servizio ora di Windows è un pacchetto di sincronizzazione completa in grado di supportare un'ampia gamma di dispositivi hardware e i protocolli di tempo. Per abilitare questo supporto, il servizio utilizza provider servizi orari plug-in. Un provider servizi orari è responsabile per l'acquisizione accurate timestamp (dalla rete o da hardware) o per fornire i timestamp in altri computer nella rete.  
  
Il provider NTP è il provider servizi orari standard incluso con il sistema operativo. Il provider NTP conforme agli standard di specificato dalla versione 3 per un client e server NTP e può interagire con SNTP client e server per garantire la compatibilità con Windows 2000 e altri client SNTP. Il provider NTP nel servizio ora di Windows è costituito da due parti:  
  
-   **Provider di output NtpServer.** Si tratta di un server che risponde alle richieste di tempo del client nella rete.  
  
-   **Provider di input NtpClient.** Questo è un client di tempo che ottiene informazioni sull'ora da un'altra origine, un dispositivo hardware o un server NTP e può restituire esempi utili per sincronizzare l'orologio locale.  
  
Sebbene le operazioni effettive di questi due provider sono strettamente correlate, appaiono indipendente per il servizio ora. A partire da Windows 2000 Server, quando un computer Windows è connesso a una rete, viene configurato come client NTP. Inoltre, i computer che eseguono l'ora di Windows service solo tentativo di sincronizzare l'ora con un controller di dominio o un'origine ora specificata manualmente per impostazione predefinita. Questi sono provider servizi orari preferito perché sono automaticamente disponibili e sicure origini del tempo.  
  
#### <a name="ntp-security"></a>Sicurezza NTP  

All'interno di una foresta di Active Directory, il servizio ora di Windows si basa sulle funzionalità di protezione dominio standard per applicare l'autenticazione di dati temporali. La sicurezza dei pacchetti trasmessi tra un computer membro del dominio e un controller di dominio locale che funge da un server NTP si basa sull'autenticazione chiave condivisa. Il servizio ora di Windows utilizza chiave di sessione Kerberos del computer per creare firme autenticate pacchetti NTP che vengono inviati attraverso la rete. NTP pacchetti non vengono trasmessi all'interno di un canale protetto accesso rete. Al contrario, quando un computer richiede il tempo da un controller di dominio nella gerarchia di domini, il servizio ora di Windows richiede che vengano autenticati l'ora. Il controller di dominio restituisce quindi le informazioni necessarie sotto forma di un valore a 64 bit che è stato autenticato con la chiave di sessione dal servizio Accesso rete. Se il pacchetto NTP restituito non è firmato con la chiave di sessione del computer o è firmato in modo non corretto, il tempo viene rifiutato. Tutti questi errori di autenticazione vengono registrati nel registro eventi. In questo modo, il servizio ora di Windows fornisce protezione per i dati NTP in una foresta di Active Directory.  
  
In genere, i client ora Windows ottengono automaticamente ora esatta per la sincronizzazione dal controller di dominio nello stesso dominio. In una foresta, i controller di dominio di un dominio figlio sincronizzare l'ora con i controller di dominio nei domini padre. Quando un server viene restituito un pacchetto NTP autenticato a un client che richiede il tempo, il pacchetto è firmato tramite una chiave di sessione Kerberos definita da un account trust tra domini. Quando un nuovo dominio di Active Directory viene unito un insieme di strutture, e il servizio Accesso rete gestisce la chiave di sessione, viene creato l'account trust tra domini. In questo modo, il controller di dominio configurati come affidabile nel dominio radice della foresta diventa l'origine dell'ora autenticato per tutti i controller di dominio in domini padre e figlio e indirettamente per tutti i computer che si trova nella struttura di dominio.  
  
Il servizio ora di Windows può essere configurato per funzionare tra foreste, ma è importante notare che questa configurazione non è sicura. Ad esempio, un server NTP potrebbe essere disponibile in una foresta diversa. Tuttavia, poiché tale computer è in una foresta diversa, non è Nessuna chiave di sessione Kerberos per l'accesso e autenticare i pacchetti NTP. Per ottenere la sincronizzazione dell'ora accurata da un computer in una foresta diversa, il client necessita dell'accesso di rete al computer e il servizio ora deve essere configurato per utilizzare un'origine di tempo specifico si trova in altra foresta. Se un client viene configurato manualmente per tempo di accesso da un server NTP all'esterno della propria gerarchia di dominio, i pacchetti NTP inviati tra il client e server di non vengono autenticati e pertanto non sono sicure. Anche con l'implementazione di trust tra foreste, il servizio ora di Windows non è sicura tra foreste. Anche se il canale sicuro Netlogon è il meccanismo di autenticazione per il servizio ora di Windows, non è supportata l'autenticazione tra foreste.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivi hardware che sono supportati dal servizio ora di Windows  
Orologi basati su hardware, ad esempio GPS o orologi radio vengono spesso utilizzati come dispositivi di orologio riferimento estremamente precisa. Per impostazione predefinita, il provider servizi orari NTP servizio ora di Windows non supporta la connessione diretta di un dispositivo hardware in un computer, sebbene sia possibile creare un provider servizi orari indipendente basata sul software che supporta questo tipo di connessione. Questo tipo di provider, in combinazione con il servizio ora di Windows, è possibile fornire un riferimento temporale stabile e affidabile.  
  
Dispositivi hardware, ad esempio un orologio cesium o un ricevitore di posizionamento GPS (Global System), forniscono l'ora corrente esatta seguendo uno standard per ottenere una definizione precisa di tempo. Orologi cesium sono estremamente stabili e non vengono influenzati da fattori quali la temperatura e pressione umidità, ma sono anche molto costosi. Un ricevitore GPS è molto meno costoso da usare ed è anche un orologio riferimento preciso. I ricevitori GPS ottengono l'ora da satelliti che ottengono l'ora da un orologio cesium. Senza l'utilizzo di un provider indipendente dal tempo, il server di riferimento ora di Windows può acquisire all'ora tramite la connessione a un server NTP esterno, che è connesso a un dispositivo hardware tramite un telefono o Internet. Le organizzazioni, ad esempio l'Osservatorio navale degli Stati Uniti fornire i server NTP connessi a orologi riferimento estremamente affidabile.  
  
Molti ricevitori GPS e altri dispositivi ora possono fungere da server NTP in una rete. È possibile configurare la foresta di Active Directory per sincronizzare l'ora da tali dispositivi hardware esterni solo se anche agiscono come server NTP sulla rete. A tale scopo, configurare il controller di dominio funzionano come controller di dominio primario (emulatore) nella radice della foresta per la sincronizzazione con il server NTP fornito dal dispositivo GPS. A tale scopo, vedere [configurare il servizio ora di Windows nell'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Simple Network Time Protocol  
Simple Network Time Protocol (SNTP) è un protocollo ora semplificato destinato a server e client che non richiedono il livello di precisione che fornisce NTP. SNTP, una versione più elementare di NTP, è il protocollo principale ora utilizzato in Windows 2000. Poiché i formati di pacchetti di rete di NTP e SNTP sono identici, i due protocolli sono interoperativi. La differenza principale tra i due è che SNTP non dispone di gestione degli errori e sistemi complessi di filtro che fornisce NTP. Per ulteriori informazioni sul protocollo ora rete semplice, vedere RFC 1769 nel Database IETF RFC.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilità tra protocolli ora  
Il servizio ora di Windows può funzionare in un ambiente misto di computer che eseguono Windows 2000, Windows XP e Windows Server 2003, poiché il protocollo SNTP utilizzato in Windows 2000 è interoperativo con il protocollo NTP in Windows XP e Windows Server 2003.  
  
Il servizio ora di Windows NT Server 4.0, chiamato TimeServ, sincronizza l'ora in una rete di Windows NT 4.0. TimeServ è una funzionalità aggiuntiva disponibile come parte di *Resource Kit di Microsoft Windows NT 4.0* e non fornisce il livello di affidabilità di sincronizzazione dell'ora richiesto da Windows Server 2003.  
  
Il servizio ora di Windows può interagire con i computer che eseguono Windows NT 4.0 perché è possibile sincronizzare l'ora con computer che eseguono Windows 2000 o Windows Server 2003. Tuttavia, un computer che esegue Windows 2000 o Windows Server 2003 non rileva automaticamente i server Windows NT 4.0. Ad esempio, se il dominio è configurato per sincronizzare l'ora utilizzando il dominio basato su gerarchia metodo di sincronizzazione e si desidera che i computer nella gerarchia di domini per sincronizzare l'ora con un controller di dominio Windows NT 4.0, è necessario configurare tali computer manualmente per la sincronizzazione con i controller di dominio Windows NT 4.0.  
  
Windows NT 4.0 utilizza un meccanismo più semplice per la sincronizzazione dell'ora rispetto a usato dal servizio ora di Windows. Pertanto, per garantire la sincronizzazione dell'ora precisa in tutta la rete, è consigliabile aggiornare qualsiasi controller di dominio Windows NT 4.0 a Windows 2000 o Windows Server 2003.  
  
## <a name="windows-time-service-processes-and-interactions"></a>Ora di Windows, processi e interazioni  

Il servizio ora di Windows è progettato per sincronizzare gli orologi dei computer in una rete. Il processo di sincronizzazione ora di rete, l'acronimo di convergenza di tempo, si verifica in una rete come ogni volta che accede a computer da un server più accurato. Convergenza ora prevede un processo mediante il quale un server autorevole fornisce l'ora corrente per i computer client sotto forma di pacchetti NTP. Le informazioni fornite all'interno di un pacchetto indicano se un intervento di regolazione deve apportate all'ora corrente del computer in modo che siano sincronizzato con il server più accurato.  
  
Come parte del processo di convergenza di tempo, i membri del dominio tentano di sincronizzare l'ora con qualsiasi controller di dominio che si trova nello stesso dominio. Se il computer è un controller di dominio, tenta di eseguire la sincronizzazione con un controller di dominio più rilevanti.  
  
Computer che eseguono Windows XP Home Edition o i computer che non fanno parte di un dominio non tentare di eseguire la sincronizzazione con la gerarchia dei domini, ma sono configurati per impostazione predefinita per ottenere l'ora da time.windows.com.  
  
Per stabilire un computer che esegue Windows Server 2003 come autorevole, il computer deve essere configurato per essere un'origine ora affidabile. Per impostazione predefinita, il primo controller di dominio che viene installato in un dominio di Windows Server 2003 viene configurato automaticamente per essere un'origine ora affidabile. Poiché il computer autorevole per il dominio, deve essere configurato per la sincronizzazione con un'origine ora esterna anziché con la gerarchia dei domini. Per impostazione predefinita, anche tutti gli altri membri del dominio di Windows Server 2003 sono configurati per la sincronizzazione con la gerarchia dei domini.  
  
Dopo aver stabilito una rete di Windows Server 2003, è possibile configurare il servizio ora di Windows per utilizzare una delle seguenti opzioni per la sincronizzazione:  
  
-   Sincronizzazione di dominio basato su gerarchia  
  
-   Un'origine di sincronizzazione manuale  
  
-   Tutti i meccanismi di sincronizzazione disponibili  
  
-   Nessuna sincronizzazione.  
  
Ciascuno di questi tipi di sincronizzazione è descritto nella sezione seguente.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronizzazione di dominio basato su gerarchia  

Sincronizzazione basata su una gerarchia di dominio utilizza la gerarchia dei domini di Active Directory per trovare un'origine affidabile con cui sincronizzare l'ora. In base alla gerarchia di dominio, il servizio ora di Windows determina l'accuratezza di ogni server di riferimento ora. In una foresta di Windows Server 2003, il computer che contiene il dominio primario (PDC) controller emulatore ruolo master operazioni, che si trova nel dominio radice della foresta, ricopre la posizione di origine dell'ora migliore, a meno che non è stata configurata un'altra origine ora affidabile. Nella figura seguente viene illustrato un percorso di sincronizzazione dell'ora tra i computer in una gerarchia di dominio.  
  
**Sincronizzazione dell'ora in una gerarchia di servizi di dominio Active Directory**  
![ora di Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)
  
#### <a name="reliable-time-source-configuration"></a>Configurazione dell'origine ora affidabile  
Un computer in cui è configurato per essere un'origine ora affidabile è identificato come radice del servizio ora di. La radice del servizio ora è il server autorevole per il dominio e in genere è configurata per recuperare l'ora da un server NTP esterno o un dispositivo hardware. Un server può essere configurato come origine ora affidabile per ottimizzare il trasferimento dell'ora in tutta la gerarchia di dominio. Se un controller di dominio è configurato per essere un'origine ora affidabile, servizio Accesso rete annuncia il controller di dominio come origine ora affidabile quando si connette alla rete. Quando altri controller di dominio cercano un'origine dell'ora per la sincronizzazione con, essi scegliere un'origine affidabile innanzitutto se è disponibile.  
  
#### <a name="time-source-selection"></a>Selezione dell'origine dell'ora  
Il processo di selezione origine ora è possibile creare due problemi in una rete:  
  
-   Cicli di sincronizzazione aggiuntive.  
  
-   Aumento del traffico di rete.  
  
Un ciclo nella rete di sincronizzazione si verifica quando ora rimane coerenza tra un gruppo di controller di dominio e contemporaneamente viene condivisa tra di essi continua senza una risincronizzazione con un'altra origine ora affidabile. Algoritmo di selezione dell'origine ora del servizio ora di Windows è progettato per proteggere questi tipi di problemi.  
  
Un computer utilizza uno dei metodi seguenti per identificare un'origine dell'ora per la sincronizzazione con:  
  
-   Se il computer non è un membro di un dominio, deve essere configurato per la sincronizzazione con un'origine ora specificata.  
  
-   Se il computer è un server membro o una workstation all'interno di un dominio, per impostazione predefinita, segue la gerarchia di Active Directory e consente di sincronizzare l'ora con un controller di dominio nel dominio locale che è attualmente in esecuzione il servizio ora di Windows.  
  
Se il computer è un controller di dominio, rendendo le query fino a sei per individuare un altro controller di dominio per la sincronizzazione con. Ogni query è progettata per identificare un'origine dell'ora con determinati attributi, ad esempio un tipo di controller di dominio, un percorso specifico, ed è o meno un'origine ora affidabile. L'origine dell'ora deve inoltre rispettare i seguenti vincoli:  
  
-   Origine ora affidabile può sincronizzare solo con un controller di dominio nel dominio padre.  
  
-   Un emulatore PDC può eseguire la sincronizzazione con un'origine ora affidabile nel proprio dominio o qualsiasi controller di dominio nel dominio padre.  
  
Se il controller di dominio non è in grado di sincronizzare con il tipo di controller di dominio che sta eseguendo una query, non viene eseguita la query. Il controller di dominio conosca il tipo di computer che si può ottenere l'ora di inizio prima di inviare la query. Ad esempio, un emulatore PDC non tenta di numeri di query, tre o sei perché un controller di dominio non tenta di sincronizzarsi con se stesso.  
  
Nella tabella seguente elenca le query eseguite da un controller di dominio a trovare un'origine dell'ora e l'ordine in cui vengono eseguite le query.  
  
**Query di origine dell'ora del controller di dominio**  
  
|Numero di query|Controller di dominio|Location|Affidabilità dell'origine dell'ora|  
|----------------|---------------------|------------|------------------------------|  
|1|Controller di dominio principale|Nel sito|Preferisce affidabili origine ora ma è possibile sincronizzare con un'origine ora non è affidabile se questo è tutto ciò che è disponibile.|  
|2|Controller di dominio locale|Nel sito|Sincronizza solo con un'origine ora affidabile.|  
|3|Emulatore PDC|Nel sito|Non è valida.<br /><br />Un controller di dominio non tenta di sincronizzarsi con se stesso.|  
|4|Controller di dominio principale|Al sito|Preferisce affidabili origine ora ma è possibile sincronizzare con un'origine ora non è affidabile se questo è tutto ciò che è disponibile.|  
|5|Controller di dominio locale|Al sito|Sincronizza solo con un'origine ora affidabile.|  
|6|Emulatore PDC|Al sito|Non è valida.<br /><br />Un controller di dominio non tenta di sincronizzarsi con se stesso.| 
  
**Nota**  
  
-   Un computer non sincronizza con se stessa. Se il computer in cui il tentativo di sincronizzazione è l'emulatore PDC locale, non tenta query 3 o 6.  
  
Ogni query restituisce un elenco di controller di dominio che può essere utilizzato come origine dell'ora. Ora di Windows assegna ogni controller di dominio che è richiesto un punteggio in base all'affidabilità e la posizione del controller di dominio. Nella tabella seguente sono elencati i punteggi assegnati da ora di Windows per ogni tipo di controller di dominio.  
  
**Determinazione del Punteggio**  
  
|Stato del Controller di dominio|Punteggio|  
|----------------------------|---------|  
|Controller di dominio nello stesso sito|8|  
|Controller di dominio contrassegnato come un'origine ora affidabile|4|  
|Controller di dominio che si trova nel dominio padre|2|  
|Controller di dominio che è un emulatore PDC|1|  
  
Quando il servizio ora di Windows determina che una volta identificati i controller di dominio con il miglior punteggio possibili, non vengono resi più query. I punteggi assegnati dal servizio ora di sono cumulativi, il che significa che un emulatore PDC che si trova nello stesso sito riceve un punteggio di nove.  
  
Se la radice del servizio ora non è configurata per la sincronizzazione con un'origine esterna, l'orologio interno hardware del computer consente di gestire il tempo.  
  
### <a name="manually-specified-synchronization"></a>La sincronizzazione manuale  
La sincronizzazione manuale consente di definire un unico peer o un elenco dei peer da cui un computer ottiene l'ora. Se il computer non è un membro di un dominio, devono essere configurato manualmente per la sincronizzazione con un'origine ora specificata. Un computer che è che un membro di un dominio è configurato per impostazione predefinita per la sincronizzazione dalla gerarchia di domini, la sincronizzazione manuale è particolarmente utile per la radice della foresta del dominio o per i computer che non fanno parte di un dominio. Specificare manualmente un server NTP esterno per la sincronizzazione con il computer autorevole per il dominio fornisce ora affidabile. Tuttavia, la configurazione del computer autorevole per il dominio per la sincronizzazione con un orologio hardware è effettivamente una soluzione migliore per fornire l'ora più preciso e sicuro per il dominio.  
  
Origini dell'ora specificata manualmente non vengono autenticate a meno che non viene scritto un provider di tempo specifico e sono pertanto vulnerabile agli attacchi. Inoltre, se un computer viene sincronizzato con un'origine specificata manualmente piuttosto che il controller di dominio di autenticazione, i due computer potrebbe non essere sincronizzata, causando l'esito negativo dell'autenticazione Kerberos. Ciò potrebbe causare altre azioni che richiedono l'autenticazione di rete avrà esito negativo, ad esempio la stampa o la condivisione di file. Se solo la radice della foresta è configurata per la sincronizzazione con un'origine esterna, tutti gli altri computer all'interno della foresta rimangano sincronizzati tra loro, rendendo difficile gli attacchi di riproduzione.  
  
### <a name="all-available-synchronization-mechanisms"></a>Tutti i meccanismi di sincronizzazione disponibili  

L'opzione "tutti i meccanismi di sincronizzazione disponibili" è il metodo di sincronizzazione più importante per gli utenti in una rete. Questo metodo consente la sincronizzazione con la gerarchia di dominio e può inoltre fornire un'alternativa origine ora, se la gerarchia dei domini non è più disponibile, a seconda della configurazione. Se il client è in grado di sincronizzare l'ora con la gerarchia dei domini, l'origine dell'ora automaticamente il fallback per l'origine dell'ora specificata per il **NtpServer** impostazione. Questo metodo di sincronizzazione è più probabile fornire ai client l'ora esatta.  

### <a name="stopping-time-synchronization"></a>Sincronizzazione dell'ora di arresto  
Vi sono alcune situazioni in cui verrà desidera impedire al computer il tempo di sincronizzazione. Ad esempio, se un computer tenta di sincronizzare da un'origine ora su Internet o da un altro sito in una rete WAN tramite una connessione remota, può comportare costosi telefonica. Quando si disabilita la sincronizzazione del computer, si impedisce che il computer sta tentando di accedere a un'origine ora su una connessione remota.  
  
È anche possibile disabilitare la sincronizzazione per evitare la generazione di errori nel registro eventi. Ogni volta che un computer tenta di eseguire la sincronizzazione con un'origine ora non è disponibile, viene generato un errore nel registro eventi. Se un'origine dell'ora viene eseguita all'esterno della rete per la manutenzione pianificata e non si prevede di riconfigurare il client di sincronizzazione da un'altra origine, è possibile disabilitare la sincronizzazione sul client per evitare che il tentativo di sincronizzazione, mentre quella del server non è disponibile.  
  
È utile disattivare la sincronizzazione nel computer in cui viene definita come la radice della rete di sincronizzazione. Ciò indica che il computer principale attendibile l'orologio locale. Se la radice della gerarchia di sincronizzazione non è impostata su **NoSync** e se non è in grado di sincronizzare con un altro, i client non accettano il pacchetto che invia questo computer perché l'ora non è attendibile.
  
Solo i server che sono considerati attendibili dai client, anche se non sono state sincronizzate con un'altra origine ora sono quelli che sono stati identificati dal client come server di riferimento orario affidabile.  
  
### <a name="disabling-the-windows-time-service"></a>Disabilitazione del servizio ora di Windows  
Il servizio ora di Windows (W32Time) può essere completamente disattivato. Se si sceglie di implementare un prodotto di sincronizzazione ora di terze parti che utilizza NTP, è necessario disabilitare il servizio ora di Windows. In questo modo tutti i server NTP richiedono l'accesso alla porta di protocollo UDP (User Datagram) 123 e fino a quando il servizio ora di Windows è in esecuzione nel sistema operativo Windows Server 2003, la porta 123 rimane riservata ora di Windows.  
  
## <a name="network-ports-used-by-windows-time-service"></a>Porte di rete utilizzata dal servizio ora di Windows  
Il servizio ora di Windows comunica in una rete per identificare le origini di ora affidabile, ottenere informazioni sull'ora e fornire le informazioni ad altri computer. Questa comunicazione viene eseguita come definito dal NTP e SNTP RFC.  
  
**Assegnazioni delle porte per il servizio ora di Windows**  
  
|Nome servizio|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/D|  
|SNTP|123|N/D|  
  
## <a name="see-also"></a>Vedi anche  
[Riferimento tecnico per il servizio ora di windows](windows-time-service-tech-ref.md)
[strumenti e impostazioni del servizio ora di windows](Windows-Time-Service-Tools-and-Settings.md)
[articolo 902229 della Microsoft Knowledge base](https://go.microsoft.com/fwlink/?LinkId=186066)