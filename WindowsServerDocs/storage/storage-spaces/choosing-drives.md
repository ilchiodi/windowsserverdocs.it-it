---
ms.assetid: 1368bc83-9121-477a-af09-4ae73ac16789
title: Scelta delle unità per Spazi di archiviazione diretta
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
ms.localizationpriority: medium
ms.openlocfilehash: b02d26e8a2f2cd699d8e1eda95cf72fe71396a21
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402893"
---
# <a name="choosing-drives-for-storage-spaces-direct"></a>Scelta delle unità per Spazi di archiviazione diretta

>Si applica a: Windows 2019, Windows Server 2016

In questo argomento vengono fornite indicazioni su come scegliere le unità per [Spazi di archiviazione diretta](storage-spaces-direct-overview.md) per soddisfare i tuoi requisiti di capacità e prestazioni.

## <a name="drive-types"></a>Tipi di unità

Attualmente, Spazi di archiviazione diretta è utilizzabile con tre tipi di unità:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>NVMe</b> (Non-Volatile Memory Express) fa riferimento alle unità SSD collocate direttamente sul bus PCIe. I fattori di forma più comuni sono U.2 da 2,5 pollici, Add-In-Card (AIC) PCIe ed M.2. La tecnologia NVMe offre un numero maggiore di operazioni di I/O al secondo (IOPS), una velocità effettiva di I/O superiore e valori di latenza inferiori rispetto a qualsiasi altro tipo di unità che supportiamo oggi.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px" >
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>SSD</b> fa riferimento alle unità SSD connesse tramite un'interfaccia SATA o SAS convenzionale.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>HDD</b> fa riferimento alle unità disco rigido magnetiche e rotazionali che assicurano grandi capacità di archiviazione.
        </td>
    </tr>
</table>

## <a name="built-in-cache"></a>Cache integrata

Spazi di archiviazione diretta include una cache lato server integrata. Si tratta di una cache di lettura e scrittura di grandi dimensioni, permanente e in tempo reale. Nelle distribuzioni con più tipi di unità, viene configurata automaticamente per l'utilizzo di tutte le unità del tipo "più veloce". Le unità rimanenti vengono utilizzate per la capacità.

Per ulteriori informazioni, consulta [Informazioni sulla cache in Spazi di archiviazione diretta](understand-the-cache.md).

## <a name="option-1--maximizing-performance"></a>Opzione 1: ottimizzazione delle prestazioni

