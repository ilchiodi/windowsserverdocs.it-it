---
title: Prestazioni I/O rete Hyper-V
description: Considerazioni sulle prestazioni di i/o di rete nell'ottimizzazione delle prestazioni di Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 17551da6cd270f05cf2d6b1a8147958f82b2c9b3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851804"
---
# <a name="hyper-v-network-io-performance"></a>Prestazioni I/O rete Hyper-V

Il server 2016 contiene diversi miglioramenti e nuove funzionalità per ottimizzare le prestazioni di rete in Hyper-V.  La documentazione su come ottimizzare le prestazioni di rete verrà inclusa in una versione futura di questo articolo.

## <a name="live-migration"></a>Migrazione in tempo reale

Live Migration consente di spostare in modo trasparente le macchine virtuali in esecuzione da un nodo di un cluster di failover a un altro nodo nello stesso cluster senza una connessione di rete eliminata o un tempo di inattività percepito.

> [!NOTE]
> Il clustering di failover richiede l'archiviazione condivisa per i nodi del cluster.

Il processo di trasferimento di una macchina virtuale in esecuzione può essere suddiviso in due fasi principali. La prima fase copia la memoria della macchina virtuale dall'host corrente al nuovo host. La seconda fase trasferisce lo stato della macchina virtuale dall'host corrente al nuovo host. La durata di entrambe le fasi è molto determinata dalla velocità con cui i dati possono essere trasferiti dall'host corrente al nuovo host.

La fornitura di una rete dedicata per il traffico Live Migration aiuta a ridurre al minimo il tempo necessario per completare una migrazione in tempo reale e garantisce tempi di migrazione coerenti.

![esempio di configurazione della migrazione in tempo reale di Hyper-v](../../media/perftune-guide-live-migration.png)

Inoltre, aumentando il numero di buffer di invio e di ricezione in ogni scheda di rete interessata nella migrazione, è possibile migliorare le prestazioni di migrazione.

In Windows Server 2012 R2 è stata introdotta un'opzione per velocizzare Live Migration comprimendo la memoria prima del trasferimento in rete o utilizzando l'accesso diretto a memoria remota (RDMA), se l'hardware lo supporta.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura Hyper-V](architecture.md)

-   [Configurazione dei server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
