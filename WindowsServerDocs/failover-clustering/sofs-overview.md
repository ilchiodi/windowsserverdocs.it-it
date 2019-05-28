---
title: Panoramica di File server di scalabilità orizzontale per dati applicazioni
description: Panoramica della funzionalità di Scale-Out File Server per Windows Server 201 R2 e Windows Server 2012.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 3e6d67eee496d19b216a4366af51ab5736229cf0
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476147"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>Panoramica di File server di scalabilità orizzontale per dati applicazioni

>Si applica a: Windows Server 2012 R2, Windows Server 2012

File server di scalabilità orizzontale è una funzionalità progettata per fornire condivisioni file di scalabilità orizzontale continuamente disponibili per l'archiviazione di applicazioni server basate su file. Tali condivisioni consentono di condividere la stessa cartella da più nodi dello stesso cluster. Questo scenario illustra come pianificare e distribuire il File server di scalabilità orizzontale.

È possibile distribuire e configurare un file server cluster usando uno dei metodi seguenti:

- **File Server di scalabilità orizzontale per dati applicazioni** questa funzionalità in cluster file server è stato introdotto in Windows Server 2012 e consente di archiviare i dati dell'applicazione server, ad esempio file delle macchine virtuali Hyper-V in condivisioni file e ottenere un livello simile di affidabilità, disponibilità, gestibilità e prestazioni elevate che si aspetta da una rete di archiviazione. Tutte le condivisioni file sono online su tutti i nodi contemporaneamente. Le condivisioni file associate a questo tipo di file server del cluster sono denominate condivisioni file di scalabilità orizzontale. Tale modalità è anche definita attivo-attivo. Questo è il tipo di file server consigliato quando si distribuisce Hyper-V su Server Message Block (SMB) o Microsoft SQL Server su SMB.
- **File server per uso generale** Si tratta della continuazione del file server del cluster supportato in Windows Server fin dall'introduzione della funzionalità Clustering di failover. Questo tipo di file server del cluster, e quindi tutte le condivisioni a esso associate, è online su un nodo alla volta. A volte, tale modalità è anche definita attivo-passivo o doppio-attivo. Le condivisioni file associate a questo tipo di file server del cluster sono denominate condivisioni file del cluster. Questo è il tipo di file server consigliato quando si distribuiscono scenari di tipo Information Worker.

## <a name="scenario-description"></a>Descrizione dello scenario

Con le condivisioni file a scalabilità orizzontale è possibile condividere la stessa cartella da più nodi di un cluster. Ad esempio, se si dispone di un cluster di quattro nodi file server che utilizza Scale-Out di Server Message Block (SMB), un computer che esegue Windows Server 2012 R2 o Windows Server 2012 possa accedere alle condivisioni file da uno qualsiasi dei quattro nodi. A questo scopo è necessario usufruire delle nuove funzionalità di clustering di failover di Windows Server e delle capacità del protocollo file server di Windows, SMB 3.0. Gli amministratori dei file server possono offrire condivisioni file di scalabilità orizzontale e servizi file continuamente disponibili alle applicazioni server e rispondere alle numerose richieste in modo rapido, semplicemente portando online un numero maggiore di server. Tutto questo è possibile in un ambiente di produzione ed è completamente trasparente all'applicazione server.

I vantaggi principali offerti dal file server di scalabilità orizzontale includono:

