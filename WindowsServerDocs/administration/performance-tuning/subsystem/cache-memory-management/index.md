---
title: Ottimizzazione delle prestazioni per i sottosistemi di gestione cache e memoria
description: Ottimizzazione delle prestazioni per i sottosistemi di gestione cache e memoria
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: c914768378303b8a36cb2e3ec468e853296249a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370024"
---
# <a name="performance-tuning-cache-and-memory-manager"></a>Ottimizzazione delle prestazioni per la gestione di cache e memoria

Per impostazione predefinita, Windows memorizza nella cache i dati di file che vengono letti dai dischi e scritti sui dischi. Ciò implica che le operazioni di lettura leggano i dati dei file da un'area della memoria di sistema, nota come cache dei file di sistema, anziché dal disco fisico. Analogamente, le operazioni di scrittura scrivono i dati dei file nella cache dei file di sistema anziché sul disco e questo tipo di cache è noto come cache write-back. La memorizzazione nella cache viene gestita per ogni oggetto file. La memorizzazione nella cache viene eseguita sotto il controllo del gestore della cache, che opera in modo continuo mentre è in esecuzione Windows.

I dati dei file nella cache dei file di sistema vengono scritti sul disco a intervalli determinati dal sistema operativo. Le pagine scaricate restano nel working set della cache di sistema (se FILE\_FLAG\_RANDOM\_ACCESS è impostato e l'handle del file non è stato chiuso) oppure nell'elenco standby dove diventano parte della memoria disponibile.

La strategia che consiste nel ritardare la scrittura dei dati nel file e nel mantenerli nella cache finché questa non viene scaricata è nota come scrittura lazy e viene attivata dal gestore della cache a un determinato intervallo di tempo. Il momento in cui un blocco di dati del file viene scaricato dipende in parte da quanto tempo è rimasto memorizzato nella cache e da quanto tempo è trascorso dall'ultima volta in cui si è avuto accesso ai dati in un'operazione di lettura. Questo garantisce che i dati del file letti frequentemente restino accessibili nella cache dei file di sistema per il massimo intervallo di tempo previsto.

Questo processo di memorizzazione nella cache dei dati del file è illustrato nella figura seguente:

![memorizzazione nella cache dei dati del file](../../media/perftune-guide-file-data-caching.png)

Come indicato dalle righe continue nella figura precedente, un'area di dati da 256 KB viene letta in uno slot della cache da 256 KB nello spazio indirizzi di sistema quando viene richiesta per la prima volta dal gestore della cache durante un'operazione di lettura del file. Un processo in modalità utente quindi copia i dati di questo slot nel proprio spazio indirizzi. Quando il processo ha completato l'accesso ai dati, riscrive i dati modificati nello stesso slot nella cache di sistema, come indicato dalla freccia tratteggiata tra lo spazio indirizzi del processo e la cache di sistema. Quando il gestore della cache determina che i dati non saranno più necessari per un determinato periodo di tempo, riscrive i dati modificati nel file sul disco, come indicato dalla freccia tratteggiata tra la cache di sistema e il disco.

**Contenuto della sezione:**

-   [Potenziali problemi di prestazioni relativi alla gestione della cache e della memoria](troubleshoot.md)

-   [Miglioramenti di gestione della memoria e della cache in Windows Server 2016](improvements-in-2016.md)
