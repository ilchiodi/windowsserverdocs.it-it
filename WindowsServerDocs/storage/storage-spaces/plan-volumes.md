---
ms.assetid: 342173ca-4e10-44f4-b2c9-02a6c26f7a4a
title: Pianificazione dei volumi in Spazi di archiviazione diretta
ms.prod: windows-server
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0825c531913d134cc5711e3c8668fd6dedc4998f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856184"
---
# <a name="planning-volumes-in-storage-spaces-direct"></a>Pianificazione dei volumi in Spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento fornisce indicazioni su come pianificare i volumi in Spazi di archiviazione diretta per soddisfare le esigenze di capacità e prestazioni dei carichi di lavoro, inclusa la scelta del file system, del tipo di resilienza e delle dimensioni.

## <a name="review-what-are-volumes"></a>Revisione: Che cosa sono i volumi

I volumi consentono di inserire i file necessari per i carichi di lavoro, ad esempio i file VHD o VHDX per le macchine virtuali Hyper-V. I volumi combinano le unità nel pool di archiviazione per introdurre i vantaggi di Spazi di archiviazione diretta in termini di tolleranza di errore, scalabilità e prestazioni.

   >[!NOTE]
   > Nella documentazione per Spazi di archiviazione diretta, usiamo il termine "volume" per fare riferimento congiunto al volume e al disco virtuale al di sotto, incluse le funzionalità fornite da altre caratteristiche di Windows incorporate, ad esempio Volumi condivisi Cluster e ReFS. La comprensione di queste differenze a livello di implementazione non è necessaria per pianificare e distribuire Spazi di archiviazione diretta correttamente.

![what-are-volumes](media/plan-volumes/what-are-volumes.png)

Tutti i volumi sono accessibili da tutti i server del cluster nello stesso momento. Una volta creati, vengono visualizzati in **C:\ClusterStorage\\** in tutti i server.

![csv-folder-screenshot](media/plan-volumes/csv-folder-screenshot.png)

## <a name="choosing-how-many-volumes-to-create"></a>Scelta del numero di volumi da creare

Consigliamo di creare un numero di volumi multiplo del numero di server nel cluster. Se, ad esempio, si dispone di 4 server, si verificheranno prestazioni più coerenti con 4 volumi totali rispetto a 3 o 5. In questo modo il cluster può distribuire la "proprietà" dei volumi (un server gestisce l'orchestrazione dei metadati per ogni volume) in modo uniforme tra i server.

È consigliabile limitare il numero totale di volumi per:

| Windows Server 2016          | Windows Server 2019          |
|------------------------------|------------------------------|
| Fino a 32 volumi per cluster | Fino a 64 volumi per cluster |

## <a name="choosing-the-filesystem"></a>Scelta del file system

Ti consigliamo di usare il nuovo [Resilient File System (ReFS)](../refs/refs-overview.md) per Spazi di archiviazione diretta. ReFS è il file system di punta specificatamente progettato per la virtualizzazione e offre numerosi vantaggi, tra cui notevoli accelerazioni delle prestazioni e protezione integrata contro il danneggiamento dei dati. Supporta quasi tutte le principali funzionalità NTFS, inclusa la deduplicazione dei dati in Windows Server, versione 1709 e successive. Per informazioni dettagliate, vedere la [tabella di confronto delle funzionalità](../refs/refs-overview.md#feature-comparison) refs.

Se il tuo carico di lavoro richiede una funzionalità che ReFS non supporta ancora, puoi usare NTFS.

   > [!TIP]
   > Volumi con file system diversi possono coesistere nello stesso cluster.

## <a name="choosing-the-resiliency-type"></a>Scelta del tipo di resilienza

I volumi in Spazi di archiviazione diretta forniscono resilienza di protezione da problemi hardware, ad esempio gli errori di unità o server, e per consentire la disponibilità ininterrotta durante la manutenzione dei server, ad esempio durante gli aggiornamenti software.

   > [!NOTE]
   > La scelta dei tipi di resilienza è indipendente dei tipi di unità di cui disponi.

### <a name="with-two-servers"></a>Con due server

Con due server nel cluster, è possibile usare il mirroring a due vie. Se si esegue Windows Server 2019, è anche possibile usare la resilienza nidificata.

Il mirroring a due vie conserva due copie di tutti i dati, una copia sulle unità in ogni server. L'efficienza di archiviazione è del 50%: per scrivere 1 TB di dati, sono necessari almeno 2 TB di capacità di archiviazione fisica nel pool di archiviazione. Il mirroring a due vie può tollerare in modo sicuro un errore hardware alla volta (un server o un'unità).

![mirroring a 2 vie](media/plan-volumes/two-way-mirror.png)

La resilienza annidata, disponibile solo in Windows Server 2019, garantisce la resilienza dei dati tra i server con mirroring a due vie, quindi aggiunge la resilienza all'interno di un server con mirroring a due vie o parità con accelerazione del mirroring. L'annidamento garantisce la resilienza dei dati anche quando un server viene riavviato o non disponibile. L'efficienza di archiviazione è il 25% con il mirroring a due vie annidato e circa il 35-40% per la parità con accelerazione speculare nidificata. La resilienza annidata può tollerare in modo sicuro due errori hardware alla volta (due unità o un server e un'unità nel server rimanente). A causa di questa maggiore resilienza dei dati, è consigliabile usare la resilienza annidata nelle distribuzioni di produzione di cluster a due server, se si esegue Windows Server 2019. Per altre informazioni, vedere [resilienza nidificata](nested-resiliency.md).

