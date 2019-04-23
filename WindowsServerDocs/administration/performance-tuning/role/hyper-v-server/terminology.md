---
title: Terminologia di Hyper-V
description: Terminologia di Hyper-v utile per ottimizzare le prestazioni di Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bc970633ff24827207eb3a27e282656f2486a6eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841142"
---
# <a name="hyper-v-terminology"></a>Terminologia di Hyper-V
Questa sezione vengono riepilogate la terminologia chiave specifica per tecnologia di macchina virtuale utilizzato nell'ambito di questo argomento l'ottimizzazione delle prestazioni:

| Nome        | Definizione           |
| ------------- |:------------|
|*partizione figlio* | Qualsiasi macchina virtuale creata dalla partizione radice.|
|*virtualizzazione di dispositivo* | Un meccanismo che consente a un hardware risorsa essere astratto e condivisi tra più consumer.|
|*dispositivo emulato*|Un dispositivo virtualizzato che simula un dispositivo hardware fisico effettivo in modo che gli utenti guest possono utilizzare i driver tipici per il dispositivo hardware.|
|*enlightenment*|Un'ottimizzazione di un sistema operativo guest per renderlo compatibile con degli ambienti di macchina virtuale e ottimizzare il comportamento per le macchine virtuali.|
|*guest*|Software è in esecuzione in una partizione. Può trattarsi di un sistema operativo completo o un kernel di piccole dimensioni, con finalità speciali. L'hypervisor è indipendente dal guest.|
|*hypervisor*|Un livello di software che si trova sopra l'hardware e di sotto di uno o più sistemi operativi. Il processo principale consiste nel fornire ambienti di esecuzione isolati denominati partizioni. Ogni partizione ha un proprio set di risorse hardware virtualizzato (CPU o CPU, memoria e i dispositivi). L'hypervisor controlla e regola l'accesso all'hardware sottostante.|
|*processore logico*| Un'unità di elaborazione che gestisce un thread di esecuzione (flusso di istruzioni). Possono esistere uno o più processori logici per ogni core del processore e uno o più core per socket del processore.|
| *accesso al disco pass-through*|Una rappresentazione di un intero disco fisico come un disco virtuale all'interno del guest. I dati e i comandi vengono passati al disco fisico (tramite dello stack per l'archiviazione nativa della partizione radice) con alcuna elaborazione interessati dallo stack virtuale.|
|*partizione radice*|La partizione radice in cui viene prima creata e sia proprietario di tutte le risorse che l'hypervisor non le utilizza, tra cui la maggior parte dei dispositivi e la memoria di sistema. La partizione radice ospita lo stack di virtualizzazione e crea e gestisce le partizioni figlio.|
|*Dispositivo specifici di Hyper-V*|Un dispositivo virtualizzato con nessun analogico hardware fisico, gli utenti guest in tal caso potrebbe essere necessario un driver (client di servizio di virtualizzazione) per il dispositivo specifici di Hyper-V. Il driver è possibile usare il bus macchina virtuale (VMBus) per comunicare con il software del dispositivo virtualizzato nella partizione radice.|
|*macchina virtuale*|Un computer virtuale che è stato creato da emulazione software e ha le stesse caratteristiche di un computer reale.|
| *commutatore di rete virtuale*|(detto anche un commutatore virtuale) Una versione virtuale di uno switch di rete fisica. Una rete virtuale può essere configurata per fornire accesso alle risorse di rete locali o esterne per una o più macchine virtuali.|
|*processore virtuale*|Un'astrazione virtuale di un processore che viene pianificata l'esecuzione in un processore logico. Una macchina virtuale può avere uno o più processori virtuali.|
|*client del servizio di virtualizzazione (VSC)*|Un modulo software che un utente guest viene caricato per l'utilizzo di una risorsa o un servizio. Per i dispositivi dei / o, il client del servizio di virtualizzazione può essere un driver di dispositivo che viene caricato il kernel del sistema operativo.|
| *provider di servizi di virtualizzazione (VSP)*|  Un provider esposto dallo stack di virtualizzazione nella partizione radice, che fornisce le risorse o servizi, ad esempio i/o a una partizione figlio.|
| *stack di virtualizzazione*|Raccolta di componenti software nella partizione radice che funzionano insieme per supportare macchine virtuali. Lo stack di virtualizzazione funziona con e si trova sopra l'hypervisor. Fornisce inoltre funzionalità di gestione.|
|*VMBus*|Meccanismo di comunicazione basata sul canale utilizzato per l'enumerazione di comunicazione e dispositivi tra partizioni nei sistemi con più partizioni virtualizzate attive. Il VMBus viene installato con i servizi di integrazione Hyper-V.|

## <a name="see-also"></a>Vedere anche

-   [Architettura di Hyper-V](architecture.md)

-   [Configurazione del server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Archiviazione di Hyper-V delle prestazioni dei / o](storage-io-performance.md)

-   [Rete Hyper-V delle prestazioni dei / o](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
