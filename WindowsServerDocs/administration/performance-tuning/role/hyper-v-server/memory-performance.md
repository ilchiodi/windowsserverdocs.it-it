---
title: Prestazioni di memoria Hyper-V
description: Considerazioni sulla memoria nell'ottimizzazione delle prestazioni di Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5f683b85657b8dd263e93380b71c646ad677950c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851724"
---
# <a name="hyper-v-memory-performance"></a>Prestazioni di memoria Hyper-V


L'hypervisor virtualizza la memoria fisica Guest per isolare le macchine virtuali tra loro e per fornire uno spazio di memoria contiguo e in base zero per ogni sistema operativo guest, come nei sistemi non virtualizzati.

## <a name="correct-memory-sizing-for-child-partitions"></a>Correggere il dimensionamento della memoria per le partizioni figlio

È consigliabile ridimensionare la memoria della macchina virtuale come in genere per le applicazioni server in un computer fisico. È necessario dimensionarlo per gestire ragionevolmente il carico previsto in orari normali e di punta, perché la memoria insufficiente può aumentare significativamente i tempi di risposta e l'utilizzo di CPU o I/O.

È possibile abilitare memoria dinamica per consentire a Windows di ridimensionare dinamicamente la memoria della macchina virtuale. Con memoria dinamica, se le applicazioni nella macchina virtuale riscontrano problemi di allocazione di memoria improvvisi di grandi dimensioni, è possibile aumentare le dimensioni del file di paging della macchina virtuale per garantire il supporto temporaneo mentre memoria dinamica risponde alla pressione della memoria.

Per ulteriori informazioni su memoria dinamica, vedere [panoramica memoria dinamica Hyper-v]( https://go.microsoft.com/fwlink/?linkid=834434) e la [Guida alla configurazione di memoria dinamica Hyper-v](https://go.microsoft.com/fwlink/?linkid=834435).

Quando si esegue Windows nella partizione figlio, è possibile utilizzare i contatori delle prestazioni seguenti all'interno di una partizione figlio per identificare se la partizione figlio sta riscontrando un numero eccessivo di richieste di memoria ed è probabile che venga eseguita in modo migliore con una dimensione di memoria della macchina virtuale superiore.

| Contatore delle prestazioni                                                         | Valore soglia suggerito                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memoria-byte riserva cache di standby                                        | La somma dei byte di riserva della cache di standby e dei byte di elenco di pagine gratuite e zero deve essere 200 MB o superiore nei sistemi con 1 GB e 300 MB o superiore nei sistemi con almeno 2 GB di RAM visibile. |
| Memoria-byte gratuiti & elenco pagine zero                                        | La somma dei byte di riserva della cache di standby e dei byte di elenco di pagine gratuite e zero deve essere 200 MB o superiore nei sistemi con 1 GB e 300 MB o superiore nei sistemi con almeno 2 GB di RAM visibile. |
| Memoria-input pagine/sec                                                    | La media in un periodo di 1 ora è minore di 10.                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>Correggere il dimensionamento della memoria per la partizione radice

La partizione radice deve disporre di memoria sufficiente per fornire servizi quali la virtualizzazione I/O, lo snapshot della macchina virtuale e la gestione per supportare le partizioni figlio.

Hyper-V in Windows Server 2016 monitora lo stato di runtime del sistema operativo di gestione della partizione radice per determinare la quantità di memoria che può essere allocata in modo sicuro alle partizioni figlio garantendo allo stesso tempo prestazioni e affidabilità elevate della partizione radice.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura Hyper-V](architecture.md)

-   [Configurazione dei server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
