---
title: Architettura Hyper-V
description: Architettura di Hyper-v condsiderations per l'ottimizzazione delle prestazioni
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 47ff4a25f67e2b03655d17ab5a57aeaa3274a835
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851834"
---
# <a name="hyper-v-architecture"></a>Architettura Hyper-V

Hyper-V offre un'architettura basata su hypervisor di tipo 1. L'hypervisor virtualizza i processori e la memoria e fornisce i meccanismi per lo stack di virtualizzazione nella partizione radice per gestire le partizioni figlio (macchine virtuali) ed esporre servizi quali i dispositivi di I/O alle macchine virtuali.

La partizione radice possiede e ha accesso diretto ai dispositivi I/O fisici. Lo stack di virtualizzazione nella partizione radice fornisce un gestore della memoria per le macchine virtuali, le API di gestione e I dispositivi di I/O virtualizzati. Implementa inoltre dispositivi emulati come il controller del disco IDE (Integrated Device Electronics) e la porta del dispositivo di input PS/2 e supporta i dispositivi sintetici specifici di Hyper-V per migliorare le prestazioni e ridurre l'overhead.

![architettura basata su hypervisor Hyper-v](../../media/perftune-guide-hyperv-arch.png)

L'architettura di I/O specifica di Hyper-V è costituita da provider di servizi di virtualizzazione (VSPs) nella partizione radice e nei client del servizio di virtualizzazione (VSC) nella partizione figlio. Ogni servizio viene esposto come un dispositivo su VMBus, che funge da bus di I/O e consente la comunicazione ad alte prestazioni tra macchine virtuali che usano meccanismi come la memoria condivisa. Il gestore Plug and Play del sistema operativo guest enumera questi dispositivi, incluso VMBus, e carica i driver di dispositivo appropriati (client del servizio virtuale). I servizi diversi dall'I/O vengono esposti anche tramite questa architettura.

A partire da Windows Server 2008, il sistema operativo offre funzionalità di illuminazione per ottimizzare il comportamento quando viene eseguito in macchine virtuali. I vantaggi includono la riduzione del costo della virtualizzazione della memoria, il miglioramento della scalabilità multicore e la riduzione dell'utilizzo della CPU in background del sistema operativo guest.

Nelle sezioni seguenti vengono descritte le procedure consigliate che garantiscono un miglioramento delle prestazioni nei server che eseguono il ruolo Hyper-V.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Configurazione dei server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
