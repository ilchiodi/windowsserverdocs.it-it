---
Title: Servizio Copia Shadow del volume
ms.date: 01/30/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 0a4af25723c6d1e796cd3255875c15faf21fb8be
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284389"
---
# <a name="volume-shadow-copy-service"></a>Servizio Copia Shadow del volume

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

Backup e ripristino dei dati aziendali critici può risultare molto complesse a causa dei problemi seguenti:

  - I dati in genere devono essere eseguito il backup, mentre le applicazioni che generano i dati sono ancora in esecuzione. Ciò significa che alcuni dei file di dati potrebbe essere aperta o potrebbero essere in uno stato incoerente.  
      
  - Se il set di dati è grande, può essere difficile eseguire il backup di tutto in una sola volta.  
      

Corretta esecuzione di operazioni di backup e ripristino richiede la stretta collaborazione tra le applicazioni di backup, le applicazioni line-of-business che vengono sottoposti a backup e l'hardware di gestione di archiviazione e software. Il Volume di servizio Copia Shadow (VSS), che è stata introdotta in Windows Server® 2003, facilita la conversazione tra questi componenti in modo che possano migliorare la collaborazione. Quando tutti i componenti supportano VSS, è possibile usare per eseguire il backup dei dati dell'applicazione senza portare offline le applicazioni.

VSS coordina le azioni necessarie per creare una copia shadow coerenti (noto anche come uno snapshot o una copia di point-in-time) dei dati consiste nell'eseguire il backup. La copia shadow è utilizzabile come-viene, o può essere usato in scenari come la seguente:

  - Si desidera eseguire il backup dell'applicazione i dati e sistema le informazioni sullo stato, tra cui l'archiviazione dei dati in un'altra unità disco rigido, su nastro o su un altro supporto rimovibile.  
      
  - Si è il data mining.  
      
  - Si siano eseguendo i backup disco-a-disco.  
      
  - È necessario un ripristino veloce dopo la perdita di dati per il ripristino dei dati per il LUN originale o a un LUN completamente nuovo che sostituisce un LUN originale che non è riuscita.  
      

