---
title: Servizio Copia Shadow del volume
ms.date: 01/30/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 1ab941e25da7171349bb24762940af3bf886c165
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "77675362"
---
# <a name="volume-shadow-copy-service"></a>Servizio Copia Shadow del volume

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

Il backup e il ripristino dei dati aziendali critici possono essere molto complessi a causa dei problemi seguenti:

  - È in genere necessario eseguire il backup dei dati mentre le applicazioni che li producono sono ancora in esecuzione. Ciò significa che alcuni file di dati potrebbero essere aperti o in uno stato non coerente.

  - Se il set di dati è di grandi dimensioni, può essere difficile eseguirne il backup in una sola volta.


Per eseguire correttamente le operazioni di backup e ripristino, è necessario un attento coordinamento tra le applicazioni di backup, le applicazioni line-of-business di cui viene eseguito il backup e l'hardware e il software di gestione dell'archiviazione. Il Servizio Copia Shadow del volume (VSS), introdotto in Windows Server® 2003, semplifica la conversazione tra questi componenti per consentire loro di operare meglio insieme. Quando tutti i componenti supportano VSS, puoi usarli per eseguire il backup dei dati delle applicazioni senza portarle offline.

VSS coordina le azioni necessarie per creare una copia shadow coerente (nota anche come snapshot o copia temporizzata) dei dati di cui deve essere eseguito il backup. La copia shadow può essere usata così com'è oppure può essere usata in scenari come i seguenti:

  - Vuoi eseguire il backup dei dati delle applicazioni e delle informazioni sullo stato del sistema, inclusa l'archiviazione dei dati in un'altra unità disco rigido, su nastro o su un altro supporto rimovibile.

  - Stai eseguendo il data mining.

  - Stai eseguendo backup da disco a disco.

  - Devi eseguire un ripristino rapido dalla perdita di dati ripristinando i dati nel LUN originale o in un LUN completamente nuovo che sostituisce un LUN originale che presentava problemi.


