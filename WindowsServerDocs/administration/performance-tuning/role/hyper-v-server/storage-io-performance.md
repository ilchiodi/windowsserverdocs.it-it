---
title: Prestazioni di I/O di archiviazione Hyper-V
description: Considerazioni sulle prestazioni di i/o di archiviazione nell'ottimizzazione delle prestazioni di Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 7c5a7b667f24ee929a80010dc51508033f991ed5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370056"
---
# <a name="hyper-v-storage-io-performance"></a>Prestazioni di I/O di archiviazione Hyper-V

In questa sezione vengono descritte le diverse opzioni e considerazioni per l'ottimizzazione delle prestazioni di I/O di archiviazione in una macchina virtuale. Il percorso di I/O di archiviazione si estende dallo stack di archiviazione Guest, tramite il livello di virtualizzazione dell'host, allo stack di archiviazione host e quindi al disco fisico. Di seguito sono illustrate le procedure di ottimizzazione possibili in ognuna di queste fasi.

## <a name="virtual-controllers"></a>Controller virtuali

Hyper-V offre tre tipi di controller virtuali: IDE, SCSI e schede bus host virtuali (HBA).

## <a name="ide"></a>IDE

I controller IDE espongono dischi IDE alla macchina virtuale. Il controller IDE è emulato ed è l'unico controller disponibile per le macchine virtuali guest che eseguono la versione precedente di Windows senza la Integration Services della macchina virtuale. L'I/O su disco eseguito utilizzando il driver del filtro IDE fornito con la macchina virtuale Integration Services è significativamente migliore rispetto alle prestazioni di I/O del disco fornite con il controller IDE emulato. È consigliabile usare i dischi IDE solo per i dischi del sistema operativo perché presentano limitazioni delle prestazioni a causa delle dimensioni massime di I/O che possono essere rilasciate a questi dispositivi.

## <a name="scsi-sas-controller"></a>SCSI (controller SAS)

I controller SCSI espongono i dischi SCSI alla macchina virtuale e ogni controller SCSI virtuale può supportare fino a 64 dispositivi. Per prestazioni ottimali, è consigliabile collegare più dischi a un singolo controller SCSI virtuale e creare controller aggiuntivi solo quando sono necessari per ridimensionare il numero di dischi connessi alla macchina virtuale. Il percorso SCSI non è emulato, che lo rende il controller preferito per qualsiasi disco diverso dal disco del sistema operativo. In realtà con le macchine virtuali di seconda generazione, è possibile usare solo il tipo di controller. Introdotta in Windows Server 2012 R2, questo controller viene segnalato come SAS per supportare VHDX condivisi.

## <a name="virtual-fibre-channel-hbas"></a>HBA Fibre Channel virtuali

È possibile configurare HBA Fibre Channel virtuali per consentire l'accesso diretto alle macchine virtuali ai Fibre Channel e ai LUN Fibre Channel over Ethernet (FCoE). I dischi Fibre Channel virtuali ignorano i file system NTFS nella partizione radice, riducendo l'utilizzo della CPU da parte dell'I/O di archiviazione.

Unità dati e unità di grandi dimensioni condivise tra più macchine virtuali (per gli scenari di clustering guest) sono candidati principali per i dischi Fibre Channel virtuali.

Per i dischi Fibre Channel virtuali sono necessarie una o più Fibre Channel schede bus host (HBA) da installare nell'host. Ogni host HBA è necessario per usare un driver HBA che supporta le funzionalità Fibre Channel/NPIV virtuali di Windows Server 2016. L'infrastruttura SAN deve supportare NPIV e le porte HBA usate per la Fibre Channel virtuale devono essere configurate in una topologia di Fibre Channel che supporti NPIV.

Per ottimizzare la velocità effettiva negli host installati con più di una HBA, è consigliabile configurare più HBA virtuali all'interno della macchina virtuale Hyper-V (è possibile configurare fino a quattro schede HBA per ogni macchina virtuale). Hyper-V tenterà automaticamente di bilanciare le schede HBA virtuali per ospitare HBA che accedono alla stessa rete SAN virtuale.

## <a name="virtual-disks"></a>Dischi virtuali

I dischi possono essere esposti alle macchine virtuali tramite i controller virtuali. Questi dischi potrebbero essere dischi rigidi virtuali che sono astrazioni di file di un disco o un disco pass-through nell'host.

## <a name="virtual-hard-disks"></a>Dischi rigidi virtuali

