---
ms.assetid: d11acbc2-40c6-4ab2-9514-2bc3ad81499a
title: Novità di Deduplicazione dati
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 4a69221548d9defff5a45413ccfe824f9788755a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876452"
---
# <a name="whats-new-in-data-deduplication"></a>Novità di Deduplicazione dati

> Si applica a: Windows Server (canale semestrale), Windows Server 2016

[Deduplicazione dati](overview.md) in Windows Server 2016 è stata ottimizzata per essere estremamente efficiente, flessibile e gestibile a livello di cloud privato. Per altre informazioni sullo stack dell'archiviazione definita da software in Windows Server 2016, vedere [Novità nell'archiviazione in Windows Server 2016](../whats-new-in-storage.md).

Deduplicazione dati include i seguenti miglioramenti in Windows Server 2016:

| Funzionalità | Novità o aggiornamento | Descrizione |
|---------------|----------------|-------------|
| [Supporto per volumi di grandi dimensioni](whats-new.md#large-volume-support) | Aggiornamento | Prima di Windows Server 2016, i volumi dovevano essere ridimensionati in modo specifico per la varianza prevista e i volumi di dimensioni superiori a 10 TB non erano buoni candidati per la deduplicazione. In Windows Server 2016 Deduplicazione dati supporta dimensioni di volume fino a 64 TB. |
| [Supporto per file di grandi dimensioni](whats-new.md#large-file-support) | Aggiornamento | Prima di Windows Server 2016 i file che raggiungevano 1 TB non erano buoni candidati per la deduplicazione. In Windows Server 2016 i file fino a 1 TB sono completamente supportati. |
| [Supporto per Nano Server](whats-new.md#nano-server-support) | Nuova | La deduplicazione dati è disponibile e completamente supportata nella nuova opzione di distribuzione Nano Server per Windows Server 2016. |
| [Supporto del backup semplificato](whats-new.md#simple-backup-support) | Nuova | In Windows Server 2012 R2 le applicazioni di backup virtualizzato come [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx) di Microsoft erano supportate grazie a una serie di passaggi di configurazione manuale. In Windows Server 2016 è stato aggiunto il nuovo tipo di utilizzo predefinito "Backup", per una distribuzione semplice di Deduplicazione dati per le applicazioni di backup virtualizzato.|
| [Supporto per l'aggiornamento in sequenza del sistema operativo cluster](whats-new.md#cluster-upgrade-support) | Nuova | La deduplicazione dati supporta la nuova funzionalità [Aggiornamento in sequenza del sistema operativo cluster](../..//failover-clustering/cluster-operating-system-rolling-upgrade.md) di Windows Server 2016. |

## <a name="large-volume-support"></a>Supporto per volumi di grandi dimensioni

**Valore aggiunto da queste modifiche**  
In Windows Server 2012 R2 per ottenere prestazioni ottimali di Deduplicazione dati, i volumi dovevano essere dimensionati in modo corretto per garantire che il processo di ottimizzazione potesse allinearsi con la frequenza delle modifiche di dati, o "varianza". In genere, ciò significava che Deduplicazione dati era efficace solo nei volumi fino a 10 TB, a seconda dei modelli di scrittura del carico di lavoro.

In Windows Server 2016, Deduplicazione dati è estremamente efficiente nei volumi fino a 64 TB.

**Differenze di funzionamento**  
In Windows Server 2012 R2, la pipeline del processo di deduplicazione dati usa un thread singolo e una coda delle operazioni di I/O per ogni volume. Per garantire che i processi di ottimizzazione non rimanessero indietro, causando una diminuzione della frequenza complessiva dell'ottimizzazione del volume, i set di dati di grandi dimensioni dovevano essere suddivisi in volumi più piccoli. Le dimensioni del volume appropriate dipendevano dalla varianza prevista per il volume. In media il valore massimo era ~ 6-7 TB per volumi a varianza elevata e ~9-10 TB per volumi a varianza ridotta.

In Windows Server 2016 la pipeline di elaborazione di Deduplicazione dati è stata riprogettata per l'esecuzione di più thread in parallelo tramite più code I/O per ogni volume. In questo modo si ottengono prestazioni che prima erano possibili solo dividendo i dati in diversi volumi più piccoli. La figura seguente rappresentata questa modifica:

![Confronto tra la pipeline del processo di deduplicazione dati in Windows Server 2012 R2 e Windows Server 2016](media/server-2016-dedup-job-pipeline.png)

Queste ottimizzazioni si applicano a [tutti i processi di deduplicazione dati](understand.md#job-info), non solo al processo di ottimizzazione.

## <a name="large-file-support"></a>Supporto per file di grandi dimensioni
**Valore aggiunto da queste modifiche**  
In Windows Server 2012 R2, i file molto grandi non sono candidati ideali per la deduplicazione dati a causa di una riduzione delle prestazioni della pipeline di elaborazione della deduplicazione. In Windows Server 2016 la deduplicazione dei file fino a 1 TB è molto efficiente e consente agli amministratori di applicare le ottimizzazioni della deduplicazione a una gamma più ampia di carichi di lavoro. Ad esempio, consente di eseguire la deduplicazione di file di grandi dimensioni in genere associata a carichi di lavoro di backup.

**Differenze di funzionamento**  
In Windows Server 2016 Deduplicazione dati sfrutta nuove strutture della mappa di flusso e altri miglioramenti avanzati per migliorare la velocità effettiva di ottimizzazione e le prestazioni di accesso. La pipeline di elaborazione di Deduplicazione dati ora può riprendere il processo di ottimizzazione dopo un failover, anziché effettuare un riavvio. Queste modifiche rendono la deduplicazione su file fino a 1 TB estremamente efficiente.

## <a name="nano-server-support"></a>Supporto per Nano Server
**Valore aggiunto da queste modifiche**  
Nano Server è una nuova opzione di distribuzione headless di Windows Server 2016 che prevede un footprint di risorse di sistema molto piccolo, si avvia molto più velocemente e richiede un numero minore di aggiornamenti e riavvii rispetto all'opzione di distribuzione Windows Server Core. Deduplicazione dati è completamente supportata in Nano Server. Per altre informazioni su Nano Server, vedere [Getting Started with Nano Server](../../get-started/getting-started-with-nano-server.md) (Introduzione a Nano Server).

## <a name="simple-backup-support">Configurazione semplificata per le applicazioni di backup virtualizzato</a>
**Valore aggiunto da queste modifiche**  
Sebbene Deduplicazione dati per le applicazioni di backup virtualizzato sia supportata in Windows Server 2012 R2, richiede la regolazione manuale delle impostazioni di deduplicazione. In Windows Server 2016 la configurazione della deduplicazione per le applicazioni di backup virtualizzato è notevolmente semplificata. Usa un'opzione di tipo di utilizzo predefinita quando viene abilitata la deduplicazione per un volume, proprio come le opzioni per File Server generale e VDI.

## <a name="cluster-upgrade-support">Supporto per l'aggiornamento in sequenza del sistema operativo cluster</a>
**Valore aggiunto da queste modifiche**  
I cluster di failover di Windows Server che eseguono Deduplicazione dati possono contenere una combinazione di nodi che eseguono la versione di Deduplicazione dati di Windows Server 2012 R2 e nodi che eseguono la versione di Windows Server 2016. Questo miglioramento offre l'accesso completo a tutti i volumi deduplicati durante un aggiornamento in sequenza del cluster, consentendo l'implementazione graduale della nuova versione di Deduplicazione dati su un cluster di Windows Server 2012 R2 esistente senza incorrere in tempi di inattività per eseguire l'aggiornamento di tutti i nodi in una sola volta.

**Differenze di funzionamento**<br />
Per le versioni precedenti di Windows Server era necessario che tutti i nodi di un cluster di failover di Windows Server avessero la stessa versione di Windows Server. A partire da Windows Server 2016, la funzionalità di aggiornamento in sequenza del cluster consente l'esecuzione del cluster in modalità mista. Deduplicazione dati supporta questa nuova configurazione del cluster in modalità mista per abilitare l'accesso completo ai dati durante un aggiornamento in sequenza del cluster.