Le funzionalità e le applicazioni Windows che usano VSS includono quanto segue:

  - [Windows Server Backup](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkId=180891)

  - [Copie shadow di cartelle condivise](https://go.microsoft.com/fwlink/?linkid=142874) (https://go.microsoft.com/fwlink/?LinkId=142874)

  - [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=180892) (https://go.microsoft.com/fwlink/?LinkId=180892)

  - [Ripristino configurazione di sistema](https://go.microsoft.com/fwlink/?linkid=180893) (https://go.microsoft.com/fwlink/?LinkId=180893)


## <a name="how-volume-shadow-copy-service-works"></a>Funzionamento del Servizio Copia Shadow del volume (VSS)

Una soluzione VSS completa richiede tutti i componenti di base seguenti:

**Servizio VSS**   Parte del sistema operativo Windows che garantisce che gli altri componenti possano comunicare tra loro correttamente e interagire.

**Richiedente VSS**   Software che richiede l'effettiva creazione di copie shadow (o altre operazioni di alto livello come l'importazione o l'eliminazione di tali copie). In genere, si tratta dell'applicazione di backup. L'utilità Windows Server Backup e l'applicazione System Center Data Protection Manager sono richiedenti VSS. I richiedenti VSS non Microsoft® includono quasi tutti i software di backup eseguiti in Windows.

**VSS writer**   Componente che garantisce la presenza di un set di dati coerente per il backup. Viene in genere fornito come parte di un'applicazione line-of-business, ad esempio SQL Server® o Exchange Server. Nel sistema operativo Windows sono inclusi VSS writer per diversi componenti di Windows, ad esempio il Registro di sistema. VSS writer non Microsoft sono inclusi in molte applicazioni per Windows che devono garantire la coerenza dei dati durante il backup.

**Provider VSS**   Componente che crea e gestisce le copie shadow. Può essere a livello software o hardware. Il sistema operativo Windows include un provider VSS che usa la copia su scrittura. Se usi una rete di archiviazione (SAN), è importante installare il provider hardware VSS per tale rete, se disponibile. Un provider hardware si fa carico dell'attività di creazione e gestione di una copia shadow eseguita dal sistema operativo host.

Il diagramma seguente illustra il modo in cui il servizio VSS si coordina con richiedenti, writer e provider per creare una copia shadow di un volume.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figura 1**   Diagramma dell'architettura del Servizio Copia Shadow del volume

### <a name="how-a-shadow-copy-is-created"></a>Modalità di creazione di una copia shadow

Questa sezione colloca nel contesto i vari ruoli del richiedente, del writer e del provider elencando i passaggi che devono essere eseguiti per creare una copia shadow. Il diagramma seguente illustra il modo in cui il Servizio Copia Shadow del volume controlla il coordinamento generale del richiedente, del writer e del provider.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figura 2** Processo di creazione di una copia shadow

Per creare una copia shadow, il richiedente, il writer e il provider eseguono le azioni seguenti:

1.  Il richiedente chiede al Servizio Copia Shadow del volume di enumerare i writer, raccogliere i metadati dei writer e prepararsi per la creazione della copia shadow.

2.  Ogni writer crea una descrizione XML dei componenti e degli archivi dati di cui è necessario eseguire il backup e li fornisce al Servizio Copia Shadow del volume. Il writer definisce anche un metodo di ripristino, che viene usato per tutti i componenti. Il Servizio Copia Shadow del volume fornisce la descrizione del writer al richiedente, che seleziona i componenti di cui verrà eseguito il backup.

3.  Il Servizio Copia Shadow del volume notifica a tutti i writer di preparare i rispettivi dati per la creazione di una copia shadow.

4.  Ogni writer prepara i dati secondo le esigenze, ad esempio completando tutte le transazioni aperte, eseguendo il rollup dei registri delle transazioni e scaricando le cache. Quando i dati sono pronti per la copia shadow, il writer invia una notifica al Servizio Copia Shadow del volume.

5.  Il Servizio Copia Shadow del volume indica ai writer di bloccare temporaneamente le richieste di I/O di scrittura nelle applicazioni (le richieste di I/O di lettura sono ancora possibili) per i pochi secondi necessari per creare la copia shadow di uno o più volumi. Il blocco delle applicazioni non può richiedere più di 60 secondi. Il Servizio Copia Shadow del volume svuota i buffer del file system e quindi blocca il file system, operazione che garantisce che i relativi metadati vengano registrati correttamente e che i dati per cui creare la copia shadow siano scritti secondo un ordine coerente.

6.  Il Servizio Copia Shadow del volume indica al provider di creare la copia shadow. Il periodo di creazione della copia shadow non dura più di 10 secondi, durante i quali tutte le richieste di I/O di scrittura nel file system rimangono bloccate.

7.  Il Servizio Copia Shadow del volume rilascia le richieste di I/O di scrittura nel file system.

8.  VSS indica ai writer di sbloccare le richieste di I/O di scrittura delle applicazioni. A questo punto, le applicazioni possono riprendere la scrittura dei dati sul disco di cui viene eseguita la copia shadow.


> [!NOTE]
> La creazione della copia shadow può essere interrotta se i writer vengono mantenuti nello stato di blocco per più di 60 secondi o se i provider impiegano più di 10 secondi per eseguire il commit della copia shadow.
<br>

9. Il richiedente può ritentare il processo (torna al passaggio 1) o inviare una notifica all'amministratore per riprovare in un secondo momento.

10. Se la copia shadow viene creata correttamente, il Servizio Copia Shadow del volume restituisce al richiedente le informazioni sulla posizione della copia shadow. In alcuni casi, la copia shadow può essere resa temporaneamente disponibile come volume di lettura/scrittura, in modo che VSS e una o più applicazioni possano modificarne il contenuto prima che la copia shadow venga completata. Dopo che VSS e le applicazioni apportano le rispettive modifiche, la copia shadow viene impostata per la sola lettura. Questa fase è denominata recupero automatico e viene usata per annullare le transazioni del file system o delle applicazioni nel volume della copia shadow che non sono state completate prima della creazione della copia shadow.


### <a name="how-the-provider-creates-a-shadow-copy"></a>Modalità di creazione di una copia shadow da parte del provider

Un provider di copie shadow hardware o software usa uno dei metodi seguenti per la creazione di una copia shadow:

**Copia completa**   Questo metodo esegue una copia completa (denominata "copia totale" o "clone") del volume originale in un determinato momento. Questa copia è di sola lettura.

**Copia su scrittura**   Questo metodo non copia il volume originale. Esegue invece una copia differenziale copiando tutte le modifiche (richieste di I/O di scrittura completate) che vengono effettuate sul volume dopo un determinato momento.

**Reindirizzamento su scrittura**   Questo metodo non copia il volume originale e non vi apporta alcuna modifica dopo un determinato momento. Esegue invece una copia differenziale reindirizzando tutte le modifiche a un volume diverso.

## <a name="complete-copy"></a>Copia completa

Una copia completa in genere viene creata generando una copia "mirror suddivisa" come indicato di seguito:

1. Il volume originale e il volume della copia shadow sono un set di volumi con mirroring.

2. Il volume della copia shadow è separato dal volume originale. Ciò interrompe la connessione mirror.


Dopo l'interruzione della connessione mirror, il volume originale e il volume della copia shadow sono indipendenti. Il volume originale continua ad accettare tutte le modifiche (richieste di I/O di scrittura), mentre il volume della copia shadow rimane un'esatta copia di sola lettura dei dati originali al momento dell'interruzione.

### <a name="copy-on-write-method"></a>Metodo copia su scrittura

Nel metodo copia su scrittura, quando si verifica una modifica relativa al volume originale (ma prima del completamento della richiesta di I/O di scrittura), ogni blocco da modificare viene letto e quindi scritto nell'area di archiviazione della copia shadow del volume (detta anche "area diff"). L'area di archiviazione della copia shadow può trovarsi nello stesso volume o in un volume diverso. In questo modo, viene conservata una copia del blocco di dati nel volume originale prima che la modifica lo sovrascriva.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Tempo</th>
<th>Dati di origine (stato e dati)</th>
<th>Copia shadow (stato e dati)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dati originali: 1 2 3 4 5</p></td>
<td><p>Nessuna copia: —</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dati modificati nella cache: da 3 a 3'</p></td>
<td><p>Copia shadow creata (solo le differenze): 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dati originali sovrascritti: 1 2 3' 4 5</p></td>
<td><p>Differenze e indice archiviati nella copia shadow: 3</p></td>
</tr>
</tbody>
</table>

**Tabella 1**   Metodo copia su scrittura per la creazione di copie shadow

Il metodo copia su scrittura è un metodo rapido per creare una copia shadow perché copia solo i dati modificati. I blocchi copiati nell'area diff possono essere combinati con i dati modificati nel volume originale per ripristinare lo stato in cui era il volume prima che venissero apportate le modifiche. Se sono presenti molte modifiche, il metodo copia su scrittura può diventare oneroso.

### <a name="redirect-on-write-method"></a>Metodo reindirizzamento su scrittura

Nel metodo reindirizzamento su scrittura, ogni volta che il volume originale riceve una modifica (richiesta di I/O di scrittura), la modifica non viene applicata al volume originale. La modifica viene invece scritta nell'area di archiviazione della copia shadow di un altro volume.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Tempo</th>
<th>Dati di origine (stato e dati)</th>
<th>Copia shadow (stato e dati)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dati originali: 1 2 3 4 5</p></td>
<td><p>Nessuna copia: —</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dati modificati nella cache: da 3 a 3'</p></td>
<td><p>Copia shadow creata (solo le differenze): 3'</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dati originali non modificati: 1 2 3 4 5</p></td>
<td><p>Differenze e indice archiviati nella copia shadow: 3'</p></td>
</tr>
</tbody>
</table>

**Tabella 2**   Metodo reindirizzamento su scrittura per la creazione di copie shadow

Come il metodo copia su scrittura, il reindirizzamento su scrittura è un metodo rapido per creare una copia shadow perché copia solo le modifiche relative ai dati. I blocchi copiati nell'area diff possono essere combinati con i dati non modificati nel volume originale per creare una copia completa e aggiornata dei dati. Se sono presenti molte richieste di I/O di lettura, il metodo reindirizzamento su scrittura può diventare oneroso.

## <a name="shadow-copy-providers"></a>Provider di copie shadow

Sono disponibili due tipi di provider di copie shadow: provider basati su hardware e provider basati su software. È disponibile anche un provider di sistema, ovvero un provider software integrato nel sistema operativo Windows.

### <a name="hardware-based-providers"></a>Provider basati su hardware

I provider di copie shadow basati su hardware fungono da interfaccia tra il Servizio Copia Shadow del volume e il livello hardware operando insieme a un controller o a un adattatore di archiviazione hardware. L'attività di creazione e gestione della copia shadow viene eseguita dall'array di archiviazione.

I provider hardware accettano sempre la copia shadow di un intero LUN, ma il Servizio Copia Shadow del volume espone solo la copia shadow del volume o dei volumi richiesti.

Un provider di copie shadow basato su hardware usa la funzionalità Servizio Copia Shadow del volume che definisce il momento, consente la sincronizzazione dei dati, gestisce la copia shadow e fornisce un'interfaccia comune con le applicazioni di backup. Tuttavia, il Servizio Copia Shadow del volume non specifica il meccanismo sottostante in base al quale il provider basato su hardware produce e gestisce le copie shadow.

### <a name="software-based-providers"></a>Provider basati su software

I provider di copie shadow basati su software in genere intercettano ed elaborano le richieste di I/O di lettura e scrittura in un livello software tra il file system e il software di gestione dei volumi.

Questi provider vengono implementati come un componente DLL in modalità utente e almeno un driver di dispositivo in modalità kernel, di solito un driver del filtro di archiviazione. A differenza dei provider basati su hardware, i provider basati su software creano copie shadow a livello software, non a livello hardware.

Un provider di copie shadow basato su software deve mantenere una visualizzazione "temporizzata" di un volume avendo accesso a un set di dati utilizzabile per ricreare lo stato del volume prima dell'ora di creazione della copia shadow. Un esempio è rappresentato dalla tecnica di copia su scrittura del provider di sistema. Il Servizio Copia Shadow del volume però non impone alcuna restrizione sulla tecnica usata dai provider basati su software per creare e gestire le copie shadow.

Un provider software è applicabile a una gamma più ampia di piattaforme di archiviazione rispetto a un provider basato su hardware e dovrebbe funzionare anche con dischi di base o volumi logici. Un volume logico è un volume creato combinando lo spazio libero di due o più dischi. Diversamente da quanto accade per le copie shadow hardware, i provider software utilizzano le risorse del sistema operativo per gestire la copia shadow.

Per altre informazioni sui dischi di base, vedi [Che cosa sono i dischi di base e i volumi?](https://go.microsoft.com/fwlink/?linkid=180894) (https://go.microsoft.com/fwlink/?LinkId=180894) in TechNet.

### <a name="system-provider"></a>Provider di sistema

Un provider di copie shadow, il provider di sistema, viene fornito nel sistema operativo Windows. Sebbene venga fornito un provider predefinito in Windows, altri fornitori sono liberi di offrire implementazioni ottimizzate per le relative applicazioni hardware e software di archiviazione.

Per mantenere la visualizzazione "temporizzata" di un volume contenuto in una copia shadow, il provider di sistema usa una tecnica di copia su scrittura. Copie dei blocchi del volume modificate dopo l'inizio della creazione della copia shadow vengono archiviate in un'area di archiviazione della copia shadow.

Il provider di sistema può esporre il volume di produzione, che può essere scritto e letto normalmente. Quando è necessaria la copia shadow, applica logicamente le differenze ai dati nel volume di produzione per esporre la copia shadow completa.

Per il provider di sistema, l'area di archiviazione della copia shadow deve trovarsi in un volume NTFS. Non è necessario che il volume per cui eseguire la copia shadow sia un volume NTFS, ma almeno un volume montato nel sistema deve essere un volume NTFS.

I file dei componenti che costituiscono il provider di sistema sono swprv.dll e volsnap.sys.

### <a name="in-box-vss-writers"></a>VSS writer inclusi

Il sistema operativo Windows include un set di VSS writer responsabili dell'enumerazione dei dati necessari alle varie funzionalità Windows.

Per altre informazioni su questi writer, visita la pagina Web seguente di Microsoft Docs:

- [VSS writer inclusi](https://docs.microsoft.com/windows/win32/vss/in-box-vss-writers) (https://docs.microsoft.com/windows/win32/vss/in-box-vss-writers)


## <a name="how-shadow-copies-are-used"></a>Modalità d'uso delle copie shadow

Oltre che per eseguire il backup dei dati delle applicazioni e delle informazioni sullo stato del sistema, le copie shadow possono essere usate per diversi scopi, inclusi i seguenti:

  - Ripristino dei LUN (risincronizzazione LUN e scambio LUN)

  - Ripristino dei singoli file (Copie shadow di cartelle condivise)

  - Data mining tramite copie shadow trasportabili


### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Ripristino dei LUN (risincronizzazione LUN e scambio LUN)

In Windows Server 2008 R2 e Windows 7 i richiedenti VSS possono usare una funzionalità del provider di copie shadow hardware denominata "risincronizzazione LUN". Si tratta di uno schema di ripristino rapido che consente a un amministratore dell'applicazione di ripristinare i dati da una copia shadow nel LUN originale o in un nuovo LUN.

La copia shadow può essere un clone completo oppure una copia shadow differenziale. In entrambi i casi, al termine dell'operazione di risincronizzazione, il LUN di destinazione avrà lo stesso contenuto del LUN della copia shadow. Durante l'operazione di risincronizzazione, l'array esegue una copia a livello di blocco dalla copia shadow al LUN di destinazione.


> [!NOTE]
> La copia shadow deve essere una copia shadow hardware trasportabile.
<br>


La maggior parte degli array consente la ripresa delle operazioni di I/O di produzione poco dopo l'inizio dell'operazione di risincronizzazione. Mentre tale operazione è in corso, le richieste di lettura vengono reindirizzate al LUN della copia shadow e le richieste di scrittura al LUN di destinazione. In questo modo, gli array possono ripristinare set di dati di grandi dimensioni e riprendere le normali operazioni nel giro di pochi secondi.

La risincronizzazione LUN è diversa dallo scambio LUN. Uno scambio LUN è uno scenario di ripristino rapido supportato da VSS a partire da Windows Server 2003 SP1. In uno scambio LUN la copia shadow viene importata e quindi convertita in un volume di lettura/scrittura. La conversione è un'operazione irreversibile, dopo la quale il volume e il LUN sottostante non possono essere controllati con le API VSS. Nell'elenco seguente vengono descritte le differenze tra la risincronizzazione LUN e lo scambio LUN:

  - Nella risincronizzazione LUN la copia shadow non viene modificata, quindi può essere usata diverse volte. Nello scambio LUN la copia shadow può essere usata una sola volta per un ripristino. Questo è importante per gli amministratori che hanno la sicurezza come massima priorità. Quando viene usata la risincronizzazione LUN, il richiedente può ritentare l'intera operazione di ripristino se si verifica un problema la prima volta.

  - Alla fine di uno scambio LUN, il LUN della copia shadow viene usato per le richieste di I/O di produzione. Per questo motivo, il LUN della copia shadow deve usare la stessa qualità di archiviazione del LUN di produzione originale per garantire che non ci siano ripercussioni negative sulle prestazioni dopo l'operazione di ripristino. Se invece viene usata la risincronizzazione LUN, il provider hardware può mantenere la copia shadow in una risorsa di archiviazione meno costosa rispetto all'archiviazione con qualità di produzione.

  - Se il LUN di destinazione non è utilizzabile ed è necessario ricrearlo, lo scambio LUN potrebbe essere più economico perché non richiede un LUN di destinazione.


> [!WARNING]
> Tutte le operazioni elencate sono operazioni a livello di LUN. Se tenti di ripristinare un volume specifico tramite la risincronizzazione LUN, involontariamente ripristinerai tutti gli altri volumi che condividono il LUN.
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Ripristino dei singoli file (Copie shadow di cartelle condivise)

La funzionalità Copie shadow di cartelle condivise usa il Servizio Copia Shadow del volume per fornire copie temporizzate dei file che si trovano in una risorsa di rete condivisa, ad esempio un file server. Con Copie shadow di cartelle condivise, gli utenti possono ripristinare rapidamente i file eliminati o modificati archiviati in rete. Poiché questa operazione può essere eseguita senza l'assistenza dell'amministratore, Copie shadow di cartelle condivise consente di aumentare la produttività e ridurre i costi amministrativi.

Per altre informazioni su questa funzionalità, vedi [Copie shadow di cartelle condivise](https://go.microsoft.com/fwlink/?linkid=180898) (https://go.microsoft.com/fwlink/?LinkId=180898) in TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Data mining tramite copie shadow trasportabili

Con un provider hardware progettato per l'uso con il Servizio Copia Shadow del volume, puoi creare copie shadow trasportabili che possono essere importate nei server all'interno dello stesso sottosistema, ad esempio una rete di archiviazione. Queste copie shadow possono essere usate per eseguire il seeding di un'installazione di produzione o di test con dati di sola lettura per il data mining.

Tramite il Servizio Copia Shadow del volume e un array di archiviazione con un provider hardware progettato per l'uso con tale servizio, è possibile creare una copia shadow del volume dei dati di origine in un server e quindi importare la copia shadow in un altro server (o di nuovo nello stesso server). Questo processo viene eseguito in pochi minuti, indipendentemente dalla dimensione dei dati. Il processo di trasporto viene eseguito tramite una serie di passaggi che usano un richiedente di copie shadow (un'applicazione di gestione dell'archiviazione) che supporta le copie shadow trasportabili.

## <a name="to-transport-a-shadow-copy"></a>Per trasportare una copia shadow

1.  Crea una copia shadow trasportabile dei dati di origine in un server.

2.  Importa la copia shadow in un server connesso alla rete di archiviazione (puoi eseguire l'importazione in un server diverso o nello stesso server).

3.  I dati ora sono pronti per essere usati.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figura 3**   Creazione di copie shadow e trasporto tra due server


> [!NOTE]
> Una copia shadow trasportabile creata in Windows Server 2003 non può essere importata in un server che esegue Windows Server 2008 o Windows Server 2008 R2. Una copia shadow trasportabile creata in Windows Server 2008 o Windows Server 2008 R2 non può essere importata in un server che esegue Windows Server 2003. Tuttavia, una copia shadow creata in Windows Server 2008 non può essere importata in un server che esegue Windows Server 2008 R2 e viceversa.
<br>


Le copie shadow sono di sola lettura. Se vuoi convertire una copia shadow in un LUN di lettura/scrittura, puoi usare un'applicazione di gestione dell'archiviazione basata su Servizio dischi virtuali (inclusi alcuni richiedenti) oltre al Servizio Copia Shadow del volume. Usando questa applicazione, puoi rimuovere la copia shadow dalla gestione del Servizio Copia Shadow del volume e convertirla in un LUN di lettura/scrittura.

Il trasporto del Servizio Copia Shadow del volume è una soluzione avanzata sui computer che eseguono Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 o Windows Server 2008 R2. Funziona solo se è presente un provider hardware nell'array di archiviazione. Il trasporto delle copie shadow può essere usato per diversi scopi, tra cui i backup su nastro, il data mining e i test.

## <a name="frequently-asked-questions"></a>Domande frequenti

Queste domande frequenti rispondono alle domande sul Servizio Copia Shadow del volume (VSS) per gli amministratori di sistema. Per informazioni sulle interfacce di programmazione di applicazioni VSS, vedi [Servizio Copia Shadow del volume](https://go.microsoft.com/fwlink/?linkid=180899) (https://go.microsoft.com/fwlink/?LinkId=180899) nella raccolta di documentazione del Centro per sviluppatori Windows.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>Quando è stato introdotto il Servizio Copia Shadow del volume? In quali versioni del sistema operativo Windows è disponibile?

Il Servizio Copia Shadow del volume (VSS) è stato introdotto in Windows XP. È disponibile in Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 e Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>Quale è la differenza tra una copia shadow e un backup?

Nel caso di un backup di un'unità disco rigido, la copia shadow creata è anche il backup. I dati possono essere copiati dalla copia shadow per un ripristino oppure la copia shadow può essere usata per uno scenario di ripristino rapido, ad esempio la risincronizzazione LUN o lo scambio LUN.

Quando i dati vengono copiati dalla copia shadow su nastro o altri supporti rimovibili, il contenuto archiviato nel supporto costituisce il backup. La stessa copia shadow può essere eliminata dopo la copia dei dati in essa contenuti.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>Qual è il volume di dimensioni maggiori supportato dal Servizio Copia Shadow del volume?

Il Servizio Copia Shadow del volume supporta una dimensione del volume fino a 64 TB.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>Ho eseguito un backup in Windows Server 2008. Posso ripristinarlo in Windows Server 2008 R2?

Dipende dal software di backup usato. La maggior parte dei programmi di backup supporta questo scenario per i dati, ma non per i backup dello stato del sistema.

Le copie shadow create in una di queste versioni di Windows possono essere usate nell'altra.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>Ho eseguito un backup in Windows Server 2003. Posso ripristinarlo in Windows Server 2008?

Dipende dal software di backup usato. Se crei una copia shadow in Windows Server 2003, non puoi usarla in Windows Server 2008. Inoltre, se crei una copia shadow in Windows Server 2008, non puoi ripristinarla in Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>Come posso disabilitare il Servizio Copia Shadow del volume?

È possibile disabilitare il Servizio Copia Shadow del volume usando Microsoft Management Console. Non è tuttavia consigliabile eseguire questa operazione. La disabilitazione del Servizio Copia Shadow del volume influisce sui software usati che dipendono da esso, ad esempio Ripristino configurazione di sistema e Windows Server Backup.

Per altre informazioni, visita i siti Web Microsoft TechNet seguenti:

- [Ripristino configurazione di sistema](https://go.microsoft.com/fwlink/?linkid=157113) (https://go.microsoft.com/fwlink/?LinkID=157113)

- [Windows Server Backup](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkID=180891)


### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>Posso escludere alcuni file da una copia shadow per risparmiare spazio?

Il Servizio Copia Shadow del volume è progettato per creare copie shadow di interi volumi. I file temporanei, ad esempio i file di paging, vengono omessi automaticamente dalle copie shadow per risparmiare spazio.

Per escludere file specifici dalle copie shadow, usa la chiave del Registro di sistema seguente: **FilesNotToSnapshot**.


> [!NOTE]
> La chiave del Registro di sistema <STRONG>FilesNotToSnapshot</STRONG> deve essere usata solo dalle applicazioni. Gli utenti che tentano di usarla incontreranno limitazioni come le seguenti:
> <br>
> <UL>
> <LI>Non è possibile eliminare file da una copia shadow creata in un server Windows usando la funzionalità Versioni precedenti.<BR><BR>
> <LI>Non è possibile eliminare file dalle copie shadow per cartelle condivise.<BR><BR>
> <LI>È possibile eliminare file da una copia shadow creata usando l'utilità <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">Diskshadow</a>, ma non è possibile eliminare file da una copia shadow creata usando l'utilità <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">Vssadmin</a>.<BR><BR>
> <LI>I file vengono eliminati da una copia shadow in base al massimo sforzo. Ciò significa che non è garantito che vengano eliminati.<BR><BR></LI></UL>


Per altre informazioni, vedi [Esclusione di file da copie shadow](https://go.microsoft.com/fwlink/?linkid=180904) (https://go.microsoft.com/fwlink/?LinkId=180904) in MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Il programma di backup non Microsoft che ho usato ha dato esito negativo e ha restituito un errore VSS. Come posso procedere?

Consulta la sezione di supporto tecnico del sito Web dell'azienda che ha creato il programma di backup. Potrebbe essere disponibile un aggiornamento del prodotto scaricabile e installabile per risolvere il problema. In caso contrario, contatta il reparto addetto al supporto tecnico dell'azienda.

Gli amministratori di sistema possono usare le informazioni per la risoluzione dei problemi del Servizio Copia Shadow del volume nel sito Web seguente della raccolta Microsoft TechNet per raccogliere informazioni diagnostiche sui problemi correlati a tale servizio.

Per altre informazioni, vedi [Servizio Copia Shadow del volume](https://go.microsoft.com/fwlink/?linkid=180905) (https://go.microsoft.com/fwlink/?LinkId=180905) in TechNet.

### <a name="what-is-the-diff-area"></a>Che cos'è l'"area diff"?

L'area di archiviazione della copia shadow (o "area diff") è la posizione in cui vengono archiviati i dati per la copia shadow creata dal provider software di sistema.

### <a name="where-is-the-diff-area-located"></a>Dove si trova l'area diff?

L'area diff può trovarsi in qualsiasi volume locale. Deve tuttavia trovarsi in un volume NTFS con spazio sufficiente per archiviarla.

### <a name="how-is-the-diff-area-location-determined"></a>In che modo viene determinata la posizione dell'area diff?

Per determinare la posizione dell'area diff, vengono valutati i criteri seguenti, in questo ordine:

  - Se un volume dispone già di una copia shadow esistente, viene usata tale posizione.

  - Se è presente un'associazione manuale preconfigurata tra il volume originale e la posizione del volume della copia shadow, viene usata tale posizione.

  - Se i due criteri precedenti non forniscono una posizione, il servizio Copia Shadow sceglie una posizione in base allo spazio libero disponibile. Se viene eseguita la copia shadow di più di un volume, il servizio Copia Shadow crea un elenco di possibili posizioni per lo snapshot in base alla dimensione dello spazio disponibile, in ordine decrescente. Il numero di posizioni fornito è uguale al numero di volumi di cui viene eseguita la copia shadow.

  - Se il volume di cui viene eseguita la copia shadow è una delle posizioni possibili, viene creata un'associazione locale. In caso contrario, viene creata un'associazione con il volume con la maggior quantità di spazio disponibile.


### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>Il Servizio Copia Shadow del volume può creare copie shadow di volumi non NTFS?

Sì. Tuttavia, possono essere effettuate copie shadow permanenti solo per i volumi NTFS. Inoltre, almeno un volume montato nel sistema deve essere un volume NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>Qual è il numero massimo di copie shadow che posso creare contemporaneamente?

Il numero massimo di volumi che è possibile includere in una singola copia shadow è 64. Tieni presente che questo numero non corrisponde al numero di copie shadow.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>Qual è il numero massimo di copie shadow software create dal provider di sistema che posso gestire per un volume?

Il numero massimo di copie shadow software per ogni volume è 512. Tuttavia, per impostazione predefinita, puoi gestire solo 64 copie shadow usate dalla funzionalità Copie shadow di cartelle condivise. Per modificare il limite per la funzionalità Copie shadow di cartelle condivise, usa la chiave del Registro di sistema seguente: **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>In che modo posso controllare lo spazio usato per lo spazio di archiviazione delle copie shadow?

Digita il comando **vssadmin resize shadowstorage**.

Per altre informazioni, vedi [Vssadmin resize shadowstorage](https://go.microsoft.com/fwlink/?linkid=180906) (https://go.microsoft.com/fwlink/?LinkId=180906) in TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>Che cosa accade quando si esaurisce lo spazio?

Le copie shadow per il volume vengono eliminate, iniziando dalla copia shadow meno recente.

## <a name="volume-shadow-copy-service-tools"></a>Strumenti per il Servizio Copia Shadow del volume

Il sistema operativo Windows offre gli strumenti seguenti per l'interazione con il Servizio Copia Shadow del volume (VSS):

  - [DiskShadow](https://go.microsoft.com/fwlink/?linkid=180907) (https://go.microsoft.com/fwlink/?LinkId=180907)

  - [VssAdmin](https://go.microsoft.com/fwlink/?linkid=84008) (https://go.microsoft.com/fwlink/?LinkId=84008)


### <a name="diskshadow"></a>DiskShadow

DiskShadow è un richiedente VSS che consente di gestire tutti gli snapshot hardware e software che puoi avere in un sistema. DiskShadow include comandi come i seguenti:

  - **list**: elenca i VSS writer, i provider VSS e le copie shadow

  - **create**: crea una nuova copia shadow

  - **import**: importa una copia shadow trasportabile

  - **expose**: espone una copia shadow permanente, ad esempio come una lettera di unità

  - **revert**: ripristina un volume in una copia shadow specificata


Questo strumento è destinato ai professionisti IT, ma può risultare utile anche per gli sviluppatori quando testano un VSS writer o un provider VSS.

DiskShadow è disponibile solo nei sistemi operativi Windows Server. Non è disponibile nei sistemi operativi client Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin consente di creare, eliminare ed elencare le informazioni sulle copie shadow. Consente inoltre di ridimensionare l'area di archiviazione della copia shadow ("area diff").

VssAdmin include comandi come i seguenti:

  - **create shadow**: crea una nuova copia shadow

  - **delete shadows**: elimina le copie shadow

  - **list providers**: elenca tutti i provider VSS registrati

  - **list writers**: elenca tutti i VSS writer sottoscritti

  - **resize shadowstorage**: modifica le dimensioni massime dell'area di archiviazione della copia shadow


VssAdmin può essere usato solo per amministrare le copie shadow create dal provider software di sistema.

VssAdmin è disponibile nelle versioni del sistema operativo client Windows e Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Chiavi del Registro di sistema per il Servizio Copia Shadow del volume

Le chiavi del Registro di sistema seguenti sono disponibili per l'uso con il Servizio Copia Shadow del volume (VSS):

  - **VssAccessControl**

  - **MaxShadowCopies**

  - **MinDiffAreaFileSize**


### <a name="vssaccesscontrol"></a>VssAccessControl

Questa chiave viene usata per specificare gli utenti che hanno accesso alle copie shadow.

Per altre informazioni, vedere le voci seguenti nel sito Web MSDN:

  - [Considerazioni sulla sicurezza per i writer](https://go.microsoft.com/fwlink/?linkid=157739) (https://go.microsoft.com/fwlink/?LinkId=157739)

  - [Considerazioni sulla sicurezza per i richiedenti](https://go.microsoft.com/fwlink/?linkid=180908) (https://go.microsoft.com/fwlink/?LinkId=180908)


### <a name="maxshadowcopies"></a>MaxShadowCopies

Questa chiave specifica il numero massimo di copie shadow accessibili dal client che possono essere archiviate in ogni volume del computer. Le copie shadow accessibili dal client vengono usate da Copie shadow di cartelle condivise.

Per altre informazioni, vedi la voce seguente nel sito Web MSDN:

**MaxShadowCopies** in [Chiavi del Registro di sistema per Backup e ripristino](https://go.microsoft.com/fwlink/?linkid=180909) (https://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Questa chiave specifica le dimensioni minime iniziali, in MB, dell'area di archiviazione della copia shadow.

Per altre informazioni, vedi la voce seguente nel sito Web MSDN:

**MinDiffAreaFileSize** in [Chiavi del Registro di sistema per Backup e ripristino](https://go.microsoft.com/fwlink/?linkid=180910) (https://go.microsoft.com/fwlink/?LinkId=180910)

### <a name="supported-operating-system-versions"></a>Versioni del sistema operativo supportate

La tabella seguente elenca le versioni minime del sistema operativo supportate per le funzionalità VSS.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Funzionalità VSS</th>
<th>Client minimo supportato</th>
<th>Server minimo supportato</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Risincronizzazione LUN</p></td>
<td><p>Nessuno supportato</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p>Chiave del Registro di sistema <strong>FilesNotToSnapshot</strong></p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="odd">
<td><p>Copie shadow trasportabili</p></td>
<td><p>Nessuno supportato</p></td>
<td><p>Windows Server 2003 con SP1</p></td>
</tr>
<tr class="even">
<td><p>Copie shadow hardware</p></td>
<td><p>Nessuno supportato</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Versioni precedenti di Windows Server</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>Ripristino rapido tramite lo scambio LUN</p></td>
<td><p>Nessuno supportato</p></td>
<td><p>Windows Server 2003 con SP1</p></td>
</tr>
<tr class="odd">
<td><p>Più importazioni di copie shadow hardware</p>
<div class="alert">
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><img src="media/volume-shadow-copy-service/Dd560667.note(WS.10).gif" />Nota</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Si tratta della possibilità di importare una copia shadow più di una volta. È possibile eseguire una sola operazione di importazione alla volta.
<p></p></td>
</tr>
</tbody>
</table>
<p></p>
</div></td>
<td><p>Nessuno supportato</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Copie shadow di cartelle condivise</p></td>
<td><p>Nessuno supportato</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Copie shadow ripristinate automaticamente trasportabili</p></td>
<td><p>Nessuno supportato</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sessioni di backup simultanee (fino a 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Singola sessione di ripristino simultanea ai backup</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 con SP2</p></td>
</tr>
<tr class="even">
<td><p>Fino a 8 sessioni di ripristino simultanee ai backup</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Vedi anche

[Servizio Copia Shadow del volume nel Centro per sviluppatori Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)
