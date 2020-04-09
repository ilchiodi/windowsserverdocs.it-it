---
title: Risolvere i problemi di prestazioni della cache e del gestore della memoria
description: Risolvere i problemi di prestazioni della cache e del gestore della memoria in Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: pavel; atales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0b01808564cfaf1eaedf30a66c774e2228205847
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851654"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Risolvere i problemi di prestazioni della cache e del gestore della memoria

Prima di Windows Server 2012, due potenziali problemi principali causavano la crescita della cache dei file di sistema fino a quando la memoria disponibile era quasi esaurita in determinati carichi di lavoro. Quando questa situazione causa un rallentamento del sistema, è possibile determinare se il server si trova in uno di questi problemi.


## <a name="counters-to-monitor"></a>Contatori da monitorare

-   Memoria\\durata media della cache di standby a lungo termine &lt; 1800 secondi

-   Memoria\\MByte disponibili insufficiente

-   Byte residenti cache di sistema\\memoria

Se la memoria\\MByte disponibili è bassa e allo stesso tempo la memoria\\Byte residenti nella cache di sistema sta consumando una parte significativa della memoria fisica, è possibile usare [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) per individuare il tipo di utilizzo della cache.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>La cache dei file di sistema contiene le strutture di dati del metafile NTFS


Questo problema è indicato da un numero molto elevato di pagine di metafile attive nell'output di RAMMAP, come illustrato nella figura seguente. Questo problema potrebbe essere stato osservato nei server occupati con milioni di file a cui si accede, causando pertanto la memorizzazione nella cache dei dati di metafile NTFS che non vengono rilasciati dalla cache.

![visualizzazione RamMap](../../media/perftune-guide-rammap.png)

Il problema è stato mitigato dallo strumento *DynCache* . In Windows Server 2012 + l'architettura è stata riprogettata e questo problema non dovrebbe essere più disponibile.

## <a name="system-file-cache-contains-memory-mapped-files"></a>Cache dei file di sistema contiene file mappati alla memoria


Questo problema è indicato da un numero molto elevato di pagine di file con mapping attivo nell'output di RAMMAP. Questo indica in genere che alcune applicazioni nel server aprono molti file di grandi dimensioni usando l'API [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) con il flag di\_file\_set di flag di accesso\_casuale.

Questo problema è descritto in dettaglio nell'articolo della Knowledge [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369). FLAG di\_FILE\_flag di accesso casuale\_è un suggerimento per il gestore della cache per la conservazione delle visualizzazioni mappate del file in memoria il più a lungo possibile (fino a quando il gestore della memoria non segnala una condizione di memoria insufficiente). Allo stesso tempo, questo flag indica a gestione cache di disabilitare la prelettura dei dati del file.

Questa situazione è stata attenuata in una certa misura grazie alla working set i miglioramenti apportati a Windows Server 2012 +, ma il problema stesso deve essere risolto principalmente dal fornitore dell'applicazione, non utilizzando il FLAG\_FILE\_l'accesso\_casuale. Una soluzione alternativa per il fornitore dell'app potrebbe consistere nell'usare una priorità di memoria insufficiente per l'accesso ai file. Questa operazione può essere eseguita usando l'API [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) . Le pagine a cui si accede con priorità di memoria insufficiente vengono rimosse dal working set in modo più aggressivo.

Gestione cache, a partire da Windows Server 2016 attenua ulteriormente questo problema ignorando FILE_FLAG_RANDOM_ACCESS quando si prendono decisioni di taglio, quindi viene considerato come qualsiasi altro file aperto senza il flag di FILE_FLAG_RANDOM_ACCESS (gestione cache rispetta comunque questo flag per disabilitare la prelettura dei dati del file). È comunque possibile causare un sovraccarico della cache di sistema se si dispone di un numero elevato di file aperti con questo flag ed è possibile accedervi in modo davvero casuale. Si consiglia vivamente di non FILE_FLAG_RANDOM_ACCESS essere utilizzato dalle applicazioni.
