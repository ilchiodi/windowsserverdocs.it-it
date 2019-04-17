---
title: Scalabilità orizzontale File Server per la panoramica dei dati di applicazione
description: Panoramica della funzionalità di scalabilità orizzontale File Server per Windows Server 201 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 04e25e9c69062611d9d14c220614f148ac5de770
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082286"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>Scalabilità orizzontale File Server per la panoramica dei dati di applicazione

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Scalabilità orizzontale File Server è una funzionalità che è progettata per offrire condivisioni di file con scalabilità orizzontale continuamente disponibili per la memorizzazione delle applicazioni basate su file server. Condivisioni di file con scalabilità orizzontale consente di condividere la stessa cartella da più nodi del cluster stesso. In questo scenario viene descritto come pianificare e distribuire con scalabilità orizzontale File Server.

È possibile distribuire e configurare un cluster di file server utilizzando uno dei metodi seguenti:

- **File Server con scalabilità orizzontale dei dati delle applicazioni** Questa funzionalità server di cluster di file è stato introdotto in Windows Server 2012 e consente di archiviare i dati dell'applicazione server, ad esempio file delle macchine virtuali Hyper-V, condivisioni di file e ottenere un livello di affidabilità, la disponibilità, gestibilità e alta simile prestazioni che si aspetta da una rete SAN. Tutte le condivisioni di file sono in linea contemporaneamente in tutti i nodi. Condivisioni di file associate al tipo di file server cluster sono denominate condivisioni di file con scalabilità orizzontale. Ciò è detta anche attivo / attivo. Si tratta del tipo di server consigliata file quando si distribuisce uno Hyper-V su Server Message Block (SMB) o Microsoft SQL Server su SMB.
- **File Server per l'utilizzo generale** Questa è la continuazione del server in cluster di file supportati in Windows Server dopo l'introduzione di Clustering di Failover. Questo tipo di file server cluster e pertanto tutte le azioni associate a file server del cluster, è in linea in un nodo alla volta. Ciò è detta anche attiva-passiva o dual attiva. Condivisioni di file associate al tipo di file server cluster sono denominate condivisioni di file non in pila. Si tratta del tipo di server consigliata file durante la distribuzione di scenari di information worker.

## <a name="scenario-description"></a>Descrizione dello scenario

Con condivisioni di file con scalabilità orizzontale, è possibile condividere la stessa cartella da più nodi di un cluster. Ad esempio, se si dispone di un cluster di server file à quatre nœuds utilizzato con scalabilità orizzontale Server Message Block (SMB), un computer che esegue Windows Server 2012 R2 o Windows Server 2012 può accedere alle condivisioni di file da uno dei quattro nodi. Questa operazione viene eseguita tramite l'utilizzo delle nuove funzionalità di Clustering di Failover Windows Server e le funzionalità del protocollo del server di file di Windows, SMB 3.0. Gli amministratori del server di file possa fornire servizi file continuamente disponibili per le applicazioni server e condivisioni di file con scalabilità orizzontale e rispondere alle esigenze maggiori rapidamente da semplicemente portare in linea di più server. Tutto ciò può essere eseguito in un ambiente di produzione, ed è completamente trasparente per il server applicazioni.

Vantaggi offerti dal Server di File con scalabilità orizzontale in includono:

