---
title: Considerazioni di simmetria di unità per spazi di archiviazione diretta
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866882"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>Considerazioni di simmetria di unità per spazi di archiviazione diretta 

> Si applica a: Windows Server 2019, Windows Server 2016

[Spazi di archiviazione diretta](storage-spaces-direct-overview.md) funziona meglio quando ogni server dispone di esattamente le stesse unità.

In realtà, è possibile riconoscere che questo non è sempre pratico: Spazi di archiviazione diretta è progettato per l'esecuzione da anni e per la scalabilità man mano che aumentano le esigenze della propria organizzazione. Oggi, si potrebbe acquistare spazioso 3 TB unità disco rigido; anno successivo, può risultare impossibile da trovare quelle così piccolo. Di conseguenza, alcune quantità di combinazione di versioni diverse è supportato.

Questo argomento illustra i vincoli e vengono forniti esempi di configurazioni supportate e non supportati.

## <a name="constraints"></a>Vincoli

### <a name="type"></a>Tipo

Tutti i server devono avere la stessa [tipi di unità](choosing-drives.md#drive-types).

Ad esempio, se un server ha NVMe, dovrebbe *tutti* hanno NVMe.

### <a name="number"></a>Numero

Tutti i server devono avere lo stesso numero di unità di ogni tipo.

Ad esempio, se un server dispone di sei unità SSD, si dovrebbe *tutti* dispone di sei unità SSD.

   > [!NOTE]
   > È accettabile per il numero di unità diversa temporaneamente durante errori o durante l'aggiunta o rimozione di unità.

### <a name="model"></a>Modello

È consigliabile usare unità dello stesso modello e versione del firmware, laddove possibile. Se non è possibile, scegliere con attenzione le unità che sono il più possibile simili. Se ne sconsiglia l'unità di combinazione di versioni diverse dello stesso tipo con caratteristiche di prestazioni o resistenza nettamente diverse (a meno che una cache e l'altra capacità) perché spazi di archiviazione diretta consente di distribuire uniformemente i/o e non discriminare basato su modello .

   > [!NOTE]
   > È accettabile per unità SATA e SAS simili e combinare.

### <a name="size"></a>Dimensione

È consigliabile usare le unità delle dimensioni dello stesso, laddove possibile. Utilizzo di unità di capacità di dimensioni diverse può comportare alcune capacità inutilizzabile e tramite unità di cache di dimensioni diverse potrebbero non migliorare le prestazioni della cache. Vedere la sezione successiva per informazioni dettagliate.

   > [!WARNING]
   > Dimensioni unità di capacità diversi tra i server potrebbero capacità abbandonate.

## <a name="understand-capacity-imbalance"></a>Comprendere: lo sbilanciamento delle capacità

Spazi di archiviazione diretta è efficace per lo sbilanciamento delle capacità tra le unità e tra server. Anche se è grave sbilanciamento, tutto ciò che continueranno a funzionare. Tuttavia, in base a diversi fattori, capacità di cui non è disponibile in ogni server potrebbe non essere utilizzabile.

Per visualizzare il motivo per cui in questo caso, prendere in considerazione l'illustrazione semplificata seguente. Ogni casella colorata rappresenta una copia di dati con mirroring. Ad esempio, le caselle contrassegnato A, oggetto ' e un ' sono tre copie degli stessi dati. Per rispettare tolleranza di errore di server, queste copie *necessario* essere archiviati in server diversi.

### <a name="stranded-capacity"></a>Capacità abbandonate

Quando viene disegnato, Server 1 (10 TB) e Server 2 (10 TB) sono pieni. Server 3 include unità più grande, pertanto la capacità totale è più grande (15 TB). Tuttavia, per archiviare più dati di tre vie su Server 3 richiederebbe copie nel Server 1 e 2 Server, che sono già pieni. Non è possibile usare la capacità di 5 TB rimanente nel Server 3 – si tratta *"bloccata"* capacità.