- **Condivisioni file attivo-attivo**. Tutti i nodi del cluster possono accettare e rispondere alle richieste del client SMB. Rendendo il contenuto della condivisione file accessibile attraverso tutti i nodi del cluster contemporaneamente, i cluster e i client SMB 3.0 collaborano per fornire un failover trasparente a nodi del cluster alternativi durante la manutenzione pianificata e gli errori non pianificati che determinano un'interruzione del servizio.
- **Maggiore larghezza di banda**. La larghezza di banda massima è la larghezza di banda totale di tutti i nodi di cluster di file server. Diversamente dalla versioni precedenti di Windows Server, la larghezza di banda totale non è più vincolata alla larghezza di banda di un singolo nodo del cluster. I vincoli vengono invece definiti dalla capacità del sistema di archiviazione di supporto. È possibile aumentare la larghezza di banda totale mediante l'aggiunta di nodi.
- **CHKDSK senza tempi di inattività**. CHKDSK in Windows Server 2012 è migliorata in modo significativo per ridurre drasticamente l'intervallo l'ora di che un file system è tenuto offline per la riparazione. I volumi condivisi cluster (CSV) migliorano ulteriormente questo passaggio eliminando del tutto la fase offline. Un file system CSVFS può usare l'utilità CHKDSK senza produrre alcun impatto sulle applicazioni con handle aperti sul file system.
- **Cache di Volume condiviso cluster**. Volumi condivisi cluster in Windows Server 2012 introduce il supporto per una cache di lettura, che può migliorare significativamente le prestazioni in determinati scenari, ad esempio in Virtual Desktop Infrastructure (VDI).
- **Gestione più semplice**. Con Scale-Out File Server, creare i server di scalabilità orizzontale e quindi aggiungere i necessari volumi condivisi cluster e le condivisioni file. Non è più necessario creare file server del cluster multipli, ognuno con dischi cluster separati, e quindi sviluppare criteri di posizione per garantire l'attività su ogni nodo del cluster.
- **Ribilanciamento automatico dei client di tipo Scale-Out File Server**. In Windows Server 2012 R2, il ribilanciamento automatico migliora la scalabilità e gestibilità per scale-out file server. Le connessioni client SMB vengono registrate per ogni condivisione file, anziché per ogni server, e i client vengono quindi reindirizzati al nodo del cluster con l'accesso migliore al volume utilizzato dalla condivisione file. In questo modo si ottiene un miglioramento dell'efficienza grazie alla riduzione del reindirizzamento del traffico tra i nodi del file server. I client vengono reindirizzati in seguito alla connessione iniziale e alla riconfigurazione dell'archiviazione del cluster.

## <a name="in-this-scenario"></a>In questo scenario

Gli argomenti seguenti sono disponibili per semplificare la distribuzione di un file server di scalabilità orizzontale:

- [Pianificazione di Scale-Out File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [Passaggio 1: Pianificare l'archiviazione in Scale-Out File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Passaggio 2: Plan for Networking in Scale-Out File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Distribuzione di Scale-Out File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [Passaggio 1: Installare i prerequisiti per Scale-Out File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [Passaggio 2: Configurare Scale-Out File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [Passaggio 3: Configurare Hyper-V per usare Scale-Out File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [Passaggio 4: Configurare Microsoft SQL Server per usare Scale-Out File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>Quando utilizzare un file server di scalabilità orizzontale

Non è consigliabile utilizzare un file server di scalabilità orizzontale se il proprio carico di lavoro genera un numero elevato di operazioni sui metadati, ad esempio l'apertura e la chiusura di file, la creazione di nuovi file o la ridenominazione dei file esistenti. Un tipico Information Worker genera di solito molte operazioni sui metadati. L'utilizzo di un file server di scalabilità orizzontale è opportuno se si è interessati alla scalabilità e alla semplicità che offre e sono necessarie solo tecnologie supportate con tale file server.

La tabella seguente elenca le capacità in SMB 3.0, i file system di Windows comuni, le tecnologie di gestione dei dati del file server e i carichi di lavoro comuni. È possibile verificare se la tecnologia è supportata con il File server di scalabilità orizzontale o se è necessario un file server cluster tradizionale, noto anche come file server per uso generale.

<table>
<thead>
<tr class="header">
<th>Area tecnologica</th>
<th>Funzionalità</th>
<th>File server cluster per uso generale</th>
<th>File server di scalabilità orizzontale</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>SMB</td>
<td>Disponibilità continua SMB</td>
<td>Yes</td>
<td>Yes</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>SMB multicanale</td>
<td>Yes</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB diretto</td>
<td>Yes</td>
<td>Yes</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>Crittografia SMB</td>
<td>Yes</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>Failover trasparente SMB</td>
<td>Sì (se è abilitata la disponibilità continua)</td>
<td>Yes</td>
</tr>
<tr class="even">
<td>File system</td>
<td>NTFS</td>
<td>Yes</td>
<td>N/D</td>
</tr>
<tr class="odd">
<td>File system</td>
<td>Resilient File System (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>Consigliato con archiviazione di spazi diretta</td>
<td>Consigliato con archiviazione di spazi diretta</td>
</tr>
<tr class="even">
<td>File system</td>
<td>File System del Volume condiviso cluster (CSV)</td>
<td>N/D</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>BranchCache</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Deduplicazione dati (Windows Server 2012)</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Deduplicazione dati (Windows Server 2012 R2)</td>
<td>Yes</td>
<td>Sì (solo VDI)</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Radice del server principale dello spazio dei nomi DFS</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Server di destinazione della cartella dello spazio dei nomi DFS</td>
<td>Yes</td>
<td>Yes</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Replica DFS (DFSR)</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Gestione risorse file server (schermate e quote)</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Infrastruttura di classificazione file</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Controllo dinamico degli accessi (accesso basato sulle attestazioni, CAP)</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Reindirizzamento cartelle</td>
<td>Yes</td>
<td>Non consigliata*</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>File offline (memorizzazione nella cache lato client)</td>
<td>Yes</td>
<td>Non consigliata*</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Profili utente mobili</td>
<td>Yes</td>
<td>Non consigliata*</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Home directory</td>
<td>Yes</td>
<td>Non consigliata*</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Cartelle di lavoro</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="odd">
<td>NFS</td>
<td>Server NFS</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr class="even">
<td>Applicazioni</td>
<td>Hyper-V</td>
<td>Non consigliata</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>Applicazioni</td>
<td>Microsoft SQL Server</td>
<td>Non consigliata</td>
<td>Yes</td>
</tr>
</tbody>
</table>

\* Reindirizzamento cartelle, file Offline, i profili utente mobili o Home directory generano un numero elevato di scritture che devono essere scritte immediatamente su disco (senza buffering) quando si usano condivisioni file sempre disponibili, la riduzione delle prestazioni rispetto a condivisioni di file generico. Le condivisioni file a disponibilità continua non sono compatibili anche con Gestione risorse file server e con i computer che eseguono Windows XP. È anche possibile che File offline non passi alla modalità offline per 3-6 minuti dopo la perdita di accesso a una condivisione da parte di un utente. Ciò può risultare frustrante per gli utenti che non usano ancora la modalità sempre offline di File offline.

## <a name="practical-applications"></a>Applicazioni pratiche

I file server di scalabilità orizzontale sono ideali per l'archiviazione di applicazioni server. Alcuni esempi di applicazioni server che possono archiviare i dati nella condivisione file di scalabilità orizzontale sono elencati di seguito:

- Il server Web Internet Information Services (IIS) può archiviare la configurazione e i dati per siti Web su una condivisione file di scalabilità orizzontale. Per altre informazioni, vedere la pagina relativa alla [Configurazione condivisa](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264).
- Hyper-V può archiviare la configurazione e dischi virtuali live su una condivisione di scalabilità orizzontale. Per altre informazioni, vedere [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).
- SQL Server può archiviare file di database live su una condivisione di scalabilità orizzontale. Per altre informazioni, vedere [Installazione di SQL Server con l'opzione di archiviazione su condivisione file SMB](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option).
- Virtual Machine Manager (VMM) può archiviare una condivisione di libreria, che contiene modelli di macchine virtuali e file correlati, su una condivisione di scalabilità orizzontale. Tuttavia, il server di libreria stesso non può essere un File Server di scalabilità orizzontale, ovvero deve essere inclusa in un server autonomo o cluster di failover che non usa il ruolo cluster File Server di scalabilità orizzontale.

Se si usa una condivisione file di scalabilità orizzontale come condivisione di libreria, sarà possibile usare solo le tecnologie compatibili con il File server di scalabilità orizzontale. Ad esempio, non è possibile usare la replica DFS per replicare una condivisione di libreria ospitata su una condivisione file di scalabilità orizzontale. È anche importante che nel file server di scalabilità orizzontale siano installati gli aggiornamenti software più recenti.

Per usare una condivisione file di scalabilità orizzontale come condivisione di libreria, aggiungere prima di tutto un server di libreria (probabilmente una macchina virtuale) con una condivisione locale o senza condivisioni. Quando si aggiunge una condivisione di libreria, scegliere una condivisione file ospitata su un file server di scalabilità orizzontale. Questa condivisione deve essere gestita da VMM e deve essere creata per l'uso esclusivo da parte del server di libreria. Assicurarsi anche di installare gli aggiornamenti più recenti nel file server di scalabilità orizzontale. Per altre informazioni sull'aggiunta di condivisioni di libreria e server di libreria VMM, vedere [aggiungere profili alla libreria VMM](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801). Per un elenco di hotfix attualmente disponibili per Servizi file e archiviazione, vedere l' [articolo 2899011 della Microsoft Knowledge Base](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie).

>[!NOTE]
>Alcuni utenti, ad esempio gli Information Worker, hanno carichi di lavoro che influiscono maggiormente sulle prestazioni. Ad esempio, operazioni quali l'apertura e la chiusura di file, la creazione di nuovi file e la ridenominazione dei file esistenti, se eseguite da più utenti, hanno un impatto significativo sulle prestazioni. Se una condivisione file è abilitata alla disponibilità continua, fornisce l'integrità dei dati, ma influisce anche sulle prestazioni complessive. La disponibilità continua richiede che i dati vengano scritti sul disco per assicurare l'integrità in caso di errore del nodo del cluster in un File server di scalabilità orizzontale. Un utente che copia molti file di grandi dimensioni in un file server potrà quindi aspettarsi prestazioni significativamente minori in una condivisione file a disponibilità continua.

## <a name="features-included-in-this-scenario"></a>Funzionalità incluse in questo scenario

Nella tabella che segue sono elencate le funzionalità che fanno parte di questo scenario e viene descritto in che modo lo supportano.

<table>
<thead>
<tr class="header">
<th>Funzionalità</th>
<th>Modalità di supporto dello scenario</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">Clustering di failover</a></td>
<td>I cluster di failover aggiunto le funzionalità seguenti in Windows Server 2012 per il supporto di tipo scale-Out file server: Nome rete distribuita, il tipo di risorsa di tipo Scale-Out File Server, volumi condivisi Cluster (CSV) 2 e il ruolo di tipo Scale-Out File Server a elevata disponibilità. Per altre informazioni su queste funzionalità, vedere <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">What ' s New in Failover Clustering in Windows Server 2012 [reindirizzamento]</a>.</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">Server Message Block</a></td>
<td>In Windows Server 2012 per supportare scale-Out File Server SMB 3.0 ha aggiunto le funzionalità seguenti: Failover trasparente SMB, SMB multicanale e SMB diretto.<br />
<br />
Per altre informazioni sulle funzionalità nuove e modificate per SMB in Windows Server 2012 R2, vedere <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">What ' s New in SMB in Windows Server</a>.</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>Altre informazioni

- [Software-Defined Storage Design Considerations Guide](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [Aumento della disponibilità di rete, archiviazione e Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [Distribuzione di File server veloci ed efficienti per applicazioni Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [Vantaggi e svantaggi della scalabilità orizzontale](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) (post di blog)
- [Reindirizzamento cartelle, file Offline e profili utente mobili](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)