- **Condivisioni di file attivo / attivo**. Tutti i nodi del cluster possono accettare ed elaborare le richieste client SMB. Eseguendo il file di condividere contenuto accessibile a tutti i nodi del cluster contemporaneamente, client e i gruppi SMB 3.0 collaborano per fornire un failover trasparente per i nodi del cluster alternativo durante la manutenzione pianificata e non pianificati errori con il servizio interruzione.
- **Maggiore larghezza di banda**. La larghezza di banda massima condivisione è la larghezza di banda totale di tutti i nodi del cluster di file server. Diversamente dalle versioni precedenti di Windows Server, la larghezza di banda totale non è vincolato non è più alla larghezza di banda di un singolo nodo del cluster; ma piuttosto, la funzionalità del sistema di archiviazione di supporto vengono definiti i vincoli. È possibile aumentare la larghezza di banda totale mediante l'aggiunta di nodi.
- **CHKDSK con zero tempi di inattività**. CHKDSK in Windows Server 2012 è notevolmente migliorate per ridurre notevolmente l'ora di che un file system è in linea per il ripristino. I volumi condivisi cluster (CSVs) eseguire presente in modo più eliminando la fase non in linea. Un sistema di File CSV (CSVFS) è possibile utilizzare CHKDSK senza influire sui applicazioni con handle aperti nel file system.
- **Cache di un indice cluster Volume condiviso**. CSVs in Windows Server 2012 introduce il supporto per una cache di lettura, che può aumento significativo delle prestazioni in alcuni casi, ad esempio in Virtual Desktop Infrastructure (VDI).
- **Semplificare la gestione**. Con scalabilità orizzontale File Server, creare i file di scalabilità orizzontale server e quindi aggiungere la CSVs necessarie e le condivisioni di file. Non è più necessario creare più server di file non in pila, ognuno con dischi di cluster distinti e quindi lo sviluppo di criteri di posizione per garantire attività in ogni nodo del cluster.
- **Automatica ribilanciamento dei client con scalabilità orizzontale File Server**. In Windows Server 2012 R2 ribilanciamento automatico migliora la scalabilità e gestibilità per i file di scalabilità orizzontale server. Si desidera tenere traccia di connessioni client SMB per condivisione file (anziché per ogni server), e i client vengono quindi reindirizzati al nodo del cluster con l'accesso ottimale per il volume utilizzato per la condivisione file. Consente di migliorare l'efficienza grazie alla riduzione reindirizzare il traffico tra i nodi di un file server. I client vengono reindirizzati dopo la connessione iniziale e quando viene riconfigurata archiviazione cluster.

## <a name="in-this-scenario"></a>In questo scenario

Negli argomenti seguenti sono disponibili per la distribuzione di un File con scalabilità orizzontale Server:

