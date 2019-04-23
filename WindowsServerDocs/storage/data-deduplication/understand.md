---
ms.assetid: acc0803b-fa05-4fc3-b94d-2916abf4fdbd
title: Informazioni sulla deduplicazione dati
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: cb17329fb0556a25bc49c2fdb6b16f878aa34194
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839382"
---
# <a name="understanding-data-deduplication"></a>Informazioni sulla deduplicazione dati

> Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo documento descrive come funziona [Deduplicazione dati](overview.md).

## <a name="how-does-dedup-work"></a>Come funziona la deduplicazione dei dati?

La Deduplicazione dati in Windows Server è stata creata con i due principi seguenti:

1. **Ottimizzazione non deve ostacolare la scrittura su disco**  
    La deduplicazione dati consente di ottimizzare i dati usando un modello di post-elaborazione. Tutti i dati vengono scritti sul disco senza essere ottimizzati per poi essere ottimizzati in un secondo tempo con la deduplicazione dati.

2. **Ottimizzazione non deve modificare la semantica di accesso**  
    Gli utenti e le applicazioni che accedono ai dati in un volume ottimizzato non sono affatto consapevoli che i file a cui stanno accedendo sono stati deduplicati.

Dopo essere stata abilitata per un volume, la deduplicazione dati viene eseguita in background per:

