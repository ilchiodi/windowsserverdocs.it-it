---
title: Considerazioni sulle prestazioni dell'hardware del server
description: Considerazioni sulle prestazioni dell'hardware del server per Windows Server 2016
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 01/08/2018
ms.openlocfilehash: 9c012711dff3746587b4a04b31d9c23ebb7de4cd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370549"
---
# <a name="server-hardware-performance-considerations"></a>Considerazioni sulle prestazioni dell'hardware del server

La sezione seguente elenca gli elementi importanti da considerare quando si sceglie l'hardware del server. Attenendosi a queste linee guida è possibile evitare colli di bottiglia delle prestazioni che potrebbero influire sulle prestazioni del server.

## <a name="processor-recommendations"></a>Consigli per i processori

Scegliere processori a 64 bit per i server. I processori a 64 bit dispongono di molto più spazio degli indirizzi e sono obbligatori per Windows Server 2016. Non verranno fornite edizioni a 32 bit del sistema operativo, ma le applicazioni a 32 bit verranno eseguite nel sistema operativo Windows Server 2016 a 64 bit.

Per aumentare le risorse di elaborazione in un server, è possibile usare un processore dotato di core con frequenza maggiore oppure aumentare il numero di core del processore. Se è la CPU la risorsa limitante nel sistema, un core con una frequenza 2x offre in genere un miglioramento delle prestazioni maggiore rispetto a due core con una frequenza 1x.

La presenza di più core non garantisce un perfetto ridimensionamento lineare e il fattore di ridimensionamento può essere anche minore se è abilitato l'hyper-threading, che si basa sulla condivisione delle risorse dello stesso core fisico.


>[!Important]
> Associare e ridimensionare il sottosistema di memoria e I/O con le prestazioni della CPU e viceversa.

Non confrontare le frequenze della CPU di produttori e generazioni di processori diversi in quanto il confronto può essere un indicatore fuorviante della velocità.

Per Hyper-V, assicurarsi che il processore supporti la funzionalità SLAT (Second Level Address Translation). Questa funzionalità viene implementata come EPT (Extended Page Tables) da Intel e come NPT (Nested Page Tables) da AMD. È possibile verificarne la presenza usando SystemInfo.exe nel server.

## <a name="cache-recommendations"></a>Consigli per le cache

Scegliere cache di processore estese L2 o L3. Nelle architetture più recenti, ad esempio Haswell o Skylake, è presente una cache LLC (Last Level Cache) o una cache L4. Le cache più estese in genere offrono prestazioni migliori e spesso sono più determinanti rispetto a una frequenza di CPU non elaborata.

## <a name="memory-ram-and-paging-storage-recommendations"></a>Consigli per la memoria (RAM) e la memoria di paging

>[!Note] 
> Alcuni sistemi possono presentare prestazioni della memoria ridotte durante l'esecuzione di una nuova installazione di Windows Server 2016 rispetto a Windows Server 2012 R2. Sono state apportate varie modifiche durante lo sviluppo di Windows Server 2016 per migliorare la sicurezza e l'affidabilità della piattaforma. Alcune di queste modifiche, ad esempio l'abilitazione di Windows Defender per impostazione predefinita, comportano percorsi di I/O più lunghi che possono ridurre le prestazioni di I/O in specifici carichi di lavoro e modelli. Microsoft consiglia di non disabilitare Windows Defender, in quanto è un livello di protezione importante per i sistemi. 

Aumentare la quantità di RAM per soddisfare le esigenze di memoria.
Se la memoria del computer è insufficiente e deve essere aumentata immediatamente, Windows usa lo spazio su disco rigido per integrare la RAM di sistema tramite una procedura denominata paging. Un paging eccessivo comporta una riduzione delle prestazioni complessive del sistema.
È possibile ottimizzare il paging usando le linee guida seguenti per il posizionamento del file di paging:
- Isola il file di paging nel dispositivo di archiviazione o almeno assicurati che non condivida gli stessi dispositivi di archiviazione di altri file usati di frequente. Posizionare ad esempio il file di paging e i file del sistema operativo su unità disco fisiche separate.

- Posizionare il file di paging su un'unità non a tolleranza di errore. In caso di errore del disco, è probabile che si verifichi un arresto anomalo del sistema. Se si posiziona il file di paging su un'unità a tolleranza di errore, tenere presente che i sistemi a tolleranza di errore sono spesso più lenti per la scrittura dei dati in quanto devono scrivere i dati in più posizioni.

- Usare più dischi o un array di dischi se si necessita di una larghezza di banda del disco aggiuntiva per il paging. Non posizionare più file di paging in partizioni diverse della stessa unità disco fisica.

## <a name="peripheral-bus-recommendations"></a>Consigli per bus di periferiche
In Windows Server 2016 le interfacce di rete e di archiviazione primarie devono essere di tipo PCI Express (PCIe), pertanto sono consigliati server con bus PCIe. Per evitare limitazioni di velocità bus, usare slot PCIe x8 e versioni successive per le schede Ethernet 10+ GB.

## <a name="disk-recommendations"></a>Consigli per i dischi
Scegliere i dischi con maggiore velocità di rotazione per ridurre i tempi di servizio per richieste casuali (circa 2 ms mediamente quando si confrontano unità di 7.200-15.000 RPM) e per aumentare la larghezza di banda per richieste sequenziali. Esistono tuttavia considerazioni relative a costi, alimentazione e così via per i dischi con elevate velocità di rotazione.

I dischi classe enterprise da 2,5 pollici possono gestire un numero significativamente maggiore di richieste casuali per secondo rispetto a unità equivalenti da 3,5 pollici.