Le funzionalità di Windows e le applicazioni che usano VSS includono quanto segue:

  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [Copie shadow di cartelle condivise](http://go.microsoft.com/fwlink/?linkid=142874) (http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=180892) (http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [Ripristino del sistema](http://go.microsoft.com/fwlink/?linkid=180893) (http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>Funzionamento del servizio Copia Shadow del Volume

Una soluzione completa di VSS richiede tutte le parti di base seguenti:

**Il servizio VSS**   parte del sistema operativo Windows che assicura gli altri componenti possa comunicare tra loro in modo corretto e collaborare.

**Richiedente VSS**   il software che richiede la creazione effettiva di copie shadow (o altre operazioni di alto livello, ad esempio l'importazione o l'eliminazione). Si tratta in genere, l'applicazione di backup. L'utilità Windows Server Backup e l'applicazione di System Center Data Protection Manager sono richiedenti VSS. Richiedenti non Microsoft® VSS comprendono quasi tutti i software di backup che viene eseguito in Windows.

**Writer VSS**   il componente che garantisce un set di dati coerente per eseguire il backup. Ciò è solitamente offerto come parte di un'applicazione line-of-business, ad esempio SQL Server® o Exchange Server. I writer VSS per i vari componenti di Windows, ad esempio il Registro di sistema sono inclusi con il sistema operativo Windows. I writer VSS non Microsoft sono inclusi in molte applicazioni per Windows che è necessario garantire la coerenza dei dati durante il backup.

**Provider servizio Copia shadow**   il componente che crea e gestisce le copie shadow. Ciò può verificarsi nel software o hardware. Il sistema operativo Windows include un provider VSS che usa copy-on-write. Se si usa una rete di archiviazione (SAN), è importante che si installa il provider hardware VSS per la rete SAN, se presente. Un provider hardware, gli Offload di attività di creazione e gestione di una copia shadow dal sistema operativo host.

Il diagramma seguente illustra come il servizio VSS coordina con richiedenti, i writer e i provider per creare una copia shadow di un volume.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figura 1**   diagramma dell'architettura del servizio Copia Shadow del Volume

### <a name="how-a-shadow-copy-is-created"></a>Come viene creata una copia Shadow

In questa sezione viene inserito i diversi ruoli del richiedente, writer e provider di contesto elencando i passaggi che devono essere eseguite per creare una copia shadow. Il diagramma seguente mostra come i controlli il coordinamento complessivo del richiedente, writer e provider servizio Copia Shadow del Volume.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figura 2** processo di creazione copia Shadow

Per creare una copia shadow, il richiedente, lo scrittore e provider di eseguire le azioni seguenti:

1.  Il richiedente richiede il servizio Copia Shadow del Volume per enumerare i writer, raccogliere i metadati del writer e preparare per la creazione di copie shadow.  
      
2.  Ogni agente di scrittura crea una descrizione XML degli archivi dati e i componenti che devono essere sottoposti a backup e lo fornisce al servizio Copia Shadow del Volume. Il writer definisce anche un metodo di ripristino, che viene usato per tutti i componenti. Il servizio Copia Shadow del Volume fornisce la descrizione del writer al richiedente, che seleziona i componenti che verranno sottoposti a backup.  
      
3.  Il servizio Copia Shadow del Volume di notifica a tutti i writer di preparare i dati per effettuare una copia shadow.  
      
4.  Ogni agente di scrittura prepara i dati come appropriato, ad esempio il completamento di tutte le transazioni aperte, in sequenza dei log delle transazioni e dello scaricamento di cache. Quando i dati sono pronti per la copia shadow, il writer di notifica al servizio Copia Shadow del Volume.  
      
5.  Il servizio Copia Shadow del Volume indica i writer per bloccare temporaneamente le richieste dei / o di scrittura (lettura i/o le richieste sono comunque possibili) dell'applicazione per i secondi necessari per creare la copia shadow del volume o i volumi. Il blocco dell'applicazione non è consentito richiedere più di 60 secondi. Il servizio Copia Shadow del Volume Svuota il buffer di file system e quindi si blocca il file system, che assicura che i metadati del file system viene registrato correttamente e i dati da una copia shadow sono scritto in un ordine coerente.  
      
6.  Il servizio Copia Shadow del Volume indica al provider di creare la copia shadow. Il periodo di creazione copia shadow dura non più di 10 secondi, durante i quali tutti scrivere le richieste dei / o al file system di rimangano bloccate.  
      
7.  Il servizio Copia Shadow del Volume versioni richieste dei / o scrittura di file system.  
      
8.  VSS indica i writer di sblocco delle richieste dei / o di scrittura dell'applicazione. A questo punto le applicazioni sono liberi di riprendere la scrittura dei dati sul disco che viene creata una copia shadow.  
      

> [!NOTE]
> La creazione di copie shadow può essere interrotta se i writer sono mantenuti nello stato di blocco per più di 60 secondi o se i provider richiedono più di 10 secondi per eseguire il commit la copia shadow. 
<br>

9. Il richiedente può ripetere il processo (Vai al passaggio 1) o inviare una notifica all'amministratore di riprovare in un secondo momento.  
      
10. Se la copia shadow è stata creata, il servizio Copia Shadow del Volume restituisce le informazioni sul percorso per la copia shadow al richiedente. In alcuni casi, la copia shadow può essere temporaneamente resi disponibile come un volume di lettura / scrittura in modo che VSS, volume e una o più applicazioni possono alterare il contenuto della copia shadow prima che sia terminata la copia shadow. Dopo aver VSS, volume e le applicazioni apportano le modifiche, viene eseguita la copia shadow sola lettura. Questa fase viene chiamata recupero automatico e viene usato per annullare le transazioni nel volume copia shadow del file system o un'applicazione che non sono state completate prima che la copia shadow è stata creata.  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>Modo in cui il Provider crea una copia Shadow

Un provider di copie shadow hardware o software usa uno dei metodi seguenti per la creazione di una copia shadow:

**Copia di completare**   questo metodo effettua una copia completa del volume originale (denominata una "copia completa" o "cloni") in un determinato punto nel tempo. Questa copia è di sola lettura.

**Copy-on-write**   questo metodo non copia il volume originale. Al contrario, ne esegue una copia differenziale tramite la copia di tutte le modifiche (richieste dei / o scrittura completati) che vengono apportate al volume dopo un determinato punto nel tempo.

**Reindirizzamento-on-write**   questo metodo non copia il volume originale e non esegue tutte le modifiche al volume originale dopo un determinato punto nel tempo. Al contrario, ne esegue una copia di backup differenziale mediante il reindirizzamento di tutte le modifiche in un volume diverso.

## <a name="complete-copy"></a>Copia completa

In genere viene creata una copia completa, rendendo un mirror"split" come indicato di seguito:

1.  Il volume originale e il volume copia shadow sono un set di volumi con mirroring.  
      
2.  Il volume copia shadow è separato dal volume originale. Questa operazione interrompe la connessione di mirror.  
      

Dopo aver interrotta la connessione di server mirror, il volume originale e il volume copia shadow sono indipendenti. Il volume originale continua ad accettare tutte le modifiche (richieste dei / o scrittura), durante la copia shadow del volume rimane una copia esatta di sola lettura dei dati originali al momento dell'interruzione.

### <a name="copy-on-write-method"></a>Copy-on-write (metodo)

Nel metodo di copia su scrittura, quando viene apportata una modifica al volume originale, ma prima della scrittura richiesta dei / o viene completata, ogni blocco da modificare viene letta e scritta quindi area archiviazione copia shadow del volume (detto anche il "area diff"). L'area di archiviazione copia shadow può trovarsi nello stesso volume o un volume diverso. Ciò consente di mantenere una copia del blocco di dati nel volume originale prima che la modifica viene sovrascritto.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>Dati di origine (stato e dati)</th>
<th>Copia shadow (stato e dati)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dati originali: 1 2 3 4 5</p></td>
<td><p>Nessuna copia::</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dati modificati nella cache: 3 di 3'</p></td>
<td><p>Copia shadow creata (solo le differenze): 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dati originali sovrascritto: 1 2 3’ 4 5</p></td>
<td><p>Le differenze e indice archiviati nella copia shadow: 3</p></td>
</tr>
</tbody>
</table>

**Tabella 1**   il metodo copy-on-write di creazione di copie shadow

Il metodo di copia su scrittura è un metodo rapido per la creazione di una copia shadow, poiché copia solo i dati che sono stati modificati. I blocchi copiati nell'area diff possono essere combinati con i dati modificati nel volume originale per ripristinare il volume per il proprio stato prima che venissero apportate le modifiche. Se sono presenti numerose modifiche, il metodo di copia su scrittura può diventare costoso.

### <a name="redirect-on-write-method"></a>Metodo di reindirizzamento in scrittura

Nel metodo di reindirizzamento in scrittura, ogni volta che il volume originale riceve una modifica (richiesta dei / o scrittura), la modifica non viene applicata al volume originale. Al contrario, la modifica viene scritto in area di archiviazione copia shadow del volume.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>Dati di origine (stato e dati)</th>
<th>Copia shadow (stato e dati)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dati originali: 1 2 3 4 5</p></td>
<td><p>Nessuna copia::</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dati modificati nella cache: 3 di 3'</p></td>
<td><p>Copia shadow creata (solo le differenze): 3’</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dati originali non modificati: 1 2 3 4 5</p></td>
<td><p>Le differenze e indice archiviati nella copia shadow: 3’</p></td>
</tr>
</tbody>
</table>

**Tabella 2**   il metodo di reindirizzamento-on-write di creazione di copie shadow

Analogamente al metodo di copia su scrittura, il metodo di reindirizzamento in scrittura è un metodo rapido per la creazione di una copia shadow, poiché copia solo le modifiche ai dati. I blocchi copiati nell'area diff possono essere combinati con i dati non modificati nel volume originale per creare una copia completa e aggiornata dei dati. Se sono presenti numerose richieste dei / o di lettura, il metodo di reindirizzamento in scrittura può diventare costoso.

## <a name="shadow-copy-providers"></a>Provider di copie shadow

Esistono due tipi di provider di copie shadow: provider basati su hardware e i provider basati su software. È inoltre disponibile un provider di sistema, che è un fornitore di software che viene incorporato nel sistema operativo Windows.

### <a name="hardware-based-providers"></a>Provider basati su hardware

Atto di provider copia shadow basato su hardware come interfaccia tra il servizio Copia Shadow del Volume e il livello di hardware da usare in combinazione con un adattatore di archiviazione di hardware o un controller. Le operazioni di creazione e gestione la copia shadow viene eseguita dall'array di archiviazione.

I provider hardware hanno sempre la copia shadow di un LUN intero, ma il servizio Copia Shadow del Volume espone solo la copia shadow del volume o i volumi che sono stati richiesti.

Un provider di copie shadow basato su hardware viene utilizzato il servizio Copia Shadow del Volume di funzionalità che definisce il punto nel tempo, consente la sincronizzazione dei dati, gestisce la copia shadow e fornisce un'interfaccia comune con applicazioni di backup. Tuttavia, il servizio Copia Shadow del Volume non specifica il meccanismo sottostante mediante il quale il provider basato su hardware genera e gestisce le copie shadow.

### <a name="software-based-providers"></a>Provider basati su software

Provider di copie shadow basata sul software in genere intercetta e processo di lettura e scrittura richieste i/o in un livello software tra il file system e il software di gestione del volume.

Questi provider vengono implementati come un componente DLL in modalità utente e i driver in modalità kernel almeno un dispositivo, in genere un driver di filtro di archiviazione. A differenza dei provider basati su hardware, provider basati su software di creare copie shadow a livello di software, non a livello di hardware.

Un provider di copie shadow in base al software necessario mantenere una visualizzazione "point-in-time" di un volume grazie all'accesso a un set di dati che può essere utilizzato per ricreare lo stato del volume prima dell'ora di creazione copia shadow. Un esempio è la tecnica di copy-on-write del provider di sistema. Tuttavia, il servizio Copia Shadow del Volume non genera restrizioni su quali tecnica usano i provider basati su software per creare e gestire le copie shadow.

Un provider di software è applicabile a una vasta gamma di piattaforme di archiviazione di un provider di servizi basati su hardware e dovrebbe funzionare altrettanto bene con volumi logici o i dischi di base. (Un volume logico è un volume che viene creato tramite la combinazione di spazio disponibile da due o più dischi). A differenza delle copie shadow hardware, fornitori di software utilizzano le risorse di sistema operativo per mantenere la copia shadow.

Per altre informazioni sui dischi di base, vedere [quali sono i volumi e dischi di base?](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) su TechNet.

### <a name="system-provider"></a>Provider di sistema

Un provider di copie shadow, il provider di sistema, viene fornito nel sistema operativo Windows. Sebbene un provider predefinito viene fornito in Windows, altri fornitori di soluzioni sono gratuite fornire implementazioni che sono ottimizzate per le applicazioni software e hardware di archiviazione.

Per mantenere la visualizzazione "point-in-time" di un volume che è contenuto in una copia shadow, il provider di sistema utilizza una tecnica di copy-on-write. Le copie dei blocchi nel volume che sono stati modificati dopo l'inizio della creazione di copie shadow vengono archiviate in un'area di archiviazione copia shadow.

Il provider di sistema può esporre il volume di produzione, che può essere scritti e letti dal normalmente. Quando è necessaria la copia shadow, si applica in modo logico le differenze per i dati nel volume di produzione per esporre la copia shadow completo.

Per il provider di sistema, l'area di archiviazione copia shadow deve essere in un volume NTFS. Il volume copia shadow non è necessario essere un volume NTFS, ma almeno un volume montato nel sistema deve essere un volume NTFS.

I file dei componenti che costituiscono il provider di sistema sono swprv.dll e Volsnap.

### <a name="in-box-vss-writers"></a>Nella casella di VSS Writer

Il sistema operativo Windows include un set di writer VSS che sono responsabili per l'enumerazione dei dati che sono richiesta da varie funzionalità di Windows.

Per altre informazioni su questi writer, vedere i siti Web Microsoft seguenti:

  - [Nella casella di VSS Writer](http://go.microsoft.com/fwlink/?linkid=180895) (http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [Nuova finestra In VSS Writer per Windows Server 2008 e Windows Vista SP1](http://go.microsoft.com/fwlink/?linkid=180896) (http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Nuova finestra In VSS Writer per Windows Server 2008 R2 e Windows 7](http://go.microsoft.com/fwlink/?linkid=180897) (http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>Utilizzo copie Shadow

Oltre al backup le informazioni sullo stato dei dati e di sistema delle applicazioni, le copie shadow sono utilizzabile per numerosi motivi, inclusi i seguenti:

  - Il ripristino delle unità logiche (LUN risincronizzazione e lo scambio di LUN)  
      
  - Il ripristino di file singoli (copie Shadow per cartelle condivise)  
      
  - Data mining usando le copie shadow trasportabili  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Il ripristino delle unità logiche (LUN risincronizzazione e lo scambio di LUN)

In Windows Server 2008 R2 e Windows 7, i richiedenti VSS possono utilizzare una funzionalità di provider del copia shadow hardware chiamata risincronizzazione LUN (o "Risincronizzazione LUN"). Si tratta di uno schema di recupero rapido che consente a un amministratore dell'applicazione ripristinare i dati da una copia shadow per il LUN originale o a un nuovo LUN.

La copia shadow può essere un clone completo o una copia shadow di backup differenziali. In entrambi i casi, al termine dell'operazione di risincronizzazione, il LUN di destinazione avrà lo stesso contenuto della copia shadow LUN. Durante l'operazione di risincronizzazione, la matrice esegue una copia a livello di blocco della copia shadow per il LUN di destinazione.


> [!NOTE]
> La copia shadow deve essere una copia shadow trasportabili hardware. 
<br>


La maggior parte delle matrici consentono operazioni dei / o di produzione riprendere subito dopo l'inizio dell'operazione di risincronizzazione. Mentre l'operazione di risincronizzazione è in corso, leggere le richieste vengono reindirizzate alla copia shadow LUN e richieste di scrittura per il LUN di destinazione. In questo modo le matrici di recuperare set di dati molto grandi e riprendere le normali operazioni dopo alcuni secondi.

La risincronizzazione LUN è diversa da LUN lo scambio. Lo scambio di LUN è uno scenario di ripristino rapido che VSS ha supportato da Windows Server 2003 SP1. In uno scambio LUN, la copia shadow è importata e convertita in un volume di lettura / scrittura. La conversione è un'operazione irreversibile e il volume e al LUN sottostante non può essere controllato con le API VSS in seguito. L'elenco seguente descrive il modo in cui la risincronizzazione LUN confronta con LUN scambio:

  - Risincronizzazione di LUN, la copia shadow non viene modificata, quindi può essere usato più volte. In sostituzione di LUN, la copia shadow è utilizzabile solo una volta per un ripristino. Per gli amministratori più attenti alla protezione, questo è importante. Quando viene usata la risincronizzazione di LUN, il richiedente può ripetere l'operazione di ripristino intero se qualcosa non funziona la prima volta.  
      
  - Al termine dello scambio LUN, la copia shadow LUN viene utilizzata per le richieste dei / o di produzione. Per questo motivo, la copia shadow LUN deve usare la stessa qualità di spazio di archiviazione come il numero di unità LOGICA di produzione originale per garantire che le prestazioni non sono interessata al termine dell'operazione di ripristino. Se invece viene utilizzata la risincronizzazione di LUN, il provider hardware possa mantenere la copia shadow nella risorsa di archiviazione meno costosa rispetto all'archiviazione di produzione di qualità.  
      
  - Se la destinazione LUN non può essere utilizzato e deve essere ricreata, lo scambio di LUN potrebbe essere più vantaggioso perché non richiede una LUN di destinazione.  
      


> [!WARNING]
> Tutte le operazioni elencate sono operazioni a livello di LUN. Se si prova a ripristinare un volume specifico usando la risincronizzazione di LUN, involontariamente si intende ripristinare tutti gli altri volumi che condividono il LUN. 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Il ripristino di file singoli (copie Shadow per cartelle condivise)

Le copie shadow per cartelle condivise utilizza il servizio Copia Shadow del Volume per fornire point-in-time copie dei file che si trovano su una risorsa di rete condivisa, ad esempio un file server. Con le copie Shadow per cartelle condivise, gli utenti possono ripristinare rapidamente i file eliminati o modificati che sono archiviati nella rete. Poiché questa operazione viene eseguita senza l'assistenza dell'amministratore, le copie Shadow per cartelle condivise può aumentare la produttività e ridurre i costi amministrativi.

Per altre informazioni sulle copie Shadow per cartelle condivise, vedere [copie Shadow per cartelle condivise](http://go.microsoft.com/fwlink/?linkid=180898) (http://go.microsoft.com/fwlink/?LinkId=180898) su TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Data mining usando le copie shadow trasportabili

Con un provider hardware che è progettato per l'uso con il servizio Copia Shadow del Volume, è possibile creare copie shadow trasportabili che possono essere importate nel server all'interno del sottosistema stesso (ad esempio, una rete SAN). Queste copie shadow sono utilizzabile per il seeding di un ambiente di produzione o di test di installazione con i dati di sola lettura per il data mining.

Con il servizio Copia Shadow del Volume e un array di archiviazione con un provider hardware che è progettato per l'uso con il servizio Copia Shadow del Volume, è possibile creare una copia shadow del volume di dati di origine in un server e quindi importare la copia shadow in un altro server  (o eseguire il backup nello stesso server). Questo processo viene eseguito in pochi minuti, indipendentemente dalle dimensioni dei dati. Il processo di trasporto avviene attraverso una serie di passaggi che utilizzano un richiedente copia shadow (applicazione di gestione dell'archiviazione) che supporta le copie shadow trasportabili.

## <a name="to-transport-a-shadow-copy"></a>Per il trasporto di una copia shadow

1.  Creare una copia shadow trasportabili dei dati di origine in un server.

2.  Importare la copia shadow in un server che è connesso alla rete SAN (è possibile importare in un altro server o il server stesso).

3.  I dati sono ora pronti per essere usato.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figura 3**   creazione di copie Shadow e trasporto tra due server


> [!NOTE]
> Una copia shadow trasportabili che viene creata in Windows Server 2003 non può essere importata in un server che esegue Windows Server 2008 o Windows Server 2008 R2. Una copia shadow trasportabili che è stata creata in Windows Server 2008 o Windows Server 2008 R2 non può essere importata in un server che esegue Windows Server 2003. Tuttavia, una copia shadow creata in Windows Server 2008 che può essere importata in un server che esegue Windows Server 2008 R2 e viceversa. 
<br>


Le copie shadow sono di sola lettura. Se si vuole convertire una copia shadow in una LUN di lettura/scrittura, è possibile usare un'applicazione di gestione archiviazione basata su Servizio dischi virtuali (inclusi alcuni richiedenti) oltre il servizio Copia Shadow del Volume. Con questa applicazione, è possibile rimuovere la copia shadow dalla gestione di servizio Copia Shadow del Volume e convertirlo in una LUN di lettura/scrittura.

Trasporto del servizio Copia Shadow volume è una soluzione avanzata su computer che eseguono Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 o Windows Server 2008 R2. Funziona solo se è disponibile un provider hardware nell'array di archiviazione. Trasporto di copia shadow è utilizzabile per numerosi scopi, tra cui i backup su nastro, i dati di data mining e il test.

## <a name="frequently-asked-questions"></a>Domande frequenti

Queste domande frequenti sui volumi Shadow copia Service (VSS) per gli amministratori di sistema. Per informazioni sulle interfacce di programmazione dell'applicazione VSS, vedere [il servizio Copia Shadow del Volume](http://go.microsoft.com/fwlink/?linkid=180899) (http://go.microsoft.com/fwlink/?LinkId=180899) nella libreria Windows Developer Center.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>Quando è stato introdotto il servizio Copia Shadow del Volume? Su quali versioni di sistema operativo Windows è disponibile?

VSS è stato introdotto in Windows XP. È disponibile in Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 e Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>Che cos'è la differenza tra una copia shadow e un backup?

Nel caso di un backup del disco rigido, la copia shadow creata è anche il backup. I dati possono essere copiati disattivata la copia shadow per il ripristino o la copia shadow può essere utilizzata per uno scenario di ripristino rapido, ad esempio, la risincronizzazione di LUN o lo scambio di LUN.

Quando i dati viene copiati dalla copia shadow su nastro o altro supporto rimovibile, il contenuto archiviato nel supporto costituisce il backup. La copia shadow stesso può essere eliminata dopo che i dati vengono copiati da quest'ultimo.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>Che cos'è il volume di dimensioni più grande che supporta servizio Copia Shadow del Volume?

Il servizio Copia Shadow del volume supporta dimensioni di volume fino a 64 TB.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>È stato eseguito un backup in Windows Server 2008. È possibile ripristinarlo quindi sul Windows Server 2008 R2?

Dipende dal software di backup è stato usato. La maggior parte dei programmi di backup supportano questo scenario per i dati, ma non per i backup dello stato del sistema.

Copie shadow vengono creati in una di queste versioni di Windows possono essere utilizzate in altro.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>È stato eseguito un backup in Windows Server 2003. È possibile ripristinarlo quindi sul Windows Server 2008?

Dipende dal software di backup che è stato usato. Se si crea una copia shadow in Windows Server 2003, è possibile usare in Windows Server 2008. Inoltre, se si crea una copia shadow in Windows Server 2008, è possibile ripristinare, in Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>Come è possibile disattivare VSS?

È possibile disabilitare il servizio Copia Shadow del Volume usando Microsoft Management Console. Tuttavia, è consigliabile non eseguire questa operazione. La disabilitazione di VSS negativamente influisce su qualsiasi software in uso che dipende da esso, ad esempio Windows Server Backup e ripristino configurazione di sistema.

Per altre informazioni, vedere i seguenti siti Web Microsoft TechNet:

  - [Ripristino del sistema](http://go.microsoft.com/fwlink/?linkid=157113) (http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Windows Server Backup](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>È possibile escludere i file da una copia shadow per risparmiare spazio?

VSS è progettato per creare copie shadow di interi volumi. I file temporanei, ad esempio i file di paging, automaticamente vengono omessi dalle copie shadow per risparmiare spazio.

Per escludere specifici file dalle copie shadow, usare la chiave del Registro di sistema seguente: **FilesNotToSnapshot**.


> [!NOTE]
> Il <STRONG>FilesNotToSnapshot</STRONG> chiave del Registro di sistema dovrà essere utilizzato solo dalle applicazioni. Gli utenti che tentano di utilizzarlo verificherà le limitazioni, ad esempio il seguente:
> <br>
> <UL>
> <LI>Non è possibile eliminare i file da una copia shadow creata in un Server Windows usando la funzionalità di versioni precedenti.<BR><BR>
> <LI>Non è possibile eliminare i file dalle copie shadow per cartelle condivise.<BR><BR>
> <LI>È possibile eliminare i file da una copia shadow creata usando il <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">Diskshadow</a> utility, ma non è possibile eliminare file da una copia shadow creata tramite il <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">Vssadmin</a> utilità.<BR><BR>
> <LI>I file vengono eliminati da una copia shadow in un modo più efficiente. Ciò significa che non sono garantiti da eliminare.<BR><BR></LI></UL>


Per altre informazioni, vedere [esclusione di file dalle copie Shadow](http://go.microsoft.com/fwlink/?linkid=180904) (http://go.microsoft.com/fwlink/?LinkId=180904) su MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Il programma di backup non Microsoft non è riuscita con un errore di VSS. Cosa posso fare?

Controllare la sezione di supporto del prodotto del sito Web della società che ha creato il programma di backup. Potrebbe esserci un aggiornamento del prodotto che è possibile scaricare e installare per risolvere il problema. In caso contrario, contattare il reparto di supporto del prodotto dell'azienda.

Gli amministratori di sistema possono utilizzare le informazioni sulla risoluzione dei problemi di VSS nel sito Web Microsoft TechNet Library per raccogliere informazioni di diagnostica sui problemi relativi a VSS.

Per altre informazioni, vedere [il servizio Copia Shadow del Volume](http://go.microsoft.com/fwlink/?linkid=180905) (http://go.microsoft.com/fwlink/?LinkId=180905) su TechNet.

### <a name="what-is-the-diff-area"></a>Che cos'è il "area diff"?

L'area di archiviazione copia shadow (o "area diff") è la posizione dove sono archiviati i dati per la copia shadow creata dal provider di software di sistema.

### <a name="where-is-the-diff-area-located"></a>Dove si trova l'area diff?

L'area diff può trovarsi in qualsiasi volume locale. Tuttavia, deve trovarsi in un volume NTFS con spazio sufficiente per archiviarli.

### <a name="how-is-the-diff-area-location-determined"></a>Come viene determinato il percorso di area diff?

In questo ordine, per determinare il percorso di area diff, vengono valutati i criteri seguenti:

  - Se un volume contiene già una copia shadow esistente, viene utilizzato tale percorso.  
      
  - Se è presente un'associazione manuale preconfigurata tra il volume originale e il percorso del volume copia shadow, viene usato tale percorso.  
      
  - Se i due criteri precedenti non viene fornito un percorso, il servizio Copia shadow sceglie un percorso basato sullo spazio libero disponibile. Se più di un volume è in corso la copia shadow, il servizio Copia shadow consente di creare un elenco delle posizioni possibili snapshot in base alla dimensione di spazio libero, in ordine decrescente. Il numero di posizioni specificato è uguale al numero dei volumi eseguita la copia shadow.  
      
  - Se il volume viene creata una copia shadow è una delle posizioni possibili, viene creata un'associazione locale. In caso contrario, viene creata un'associazione con il volume con più spazio disponibile.  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>VSS può creare copie shadow dei volumi non NTFS?

Sì. Tuttavia, è possono eseguire copie shadow permanente solo per i volumi NTFS. Inoltre, almeno un volume montato nel sistema deve essere un volume NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>Che cos'è il numero massimo di copie shadow che è possibile creare contemporaneamente?

Il numero massimo di volumi replicati mediante copiata shadow in un set di copie shadow singola è 64. Si noti che questo non è identico al numero di copie shadow.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>Qual è il numero massimo di copie shadow software creato dal provider di sistema che è possibile gestire per un volume?

Il numero massimo è di copie shadow per ogni volume è 512. Tuttavia, per impostazione predefinita è solo possibile mantenere 64 copie shadow che vengono utilizzate dalla funzionalità di copie Shadow di cartelle condivise. Per modificare il limite per la funzionalità copie Shadow di cartelle condivise, usare la chiave del Registro di sistema seguente: **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>Come è possibile controllare lo spazio utilizzato per spazio di archiviazione copia shadow?

Tipo di **vssadmin ridimensionare shadowstorage** comando.

Per altre informazioni, vedere [Vssadmin ridimensionare shadowstorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) su TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>Cosa accade quando ho esaurito lo spazio?

Vengono eliminate le copie shadow per volume, che iniziano con la copia shadow meno recente.

## <a name="volume-shadow-copy-service-tools"></a>Strumenti di servizio Copia Shadow volume

Il sistema operativo Windows offre gli strumenti seguenti per l'uso di VSS:

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

DiskShadow è un richiedente VSS che è possibile usare per gestire tutti gli hardware e software snapshot che è possibile avere in un sistema. DiskShadow include comandi simile al seguente:

  - **list**: Elenca i writer VSS, VSS provider e le copie shadow  
      
  - **creare**: Crea una nuova copia shadow  
      
  - **import**: Importa una copia shadow trasportabili  
      
  - **expose**: Espone una copia shadow permanente (come una lettera di unità, ad esempio)  
      
  - **revert**: Ripristina un volume in una copia shadow specificata  
      

Questo strumento è destinato all'uso per i professionisti IT, ma gli sviluppatori potrebbero inoltre risultare utile durante il test di VSS writer del provider servizio Copia shadow.

DiskShadow è disponibile solo nei sistemi operativi Windows Server. Non è disponibile nei sistemi operativi client Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin consente di creare, eliminare ed elencare le informazioni sulle copie shadow. Può essere utilizzato anche per ridimensionare l'area di archiviazione copia shadow ("area diff").

VssAdmin include comandi simile al seguente:

  - **creare ombreggiatura**: Crea una nuova copia shadow  
      
  - **eliminare le ombreggiature**: Elimina le copie shadow  
      
  - **elencare i provider**: Elenca tutti i provider VSS registrati  
      
  - **Elenca i writer**: Elenca tutti sottoscritti VSS Writer  
      
  - **ridimensionare shadowstorage**: Modifica la dimensione massima dell'area di archiviazione copia shadow  
      

VssAdmin utilizzabile solo per amministrare le copie shadow create dal provider di software di sistema.

VssAdmin è disponibile nel client Windows e le versioni del sistema operativo Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Chiavi del Registro di sistema al servizio Copia Shadow volume

Le chiavi del Registro di sistema seguenti sono disponibili per l'uso con VSS:

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

Questa chiave viene usata per specificare quali utenti hanno accesso alle copie shadow.

Per altre informazioni, vedere le seguenti voci nel sito Web MSDN:

  - [Considerazioni sulla sicurezza per i writer](http://go.microsoft.com/fwlink/?linkid=157739) (http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [Considerazioni sulla sicurezza per i richiedenti](http://go.microsoft.com/fwlink/?linkid=180908) (http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

Questa chiave specifica il numero massimo di copie shadow accessibili dal client che possono essere archiviati in ogni volume del computer. Copie shadow accessibili dal client vengono usate dalle copie Shadow per cartelle condivise.

Per altre informazioni, vedere la voce seguente sul sito Web MSDN:

**MaxShadowCopies** sotto [le chiavi del Registro di sistema per il Backup e ripristino](http://go.microsoft.com/fwlink/?linkid=180909) (http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Questa chiave specifica la dimensione minima iniziale, in MB, dell'area di archiviazione copia shadow.

Per altre informazioni, vedere la voce seguente sul sito Web MSDN:

**MinDiffAreaFileSize** sotto [le chiavi del Registro di sistema per il Backup e ripristino](http://go.microsoft.com/fwlink/?linkid=180910) (http://go.microsoft.com/fwlink/?LinkId=180910)

`##`& ' Supportate le versioni del sistema operativo

Nella tabella seguente elenca le versioni di sistema operativo minimo supportato per le funzionalità VSS.


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
<td><p><strong>FilesNotToSnapshot</strong> chiave del Registro di sistema</p></td>
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
<td>Questa è la possibilità di importare una copia shadow più volte. Solo un'importazione operazione può essere eseguita in un momento.
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
<td><p>Copie shadow per cartelle condivise</p></td>
<td><p>Nessuno supportato</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Copie shadow trasportabili recuperato automaticamente</p></td>
<td><p>Nessuno supportato</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sessioni simultanee di backup (fino a 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Sessione di ripristino singolo simultanea con i backup</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 with SP2</p></td>
</tr>
<tr class="even">
<td><p>Fino a 8 Ripristino configurazione di sessioni simultanee con i backup</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Vedere anche

[Servizio Copia Shadow del volume nel centro per sviluppatori Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)