Sono disponibili due formati di disco rigido virtuale, VHD e VHDX. Ognuno di questi formati supporta tre tipi di file di disco rigido virtuale.

## <a name="vhd-format"></a>Formato VHD

Il formato VHD è l'unico formato di disco rigido virtuale supportato da Hyper-V nelle versioni precedenti. Introdotta in Windows Server 2012, il formato VHD è stato modificato per consentire un migliore allineamento, il che comporta prestazioni significativamente migliori nei nuovi dischi di settore di grandi dimensioni.

Qualsiasi nuovo VHD creato in Windows Server 2012 o versioni successive dispone dell'allineamento ottimale di 4 KB. Questo formato allineato è completamente compatibile con i sistemi operativi Windows Server precedenti. Tuttavia, la proprietà di allineamento verrà interruppe per le nuove allocazioni dai parser che non sono compatibili con l'allineamento a 4 KB (ad esempio un parser VHD da una versione precedente di Windows Server o un parser non Microsoft).

Qualsiasi disco rigido virtuale spostato da una versione precedente non viene convertito automaticamente in questo nuovo formato VHD migliorato.

Per eseguire la conversione in un nuovo formato VHD, eseguire il comando seguente di Windows PowerShell:

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

È possibile controllare la proprietà di allineamento per tutti i dischi rigidi virtuali nel sistema e deve essere convertita nell'allineamento ottimale di 4 KB. Si crea un nuovo disco rigido virtuale con i dati del disco rigido virtuale originale usando l'opzione **Crea da origine** .

Per verificare l'allineamento utilizzando Windows PowerShell, esaminare la linea di allineamento, come illustrato di seguito:

``` syntax
Get-VHD –Path E:\vms\testvhd\test.vhd

Path                    : E:\vms\testvhd\test.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69245440
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 10
Alignment               : 0
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

Per verificare l'allineamento utilizzando Windows PowerShell, esaminare la linea di allineamento, come illustrato di seguito:

``` syntax
Get-VHD –Path E:\vms\testvhd\test-converted.vhd

Path                    : E:\vms\testvhd\test-converted.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69369856
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 0
Alignment               : 1
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

## <a name="vhdx-format"></a>Formato VHDX

VHDX è un nuovo formato di disco rigido virtuale introdotto in Windows Server 2012, che consente di creare dischi virtuali resilienti a prestazioni elevate fino a 64 terabyte. I vantaggi di questo formato includono:

-   Supporto per la capacità di archiviazione su disco rigido virtuale fino a 64 terabyte.

-   Protezione contro il danneggiamento dei dati in caso di interruzioni dell'alimentazione grazie alla registrazione degli aggiornamenti nelle strutture dei metadati VHDX.

-   Possibilità di archiviare metadati personalizzati su un file, che un utente potrebbe voler registrare, ad esempio la versione del sistema operativo o le patch applicate.

Il formato VHDX offre inoltre i seguenti vantaggi in merito alle prestazioni:

-   Migliore allineamento del formato di disco rigido virtuale per assicurare risultati migliori sui dischi con settori di grandi dimensioni.

-   Dimensioni dei blocchi più grandi per i dischi dinamici e differenziali, che consentono a questi dischi di adattamento alle in base alle esigenze del carico di lavoro.

-   disco virtuale con settore logico da 4 KB che consente un miglioramento delle prestazioni quando viene usato da applicazioni e carichi di lavoro progettati per settori di 4 KB.

-   Efficienza nella rappresentazione dei dati, che comporta dimensioni di file più ridotte e consente al dispositivo di archiviazione fisica sottostante di recuperare lo spazio inutilizzato. Per il trim sono necessari i dischi pass-through o SCSI e i componenti hardware compatibili con trim.

Quando si esegue l'aggiornamento a Windows Server 2016, è consigliabile convertire tutti i file VHD nel formato VHDX a causa di questi vantaggi. L'unico scenario in cui avrebbe senso mantenere i file nel formato VHD è quando una macchina virtuale può essere spostata in una versione precedente di Hyper-V che non supporta il formato VHDX.

## <a name="types-of-virtual-hard-disk-files"></a>Tipi di file di disco rigido virtuale

Sono disponibili tre tipi di file VHD. Le sezioni seguenti rappresentano le caratteristiche di prestazioni e i compromessi tra i tipi.

Per quanto riguarda la selezione di un tipo di file VHD, è necessario prendere in considerazione le indicazioni seguenti:

