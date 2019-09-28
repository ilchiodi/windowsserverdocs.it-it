---
title: Configurazione di Hyper-V
description: Considerazioni sulla configurazione di Hyper-V per l'ottimizzazione delle prestazioni
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f21115265ca7d2788fc0be078860048602d82c0f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370165"
---
# <a name="hyper-v-configuration"></a>Configurazione di Hyper-V

## <a name="hardware-selection"></a>Selezione hardware

Le considerazioni relative all'hardware per i server che eseguono Hyper-V sono in genere simili a quelle dei server non virtualizzati, ma i server che eseguono Hyper-V possono presentare un maggiore utilizzo della CPU, utilizzare una quantità maggiore di memoria e una maggiore larghezza di banda di I/O a causa del consolidamento dei server.

-   **Processori**

    Hyper-V in Windows Server 2016 presenta i processori logici come uno o più processori virtuali per ogni macchina virtuale attiva. Hyper-V richiede ora processori che supportano tecnologie di secondo livello, ad esempio l'EPT (Extended Page Table) o le tabelle di pagine nidificate (NPT).

-   **Cache**

    Hyper-V può trarre vantaggio dalle cache del processore più grandi, specialmente per i caricamenti con una grande working set in memoria e in configurazioni di macchine virtuali in cui il rapporto tra processori virtuali e processori logici è elevato.

-   **Memoria**

    Il server fisico richiede memoria sufficiente per le partizioni radice e figlio. La partizione radice richiede memoria per eseguire in modo efficiente le operazioni di I/o per conto delle macchine virtuali e delle operazioni, ad esempio uno snapshot della macchina virtuale. Hyper-V assicura che la memoria disponibile sia sufficiente per la partizione radice e consente di assegnare memoria rimanente alle partizioni figlio. Le partizioni figlio devono essere dimensionate in base alle esigenze del carico previsto per ogni macchina virtuale.

-   **Archiviazione**

    L'hardware di archiviazione deve avere una larghezza di banda di I/O sufficiente per soddisfare le esigenze attuali e future delle macchine virtuali ospitate dal server fisico. Considerare questi requisiti quando si selezionano i controller e i dischi di archiviazione e si sceglie la configurazione RAID. L'inserimento di macchine virtuali con carichi di lavoro a elevato utilizzo di disco in dischi fisici diversi comporta probabilmente un miglioramento delle prestazioni complessive. Se, ad esempio, quattro macchine virtuali condividono un singolo disco e lo usano attivamente, ogni macchina virtuale può produrre solo il 25% della larghezza di banda del disco.

## <a name="power-plan-considerations"></a>Considerazioni sulla combinazione per il risparmio di energia

Come tecnologia di base, la virtualizzazione è uno strumento potente che consente di aumentare la densità del carico di lavoro del server, riducendo il numero di server fisici necessari nel Data Center, aumentando l'efficienza operativa e riducendo i costi di consumo energetico. Il risparmio energia è essenziale per la gestione dei costi. 

In un ambiente di Data Center ideale, il consumo di energia è gestito consolidando il lavoro sui computer fino a quando non sono occupati e quindi disattivando i computer inattivi. Se questo approccio non è pratico, gli amministratori possono sfruttare le combinazioni per il risparmio di energia negli host fisici per assicurarsi che non utilizzino più energia del necessario. 

Le tecniche di risparmio energia del server hanno un costo, in particolare perché i carichi di lavoro dei tenant non sono attendibili per dettare i criteri sull'infrastruttura fisica del provider di hosting. Il software del livello host viene lasciato dedurre a massimizzare la velocità effettiva riducendo al minimo il consumo di energia elettrica. Nella maggior parte dei computer inattivi, questa situazione può causare la conclusione dell'infrastruttura fisica, in modo da garantire che il risparmio di energia moderato sia appropriato, in modo che i singoli carichi di lavoro del tenant vengano eseguiti più lentamente rispetto a quelli

Windows Server utilizza la virtualizzazione in un'ampia gamma di scenari. Da un server IIS con caricamento leggero a un SQL Server moderatamente occupato, a un host cloud con Hyper-V che esegue centinaia di macchine virtuali per server. Ognuno di questi scenari può avere requisiti hardware, software e di prestazioni univoci. Per impostazione predefinita, Windows Server usa e consiglia la combinazione per il risparmio di energia **bilanciata** che consente la conservazione del risparmio energia ridimensionando le prestazioni del processore in base all'utilizzo della CPU.

