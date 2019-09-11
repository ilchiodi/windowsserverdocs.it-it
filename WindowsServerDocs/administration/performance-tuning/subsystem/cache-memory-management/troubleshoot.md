---
title: Risolvere i problemi di prestazioni della cache e del gestore della memoria
description: Risolvere i problemi di prestazioni della cache e del gestore della memoria in Windows Server 16
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 56871924311a945d62fef8a7ef7231889ba018cc
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866409"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Risolvere i problemi di prestazioni della cache e del gestore della memoria

Prima di Windows Server 2012, due potenziali problemi principali causavano la crescita della cache dei file di sistema fino a quando la memoria disponibile era quasi esaurita in determinati carichi di lavoro. Quando questa situazione causa un rallentamento del sistema, è possibile determinare se il server si trova in uno di questi problemi.


## <a name="counters-to-monitor"></a>Contatori da monitorare

-   \\ Durata&lt; media della cache di standby a lungo termine della memoria 1800 secondi

-   MB\\di memoria disponibili insufficiente

-   Byte\\residenti cache del sistema di memoria

Se la\\memoria disponibile in MB è bassa e allo stesso\\tempo i byte residenti della cache del sistema utilizzano una parte significativa della memoria fisica, è possibile utilizzare [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) per individuare il tipo di utilizzo della cache.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>La cache dei file di sistema contiene le strutture di dati del metafile NTFS


Questo problema è indicato da un numero molto elevato di pagine di metafile attive nell'output di RAMMAP, come illustrato nella figura seguente. Questo problema potrebbe essere stato osservato nei server occupati con milioni di file a cui si accede, causando pertanto la memorizzazione nella cache dei dati di metafile NTFS che non vengono rilasciati dalla cache.

![visualizzazione RamMap](../../media/perftune-guide-rammap.png)

Il problema è stato mitigato dallo strumento *DynCache* . In Windows Server 2012 + l'architettura è stata riprogettata e questo problema non dovrebbe essere più disponibile.

## <a name="system-file-cache-contains-memory-mapped-files"></a>Cache dei file di sistema contiene file mappati alla memoria


Questo problema è indicato da un numero molto elevato di pagine di file con mapping attivo nell'output di RAMMAP. Questo indica in genere che alcune applicazioni nel server aprono molti file di grandi dimensioni usando l'API [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) con\_il\_flag\_di accesso casuale flag di file impostato.

Questo problema è descritto in dettaglio nell'articolo della Knowledge [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369). Flag\_file\_flag\_di accesso casuale è un suggerimento per il gestore della cache per la conservazione delle visualizzazioni mappate del file in memoria il più a lungo possibile (fino a quando il gestore della memoria non segnala una condizione di memoria insufficiente). Allo stesso tempo, questo flag indica a gestione cache di disabilitare la prelettura dei dati del file.

Questa situazione è stata attenuata in una certa misura grazie alla working set i miglioramenti apportati a Windows Server 2012 +, ma il problema stesso deve essere risolto principalmente dal fornitore dell'applicazione\_,\_senza usare flag di file casuali\_.Accesso. Una soluzione alternativa per il fornitore dell'app potrebbe consistere nell'usare una priorità di memoria insufficiente per l'accesso ai file. Questa operazione può essere eseguita usando l'API [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) . Le pagine a cui si accede con priorità di memoria insufficiente vengono rimosse dal working set in modo più aggressivo.

Gestione cache, a partire da Windows Server 2016 attenua ulteriormente questo problema ignorando FILE_FLAG_RANDOM_ACCESS durante le decisioni di taglio, in modo che venga trattato esattamente come qualsiasi altro file aperto senza il Flag FILE_FLAG_RANDOM_ACCESS (il gestore della cache ancora rispetta questa flag per disabilitare la prelettura dei dati del file. È comunque possibile causare un sovraccarico della cache di sistema se si dispone di un numero elevato di file aperti con questo flag ed è possibile accedervi in modo davvero casuale. Si consiglia vivamente di non usare FILE_FLAG_RANDOM_ACCESS per le applicazioni.
