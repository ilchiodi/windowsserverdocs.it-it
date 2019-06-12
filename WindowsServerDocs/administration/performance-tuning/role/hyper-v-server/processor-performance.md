---
title: Prestazioni del processore di Hyper-V
description: Considerazioni sulle prestazioni del processore nell'ottimizzazione delle prestazioni di Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f16ee9cff9c244a8c579e008bced1e90b1a20673
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435597"
---
# <a name="hyper-v-processor-performance"></a>Prestazioni del processore di Hyper-V


## <a name="virtual-machine-integration-services"></a>Servizi di integrazione di macchine virtuali

I servizi di integrazione di macchina virtuale includono abilitate per i driver per i dispositivi dei / o specifici di Hyper-V, che riduce notevolmente il sovraccarico della CPU per i/o rispetto a dispositivi emulati. È consigliabile installare la versione più recente dei servizi di integrazione di macchina virtuale in ogni macchina virtuale supportate. La diminuzione di servizi l'utilizzo della CPU Guest, dopo l'inattività i guest a ampiamente usato gli utenti Guest e migliora la velocità effettiva dei / o. Questo è il primo passaggio nell'ottimizzazione delle prestazioni in un server che esegue Hyper-V. Per un elenco di sistemi operativi guest supportati, vedere [Panoramica di Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="virtual-processors"></a>Processori virtuali

Hyper-V in Windows Server 2016 supporta un massimo di 240 processori virtuali per ogni macchina virtuale. Le macchine virtuali con carichi che non sono a elevato utilizzo della CPU deve essere configurate per utilizzare un processore virtuale. Ciò è dovuto l'overhead aggiuntivo che è associato a più processori virtuali, ad esempio i costi di sincronizzazione aggiuntiva nel sistema operativo guest.

Se la macchina virtuale richiede più di una CPU di elaborazione in condizioni di carico di picco, aumentare il numero di processori virtuali.

## <a name="background-activity"></a>Attività in background

Riducendo al minimo le attività in background nelle macchine virtuali inattive rilascia cicli di CPU che possono essere utilizzati in un punto da altre macchine virtuali. Gli utenti guest di Windows in genere utilizzato meno dell'1% di una singola CPU quando sono inattivi. Di seguito sono diverse procedure consigliate per ridurre al minimo l'utilizzo della CPU di una macchina virtuale in background:

-   Installare la versione più recente dei servizi di integrazione di macchina virtuale.

-   Rimuovere la scheda di rete emulata tramite la finestra di dialogo Impostazioni macchina virtuale (adapter di utilizzare il Microsoft specifico Hyper-V).

-   Rimuovere i dispositivi inutilizzati, ad esempio la porta CD-ROM e COM o scollegare i relativi supporti.

-   Mantenere il sistema operativo guest di Windows nella schermata di accesso quando non è in uso e disattivare lo screen saver.

-   Esaminare le attività pianificate e i servizi abilitati per impostazione predefinita.

-   Esaminare i provider di traccia ETW sono attivati per impostazione predefinita eseguendo **logman.exe query - ets**

-   Migliorare le applicazioni server per ridurre l'attività periodica (ad esempio, i timer).

-   Chiudere Server Manager nei sistemi operativi host e guest.

-   Non lasciare in esecuzione Hyper-V Manager, perché vengono costantemente aggiornati sull'anteprima della macchina virtuale.

Di seguito sono procedure consigliate aggiuntive per la configurazione di un *versione client* di Windows in una macchina virtuale per ridurre l'utilizzo complessivo della CPU:

-   Disabilitare i servizi in background, ad esempio SuperFetch e di Windows Search.

-   Disabilitare le attività pianificate, ad esempio la deframmentazione pianificato.

## <a name="virtual-numa"></a>NUMA virtuale

Per abilitare la virtualizzazione delle grandi carichi di lavoro di scalabilità verticale, Hyper-V in Windows Server 2016 espanso i limiti di scalabilità di macchine virtuali. Una singola macchina virtuale possono essere assegnata fino a 240 processori virtuali e 12 TB di memoria. Durante la creazione di tali macchine virtuali di grandi dimensioni, verrà utilizzata probabile che la memoria di più nodi NUMA nel sistema host. In questo tipo configurazione della macchina virtuale, se la memoria e processori virtuali non vengono allocati dallo stesso nodo NUMA, i carichi di lavoro possono avere peggioramento delle prestazioni a causa dell'impossibilità di trarre vantaggio dalle ottimizzazioni di NUMA.

In Windows Server 2016, Hyper-V offre una topologia NUMA virtuale per le macchine virtuali. Per impostazione predefinita, questa topologia è ottimizzata in modo da corrispondere alla topologia NUMA del computer host sottostante. L'esposizione di una topologia NUMA virtuale in una macchina virtuale consente al sistema operativo guest e alle eventuali applicazioni compatibili con NUMA eseguite in tale sistema operativo di sfruttare i vantaggi offerti dalle ottimizzazioni delle prestazioni NUMA, proprio come se fossero eseguiti in un computer fisico.

Non c'è alcuna differenza fra una topologia NUMA virtuale e una fisica dal punto di vista del carico di lavoro. In una macchina virtuale, quando un carico di lavoro alloca memoria locale per i dati e accede a tali dati nello stesso nodo NUMA, nel sistema fisico sottostante l'accesso alla memoria locale avviene rapidamente. In questo modo si evita un calo delle prestazioni dovuto all'accesso alla memoria remota. Solo le applicazioni compatibili con NUMA possono trarre vantaggio della vNUMA.

Microsoft SQL Server è un esempio di applicazione compatibile con NUMA. Per altre informazioni, vedi [Understanding Non-uniform Memory Access](https://technet.microsoft.com/library/ms178144.aspx).

NUMA virtuale e la memoria dinamica sono due caratteristiche che non possono essere usate insieme. Una macchina virtuale con la memoria dinamica abilitata ha in effetti un solo nodo NUMA virtuale e nessuna topologia NUMA viene presentata alla macchina virtuale, indipendentemente dalle impostazioni di NUMA virtuale.

Per altre informazioni su NUMA virtuale, vedere [Panoramica su NUMA virtuale Hyper-V](https://technet.microsoft.com/library/dn282282.aspx).

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura Hyper-V](architecture.md)

-   [Configurazione dei server Hyper-V](configuration.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
