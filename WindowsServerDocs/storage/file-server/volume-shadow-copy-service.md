---
title: Servizio Copia Shadow del volume
ms.date: 01/30/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 19e07504dad49c5e23cc49630015529e2a746aa7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394453"
---
# <a name="volume-shadow-copy-service"></a>Servizio Copia Shadow del volume

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

Il backup e il ripristino dei dati aziendali critici possono essere molto complessi a causa dei problemi seguenti:

  - È in genere necessario eseguire il backup dei dati mentre le applicazioni che generano i dati sono ancora in esecuzione. Ciò significa che alcuni file di dati potrebbero essere aperti o che potrebbero essere in uno stato incoerente.  
      
  - Se il set di dati è di grandi dimensioni, può essere difficile eseguirne il backup in una sola volta.  
      

Per eseguire correttamente le operazioni di backup e ripristino, è necessario un coordinamento di chiusura tra le applicazioni di backup, le applicazioni line-of-business di cui viene eseguito il backup e l'hardware e il software di gestione dell'archiviazione. Il Servizio Copia Shadow del volume (VSS), introdotto in Windows Server® 2003, semplifica la conversazione tra questi componenti per consentire loro di lavorare meglio insieme. Quando tutti i componenti supportano il servizio Copia Shadow del volume, è possibile usarli per eseguire il backup dei dati dell'applicazione senza portare offline le applicazioni.

VSS coordina le azioni necessarie per creare una copia shadow coerente (anche nota come una copia snapshot o temporizzata) dei dati di cui eseguire il backup. La copia shadow può essere usata così com'è oppure può essere usata in scenari come i seguenti:

  - Si desidera eseguire il backup dei dati delle applicazioni e delle informazioni sullo stato del sistema, inclusa l'archiviazione dei dati in un'altra unità disco rigido, su nastro o su un altro supporto rimovibile.  
      
  - Si è data mining.  
      
  - Si stanno eseguendo backup da disco a disco.  
      
  - È necessario un ripristino rapido dalla perdita di dati ripristinando i dati nel LUN originale o in un LUN completamente nuovo che sostituisce un LUN originale che ha avuto esito negativo.  
      

