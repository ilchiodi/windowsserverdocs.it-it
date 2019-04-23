---
title: Rilevamento dei colli di bottiglia in un ambiente virtualizzato
description: Come rilevare e risolvere i colli di bottiglia delle prestazioni di potenziali Hyper-v
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cdad5f0cc3b0e49ae46e975e3acc2c48a18e5f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867532"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>Rilevamento dei colli di bottiglia in un ambiente virtualizzato

In questa sezione fornisce alcuni suggerimenti su cosa monitorare tramite Performance Monitor e come identificare dove il problema potrebbe essere quando l'host o alcune delle macchine virtuali non si eseguono come potrebbe avere previsto.

## <a name="processor-bottlenecks"></a>Colli di bottiglia del processore

Ecco alcuni scenari comuni che potrebbero causare colli di bottiglia del processore:

-   Uno o più processori logici vengono caricati

-   Vengono caricati uno o più processori virtuali

È possibile usare i seguenti contatori delle prestazioni dall'host:

-   Utilizzo del processore logico - \\processore logico Hypervisor Hyper-V (\*)\\% totale tempo di esecuzione

-   Utilizzo del processore virtuale - \\processore virtuale Hypervisor Hyper-V (\*)\\% totale tempo di esecuzione

-   Utilizzo del processore virtuale - radice \\processore virtuale radice di Hypervisor Hyper-V (\*)\\% totale tempo di esecuzione

Se il **processore logico Hypervisor Hyper-V (\_totale)\\% Runtime totale** contatore è superiore al 90%, l'host è in overload. È necessario aggiungere maggiore potenza di elaborazione o spostare alcune macchine virtuali in un host diverso.

Se il **Processor(VM Name:VP x) virtuale Hypervisor Hyper-V\\% Runtime totale** contatore è superiore al 90% per tutti i processori virtuali, è necessario eseguire le operazioni seguenti:

-   Verificare che l'host non sia sovraccarica.

-   Scoprire se il carico di lavoro può sfruttare più processori virtuali

-   Assegnare più processori virtuali per la macchina virtuale

Se **Processor(VM Name:VP x) virtuale Hypervisor Hyper-V\\% Runtime totale** contatore è superiore al 90% per alcuni, ma non tutti i processori virtuali, è necessario eseguire le operazioni seguenti:

-   Se il carico di lavoro viene visualizzato a elevato utilizzo di rete, è consigliabile usare vRSS.

-   Se le macchine virtuali non sono in esecuzione Windows Server 2012 R2, è necessario aggiungere altre schede di rete.

-   Se il carico di lavoro a elevato utilizzo di archiviazione, è necessario abilitare NUMA virtuale e aggiungere ulteriori dischi virtuali.

Se il **processore virtuale radice di Hypervisor Hyper-V (VP radice x)\\% Runtime totale** contatore è oltre il 90% per alcuni, ma non tutti i processori virtuali e il **processore (x)\\% tempo di Interrupt e Processore (x)\\% tempo DPC** contatore si traduce approssimativamente il valore per il **radice virtuale Processor(Root VP x)\\% Runtime totale** del contatore, è necessario assicurarsi abilitare VMQ nel gruppo le schede di rete.

## <a name="memory-bottlenecks"></a>Colli di bottiglia della memoria

Ecco alcuni scenari comuni che potrebbero causare colli di bottiglia della memoria:

-   L'host non risponde.

-   Impossibile avviare le macchine virtuali.

-   Memoria insufficiente per le macchine virtuali.

È possibile usare i seguenti contatori delle prestazioni dall'host:

-   Memoria\\MByte disponibili

-   Servizio di bilanciamento memoria dinamica di Hyper-V (\*)\\memoria disponibile

È possibile usare i seguenti contatori delle prestazioni dalla macchina virtuale:

-   Memoria\\MByte disponibili

Se il **memoria\\MByte disponibili** e **bilanciamento memoria dinamica di Hyper-V (\*)\\memoria disponibile** contatori sono insufficienti nell'host, è consigliabile arrestare non essenziali di servizi ed eseguire la migrazione di uno o più macchine virtuali in un altro host.

Se il **memoria\\MByte disponibili** contatore è insufficiente nella macchina virtuale, è necessario assegnare una maggiore quantità di memoria alla macchina virtuale. Se si utilizza la memoria dinamica, è necessario aumentare l'impostazione massima di memoria.

## <a name="network-bottlenecks"></a>Colli di bottiglia di rete

Ecco alcuni scenari comuni che potrebbero causare colli di bottiglia di rete:

-   L'host è di rete associato.

-   La macchina virtuale è rete associata.

È possibile usare i seguenti contatori delle prestazioni dall'host:

-   Interfaccia di rete (*nome scheda di rete*)\\byte/sec

È possibile usare i seguenti contatori delle prestazioni dalla macchina virtuale:

-   Scheda di rete virtuale di Hyper-V (*nome macchina virtuale&lt;GUID&gt;*)\\byte/sec

Se il **byte/sec scheda NIC fisica** contatore è maggiore o uguale a 90% della capacità, è necessario aggiungere altre schede di rete, eseguire la migrazione di macchine virtuali in un altro host e configurare le impostazioni QoS di rete.

Se il **byte/sec scheda di rete virtuale di Hyper-V** contatore è maggiore o uguale a 250 MBps, è necessario aggiungere schede di rete raggruppata aggiuntive nella macchina virtuale, abilitare vRSS e usare SR-IOV.

Se i carichi di lavoro non soddisfa la latenza di rete, abilitare SR-IOV presentare le risorse della scheda di rete fisica alla macchina virtuale.

## <a name="storage-bottlenecks"></a>Colli di bottiglia di archiviazione

Ecco alcuni scenari comuni che potrebbero causare colli di bottiglia di archiviazione:

-   L'host e macchina virtuale di operazioni sono lente o raggiungere il timeout.

-   La macchina virtuale è lenta.

È possibile usare i seguenti contatori delle prestazioni dall'host:

-   Disco fisico (*lettera disco*)\\Media letture disco/sec

-   Disco fisico (*lettera disco*)\\Media scritture disco/sec

-   Disco fisico (*lettera disco*)\\Media trasf. disco Media lettura coda

-   Disco fisico (*lettera disco*)\\media coda scrittura Media trasf. disco

Se le latenze sono costantemente superiore a 50 ms, è necessario eseguire le operazioni seguenti:

-   Distribuire le macchine virtuali tra ulteriore spazio di archiviazione

-   Prendere in considerazione l'acquisto di archiviazione più veloci

-   Prendere in considerazione livelli spazi di archiviazione, che è stata introdotta in Windows Server 2012 R2

-   È consigliabile usare QoS di archiviazione, che è stata introdotta in Windows Server 2012 R2

-   Utilizzare VHDX

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura di Hyper-V](architecture.md)

-   [Configurazione del server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Archiviazione di Hyper-V delle prestazioni dei / o](storage-io-performance.md)

-   [Rete Hyper-V delle prestazioni dei / o](network-io-performance.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