- Identificare i modelli ripetuti in tutti i file di quel volume.
- Spostare facilmente queste porzioni, o blocchi, con puntatori speciali di nome [reparse point](#dedup-term-reparse-point) che puntano a una copia univoca di quel blocco.

Ciò si verifica nei quattro passaggi seguenti:

1. Analizzare il file system per individuare i file che soddisfano i criteri di ottimizzazione.  
![Analisi del file system](media/understanding-dedup-how-dedup-works-1.gif)  
2. Suddivisione dei file in blocchi di dimensioni variabili.  
![Suddivisione dei file in blocchi](media/understanding-dedup-how-dedup-works-2.gif)
3. Identificazione dei blocchi univoci.  
![Identificazione dei blocchi univoci](media/understanding-dedup-how-dedup-works-3.gif)
4. Collocazione dei blocchi nell'archivio blocchi e, facoltativamente, compressione.  
![Spostamento nell'archivio blocchi](media/understanding-dedup-how-dedup-works-4.gif)
5. Sostituzione del flusso di file originale ora ottimizzato con un reparse point nell'archivio blocchi.  
![Sostituzione del flusso di file con un reparse point](media/understanding-dedup-how-dedup-works-5.gif)

Quando vengono letti i file ottimizzati, il file system invia i file con un reparse point al filtro del file system per la deduplicazione dati (Dedup.sys). Il filtro reindirizza l'operazione di lettura ai blocchi appropriati che costituiscono il flusso per il file nell'archivio blocchi. Le modifiche apportate agli intervalli di un file deduplicato vengono scritte sul disco e ottimizzate alla sua esecuzione successiva tramite il [processo di ottimizzazione](understand.md#job-info).

## <a id="usage-type"></a>Tipi di utilizzo
I seguenti tipi di uso indicano la configurazione più ragionevole di deduplicazione dati per carichi di lavoro comuni:  

| Tipo di uso | Carichi di lavoro ideali | Differenze |
|------------|-----------------|------------------|
| <a id="usage-type-default"></a>Default | File server per uso generale:<ul><li>Condivisioni del team</li><li>Cartelle di lavoro</li><li>Reindirizzamento cartelle</li><li>Condivisioni per lo sviluppo di software</li></ul> | <ul><li>Ottimizzazione in background</li><li>Criteri predefiniti di ottimizzazione:<ul><li>Età minima del file = 3 giorni</li><li>Ottimizzazione dei file in uso = No</li><li>Ottimizzazione dei file parziali = No</li></ul></li></ul> |
| <a id="usage-type-hyperv"></a>Hyper-V | Server VDI (Virtual Desktop Infrastructure) | <ul><li>Ottimizzazione in background</li><li>Criteri predefiniti di ottimizzazione:<ul><li>Età minima del file = 3 giorni</li><li>Ottimizzazione dei file in uso = Sì</li><li>Ottimizzazione dei file parziali = Sì</li></ul></li><li>Modifiche avanzate per l'interoperabilità di Hyper-V</li></ul> |
| <a id="usage-type-backup"></a>Backup | Applicazioni di backup virtualizzate, ad esempio [Microsoft Data Protection Manager (DPM)](https://technet.microsoft.com/library/hh758173.aspx) | <ul><li>Ottimizzazione della priorità</li><li>Criteri predefiniti di ottimizzazione:<ul><li>Età minima del file = 0 giorni</li><li>Ottimizzazione dei file in uso = Sì</li><li>Ottimizzazione dei file parziali = No</li></ul></li><li>Modifiche avanzate per l'interoperabilità con soluzioni DPM/analoghe a DPM</li></ul> |

## <a id="job-info"></a>Processi
La deduplicazione dati usa una strategia di post-elaborazione per ottimizzare e mantenere l'efficienza dello spazio del volume.

| Nome processo | Descrizioni del processo | Pianificazione predefinita |
|----------|------------------|------------------|
| <a id="job-info-optimization"></a>Ottimizzazione | Il processo **Ottimizzazione** esegue la deduplicazione suddividendo i dati in blocchi su un volume in base alle impostazioni dei criteri del volume, comprimendo facoltativamente tali blocchi e archiviandoli in modo univoco nell'archivio blocchi. Il processo di ottimizzazione che usa la deduplicazione dati è descritto in dettaglio in [Come funziona la deduplicazione dati?](understand.md#how-does-dedup-work). | Una volta ogni ora |
| <a id="job-info-gc"></a>Operazione di Garbage Collection | Il processo **Garbage Collection** richiede il recupero di spazio su disco rimuovendo blocchi inutili a cui i file che sono stati recentemente modificati o eliminati non fanno più riferimento. | Ogni sabato alle 2:35 |
| <a id="job-info-scrubbing"></a>Pulitura dell'integrità | Il processo **Pulitura dell'integrità** identifica eventuali danneggiamenti nell'archivio blocchi causati da errori del disco o settori danneggiati. Quando possibile, la deduplicazione dati può usare automaticamente le funzionalità di volume (come mirror o parità in un volume di Spazi di archiviazione) per ricostruire i dati danneggiati. Tramite la deduplicazione dati vengono anche mantenute copie di backup dei blocchi usati più di frequente quando vi viene fatto riferimento più di 100 volte in un'area denominata hotspot. | Ogni sabato alle 3:35 |
| <a id="job-info-unoptimization"></a>Annullamento dell'ottimizzazione | Il processo **Annullamento dell'ottimizzazione** può essere eseguito solo manualmente. Questo processo speciale annulla l'ottimizzazione eseguita dalla deduplicazione e disabilita la deduplicazione dati per il volume. | [Solo su richiesta](run.md#disabling-dedup) |

## <a id="dedup-term"></a>Terminologia di deduplicazione dati
| Nome | Definizione |
|------|------------|
| <a id="dedup-term-chunk"></a>blocco | Un blocco è una sezione di un file che potrebbe verificarsi in altri file simili secondo l'algoritmo di suddivisione in blocchi di Deduplicazione dati. |
| <a id="dedup-term-chunk-store"></a>Archivio blocchi | L'archivio dei blocchi è una serie organizzata di file contenitore nella cartella Informazioni del volume di sistema che usa la deduplicazione dati per archiviare in modo univoco i blocchi. |
| <a id="dedup-term-dedup"></a>deduplicazione | Un'abbreviazione di Deduplicazione dati usata in PowerShell, nelle API e nei componenti di Windows Server e di uso comune nella community di Windows Server. |
| <a id="dedup-term-file-metadata"></a>I metadati dei file | Ogni file contiene metadati che descrivono le proprietà interessanti sul file che non sono correlate al contenuto principale del file. Ad esempio, data di creazione, data dell'ultima lettura, autore e così via. |
| <a id="dedup-term-file-stream"></a>Flusso di file | Il flusso di file è il contenuto principale del file. Questa è la parte del file che la deduplicazione dati ottimizza. |
| <a id="dedup-term-file-system"></a>File system | Il file system è la struttura dei dati su disco e software che consente al sistema operativo di archiviare i file sul supporto di archiviazione. La deduplicazione dati è supportata nei volumi NTFS formattati. |
| <a id="dedup-term-file-system-filter"></a>Filtro del file system | Un filtro del file system è un plug-in che modifica il comportamento predefinito del file system. Per mantenere la semantica di accesso, la deduplicazione dati usa un filtro del file system (Dedup.sys) per reindirizzare le letture del contenuto ottimizzato in modo completamente trasparente all'utente o applicazione che effettua la richiesta di lettura. |
| <a id="dedup-term-optimization"></a>Ottimizzazione | Un file viene considerato ottimizzato o deduplicato da Deduplicazione dati se è stato suddiviso in blocchi e i blocchi univoci sono stati archiviati nell'archivio blocchi. |
| <a id="dedup-term-in-policy"></a>Criteri di ottimizzazione | I criteri di ottimizzazione specificano quali file devono essere considerati per la deduplicazione dati. Ad esempio, i file potrebbero risultare non idonei ai criteri se sono completamente nuovi, aperti, in un determinato percorso del volume o di un determinato tipo. |
| <a id="dedup-term-reparse-point"></a>Il punto di analisi | Un [reparse point](https://msdn.microsoft.com/library/windows/desktop/aa365503.aspx) è un tag speciale che notifica al file system di passare le operazioni di I/O a un filtro specifico del file system. Quando il flusso di file del file è stato ottimizzato, la deduplicazione dati sostituisce il flusso di file con un reparse point, che consente alla funzionalità di mantenere la semantica di accesso per tale file. |
| <a id="dedup-term-volume"></a>Volume | Un volume è un costrutto di Windows per un'unità di archiviazione logica che può estendere diversi dispositivi di archiviazione fisica in un uno o più server. La deduplicazione è abilitata in base al principio di volume per volume. |
| <a id="dedup-term-workload"></a>carico di lavoro | Un carico di lavoro è un'applicazione che viene eseguita su Windows Server. Esempi dei carichi di lavoro includono file server a scopi generici, Hyper-V e SQL Server. |

> [!Warning]  
> A meno che non sia richiesto dal personale di supporto Microsoft autorizzato, non tentare di modificare manualmente l'archivio blocchi. Questa azione può comportare il danneggiamento o la perdita dei dati.

## <a name="frequently-asked-questions"></a>Domande frequenti
**La deduplicazione dati è diversa da altri prodotti di ottimizzazione?**  
Vi sono alcune differenze importanti tra la deduplicazione dati e altri prodotti comuni di ottimizzazione dell'archiviazione:

* *Come deduplicazione dati è diversa da una singola istanza di Store?*  
    Single Instance Store, o SIS, è una tecnologia precedente a Deduplicazione dati introdotta in Windows Storage Server 2008 R2. Single Instance Store ottimizzava un volume identificando i file completamente identici e sostituendoli con collegamenti logici a una singola copia di un file archiviato nell'archivio comune SIS. A differenza di Single Instance Store, Deduplicazione dati può risparmiare spazio da file che non sono identici ma condividono molti modelli comuni e da file che a loro volta contengono molti modelli ripetuti. Single Instance Store è stata deprecata in Windows Server 2012 R2 e rimossa in Windows Server 2016 a favore di Deduplicazione dati.

* *La deduplicazione dati è diversa dalla compressione NTFS?*  
    La compressione NTFS è una funzionalità di NTFS che può essere abilitata facoltativamente a livello di volume. Con la compressione NTFS ogni singolo file è ottimizzato singolarmente tramite la compressione in fase di scrittura. A differenza della compressione NTFS, Deduplicazione dati può risparmiare spazio da tutti i file in un volume. Questo rappresenta un vantaggio rispetto alla compressione NTFS perché i file possono avere <u>sia</u> una duplicazione interna, che è interessata dalla compressione NTFS, sia analogie con altri file nel volume, che non viene interessato dalla compressione NTFS. Deduplicazione dati include anche un modello di post-elaborazione, il che significa che i nuovi file o le modifiche ai file esistenti verranno scritti sul disco e ottimizzati solo in un momento successivo da Deduplicazione dati.

* *La deduplicazione dati è diversa dai formati di archivio come zip, rar, 7z, cab e così via.?*  
    I formati file di archivio come i file con estensione zip, rar, 7z, cab e così via eseguono la compressione su un set di file specificato. Come la deduplicazione dei dati, i modelli duplicati all'interno dei file e modelli duplicati tra file sono ottimizzati. È tuttavia necessario scegliere i file che si vuole includere nell'archivio. Anche la semantica di accesso è diversa. Per accedere a un file specifico all'interno dell'archivio, è necessario aprire l'archivio, selezionare un file specifico e decomprimere il file per l'uso. Deduplicazione dati funziona in modo trasparente per gli utenti e gli amministratori e non richiede un avvio manuale. Deduplicazione dati consente anche di mantenere la semantica di accesso: i file ottimizzati appaiono invariati dopo l'ottimizzazione.

**È possibile modificare le impostazioni di deduplicazione dati per il tipo di uso selezionato?**  
Sì. Anche se Deduplicazione dati offre impostazioni predefinite ragionevoli per **Carichi di lavoro consigliati**, può comunque risultare utile modificare le relative impostazioni per sfruttare al meglio l'archiviazione. Inoltre, altri carichi di lavoro [richiedono alcune modifiche per verificare che Deduplicazione dati non interferisca con il carico di lavoro](install-enable.md#enable-dedup-sometimes-considerations).

**Si eseguono manualmente un processo di deduplicazione dati?**  
Sì, [tutti i processi di Deduplicazione dati possono essere eseguiti manualmente](run.md#running-dedup-jobs-manually). Ciò può essere opportuno se i processi pianificati non sono stati eseguiti a causa di risorse di sistema insufficienti o di un errore. Inoltre, il processo di annullamento dell'ottimizzazione può essere eseguito solo manualmente.

**È possibile monitorare la cronologia dei risultati dei processi di deduplicazione dati?**  
Sì, [ogni processo di deduplicazione dati costituisce una voce nel registro eventi di Windows](run.md#monitoring-dedup).

**È possibile modificare le pianificazioni predefinite per i processi di deduplicazione dati sul mio sistema?**  
Sì, [tutte le pianificazioni sono configurabili](advanced-settings.md#modifying-job-schedules). Modificare le pianificazioni predefinite di Deduplicazione dati è particolarmente utile per garantire che i processi di Deduplicazione dati dispongano di tempo a sufficienza per essere completati e non siano in competizione per le risorse con il carico di lavoro.
