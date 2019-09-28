---
title: Panoramica di File server di scalabilità orizzontale per dati applicazioni
description: Panoramica della funzionalità File server di scalabilità orizzontale per Windows Server 201 R2 e Windows Server 2012.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: da7c90bdb1c4a2fbdb2e518f34abe9cbfef2fc29
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392040"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>Panoramica di File server di scalabilità orizzontale per dati applicazioni

>Si applica a: Windows Server 2012 R2, Windows Server 2012

File server di scalabilità orizzontale è una funzionalità progettata per fornire condivisioni file di scalabilità orizzontale continuamente disponibili per l'archiviazione di applicazioni server basate su file. Tali condivisioni consentono di condividere la stessa cartella da più nodi dello stesso cluster. Questo scenario illustra come pianificare e distribuire il File server di scalabilità orizzontale.

È possibile distribuire e configurare un file server cluster usando uno dei metodi seguenti:

- **File server di scalabilità orizzontale per i dati delle applicazioni** Questa funzionalità di file server cluster è stata introdotta in Windows Server 2012 e consente di archiviare i dati delle applicazioni server, ad esempio i file delle macchine virtuali Hyper-V, le condivisioni file e ottenere un livello di affidabilità, disponibilità, gestibilità e elevato analogo. prestazioni che ci si aspetta da una rete di archiviazione. Tutte le condivisioni file sono online su tutti i nodi contemporaneamente. Le condivisioni file associate a questo tipo di file server del cluster sono denominate condivisioni file di scalabilità orizzontale. Tale modalità è anche definita attivo-attivo. Questo è il tipo di file server consigliato quando si distribuisce Hyper-V su Server Message Block (SMB) o Microsoft SQL Server su SMB.
- **File server per uso generale** Si tratta della continuazione del file server del cluster supportato in Windows Server fin dall'introduzione della funzionalità Clustering di failover. Questo tipo di file server del cluster, e quindi tutte le condivisioni a esso associate, è online su un nodo alla volta. A volte, tale modalità è anche definita attivo-passivo o doppio-attivo. Le condivisioni file associate a questo tipo di file server del cluster sono denominate condivisioni file del cluster. Questo è il tipo di file server consigliato quando si distribuiscono scenari di tipo Information Worker.

## <a name="scenario-description"></a>Descrizione dello scenario

Con le condivisioni file a scalabilità orizzontale è possibile condividere la stessa cartella da più nodi di un cluster. Se, ad esempio, si dispone di un cluster file server a quattro nodi che utilizza la scalabilità orizzontale SMB (Server Message Block), un computer che esegue Windows Server 2012 R2 o Windows Server 2012 può accedere a condivisioni file da uno qualsiasi dei quattro nodi. A questo scopo è necessario usufruire delle nuove funzionalità di clustering di failover di Windows Server e delle capacità del protocollo file server di Windows, SMB 3.0. Gli amministratori dei file server possono offrire condivisioni file di scalabilità orizzontale e servizi file continuamente disponibili alle applicazioni server e rispondere alle numerose richieste in modo rapido, semplicemente portando online un numero maggiore di server. Tutto questo è possibile in un ambiente di produzione ed è completamente trasparente all'applicazione server.

I vantaggi principali offerti dal file server di scalabilità orizzontale includono:

- **Condivisioni file attive-attive**. Tutti i nodi del cluster possono accettare e gestire le richieste del client SMB. Rendendo il contenuto della condivisione file accessibile attraverso tutti i nodi del cluster contemporaneamente, i cluster e i client SMB 3.0 collaborano per fornire un failover trasparente a nodi del cluster alternativi durante la manutenzione pianificata e gli errori non pianificati che determinano un'interruzione del servizio.
- **Maggiore larghezza di banda**. La larghezza di banda massima della condivisione corrisponde alla larghezza di banda totale di tutti i nodi del cluster file server. Diversamente dalla versioni precedenti di Windows Server, la larghezza di banda totale non è più vincolata alla larghezza di banda di un singolo nodo del cluster. I vincoli vengono invece definiti dalla capacità del sistema di archiviazione di supporto. È possibile aumentare la larghezza di banda totale mediante l'aggiunta di nodi.
- **Chkdsk senza tempi di inattività**. CHKDSK in Windows Server 2012 è notevolmente migliorato per ridurre drasticamente il tempo in cui un file system è offline per il ripristino. I volumi condivisi cluster (CSV) migliorano ulteriormente questo passaggio eliminando del tutto la fase offline. Un file system CSVFS può usare l'utilità CHKDSK senza produrre alcun impatto sulle applicazioni con handle aperti sul file system.
- **Cache del volume condiviso cluster**. CSVs in Windows Server 2012 introduce il supporto per una cache di lettura, che può migliorare significativamente le prestazioni in determinati scenari, ad esempio nell'infrastruttura VDI (Virtual Desktop Infrastructure).
- **Gestione più semplice**. Con File server di scalabilità orizzontale, si creano i file server di scalabilità orizzontale e quindi si aggiungono le CSVs e le condivisioni file necessarie. Non è più necessario creare file server del cluster multipli, ognuno con dischi cluster separati, e quindi sviluppare criteri di posizione per garantire l'attività su ogni nodo del cluster.
- **Ribilanciamento automatico dei client di file server di scalabilità orizzontale**. In Windows Server 2012 R2 il ribilanciamento automatico migliora la scalabilità e la gestibilità per i file server di scalabilità orizzontale. Le connessioni client SMB vengono registrate per ogni condivisione file, anziché per ogni server, e i client vengono quindi reindirizzati al nodo del cluster con l'accesso migliore al volume utilizzato dalla condivisione file. In questo modo si ottiene un miglioramento dell'efficienza grazie alla riduzione del reindirizzamento del traffico tra i nodi del file server. I client vengono reindirizzati in seguito alla connessione iniziale e alla riconfigurazione dell'archiviazione del cluster.

## <a name="in-this-scenario"></a>In questo scenario

Gli argomenti seguenti sono disponibili per semplificare la distribuzione di un file server di scalabilità orizzontale:

- [Pianificare File server di scalabilità orizzontale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [Passaggio 1: Pianificare l'archiviazione in File server di scalabilità orizzontale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Passaggio 2: Pianificare la rete in File server di scalabilità orizzontale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Distribuisci File server di scalabilità orizzontale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [Passaggio 1: Installare i prerequisiti per File server di scalabilità orizzontale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [Passaggio 2: Configurare File server di scalabilità orizzontale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [Passaggio 3: Configurare Hyper-V per l'uso di File server di scalabilità orizzontale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [Passaggio 4: Configurare Microsoft SQL Server per l'uso di File server di scalabilità orizzontale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

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
<td>Resilient file System (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">refs</a>)</td>
<td>Consigliato con Spazi di archiviazione diretta</td>
<td>Consigliato con Spazi di archiviazione diretta</td>
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
<td>Non consigliato<em></td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>File offline (memorizzazione nella cache lato client)</td>
<td>Yes</td>
<td>Non consigliata</em></td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Profili utente mobili</td>
<td>Yes</td>
<td>Non consigliato<em></td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Home directory</td>
<td>Yes</td>
<td>Non consigliata</em></td>
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

\*Reindirizzamento cartelle, File offline, profili utente mobili o Home Directory generano un numero elevato di scritture che devono essere scritte immediatamente su disco (senza buffering) quando si usano condivisioni file continuamente disponibili, riducendo le prestazioni rispetto a condivisioni file per utilizzo generico. Le condivisioni file a disponibilità continua non sono compatibili anche con Gestione risorse file server e con i computer che eseguono Windows XP. Inoltre, File offline potrebbe non passare alla modalità offline per 3-6 minuti dopo che un utente perde l'accesso a una condivisione, che potrebbe impedire agli utenti che non usano ancora la modalità sempre offline di File offline.

## <a name="practical-applications"></a>Applicazioni pratiche

I file server di scalabilità orizzontale sono ideali per l'archiviazione di applicazioni server. Alcuni esempi di applicazioni server che possono archiviare i dati nella condivisione file di scalabilità orizzontale sono elencati di seguito:

- Il server Web Internet Information Services (IIS) può archiviare la configurazione e i dati per siti Web su una condivisione file di scalabilità orizzontale. Per altre informazioni, vedere la pagina relativa alla [Configurazione condivisa](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264).
- Hyper-V può archiviare la configurazione e dischi virtuali live su una condivisione di scalabilità orizzontale. Per altre informazioni, vedere [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).
- SQL Server può archiviare file di database live su una condivisione di scalabilità orizzontale. Per altre informazioni, vedere [Installazione di SQL Server con l'opzione di archiviazione su condivisione file SMB](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option).
- Virtual Machine Manager (VMM) può archiviare una condivisione di libreria, che contiene modelli di macchine virtuali e file correlati, su una condivisione di scalabilità orizzontale. Tuttavia, il server di libreria non può essere una File server di scalabilità orizzontale, ma deve trovarsi in un server autonomo o in un cluster di failover che non usa il ruolo del cluster File server di scalabilità orizzontale.

Se si usa una condivisione file di scalabilità orizzontale come condivisione di libreria, sarà possibile usare solo le tecnologie compatibili con il File server di scalabilità orizzontale. Ad esempio, non è possibile usare Replica DFS per replicare una condivisione di libreria ospitata in una condivisione file di scalabilità orizzontale. È anche importante che nel file server di scalabilità orizzontale siano installati gli aggiornamenti software più recenti.

Per usare una condivisione file di scalabilità orizzontale come condivisione di libreria, aggiungere prima di tutto un server di libreria (probabilmente una macchina virtuale) con una condivisione locale o senza condivisioni. Quando si aggiunge una condivisione di libreria, scegliere una condivisione file ospitata in un file server con scalabilità orizzontale. Questa condivisione deve essere gestita da VMM e deve essere creata per l'uso esclusivo da parte del server di libreria. Assicurarsi anche di installare gli aggiornamenti più recenti nel file server di scalabilità orizzontale. Per ulteriori informazioni sull'aggiunta di server di libreria VMM e condivisioni di libreria, vedere [aggiungere profili alla libreria VMM](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801). Per un elenco di hotfix attualmente disponibili per Servizi file e archiviazione, vedere l' [articolo 2899011 della Microsoft Knowledge Base](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie).

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
<td>I cluster di failover hanno aggiunto le funzionalità seguenti in Windows Server 2012 per supportare il file server di scalabilità orizzontale: Il nome della rete distribuita, il tipo di risorsa File server di scalabilità orizzontale, i volumi condivisi del cluster (CSV) 2 e il ruolo di disponibilità elevata File server di scalabilità orizzontale. Per ulteriori informazioni su queste funzionalità, vedere <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">&#39;novità di clustering di failover in Windows Server 2012 [reindirizzamento]</a>.</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">Server Message Block</a></td>
<td>SMB 3,0 ha aggiunto le funzionalità seguenti in Windows Server 2012 per supportare il file server di scalabilità orizzontale: Failover trasparente SMB, SMB multicanale e SMB diretto.<br />
<br />
Per ulteriori informazioni sulle funzionalità nuove e modificate per SMB in Windows Server 2012 R2, vedere <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">&#39;novità di SMB in Windows Server</a>.</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>Altre informazioni

- [Guida alle considerazioni di progettazione dell'archiviazione definita dal software](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [Aumento della disponibilità di server, archiviazione e rete](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [Distribuzione di file server veloci ed efficienti per applicazioni server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [Vantaggi e svantaggi della scalabilità orizzontale](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) (post di blog)
- [Reindirizzamento cartelle, File offline e profili utente mobili](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)