Archiviare i dati a cui si accede di frequente, soprattutto i dati ad accesso sequenziale, quasi all'inizio di un disco in quanto questa parte corrisponde approssimativamente alle tracce più esterne (più veloci).

Il consolidamento di unità di piccole dimensioni in un numero inferiore di unità con capacità elevata può ridurre le prestazioni di memoria complessive. Un minor numero di spindle corrisponde a una concorrenza ridotta del servizio per le richieste e pertanto potenzialmente a una riduzione della velocità effettiva e a tempi di risposta maggiori (a seconda dell'intensità del carico di lavoro).

È utile usare unità SSD e dischi flash ad alta velocità per la lettura di dischi principalmente con velocità di I/O elevate o con operazioni di I/O sensibili alla latenza. I dischi di avvio sono buoni candidati per l'uso di unità SSD o dischi flash ad alta velocità perché possono migliorare i tempi di avvio in modo significativo.

Le unità SSD NVMe offrono prestazioni superiori con maggiore profondità della coda dei comandi, elaborazione degli interrupt più efficiente e maggiore efficienza per i comandi 4 KB. Questo si rivela particolarmente utile per scenari che richiedono impegnative operazioni di I/O simultanee.


## <a name="network-and-storage-adapter-recommendations"></a>Consigli per schede di rete e adattatori di archiviazione

La sezione seguente elenca le caratteristiche consigliate per le schede di rete e gli adattatori di archiviazione per server ad alte prestazioni. Queste impostazioni consentono di impedire colli di bottiglia dell'hardware di rete o di archiviazione in caso di sovraccarico.

### <a name="certified-adapter-usage"></a>Uso di adattatori certificati
Usare un adattatore che abbia superato il gruppo di test Certificazione hardware Windows.

### <a name="64-bit-capability"></a>Funzionalità a 64 bit
Gli adattatori a 64 bit possono eseguire operazioni di accesso diretto alla memoria (DMA) da e verso posizioni di memoria fisica elevata (oltre 4 GB). Se il driver non supporta operazioni DMA superiori a 4 GB, il sistema esegue il doppio buffer delle operazioni di I/O in spazi degli indirizzi fisici inferiori a 4 GB.

### <a name="copper-and-fiber-adapters"></a>Adattatori in rame e fibra ottica
Gli adattatori in rame in genere hanno le stesse prestazioni delle rispettive controparti in fibra ottica e sono entrambi disponibili in alcuni adattatori Fibre Channel. Alcuni ambienti sono più adatti per adattatori in rame, mentre altri sono più adatti per adattatori in fibra ottica.

### <a name="dual--or-quad-port-adapters"></a>Adattatori Dual o Quad Port
Gli adattatori multiporta sono utili per i server con un numero limitato di slot PCI.

Per risolvere i limiti SCSI per il numero di dischi che possono essere collegati a un bus SCSI, alcuni adattatori forniscono due o quattro bus SCSI in una singola scheda. Per gli adattatori Fibre Channel in genere non sono previsti limiti per il numero di dischi collegati a un adattatore, a meno che non si trovino dietro un'interfaccia SCSI.

Anche per gli adattatori SAS (Serial Attached SCSI) e SATA (Serial ATA) è previsto un numero limitato di connessioni a causa della natura seriale dei protocolli, ma è possibile collegare più dischi tramite commutatori.

Nelle schede di rete questa funzionalità è utile per il bilanciamento del carico o per gli scenari di failover. L'uso di due schede di rete a una porta in genere garantisce prestazioni migliori rispetto all'uso di una singola scheda di rete a due porte per lo stesso carico di lavoro.

La limitazione dei bus PCI può essere un fattore rilevante nella riduzione delle prestazioni per le schede multiporta. È importante quindi posizionarle in uno slot PCIe ad alte prestazioni che fornisca sufficiente larghezza di banda.

### <a name="interrupt-moderation"></a>Regolazione di interrupt
Alcune schede possono regolare la frequenza di interrupt dei processori dell'host per indicare l'attività o il relativo completamento. La regolazione degli interrupt spesso comporta la riduzione del carico della CPU sull'host. Se tuttavia non viene eseguita in modo intelligente, il risparmio della CPU può aumentare la latenza.

### <a name="receive-side-scaling-rss-support"></a>Supporto RSS (Receive Side Scaling)
RSS consente di adattare l'elaborazione della ricezione di pacchetti al numero di processori disponibili nel computer. Ciò è particolarmente importante con Ethernet a 10 GB e più veloce.

### <a name="offload-capability-and-other-advanced-features-such-as-message-signaled-interrupt-msi-x"></a>Funzionalità di offload e altre funzionalità avanzate, ad esempio MSI-X
Gli adattatori che supportano l'offload consentono di ridurre l'uso della CPU offrendo pertanto prestazioni migliori.

### <a name="dynamic-interrupt-and-deferred-procedure-call-dpc-redirection"></a>Reindirizzamento dinamico di interrupt e chiamate di procedura differite (DPC)
In Windows Server 2016 le operazioni di I/O Numa consentono agli adattatori di archiviazione PCIe di reindirizzare in modo dinamico interrupt e DPC e si rivelano utili ai sistemi multiprocessore migliorando il partizionamento del carico di lavoro, i riscontri nella cache e l'uso delle interconnessioni hardware per carichi di lavoro con uso intensivo dell'I/O.

## <a name="see-also"></a>Vedi anche
- [Considerazioni sull'alimentazione dell'hardware del server](power.md)
- [Risparmio energia e ottimizzazione delle prestazioni](power/power-performance-tuning.md)
- [Ottimizzazione di Risparmio energia del processore](power/processor-power-management-tuning.md)
- [Parametri della combinazione per il risparmio di energia Bilanciato](power/recommended-balanced-plan-parameters.md)
