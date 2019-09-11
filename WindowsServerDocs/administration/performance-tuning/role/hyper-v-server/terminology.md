---
title: Terminologia di Hyper-V
description: Terminologia di Hyper-v utile nell'ottimizzazione delle prestazioni di Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: d18557a205f8366631becb65b7460c07757db3d5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866533"
---
# <a name="hyper-v-terminology"></a>Terminologia di Hyper-V
In questa sezione viene riepilogata la terminologia chiave specifica per la tecnologia delle macchine virtuali utilizzata in questo argomento di ottimizzazione delle prestazioni:

| Nome        | Definizione           |
| ------------- |:------------|
|*partizione figlio* | Qualsiasi macchina virtuale creata dalla partizione radice.|
|*virtualizzazione dei dispositivi* | Meccanismo che consente di astrarre e condividere una risorsa hardware tra più consumer.|
|*dispositivo emulato*|Un dispositivo virtualizzato che simula un dispositivo hardware fisico reale, in modo che gli utenti guest possano usare i driver tipici del dispositivo hardware.|
|*enlightenment*|Ottimizzazione per un sistema operativo guest per renderla consapevole degli ambienti di macchine virtuali e ottimizzare il comportamento per le macchine virtuali.|
|*Guest*|Software in esecuzione in una partizione. Può trattarsi di un sistema operativo completo o di un piccolo kernel per scopi specifici. L'hypervisor è indipendente dal Guest.|
|*hypervisor*|Un livello di software che si trova sopra l'hardware e al di sotto di uno o più sistemi operativi. Il processo principale consiste nel fornire ambienti di esecuzione isolati denominati partizioni. Ogni partizione dispone di un proprio set di risorse hardware virtualizzate (unità di elaborazione centrale, CPU, memoria e dispositivi). L'hypervisor controlla e regola l'accesso all'hardware sottostante.|
|*processore logico*| Unità di elaborazione che gestisce un thread di esecuzione (flusso di istruzioni). Possono essere presenti uno o più processori logici per core del processore e uno o più core per ogni socket del processore.|
| *accesso al disco pass-through*|Rappresentazione di un intero disco fisico come disco virtuale all'interno del Guest. I dati e i comandi vengono passati al disco fisico (tramite lo stack di archiviazione nativo della partizione radice) senza elaborazione corrispondente da parte dello stack virtuale.|
|*partizione radice*|La partizione radice creata per prima e possiede tutte le risorse che l'hypervisor non esegue, inclusa la maggior parte dei dispositivi e della memoria di sistema. La partizione radice ospita lo stack di virtualizzazione e crea e gestisce le partizioni figlio.|
|*Dispositivo specifico di Hyper-V*|Un dispositivo virtualizzato senza analogico hardware fisico, in modo che i guest possano richiedere un driver (client del servizio di virtualizzazione) a tale dispositivo specifico di Hyper-V. Il driver può usare il bus di macchina virtuale (VMBus) per comunicare con il software del dispositivo virtualizzato nella partizione radice.|
|*macchina virtuale*|Un computer virtuale creato dall'emulazione software e con le stesse caratteristiche di un computer reale.|
| *switch di rete virtuale*|(noto anche come Commuter virtuale) Versione virtuale di uno switch di rete fisica. Una rete virtuale può essere configurata per fornire accesso alle risorse di rete locali o esterne per una o più macchine virtuali.|
|*processore virtuale*|Astrazione virtuale di un processore pianificato per l'esecuzione in un processore logico. Una macchina virtuale può avere uno o più processori virtuali.|
|*client del servizio di virtualizzazione (VSC)*|Un modulo software caricato da un Guest per l'utilizzo di una risorsa o di un servizio. Per i dispositivi di I/O, il client del servizio di virtualizzazione può essere un driver di dispositivo caricato dal kernel del sistema operativo.|
| *provider di servizi di virtualizzazione (VSP)*|  Provider esposto dallo stack di virtualizzazione nella partizione radice che fornisce risorse o servizi quali l'I/O a una partizione figlio.|
| *stack di virtualizzazione*|Raccolta di componenti software nella partizione radice che interagiscono per supportare le macchine virtuali. Lo stack di virtualizzazione funziona con e si trova sopra l'hypervisor. Fornisce anche funzionalità di gestione.|
|*VMBus*|Meccanismo di comunicazione basato su canale usato per la comunicazione tra partizioni e l'enumerazione dei dispositivi nei sistemi con più partizioni virtualizzate attive. Il VMBus viene installato con i servizi di integrazione Hyper-V.|

## <a name="see-also"></a>Vedere anche

-   [Architettura Hyper-V](architecture.md)

-   [Configurazione dei server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
