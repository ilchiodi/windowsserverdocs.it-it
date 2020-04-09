---
title: Considerazioni sulla simmetria dell'unità per Spazi di archiviazione diretta
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
ms.localizationpriority: medium
ms.openlocfilehash: b06d69c020ea38a2fb9f23df2cfd9cd4191ae315
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857554"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>Considerazioni sulla simmetria dell'unità per Spazi di archiviazione diretta 

> Si applica a: Windows Server 2019, Windows Server 2016

[Spazi di archiviazione diretta](storage-spaces-direct-overview.md) funziona meglio quando ogni server ha esattamente le stesse unità.

In realtà, questa situazione non è sempre pratica: Spazi di archiviazione diretta è progettata per essere eseguita per anni e per la scalabilità in base alle esigenze dell'organizzazione. Attualmente, è possibile acquistare unità disco rigido da 3 TB spaziose; il prossimo anno potrebbe non essere più possibile trovare quelli di dimensioni ridotte. È pertanto supportata una certa quantità di combinazioni e corrispondenze.

In questo argomento vengono illustrati i vincoli e vengono forniti esempi di configurazioni supportate e non supportate.

## <a name="constraints"></a>Vincoli

### <a name="type"></a>Type

Tutti i server devono avere gli stessi [tipi di unità](choosing-drives.md#drive-types).

Se, ad esempio, un server dispone di NVMe, *tutti* dovrebbero avere NVMe.

### <a name="number"></a>Numero

Tutti i server devono avere lo stesso numero di unità di ogni tipo.

Se, ad esempio, un server dispone di sei unità SSD, devono avere *tutti* sei unità SSD.

   > [!NOTE]
   > Il numero di unità può variare temporaneamente durante gli errori o durante l'aggiunta o la rimozione di unità.

### <a name="model"></a>Modello

Quando possibile, è consigliabile usare unità dello stesso modello e versione del firmware. Se non è possibile, selezionare con attenzione le unità più simili possibile. Sono sconsigliate le unità combinate e corrispondenti dello stesso tipo con caratteristiche di prestazioni o di resistenza nettamente diverse (a meno che non si tratti di una cache e l'altra è la capacità) perché Spazi di archiviazione diretta distribuisce i/o in modo uniforme e non discrimina in base al modello.

   > [!NOTE]
   > È accettabile combinare unità SATA e SAS analoghe.

### <a name="size"></a>Dimensione

Quando possibile, è consigliabile usare unità con le stesse dimensioni. L'uso di unità di capacità di dimensioni diverse può comportare una capacità inutilizzabile e l'uso di unità cache di dimensioni diverse potrebbe non migliorare le prestazioni della cache. Per informazioni dettagliate, vedere la sezione successiva.

   > [!WARNING]
   > Le dimensioni delle unità di capacità diverse tra i server possono comportare una capacità incagliata.

## <a name="understand-capacity-imbalance"></a>Informazioni: squilibrio della capacità

Spazi di archiviazione diretta è affidabile per lo squilibrio della capacità tra le unità e tra i server. Anche se lo squilibrio è grave, tutto continuerà a funzionare. Tuttavia, a seconda di diversi fattori, la capacità che non è disponibile in ogni server potrebbe non essere utilizzabile.

Per capire il motivo per cui si verifica questo problema, considerare l'illustrazione semplificata riportata di seguito Ogni casella colorata rappresenta una copia dei dati con mirroring. Ad esempio, le caselle contrassegnate come, A ' è ' sono tre copie degli stessi dati. Per rispettare la tolleranza di errore del server, è *necessario* che queste copie siano archiviate in server diversi.

### <a name="stranded-capacity"></a>Capacità incagliata

Come disegnato, il server 1 (10 TB) e il server 2 (10 TB) sono pieni. Il server 3 dispone di unità di dimensioni maggiori, quindi la capacità totale è maggiore (15 TB). Tuttavia, per archiviare più dati mirror a tre vie sul server 3, è necessario eseguire copie anche in Server 1 e server 2, che sono già completi. Non è possibile usare la capacità rimanente di 5 TB sul server 3, ovvero la capacità *"Stranded"* .

![Mirroring a tre vie, tre server, capacità incagliata](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>Posizionamento ottimale

Viceversa, con quattro server di 10 TB, 10 TB, 10 TB e 15 TB di capacità e resilienza del mirror a tre vie, *è* possibile inserire in modo valido le copie in modo da usare tutta la capacità disponibile, come disegnato. Ogni volta che è possibile, il Spazi di archiviazione diretta allocatore troverà e utilizzerà la posizione ottimale, senza capacità incagliata.

![Mirroring a tre vie, quattro server, capacità senza fili](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

Il numero di server, la resilienza, la gravità dello squilibrio della capacità e altri fattori influiscono sull'eventuale presenza di capacità incagliata. **La regola generale più prudente consiste nel presupporre che sia possibile utilizzare solo la capacità disponibile in ogni server.**

## <a name="understand-cache-imbalance"></a>Informazioni: squilibrio della cache

Spazi di archiviazione diretta è affidabile per la cache dello squilibrio tra le unità e tra i server. Anche se lo squilibrio è grave, tutto continuerà a funzionare. Inoltre, Spazi di archiviazione diretta utilizza sempre tutta la cache disponibile al più completo.

Tuttavia, l'utilizzo di unità cache di dimensioni diverse potrebbe non migliorare le prestazioni della cache in modo uniforme o prevedibile: solo i/o per le [associazioni](understand-the-cache.md#server-side-architecture) con unità cache più grandi possono determinare prestazioni migliori. Spazi di archiviazione diretta distribuisce le operazioni di i/o in modo uniforme tra le associazioni e non discrimina in base al rapporto tra cache e capacità.

![Squilibrio della cache](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Per ulteriori informazioni sulle associazioni della cache, vedere [informazioni sulla cache](understand-the-cache.md) .

## <a name="example-configurations"></a>Configurazioni di esempio

Ecco alcune configurazioni supportate e non supportate:

### <a name="supported-supported-different-models-between-servers"></a>![supportati](media/drive-symmetry-considerations/supported.png) Supportato: modelli diversi tra server

I primi due server utilizzano il modello NVMe "X", ma il terzo server utilizza il modello NVMe "Z", che è molto simile.

| Server 1                    | Server 2                    | Server 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe modello X (cache)    | 2 x NVMe modello X (cache)    | 2 x modello NVMe Z (cache)    |
| Modello unità SSD 10 x (capacità) | Modello unità SSD 10 x (capacità) | Modello unità SSD 10 x (capacità) |

Questa configurazione è supportata.

### <a name="supported-supported-different-models-within-server"></a>![supportati](media/drive-symmetry-considerations/supported.png) Supportato: modelli diversi all'interno del server

Ogni server utilizza una combinazione diversa di modelli HDD "Y" e "Z", che sono molto simili. Ogni server ha 10 unità disco rigido totali.

| Server 1                   | Server 2                   | Server 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x modello SSD X (cache)    | 2 x modello SSD X (cache)    | 2 x modello SSD X (cache)    |
| 7 x modello HDD Y (capacità) | 5 x modello HDD Y (capacità) | 1 x modello HDD Y (capacità) |
| 3 x modello HDD Z (capacità) | 5 x modello HDD Z (capacità) | 3 x modello HDD Z (capacità) |

Questa configurazione è supportata.

### <a name="supported-supported-different-sizes-across-servers"></a>![supportati](media/drive-symmetry-considerations/supported.png) Supportato: dimensioni diverse tra i server

I primi due server utilizzano HDD da 4 TB, ma il terzo server utilizza un HDD di 6 TB molto simile.

| Server 1                | Server 2                | Server 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) |
| 4 x 4 TB HDD (capacità) | 4 x 4 TB HDD (capacità) | 4 x 6 TB HDD (capacità) |

Questa funzionalità è supportata, anche se comporterà una capacità incagliata.

### <a name="supported-supported-different-sizes-within-server"></a>![supportati](media/drive-symmetry-considerations/supported.png) Supportato: dimensioni diverse all'interno del server

Ogni server usa una combinazione diversa di 1,2 TB e un SSD 1,6 TB molto simile. Ogni server dispone di 4 unità SSD totali.

| Server 1                 | Server 2                 | Server 3                 |
|--------------------------|--------------------------|--------------------------|
| SSD da 3 x 1,2 TB (cache)   | unità SSD da 2 x 1,2 TB (cache)   | unità SSD da 4 x 1,2 TB (cache)   |
| unità SSD da 1 x 1,6 TB (cache)   | unità SSD da 2 x 1,6 TB (cache)   | -                        |
| HDD da 20 x 4 TB (capacità) | HDD da 20 x 4 TB (capacità) | HDD da 20 x 4 TB (capacità) |

Questa configurazione è supportata.

### <a name="unsupported-not-supported-different-types-of-drives-across-servers"></a>![non supportati](media/drive-symmetry-considerations/unsupported.png) Non supportato: tipi diversi di unità tra server

Il server 1 ha NVMe, ma non gli altri.

| Server 1            | Server 2            | Server 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)    | -                   | -                   |
| -                   | unità SSD 6 x (cache)     | unità SSD 6 x (cache)     |
| 18 x HDD (capacità) | 18 x HDD (capacità) | 18 x HDD (capacità) |

Questa operazione non è supportata. I tipi di unità dovrebbero essere gli stessi in ogni server.

### <a name="unsupported-not-supported-different-number-of-each-type-across-servers"></a>![non supportati](media/drive-symmetry-considerations/unsupported.png) Non supportato: numero diverso di ogni tipo tra server

Il server 3 ha più unità rispetto alle altre.

| Server 1            | Server 2            | Server 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)    | 2 x NVMe (cache)    | 4 x NVMe (cache)    |
| HDD 10 x (capacità) | HDD 10 x (capacità) | 20 x HDD (capacità) |

Questa operazione non è supportata. Il numero di unità di ogni tipo deve essere lo stesso in ogni server.

### <a name="unsupported-not-supported-only-hdd-drives"></a>![non supportati](media/drive-symmetry-considerations/unsupported.png) Non supportato: solo unità HDD

Tutti i server hanno solo unità HDD connesse.

|Server 1|Server 2|Server 3|
|-|-|-| 
|18 x HDD (capacità) |18 x HDD (capacità)|18 x HDD (capacità)|

Questa operazione non è supportata. È necessario aggiungere almeno due unità cache (NvME o SSD) collegate a ogni server.

## <a name="summary"></a>Riepilogo

Per ricapitolare, ogni server del cluster deve avere gli stessi tipi di unità e lo stesso numero di ogni tipo. È supportata per combinare modelli di unità e dimensioni di unità in base alle esigenze, con le considerazioni precedenti.

| Vincolo                               |               |
|------------------------------------------|---------------|
| Gli stessi tipi di unità in ogni server     | **Obbligatorio**  |
| Stesso numero di ogni tipo in ogni server | **Obbligatorio**  |
| Stessi modelli di unità in ogni server        | Consigliato   |
| Dimensioni delle unità in ogni server         | Consigliato   |

## <a name="see-also"></a>Vedere anche

- [Requisiti hardware Spazi di archiviazione diretta](storage-spaces-direct-hardware-requirements.md)
- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