-   Quando si usa il formato VHD, si consiglia di usare il tipo fisso perché offre migliori caratteristiche di resilienza e prestazioni rispetto agli altri tipi di file VHD.

-   Quando si usa il formato VHDX, è consigliabile usare il tipo dinamico perché offre garanzie di resilienza oltre al risparmio di spazio associato all'allocazione dello spazio solo quando è necessario.

-   È inoltre consigliabile usare il tipo fisso, indipendentemente dal formato, quando l'archiviazione sul volume di hosting non viene monitorata attivamente per assicurarsi che sia presente spazio su disco sufficiente quando si espande il file VHD in fase di esecuzione.

-   Gli snapshot di una macchina virtuale creano un disco rigido virtuale differenze per archiviare le scritture nei dischi. Avere solo pochi snapshot può elevare l'utilizzo della CPU delle operazioni di I/O di archiviazione, ma potrebbe non influire negativamente sulle prestazioni, tranne nei carichi di lavoro del server a elevato utilizzo di I/O. Tuttavia, la presenza di una catena di snapshot di grandi dimensioni può influire in modo significativo sulle prestazioni perché la lettura dal disco rigido virtuale può richiedere la verifica dei blocchi richiesti in molti dischi rigidi virtuali differenze. Mantenere brevi le catene di snapshot è importante per garantire prestazioni ottimali di I/O su disco.

## <a name="fixed-virtual-hard-disk-type"></a>Tipo di disco rigido virtuale fisso

Lo spazio per il disco rigido virtuale viene innanzitutto allocato quando viene creato il file VHD. Questo tipo di file VHD è meno probabile che venga frammentato, riducendo la velocità effettiva di I/O quando un singolo I/O viene suddiviso in più I/O. Dispone del sovraccarico minimo della CPU dei tre tipi di file VHD poiché le letture e le Scritture non richiedono la ricerca del mapping del blocco.

## <a name="dynamic-virtual-hard-disk-type"></a>Tipo di disco rigido virtuale dinamico

Lo spazio per il disco rigido virtuale viene allocato su richiesta. I blocchi nel disco iniziano come blocchi non allocati e non sono supportati da alcuno spazio effettivo nel file. Quando un blocco viene scritto per la prima volta in, lo stack di virtualizzazione deve allocare spazio all'interno del file VHD per il blocco, quindi aggiornare i metadati. Ciò aumenta il numero di i/o del disco necessari per la scrittura e aumenta l'utilizzo della CPU. Letture e scritture nei blocchi esistenti comportano l'accesso al disco e il sovraccarico della CPU durante la ricerca del mapping dei blocchi nei metadati.

## <a name="differencing-virtual-hard-disk-type"></a>Tipo di disco rigido virtuale differenze

Il disco rigido virtuale punta a un file VHD padre. Le scritture nei blocchi non scritti per comportare l'allocazione di spazio nel file VHD, come per un VHD ad espansione dinamica. Se il blocco è stato scritto, le letture vengono gestite dal file VHD. In caso contrario, vengono serviti dal file VHD padre. In entrambi i casi, i metadati vengono letti per determinare il mapping del blocco. Le operazioni di lettura e scrittura in questo VHD possono richiedere più CPU e ottenere un numero maggiore di operazioni di I/o rispetto a un file VHD fisso.

## <a name="block-size-considerations"></a>Considerazioni sulle dimensioni del blocco

Le dimensioni del blocco possono avere un notevole effetto sulle prestazioni. È ottimale per abbinare le dimensioni del blocco ai modelli di allocazione del carico di lavoro che utilizza il disco. Se, ad esempio, un'applicazione viene allocata in blocchi di 16 MB, sarà ottimale avere una dimensione del blocco del disco rigido virtuale di 16 MB. Una dimensione di blocco &gt;di 2 MB è possibile solo nei dischi rigidi virtuali con il formato VHDX. Una dimensione di blocco maggiore rispetto al modello di allocazione per un carico di lavoro di I/O casuale aumenterà significativamente l'utilizzo dello spazio nell'host.

## <a name="sector-size-implications"></a>Implicazioni sulle dimensioni del settore

La maggior parte del settore del software è dipendono da settori disco di 512 byte, ma lo standard sta per essere spostato in settori di dischi da 4 KB. Per ridurre i problemi di compatibilità che potrebbero derivare da una modifica nelle dimensioni del settore, i fornitori di dischi rigidi introducono una dimensione transitoria denominata unità di emulazione 512 (512e).