Se il tuo obiettivo è ottenere valori di latenza prevedibili, uniformi e inferiori a un millisecondo per letture e scritture casuali di qualsiasi tipo di dati oppure ottenere un numero molto alto di operazioni IOPS (ne abbiamo ottenute [oltre sei milioni](https://www.youtube.com/watch?v=0LviCzsudGY&t=28m)) o una velocità effettiva di I/O estremamente elevata (siamo arrivati a [oltre 1 Tb/s](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=16m50s)), dovresti optare per una soluzione all-flash,

che attualmente puoi ottenere percorrendo tre strade:

![All-Flash-Deployment-Possibilities](media/choosing-drives-and-resiliency-types/All-Flash-Deployment-Possibilities.png)

1. **Tutti NVMe.** L'uso di sole unità NVMe assicura prestazioni eccellenti, inclusa la bassa latenza più prevedibile. Se tutte le unità sono dello stesso modello, non è prevista cache. Puoi anche combinare modelli NVMe di maggiore e minore resistenza e configurare i primi come cache per le scritture dei secondi ([per questo sono necessarie attività di configurazione](understand-the-cache.md#manual-configuration)).

2. **NVMe e SSD.** Se utilizzi unità NVMe insieme a unità SSD, le NVMe fungeranno automaticamente da cache per le scritture nelle SSD. In questo modo le scritture possono essere unite nella cache e rimosse solo se e quando necessario, riducendo l'usura delle SSD. Questa configurazione fornisce caratteristiche di scrittura simili a quella della tecnologia NVMe, mentre le letture vengono direttamente servite dalle altrettanto veloci unità SSD.

3. **Tutte le unità SSD.** Come nel caso di tutte unità NVMe, se tutte le unità sono dello stesso modello, non è prevista cache. Se combini modelli di maggiore e minore resistenza, puoi configurare i primi come cache per le scritture dei secondi ([per questo sono necessarie attività di configurazione](understand-the-cache.md#manual-configuration)).

   >[!NOTE]
   > Un vantaggio dell'uso di tutte unità NVMe o tutte unità SSD senza cache sta nel fatto che ogni unità fornisce capacità di archiviazione utilizzabile. La capacità non viene impiegata per l'attività di memorizzazione nella cache, cosa che può essere interessante su scale inferiori.

## <a name="option-2--balancing-performance-and-capacity"></a>Opzione 2: bilanciamento di prestazioni e capacità

Per gli ambienti caratterizzati da una varietà di applicazioni e carichi di lavoro, alcuni dei quali devono soddisfare rigorosi requisiti di prestazioni mentre altri richiedono una considerevole capacità di archiviazione, dovresti optare per una soluzione ibrida con unità NVMe o SSD che fungono da cache per unità HDD più grandi.

![Hybrid-Deployment-Possibilities](media/choosing-drives-and-resiliency-types/Hybrid-Deployment-Possibilities.png)

1. **Unità NVMe + HDD**. Le unità NVMe fungeranno da cache per letture e scritture, accelerandole. La memorizzazione nella cache delle letture consente alle unità HDD di focalizzare l'attività sulle scritture. La memorizzazione nella cache delle scritture assorbe i picchi e permette che le scritture vengano unite e rimosse dalla cache solo se e quando necessario, in un modo artificialmente serializzato che ottimizza i valori di IOPS e velocità effettiva di I/O delle unità HDD. Questa configurazione fornisce caratteristiche di scrittura simili a quelle della tecnologia NVMe e, per i dati letti di frequente o di recente, anche analoghe caratteristiche di lettura.

2. **Unità SSD + HDD**. Come nel caso precedente, le unità SSD fungeranno da cache per letture e scritture, accelerandole. Questa configurazione fornisce caratteristiche di scrittura simili a quelle della tecnologia SSD e, per i dati letti di frequente o di recente, anche analoghe caratteristiche di lettura.

    Puoi valutare un'ulteriore opzione, piuttosto particolare: l'uso di un'unità di *tutti e tre* i tipi.

3. **NVMe + SSD + HDD.** Con unità di tutte e tre i tipi, le unità NVMe fungeranno da cache sia per le SSD che per le HDD. L'aspetto interessante è che puoi creare volumi sia sulle SSD che sulle HDD, affiancati nello stesso cluster e tutti accelerati dalla tecnologia NVMe. I primi corrispondono esattamente a una distribuzione all-flash e i secondi alle distribuzioni ibride descritte prima. Concettualmente è come disporre di due pool in gran parte indipendenti in termini di gestione della capacitò, cicli di errore e ripristino e così via.

   >[!IMPORTANT]
   > Ti consigliamo di usare il livello SSD per distribuire i carichi di lavoro più sensibili alle prestazioni su all-flash.

## <a name="option-3--maximizing-capacity"></a>Opzione 3: ottimizzazione della capacità

Per i carichi di lavoro con scritture poco frequenti ma esigenze di vasta capacità, ad esempio l'archiviazione, le destinazioni di backup, i data warehouse o l'archiviazione di dati ad accesso sporadico, dovresti combinare poche unità SSD per la memorizzazione nella cache con molte unità HDD più grandi per la capacità.

![Opzioni di distribuzione per l'ottimizzazione della capacità](media/choosing-drives-and-resiliency-types/maximizing-capacity.png)

1. **Unità SSD + HDD**. Le unità SSD fungeranno da cache per letture e scritture, per assorbire i picchi e offrire prestazioni di scrittura SSD, e successivamente assicureranno la rimozione ottimizzata dalla cache con spostamento nelle unità HDD.

>[!IMPORTANT]
>La configurazione solo con HDD non è supportata. Non è consigliabile eseguire la memorizzazione nella cache di unità SSD a bassa resistenza a un SSD a bassa resistenza.

## <a name="sizing-considerations"></a>Considerazioni sulle dimensioni

### <a name="cache"></a>Cache

Ogni server deve disporre di almeno due unità cache (il minimo richiesto per la ridondanza). È consigliabile che il numero delle unità di capacità sia un multiplo del numero delle unità cache. Ad esempio, con quattro unità cache, le prestazioni risulteranno più uniformi con otto unità di capacità (rapporto 1:2) che con 7 o 9.

La cache deve essere dimensionata in modo da supportare la working set delle applicazioni e dei carichi di lavoro, ovvero tutti i dati che stanno attivamente leggendo e scrivendo in un determinato momento. A parte questo, non ci sono specifici requisiti di dimensioni per la cache. Per le distribuzioni con HDD, un punto di partenza equo è il 10% della capacità, ad esempio se ogni server ha 4 x 4 TB HDD = 16 TB di capacità, quindi 2 x 800 GB SSD = 1,6 TB di cache per server. Per le distribuzioni di tutti i flash, soprattutto con unità SSD di [durata molto elevata](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/) , può essere giusto iniziare a avvicinarsi al 5% della capacità, ad esempio se ogni server ha 24 unità ssd da 1,2 tb = 28,8 TB di capacità, quindi 2 x 750 GB NVMe = 1,5 TB di cache per server. Puoi sempre aggiungere o rimuovere unità cache in un secondo momento in base alle esigenze.

### <a name="general"></a>Generale

Consigliamo di limitare la capacità di archiviazione totale per server a circa 100 terabyte (TB). Maggiore è la capacità di archiviazione per server, maggiore è il tempo necessario per risincronizzare i dati dopo un periodo di inattività o un riavvio, ad esempio quando si esegue un aggiornamento software.

La dimensione massima corrente per ogni pool di archiviazione è 4 petabyte (PB) (4.000 TB) per Windows Server 2019 o 1 petabyte per Windows Server 2016.

## <a name="see-also"></a>Vedere anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Comprendere la cache in Spazi di archiviazione diretta](understand-the-cache.md)
- [Requisiti hardware Spazi di archiviazione diretta](storage-spaces-direct-hardware-requirements.md)
- [Pianificazione di volumi in Spazi di archiviazione diretta](plan-volumes.md)
- [Tolleranza di errore ed efficienza di archiviazione](storage-spaces-fault-tolerance.md)
