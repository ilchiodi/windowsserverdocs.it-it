---
title: Configurazione di Hyper-V
description: Considerazioni sulla configurazione di Hyper-V per ottimizzare le prestazioni
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: baea091482818c581414ba1d9c1c01db2a52e3d7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435660"
---
# <a name="hyper-v-configuration"></a>Configurazione di Hyper-V

## <a name="hardware-selection"></a>Selezione dell'hardware

Le considerazioni sull'hardware per server che eseguono Hyper-V a livello generale sono simili a quelle dei server non virtualizzato, ma i server che eseguono Hyper-V possono presentare un aumento dell'utilizzo della CPU utilizzata più memoria e necessario maggiore larghezza di banda dei / o a causa di consolidamento dei server.

-   **Processori**

    Hyper-V in Windows Server 2016 presenta i processori logici come uno o più processori virtuali a ogni macchina virtuale attiva. Hyper-V è ora necessario processori che supportano tecnologie secondo SLAT Level Address Translation (), ad esempio Extended pagina tabelle (EPT) o annidati pagina tabelle (TNP).

-   **Cache**

    Hyper-V possono trarre vantaggio dalla cache del processore più grandi, specialmente per i caricamenti con un grande working set in memoria e nelle configurazioni di macchina virtuale in cui il rapporto di processori virtuali processori logici è elevato.

-   **Memoria**

    Il server fisico richiede memoria sufficiente per le partizioni radice e figlio. La partizione radice richiede memoria per eseguire in modo efficiente i/o per conto di macchine virtuali e operazioni, ad esempio uno snapshot macchina virtuale. Hyper-V assicura che sia disponibile nella partizione radice memoria sufficiente e consente di memoria da assegnare alle partizioni figlio rimanente. Partizioni figlio devono essere ridimensionate in base alle esigenze del carico previsto per ogni macchina virtuale.

-   **Archiviazione**

    L'hardware di archiviazione deve avere sufficiente larghezza di banda dei / o e capacità per soddisfare le esigenze attuali e future di virtuale dei computer che ospita il server fisico. Quando si selezionano i controller di archiviazione e i dischi e scegliere la configurazione del RAID, prendere in considerazione tali requisiti. Distribuzione di macchine virtuali con carichi di lavoro estremamente intensivo dei dischi su dischi fisici diversi in genere può migliorare le prestazioni complessive. Ad esempio, se quattro macchine virtuali condividono un singolo disco e la Usa attivamente, ogni macchina virtuale può restituire solo il 25% della larghezza di banda del disco.

## <a name="power-plan-considerations"></a>Considerazioni sul piano di risparmio energia

Come una tecnologia di base, la virtualizzazione è un potente strumento utile per l'aumento di densità del carico di lavoro server, riducendo il numero di server fisici necessari nel tuo Data Center, aumentando l'efficienza operativa e riducendo i costi di consumo di potenza. Risparmio energia è essenziale per la gestione dei costi. 

In un ambiente di Data Center ideale, consumo di energia elettrica viene gestito mediante il consolidamento di lavoro nei computer fino a quando non sono che principalmente ci occupati e quindi la disattivazione di inattività delle macchine. Se questo approccio non è pratico, gli amministratori possono sfruttare le combinazioni di risparmio di energia negli host fisici per garantire che non usano più energia rispetto al necessario. 

Le tecniche di gestione di server alimentazione forniti con un costo, in particolare come tenant di carichi di lavoro non vengono considerati attendibili di dettare criteri sull'infrastruttura fisica del provider. Il software di livello host resta da inferire come ottimizzare la velocità effettiva, riducendo al contempo consumo di energia elettrica. Nelle macchine principalmente inattivo, ciò può causare l'infrastruttura fisica alla conclusione che moderato consumo energetico è appropriato, causando i carichi di lavoro di singoli tenant in esecuzione più lenta rispetto a potrebbero in caso contrario.

Windows Server utilizza la virtualizzazione in un'ampia gamma di scenari. Da un carico ridotto Server IIS a SQL Server moderata, a un host del cloud con Hyper-V che eseguono centinaia di macchine virtuali per server. Ognuno di questi scenari può avere requisiti univoci di hardware, software e delle prestazioni. Per impostazione predefinita, viene utilizzato Windows Server e vengono suggerite le **bilanciato** risparmio di energia che consente di risparmio energetico attraverso la scalabilità delle prestazioni del processore basato sull'utilizzo della CPU.