![Parità con accelerazione speculare nidificata](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="with-three-servers"></a>Con tre server

Con tre server, dovresti usare il mirroring a tre vie per una migliore tolleranza di errore e migliori prestazioni. Il mirroring a tre vie mantiene tre copie di tutti i dati, una copia sulle unità in ogni server. L'efficienza di archiviazione è pari al 33,3%: per scrivere 1 TB di dati sono necessari almeno 3 TB di capacità di archiviazione fisica nel pool di archiviazione. Il mirroring a tre vie può tollerare [almeno due problemi hardware (unità o server) alla volta](storage-spaces-fault-tolerance.md#examples). Se 2 nodi diventano non disponibili, il pool di archiviazione perderà il quorum, perché 2/3 dei dischi non sono disponibili e i dischi virtuali non saranno accessibili. Tuttavia, un nodo può essere inattivo e uno o più dischi in un altro nodo possono avere esito negativo e i dischi virtuali rimarranno online. Ad esempio, se stai riavviando un server quando improvvisamente si verifica un problema a un'altra unità o server, tutti i dati rimangono sicuri e sempre accessibili.

![mirroring a 3 vie](media/plan-volumes/three-way-mirror.png)

### <a name="with-four-or-more-servers"></a>Con quattro o più server

Con quattro o più server, è possibile scegliere per ogni volume se usare il mirroring a tre vie, la doppia parità (spesso detta "codifica di cancellazione") o combinare le due con la parità con accelerazione del mirroring.

La doppia parità offre la stessa tolleranza di errore del mirroring a 3 vie, ma con una migliore efficienza di archiviazione. Con quattro server, l'efficienza di archiviazione è il 50.0%, per archiviare 2 TB di dati, sono necessari 4 TB di capacità di archiviazione fisica nel pool di archiviazione. Questo valore aumenta fino al 66,7% di efficienza di archiviazione con sette server e continua fino all'80,0%. Lo svantaggio è che la codifica della parità richiede un uso maggiore delle risorse di calcolo, cosa che può limitare le prestazioni.

![doppia parità](media/plan-volumes/dual-parity.png)

Il tipo di resilienza da usare dipende dalle esigenze del carico di lavoro. Di seguito è riportata una tabella in cui vengono riepilogati i carichi di lavoro idonei per ogni tipo di resilienza, nonché le prestazioni e l'efficienza di archiviazione di ogni tipo di resilienza.

| Tipo di resilienza | Efficienza della capacità | Velocità | Carichi di lavoro |
| ------------------- | ----------------------  | --------- | ------------- |
| **Mirror**         | ![Efficienza di archiviazione che mostra il 33%](media/plan-volumes/3-way-mirror-storage-efficiency.png)<br>Mirroring a tre vie: 33% <br>Mirroring a due vie: 50%     |![Prestazioni con risultati pari al 100%](media/plan-volumes/three-way-mirror-perf.png)<br> Prestazioni più elevate  | Carichi di lavoro virtualizzati<br> Database<br>Altri carichi di lavoro ad alte prestazioni |
| **Parità accelerata con mirror** |![Efficienza di archiviazione mostrata intorno al 50%](media/plan-volumes/mirror-accelerated-parity-storage-efficiency.png)<br> Dipende dalla proporzione del mirror e della parità | ![Prestazioni che mostrano circa il 20%](media/plan-volumes/mirror-accelerated-parity-perf.png)<br>Molto più lento rispetto al mirror, ma fino a due volte più veloce della doppia parità<br> Ideale per scritture e letture sequenziali di grandi dimensioni | Archiviazione e backup<br> Infrastruttura desktop virtualizzata     |
| **Doppia parità**               | ![Efficienza di archiviazione mostrata intorno al 80%](media/plan-volumes/dual-parity-storage-efficiency.png)<br>4 server: 50% <br>16 server: fino al 80% | ![Prestazioni che mostrano circa il 10%](media/plan-volumes/dual-parity-perf.png)<br>Latenza di I/O più elevata & utilizzo CPU durante le operazioni di scrittura<br> Ideale per scritture e letture sequenziali di grandi dimensioni | Archiviazione e backup<br> Infrastruttura desktop virtualizzata  |

#### <a name="when-performance-matters-most"></a>Quando le prestazioni sono l'elemento più importante

I carichi di lavoro che hanno rigidi requisiti di latenza o che richiedono molte operazioni IOPS casuali miste, ad esempio database di SQL Server o macchine virtuali Hyper-V sensibili alle prestazioni, dovrebbero essere eseguiti in volumi che usano il mirroring per ottimizzare le prestazioni.

   >[!TIP]
   > Il mirroring è più veloce di qualsiasi altro tipo di resilienza. Usiamo il mirroring per quasi tutti i nostri esempi di prestazioni.

#### <a name="when-capacity-matters-most"></a>Quando la capacità è l'elemento più importante

I carichi di lavoro che scrivono raramente, ad esempio data warehouse o l'archiviazione di dati ad accesso sporadico, dovrebbero essere eseguiti in volumi che usano la doppia parità per ottimizzare l'efficienza di archiviazione. Altri carichi di lavoro specifici, ad esempio i file server tradizionali, Virtual Desktop Infrastructure (VDI) o altri che non creano molto traffico I/O casuale a rapida deviazione e/o non richiedono prestazioni ottimali possono anche usare la doppia parità, a tua discrezione. La parità inevitabilmente aumenta l'utilizzo della CPU e latenza di I/O, in particolare sulle scritture, rispetto al mirroring.

#### <a name="when-data-is-written-in-bulk"></a>Quando i dati vengono scritti in blocco

Per i carichi di lavoro con scritture eseguite in grandi passaggi sequenziali, ad esempio l'archiviazione o le destinazioni di backup, è disponibile un'altra opzione che è una novità in Windows Server 2016: un volume può combinare mirroring e doppia parità. Le scritture arrivano prima nella parte con mirroring e vengono spostate gradualmente nella parte con parità in un secondo momento. Questo accelera l'inserimento e riduce l'utilizzo delle risorse all'arrivo di scritture di grandi dimensioni, prolungando il tempo disponibile per la codifica della parità a elevato uso delle risorse di calcolo. Quando definisci le dimensioni delle parti, considera che la quantità delle scritture che si verificano contemporaneamente (ad esempio un backup giornaliero) deve essere comodamente contenuta nella parte del mirror. Ad esempio, se inserisci 100 GB una volta al giorno, valuta di usare il mirroring per una quantità compresa tra 150 e 200 GB e la doppia parità per il resto.

L'efficienza di archiviazione risultante dipende dalle proporzioni che scegli. Vedi [questa demo](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) per alcuni esempi.

   > [!TIP]
   > Se si osserva una riduzione rapida delle prestazioni di scrittura legato attraverso l'inserimento dei dati, potrebbe indicare che la parte del mirror non è sufficientemente grande o che la parità con accelerazione del mirror non è adatta per il caso d'uso. Ad esempio, se le prestazioni di scrittura diminuiscono da 400 MB/s a 40 MB/s, è consigliabile espandere la parte speculare o passare al mirroring a tre vie.

### <a name="about-deployments-with-nvme-ssd-and-hdd"></a>Informazioni sulle distribuzioni con unità NVMe, SSD e HDD

Nelle distribuzioni con due tipi di unità, le unità più veloci fungono da cache mentre le unità più lente offrono la capacità. Ciò si verifica automaticamente: per ulteriori informazioni, vedi [Informazioni sulla cache in Spazi di archiviazione diretta](understand-the-cache.md). In tali distribuzioni, tutti i volumi in ultima analisi si trovano sullo stesso tipo di unità, le unità di capacità.

Nelle distribuzioni con tutti e tre i tipi di unità, solo le unità più veloci (NVMe) offrono funzioni di cache, lasciando due tipi di unità (SSD e HDD) a fornire capacità. Per ogni volume, puoi scegliere se debba risiedere completamente nel livello SSD, completamente nel livello HDD o se si estende su entrambi.

   > [!IMPORTANT]
   > Ti consigliamo di usare il livello SSD per distribuire i carichi di lavoro più sensibili alle prestazioni su all-flash.

## <a name="choosing-the-size-of-volumes"></a>Scelta delle dimensioni dei volumi

È consigliabile limitare le dimensioni di ogni volume a:

| Windows Server 2016 | Windows Server 2019 |
| ------------------- | ------------------- |
| Fino a 32 TB         | Fino a 64 TB         |

   > [!TIP]
   > Se si usa una soluzione di backup che si basa sul servizio Copia Shadow del volume (VSS) e sul provider di software VolSnap, così come è comune con i carichi di lavoro file server, la limitazione delle dimensioni del volume a 10 TB consentirà di migliorare le prestazioni e l'affidabilità. Le soluzioni di backup che utilizzano la più recente API RCT Hyper-V e/o la clonazione dei blocchi ReFS e/o le API di backup SQL native ottengono buone prestazioni fino a 32 TB e oltre.

### <a name="footprint"></a>Footprint

Le dimensioni di un volume si riferiscono alla sua capacità utilizzabile, la quantità di dati che può archiviare. Questo valore viene fornito dal parametro **-Size** del cmdlet **New-Volume** e quindi viene visualizzato nella proprietà **Size** quando esegui il cmdlet **Get-Volume**.

Le dimensioni sono diverse dal *footprint* del volume, cioè la capacità di archiviazione fisica totale che occupa nel pool di archiviazione. Il footprint dipende dal relativo tipo di resilienza. Ad esempio, nei volumi che usano il mirroring a tre vie il footprint è pari a tre volte le dimensioni.

Il footprint dei volumi deve essere contenuto nel pool di archiviazione.

![size-versus-footprint](media/plan-volumes/size-versus-footprint.png)

### <a name="reserve-capacity"></a>Capacità di riserva

Lasciare capacità non allocata nel pool di archiviazione fornisce ai volumi lo spazio per il ripristino "sul posto" dopo errori delle unità, migliorando la sicurezza dei dati e le prestazioni. Se la capacità è sufficiente, un ripristino parallelo e immediato sul posto, può ripristinare la completa resilienza dei volumi anche prima che le unità con errori vengano sostituite. Questo avviene automaticamente.

Consigliamo di riservare l'equivalente di un'unità di capacità per ogni server, fino a 4 unità. Puoi riservare più capacità a tua discrezione, ma questa raccomandazione minima garantisce un ripristino parallelo e immediato sul posto dopo l'errore di qualsiasi unità.

![reserve](media/plan-volumes/reserve.png)

Ad esempio, se disponi di 2 server e utilizzi unità di capacità da 1 TB, riserva 2 x 1 = 2 TB del pool. Se disponi di 3 server e unità di capacità da 1 TB, riserva 3 x 1 = 3 TB. Se disponi di 4 o più server e unità di capacità da 1 TB, riserva 4 x 1 = 4 TB.

   >[!NOTE]
   > In cluster con unità di tutti i tre tipi (NVMe + SSD + HDD), è consigliabile riservare l'equivalente di una unità SSD più un'unità HDD per ogni server, fino a 4 unità per ciascuno.

## <a name="example-capacity-planning"></a>Esempio: Pianificazione della capacità

Prendi in considerazione un cluster di quattro server. Ogni server dispone di alcune unità cache più sedici unità da 2 TB per la capacità.

```
4 servers x 16 drives each x 2 TB each = 128 TB
```

Di questi 128 TB nel pool di archiviazione, riserviamo quattro unità, o 8 TB, in modo da consentire le operazioni di ripristino sul posto senza necessità di affrettarsi per sostituire le unità con errori. In questo modo rimangono 120 TB di capacità di archiviazione fisica nel pool di con cui possiamo creare volumi.

```
128 TB – (4 x 2 TB) = 120 TB
```

Supponi che nella distribuzione dobbiamo ospitare alcune macchine virtuali Hyper-V notevolmente attive, ma che abbiamo archiviato anche molti dati ad accesso sporadico, come file vecchi e backup che è necessario conservare. Poiché abbiamo quattro server, creiamo quattro volumi.

Inseriamo le macchine virtuali nei primi due volumi, *Volume1* e *Volume2*. Scegliamo ReFS come file system (per creazione e checkpoint più veloci) e il mirroring a tre vie per la resilienza per ottimizzare le prestazioni. Inseriamo l'archiviazione dei dati ad accesso sporadico sugli altri due volumi, *Volume 3* e *Volume 4*. Scegliamo NTFS come file system (per la deduplicazione dei dati) e la doppia parità per la resilienza per ottimizzare la capacità.

Non è necessario definire le stesse dimensioni per tutti i volumi, ma per semplicità supponiamo di crearli tutti di 12 TB.

*Volume1* e *Volume2* occuperanno ciascuno 12 TB x 33,3% di efficienza = 36 TB di capacità di archiviazione fisica.

*Volume3* e *Volume4* occuperanno ciascuno 12 TB x 50,0% di efficienza = 24 TB di capacità di archiviazione fisica.

```
36 TB + 36 TB + 24 TB + 24 TB = 120 TB
```

I quattro volumi occuperanno esattamente la capacità di archiviazione fisica disponibile nel nostro pool. Perfetto!

![esempio](media/plan-volumes/example.png)

   >[!TIP]
   > Non è necessario creare immediatamente tutti i volumi. Puoi sempre estendere i volumi o creare nuovi volumi in un secondo momento.

Per semplicità, questo esempio usa sempre unità decimali (in base 10), ovvero 1 TB = 1.000.000.000.000 byte. Tuttavia, le quantità di archiviazione in Windows vengono visualizzate in unità binarie (in base 2). Ogni unità da 2 TB, ad esempio, viene visualizzata come da 1,82 TiB in Windows. Analogamente, il pool di archiviazione da 128 TB viene visualizzato come da 116,41 TiB. Si tratta di un comportamento previsto.

## <a name="usage"></a>Utilizzo

Vedi [Creazione di volumi in Spazi di archiviazione diretta](create-volumes.md).

### <a name="see-also"></a>Vedere anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Scelta di unità per Spazi di archiviazione diretta](choosing-drives.md)
- [Tolleranza di errore ed efficienza di archiviazione](storage-spaces-fault-tolerance.md)
