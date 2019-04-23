---
title: Risolvere i problemi di prestazioni di gestione della memoria e Cache
description: Risolvere i problemi di Cache e i problemi di prestazioni di gestione della memoria in Windows Server 16
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 66c7e2a6b264a837c65df927b271fadd2672fa24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835792"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Risolvere i problemi di prestazioni di gestione della memoria e Cache

Prima di Windows Server 2012, due principali problemi potenziali causato cache del file system a crescere fino a quando la memoria disponibile è stata quasi esaurita in determinati carichi di lavoro. Quando il risultato situazione è il sistema in corso lenta, è possibile determinare se il server è rivolta a uno di questi problemi.


## <a name="counters-to-monitor"></a>Contatori da monitorare

-   Memoria\\Long-Term durata media di Standby della Cache (s) &lt; 1800 secondi

-   Memoria\\MByte disponibili è insufficiente

-   Memoria\\Byte residenti Cache di sistema

Se memoria\\MByte disponibili è basso e allo stesso tempo memoria\\Byte residenti Cache di sistema utilizza parte significativa della memoria fisica, è possibile usare [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) per scoprire quali la cache viene utilizzata per.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>Cache del file System contiene strutture di dati NTFS metafile


Questo problema è indicato da un numero molto elevato di pagine attivi Metafile RAMMAP di output, come illustrato nella figura seguente. Questo problema potrebbe essere osservato nei server occupato con milioni di file di cui si accede, risultante in tal modo la memorizzazione nella cache i dati di metafile NTFS non viene rilasciati dalla cache.

![visualizzazione rammap](../../media/perftune-guide-rammap.png)

Il problema consente di essere attenuata *DynCache* dello strumento. In Windows Server 2012 e l'architettura è stata riprogettata e questo problema non è più incluso.

## <a name="system-file-cache-contains-memory-mapped-files"></a>Cache del file System contiene file mappati alla memoria


Questo problema è indicato da un numero molto elevato di pagine di file con mapping attivi nell'output RAMMAP. Questo in genere significa che alcune applicazioni nel server sta aprendo numerosi file di grandi dimensioni mediante [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) API con FILE\_FLAG\_RANDOM\_set di flag di accesso.

Questo problema è descritto in dettaglio nell'articolo della Knowledge Base [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369). FILE\_FLAG\_RANDOM\_flag di accesso è un hint per la gestione della Cache per mantenere le viste con mapping del file nella memoria fin quando possibile (fino a quando Memory Manager non segnalare la condizione di memoria insufficiente). Allo stesso tempo, questo flag indica a gestione della Cache per disabilitare la prelettura dei dati dei file.

Questa situazione è stata attenuata in una certa misura la possibilità di lavorare insieme trimming miglioramenti in Windows Server 2012 +, ma il problema stesso deve essere affrontata principalmente dal fornitore dell'applicazione, non usare FILE\_FLAG\_RANDOM\_L'accesso. Una soluzione alternativa per il fornitore dell'app potrebbe consistere nell'utilizzare la priorità di memoria insufficiente durante l'accesso a file. Ciò può essere ottenuto tramite il [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) API. Le pagine che sono accessibili con priorità di memoria insufficiente vengono rimosse dal working set in modo più aggressivo.

Gestione della cache, a partire da Windows Server 2016 ulteriormente mitiga ignorando FILE_FLAG_RANDOM_ACCESS prendere decisioni di taglio, pertanto viene considerato come qualsiasi altro file aperto senza il flag FILE_FLAG_RANDOM_ACCESS (gestione della Cache ancora rispetta questo flag per disabilitare la prelettura di dati di file). Si possono causare bloat cache di sistema se si dispone di un numero elevato di file aperto con questo flag e vi si accede in modo casuale. È consigliabile FILE_FLAG_RANDOM_ACCESS non utilizzate dalle applicazioni.
