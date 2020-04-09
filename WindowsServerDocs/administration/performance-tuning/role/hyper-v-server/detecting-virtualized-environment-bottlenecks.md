---
title: Rilevamento di colli di bottiglia in un ambiente virtualizzato
description: Come rilevare e risolvere potenziali colli di bottiglia delle prestazioni di Hyper-v
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 211f35c151e94bc8b8a11a614edad18053cb18b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851779"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>Rilevamento di colli di bottiglia in un ambiente virtualizzato

In questa sezione vengono illustrati alcuni suggerimenti sugli elementi da monitorare tramite Performance Monitor e su come identificare il problema che può verificarsi quando l'host o alcune macchine virtuali non vengono eseguite come previsto.

## <a name="processor-bottlenecks"></a>Colli di bottiglia del processore

Di seguito sono riportati alcuni scenari comuni che possono causare colli di bottiglia del processore:

-   Sono stati caricati uno o più processori logici

-   Sono stati caricati uno o più processori virtuali

È possibile utilizzare i contatori delle prestazioni seguenti dall'host:

-   Utilizzo del processore logico-\\processore logico hypervisor Hyper-V (\*)\\% tempo di esecuzione totale

-   Utilizzo del processore virtuale-\\processore virtuale hypervisor Hyper-V (\*)\\% tempo di esecuzione totale

-   Utilizzo del processore virtuale radice-\\processore virtuale radice hypervisor Hyper-V (\*)\\% tempo di esecuzione totale

Se il **processore logico hypervisor Hyper-V (\_Total)\\% totale contatore Runtime** è superiore al 90%, l'host è sovraccarico. È necessario aggiungere ulteriore potenza di elaborazione o spostare alcune macchine virtuali in un host diverso.

Se il **processore virtuale hypervisor Hyper-V (nome VM: VP x)\\% totale contatore Runtime** è superiore al 90% per tutti i processori virtuali, è necessario eseguire le operazioni seguenti:

-   Verificare che l'host non sia sovraccarico

-   Determinare se il carico di lavoro può sfruttare più processori virtuali

-   Assegnare più processori virtuali alla macchina virtuale

Se il **processore virtuale hypervisor Hyper-V (nome VM: VP x)\\% totale contatore Runtime** è superiore al 90% per alcuni, ma non tutti, i processori virtuali, è necessario eseguire le operazioni seguenti:

-   Se il carico di lavoro riceve un elevato utilizzo di rete, è necessario considerare l'uso di vRSS.

-   Se le macchine virtuali non eseguono Windows Server 2012 R2, è necessario aggiungere altre schede di rete.

-   Se il carico di lavoro è a elevato utilizzo di risorse di archiviazione, è necessario abilitare NUMA virtuale e aggiungere altri dischi virtuali.

Se il **processore virtuale radice hypervisor Hyper-V (radice VP x)\\% totale contatore Runtime** è superiore al 90% per alcuni, tuttavia, non tutti i processori virtuali e il **processore (x)\\% tempo di interrupt e processore (x)\\% tempo DPC** aumentano approssimativamente fino al valore per il contatore del **processore virtuale radice (VP x)\\% Total Runtime** , è necessario assicurarsi di abilitare VMQ nelle schede di rete.

## <a name="memory-bottlenecks"></a>Colli di bottiglia della memoria

Di seguito sono riportati alcuni scenari comuni che possono causare colli di bottiglia della memoria:

-   L'host non risponde.

-   Impossibile avviare le macchine virtuali.

-   Memoria esaurita per le macchine virtuali.

È possibile utilizzare i contatori delle prestazioni seguenti dall'host:

-   Memoria\\MByte disponibili

-   Servizio di bilanciamento del memoria dinamica Hyper-V (\*)\\memoria disponibile

Dalla macchina virtuale è possibile usare i contatori delle prestazioni seguenti:

-   Memoria\\MByte disponibili

Se la **memoria\\MByte disponibili** e il servizio di **bilanciamento del memoria dinamica Hyper-V (\*)\\** i contatori di memoria disponibili sono insufficienti nell'host, è necessario arrestare i servizi non essenziali ed eseguire la migrazione di una o più macchine virtuali in un altro host.

Se il contatore **memoria\\MByte disponibili** è insufficiente nella macchina virtuale, è necessario assegnare ulteriore memoria alla macchina virtuale. Se si utilizza memoria dinamica, è necessario aumentare l'impostazione di memoria massima.

## <a name="network-bottlenecks"></a>Colli di bottiglia di rete

Di seguito sono riportati alcuni scenari comuni che possono causare colli di bottiglia di rete:

-   L'host è associato alla rete.

-   La macchina virtuale è associata alla rete.

È possibile utilizzare i contatori delle prestazioni seguenti dall'host:

-   Interfaccia di rete (*Nome scheda di rete*)\\byte/sec

Dalla macchina virtuale è possibile usare i contatori delle prestazioni seguenti:

-   Scheda di rete virtuale Hyper-V (nome*nome macchina virtuale&lt;GUID&gt;* )\\byte/sec

Se il contatore **byte/sec di NIC fisico** è maggiore o uguale al 90% di capacità, è necessario aggiungere schede di rete aggiuntive, eseguire la migrazione di macchine virtuali a un altro host e configurare QoS di rete.

Se il contatore **byte/sec scheda di rete virtuale Hyper-V** è maggiore o uguale a 250 Mbps, è necessario aggiungere altre schede di rete raggruppate nella macchina virtuale, abilitare vRSS e usare SR-IOV.

Se i carichi di lavoro non sono in grado di soddisfare la latenza di rete, abilitare SR-IOV per presentare le risorse della scheda di rete fisica alla macchina virtuale.

## <a name="storage-bottlenecks"></a>Colli di bottiglia di archiviazione

Di seguito sono riportati alcuni scenari comuni che possono causare colli di bottiglia di archiviazione:

-   Le operazioni dell'host e della macchina virtuale sono lente o di timeout.

-   La macchina virtuale è lenta.

È possibile utilizzare i contatori delle prestazioni seguenti dall'host:

-   Disco fisico (*lettera disco*)\\Media letture disco/sec

-   Disco fisico (*lettera disco*)\\media scritture disco/sec

-   Disco fisico (*lettera disco*)\\lunghezza media coda lettura disco

-   Disco fisico (*lettera disco*)\\lunghezza media coda di scrittura su disco

Se le latenze sono costantemente maggiori di 50 ms, è necessario eseguire le operazioni seguenti:

-   Distribuire le macchine virtuali in una risorsa di archiviazione aggiuntiva

-   Prendere in considerazione l'acquisto di archiviazione più veloce

-   Prendere in considerazione gli spazi di archiviazione a più livelli introdotti in Windows Server 2012 R2

-   Prendere in considerazione l'uso di QoS di archiviazione, introdotto in Windows Server 2012 R2

-   Usare VHDX

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura Hyper-V](architecture.md)

-   [Configurazione dei server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