Queste unità di emulazione offrono alcuni dei vantaggi offerti da unità native del settore disco da 4 KB, ad esempio un miglioramento dell'efficienza del formato e uno schema migliorato per i codici di correzione degli errori (ECC). Presentano un minor numero di problemi di compatibilità che si verificano esponendo una dimensione di settore di 4 KB sull'interfaccia disco.

## <a name="support-for-512e-disks"></a>Supporto per dischi 512e

Un disco 512e può eseguire operazioni di scrittura solo in termini di settore fisico, ovvero non può scrivere direttamente un settore 512byte emesso. Il processo interno nel disco che rende possibili queste Scritture segue questa procedura:

-   Il disco legge il settore fisico a 4 KB nella propria cache interna, che contiene il settore logico a 512 byte a cui si fa riferimento nella scrittura.

-   I dati nel buffer a 4 KB vengono modificati in modo da includere il settore a 512 byte aggiornato.

-   Il disco esegue una scrittura del buffer a 4 KB aggiornato nel proprio settore fisico sul disco.

Questo processo è denominato lettura-modifica-scrittura (RMW). L'effetto complessivo sulle prestazioni del processo RMW dipende dai carichi di lavoro. Il processo RMW causa un calo delle prestazioni nei dischi rigidi virtuali per i motivi seguenti:

-   I dischi rigidi virtuali dinamici e differenze presentano una bitmap settore a 512 byte prima del payload dei dati. Inoltre, il piè di pagina, l'intestazione e i localizzatori padre sono allineati a un settore a 512 byte. È comune che il driver del disco rigido virtuale rilascia i comandi di scrittura a 512 byte per aggiornare queste strutture, dando come risultato il processo RMW descritto in precedenza.

-   Le applicazioni in genere rilasciano letture e scritture in multipli di 4 KB di dimensioni (la dimensione predefinita del cluster NTFS). Poiché è presente una bitmap settore a 512 byte prima del blocco del payload dei dati di dischi rigidi virtuali dinamici e differenze, i blocchi da 4 KB non sono allineati al limite fisico di 4 KB. Nella figura seguente viene illustrato un blocco VHD da 4 KB (evidenziato) non allineato con un limite fisico di 4 KB.

![blocco VHD da 4 KB](../../media/perftune-guide-vhd-4kb-block.png)

Ogni comando Write di 4 KB emesso dal parser corrente per aggiornare i dati del payload restituisce due letture per due blocchi sul disco, che vengono quindi aggiornati e successivamente riscritti nei due blocchi del disco. Hyper-V in Windows Server 2016 attenua alcuni degli effetti sulle prestazioni sui dischi 512e nello stack VHD, preparando le strutture citate in precedenza per l'allineamento a limiti di 4 KB nel formato VHD. In questo modo si evita l'effetto RMW quando si accede ai dati all'interno del file del disco rigido virtuale e quando si aggiornano le strutture dei metadati del disco rigido virtuale.

Come indicato in precedenza, i VHD copiati da versioni precedenti di Windows Server non verranno allineati automaticamente a 4 KB. È possibile convertirli manualmente per allinearli in modo ottimale utilizzando l'opzione **copia da** disco di origine disponibile nelle interfacce VHD.

Per impostazione predefinita, i VHD vengono esposti con una dimensione di settore fisico di 512 byte. Questa operazione viene eseguita per garantire che le applicazioni dipendenti dalle dimensioni del settore fisico non siano interessate quando l'applicazione e i dischi rigidi virtuali vengono spostati da una versione precedente di Windows Server.

Per impostazione predefinita, i dischi con il formato VHDX vengono creati con le dimensioni del settore fisico da 4 KB per ottimizzare i dischi regolari del profilo di prestazioni e i dischi di settore di grandi dimensioni. Per sfruttare al massimo i settori di 4 KB, è consigliabile usare il formato VHDX.

## <a name="support-for-native-4kb-disks"></a>Supporto per dischi nativi da 4 KB

Hyper-V in Windows Server 2012 R2 e versioni successive supportano dischi nativi da 4 KB. Tuttavia è comunque possibile archiviare il disco rigido virtuale in un disco nativo da 4 KB. Questa operazione viene eseguita implementando un algoritmo software RMW nel livello stack di archiviazione virtuale che converte le richieste di accesso e aggiornamento a 512 byte ai corrispondenti accessi e aggiornamenti a 4 KB.

