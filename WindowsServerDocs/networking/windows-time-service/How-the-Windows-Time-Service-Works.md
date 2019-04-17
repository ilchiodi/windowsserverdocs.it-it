---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Come funziona il servizio ora di Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: c9ab52229c2a16d6ae6b0b2733c52e53a25cfa44
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/05/2018
---
# <a name="how-the-windows-time-service-works"></a>Come funziona il servizio ora di Windows

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**In questa sezione**  
  
-   [Architettura del servizio ora di Windows](#w2k3tr_times_how_rrfo)  
  
-   [Protocolli ora servizio ora di Windows](#w2k3tr_times_how_ekoc)  
  
-   [Ora di Windows, processi e interazioni](#w2k3tr_times_how_izcr)  
  
-   [Porte di rete utilizzata dal servizio ora di Windows](#w2k3tr_times_how_ydum)  
  
> [!NOTE]  
> Questo argomento illustra solo come funziona il servizio ora di Windows (W32Time). Per informazioni su come configurare il servizio ora di Windows, vedere l'elenco di argomenti nella sezione [dove trovare informazioni di Windows ora servizio configurazione](https://technet.microsoft.com/library/cc773061.aspx).  
  
> [!NOTE]  
> In Windows Server 2003 e Microsoft Windows 2000 Server, il servizio directory è denominato servizio directory Active Directory. In Windows Server 2008 e versioni successive, il servizio directory è denominato servizi di dominio Active Directory (AD DS). Il resto di questo argomento fa riferimento a servizi di dominio Active Directory, ma le informazioni sono applicabili anche a Active Directory.  
  
Anche se il servizio ora di Windows non è un'implementazione esatta del protocollo NTP (Network Time), Usa la suite di algoritmi complessa definiti nelle specifiche NTP per garantire che gli orologi dei computer in una rete siano più accurati possibile. In teoria, tutti gli orologi dei computer in un dominio di Active Directory vengono sincronizzati con l'ora di un computer autorevole. Molti fattori possono influire sulla sincronizzazione dell'ora in una rete. I seguenti fattori spesso influiscono sull'accuratezza della sincronizzazione in Active Directory:  
  
-   Condizioni della rete  
  
-   L'accuratezza dell'orologio hardware del computer  
  
-   La quantità di risorse della CPU e della rete disponibili per il servizio ora di Windows  
  
> [!IMPORTANT]  
> Prima di Windows Server 2016, il servizio W32Time non è stato progettato per soddisfare le esigenze dell'applicazione di scadenza.  Tuttavia, gli aggiornamenti a Windows Server 2016 ora consentono di implementare una soluzione per 1 ms accuratezza nel dominio.  Vedere [ora di Windows 2016 accurata](accurate-time.md) e [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](https://go.microsoft.com/fwlink/?LinkID=179459) per ulteriori informazioni.  
  
Computer che sincronizzi all'ora meno frequentemente o non aggiunti a un dominio sono configurati, per impostazione predefinita, per la sincronizzazione con time.windows.com. Pertanto, è possibile garantire l'accuratezza di tempo nei computer che dispongono di intermittente o nessuna connessione di rete.  
  
Un insieme di strutture di dominio Active Directory dispone di una gerarchia di sincronizzazione di tempo predeterminato. Il servizio ora di Windows sincronizza il tempo tra i computer all'interno della gerarchia, con gli orologi di riferimento più accurati nella parte superiore. Se più di un'origine ora è configurata in un computer, l'ora di Windows utilizza algoritmi NTP per selezionare l'origine dell'ora migliore dalle origini configurate in base alle capacità del computer per la sincronizzazione con tale origine dell'ora. Il servizio ora di Windows non supporta la sincronizzazione di rete da broadcast o multicast peer. Per ulteriori informazioni su queste funzionalità NTP, vedere RFC 1305 nel Database IETF RFC.  
  
Ogni computer che esegue il servizio ora di Windows utilizza il servizio per mantenere l'ora più accurata. Computer che sono membri di un dominio di agire come client ora per impostazione predefinita, pertanto, nella maggior parte dei casi non è necessario configurare il servizio ora di Windows. Tuttavia, il servizio ora di Windows può essere configurato per richiedere l'ora da un'origine ora riferimento designato e ora può anche fornire ai client.
  
Il livello a cui l'ora del computer sia precisa viene chiamato un strato. L'origine dell'ora più accurata in una rete (ad esempio un orologio hardware) occupa il livello più basso strato o uno strato. Questa origine ora esatta viene chiamata un orologio di riferimento. Un server NTP che acquisisce il tempo di direttamente da un orologio riferimento occupa un strato di un livello più elevato rispetto a quello dell'orologio del riferimento. Risorse di acquisire l'ora del server NTP sono due passaggi per l'orologio di riferimento e pertanto occupano un strato di due superiore l'origine dell'ora più accurata e così via. Come numero strato di un computer aumenta, l'ora del clock di sistema potrebbe diventare meno accurato. Pertanto, il livello strato di qualsiasi computer è un indicatore in quale misura tale computer viene sincronizzato con l'origine dell'ora più accurata.  
  
Quando il gestore W32Time riceve esempi, utilizza algoritmi speciali in NTP per determinare quale degli esempi di tempo risulta più appropriato per l'uso. Il servizio ora utilizza anche un altro set di algoritmi per determinare quale delle origini ora configurata è il più accurato. Quando il servizio ora ha determinato quali Time è migliore, in base ai criteri precedenti, consente di regolare la frequenza di clock locale per consentire la convergenza verso l'ora corretta. Se la differenza tra l'orologio locale e l'esempio di ora esatta selezionato (detto anche il tempo di inclinazione) è troppo grande per correggere regolando la frequenza di clock locale, il servizio ora imposta l'orologio locale sull'ora corretta. Questa regolazione di frequenza di clock o modifica dell'ora dell'orologio diretto è noto come disciplina di orologio.  
  
## <a name="w2k3tr_times_how_rrfo"></a>Architettura del servizio ora di Windows  
Il servizio ora di Windows include i componenti seguenti:  
  
-   Gestione controllo servizi  
  
-   Gestione del servizio ora di Windows  
  
-   Disciplina di orologio  
  
-   Provider servizi orari  
  
Nella figura seguente è illustrata l'architettura del servizio ora di Windows.  
  
**Architettura del servizio ora di Windows**  
  
![Ora di Windows](media/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)  
  
Gestione controllo servizi è responsabile dell'avvio e arresto del servizio ora di Windows. Gestione del servizio ora di Windows è responsabile dell'avvio l'azione del provider servizi orari NTP incluso con il sistema operativo. Gestione del servizio ora di Windows controlla tutte le funzioni del servizio ora di Windows e l'unione di tutti gli esempi di tempo. Oltre a fornire informazioni sullo stato del sistema corrente, ad esempio l'origine dell'ora corrente o l'ultima volta l'orologio di sistema è stato aggiornato, Gestione servizio ora di Windows è anche responsabile per la creazione di eventi nel registro eventi.  
  
Il processo di sincronizzazione ora prevede i passaggi seguenti:  
  
-   Provider di input richiesta e ricevere esempi ora da origini dell'ora NTP configurate.  
  
-   Questi esempi vengono quindi passati al tempo Gestione servizi di Windows, che raccoglie tutti gli esempi e li passa il sottocomponente disciplina di orologio.  
  
-   Il sottocomponente disciplina orologio applica gli algoritmi NTP che comporta la selezione dell'esempio tempo migliore.  
  
-   Il sottocomponente disciplina orologio consente di regolare l'ora dell'orologio di sistema per l'ora più accurata regolare la frequenza di clock o modificando direttamente l'ora.  
  
Se un computer è stato designato come un server, può inviare il tempo a qualsiasi computer che richiede la sincronizzazione dell'ora in qualsiasi punto del processo.  
  
## <a name="w2k3tr_times_how_ekoc"></a>Protocolli ora servizio ora di Windows  

Protocolli ora determinano in quale misura due computer orologi vengono sincronizzati. Un protocollo di tempo è responsabile per determinare le migliori informazioni disponibili per l'ora e convergenti gli orologi per garantire che un'ora coerenza nei sistemi separati.  
  
Il servizio ora di Windows utilizza il protocollo NTP (Network Time) per sincronizzare l'ora in una rete. NTP è un protocollo di tempo Internet che include gli algoritmi di disciplina necessari per la sincronizzazione degli orologi. NTP è un protocollo di tempo più accurato rispetto il Simple Network Time Protocol (SNTP) che viene utilizzato in alcune versioni di Windows. tuttavia W32Time continua a supportare SNTP per abilitare la compatibilità con computer che eseguono servizi basati su SNTP tempo, ad esempio Windows 2000.  
  
### <a name="network-time-protocol"></a>Network Time Protocol  
Protocollo NTP (Network Time) il valore predefinito è ora il protocollo di sincronizzazione utilizzato dal servizio ora di Windows nel sistema operativo. NTP è un protocollo di tempo a tolleranza d'errore e altamente scalabile ed è il protocollo utilizzato più spesso per sincronizzare gli orologi dei computer tramite un riferimento ora designato.  
  
Sincronizzazione dell'ora NTP viene eseguita in un periodo di tempo e comporta il trasferimento di NTP pacchetti in rete. Pacchetti NTP contengono timestamp, che includono un campione ora sia il client e il server che partecipa della sincronizzazione temporale.  
  
NTP si basa su un orologio di riferimento per definire l'ora più accurata da utilizzare e Sincronizza tutti gli orologi di una rete di orologio tale riferimento. NTP utilizza Coordinated Universal Time (UTC) come standard universale per ora corrente. UTC è indipendente dei fusi orari e Abilita NTP essere utilizzati ovunque nel mondo indipendentemente dalle impostazioni del fuso orario.  
  
#### <a name="ntp-algorithms"></a>Algoritmi NTP  
NTP include due algoritmi, un algoritmo di filtraggio orologio e un algoritmo di selezione di orologio, per agevolare il servizio ora di Windows per determinare la migliore esempio time. L'algoritmo di filtraggio orologio è progettato per esplorare gli esempi di tempo che vengono ricevuti da origini dell'ora richiesto e determinano il migliore esempi da ogni origine. L'algoritmo di selezione di orologio determina quindi server di più preciso sulla rete. Queste informazioni vengono quindi passate all'algoritmo di disciplina di orologio, che utilizza le informazioni raccolte per correggere l'orologio locale del computer, durante la compensazione degli errori a causa di imprecisione orologio computer e la latenza di rete.  
  
Gli algoritmi NTP sono più accurati in condizioni di carico di rete e server light-medio. Come con qualsiasi algoritmo che tiene conto di tempo di transito in rete, gli algoritmi NTP potrebbero scarse in condizioni di congestione della rete estremi. Per ulteriori informazioni sugli algoritmi NTP, vedere RFC 1305 nel Database IETF RFC.  
  
#### <a name="ntp-time-provider"></a>Provider servizi orari NTP  
Il servizio ora di Windows è un pacchetto di sincronizzazione ora di completamento in grado di supportare un'ampia gamma di dispositivi hardware e i protocolli di tempo. Per abilitare questo supporto, il servizio utilizza provider servizi orari plug. Un provider servizi orari è responsabile per entrambi acquisizione accurate timestamp (dalla rete o da hardware) o per fornire i timestamp in altri computer in rete.  
  
Il provider NTP è il provider servizi orari standard incluso con il sistema operativo. Il provider NTP conforme agli standard di specificato dalla versione 3 per un client e server NTP e può interagire con SNTP client e server per la compatibilità con Windows 2000 e altri client SNTP. Il provider NTP nel servizio ora di Windows è costituito da due parti:  
  
-   **Provider di output NtpServer.** Si tratta di un server che risponde alle richieste di tempo del client nella rete.  
  
-   **Provider di input NtpClient.** Si tratta di un client di tempo che ottiene informazioni sull'ora da un'altra origine, un dispositivo hardware o un server NTP e può restituire esempi utili per sincronizzare l'orologio locale.  
  
Sebbene le operazioni effettive di questi due provider sono strettamente correlate, appaiono indipendente per il servizio ora. A partire da Windows 2000 Server, quando un computer Windows è connesso a una rete, viene configurato come client NTP. Inoltre, i computer che eseguono l'ora di Windows service solo tentativo di sincronizzare l'ora con un controller di dominio o un'origine ora specificata manualmente per impostazione predefinita. Questi sono provider servizi orari preferito perché sono automaticamente disponibili e sicure origini del tempo.  
  
#### <a name="ntp-security"></a>Sicurezza NTP  

All'interno di una foresta di Active Directory, il servizio ora di Windows si basa sulle funzionalità di protezione dominio standard per applicare l'autenticazione di dati temporali. La sicurezza dei pacchetti NTP inviati tra un computer membro del dominio e un controller di dominio locale che funge da un server si basa sull'autenticazione chiave condivisa. Il servizio ora di Windows utilizza chiave di sessione Kerberos del computer per creare firme autenticate pacchetti NTP che vengono inviati attraverso la rete. NTP pacchetti non vengono trasmessi all'interno di canale sicuro Netlogon. Al contrario, quando un computer richiede il tempo da un controller di dominio nella gerarchia di dominio, il servizio ora di Windows richiede che vengano autenticati l'ora. Il controller di dominio restituisce quindi le informazioni necessarie sotto forma di un valore a 64 bit che è stato autenticato con la chiave di sessione dal servizio Accesso rete. Se il pacchetto NTP restituito non è firmato con la chiave di sessione del computer o è firmato in modo non corretto, il tempo viene rifiutato. Tutti questi errori di autenticazione vengono registrati nel registro eventi. In questo modo, il servizio ora di Windows fornisce protezione per i dati NTP in una foresta di Active Directory.  
  
In genere, i client ora Windows ottengono automaticamente ora esatta per la sincronizzazione dal controller di dominio nello stesso dominio. In una foresta, i controller di dominio di un dominio figlio sincronizzare l'ora con i controller di dominio nei domini padre. Quando un server viene restituito un pacchetto NTP autenticato a un client che richiede il tempo, il pacchetto è firmato tramite una chiave di sessione Kerberos definita da un account trust tra domini. Quando un nuovo dominio di Active Directory viene unito un insieme di strutture, e il servizio Accesso rete gestisce la chiave di sessione, viene creato l'account trust tra domini. In questo modo, il controller di dominio configurati come affidabile nel dominio radice della foresta diventa l'origine dell'ora autenticato per tutti i controller di dominio in domini padre e figlio e indirettamente per tutti i computer che si trova nell'albero di dominio.  
  
Il servizio ora di Windows può essere configurato per funzionare tra foreste, ma è importante notare che questa configurazione non è sicura. Ad esempio, un server NTP potrebbe essere disponibile in una foresta diversa. Tuttavia, poiché tale computer è in una foresta diversa, non è Nessuna chiave di sessione Kerberos con cui eseguire l'accesso e autenticare i pacchetti NTP. Per ottenere la sincronizzazione dell'ora accurata da un computer in una foresta diversa, il client necessita dell'accesso di rete al computer e il servizio ora deve essere configurato per utilizzare un'origine di tempo specifico si trova in altra foresta. Se un client viene configurato manualmente per tempo di accesso da un server NTP all'esterno della propria gerarchia di dominio, i pacchetti NTP inviati tra il client e server non vengono autenticati e pertanto non sono sicure. Anche con l'implementazione di trust tra foreste, il servizio ora di Windows non è sicura tra foreste. Anche se il canale sicuro Netlogon è il meccanismo di autenticazione per il servizio ora di Windows, l'autenticazione tra foreste non è supportato.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivi hardware supportati dal servizio ora di Windows  
Orologi basati su hardware, ad esempio GPS o orologi radio vengono spesso utilizzati come dispositivi di orologio riferimento estremamente precisa. Per impostazione predefinita, il provider servizi orari NTP servizio ora di Windows non supporta la connessione diretta di un dispositivo hardware in un computer, anche se è possibile creare un provider servizi orari indipendente basata sul software che supporta questo tipo di connessione. Questo tipo di provider, in combinazione con il servizio ora di Windows, può fornire un riferimento temporale stabile e affidabile.  
  
Dispositivi hardware, ad esempio un orologio cesium o un ricevitore di posizionamento GPS (Global System), specificare l'ora corrente esatta seguendo uno standard per ottenere una definizione precisa di tempo. Orologi cesium sono estremamente stabili e non vengono influenzati da fattori quali la temperatura, pressione umidità, ma sono anche molto costosi. Un ricevitore GPS è molto meno costoso da usare ed è anche un orologio riferimento preciso. I ricevitori GPS ottengono l'ora da satelliti che ottengono l'ora da un orologio cesium. Senza l'utilizzo di un provider servizi orari indipendente, il server di riferimento ora di Windows può acquisire all'ora tramite una connessione a un server NTP esterno, che è connesso a un dispositivo hardware tramite un telefono o Internet. Le organizzazioni, ad esempio l'Osservatorio navale degli Stati Uniti forniscono i server NTP connessi a orologi riferimento estremamente affidabile.  
  
Molti ricevitori GPS e altri dispositivi ora possono fungere da server NTP in una rete. È possibile configurare la foresta di Active Directory per sincronizzare l'ora da tali dispositivi hardware esterni solo se anche agiscono come server NTP sulla rete. A tale scopo, configurare il controller di dominio funzionano come controller di dominio primario (emulatore) nella radice della foresta per la sincronizzazione con il server NTP fornito dal dispositivo GPS. A tale scopo, vedere [configurare il servizio ora di Windows sull'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Simple Network Time Protocol  
Simple Network Time Protocol (SNTP) è un protocollo ora semplificato destinato a server e client che non richiedono il livello di precisione che fornisce NTP. SNTP, una versione più elementare di NTP, è il protocollo principale ora utilizzato in Windows 2000. Poiché i formati di pacchetti di rete di NTP e SNTP sono identici, i due protocolli sono interoperativi. La differenza principale tra i due è che SNTP non dispone di gestione degli errori e sistemi complessi di filtro che fornisce NTP. Per ulteriori informazioni sul protocollo ora rete semplice, vedere RFC 1769 nel Database IETF RFC.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilità tra protocolli ora  
Il servizio ora di Windows può funzionare in un ambiente misto di computer che eseguono Windows 2000, Windows XP e Windows Server 2003, poiché il protocollo SNTP utilizzato in Windows 2000 è interoperativo con il protocollo NTP in Windows XP e Windows Server 2003.  
  
Il servizio ora di Windows NT Server 4.0, chiamato TimeServ, sincronizza l'ora in una rete di Windows NT 4.0. TimeServ è una funzionalità aggiuntiva disponibile come parte di *Resource Kit di Microsoft Windows NT 4.0* e non fornisce il livello di affidabilità di sincronizzazione dell'ora richiesto da Windows Server 2003.  
  
Il servizio ora di Windows può interagire con i computer che eseguono Windows NT 4.0 perché è possibile sincronizzare l'ora con computer che eseguono Windows 2000 o Windows Server 2003. Tuttavia, un computer che esegue Windows 2000 o Windows Server 2003 non rileva automaticamente i server di riferimento ora di Windows NT 4.0. Ad esempio, se il dominio è configurato per sincronizzare l'ora utilizzando il dominio basato su gerarchia metodo di sincronizzazione e si desidera che i computer nella gerarchia di domini per sincronizzare l'ora con un controller di dominio Windows NT 4.0, è necessario configurare tali computer manualmente per la sincronizzazione con i controller di dominio Windows NT 4.0.  
  
Windows NT 4.0 utilizza un meccanismo più semplice per la sincronizzazione dell'ora rispetto a viene utilizzato il servizio ora di Windows. Pertanto, per garantire la sincronizzazione dell'ora precisa in tutta la rete, è consigliabile aggiornare qualsiasi controller di dominio Windows NT 4.0 a Windows 2000 o Windows Server 2003.  
  
## <a name="w2k3tr_times_how_izcr"></a>Ora di Windows, processi e interazioni  

Il servizio ora di Windows è progettato per sincronizzare gli orologi dei computer in una rete. Il processo di sincronizzazione ora di rete, l'acronimo di convergenza di tempo, si verifica in una rete come ogni volta che accede a computer da un server più accurato. Convergenza ora prevede un processo mediante il quale un server autorevole fornisce l'ora corrente per i computer client sotto forma di pacchetti NTP. Le informazioni fornite all'interno di un pacchetto indicano se un intervento di regolazione deve apportate all'ora corrente del computer in modo che è sincronizzato con il server più accurato.  
  
Come parte del processo di convergenza di tempo, i membri del dominio tentano di sincronizzare l'ora con qualsiasi controller di dominio che si trova nello stesso dominio. Se il computer è un controller di dominio, tenta di sincronizzare con un controller di dominio più rilevanti.  
  
Computer che eseguono Windows XP Home Edition o i computer che non appartengono a un dominio non tenta di sincronizzarsi con la gerarchia dei domini, ma sono configurati per impostazione predefinita per ottenere l'ora da time.windows.com.  
  
Per stabilire un computer che esegue Windows Server 2003 come autorevole, il computer deve essere configurato per essere un'origine ora affidabile. Per impostazione predefinita, il primo controller di dominio che viene installato in un dominio Windows Server 2003 viene configurato automaticamente per essere un'origine ora affidabile. Poiché il computer autorevole per il dominio, deve essere configurato per la sincronizzazione con un'origine ora esterna anziché con la gerarchia dei domini. Per impostazione predefinita, tutti gli altri membri del dominio di Windows Server 2003 sono configurati anche per la sincronizzazione con la gerarchia dei domini.  
  
Dopo aver stabilito una rete di Windows Server 2003, è possibile configurare il servizio ora di Windows per utilizzare una delle seguenti opzioni per la sincronizzazione:  
  
-   Sincronizzazione di dominio basato su gerarchia  
  
-   Un'origine di sincronizzazione manuale  
  
-   Tutti i meccanismi di sincronizzazione disponibili  
  
-   Nessuna sincronizzazione.  
  
Ognuno di questi tipi di sincronizzazione è descritto nella sezione seguente.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronizzazione di dominio basato su gerarchia  

Sincronizzazione basata su una gerarchia di dominio utilizza la gerarchia di dominio Active Directory per trovare un'origine affidabile con cui sincronizzare l'ora. In base alla gerarchia di dominio, il servizio ora di Windows determina l'accuratezza di ogni server di tempo. In una foresta di Windows Server 2003, il computer che contiene il dominio primario (PDC) controller emulatore ruolo master operazioni, che si trova nel dominio radice della foresta, ricopre la posizione di origine dell'ora migliore, a meno che non è stata configurata un'altra origine ora affidabile. Nella figura seguente viene illustrato un percorso di sincronizzazione dell'ora tra i computer in una gerarchia di dominio.  
  
**Sincronizzazione dell'ora in una gerarchia di Active Directory**  
  
![Ora di Windows](media/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)  
  
#### <a name="reliable-time-source-configuration"></a>Configurazione dell'origine ora affidabile  
Un computer in cui è configurato per essere un'origine ora affidabile è identificato come radice del servizio ora di. La radice del servizio ora è il server autorevole per il dominio e in genere è configurata per recuperare l'ora da un server NTP esterno o un dispositivo hardware. Un server può essere configurato come un'origine ora affidabile per ottimizzare il trasferimento dell'ora in tutta la gerarchia di dominio. Se un controller di dominio è configurato per essere un'origine ora affidabile, servizio Accesso rete annuncia il controller di dominio come origine ora affidabile quando si connette alla rete. Quando altri controller di dominio cercano un'origine dell'ora per la sincronizzazione con, essi scegliere un'origine affidabile innanzitutto se disponibile.  
  
#### <a name="time-source-selection"></a>Selezione dell'origine dell'ora  
Il processo di selezione origine ora è possibile creare due problemi in una rete:  
  
-   Cicli di sincronizzazione aggiuntive.  
  
-   Aumento del traffico di rete.  
  
Un ciclo nella rete di sincronizzazione si verifica quando ora rimane coerenza tra un gruppo di controller di dominio e contemporaneamente viene condivisa tra di essi continua senza una risincronizzazione con un'altra origine ora affidabile. Algoritmo di selezione dell'origine ora del servizio ora di Windows è progettato per proteggere questi tipi di problemi.  
  
Un computer utilizza uno dei metodi seguenti per identificare un'origine dell'ora per la sincronizzazione con:  
  
-   Se il computer non è un membro di un dominio, deve essere configurato per la sincronizzazione con un'origine ora specificata.  
  
-   Se il computer è un server membro o una workstation all'interno di un dominio, per impostazione predefinita, segue la gerarchia di dominio Active Directory e sincronizza l'ora con un controller di dominio nel dominio locale che è attualmente in esecuzione il servizio ora di Windows.  
  
Se il computer è un controller di dominio, rendendo le query fino a sei per individuare un altro controller di dominio per la sincronizzazione con. Ogni query è progettata per identificare un'origine dell'ora con determinati attributi, ad esempio un tipo di controller di dominio, un percorso specifico, ed è o meno un'origine ora affidabile. L'origine dell'ora deve inoltre rispettare i seguenti vincoli:  
  
-   Un'origine ora affidabile può sincronizzare solo con un controller di dominio nel dominio padre.  
  
-   Un emulatore PDC può sincronizzare con un'origine ora affidabile nel proprio dominio o qualsiasi controller di dominio nel dominio padre.  
  
Se il controller di dominio non è in grado di sincronizzare con il tipo di controller di dominio che sta eseguendo una query, non viene eseguita la query. Il controller di dominio conosca il tipo di computer si può ottenere l'ora di inizio prima di inviare la query. Ad esempio, un emulatore PDC non tenta di numeri di query, tre o sei perché un controller di dominio non tenta di sincronizzarsi con se stesso.  
  
La tabella seguente elenca le query che un controller di dominio consente di trovare un'origine dell'ora e l'ordine in cui vengono eseguite le query.  
  
**Origine ora del Controller di dominio esegue una query**  
  
|Numero di query|Controller di dominio|Posizione|Affidabilità dell'origine dell'ora|  
|----------------|---------------------|------------|------------------------------|  
|1|Controller di dominio principale|Nel sito|Preferisce affidabili origine ora ma è possibile sincronizzare con un'origine ora non è affidabile se questo è tutto ciò che è disponibile.|  
|2|Controller di dominio locale|Nel sito|Sincronizza solo con un'origine ora affidabile.|  
|3|Emulatore PDC|Nel sito|Non è applicabile.<br /><br />Un controller di dominio non tenta di sincronizzarsi con se stesso.|  
|4|Controller di dominio principale|Il sito|Preferisce affidabili origine ora ma è possibile sincronizzare con un'origine ora non è affidabile se questo è tutto ciò che è disponibile.|  
|5|Controller di dominio locale|Il sito|Sincronizza solo con un'origine ora affidabile.|  
|6|Emulatore PDC|Il sito|Non è applicabile.<br /><br />Un controller di dominio non tenta di sincronizzarsi con se stesso.|  
  
**Nota**  
  
-   Un computer non sincronizza con se stesso. Se il computer tentativo di sincronizzazione è l'emulatore PDC locale, non tenta query 3 o 6.  
  
Ogni query restituisce un elenco di controller di dominio che può essere utilizzato come origine dell'ora. Ora di Windows assegna ogni controller di dominio che è richiesto un punteggio in base all'affidabilità e la posizione del controller di dominio. Nella tabella seguente sono elencati i punteggi assegnati da ora di Windows per ogni tipo di controller di dominio.  
  
**Determinazione di punteggio**  
  
|Stato del Controller di dominio|Punteggio|  
|----------------------------|---------|  
|Controller di dominio nello stesso sito|8|  
|Controller di dominio contrassegnato come un'origine ora affidabile|4|  
|Controller di dominio che si trova nel dominio padre|2|  
|Controller di dominio che è un emulatore PDC|1|  
  
Quando il servizio ora di Windows determina che una volta identificati i controller di dominio con il miglior punteggio possibili, non vengono resi più query. I punteggi assegnati dal servizio ora di sono cumulativi, il che significa che un emulatore PDC che si trova nello stesso sito riceve un punteggio di nove.  
  
Se la radice del servizio ora non è configurata per la sincronizzazione con un'origine esterna, l'orologio interno hardware del computer determina il tempo.  
  
### <a name="manually-specified-synchronization"></a>Sincronizzazione manuale  
La sincronizzazione manuale consente di designare un unico peer o un elenco dei peer da cui un computer ottiene l'ora. Se il computer non è un membro di un dominio, deve essere configurato manualmente per la sincronizzazione con un'origine ora specificata. Un computer che è che un membro di un dominio è configurato per impostazione predefinita per la sincronizzazione dalla gerarchia di dominio, la sincronizzazione manuale è particolarmente utile per la radice della foresta del dominio o per i computer che non appartengono a un dominio. Specificare manualmente un server NTP esterno per la sincronizzazione con il computer autorevole per il dominio fornisce ora affidabile. Tuttavia, la configurazione del computer autorevole per il dominio per la sincronizzazione con un orologio hardware è effettivamente una soluzione migliore per fornire l'ora più preciso e sicuro al dominio.  
  
Origini dell'ora specificata manualmente non vengono autenticate a meno che non viene scritto un provider servizi orari specifici per loro e sono pertanto vulnerabile agli attacchi. Inoltre, se un computer viene sincronizzato con un'origine specificata manualmente piuttosto che il controller di dominio che esegue l'autenticazione, i due computer potrebbe essere sincronizzata, causando l'esito negativo dell'autenticazione Kerberos. Ciò potrebbe causare altre azioni che richiedono l'autenticazione di rete non riesce, ad esempio la stampa o la condivisione di file. Se solo la radice della foresta è configurata per la sincronizzazione con un'origine esterna, tutti gli altri computer all'interno della foresta rimangano sincronizzati tra loro, rendendo difficile gli attacchi di riproduzione.  
  
### <a name="all-available-synchronization-mechanisms"></a>Tutti i meccanismi di sincronizzazione disponibili  

L'opzione "tutti i meccanismi di sincronizzazione disponibili" è il metodo di sincronizzazione più importante per gli utenti in una rete. Questo metodo consente la sincronizzazione con la gerarchia dei domini e può anche fornire un'alternativa origine ora, se la gerarchia dei domini non è più disponibile, a seconda della configurazione. Se il client è in grado di sincronizzare l'ora con la gerarchia dei domini, l'origine dell'ora automaticamente il fallback per l'origine dell'ora specificata per il **NtpServer** impostazione. Questo metodo di sincronizzazione è più probabile fornire l'ora esatta ai client.  

### <a name="stopping-time-synchronization"></a>Sincronizzazione dell'ora di arresto  
Vi sono alcune situazioni in cui verrà desidera impedire al computer il tempo di sincronizzazione. Ad esempio, se un computer tenta di sincronizzare da un'origine ora su Internet o da un altro sito in una rete WAN tramite una connessione remota, può comportare costosi telefonica. Quando si disabilita la sincronizzazione in tale computer, si impedisce che il computer sta tentando di accedere a un'origine ora su una connessione remota.  
  
È anche possibile disabilitare la sincronizzazione per evitare la generazione di errori nel registro eventi. Ogni volta che un computer tenta di sincronizzare con un'origine ora è disponibile, viene generato un errore nel registro eventi. Se un'origine ora viene eseguita all'esterno della rete per la manutenzione pianificata e non si prevede di riconfigurare il client per la sincronizzazione da un'altra origine, è possibile disabilitare la sincronizzazione sul client per impedire che il tentativo di sincronizzazione, mentre il server non è disponibile.  
  
È utile disabilitare la sincronizzazione nel computer in cui viene definita come la radice della rete di sincronizzazione. Ciò indica che il computer principale attendibile l'orologio locale. Se la radice della gerarchia di sincronizzazione non è impostata su **NoSync** e se non è possibile eseguire la sincronizzazione con un'altra origine ora, i client non accettano il pacchetto che invia questo computer perché l'ora non è attendibile.  
  
L'unica volta server sono considerati attendibili dai client, anche se non sono state sincronizzate con un'altra origine ora sono quelli che sono stati identificati dal client come server di riferimento orario affidabile.  
  
### <a name="disabling-the-windows-time-service"></a>Disabilitare il servizio ora di Windows  
Il servizio ora di Windows (W32Time) può essere completamente disattivato. Se si sceglie di implementare un prodotto di sincronizzazione ora di terze parti che utilizza NTP, è necessario disabilitare il servizio ora di Windows. Questo avviene perché tutti i server NTP richiedono l'accesso alla porta di protocollo UDP (User Datagram) 123 e fino a quando il servizio ora di Windows è in esecuzione nel sistema operativo Windows Server 2003, la porta 123 rimane riservata ora di Windows.  
  
## <a name="w2k3tr_times_how_ydum"></a>Porte di rete utilizzata dal servizio ora di Windows  
Il servizio ora di Windows comunica in una rete per identificare le origini di ora affidabile, ottenere informazioni sull'ora e fornire le informazioni ad altri computer. Questa comunicazione viene eseguita come definito dal NTP e SNTP RFC.  
  
**Assegnazioni delle porte per il servizio ora di Windows**  
  
|Nome del servizio|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/D|  
|SNTP|123|N/D|  
  
## <a name="see-also"></a>Vedere anche  
[Documentazione tecnica su Windows ora servizio](https://technet.microsoft.com/library/cc773061.aspx)  
[Impostazioni e strumenti servizio ora di Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Articolo della Microsoft Knowledge Base 902229](https://go.microsoft.com/fwlink/?LinkId=186066)  
  


