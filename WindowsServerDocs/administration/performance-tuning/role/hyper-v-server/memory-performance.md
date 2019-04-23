---
title: Prestazioni della memoria di Hyper-V
description: Considerazioni sulla memoria in Hyper-V l'ottimizzazione delle prestazioni
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 63a1b654b8ac52725cc5dd87c8b245f9dfaf40f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848072"
---
# <a name="hyper-v-memory-performance"></a>Prestazioni della memoria di Hyper-V


L'hypervisor virtualizza la memoria fisica guest per isolare le macchine virtuali tra loro e per fornire uno spazio di memoria contigue, in base zero per ogni sistema operativo guest, come in sistemi non virtualizzati.

## <a name="correct-memory-sizing-for-child-partitions"></a>Correggere il dimensionamento della memoria per le partizioni figlio

È necessario adattare le dimensioni di memoria della macchina virtuale come avviene in genere per le applicazioni server in un computer fisico. È necessario ridimensionare in modo che in grado di gestire il carico previsto in ordinario e ore di picco perché memoria insufficiente può aumentare notevolmente i tempi di risposta e l'utilizzo della CPU o i/o.

È possibile abilitare la memoria dinamica consentire di Windows impostare le dimensioni di memoria della macchina virtuale in modo dinamico. Se le applicazioni nella macchina virtuale si verifichino problemi effettua allocazioni di grandi quantità di memoria improvvisi, con la memoria dinamica, è possibile aumentare le dimensioni del file di paging per la macchina virtuale garantire backup temporaneo mentre la memoria dinamica risponde alle richieste di memoria.

Per altre informazioni sulla memoria dinamica, vedere [Hyper-V Dynamic Memory Overview]( https://go.microsoft.com/fwlink/?linkid=834434) e [Guida configurazione memoria dinamica di Hyper-V](https://go.microsoft.com/fwlink/?linkid=834435).

Durante l'esecuzione di Windows nella partizione figlio, è possibile utilizzare i seguenti contatori delle prestazioni all'interno di una partizione figlio per determinare se la partizione figlio ha riscontrato un utilizzo elevato della memoria ed è probabile che offrono prestazioni migliori con una dimensione maggiore di memoria macchina virtuale.

| Contatore delle prestazioni                                                         | Valore di soglia suggerito                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memoria: byte di riserva Cache di Standby                                        | Somma dei byte di riserva Cache di Standby e gratuito e byte di pagine deve essere pari a 200 MB o altre informazioni su sistemi con 1 GB e 300 MB o più nei sistemi con almeno 2 GB di RAM visibile. |
| Memoria-gratuita & Zero byte di pagine                                        | Somma dei byte di riserva Cache di Standby e gratuito e byte di pagine deve essere pari a 200 MB o altre informazioni su sistemi con 1 GB e 300 MB o più nei sistemi con almeno 2 GB di RAM visibile. |
| Memoria: Input pagine/Sec                                                    | In un periodo di 1 ora la media è inferiore a 10.                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>Correggere il dimensionamento della memoria per la partizione radice

La partizione radice deve disporre di memoria sufficiente per fornire servizi, ad esempio i/o virtualization, snapshot della macchina virtuale e la gestione a supporto delle partizioni figlio.

Hyper-V in Windows Server 2016 consente di monitorare lo stato di runtime del sistema operativo di gestione della partizione radice per determinare la quantità di memoria può essere allocata in modo sicuro alle partizioni figlio, continuando a garantire prestazioni elevate e l'affidabilità della partizione radice.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura di Hyper-V](architecture.md)

-   [Configurazione del server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Archiviazione di Hyper-V delle prestazioni dei / o](storage-io-performance.md)

-   [Rete Hyper-V delle prestazioni dei / o](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