Con il **bilanciato** risparmio di energia, il più elevato di stati di alimentazione (e latenze più basse di risposta nei carichi di lavoro tenant) vengono applicate solo quando l'host fisico è relativamente occupato. Se si prediligono risposta deterministica, a bassa latenza per tutti i carichi di lavoro del tenant, è necessario considerare il passaggio dal valore predefinito **bilanciato** risparmio di energia per il **ad alte prestazioni** risparmio di energia. Il **ad alte prestazioni** risparmio di energia verrà i processori a velocità tutto il tempo, in modo efficace la disabilitazione di attivazione basata su richiesta con altre tecniche di gestione dell'alimentazione e ottimizzare le prestazioni del risparmio energia.

Per i clienti, che si sono soddisfatti con i risparmi derivanti dalla riduzione del numero di server fisici e per assicurarsi di raggiungimento di prestazioni massime per i carichi di lavoro virtualizzati, è consigliabile usare la **ad alte prestazioni** risparmio di energia.

Per altre indicazioni e informazioni dettagliate sull'utilizzo di risparmio di energia per ottimizzare l'infrastruttura, leggere [consigliato bilanciato Power prevede i parametri per tempi di risposta rapida](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Opzione di installazione dei componenti di base del server

Funzionalità di Windows Server 2016 l'opzione di installazione Server Core. Server Core offre un ambiente minimo per l'hosting di un set selezionato di ruoli di server, incluso Hyper-V. È dotato di un footprint del disco per l'host del sistema operativo e un attacco più piccolo e nell'area di manutenzione. Pertanto, è consigliabile che i server di virtualizzazione Hyper-V usano l'opzione di installazione Server Core.

Un'installazione Server Core offre una finestra della console solo quando l'utente è connesso, ma Hyper-V espone le funzionalità di gestione remoto compresi [Windows Powershell](https://technet.microsoft.com/library/hh848559.aspx) in modo che gli amministratori possono gestire in remoto.

## <a name="dedicated-server-role"></a>Ruolo server dedicato

La partizione radice deve essere dedicata a Hyper-V. Esecuzione di altri ruoli del server in un server che esegue Hyper-V può influenzare negativamente le prestazioni del server di virtualizzazione, specialmente se impiegano larghezza di banda della CPU, memoria o i/o significativo. Riducendo al minimo i ruoli del server nella partizione radice presenta vantaggi aggiuntivi, ad esempio la riduzione della superficie di attacco.

Gli amministratori di sistema devono valutare attentamente il software installato nella partizione radice perché alcuni software può compromettere le prestazioni complessive del server che esegue Hyper-V.

## <a name="guest-operating-systems"></a>Sistemi operativi guest

Hyper-V supporta ed è stata ottimizzata per un numero di diversi sistemi operativi guest. Il numero di processori virtuali che sono supportati per ogni guest dipende dal sistema operativo guest. Per un elenco dei sistemi operativi guest supportati, vedere [Panoramica di Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="cpu-statistics"></a>Statistiche sulla CPU

Hyper-V vengono pubblicati i contatori delle prestazioni per la descrizione del comportamento del server di virtualizzazione e l'utilizzo delle risorse del report. Il set standard di strumenti per la visualizzazione dei contatori delle prestazioni in Windows comprende Performance Monitor e Logman.exe, che può visualizzare e registrare i contatori delle prestazioni di Hyper-V. I nomi degli oggetti relativi contatori sono preceduti **Hyper-V**.

È necessario sempre misurare l'utilizzo della CPU del sistema fisico tramite i contatori delle prestazioni processore logico Hypervisor Hyper-V. L'utilizzo della CPU contatori tale report in Gestione attività e Performance Monitor nella radice e partizioni figlio non riflettere l'effettivo utilizzo della CPU fisica. Usare i contatori delle prestazioni seguenti per monitorare le prestazioni:

- **Processore logico Hypervisor Hyper-V (\*)\\% tempo di esecuzione totale** il tempo di inattività totale dei processori logici

- **Processore logico Hypervisor Hyper-V (\*)\\% tempo di esecuzione Guest** impiegato il tempo di esecuzione cicli all'interno di un utente guest o host

- **Processore logico Hypervisor Hyper-V (\*)\\% tempo di esecuzione Hypervisor** il tempo impiegato in esecuzione all'interno dell'hypervisor

- **Processore virtuale radice di Hypervisor Hyper-V (\*)\\\\** * misura l'utilizzo della CPU della partizione radice

- **Processore virtuale Hypervisor Hyper-V (\*)\\\\** * misura l'utilizzo della CPU delle partizioni di guest


## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura Hyper-V](architecture.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Prestazioni di I/O dell'archiviazione di Hyper-V](storage-io-performance.md)

-   [Prestazioni di I/O della rete di Hyper-V](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