![Tre vie, tre server, nei guai capacità](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>Posizionamento ottima

Al contrario, con quattro server di 10 TB, 10 TB, 10 TB e 15 TB di capacità e resilienza mirror a tre vie, si *è* possibili collocare una copia munita in modo che utilizzi tutta la capacità disponibile, come disegnata. Ogni volta che è possibile, l'allocatore di spazi di archiviazione diretta verrà Trova e Usa il posizionamento ottima, non lasciando alcuna capacità abbandonati.

![Tre vie, quattro server, alcuna capacità abbandonati](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

Il numero di server, la resilienza, la gravità dello sbilanciamento di capacità e di altri fattori influisce sulla capacità abbandonati. **La regola generale più prudente consiste nel presupporre che solo la capacità disponibile in ogni server è garantito che sia utilizzabile.**

## <a name="understand-cache-imbalance"></a>Informazioni: lo sbilanciamento delle cache

Spazi di archiviazione diretta è efficace per lo sbilanciamento delle cache in unità e tra server. Anche se è grave sbilanciamento, tutto ciò che continueranno a funzionare. Inoltre, spazi di archiviazione diretta Usa sempre tutte le cache disponibili appieno le funzionalità.

Tuttavia, utilizzando le unità di cache di dimensioni diverse potrà non migliorare le prestazioni della cache in modo uniforme o in modo prevedibile: solo i/o al [drive associazioni](understand-the-cache.md#server-side-architecture) con cache più grande unità potrebbe essere visualizzato miglioramento delle prestazioni. Spazi di archiviazione diretta distribuisce i/o in modo uniforme tra le associazioni e non discriminare in base alle proporzioni alla capacità della cache.

![Sbilanciamento delle cache](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Visualizzare [informazioni sulla cache](understand-the-cache.md) per altre informazioni sulle associazioni di cache.

## <a name="example-configurations"></a>Configurazioni di esempio

Ecco alcune configurazioni supportati e non supportati:

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-between-servers"></a>![supportata](media/drive-symmetry-considerations/supported.png) Supportati: diversi modelli tra server

I primi due server usare il modello NVMe "X", ma il terzo server usa il modello di NVMe "Z", che è molto simile.

| Server 1                    | Server 2                    | Server 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x modello NVMe X (cache)    | 2 x modello NVMe X (cache)    | 2 x NVMe modello Z (cache)    |
| 10 volte il modello di unità SSD Y (capacità) | 10 volte il modello di unità SSD Y (capacità) | 10 volte il modello di unità SSD Y (capacità) |

Questa è supportata.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-within-server"></a>![supportata](media/drive-symmetry-considerations/supported.png) Supportati: modelli diversi all'interno di server

Ogni server utilizza alcune diversa combinazione di modelli di unità disco rigido "Y" e "Z", che sono molto simili. Ogni server dispone di 10 unità HDD totali.

| Server 1                   | Server 2                   | Server 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x modello SSD X (cache)    | 2 x modello SSD X (cache)    | 2 x modello SSD X (cache)    |
| 7 giorni su HDD modello Y (capacità) | 5 x unità HDD modello Y (capacità) | 1 x unità HDD modello Y (capacità) |
| 3x HDD modello Z (capacità) | 5 x unità HDD modello Z (capacità) | 9 x unità HDD modello Z (capacità) |

Questa è supportata.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-across-servers"></a>![supportata](media/drive-symmetry-considerations/supported.png) Supportati: dimensioni diverse tra server

I primi due server usano HDD da 4 TB, ma il terzo server utilizzato molto simili 6 TB del disco rigido.

| Server 1                | Server 2                | Server 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) |
| 4 x 4 TB (capacità) di unità disco rigido | 4 x 4 TB (capacità) di unità disco rigido | 4 x 6 TB (capacità) di unità disco rigido |

Questa è supportata, anche se si verificherà capacità abbandonate.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-within-server"></a>![supportata](media/drive-symmetry-considerations/supported.png) Supportati: dimensioni diverse all'interno di server

Ogni server utilizza alcune diversa combinazione di 1,2 TB e molto simile 1,6 TB unità SSD. Ogni server dispone di 4 unità SSD totali.

| Server 1                 | Server 2                 | Server 3                 |
|--------------------------|--------------------------|--------------------------|
| 3 x 1,2 TB unità SSD (cache)   | 2 x 1,2 TB unità SSD (cache)   | 4 x 1,2 TB unità SSD (cache)   |
| 1,6 1 TB, unità SSD (cache)   | TB di 1,6 2 unità SSD (cache)   | -                        |
| 20 x 4 TB (capacità) di unità disco rigido | 20 x 4 TB (capacità) di unità disco rigido | 20 x 4 TB (capacità) di unità disco rigido |

Questa è supportata.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-types-of-drives-across-servers"></a>![non supportato](media/drive-symmetry-considerations/unsupported.png) Non è supportato: tipi diversi di unità tra server

Server 1 dispone NVMe ma non gli altri.

| Server 1            | Server 2            | Server 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)    | -                   | -                   |
| -                   | 6 x SSD (cache)     | 6 x SSD (cache)     |
| 18 x HDD (capacità) | 18 x HDD (capacità) | 18 x HDD (capacità) |

Non è supportato. I tipi di unità devono essere identico in ogni server.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-number-of-each-type-across-servers"></a>![non supportato](media/drive-symmetry-considerations/unsupported.png) Non è supportato: un numero diverso di ogni tipo tra server

Server 3 include più unità rispetto agli altri.

| Server 1            | Server 2            | Server 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)    | 2 x NVMe (cache)    | 4 x NVMe (cache)    |
| 10 x HDD (capacità) | 10 x HDD (capacità) | 20 x HDD (capacità) |

Non è supportato. Il numero di unità di ogni tipo deve essere identico in ogni server.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-only-hdd-drives"></a>![non supportato](media/drive-symmetry-considerations/unsupported.png) Non è supportato: solo le unità HDD

Tutti i server hanno solo le unità HDD connessi.

|Server 1|Server 2|Server 3|
|-|-|-| 
|18 x HDD (capacità) |18 x HDD (capacità)|18 x HDD (capacità)|

Non è supportato. È necessario aggiungere almeno due unità di cache (NvME o unità SSD) collegati a ogni server.

## <a name="summary"></a>Riepilogo

Ricapitolando, ogni server del cluster deve avere gli stessi tipi di unità e lo stesso numero di ogni tipo. È supportata per i modelli e combinare unità e le dimensioni delle unità in base alle esigenze, tenendo presente quanto riportato sopra.

| Vincolo                               |               |
|------------------------------------------|---------------|
| Stessi tipi di unità in ogni server     | **Obbligatorio**  |
| Lo stesso numero di ogni tipo in tutti i server | **Obbligatorio**  |
| Modelli di unità stessa in tutti i server        | Consigliato   |
| Dimensioni delle unità stessa in tutti i server         | Consigliato   |

## <a name="see-also"></a>Vedere anche

- [Requisiti hardware diretto di spazi di archiviazione](storage-spaces-direct-hardware-requirements.md)
- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)
