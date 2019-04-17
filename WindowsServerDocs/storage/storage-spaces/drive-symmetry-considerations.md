---
title: Considerazioni sulla simmetria unità per spazi di archiviazione diretta
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: f2ef58003da6de049c7c4b578f789a97e0a0f512
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/26/2018
ms.locfileid: "5591847"
---
# Considerazioni sulla simmetria unità per spazi di archiviazione diretta 

> Si applica a: Windows Server 2019, Windows Server 2016

[Spazi di archiviazione diretta](storage-spaces-direct-overview.md) funziona meglio quando ogni server dispone di esattamente la stessa unità.

In realtà, ci rendiamo conto questo non è sempre pratico: spazi di archiviazione diretta è progettato per funzionare per anni e scale come incrementare le esigenze dell'organizzazione. Oggi, puoi acquistare spazioso 3 TB unità disco rigida. anno successivo, può diventare Impossibile trovate quelle che piccole. Di conseguenza, alcuni quantità di missaggio e corrispondenza è supportato.

Questo argomento illustra i vincoli e fornisce esempi di configurazioni supportate e non supportate.

## Vincoli

### Tipo

Tutti i server devono avere gli stessi [tipi di unità](choosing-drives.md#drive-types).

Ad esempio, se un server ha NVMe, dovrebbe *tutti* hanno NVMe.

### Numero

Tutti i server devono avere lo stesso numero di unità di ogni tipo.

Ad esempio, se un server ha sei unità SSD, dovrebbero *tutti* hanno sei unità SSD.

   > [!NOTE]
   > È accettabile per il numero di unità in base alle diverse temporaneamente durante gli errori o durante l'aggiunta o la rimozione di unità.

### Modello

Ti consigliamo di usare le unità dello stesso modello e versione del firmware, se possibile. Se non è possibile, selezionare con attenzione le unità che sono il più possibile simili. Ti sconsigliamo di missaggio e corrispondenza unità dello stesso tipo con le caratteristiche delle prestazioni o di una resistenza drasticamente diversi (a meno che una cache e l'altro è capacità) perché spazi di archiviazione diretta distribuisce in modo uniforme i/o e non distinguere in base a modelli .

   > [!NOTE]
   > È accettabile alle unità SATA e SAS simili mix e corrispondenza.

### Dimensioni

Ti consigliamo di usare le unità delle stesse dimensioni di ogni volta che è possibile. Uso di unità di capacità di dimensioni diverse potrebbe causare capacità inutilizzabili e con unità cache di dimensioni diverse potrebbero non migliorare le prestazioni della cache. Vedi la sezione successiva per informazioni dettagliate.

   > [!WARNING]
   > Capacità può comportare differenti dimensioni delle unità di capacità tra server.

## Comprendere: squilibrio capacità

Spazi di archiviazione diretta è affidabili per squilibrio capacità tra unità e tra server. Anche se lo squilibrio grave, tutti gli elementi continueranno a funzionare. Tuttavia, a seconda di diversi fattori, capacità non disponibile in ogni server potrebbe non essere utilizzabile.

Per vedere perché in questo caso, Prendi in considerazione la figura semplificata riportato di seguito. Ogni casella colorato rappresenta una copia dei dati con mirroring. Ad esempio, le caselle di controllo contrassegnata come A, A' e un pollici sono tre copie degli stessi dati. In modo da rispettare tolleranza di errore server, queste copie *devono* essere archiviati in server diversi.

### Capacità

Quando viene disegnato, Server 1 (10 TB) e Server 2 (10 TB) sono completo. Server 3 dispone di unità più grandi, pertanto la capacità totale è più grande (15 TB). Tuttavia, per archiviare i dati di mirroring a tre vie altre su Server 3 richiederebbe copie sul Server 1 e 2 Server troppo, che sono già completo. Il 5 TB di capacità rimanenti su Server 3 non può essere usato, ma *"bloccata"* capacità.

![Mirroring a tre vie, tre server, bloccata capacità](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### Posizionamento ottimale

Al contrario, con quattro server di 10 TB, 10 TB, 10 TB e 15 TB di capacità e la resilienza con mirroring a tre vie, *è* possibile inserire copie validamente concesse in modo che usa tutta la capacità disponibile, come disegnato. Ogni volta che ciò è possibile, allocatore di spazi di archiviazione diretta verrà trovare e usare la posizione ottimale, non lasciare alcuna capacità.

![Mirroring a tre vie, quattro server, nessun capacità](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

Influisce sulla capacità è il numero di server, la resilienza, la gravità di sbilanciamento capacità e altri fattori. **La regola generale più prudente è presumere che sia possibile è garantita sola capacità disponibili in ogni server.**

## Comprendere: squilibrio cache

Spazi di archiviazione diretta è affidabili per squilibrio cache tra unità e tra server. Anche se lo squilibrio grave, tutti gli elementi continueranno a funzionare. Inoltre, spazi di archiviazione diretta Usa sempre tutte le cache disponibile appieno le funzionalità.

Tuttavia, con unità cache di dimensioni diverse potrebbero non migliorare le prestazioni della cache in modo uniforme o prevedibile: solo operazioni i/o [le associazioni](understand-the-cache.md#server-side-architecture) con unità cache più grande potrebbe essere visualizzato un miglioramento delle prestazioni. Spazi di archiviazione diretta distribuisce i/o in modo uniforme tra i binding e non distinguere in base a rapporto alla capacità di cache.

![Squilibrio cache](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Visualizzare [informazioni sulla cache](understand-the-cache.md) per ottenere altre informazioni sulle associazioni di cache.

## Configurazioni di esempio

Ecco alcune configurazioni supportate e non supportati:

### ![supportato](media/drive-symmetry-considerations/supported.png) Supportato: modelli diversi tra i server

I primi due server usa il modello di unità NVMe "X", ma il terzo server usa il modello di unità NVMe "Z", che è molto simile.

| Server 1                    | Server 2                    | Server 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe modello X (cache)    | 2 x NVMe modello X (cache)    | 2 x NVMe modello Z (cache)    |
| 10 x SSD modello Y (capacità) | 10 x SSD modello Y (capacità) | 10 x SSD modello Y (capacità) |

Questo è supportato.

### ![supportato](media/drive-symmetry-considerations/supported.png) Supportato: diversi modelli all'interno di server

Ogni server utilizza alcuni tipi diversi di modelli HDD "Y" e "Z", che sono molto simili. Ogni server dispone di 10 unità disco rigido totale.

| Server 1                   | Server 2                   | Server 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x SSD modello X (cache)    | 2 x SSD modello X (cache)    | 2 x SSD modello X (cache)    |
| 7 x HDD modello Y (capacità) | 5 x HDD modello Y (capacità) | 1 x HDD modello Y (capacità) |
| 3 x HDD modello Z (capacità) | 5 x HDD modello Z (capacità) | 9 x HDD modello Z (capacità) |

Questo è supportato.

### ![supportato](media/drive-symmetry-considerations/supported.png) Supportato: dimensioni diverse tra server

I primi due server usare HDD da 4 TB, ma il terzo server usa molto simile 6 TB HDD.

| Server 1                | Server 2                | Server 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) |
| 4 x 4 TB (capacità) HDD | 4 x 4 TB (capacità) HDD | 4 x 6 TB (capacità) HDD |

Questo è supportato, anche se il risultato sarà capacità.

### ![supportato](media/drive-symmetry-considerations/supported.png) Supportato: dimensioni diverse all'interno di server

Ogni server utilizza alcuni tipi diversi di TB 1.2 e molto simile 1,6 TB SSD. Ogni server dispone di 4 unità SSD totale.

| Server 1                 | Server 2                 | Server 3                 |
|--------------------------|--------------------------|--------------------------|
| 1.2 x 3 TB unità SSD (cache)   | 1.2 x 2 TB unità SSD (cache)   | 4 x 1.2 TB unità SSD (cache)   |
| 1 x 1,6 TB unità SSD (cache)   | da 2 x 1,6 TB unità SSD (cache)   | -                        |
| 20 x 4 TB (capacità) HDD | 20 x 4 TB (capacità) HDD | 20 x 4 TB (capacità) HDD |

Questo è supportato.

### ![non supportato](media/drive-symmetry-considerations/unsupported.png) Non supportato: diversi tipi di unità tra server

1 server ha NVMe, ma non gli altri.

| Server 1            | Server 2            | Server 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)    | -                   | -                   |
| -                   | 6 x SSD (cache)     | 6 x SSD (cache)     |
| 18 x HDD (capacità) | 18 x HDD (capacità) | 18 x HDD (capacità) |

Questo non è supportato. I tipi di unità devono essere lo stesso in ogni server.

### ![non supportato](media/drive-symmetry-considerations/unsupported.png) Non supportato: numero diverso di ogni tipo tra server

Server 3 ha altre unità rispetto agli altri.

| Server 1            | Server 2            | Server 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)    | 2 x NVMe (cache)    | 4 x NVMe (cache)    |
| 10 x HDD (capacità) | 10 x HDD (capacità) | 20 x HDD (capacità) |

Questo non è supportato. Il numero delle unità di ogni tipo di debba essere identici in ogni server.

### ![non supportato](media/drive-symmetry-considerations/unsupported.png) Non supportato: solo unità HDD

Tutti i server hanno solo unità HDD connessa.

|Server 1|Server 2|Server 3|
|-|-|-| 
|18 x HDD (capacità) |18 x HDD (capacità)|18 x HDD (capacità)|

Questo non è supportato. Devi aggiungere almeno due unità di cache (NvME o SSD) associati a ogni server.

## Riepilogo

Per riassumere, tutti i server del cluster deve avere gli stessi tipi di unità e lo stesso numero di ogni tipo. È supportato per unità mix-and-match modelli e le dimensioni delle unità in base alle esigenze, con le considerazioni sopra.

| Vincolo                               |               |
|------------------------------------------|---------------|
| Stessi tipi di unità in ogni server     | **Obbligatorio**  |
| Stesso numero di ogni tipo in ogni server | **Obbligatorio**  |
| Stesso modelli di unità in ogni server        | Consigliato   |
| Stesse dimensioni di unità in ogni server         | Consigliato   |

## Vedi anche

- [Requisiti hardware di Spazi di archiviazione diretta](storage-spaces-direct-hardware-requirements.md)
- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
