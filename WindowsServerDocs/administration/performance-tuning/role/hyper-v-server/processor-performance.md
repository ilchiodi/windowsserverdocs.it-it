---
title: Prestazioni del processore Hyper-V
description: Considerazioni sulle prestazioni del processore nell'ottimizzazione delle prestazioni di Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fc1d6bdb848ea9662ba9b3d3119f286af3476688
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851754"
---
# <a name="hyper-v-processor-performance"></a>Prestazioni del processore Hyper-V


## <a name="virtual-machine-integration-services"></a>Servizi di integrazione macchina virtuale

La macchina virtuale Integration Services includere i driver illuminati per i dispositivi di I/O specifici di Hyper-V, riducendo significativamente il sovraccarico della CPU per l'I/O rispetto ai dispositivi emulati. È necessario installare la versione più recente della macchina virtuale Integration Services in ogni macchina virtuale supportata. I servizi riducono l'utilizzo della CPU da parte dei guest, dagli utenti inattivi ai guest usati molto spesso e migliorano la velocità effettiva di I/O. Questo è il primo passaggio per l'ottimizzazione delle prestazioni in un server che esegue Hyper-V. Per un elenco dei sistemi operativi guest supportati, vedere [Panoramica di Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="virtual-processors"></a>Processori virtuali

Hyper-V in Windows Server 2016 supporta un massimo di 240 processori virtuali per ogni macchina virtuale. Le macchine virtuali con carichi che non richiedono un utilizzo intensivo della CPU devono essere configurate per l'utilizzo di un solo processore virtuale. Ciò è dovuto al sovraccarico aggiuntivo associato a più processori virtuali, ad esempio costi aggiuntivi di sincronizzazione nel sistema operativo guest.

Aumentare il numero di processori virtuali se la macchina virtuale richiede più di una CPU di elaborazione sotto il picco di carico.

## <a name="background-activity"></a>Attività in background

La riduzione delle attività in background in macchine virtuali inattive rilascia cicli CPU che possono essere usati altrove da altre macchine virtuali. I guest Windows in genere utilizzano meno di una percentuale di CPU quando sono inattivi. Di seguito sono riportate alcune procedure consigliate per ridurre al minimo l'utilizzo della CPU in background di una macchina virtuale:

-   Installare la versione più recente della Integration Services della macchina virtuale.

-   Rimuovere la scheda di rete emulata tramite la finestra di dialogo Impostazioni macchina virtuale (utilizzare l'adapter specifico per Microsoft Hyper-V).

-   Rimuovere i dispositivi inutilizzati, ad esempio CD-ROM e porta COM, oppure disconnettere i relativi supporti.

-   Quando non è in uso, il sistema operativo guest di Windows deve essere mantenuto nella schermata di accesso e disabilitare il screen saver.

-   Esaminare le attività e i servizi pianificati che sono abilitati per impostazione predefinita.

-   Esaminare i provider di traccia ETW attivati per impostazione predefinita eseguendo la **query logman. exe-ETS**

-   Migliorare le applicazioni server per ridurre le attività periodiche (ad esempio i timer).

-   Chiudere Server Manager nei sistemi operativi host e Guest.

-   Non lasciare la console di gestione di Hyper-V in esecuzione perché aggiorna costantemente l'anteprima della macchina virtuale.

Di seguito sono riportate le procedure consigliate aggiuntive per la configurazione di una *versione client* di Windows in una macchina virtuale per ridurre l'utilizzo complessivo della CPU:

-   Disabilitare i servizi in background come SuperFetch e Windows Search.

-   Disabilitare le attività pianificate come la deframmentazione pianificata.

## <a name="virtual-numa"></a>NUMA virtuale

Per abilitare la virtualizzazione di carichi di lavoro con scalabilità verticale di grandi dimensioni, Hyper-V in Windows Server 2016 ha espanso i limiti di scalabilità di macchine virtuali. A una singola macchina virtuale possono essere assegnati fino a 240 processori virtuali e 12 TB di memoria. Quando si creano macchine virtuali di grandi dimensioni, è probabile che venga usata la memoria di più nodi NUMA nel sistema host. In tale configurazione della macchina virtuale, se i processori virtuali e la memoria non vengono allocati dallo stesso nodo NUMA, i carichi di lavoro potrebbero avere prestazioni non valide grazie all'impossibilità di sfruttare le ottimizzazioni NUMA.

In Windows Server 2016, Hyper-V offre una topologia NUMA virtuale per le macchine virtuali. Per impostazione predefinita, questa topologia è ottimizzata in modo da corrispondere alla topologia NUMA del computer host sottostante. L'esposizione di una topologia NUMA virtuale in una macchina virtuale consente al sistema operativo guest e alle eventuali applicazioni compatibili con NUMA eseguite in tale sistema operativo di sfruttare i vantaggi offerti dalle ottimizzazioni delle prestazioni NUMA, proprio come se fossero eseguiti in un computer fisico.

Non esiste alcuna distinzione tra una NUMA virtuale e una NUMA fisica dal punto di vista del carico di lavoro. In una macchina virtuale, quando un carico di lavoro alloca memoria locale per i dati e accede a tali dati nello stesso nodo NUMA, nel sistema fisico sottostante l'accesso alla memoria locale avviene rapidamente. In questo modo si evita un calo delle prestazioni dovuto all'accesso alla memoria remota. Solo le applicazioni compatibili con NUMA possono trarre vantaggio da vNUMA.

Microsoft SQL Server è un esempio di applicazione compatibile con NUMA. Per altre informazioni, vedere [informazioni sull'accesso non uniforme alla memoria](https://technet.microsoft.com/library/ms178144.aspx).

NUMA virtuale e la memoria dinamica sono due caratteristiche che non possono essere usate insieme. Una macchina virtuale con la memoria dinamica abilitata ha in effetti un solo nodo NUMA virtuale e nessuna topologia NUMA viene presentata alla macchina virtuale, indipendentemente dalle impostazioni di NUMA virtuale.

Per altre informazioni su NUMA virtuale, vedere [Panoramica di NUMA virtuale Hyper-V](https://technet.microsoft.com/library/dn282282.aspx).

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura Hyper-V](architecture.md)

-   [Configurazione dei server Hyper-V](configuration.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