Poiché il file VHD può esporsi solo come dischi di dimensioni del settore logico a 512 byte, è molto probabile che siano presenti applicazioni che emettono richieste di I/O a 512 byte. In questi casi, il livello RMW soddisferà queste richieste e causerà un calo delle prestazioni. Questo vale anche per un disco formattato con VHDX con una dimensione di settore logica di 512 byte.

È possibile configurare un file VHDX in modo che venga esposto come disco di dimensioni del settore logico da 4 KB e si tratta di una configurazione ottimale per le prestazioni quando il disco è ospitato su un dispositivo fisico nativo da 4 KB. È necessario prestare attenzione per garantire che il Guest e l'applicazione che usa il disco virtuale siano supportati dalle dimensioni del settore logico di 4 KB. La formattazione VHDX funzionerà correttamente in un dispositivo con dimensioni di settore logico da 4 KB.

## <a name="pass-through-disks"></a>Dischi pass-through

Il disco rigido virtuale in una macchina virtuale può essere mappato direttamente a un disco fisico o a un numero di unità logica (LUN), anziché a un file VHD. Il vantaggio è che questa configurazione Ignora la file system NTFS nella partizione radice, riducendo l'utilizzo della CPU da parte dell'I/O di archiviazione. Il rischio è che i dischi fisici o i LUN possono essere più difficili da spostare tra computer rispetto ai file VHD.

I dischi pass-through devono essere evitati a causa delle limitazioni introdotte negli scenari di migrazione delle macchine virtuali.

## <a name="advanced-storage-features"></a>Funzionalità di archiviazione avanzate

### <a name="storage-quality-of-service-qos"></a>QoS di archiviazione

A partire da Windows Server 2012 R2, Hyper-V offre la possibilità di impostare determinati parametri di qualità del servizio (QoS) per l'archiviazione nelle macchine virtuali. QoS di archiviazione consente di isolare le prestazioni di archiviazione in un ambiente multitenant e fornisce meccanismi di notifica per le situazioni in cui le prestazioni di I/O di archiviazione non soddisfano la soglia definita per garantire un'esecuzione efficiente dei carichi di lavoro delle macchine virtuali.

QoS di archiviazione consente di specificare un valore massimo per le operazioni di input/output al secondo (IOPS) per un disco rigido virtuale. Un amministratore può limitare l'I/O di archiviazione per impedire a un tenant di continuare a consumare una quantità di risorse di archiviazione eccessiva, con possibili ripercussioni negative su un altro tenant.

È anche possibile impostare un valore minimo di IOPS. L'amministratore riceverà una notifica quando il valore di IOPS per un determinato disco rigido virtuale scende sotto la soglia necessaria per assicurare prestazioni ottimali.

È stata aggiornata anche l'infrastruttura metrica delle macchine virtuali, con parametri di archiviazione che consentono all'amministratore di monitorare le prestazioni ed eseguire il chargeback dei parametri correlati.

I valori massimi e minimi sono specificati in termini di IOPS normalizzato, dove ogni 8 KB di dati viene conteggiato come un I/O.

Di seguito sono riportate alcune limitazioni:

-   Solo per i dischi virtuali

-   Il disco differenze non può avere un disco virtuale padre in un volume diverso

-   Replica: QoS per il sito di replica configurato separatamente dal sito primario

-   VHDX condiviso non è supportato

