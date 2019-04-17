---
title: Utilizzare volumi condivisi del Cluster in un cluster di failover
description: Come utilizzare volumi condivisi del Cluster in un cluster di failover.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f5bd0ad05bdc2573a5ea0abbe165de2d3e7f5c8f
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233517"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>Utilizzare volumi condivisi del Cluster in un cluster di failover

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cluster condiviso volumi (con estensione CSV) consentono più nodi in un cluster di failover contemporaneamente disporre dell'accesso in lettura / scrittura per lo stesso LUN (disco) che viene eseguito il provisioning come un volume NTFS. (In Windows Server 2012 R2, il disco possibile effettuare il provisioning come file system NTFS o resiliente File System (ReFS).) Con CSV, ruoli del cluster possono eseguire il failover rapidamente da un nodo in un altro nodo senza richiedere una modifica della proprietà unità, o la disinstallazione e reinstallazione un volume. CSV consentono inoltre di semplificare la gestione di un numero potenzialmente elevato di LUN in un cluster di failover.

CSV offrono un utilizzo generale, non in pila file system, i livelli di sopra NTFS (o ReFS in Windows Server 2012 R2). Le applicazioni CSV includono:

- Cluster di file disco rigido virtuale (VHD) per le macchine virtuali di cluster Hyper-V
- Condivisioni di file con scalabilità orizzontale per archiviare i dati dell'applicazione per il ruolo di cluster con scalabilità orizzontale File Server. File delle macchine virtuali Hyper-V e dati di Microsoft SQL Server sono esempi di dati applicazione per il ruolo. (Tenere presente che ReFS non è supportata per un File Server con scalabilità orizzontale). Per ulteriori informazioni sulla scalabilità orizzontale File Server, vedere [File Server con scalabilità orizzontale dei dati delle applicazioni](sofs-overview.md).

>[!NOTE]
>CSV non supporta il carico di lavoro non in pila di Microsoft SQL Server in SQL Server 2012 e versioni precedenti di SQL Server.

In Windows Server 2012 è stata notevolmente migliorate funzionalità CSV. Ad esempio, sono state rimosse dipendenze dei servizi di dominio Active Directory. È stato aggiunto il supporto per i miglioramenti a livello funzionale **chkdsk**, per l'interoperabilità con applicazioni antivirus e di backup e per l'integrazione con le funzionalità di archiviazione generale, ad esempio volumi crittografati con BitLocker e spazi di archiviazione. Per una panoramica delle funzionalità dalle CSV è stato introdotto in Windows Server 2012, vedere [What's New in Clustering di Failover di Windows Server 2012 \[redirected\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

Windows Server 2012 R2 introduce funzionalità aggiuntive, ad esempio di proprietà CSV distribuita, l'aumento resilienza attraverso la disponibilità del servizio di Server, una maggiore flessibilità nella quantità di memoria fisica che è possibile allocare alla cache CSV, meglio diagnosibility e interoperabilità avanzata che includa il supporto per deduplication e ReFS. Per ulteriori informazioni, vedere [What's New in Clustering di Failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

>[!NOTE]
>Per informazioni sull'utilizzo di dati deduplication in CSV per gli scenari di Virtual Desktop Infrastructure (VDI), vedere che il post di blog [Deduplication dati di distribuzione per l'archiviazione VDI in Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) e [estensione dati Deduplication ai nuovi carichi di lavoro in Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx).

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>Rivedere i requisiti e le considerazioni relative all'utilizzo CSV in un cluster di failover

Prima di utilizzare CSV in un cluster di failover, esaminare la rete, archiviazione e altri requisiti e considerazioni in questa sezione.

### <a name="network-configuration-considerations"></a>Considerazioni sulla configurazione di rete

Quando si configurano le reti che supportano CSV, considerare quanto segue.

- **Più reti e più schede di rete**. Per abilitare la tolleranza di errore in caso di errore di rete, è consigliabile che più reti cluster trasportare traffico CSV o configurare collaborato schede di rete.
    
    Se i nodi del cluster sono connessi alla rete che non devono essere utilizzati dal cluster, sarà opportuno disabilitarle. Ad esempio, è consigliabile disabilitare reti iSCSI per l'uso di cluster impedire il traffico CSV su tali reti. Per disabilitare una rete, in Gestione Cluster di Failover, selezionare **reti**, selezionare la rete, selezionare l'azione di **proprietà** e quindi selezionare **non consentire le comunicazioni di rete cluster della rete**. In alternativa, è possibile configurare la proprietà **Role** della rete utilizzando il cmdlet di Windows PowerShell [Get-ClusterNetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) .
