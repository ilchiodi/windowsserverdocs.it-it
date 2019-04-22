---
title: Prestazioni dei / o rete di Hyper-V
description: Considerazioni sulle prestazioni dei / o in Hyper-V ottimizzazione delle prestazioni di rete
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: d52c4fff6c7e06fb0a9f2b44ea51a0a790e6674d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814362"
---
# <a name="hyper-v-network-io-performance"></a>Prestazioni dei / o rete di Hyper-V

Server 2016 contiene numerosi miglioramenti e nuove funzionalità per ottimizzare le prestazioni di rete in Hyper-V.  Documentazione su come ottimizzare le prestazioni di rete verrà inclusi in una versione futura di questo articolo.

## <a name="live-migration"></a>Migrazione in tempo reale

Migrazione in tempo reale consente di spostare in modo trasparente macchine virtuali in esecuzione da un nodo di un cluster di failover a un altro nodo nello stesso cluster senza una connessione di rete eliminato o inattività.

**Nota**    Clustering di Failover richiede l'archiviazione condivisa per i nodi del cluster.

Il processo di spostamento di una macchina virtuale in esecuzione può essere suddivisa in due fasi principali. La prima fase copia la memoria della macchina virtuale dall'host corrente per il nuovo host. La seconda fase trasferisce lo stato della macchina virtuale dall'host corrente per il nuovo host. La durata di entrambe le fasi notevolmente è determinato dalla velocità con cui i dati possono essere trasferiti dall'host corrente per il nuovo host.

Fornisce una rete dedicata per la migrazione in tempo reale il traffico consente di ridurre al minimo il tempo necessario per completare una migrazione in tempo reale e garantisce i tempi di migrazione coerente.

![esempio di configurazione di live migration hyper-v](../../media/perftune-guide-live-migration.png)

Inoltre, l'aumento del numero di trasmissione e i buffer di ricezione in ogni rete adapter coinvolti nel processo di migrazione può migliorare le prestazioni della migrazione.

Windows Server 2012 R2 introdotta un'opzione per velocizzare la migrazione in tempo reale mediante la compressione della memoria prima di trasferire in rete oppure utilizzare Direct accesso memoria remota (RDMA), se l'hardware supporta questa funzionalità.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura di Hyper-V](architecture.md)

-   [Configurazione del server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Archiviazione di Hyper-V delle prestazioni dei / o](storage-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