Per altre informazioni sulla qualità del servizio di archiviazione, vedere la pagina relativa alla [qualità del servizio di archiviazione per Hyper-V](https://technet.microsoft.com/library/dn282281.aspx).

### <a name="numa-io"></a>I/O NUMA

Windows Server 2012 e versioni successive supportano macchine virtuali di grandi dimensioni e qualsiasi configurazione di macchine virtuali di grandi dimensioni (ad esempio, una configurazione con Microsoft SQL Server in esecuzione con processori virtuali 64) richiederà anche la scalabilità in termini di velocità effettiva di I/O.

I seguenti miglioramenti chiave introdotti per la prima volta nello stack di archiviazione di Windows Server 2012 e Hyper-V forniscono le esigenze di scalabilità di I/O di macchine virtuali di grandi dimensioni:

-   Aumento del numero di canali di comunicazione creati tra i dispositivi Guest e lo stack di archiviazione host.

-   Meccanismo di completamento I/O più efficiente che interferisce con la distribuzione di interrupt tra i processori virtuali per evitare interruzioni di interprocessore dispendiose.

Introdotta in Windows Server 2012, sono presenti alcune voci del registro di sistema,\\situate\\in\\HKLM\\System\\CurrentControlSet Enum VMBus\\{Device ID} {Instance ID}\\StorChannel, che consente di modificare il numero di canali. Consentono inoltre di allineare i processori virtuali che gestiscono i completamenti di I/O alle CPU virtuali assegnate dall'applicazione come processori di I/O. Le impostazioni del registro di sistema sono configurate in base all'adapter sulla chiave hardware del dispositivo.

-   **ChannelCount (DWORD)** Numero totale di canali da usare, con un massimo di 16. Il valore predefinito è un limite massimo, ovvero il numero di processori virtuali/16.

-   **ChannelMask (QWord)** Affinità processori per i canali. Se non è impostato o è impostato su 0, per impostazione predefinita viene usato l'algoritmo di distribuzione del canale esistente usato per l'archiviazione normale o per i canali di rete. In questo modo si garantisce che i canali di archiviazione non siano in conflitto con i canali di rete.

### <a name="offloaded-data-transfer-integration"></a>Trasferimento dati l'integrazione con offload

Le attività di manutenzione cruciali per i dischi rigidi virtuali, ad esempio merge, spostamento e compattazione, dipendono dalla copia di grandi quantità di dati. Con il metodo corrente il processo di copia implica la lettura e la scrittura dei dati in posizioni diverse e può quindi richiedere molto tempo. Vengono inoltre utilizzate risorse di CPU e memoria nell'host, che potrebbero essere state utilizzate per il servizio di macchine virtuali.

I fornitori di reti di archiviazione (SAN, Storage Area Network) sono già al lavoro per offrire operazioni di copia quasi istantanea di grandi quantità di dati. Questa archiviazione è progettata per consentire al sistema superiore ai dischi di specificare lo spostamento di un set di dati specifico da una posizione a un'altra. Questa funzionalità hardware è nota come Trasferimento dati con offload.

Hyper-V in Windows Server 2012 e versioni successive supporta le operazioni di offload Trasferimento dati (ODX) in modo che queste operazioni possano essere passate dal sistema operativo guest all'hardware host. Ciò garantisce che il carico di lavoro possa usare l'archiviazione abilitata per ODX come se fosse in esecuzione in un ambiente non virtualizzato. Lo stack di archiviazione Hyper-V rilascia anche le operazioni ODX durante le operazioni di manutenzione per i dischi rigidi virtuali, ad esempio l'Unione di dischi e le metaoperazioni di migrazione dell'archiviazione in cui vengono spostati grandi quantità di dati

### <a name="unmap-integration"></a>Integrazione annullare

I file del disco rigido virtuale esistono come file in un volume di archiviazione e condividono lo spazio disponibile con altri file. Poiché le dimensioni di questi file tendono a essere di grandi dimensioni, lo spazio utilizzato può crescere rapidamente. La richiesta di una maggiore quantità di risorse di archiviazione fisica influiscono sul budget hardware IT. È importante ottimizzare il più possibile l'uso dell'archiviazione fisica.

Prima di Windows Server 2012, quando le applicazioni eliminano il contenuto all'interno di un disco rigido virtuale, che ha effettivamente abbandonato lo spazio di archiviazione del contenuto, lo stack di archiviazione di Windows nel sistema operativo guest e nell'host Hyper-V aveva limitazioni che impedivano questa operazione le informazioni vengono comunicate al disco rigido virtuale e al dispositivo di archiviazione fisica. Ciò ha impedito allo stack di archiviazione Hyper-V di ottimizzare l'utilizzo dello spazio da parte dei file di disco virtuale basati su VHD. Ha inoltre impedito al dispositivo di archiviazione sottostante di recuperare lo spazio occupato in precedenza dai dati eliminati.

A partire da Windows Server 2012, Hyper-V supporta le notifiche annullare, che consentono di rendere più efficienti i file VHDX per rappresentare i dati al suo interno. Ciò comporta dimensioni di file più ridotte e consente al dispositivo di archiviazione fisica sottostante di recuperare lo spazio inutilizzato.

Solo i controller SCSI specifici di Hyper-V, IDE illuminati e Fibre Channel virtuali consentono al comando annullare del Guest di raggiungere lo stack di archiviazione virtuale dell'host. Nei dischi rigidi virtuali solo i dischi virtuali formattati come VHDX supportano i comandi annullare dal Guest.

Per questi motivi, è consigliabile usare i file VHDX collegati a un controller SCSI quando non si usano dischi Fibre Channel virtuali.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura Hyper-V](architecture.md)

-   [Configurazione dei server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
