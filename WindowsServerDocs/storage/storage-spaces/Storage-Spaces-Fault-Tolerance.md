---
title: Tolleranza di errore ed efficienza di archiviazione in Spazi di archiviazione diretta
ms.prod: windows-server
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/11/2017
ms.assetid: 5e1d7ecc-e22e-467f-8142-bad6d82fc5d0
description: Una descrizione delle opzioni di resilienza in Spazi di archiviazione diretta, tra cui mirroring e parità.
ms.localizationpriority: medium
ms.openlocfilehash: d2220584c0021352110b27c3107d1113eb17ef59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393811"
---
# <a name="fault-tolerance-and-storage-efficiency-in-storage-spaces-direct"></a>Tolleranza di errore ed efficienza di archiviazione in Spazi di archiviazione diretta

>Si applica a: Windows Server 2016

Questo argomento presenta le opzioni di resilienza disponibili in [Spazi di archiviazione diretta](storage-spaces-direct-overview.md) e descrive i requisiti di scalabilità, l'efficienza di archiviazione, nonché i compromessi e i vantaggi generali di ogni soluzione. Fornisce anche alcune istruzioni di base, nonché riferimenti a interessanti documenti, blog e contenuti aggiuntivi per ottenere altre informazioni.

Se conosci già Spazi di archiviazione, puoi passare alla sezione [Riepilogo](#summary).

## <a name="overview"></a>Panoramica

Essenzialmente, lo scopo di Spazi di archiviazione è quello di offrire tolleranza di errore, spesso denominata "resilienza", per i dati. L'implementazione è simile a RAID, ad eccezione del fatto che la distribuzione avviene nei server e l'implementazione nel software.

Come con RAID, ci sono diverse modalità d'uso di Spazi di archiviazione, che comportano compromessi diversi tra tolleranza di errore, efficienza di archiviazione e complessità di calcolo. A grandi linee, le opzioni rientrano in due categorie, ovvero "mirroring" e "parità"; quest'ultima a volte denominata anche "codifica di cancellazione".

## <a name="mirroring"></a>Mirroring

Il mirroring fornisce tolleranza di errore grazie alla presenza di più copie di tutti i dati. Questa opzione è analoga a RAID-1. Le modalità di striping e posizionamento dei dati non sono banali (vedi [questo blog](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/) per saperne di più), ma è vero che tutti i dati archiviati usando il mirroring vengono scritti completamente più volte. Ogni copia viene scritta in un componente hardware fisico diverso, ovvero unità diverse in server diversi, che si presuppone possano essere soggetti a errori in modo indipendente.

In Windows Server 2016, Spazi di archiviazione offre due tipi di mirroring: a 2 vie e a 3 vie.

### <a name="two-way-mirror"></a>Mirroring a 2 vie

Il mirroring a 2 vie scrive due copie di tutti gli elementi. L'efficienza di archiviazione è pari al 50%: per scrivere 1 TB di dati sono necessari almeno 2 TB di capacità di archiviazione fisica. Analogamente, sono necessari almeno due ["domini di errore" hardware](../../failover-clustering/fault-domains.md) con Spazi di archiviazione diretta, ovvero due server.

![mirroring a 2 vie](media/Storage-Spaces-Fault-Tolerance/two-way-mirror-180px.png)

   >[!WARNING]
   > Se hai più di due server, è consigliabile usare invece il mirorring a 3 vie.

### <a name="three-way-mirror"></a>Mirroring a 3 vie

Il mirroring a 3 vie scrive tre copie di tutti gli elementi. L'efficienza di archiviazione è pari al 33,3%: per scrivere 1 TB di dati sono necessari almeno 3 TB di capacità di archiviazione fisica. Analogamente, sono necessari almeno tre domini di errore hardware con Spazi di archiviazione diretta, ovvero tre server.

Il mirroring a 3 vie può tollerare almeno [due problemi hardware (unità o server) alla volta](#examples). Ad esempio, se stai riavviando un server quando improvvisamente si verifica un problema a un'altra unità o server, tutti i dati rimangono sicuri e sempre accessibili.

![mirroring a 3 vie](media/Storage-Spaces-Fault-Tolerance/three-way-mirror-180px.png)

## <a name="parity"></a>Parità

La codifica di parità, spesso detta "codifica di cancellazione", offre tolleranza di errore usando un'aritmetica bit per bit, che può essere [notevolmente complessa](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/LRC12-cheng20webpage.pdf). Il funzionamento è meno ovvio rispetto al mirroring e sono disponibili numerose risorse online (ad esempio la guida di terze parti [Dummies Guide to Erasure Coding](http://smahesh.com/blog/2012/07/01/dummies-guide-to-erasure-coding/)) che possono aiutarti a capirlo. È sufficiente notare che questo metodo offre maggiore efficienza di archiviazione senza compromettere la tolleranza di errore.

In Windows Server 2016 Spazi di archiviazione offre due tipi di parità, singola e doppia; quest'ultima usa una tecnica avanzata denominata "codici di ricostruzione locali" su scala più ampia.

> [!IMPORTANT]
> Ti consigliamo di usare il mirroring per i carichi di lavoro più sensibili alle prestazioni. Per altre informazioni su come bilanciare prestazioni e capacità in base al carico di lavoro, vedi [Pianificare volumi](plan-volumes.md#choosing-the-resiliency-type).

### <a name="single-parity"></a>Parità singola
La parità singola mantiene solo un simbolo di parità bit per bit, che offre tolleranza di errore solo per un errore alla volta. Questa opzione è analoga a RAID-5. Per usare la parità singola, sono necessari almeno tre domini di errore hardware con Spazi di archiviazione diretta, ovvero tre server. Poiché il mirroring a 3 vie fornisce maggiore tolleranza di errore su scala analoga, l'uso della parità singola non è consigliabile. Ma se vuoi usarla è disponibile e completamente supportata.

   >[!WARNING]
   > Ti sconsigliamo di usare la parità singola, poiché può tollerare solo un errore hardware alla volta: se stai riavviando un server quando improvvisamente si verifica un errore in un'altra unità o server, subirai tempi di inattività. Se hai solo tre server, ti consigliamo di usare il mirroring a 3 vie. Se hai quattro o più server, vedi la sezione successiva.

### <a name="dual-parity"></a>Doppia parità

La doppia parità implementa i codici di correzione d'errore Reed-Solomon per mantenere due simboli di parità bit per bit, fornendo così lo stesso livello di tolleranza di errore del mirroring a 3 vie (ovvero fino a due errori per volta), ma con una migliore efficienza di archiviazione. Questa opzione è analoga a RAID-6. Per usare la doppia parità, sono necessari almeno quattro domini di errore hardware con Spazi di archiviazione diretta, ovvero quattro server. In questo caso, l'efficienza di archiviazione è pari al 50%: per archiviare 2 TB di dati sono necessari 4 TB di capacità di archiviazione fisica.

![doppia parità](media/Storage-Spaces-Fault-Tolerance/dual-parity-180px.png)

L'efficienza di archiviazione della doppia parità aumenta con l'aumentare del numero di domini di errore hardware, dal 50% fino all'80%. Ad esempio, con sette domini di errore (con Spazi di archiviazione diretta, ovvero sette server) l'efficienza passa al 66,7%: per archiviare 4 TB di dati sono sufficienti 6 TB di capacità di archiviazione fisica.

![doppia parità estesa](media/Storage-Spaces-Fault-Tolerance/dual-parity-wide-180px.png)

Vedi la sezione [Riepilogo](#summary) per informazioni sull'efficienza della doppia parità e dei codici di ricostruzione locali su qualsiasi scala.

### <a name="local-reconstruction-codes"></a>Codici di ricostruzione locali

Spazi di archiviazione in Windows Server 2016 introduce una tecnica avanzata sviluppata da Microsoft Research detta dei "codici di ricostruzione locali" o LRC (Local Reconstruction Codes). Su ampia scala, la doppia parità usa i codici di ricostruzione locali per suddividere codifica/decodifica in alcuni gruppi più piccoli, per ridurre il sovraccarico in caso di scrittura o di ripristino dagli errori.

Con le unità disco rigido (HDD) la dimensione del gruppo è di quattro simboli, mentre con le unità SSD è di sei simboli. Ecco, ad esempio, il layout con unità disco rigido e 12 domini di errore hardware (ovvero 12 server): ci sono due gruppi di quattro simboli di dati. L'efficienza di archiviazione ottenuta è del 72,7%.

![codici di ricostruzione locali](media/Storage-Spaces-Fault-Tolerance/local-reconstruction-codes-180px.png)

Ti consigliamo di leggere questa presentazione dettagliata approfondita, ma perfettamente comprensibile, relativa al [modo in cui i codici di ricostruzione locali gestiscono diversi scenari di errore e i motivi per cui sono interessanti](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/), a cura di [Claus Joergensen](https://twitter.com/clausjor).

## <a name="mirror-accelerated-parity"></a>Parità accelerata con mirror

A partire da Windows Server 2016, un volume di Spazi di archiviazione diretta può offrire in parte mirroring e in parte parità. Le scritture arrivano prima nella parte con mirroring e vengono spostate gradualmente nella parte con parità in un secondo momento. In effetti, si tratta di [usare il mirroring per accelerare la codifica di cancellazione](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/).

Per combinare mirroring a 3 vie e doppia parità, devi avere almeno quattro domini di errore, ovvero quattro server.

L'efficienza di archiviazione della parità accelerata con mirror è una via di mezzo tra ciò che otterresti usando solo il mirroring o solo la parità e dipende dalle proporzioni che scegli. La demo di questa presentazione, in corrispondenza del minuto 37, mostra ad esempio [diverse combinazioni per ottenere un'efficienza pari al 46%, 54% e 65%](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) con 12 server.

> [!IMPORTANT]
> Ti consigliamo di usare il mirroring per i carichi di lavoro più sensibili alle prestazioni. Per altre informazioni su come bilanciare prestazioni e capacità in base al carico di lavoro, vedi [Pianificare volumi](plan-volumes.md#choosing-the-resiliency-type).

## <a name="summary"></a>Riepilogo

Questa sezione riepiloga i tipi resilienza disponibili in Spazi di archiviazione diretta, i requisiti di scala minimi per ogni tipo, il numero di errori che ogni tipo può tollerare e l'efficienza di archiviazione corrispondente.

### <a name="resiliency-types"></a>Tipi di resilienza

|    Resilienza          |    Tolleranza di errore       |    Efficienza di archiviazione      |
|------------------------|----------------------------|----------------------------|
|    Mirroring a 2 vie      |    1                       |    50%                   |
|    Mirroring a 3 vie    |    2                       |    33,3%                   |
|    Doppia parità         |    2                       |    50% - 80%           |
|    Modalità mista               |    2                       |    33,3% - 80%           |

### <a name="minimum-scale-requirements"></a>Requisiti di scala minimi

|    Resilienza          |    Numero minimo di domini di errore necessari   |
|------------------------|-------------------------------------|
|    Mirroring a 2 vie      |    2                                |
|    Mirroring a 3 vie    |    3                                |
|    Doppia parità         |    4                                |
|    Modalità mista               |    4                                |

   >[!TIP]
   > A meno che tu non usi la [tolleranza di errore tramite chassis o rack](../../failover-clustering/fault-domains.md), il numero di domini di errore indica il numero di server. Il numero di unità in ogni server non influisce sui tipi di resilienza che puoi usare, a patto che siano soddisfatti i requisiti minimi per Spazi di archiviazione diretta. 

### <a name="dual-parity-efficiency-for-hybrid-deployments"></a>Efficienza della doppia parità per le distribuzioni ibride

Questa tabella mostra l'efficienza di archiviazione della doppia parità e dei codici di ricostruzione locali per distribuzioni ibride di dimensioni diverse contenenti sia unità disco rigido (HDD) che unità SSD.

|    Domini di errore      |    Layout           |    Efficienza   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50%        |
|    5                  |    RS 2+2           |    50%        |
|    6                  |    RS 2+2           |    50%        |
|    7                  |    RS 4+2           |    66,7%        |
|    8                  |    RS 4+2           |    66,7%        |
|    9                  |    RS 4+2           |    66,7%        |
|    10                 |    RS 4+2           |    66,7%        |
|    11                 |    RS 4+2           |    66,7%        |
|    12                 |    LRC (8, 2, 1)    |    72,7%        |
|    13                 |    LRC (8, 2, 1)    |    72,7%        |
|    14                 |    LRC (8, 2, 1)    |    72,7%        |
|    15                 |    LRC (8, 2, 1)    |    72,7%        |
|    16                 |    LRC (8, 2, 1)    |    72,7%        |

### <a name="dual-parity-efficiency-for-all-flash-deployments"></a>Efficienza della doppia parità per le distribuzioni all-flash

Questa tabella mostra l'efficienza di archiviazione della doppia parità e dei codici di ricostruzione locali per distribuzioni all-flash di dimensioni diverse contenenti solo unità SSD. Il layout con parità può usare gruppi con dimensioni maggiori e ottenere maggiore efficienza di archiviazione in una configurazione all-flash.

|    Domini di errore      |    Layout           |    Efficienza   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50%        |
|    5                  |    RS 2+2           |    50%        |
|    6                  |    RS 2+2           |    50%        |
|    7                  |    RS 4+2           |    66,7%        |
|    8                  |    RS 4+2           |    66,7%        |
|    9                  |    RS 6+2           |    75%        |
|    10                 |    RS 6+2           |    75%        |
|    11                 |    RS 6+2           |    75%        |
|    12                 |    RS 6+2           |    75%        |
|    13                 |    RS 6+2           |    75%        |
|    14                 |    RS 6+2           |    75%        |
|    15                 |    RS 6+2           |    75%        |
|    16                 |    LRC (12, 2, 1)   |    80%        |

## <a name="examples"></a>Esempi

A meno che tu non abbia solo due server, ti consigliamo di usare il mirroring a 3 vie e/o la doppia parità, perché offrono una migliore tolleranza di errore. In particolare, garantiscono che tutti i dati siano sicuri e sempre accessibili, anche nel caso in cui si verifichino errori contemporaneamente in due domini di errore con Spazi di archiviazione diretta, ovvero due server.

### <a name="examples-where-everything-stays-online"></a>Esempi di casi in cui tutti i componenti rimangono online

Questi sei esempi mostrano le situazioni che **possono** essere tollerate dal mirroring a 3 vie e/o dalla doppia parità.

- **1.**    Perdita di un'unità (incluse le unità cache)
- **2.**    Un server è andato perso

![esempi 1 e 2 relativi alla tolleranza di errore](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-12.png)

- **3.**    Un server e un'unità perdute
- **4.**    Due unità perdute in server diversi

![esempi 3 e 4 relativi alla tolleranza di errore](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-34.png)

- **5.**    Sono state perse più di due unità, purché siano interessati al massimo due server
- **6.**    Due server persi

![esempi 5 e 6 relativi alla tolleranza di errore](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-56.png)

...in tutti i casi, tutti i volumi rimangono online. Assicurati che il cluster mantenga il quorum.

### <a name="examples-where-everything-goes-offline"></a>Esempi di casi in cui tutti i componenti passano offline

Nel tempo, Spazi di archiviazione può tollerare un numero qualsiasi di errori, perché ripristina la resilienza completa dopo ognuno di essi, a condizione che ci sia un tempo sufficiente per farlo. Tuttavia, in un momento specifico sono tollerati errori in un massimo di due domini di errore. Gli esempi seguenti mostrano le situazioni che **non possono** essere tollerate dal mirroring a 3 vie e/o dalla doppia parità.

- **7.** Unità perse in tre o più server contemporaneamente
- **8.** Tre o più server persi contemporaneamente

![esempi 7 e 8 relativi alla tolleranza di errore](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-78.png)

## <a name="usage"></a>Utilizzo

Consulta [Creazione di volumi in Spazi di archiviazione diretta](create-volumes.md).

## <a name="see-also"></a>Vedere anche

Ognuno dei link seguenti è presente anche nel corpo di questo argomento.

- [Spazi di archiviazione diretta in Windows Server 2016](storage-spaces-direct-overview.md)
- [Consapevolezza del dominio di errore in Windows Server 2016](../../failover-clustering/fault-domains.md)
- [Cancellazione della codifica in Azure da Microsoft Research](https://www.microsoft.com/en-us/research/publication/erasure-coding-in-windows-azure-storage/)
- [Codici di ricostruzione locali e accelerazione di volumi di parità](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/)
- [Volumi nell'API di gestione dell'archiviazione](https://blogs.technet.microsoft.com/filecab/2016/08/29/deep-dive-volumes-in-spaces-direct/)
- [Demo sull'efficienza dell'archiviazione in Microsoft Ignite 2016](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s)
- [ANTEPRIMA del calcolatore di capacità per Spazi di archiviazione diretta](http://aka.ms/s2dcalc)