- **Proprietà scheda di rete**. Nelle proprietà di tutte le schede che contengono le comunicazioni del cluster, assicurarsi che siano abilitate le impostazioni seguenti:

  - **Client per reti Microsoft** e **File e stampanti per reti Microsoft**. Queste impostazioni supportano Server Message Block (SMB) 3.0, che viene utilizzato per impostazione predefinita per eseguire il traffico CSV tra i nodi. Per abilitare SMB, verificare inoltre che il servizio Server e il servizio Workstation siano in esecuzione e che siano stati configurati per l'avvio automatico in ogni nodo del cluster.

    >[!NOTE]
    >In Windows Server 2012 R2 sono presenti più istanze del servizio per ogni nodo del cluster di failover. Non vi è l'istanza predefinita che gestisce il traffico in arrivo da client SMB che accedono a condivisioni file regolare e una seconda istanza CSV che gestisce solo il traffico tra i nodi CSV. Inoltre, se il servizio Server in un nodo diventa non integro, la proprietà CSV automaticamente esegue la transizione a un altro nodo.

    SMB 3.0 include le funzionalità multicanale SMB e diretto di SMB, che consentono il traffico CSV flusso su più reti nel cluster e utilizzare schede di rete che supportano remoto diretta memoria Access (RDMA). Per impostazione predefinita, multicanale SMB viene utilizzato per il traffico CSV. Per ulteriori informazioni, vedere [Cenni preliminari su Server Message Block](../storage/file-server/file-server-smb-overview.md).
  - **Cluster di Failover Microsoft scheda virtuale prestazioni filtro**. Questa impostazione consente di migliorare la capacità di nodi di eseguire il reindirizzamento dei / o quando è necessaria per raggiungere CSV, ad esempio, quando un errore di connettività impedisce a un nodo di connessione diretta a disco CSV. Per ulteriori informazioni, vedere [sui / o di sincronizzazione e il reindirizzamento dei / o comunicazioni CSV](#about-i/o-synchronization-and-i/o-redirection-in-csv-communication) più avanti in questo argomento.
- **Ordine di priorità rete cluster**. In generale, è consigliabile non modificare le preferenze di cluster configurati per le altre reti.
- **Configurazione di subnet IP**. È necessario per i nodi in una rete che utilizzano CSV alcuna configurazione subnet specifica. CSV può supportare multisubnet cluster.
- **Qualità del servizio (QoS) basata sui criteri**. È consigliabile configurare un criterio di priorità QoS e un criterio di larghezza di banda minima per il traffico di rete in ogni nodo quando si utilizza CSV. Per ulteriori informazioni, vedere [Qualità del servizio (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>).
- **Rete di archiviazione**. Per consigli di rete di archiviazione, rivedere le linee guida fornite per il fornitore di archiviazione. Per ulteriori considerazioni sull'archiviazione di CSV, vedere [requisiti di configurazione di archiviazione e disco](#storage-and-disk-configuration-requirements) più avanti in questo argomento.

Per una panoramica dell'hardware, rete e requisiti di archiviazione per i cluster di failover, vedere [requisiti Hardware di Clustering di Failover e le opzioni di archiviazione](clustering-requirements.md).

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>Sulla sincronizzazione dei / o e il reindirizzamento dei / o comunicazioni CSV

- **Sincronizzazione dei / o**: CSV consente più nodi abbiano accesso in lettura / scrittura contemporaneamente alla stessa archiviazione condivisa. Quando un nodo di input/output (i/o) su un volume CSV, il nodo comunica direttamente con l'archiviazione, ad esempio tramite una rete di archiviazione (SAN). Tuttavia, in qualsiasi momento, un singolo nodo (noti come il nodo del coordinatore) "è proprietario" la risorsa disco fisico associato al LUN. Il nodo del coordinatore per un volume CSV viene visualizzato in Gestione Cluster di Failover come **Proprietario del nodo** in **dischi**. Viene visualizzato anche nell'output del cmdlet di Windows PowerShell [Get-ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) .

  >[!NOTE]
  >In Windows Server 2012 R2, la proprietà CSV giustificata tra i nodi del cluster di failover in base al numero di volumi CSV ogni nodo del proprietario. Inoltre, la proprietà viene automaticamente il ribilanciamento di quando esistono condizioni, ad esempio CSV failover, un nodo si ricollega il cluster, aggiungere un nuovo nodo al cluster, si riavvia un nodo cluster o si avvia il cluster di failover dopo che è stato arrestato.

  Quando si verificano determinati piccole modifiche nel file system su un volume CSV, è necessario sincronizzare metadati in ognuno dei nodi fisici che accedono al LUN, non solo sul nodo unico coordinatore. Ad esempio, quando viene avviata, creata o eliminata una macchina virtuale in un volume CSV oppure quando viene eseguita la migrazione di una macchina virtuale, queste informazioni devono essere sincronizzati su tutti i nodi fisici che accedono alla macchina virtuale. Queste operazioni di aggiornamento dei metadati si verificano in parallelo in reti di cluster utilizzando SMB 3.0. Queste operazioni non richiedono tutti i nodi fisici per comunicare con l'archiviazione condivisa.

- **Reindirizzamento dei / o**: errori di connettività di archiviazione e alcune operazioni di archiviazione possono impedire un determinato nodo comunicano direttamente con l'archiviazione. Per gestire funzione mentre il nodo non comunica con l'archiviazione, il nodo reindirizza il disco dei / o tramite una rete cluster per il nodo del coordinatore dove il disco è attualmente installato. Se il nodo coordinatore corrente si è verificato un errore di connettività di archiviazione, tutte le operazioni dei / o su disco vengono accodate temporaneamente durante un nuovo nodo viene stabilito come nodo coordinatore.

Il server utilizza uno dei seguenti modalità reindirizzamento i/o, in base alla situazione:

- **Reindirizzamento di sistema di file** Reindirizzamento viene per volume, ad esempio, quando gli snapshot CSV derivano da un'applicazione di backup quando un volume CSV manualmente viene messo in modalità dei / o reindirizzata.
- **Reindirizzamento di blocco** Reindirizzamento è a livello di blocco dei file, ad esempio, se archiviazione si perde la connettività a un volume. Il reindirizzamento di blocco è notevolmente più veloce di reindirizzamento di file del sistema.

In Windows Server 2012 R2, è possibile visualizzare lo stato di un volume CSV in base al nodo. Ad esempio, è possibile visualizzare se i/o è diretto o reindirizzato oppure se non è disponibile il volume CSV. Se un volume CSV è in modalità dei / o reindirizzata, è inoltre possibile visualizzare il motivo. Utilizzare il cmdlet di Windows PowerShell **Get-ClusterSharedVolumeState** per visualizzare queste informazioni.

>[!NOTE]
> * In Windows Server 2012, a causa dei miglioramenti nella progettazione CSV, CSV eseguire ulteriori operazioni in modalità dei / o direttamente da quello visualizzato in Windows Server 2008 R2.
> * Grazie all'integrazione di CSV con funzionalità SMB 3.0, ad esempio multicanale SMB e diretto di SMB, può essere trasferito reindirizzato il traffico i/o su più reti cluster.
> * È consigliabile pianificare le reti del cluster per consentire l'aumento del traffico di rete per il nodo del coordinatore potenziale durante il reindirizzamento dei / o.

### <a name="storage-and-disk-configuration-requirements"></a>Requisiti di configurazione di archiviazione e disco

Per utilizzare CSV, l'archiviazione e i dischi devono soddisfare i requisiti seguenti:

- **Formato di file system**. In Windows Server 2012 R2, uno spazio su disco o di archiviazione per un volume CSV deve essere un disco di base che viene partizionato con file system NTFS o ReFS. In Windows Server 2012, uno spazio su disco o di archiviazione per un volume CSV deve essere un disco di base che viene partizionato con NTFS.

  CSV sono elencati i requisiti aggiuntivi seguenti:
    
  - In Windows Server 2012 R2, è possibile utilizzare un disco per CSV formattato con FAT o FAT32.
  - In Windows Server 2012, è possibile utilizzare un disco per CSV formattato con FAT, FAT32 o ReFS.
  - Se si desidera utilizzare uno spazio di archiviazione per un CSV, è possibile configurare uno spazio semplice o uno spazio mirror. In Windows Server 2012 R2, è inoltre possibile configurare uno spazio di controllo corrispondente. (In Windows Server 2012, CSV non supporta gli spazi di parità.)
  - CSV non può essere utilizzato come disco quorum controllo. Per ulteriori informazioni sul quorum del cluster, vedere [Understanding Quorum in spazi di archiviazione diretta](../storage/storage-spaces/understand-quorum.md).
  - Dopo aver aggiunto un disco in un formato CSV, viene indicato nel formato CSVFS (per Office System File CSV). In questo modo il cluster e altro software per differenziare l'archiviazione CSV da altri NTFS o archiviazione ReFS. In generale, CSVFS supporta la stessa funzionalità del file system NTFS o ReFS. Tuttavia, alcune funzionalità non sono supportate. In Windows Server 2012 R2, ad esempio, sarà possibile abilitare la compressione su CSV. In Windows Server 2012, è possibile abilitare deduplication dati o la compressione su CSV.
- **Il tipo di risorsa del cluster**. Per un volume CSV, è necessario utilizzare il tipo di risorsa disco fisico. Per impostazione predefinita, in questo modo viene configurato automaticamente un disco o spazio di archiviazione che viene aggiunto all'archiviazione del cluster.
- **Scelta di CSV dischi o altri dischi dell'archivio del cluster**. Quando si sceglie uno o più dischi per una macchina virtuale in cluster, valutare come verrà utilizzata ogni disco. Se un disco verrà utilizzato per archiviare i file creati mediante la tecnologia Hyper-V, ad esempio file VHD o i file di configurazione, è possibile scegliere tra i dischi CSV o gli altri dischi disponibili nell'archivio di cluster. Se un disco è un disco fisico è direttamente associata alla macchina virtuale (denominata anche un disco pass-through), non è possibile scegliere un disco CSV ed è necessario scegliere tra gli altri dischi disponibili nell'archivio di cluster.
- **Nome del percorso per l'identificazione dei dischi**. Dischi in CSV vengono identificati con un nome di percorso. Ogni percorso sembra sull'unità di sistema del nodo come volume numerato nella cartella **\\ClusterStorage** . In questo percorso è lo stesso quando viene visualizzato da qualsiasi nodo del cluster. Se necessario, è possibile rinominare i volumi.

Requisiti di archiviazione per CSV, rivedere le linee guida fornite per il fornitore di archiviazione. Per ulteriore spazio di archiviazione considerazioni per la pianificazione CSV, vedere [pianificare l'utilizzo CSV in un cluster di failover](#plan-to-use-csv-in-a-failover-cluster) più avanti in questo argomento.

### <a name="node-requirements"></a>Requisiti di nodo

Per utilizzare CSV, i nodi devono soddisfare i requisiti seguenti:

- **Lettera di unità della disco di sistema**. In tutti i nodi, la lettera di unità disco di sistema deve essere lo stesso.
- **Protocollo di autenticazione**. Il protocollo NTLM deve essere abilitato in tutti i nodi. Questa opzione è attivata per impostazione predefinita.

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>Pianificare l'utilizzo CSV in un cluster di failover

In questa sezione sono elencate considerazioni sulla pianificazione e consigli per l'utilizzo CSV in un cluster di failover che esegue Windows Server 2012 R2 o Windows Server 2012.

>[!IMPORTANT]
>Chiedere al fornitore di archiviazione per suggerimenti su come configurare l'unità di archiviazione specifici di CSV. Se i suggerimenti dal fornitore archiviazione differiscono dalle informazioni contenute in questo argomento, utilizzare i suggerimenti del fornitore di archiviazione.

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Disposizione di LUN, volumi e file VHD

Per utilizzare al meglio CSV per fornire l'archivio per le macchine virtuali in cluster, è utile rivedere come si potrebbe disporre LUN (dischi) quando si configurano i server fisici. Quando si configurano le macchine virtuali corrispondenti, provare a modificare i file VHD in modo analogo.

Si consideri un server fisico per cui l'organizzazione i dischi e i file nel modo seguente:

- File di sistema, inclusi i file di paging, su un disco fisico
- File di dati in un altro disco fisico

Per una macchina virtuale non in pila equivalente, è consigliabile organizzare i volumi e i file in modo simile:

- File di sistema, inclusi un file di paging in un file VHD in uno CSV
- File di dati in un file VHD in un'altra CSV

Se si aggiunge un'altra macchina virtuale, se possibile, è consigliabile mantenere la stessa disposizione per i dischi rigidi virtuali in tale macchina virtuale.

### <a name="number-and-size-of-luns-and-volumes"></a>Numero e dimensione dei volumi e LUN

Quando si pianifica la configurazione di archiviazione per un cluster di failover che utilizza CSV, tenere presente quanto segue:

- Decidere il numero LUN configurare, consultare il fornitore di archiviazione. Ad esempio, il fornitore di archiviazione può consigliabile configurare ogni LUN con una partizione e archiviare un volume CSV.
- Non esistono limiti per il numero di macchine virtuali che possono essere supportati in un singolo volume CSV. Tuttavia, è necessario considerare il numero di macchine virtuali che si prevede di avere nel cluster e il carico di lavoro (operazioni dei / o al secondo) per ogni macchina virtuale. Considera gli esempi che seguono:

  - Un'organizzazione viene effettuata la distribuzione di macchine virtuali che dovranno supportare un'infrastruttura VDI (VDI), che corrisponde a un carico di lavoro relativamente chiaro. Il cluster viene utilizzata l'archiviazione ad alte prestazioni. Amministrazione cluster, dopo aver consultato con il fornitore di archiviazione, decide di inserire un numero relativamente elevato di macchine virtuali per volume CSV.
  - Un'altra organizzazione viene effettuata la distribuzione di un numero elevato di macchine virtuali che dovranno supportare un'applicazione di database utilizzati frequentemente, che corrisponde a un maggiore carico di lavoro. Il cluster viene utilizzata l'archiviazione esegue inferiore. Amministrazione cluster, dopo aver consultato con il fornitore di archiviazione, decide di inserire un numero relativamente piccolo di macchine virtuali per volume CSV.
- Quando si pianifica la configurazione di archiviazione per una determinata macchina virtuale, prendere in considerazione i requisiti del servizio, applicazione o ruolo in grado di supportare la macchina virtuale su disco. Informazioni su questi requisiti consente di evitare conflitti di disco che possono causare un rallentamento delle prestazioni. Configurazione di archiviazione della macchina virtuale altamente dovrebbe essere simile a configurazione di archiviazione utilizzato per un server fisico che esegue il servizio stesso, applicazione o ruolo. Per ulteriori informazioni, vedere [disposizione di LUN, volumi e file VHD](#arrangement-of-luns,-volumes,-and-vhd-files) più indietro in questo argomento.

    È anche possibile attenuare conflitto tra dischi richiedendo archiviazione con un numero elevato di dischi rigidi fisici indipendenti. Scegliere l'hardware di archiviazione conseguenza e rivolgersi al fornitore per ottimizzare le prestazioni dello spazio di archiviazione.
- A seconda dei carichi di lavoro del cluster e le necessità di operazioni dei / o, è possibile prendere in considerazione solo una percentuale delle macchine virtuali per accedere a ogni LUN, mentre altre macchine virtuali non dispone di connettività e vengono invece dedicati per calcolare le operazioni di configurazione.

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>Aggiungere un disco in CSV in un cluster di failover

CSV è attivata per impostazione predefinita in Clustering di Failover. Per aggiungere un disco CSV, è necessario aggiungere un disco per il gruppo di **Archiviazione disponibili** del cluster (se non è già stato aggiunto) e quindi aggiungere il disco in CSV nel cluster. Per eseguire queste procedure, è possibile utilizzare Gestione Cluster di Failover o i cmdlet di Windows PowerShell per i cluster di Failover.

### <a name="add-a-disk-to-available-storage"></a>Aggiungere un disco di spazio di archiviazione

1. In Gestione di Cluster di Failover, nell'albero della console espandere il nome del cluster e quindi espandere **spazio di archiviazione**.
2. Pulsante destro del mouse **dischi**e quindi selezionare **Aggiungi disco**. Viene visualizzato un elenco che mostra i dischi che possono essere aggiunti per l'utilizzo in un cluster di failover.
3. Selezionare i dischi che si desidera aggiungere e quindi scegliere **OK**.

    I dischi ora sono assegnati al gruppo di **Archiviazione disponibili** .

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Comandi di Windows PowerShell equivalenti (aggiungere un disco di spazio di archiviazione)

Cmdlet di Windows PowerShell o i cmdlet seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in un'unica riga, anche se potrebbero apparire ritorno a capo automatico nelle diverse righe qui a causa di vincoli di formattazione.

Nell'esempio seguente vengono identificati i dischi pronti essere aggiunto al cluster e quindi viene aggiunto al gruppo di **Archiviazione disponibili** .

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>Aggiungere un disco di archiviazione disponibile in formato CSV

1. Nella gestione di Cluster di Failover, nell'albero della console espandere il nome del cluster, espandere **archiviazione**e quindi selezionare **i dischi**.
2. Selezionare uno o più dischi assegnati a **Spazio di archiviazione**, destro della selezione e quindi scegliere **Aggiungi a volumi condivisi del Cluster**.

    I dischi sono ora assegnati al gruppo di **Cluster condiviso Volume** del cluster. I dischi vengono esposte per ogni nodo del cluster come numerati volumi (punti di montaggio) nella cartella % SystemDisk % ClusterStorage. Vengono visualizzati i volumi del file System CSVFS.

>[!NOTE]
>È possibile rinominare i volumi CSV nella cartella % SystemDisk % ClusterStorage.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Comandi di Windows PowerShell equivalenti (aggiungere un disco in CSV)

Cmdlet di Windows PowerShell o i cmdlet seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in un'unica riga, anche se potrebbero apparire ritorno a capo automatico nelle diverse righe qui a causa di vincoli di formattazione.

Nell'esempio seguente viene aggiunto *Cluster disco 1* **Archiviazione disponibile** in CSV nel cluster locale.

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>Abilitare la cache CSV per lettura a elevata frequenza dei carichi di lavoro (facoltativi)

La cache CSV fornisce la memorizzazione nella cache a livello di blocco di sola lettura alle operazioni dei / o senza buffer tramite l'allocazione di memoria di sistema (RAM) come una cache di scrittura. (Operazioni dei / o senza buffer non sono memorizzata nella cache dal gestore della cache.) Ciò può migliorare le prestazioni per le applicazioni, ad esempio Hyper-V, che esegue le operazioni dei / o senza buffer quando si accede a un disco rigido virtuale. La cache CSV è possibile migliorare le prestazioni delle richieste di lettura senza la memorizzazione nella cache le richieste di scrittura. Attivazione della cache CSV è anche utile per gli scenari di scalabilità orizzontale File Server.

>[!NOTE]
>Si consiglia di abilitare la cache CSV per le distribuzioni in tutti i cluster Hyper-V e con scalabilità orizzontale File Server.

Per impostazione predefinita in Windows Server 2012, la cache CSV è disattivata. In Windows Server 2012 R2, la cache CSV è attivata per impostazione predefinita. Tuttavia, è comunque necessario allocare le dimensioni della cache del blocco di.

Nella tabella seguente vengono descritte le due impostazioni di configurazione che controllano la cache CSV.

<table>
<thead>
<tr class="header">
<th>Nome della proprietà in Windows Server 2012 R2</th>
<th>Nome della proprietà in Windows Server 2012</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>BlockCacheSize</strong></td>
<td><strong>SharedVolumeBlockCacheSizeInMB</strong></td>
<td>Si tratta di una proprietà comuni di cluster che consente di definire la quantità di memoria (in MB) di riserva per la cache CSV in ogni nodo del cluster. Ad esempio, se viene definito un valore pari a 512, 512 MB di memoria di sistema è riservato in ogni nodo. (Nella maggior parte dei cluster, 512 MB è un valore consigliato). L'impostazione predefinita è 0 (disabilitato).</td>
</tr>
<tr class="even">
<td><strong>EnableBlockCache</strong></td>
<td><strong>CsvEnableBlockCache</strong></td>
<td>Si tratta di una proprietà privata del cluster risorsa disco fisico. Consente di abilitare la cache CSV su un disco singolo viene aggiunto in CSV. In Windows Server 2012, l'impostazione predefinita è 0 (per la disattivazione). Per abilitare la cache CSV su un disco, configurare un valore pari a 1. Questa impostazione è attivata per impostazione predefinita, in Windows Server 2012 R2.</td>
</tr>
</tbody>
</table>

È possibile monitorare la cache CSV in Performance Monitor aggiungendo i contatori in **Cluster di Cache del Volume CSV**.

#### <a name="configure-the-csv-cache"></a>Configurare la cache CSV

1. Avvio di Windows PowerShell come amministratore.
2. Per definire una cache di *512* MB viene riservato in ogni nodo, digitare quanto segue:

    - Per Windows Server 2012 R2:

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - Per Windows Server 2012:

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. In Windows Server 2012 per abilitare la cache CSV in CSV denominato *Cluster disco 1*, immettere quanto segue:

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * In Windows Server 2012, è possibile allocare solo il 20% della RAM fisica totale per la cache CSV. In Windows Server 2012 R2, è possibile allocare fino all'80%. Poiché con scalabilità orizzontale File server non sono in genere memoria limitata, è possibile eseguire miglioramenti delle prestazioni di grandi dimensioni utilizzando la memoria aggiuntiva per la cache CSV.
> * Per evitare conflitti di risorse, è necessario riavviare ogni nodo del cluster dopo aver modificato la memoria allocata per la cache CSV. In Windows Server 2012 R2, riavviare il sistema non è più necessario.
> * Dopo aver attivato o disattivato cache CSV su un disco singolo, per l'impostazione abbia effetto, è necessario disconnettere la risorsa disco fisico e riportare in linea. (Per impostazione predefinita, in Windows Server 2012 R2, la cache CSV è attivata). 
> * Per ulteriori informazioni sulla cache CSV contenente informazioni sui contatori delle prestazioni, vedere il post di blog [come attiva Cache CSV](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/).

## <a name="back-up-csv"></a>Eseguire il backup CSV

Sono disponibili diversi metodi per eseguire il backup di informazioni archiviate in CSV in un cluster di failover. È possibile utilizzare un'applicazione di backup Microsoft o di un'applicazione. In generale, CSV non impongono particolari requisiti di backup oltre a quelle per l'archiviazione non in pila formattata con file system NTFS o ReFS. I backup CSV anche non interferire con altre operazioni di archiviazione CSV.

Quando si seleziona un'applicazione di backup e backup per CSV, è necessario considerare i fattori seguenti:

- Backup a livello di volume di un volume CSV può essere eseguito da qualsiasi nodo che si connette a volume CSV.
- L'applicazione di backup può utilizzare software o hardware. A seconda della possibilità per l'applicazione di backup per supportarli, i backup possono utilizzare gli snapshot di servizio Copia Shadow del Volume (VSS) applicazione coerente e coerenti arresto anomalo del sistema.
- Se si esegue il backup CSV contenenti più macchine virtuali in esecuzione, è consigliabile in genere scegliere un metodo di backup gestione basate sul sistema operativo. Se nell'applicazione di backup supportato dal, più macchine virtuali è possibile eseguire il backup contemporaneamente.
- CSV supporta backup richiedente che eseguono Windows Server 2012 R2 Backup, Backup di Windows Server 2012 o Windows Server 2008 R2 Backup. Tuttavia, Windows Server Backup in genere sono disponibili solo una base soluzione di backup che non può essere adeguata per le organizzazioni con i gruppi di dimensioni maggiori. Windows Server Backup non supporta il backup dell'applicazione coerente macchina virtuale in CSV. Supporta solo backup a livello di volume coerente arresto anomalo del sistema. (Se si ripristina un backup di arresto anomalo coerente, la macchina virtuale sarà nello stesso stato che era se la macchina virtuale con un arresto anomalo al momento giusto è stato eseguito il backup.) Un backup di una macchina virtuale in un volume CSV avrà esito positivo, ma viene registrato un evento di errore che indica che questo non è supportato.
- Potrebbe essere necessario delle credenziali amministrative per il backup di un cluster di failover.

>[!IMPORTANT]
>Assicurarsi di leggere con attenzione i dati nell'applicazione di backup esegue il backup e ripristina, le caratteristiche CSV supporta, e le risorse necessarie per l'applicazione in ogni nodo cluster.

>[!WARNING]
>Se si desidera ripristinare i dati di backup su un volume CSV, tenere presente le funzionalità e le limitazioni di applicazione di backup per gestire e ripristinare i dati dell'applicazione coerente tra i nodi del cluster. Ad esempio, con alcune applicazioni, se il file CSV viene ripristinato in un nodo diverso dal nodo di cui è stato eseguito il volume CSV verso l'alto, potrebbe essere inavvertitamente verranno sovrascritti importanti dati sullo stato dell'applicazione nel nodo a cui il ripristino è in corso.

## <a name="more-information"></a>Ulteriori informazioni

- [Clustering di failover](failover-clustering.md)
- [Distribuire spazi di archiviazione in cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)