Con la combinazione per il risparmio di energia **bilanciata** , gli Stati di alimentazione più elevati e le latenze di risposta più bassa nei carichi di lavoro dei tenant vengono applicati solo quando l'host fisico è relativamente occupato. Se si imposta un valore di risposta deterministica a bassa latenza per tutti i carichi di lavoro del tenant, è consigliabile passare dalla combinazione per il risparmio di energia predefinita **bilanciata** a quella a **prestazioni elevate** . La combinazione per il risparmio di energia a **prestazioni elevate** consente di eseguire tutti i processori alla massima velocità, disabilitando in modo efficace il cambio basato su richiesta insieme ad altre tecniche di risparmio energia e ottimizzando le prestazioni rispetto al risparmio energetico.

Per i clienti, che sono soddisfatti del risparmio sui costi derivanti dalla riduzione del numero di server fisici e vogliono garantire che raggiungano le massime prestazioni per i carichi di lavoro virtualizzati, è consigliabile usare la combinazione per il risparmio di energia a **prestazioni elevate** .

Per consigli aggiuntivi e informazioni dettagliate sull'uso delle combinazioni per il risparmio di energia per ottimizzare l'infrastruttura, vedere [parametri di combinazione per il risparmio di energia consigliati per tempi di risposta rapidi](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Opzione di installazione dei componenti di base del server

Windows Server 2016 dispone dell'opzione di installazione dei componenti di base del server. Server Core offre un ambiente minimo per l'hosting di un set selezionato di ruoli del server, tra cui Hyper-V. Presenta un footprint di disco inferiore per il sistema operativo host e una superficie di attacco e manutenzione più piccola. Pertanto, è consigliabile che i server di virtualizzazione Hyper-V usino l'opzione di installazione dei componenti di base del server.

Un'installazione dei componenti di base del server offre una finestra della console solo quando l'utente è connesso, ma Hyper-V espone le funzionalità di gestione remota, tra cui [Windows PowerShell](https://technet.microsoft.com/library/hh848559.aspx) , in modo che gli amministratori possano gestirla in remoto.

## <a name="dedicated-server-role"></a>Ruolo server dedicato

La partizione radice deve essere dedicata a Hyper-V. L'esecuzione di ruoli server aggiuntivi in un server che esegue Hyper-V può influire negativamente sulle prestazioni del server di virtualizzazione, soprattutto se utilizzano CPU, memoria o larghezza di banda di I/O significative. Per ridurre al minimo i ruoli del server nella partizione radice sono presenti vantaggi aggiuntivi, ad esempio la riduzione della superficie di attacco.

Gli amministratori di sistema devono considerare attentamente il software installato nella partizione radice, in quanto il software può influire negativamente sulle prestazioni complessive del server che esegue Hyper-V.

## <a name="guest-operating-systems"></a>Sistemi operativi guest

Hyper-V supporta ed è stato ottimizzato per vari sistemi operativi guest. Il numero di processori virtuali supportati per guest dipende dal sistema operativo guest. Per un elenco dei sistemi operativi guest supportati, vedere [Panoramica di Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="cpu-statistics"></a>Statistiche CPU

Hyper-V pubblica i contatori delle prestazioni per identificare il comportamento del server di virtualizzazione e segnalare l'utilizzo delle risorse. Il set standard di strumenti per la visualizzazione dei contatori delle prestazioni in Windows include performance monitor e logman. exe, che consente di visualizzare e registrare i contatori delle prestazioni di Hyper-V. I nomi degli oggetti contatore pertinenti sono preceduti da **Hyper-V**.

È sempre necessario misurare l'utilizzo della CPU del sistema fisico usando i contatori delle prestazioni del processore logico hypervisor Hyper-V. I contatori di utilizzo della CPU segnalati da Gestione attività e performance monitor nelle partizioni radice e figlio non riflettono l'effettivo utilizzo della CPU fisica. Usare i contatori delle prestazioni seguenti per monitorare le prestazioni:

- **Processore logico hypervisor Hyper-V (\*) \\% tempo di esecuzione totale** Tempo di non inattività totale dei processori logici

- **Processore logico hypervisor Hyper-V (\*) \\% tempo di esecuzione guest** Tempo impiegato per l'esecuzione di cicli in un Guest o nell'host

- **Processore logico hypervisor Hyper-V (\*) \\% tempo di esecuzione hypervisor** Tempo impiegato per l'esecuzione nell'hypervisor

- **Processore virtuale radice hypervisor Hyper-V (\*)\\\\** * misura l'utilizzo della CPU della partizione radice

- **Processore virtuale hypervisor Hyper-V (\*)\\\\** * misura l'utilizzo della CPU da parte delle partizioni Guest


## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura Hyper-V](architecture.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