Le funzionalità di Windows e le applicazioni che usano VSS includono quanto segue:

  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [Copie shadow di cartelle condivise](http://go.microsoft.com/fwlink/?linkid=142874) (http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=180892) (http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [Ripristino configurazione di sistema](http://go.microsoft.com/fwlink/?linkid=180893) (http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>Funzionamento di Servizio Copia Shadow del volume

Una soluzione VSS completa richiede tutte le parti di base seguenti:

Il **servizio VSS**   parte del sistema operativo Windows che garantisce che gli altri componenti possano comunicare correttamente tra loro e interagiscono.

Il **richiedente VSS**   il software che richiede la creazione effettiva di copie shadow (o altre operazioni di alto livello, ad esempio l'importazione o l'eliminazione). In genere, si tratta dell'applicazione di backup. L'utilità Windows Server Backup e l'applicazione System Center Data Protection Manager sono richiedenti VSS. I richiedenti non Microsoft® VSS includono quasi tutto il software di backup eseguito in Windows.

**VSS writer**   il componente che garantisce la presenza di un set di dati coerente per il backup. Questa operazione viene in genere fornita come parte di un'applicazione line-of-business, ad esempio SQL Server® o Exchange Server. I writer VSS per diversi componenti di Windows, ad esempio il registro di sistema, sono inclusi nel sistema operativo Windows. I writer VSS non Microsoft sono inclusi in molte applicazioni per Windows che devono garantire la coerenza dei dati durante il backup.

Il **provider VSS**   il componente che crea e gestisce le copie shadow. Questo problema può verificarsi nel software o nell'hardware. Il sistema operativo Windows include un provider VSS che usa copy-on-Write. Se si usa una rete di archiviazione (SAN), è importante installare il provider hardware VSS per la SAN, se disponibile. Un provider hardware Scarica l'attività di creazione e gestione di una copia shadow dal sistema operativo host.

Il diagramma seguente illustra il modo in cui il servizio VSS coordina i richiedenti, i writer e i provider per creare una copia shadow di un volume.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figura 1**   diagramma dell'architettura di servizio Copia Shadow del volume

### <a name="how-a-shadow-copy-is-created"></a>Modalità di creazione di una copia shadow

Questa sezione inserisce nel contesto i vari ruoli del richiedente, del writer e del provider elencando i passaggi che devono essere eseguiti per creare una copia shadow. Il diagramma seguente illustra il modo in cui il Servizio Copia Shadow del volume controlla il coordinamento generale del richiedente, del writer e del provider.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figura 2** Processo di creazione copia shadow

Per creare una copia shadow, il richiedente, il writer e il provider eseguono le azioni seguenti:

1.  Il richiedente chiede al Servizio Copia Shadow del volume di enumerare i writer, raccogliere i metadati del writer e prepararsi per la creazione di copie shadow.  
      
2.  Ogni writer crea una descrizione XML dei componenti e degli archivi dati di cui è necessario eseguire il backup e li fornisce all'Servizio Copia Shadow del volume. Il writer definisce anche un metodo Restore, che viene usato per tutti i componenti. Il Servizio Copia Shadow del volume fornisce la descrizione del writer al richiedente, che seleziona i componenti di cui verrà eseguito il backup.  
      
3.  Il Servizio Copia Shadow del volume notifica a tutti i writer di preparare i dati per la creazione di una copia shadow.  
      
4.  Ogni writer prepara i dati nel modo appropriato, ad esempio completando tutte le transazioni aperte, i log delle transazioni in sequenza e le cache di svuotamento. Quando i dati sono pronti per la copia shadow, il writer invia una notifica all'Servizio Copia Shadow del volume.  
      
5.  Il Servizio Copia Shadow del volume indica ai writer di bloccare temporaneamente le richieste di I/O di scrittura dell'applicazione (le richieste di I/O di lettura sono ancora possibili) per i pochi secondi necessari per creare la copia shadow del volume o dei volumi. Il blocco dell'applicazione non può richiedere più di 60 secondi. Il Servizio Copia Shadow del volume svuota i buffer di file system e quindi blocca il file system, che garantisce che i metadati del file system vengano registrati correttamente e che i dati da replicare siano scritti in modo coerente.  
      
6.  Il Servizio Copia Shadow del volume indica al provider di creare la copia shadow. Il periodo di creazione della copia shadow non dura più di 10 secondi, durante i quali tutte le richieste I/O di scrittura alla file system rimangono bloccate.  
      
7.  Il Servizio Copia Shadow del volume rilascia file system le richieste di I/O di scrittura.  
      
8.  VSS indica ai writer di scongelare le richieste di I/O di scrittura dell'applicazione. A questo punto, le applicazioni possono riprendere la scrittura dei dati sul disco a cui viene eseguita la copia shadow.  
      

> [!NOTE]
> La creazione della copia shadow può essere interrotta se i writer sono conservati nello stato di blocco per più di 60 secondi o se i provider impiegano più di 10 secondi per eseguire il commit della copia shadow. 
<br>

9. Il richiedente può ritentare il processo (tornare al passaggio 1) o inviare una notifica all'amministratore per riprovare in un secondo momento.  
      
10. Se la copia shadow viene creata correttamente, il Servizio Copia Shadow del volume restituisce al richiedente le informazioni sulla posizione per la copia shadow. In alcuni casi, la copia shadow può essere resa temporaneamente disponibile come volume di lettura/scrittura in modo che VSS e una o più applicazioni possano modificare il contenuto della copia shadow prima che la copia shadow sia terminata. Dopo che le modifiche sono state apportate da VSS e dalle applicazioni, la copia shadow viene resa di sola lettura. Questa fase è denominata recupero automatico e viene usata per annullare le transazioni di file o di file System nel volume della copia shadow che non sono state completate prima della creazione della copia shadow.  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>Modalità di creazione di una copia shadow da parte del provider

Un provider di copia shadow hardware o software usa uno dei metodi seguenti per la creazione di una copia shadow:

**Completa copia**   questo metodo esegue una copia completa (denominata "copia completa" o "Clona") del volume originale in un determinato momento. Questa copia è di sola lettura.

**Copy-on-write**   questo metodo non copia il volume originale. Esegue invece una copia differenziale copiando tutte le modifiche (richieste di I/O di scrittura completate) che vengono effettuate al volume dopo un determinato momento.

**Redirect-on-write**   questo metodo non copia il volume originale e non esegue alcuna modifica al volume originale dopo un determinato momento. Esegue invece una copia differenziale reindirizzando tutte le modifiche a un volume diverso.

## <a name="complete-copy"></a>Copia completa

Una copia completa viene in genere creata creando uno "split mirror" come indicato di seguito:

1.  Il volume originale e il volume della copia shadow sono un set di volumi con mirroring.  
      
2.  Il volume della copia shadow è separato dal volume originale. Questa operazione interrompe la connessione mirror.  
      

Dopo che la connessione mirror è stata interruppe, il volume originale e il volume della copia shadow sono indipendenti. Il volume originale continua ad accettare tutte le modifiche (richieste di I/O di scrittura), mentre il volume della copia shadow rimane un'esatta copia di sola lettura dei dati originali al momento dell'errore.

### <a name="copy-on-write-method"></a>Metodo Copy-on-Write

Nel metodo Copy-on-Write, quando si verifica una modifica al volume originale (ma prima del completamento della richiesta di I/O di scrittura), ogni blocco da modificare viene letto e quindi scritto nell'area di archiviazione della copia shadow del volume (detta anche "area diff"). L'area di archiviazione della copia shadow può trovarsi nello stesso volume o in un volume diverso. In questo modo si conserva una copia del blocco di dati nel volume originale prima che la modifica la sovrascriva.


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
<th>Copia Shadow (stato e dati)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dati originali: 1 2 3 4 5</p></td>
<td><p>Nessuna copia:-</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dati modificati nella cache: da 3 a 3'</p></td>
<td><p>Copia shadow creata (solo differenze): 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dati originali sovrascritti: 1 2 3' 4 5</p></td>
<td><p>Differenze e indice archiviati nella copia shadow: 3</p></td>
</tr>
</tbody>
</table>

**Tabella 1**   il metodo Copy-on-Write per la creazione di copie shadow

Il metodo Copy-on-Write è un metodo rapido per la creazione di una copia shadow, perché copia solo i dati modificati. I blocchi copiati nell'area diff possono essere combinati con i dati modificati nel volume originale per ripristinare lo stato del volume prima che siano state apportate le modifiche. Se sono presenti molte modifiche, il metodo Copy-on-Write può diventare costoso.

### <a name="redirect-on-write-method"></a>Metodo di reindirizzamento in scrittura

Nel metodo di reindirizzamento in scrittura, ogni volta che il volume originale riceve una modifica (richiesta di I/O di scrittura), la modifica non viene applicata al volume originale. La modifica viene invece scritta nell'area di archiviazione della copia shadow di un altro volume.


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
<th>Copia Shadow (stato e dati)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dati originali: 1 2 3 4 5</p></td>
<td><p>Nessuna copia:-</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dati modificati nella cache: da 3 a 3'</p></td>
<td><p>Copia shadow creata (solo differenze): 3'</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dati originali non modificati: 1 2 3 4 5</p></td>
<td><p>Differenze e indice archiviati nella copia shadow: 3'</p></td>
</tr>
</tbody>
</table>

**Tabella 2**   il metodo di reindirizzamento in scrittura per la creazione di copie shadow

Come il metodo Copy-on-Write, il metodo Redirect-on-Write è un metodo rapido per la creazione di una copia shadow, perché copia solo le modifiche apportate ai dati. I blocchi copiati nell'area diff possono essere combinati con i dati non modificati nel volume originale per creare una copia completa e aggiornata dei dati. Se sono presenti molte richieste di I/O di lettura, il metodo di reindirizzamento in scrittura può diventare costoso.

## <a name="shadow-copy-providers"></a>Provider di copie shadow

Sono disponibili due tipi di provider di copie shadow: provider basati su hardware e provider basati su software. È disponibile anche un provider di sistema, ovvero un provider software integrato nel sistema operativo Windows.

### <a name="hardware-based-providers"></a>Provider basati su hardware

I provider di copie shadow basate su hardware fungono da interfaccia tra il Servizio Copia Shadow del volume e il livello hardware lavorando insieme a un adattatore o a un controller di archiviazione hardware. Il lavoro di creazione e gestione della copia shadow viene eseguito dall'array di archiviazione.

I provider hardware accettano sempre la copia shadow di un intero LUN, ma il Servizio Copia Shadow del volume espone solo la copia shadow del volume o dei volumi richiesti.

Un provider di copie shadow basato su hardware utilizza la funzionalità Servizio Copia Shadow del volume che definisce il punto nel tempo, consente la sincronizzazione dei dati, gestisce la copia shadow e fornisce un'interfaccia comune con le applicazioni di backup. Tuttavia, il Servizio Copia Shadow del volume non specifica il meccanismo sottostante in base al quale il provider basato su hardware produce e gestisce copie shadow.

### <a name="software-based-providers"></a>Provider basati su software

I provider di copie shadow basate su software in genere intercettano ed elaborano le richieste di I/O in lettura e scrittura in un livello software tra il file system e il software di gestione dei volumi.

Questi provider vengono implementati come un componente DLL in modalità utente e almeno un driver di dispositivo in modalità kernel, in genere un driver del filtro di archiviazione. A differenza dei provider basati su hardware, i provider basati su software creano copie shadow a livello di software, non a livello di hardware.

Un provider di copie shadow basato su software deve mantenere una visualizzazione temporizzata di un volume con accesso a un set di dati che può essere usato per ricreare lo stato del volume prima dell'ora di creazione della copia shadow. Un esempio è la tecnica copy-on-Write del provider di sistema. Tuttavia, il Servizio Copia Shadow del volume non impone alcuna restrizione sulla tecnica utilizzata dai provider basati su software per creare e gestire copie shadow.

Un provider di software è applicabile a una gamma più ampia di piattaforme di archiviazione rispetto a un provider basato su hardware e dovrebbe funzionare anche con dischi di base o volumi logici. Un volume logico è un volume creato combinando spazio libero da due o più dischi. Diversamente dalle copie shadow dell'hardware, i provider software utilizzano le risorse del sistema operativo per gestire la copia shadow.

Per altre informazioni sui dischi di base, vedere [che cosa sono i dischi e i volumi di base?](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) su TechNet.

### <a name="system-provider"></a>Provider di sistema

Un provider di copia shadow, il provider di sistema, viene fornito nel sistema operativo Windows. Sebbene venga fornito un provider predefinito in Windows, altri fornitori sono liberi di fornire implementazioni ottimizzate per le applicazioni hardware e software di archiviazione.

Per mantenere la visualizzazione "temporizzata" di un volume contenuto in una copia shadow, il provider di sistema utilizza una tecnica copy-on-Write. Le copie dei blocchi nel volume modificate dall'inizio della creazione della copia shadow vengono archiviate in un'area di archiviazione della copia shadow.

Il provider di sistema può esporre il volume di produzione, che può essere scritto e letto normalmente. Quando è necessaria la copia shadow, applica logicamente le differenze ai dati nel volume di produzione per esporre la copia shadow completa.

Per il provider di sistema, l'area di archiviazione copia shadow deve trovarsi in un volume NTFS. Il volume da replicare non deve essere un volume NTFS, ma almeno un volume montato nel sistema deve essere un volume NTFS.

I file dei componenti che costituiscono il provider di sistema sono swprv. dll e Volsnap. sys.

### <a name="in-box-vss-writers"></a>Writer VSS in-box

Il sistema operativo Windows include un set di writer VSS responsabili dell'enumerazione dei dati necessari per varie funzionalità di Windows.

Per ulteriori informazioni su questi writer, vedere i siti Web Microsoft seguenti:

  - [Writer VSS in-box](http://go.microsoft.com/fwlink/?linkid=180895) (http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [Nuovi writer VSS predefiniti per Windows Server 2008 e Windows Vista SP1](http://go.microsoft.com/fwlink/?linkid=180896) (http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Nuovi writer VSS predefiniti per Windows Server 2008 R2 e Windows 7](http://go.microsoft.com/fwlink/?linkid=180897) (http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>Modalità di utilizzo delle copie shadow

Oltre a eseguire il backup dei dati delle applicazioni e delle informazioni sullo stato del sistema, è possibile utilizzare le copie shadow per diversi scopi, inclusi i seguenti:

  - Ripristino di LUN (risincronizzazione LUN e swapping LUN)  
      
  - Ripristino di singoli file (copie shadow per cartelle condivise)  
      
  - Data mining tramite copie shadow trasportabili  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Ripristino di LUN (risincronizzazione LUN e swapping LUN)

In Windows Server 2008 R2 e Windows 7, i richiedenti VSS possono utilizzare una funzionalità del provider di copia shadow hardware denominata risincronizzazione LUN (o "risincronizzazione LUN"). Si tratta di uno schema di ripristino rapido che consente a un amministratore dell'applicazione di ripristinare i dati da una copia shadow nel LUN originale o in un nuovo LUN.

La copia shadow può essere un clone completo o una copia shadow differenziale. In entrambi i casi, al termine dell'operazione di risincronizzazione, il LUN di destinazione avrà lo stesso contenuto del LUN della copia shadow. Durante l'operazione di risincronizzazione, la matrice esegue una copia a livello di blocco dalla copia shadow al LUN di destinazione.


> [!NOTE]
> La copia shadow deve essere una copia shadow dell'hardware trasportabile. 
<br>


La maggior parte delle matrici consente la ripresa delle operazioni di I/O di produzione poco dopo l'inizio dell'operazione di risincronizzazione. Mentre è in corso l'operazione di risincronizzazione, le richieste di lettura vengono reindirizzate al LUN della copia shadow e scrivono le richieste nel LUN di destinazione. In questo modo gli array possono ripristinare set di dati di grandi dimensioni e riprendere le normali operazioni in pochi secondi.

La risincronizzazione LUN è diversa dallo scambio di LUN. Uno scambio LUN è uno scenario di ripristino rapido supportato da VSS da Windows Server 2003 SP1. In uno scambio LUN la copia shadow viene importata e quindi convertita in un volume di lettura/scrittura. La conversione è un'operazione irreversibile e il volume e il LUN sottostante non possono essere controllati con le API VSS dopo tale operazione. Nell'elenco seguente viene descritto in che modo la risincronizzazione LUN viene confrontata con lo scambio LUN:

  - Nella risincronizzazione LUN la copia shadow non viene modificata, quindi può essere usata più volte. Nello swapping LUN la copia shadow può essere usata una sola volta per un ripristino. Questo è importante per gli amministratori con la massima sicurezza. Quando si usa la risincronizzazione LUN, il richiedente può ritentare l'intera operazione di ripristino se si verifica un errore la prima volta.  
      
  - Alla fine di uno scambio LUN, il LUN della copia shadow viene usato per le richieste di I/O di produzione. Per questo motivo, il LUN della copia shadow deve usare la stessa qualità di archiviazione del LUN di produzione originale per garantire che le prestazioni non vengano influenzate dopo l'operazione di ripristino. Se invece viene utilizzata la risincronizzazione LUN, il provider hardware può gestire la copia shadow in una risorsa di archiviazione meno costosa rispetto all'archiviazione di qualità della produzione.  
      
  - Se il LUN di destinazione non è utilizzabile ed è necessario ricrearlo, lo swapping dei LUN potrebbe essere più economico perché non richiede un LUN di destinazione.  
      


> [!WARNING]
> Tutte le operazioni elencate sono operazioni a livello di LUN. Se si tenta di ripristinare un volume specifico utilizzando la risincronizzazione LUN, si involontariamente ripristinare tutti gli altri volumi che condividono il LUN. 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Ripristino di singoli file (copie shadow per cartelle condivise)

Copie shadow per cartelle condivise usa il Servizio Copia Shadow del volume per fornire copie temporizzate dei file che si trovano in una risorsa di rete condivisa, ad esempio un file server. Con copie shadow per cartelle condivise, gli utenti possono ripristinare rapidamente i file eliminati o modificati archiviati nella rete. Poiché possono eseguire questa operazione senza l'assistenza dell'amministratore, copie shadow per cartelle condivise possibile aumentare la produttività e ridurre i costi amministrativi.

Per ulteriori informazioni su copie shadow per cartelle condivise, vedere [copie shadow per cartelle condivise](http://go.microsoft.com/fwlink/?linkid=180898) (http://go.microsoft.com/fwlink/?LinkId=180898) su TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Data mining tramite copie shadow trasportabili

Con un provider hardware progettato per l'utilizzo con il Servizio Copia Shadow del volume, è possibile creare copie shadow trasportabili che possono essere importate su server all'interno dello stesso sottosistema, ad esempio una rete SAN. Queste copie shadow possono essere utilizzate per eseguire il seeding di un'installazione di produzione o di test con dati di sola lettura per data mining.

Con il Servizio Copia Shadow del volume e un array di archiviazione con un provider hardware progettato per l'uso con la Servizio Copia Shadow del volume, è possibile creare una copia shadow del volume dei dati di origine in un server e quindi importare la copia shadow in un altro server.  o di nuovo nello stesso server. Questo processo viene eseguito in pochi minuti, indipendentemente dalla dimensione dei dati. Il processo di trasporto viene eseguito tramite una serie di passaggi che usano un richiedente di copia shadow (un'applicazione di gestione dell'archiviazione) che supporta le copie shadow trasportabili.

## <a name="to-transport-a-shadow-copy"></a>Per trasportare una copia shadow

1.  Creare una copia shadow trasportabile dei dati di origine in un server.

2.  Importare la copia shadow in un server connesso alla SAN (è possibile importare in un server diverso o nello stesso server).

3.  I dati sono ora pronti per essere usati.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figura 3**   creazione e il trasporto di copie shadow tra due server


> [!NOTE]
> Una copia shadow trasportabile creata in Windows Server 2003 non può essere importata in un server che esegue Windows Server 2008 o Windows Server 2008 R2. Una copia shadow trasportabile creata in Windows Server 2008 o Windows Server 2008 R2 non può essere importata in un server che esegue Windows Server 2003. Tuttavia, è possibile importare una copia shadow creata in Windows Server 2008 in un server che esegue Windows Server 2008 R2 e viceversa. 
<br>


Le copie shadow sono di sola lettura. Se si vuole convertire una copia shadow in un LUN di lettura/scrittura, è possibile usare un'applicazione di gestione dell'archiviazione basata su un servizio disco virtuale (inclusi alcuni richiedenti) oltre al Servizio Copia Shadow del volume. Utilizzando questa applicazione, è possibile rimuovere la copia shadow dalla gestione Servizio Copia Shadow del volume e convertirla in un LUN di lettura/scrittura.

Il trasporto Servizio Copia Shadow del volume è una soluzione avanzata sui computer che eseguono Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 o Windows Server 2008 R2. Funziona solo se è presente un provider hardware nell'array di archiviazione. Il trasporto delle copie shadow può essere utilizzato per diversi scopi, tra cui backup su nastro, data mining e test.

## <a name="frequently-asked-questions"></a>Domande frequenti

Queste domande frequenti rispondono alle domande sugli Servizio Copia Shadow del volume (VSS) per gli amministratori di sistema. Per informazioni sulle interfacce di programmazione dell'applicazione VSS, vedere [servizio Copia Shadow del volume](http://go.microsoft.com/fwlink/?linkid=180899) (http://go.microsoft.com/fwlink/?LinkId=180899) nella libreria del centro per sviluppatori Windows.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>Quando è stato Servizio Copia Shadow del volume introdotto? In quali versioni del sistema operativo Windows è disponibile?

VSS è stato introdotto in Windows XP. È disponibile in Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 e Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>Qual è la differenza tra una copia shadow e un backup?

Nel caso di un backup di unità disco rigido, la copia shadow creata è anche il backup. I dati possono essere copiati fuori dalla copia shadow per un ripristino oppure la copia shadow può essere usata per uno scenario di ripristino rapido, ad esempio la risincronizzazione LUN o lo scambio di LUN.

Quando i dati vengono copiati dalla copia shadow su nastro o da altri supporti rimovibili, il contenuto archiviato nel supporto costituisce il backup. La copia shadow può essere eliminata dopo la copia dei dati.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>Qual è il volume di dimensioni maggiori supportato da Servizio Copia Shadow del volume?

Servizio Copia Shadow del volume supporta una dimensione del volume fino a 64 TB.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>Ho eseguito un backup in Windows Server 2008. È possibile ripristinarlo in Windows Server 2008 R2?

Dipende dal software di backup utilizzato. La maggior parte dei programmi di backup supporta questo scenario per i dati, ma non per i backup dello stato del sistema.

Le copie shadow create in una di queste versioni di Windows possono essere usate nell'altra.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>Ho eseguito un backup in Windows Server 2003. È possibile ripristinarlo in Windows Server 2008?

Dipende dal software di backup utilizzato. Se si crea una copia shadow in Windows Server 2003, non è possibile usarla in Windows Server 2008. Inoltre, se si crea una copia shadow in Windows Server 2008, non è possibile ripristinarla in Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>Come è possibile disabilitare VSS?

È possibile disabilitare il Servizio Copia Shadow del volume utilizzando Microsoft Management Console. Tuttavia, non è necessario eseguire questa operazione. La disabilitazione del servizio Copia Shadow del volume influisce negativamente sui software usati che dipendono da esso, ad esempio Ripristino configurazione di sistema e Windows Server Backup.

Per ulteriori informazioni, vedere i seguenti siti Web di Microsoft TechNet:

  - [Ripristino configurazione di sistema](http://go.microsoft.com/fwlink/?linkid=157113) (http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>È possibile escludere i file da una copia shadow per risparmiare spazio?

VSS è progettato per creare copie shadow di interi volumi. I file temporanei, ad esempio i file di paging, vengono automaticamente omessi dalle copie shadow per risparmiare spazio.

Per escludere file specifici dalle copie shadow, usare la chiave del registro di sistema seguente: **FilesNotToSnapshot**.


> [!NOTE]
> La chiave del registro di sistema <STRONG>FilesNotToSnapshot</STRONG> può essere usata solo dalle applicazioni. Gli utenti che tentano di usarli incontreranno limitazioni come le seguenti:
> <br>
> <UL>
> <LI>Non è possibile eliminare i file da una copia shadow creata in un server Windows utilizzando la funzionalità versioni precedenti.<BR><BR>
> <LI>Non è possibile eliminare i file dalle copie shadow per le cartelle condivise.<BR><BR>
> <LI>Consente di eliminare i file da una copia shadow creata con l'utilità <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">DiskShadow</a> , ma non di eliminare i file da una copia shadow creata tramite l'utilità <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">vssadmin</a> .<BR><BR>
> <LI>I file vengono eliminati da una copia shadow in base al massimo sforzo. Ciò significa che non è garantito che vengano eliminati.<BR><BR></LI></UL>


Per ulteriori informazioni, vedere [esclusione di file dalle copie shadow](http://go.microsoft.com/fwlink/?linkid=180904) (http://go.microsoft.com/fwlink/?LinkId=180904) su MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Il programma di backup non Microsoft non è riuscito con un errore VSS. Cosa posso fare?

Consultare la sezione supporto tecnico del sito Web della società che ha creato il programma di backup. Per risolvere il problema potrebbe essere disponibile un aggiornamento del prodotto che è possibile scaricare e installare. In caso contrario, contattare il reparto supporto tecnico aziendale.

Gli amministratori di sistema possono utilizzare le informazioni sulla risoluzione dei problemi del servizio Copia Shadow del volume nel sito Web Microsoft TechNet Library seguente per raccogliere informazioni diagnostiche sui problemi correlati a VSS.

Per ulteriori informazioni, vedere [servizio Copia Shadow del volume](http://go.microsoft.com/fwlink/?linkid=180905) (http://go.microsoft.com/fwlink/?LinkId=180905) su TechNet.

### <a name="what-is-the-diff-area"></a>Che cos'è la "area diff"?

L'area di archiviazione della copia shadow (o "area diff") è la posizione in cui vengono archiviati i dati per la copia shadow creata dal provider software di sistema.

### <a name="where-is-the-diff-area-located"></a>Dove si trova l'area diff?

L'area diff può trovarsi in qualsiasi volume locale. Tuttavia, deve trovarsi in un volume NTFS con spazio sufficiente per l'archiviazione.

### <a name="how-is-the-diff-area-location-determined"></a>In che modo viene determinata la posizione dell'area diff?

I criteri seguenti vengono valutati, in questo ordine, per determinare il percorso dell'area diff:

  - Se un volume dispone già di una copia shadow esistente, verrà utilizzata tale posizione.  
      
  - Se è presente un'associazione manuale preconfigurata tra il volume originale e il percorso del volume di copia shadow, viene usata tale località.  
      
  - Se i due criteri precedenti non forniscono una posizione, il servizio Copia Shadow sceglie una località in base allo spazio disponibile. Se viene eseguita la copia shadow di più di un volume, il servizio Copia Shadow crea un elenco di possibili percorsi di snapshot in base alla dimensione dello spazio disponibile, in ordine decrescente. Il numero di posizioni specificato è uguale al numero di volumi di cui viene eseguita la copia shadow.  
      
  - Se il volume da replicare è uno dei percorsi possibili, viene creata un'associazione locale. In caso contrario, viene creata un'associazione con il volume con lo spazio più disponibile.  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>VSS può creare copie shadow di volumi non NTFS?

Sì. Tuttavia, le copie shadow permanenti possono essere eseguite solo per i volumi NTFS. Inoltre, almeno un volume montato sul sistema deve essere un volume NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>Qual è il numero massimo di copie shadow che è possibile creare contemporaneamente?

Il numero massimo di volumi copia shadow in un set di copie shadow singolo è 64. Si noti che questa operazione non corrisponde al numero di copie shadow.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>Qual è il numero massimo di copie shadow software create dal provider di sistema che è possibile gestire per un volume?

Il numero massimo di copie shadow software per ogni volume è 512. Per impostazione predefinita, tuttavia, è possibile gestire solo le copie shadow 64 utilizzate dalle copie shadow della funzionalità cartelle condivise. Per modificare il limite per le copie shadow della funzionalità cartelle condivise, usare la chiave del registro di sistema seguente: **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>Come è possibile controllare lo spazio usato per lo spazio di archiviazione della copia shadow?

Digitare il comando **vssadmin resize shadowstorage** .

Per ulteriori informazioni, vedere [vssadmin resize shadowstorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) su TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>Cosa accade quando si esaurisce lo spazio?

Le copie shadow per il volume vengono eliminate, iniziando con la copia shadow meno recente.

## <a name="volume-shadow-copy-service-tools"></a>Strumenti di Servizio Copia Shadow del volume

Il sistema operativo Windows offre gli strumenti seguenti per l'utilizzo di VSS:

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [Vssadmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

DiskShadow è un richiedente del servizio Copia Shadow del volume che è possibile usare per gestire tutti gli snapshot hardware e software che è possibile avere in un sistema. DiskShadow include comandi come i seguenti:

  - **elenco**: elenca i writer VSS, i provider VSS e le copie shadow  
      
  - **Crea**: crea una nuova copia shadow  
      
  - **importazione**: importa una copia shadow trasportabile  
      
  - **Expose**: espone una copia shadow persistente, ad esempio una lettera di unità.  
      
  - **Annulla**: ripristina un volume a una copia shadow specificata  
      

Questo strumento è destinato ai professionisti IT, ma può risultare utile anche per il test di un provider VSS writer o VSS.

DiskShadow è disponibile solo nei sistemi operativi Windows Server. Non è disponibile nei sistemi operativi client Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin viene utilizzato per creare, eliminare ed elencare le informazioni sulle copie shadow. Può anche essere usato per ridimensionare l'area di archiviazione della copia shadow ("area diff").

VssAdmin include comandi come i seguenti:

  - **Crea ombreggiatura**: crea una nuova copia shadow  
      
  - **Elimina ombre**: Elimina le copie shadow  
      
  - **list providers**: elenca tutti i provider VSS registrati  
      
  - **Elenca writer**: elenca tutti i writer VSS sottoscritti  
      
  - **resize shadowstorage**: modifica le dimensioni massime dell'area di archiviazione della copia shadow  
      

VssAdmin può essere utilizzato solo per amministrare le copie shadow create dal provider software di sistema.

VssAdmin è disponibile nelle versioni del sistema operativo Windows client e Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Chiavi del registro di sistema Servizio Copia Shadow del volume

Le seguenti chiavi del registro di sistema sono disponibili per l'utilizzo con VSS:

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

Questa chiave viene usata per specificare gli utenti che hanno accesso alle copie shadow.

Per ulteriori informazioni, vedere le voci seguenti sul sito Web MSDN:

  - [Considerazioni sulla sicurezza per i writer](http://go.microsoft.com/fwlink/?linkid=157739) (http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [Considerazioni sulla sicurezza per i richiedenti](http://go.microsoft.com/fwlink/?linkid=180908) (http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

Questa chiave specifica il numero massimo di copie shadow accessibili dal client che possono essere archiviate in ogni volume del computer. Le copie shadow accessibili dal client vengono utilizzate da copie shadow per cartelle condivise.

Per ulteriori informazioni, vedere la voce seguente sul sito Web MSDN:

**MaxShadowCopies** in [chiavi del registro di sistema per il backup e il ripristino](http://go.microsoft.com/fwlink/?linkid=180909) (http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Questa chiave specifica le dimensioni minime iniziali, in MB, dell'area di archiviazione della copia shadow.

Per ulteriori informazioni, vedere la voce seguente sul sito Web MSDN:

**MinDiffAreaFileSize** in [chiavi del registro di sistema per il backup e il ripristino](http://go.microsoft.com/fwlink/?linkid=180910) (http://go.microsoft.com/fwlink/?LinkId=180910)

Versioni del sistema operativo supportate da `##`#'

Nella tabella seguente sono elencate le versioni minime del sistema operativo supportate per le funzionalità VSS.


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
<td><p>Chiave del registro di sistema <strong>FilesNotToSnapshot</strong></p></td>
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
<td><p>copie shadow per cartelle condivise</p></td>
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
<td><p>Singola sessione di ripristino simultanea con backup</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 con SP2</p></td>
</tr>
<tr class="even">
<td><p>Fino a 8 sessioni di ripristino simultanee con backup</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Vedi anche

[Servizio Copia Shadow del volume nel centro per sviluppatori Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)