- [Pianificare la scalabilità orizzontale File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [Passaggio 1: Pianificare per l'archiviazione nel File di scalabilità orizzontale Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Passaggio 2: Pianificare per le reti nel File di scalabilità orizzontale Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Distribuire File di scalabilità orizzontale Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [Passaggio 1: Installare i prerequisiti per la scalabilità orizzontale File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [Passaggio 2: Configurare il File con scalabilità orizzontale Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [Passaggio 3: Configurare Hyper-V per l'utilizzo con scalabilità orizzontale File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [Passaggio 4: Configurare Microsoft SQL Server per l'utilizzo con scalabilità orizzontale File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>Quando utilizzare i File con scalabilità orizzontale Server

Non utilizzare File con scalabilità orizzontale Server se il carico di lavoro genererà un numero elevato di operazioni sui metadati, ad esempio apertura di file, chiudere i file, creazione di nuovi file o la ridenominazione di un file esistente. Un tipica informativi genera una grande quantità di operazioni sui metadati. Se si è interessati la scalabilità e la semplicità che saranno disponibili e se è necessario solo tecnologie supportate con scalabilità orizzontale File Server, è necessario utilizzare un Server di File con scalabilità orizzontale.

Nella tabella seguente sono elencate le funzionalità in SMB 3.0, i comuni Windows file System, le tecnologie di gestione file server dei dati e carichi di lavoro comuni. È possibile visualizzare se è supportata la tecnologia con scalabilità orizzontale File Server, o se è necessario un tradizionale file server cluster (noto anche come un file server per l'utilizzo generale).

<table>
<thead>
<tr class="header">
<th>Area di tecnologia</th>
<th>Funzionalità</th>
<th>Cluster di Server i File di utilizzo generale</th>
<th>File di scalabilità orizzontale Server</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>SMB</td>
<td>Disponibilità continua SMB</td>
<td>Sì</td>
<td>Sì</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>Multicanale SMB</td>
<td>Sì</td>
<td>Sì</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>Diretto SMB</td>
<td>Sì</td>
<td>Sì</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>Crittografia SMB</td>
<td>Sì</td>
<td>Sì</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>Failover trasparente SMB</td>
<td>Sì (se è abilitata la disponibilità continua)</td>
<td>Sì</td>
</tr>
<tr class="even">
<td>File system</td>
<td>NTFS</td>
<td>Sì</td>
<td>N/D</td>
</tr>
<tr class="odd">
<td>File system</td>
<td>Resiliente File System (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>Consigliato con l'archivio di spaziatura diretto</td>
<td>Consigliato con l'archivio di spaziatura diretto</td>
</tr>
<tr class="even">
<td>File system</td>
<td>Cluster condiviso Volume File System (con estensione CSV)</td>
<td>N/D</td>
<td>Sì</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>BranchCache</td>
<td>Sì</td>
<td>No</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Dati Deduplication (Windows Server 2012)</td>
<td>Sì</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Dati Deduplication (Windows Server 2012 R2)</td>
<td>Sì</td>
<td>Sì (solo VDI)</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Radice del server DFS Namespace (REPLICHI) radice</td>
<td>Sì</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Server di destinazione della cartella DFS Namespace (REPLICHI)</td>
<td>Sì</td>
<td>Sì</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Replica DFS (che il servizio)</td>
<td>Sì</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Gestione risorse di file Server (schermate e le quote)</td>
<td>Sì</td>
<td>No</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Infrastruttura di classificazione dei file</td>
<td>Sì</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Controllo dell'accesso dinamico (accesso basato su attestazioni, CAP)</td>
<td>Sì</td>
<td>No</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Reindirizzamento cartelle</td>
<td>Sì</td>
<td>Non è consigliabile *</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>File non in linea (cache sul lato client)</td>
<td>Sì</td>
<td>Non è consigliabile *</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Profili utente</td>
<td>Sì</td>
<td>Non è consigliabile *</td>
</tr>
<tr class="odd">
<td>Gestione dei file</td>
<td>Home directory</td>
<td>Sì</td>
<td>Non è consigliabile *</td>
</tr>
<tr class="even">
<td>Gestione dei file</td>
<td>Cartelle di lavoro</td>
<td>Sì</td>
<td>No</td>
</tr>
<tr class="odd">
<td>NFS</td>
<td>Server NFS</td>
<td>Sì</td>
<td>No</td>
</tr>
<tr class="even">
<td>Applicazioni</td>
<td>Hyper-V</td>
<td>Non è consigliata</td>
<td>Sì</td>
</tr>
<tr class="odd">
<td>Applicazioni</td>
<td>Microsoft SQL Server</td>
<td>Non è consigliata</td>
<td>Sì</td>
</tr>
</tbody>
</table>

\ * Reindirizzamento cartelle, file non in linea, i profili utente o delle Home directory generare un numero elevato di operazioni di scrittura che devono essere scritte immediatamente nel disco (senza il buffer) quando si utilizzano le condivisioni file continuamente disponibili, la riduzione delle prestazioni rispetto alla condivisioni di file generico. Condivisioni di file disponibili sono anche continuamente compatibile con Gestione risorse File Server e PC che eseguono Windows XP. Inoltre, file non in linea potrebbe non eseguire la transizione alla modalità offline per 3-6 minuti dopo che un utente perde l'accesso a una condivisione, che può disagio per gli utenti che non utilizzano ancora la modalità di sempre non in linea di file non in linea.

## <a name="practical-applications"></a>Applicazioni pratiche

Server di File con scalabilità orizzontale sono la soluzione ideale per la memorizzazione delle applicazioni server. Alcuni esempi di applicazioni server che possono essere archiviati i dati in una condivisione file di scalabilità orizzontale sono elencate di seguito:

- Il server Web di Internet Information Services (IIS) possibile archiviare dati e la configurazione di siti Web in una condivisione file di scalabilità orizzontale. Per ulteriori informazioni, vedere [Configurazione condivisa](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264).
- Hyper-V può archiviare live dischi virtuali e configurazione in una condivisione file di scalabilità orizzontale. Per ulteriori informazioni, vedere [Distribuzione di Hyper-V su SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).
- SQL Server può archiviare i file di database attivo in una condivisione file di scalabilità orizzontale. Per ulteriori informazioni, vedere [installazione di SQL Server con file SMB condividere come un'opzione di archiviazione](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option).
- Virtual Machine Manager (VMM) può archiviare una condivisione di libreria (che contiene i modelli di macchine virtuali e i file correlati) in una condivisione file di scalabilità orizzontale. Tuttavia, il server di libreria non può essere un File Server con scalabilità orizzontale, deve essere un server autonomo o un cluster di failover che non utilizza il ruolo di cluster con scalabilità orizzontale File Server.

Se si utilizza una condivisione file di scalabilità orizzontale come una condivisione di libreria, è possibile utilizzare solo le tecnologie che sono compatibili con scalabilità orizzontale File Server. Ad esempio, è possibile utilizzare la replica DFS per replicare una condivisione di libreria ospitata in una condivisione file di scalabilità orizzontale. È inoltre importante che il file con scalabilità orizzontale server sono gli aggiornamenti software più recenti installati.

Per utilizzare una condivisione file di scalabilità orizzontale come una condivisione di libreria, innanzitutto aggiungere un server di libreria (probabilmente una macchina virtuale) con una condivisione locale o non condivisioni affatto. Quando si aggiunge una condivisione di libreria, quindi scegliere una condivisione di file che è ospitata in un file con scalabilità orizzontale server. La condivisione deve essere gestiti VMM e creati esclusivamente per l'utilizzo per il server della raccolta. Verificare di installare gli aggiornamenti più recenti del file di scalabilità orizzontale server. Per ulteriori informazioni sull'aggiunta di server di libreria VMM e condivisioni di libreria, vedere [Add profili alla libreria di VMM](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801). Per un elenco degli aggiornamenti rapidi attualmente disponibili per File e servizi di archiviazione, vedere [l'articolo della Microsoft Knowledge Base 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie).

>[!NOTE]
>Alcuni utenti, ad esempio information worker dispongono di carichi di lavoro che hanno un impatto maggiore sulle prestazioni. Ad esempio, operazioni like di apertura e chiusura file, creazione di nuovi file e ridenominazione dei file esistenti, quando eseguita da più utenti, di avere un impatto sulle prestazioni. Se una condivisione di file è abilitata con disponibilità continua, utilizzarlo per fornire l'integrità dei dati, ma influisce anche le prestazioni complessive. Disponibilità continua, è necessario che scrive dati tramite su disco per garantire l'integrità in caso di errore di un nodo del cluster in un File Server con scalabilità orizzontale. Un utente che consente di copiare i file di grandi dimensioni diverse in un file server può dare in modo significativo un rallentamento delle prestazioni in condivisione file sempre disponibile.

## <a name="features-included-in-this-scenario"></a>Caratteristiche incluse in questo scenario

Nella tabella seguente sono elencate le caratteristiche che fanno parte di questo scenario e viene descritto come supportano.

<table>
<thead>
<tr class="header">
<th>Funzionalità</th>
<th>Modalità di supporto in questo scenario</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">Clustering di failover</a></td>
<td>Cluster di failover aggiunte le caratteristiche seguenti in Windows Server 2012 per supportare i file di scalabilità orizzontale server: nome di rete distribuita, il tipo di risorsa con scalabilità orizzontale File Server, Cluster condiviso volumi (con estensione CSV) 2 e il ruolo di disponibilità elevata di scalabilità orizzontale File Server. Per ulteriori informazioni su queste funzionalità, vedere <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">What's New in Clustering di Failover in Windows Server 2012 [reindirizzate]</a>.</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">Server Message Block</a></td>
<td>SMB 3.0 aggiunte le caratteristiche seguenti in Windows Server 2012 per il supporto di scalabilità orizzontale File Server: Failover trasparente SMB, multicanale SMB e diretto di SMB.<br />
<br />
Per ulteriori informazioni sulla funzionalità nuove e modificate per SMB in Windows Server 2012 R2, vedere <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">What's New in SMB in Windows Server</a>.</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>Ulteriori informazioni

- [Considerazioni relative alla Guida alla progettazione di archiviazione definiti software](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [L'aumento di Server, archiviazione e disponibilità della rete](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [Distribuzione di server File veloce ed efficiente per le applicazioni Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [Per implementare la scalabilità disconnessi o non in scala, ovvero la domanda](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) (post di blog)
- [Reindirizzamento cartelle, file offline e profili utente mobili](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)