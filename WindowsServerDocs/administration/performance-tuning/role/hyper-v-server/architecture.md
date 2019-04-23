---
title: Architettura Hyper-V
description: Architettura di Hyper-v condsiderations per ottimizzare le prestazioni
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fcc87b04698a44e115c8f49150fe33443f8e6a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890282"
---
# <a name="hyper-v-architecture"></a>Architettura Hyper-V

Hyper-V offre un'architettura basata su hypervisor di tipo 1. L'hypervisor virtualizza processori e memoria e fornisce meccanismi per lo stack di virtualizzazione nella partizione radice di gestione delle partizioni figlio (macchine virtuali) e di esporre servizi, ad esempio i dispositivi dei / o alle macchine virtuali.

La partizione radice è proprietaria e ha accesso diretto ai dispositivi dei / o fisici. Lo stack di virtualizzazione nella partizione radice fornisce un gestore della memoria per le macchine virtuali, le API di gestione e dispositivi dei / o virtualizzati. Implementa inoltre dispositivi emulati, ad esempio il controller del disco integrated device electronics (IDE) e porta di input device PS/2 che supporta i dispositivi sintetici specifici di Hyper-V per migliorare le prestazioni e sovraccarico ridotto.

![architettura basata su hypervisor Hyper-v](../../media/perftune-guide-hyperv-arch.png)

L'architettura dei / o specifici di Hyper-V è costituito da fornitori di servizi di virtualizzazione (vsp) nella radice partizione e la virtualizzazione del servizio client (VSC) nella partizione figlio. Ogni servizio viene esposto come un dispositivo su VMBus, che agisce come un bus i/o e consente la comunicazione ad alte prestazioni tra le macchine virtuali che usano meccanismi, ad esempio la memoria condivisa. Gestione di Plug and Play del sistema operativo guest enumera questi dispositivi, tra cui VMBus e carica il driver di dispositivo appropriati (i client del servizio virtuale). Servizi diversi dai / o sono inoltre esposte tramite questa architettura.

A partire da Windows Server 2008, gli enlightenment funzionalità il sistema operativo per ottimizzare il comportamento quando è in esecuzione nelle macchine virtuali. I vantaggi includono riducendo i costi della virtualizzazione di memoria, miglioramento della scalabilità multicore e riducendo l'utilizzo della CPU del sistema operativo guest in background.

Nelle sezioni seguenti vengono suggeriscono le procedure ottimali che producono un miglioramento delle prestazioni nei server che eseguono il ruolo Hyper-V.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Configurazione del server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Archiviazione di Hyper-V delle prestazioni dei / o](storage-io-performance.md)

-   [Rete Hyper-V delle prestazioni dei / o](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
