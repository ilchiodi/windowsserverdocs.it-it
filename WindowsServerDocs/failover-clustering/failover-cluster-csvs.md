---
title: Usare volumi condivisi Cluster in un cluster di failover
description: Come usare volumi condivisi del Cluster in un cluster di failover.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 00f29c70628f2869e9f3aeffd0d08032bce5aeda
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034186"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>Usare volumi condivisi Cluster in un cluster di failover

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

I volumi condivisi cluster (CSV, Cluster Shared Volumes) permettono a più nodi in un cluster di failover di accedere contemporaneamente in lettura/scrittura allo stesso LUN (disco) di cui viene eseguito il provisioning come volume NTFS. (In Windows Server 2012 R2, il disco possa essere sottoposti a provisioning come NTFS o Resilient File System (ReFS).) Grazie al volume CSV, è possibile eseguire rapidamente il failover dei ruoli del cluster in un altro nodo, senza dover modificare la proprietà dell'unità o smontare e rimontare un volume. CSV semplifica anche la gestione di un numero potenzialmente elevato di LUN in un cluster di failover.

Volume CSV offre un sistema generale e in cluster di file, che rappresenta un livello superiore (NTFS o ReFS in Windows Server 2012 R2). Le applicazioni CSV includono:

- File di disco rigido virtuale (VHD, Virtual Hard Disk) in cluster per macchine virtuali Hyper-V
- Scalabilità orizzontale di condivisioni di file per l'archiviazione di dati applicazione per il ruolo del cluster File server di scalabilità orizzontale. Esempi dei dati applicazione per questo ruolo includono i file di VM Hyper-V e i dati di Microsoft SQL Server. Si noti che ReFS non è supportato per un File server di scalabilità orizzontale. Per altre informazioni sui File Server di scalabilità orizzontale, vedere [Scale-Out File Server per i dati dell'applicazione](sofs-overview.md).

>[!NOTE]
>In CSV non è supportato il carico di lavoro cluster di Microsoft SQL Server in SQL Server 2012 e in versioni precedenti di SQL Server.

Funzionalità del volume CSV è stata notevolmente migliorata in Windows Server 2012. Sono state ad esempio rimosse le dipendenze da Servizi di dominio Active Directory. È stato aggiunto supporto per i miglioramenti funzionali in **chkdsk**, per l'interoperabilità con applicazioni antivirus e di backup e per l'integrazione con funzionalità generali di archiviazione, quali i volumi con crittografia BitLocker e Spazi di archiviazione. Per una panoramica delle funzionalità CSV introdotte in Windows Server 2012, vedere [What ' s New in Failover Clustering in Windows Server 2012 \[reindirizzato\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

Windows Server 2012 R2 introduce funzionalità aggiuntive, ad esempio la proprietà CSV distribuita, un incremento di resilienza tramite la disponibilità del servizio Server, una maggiore flessibilità nella quantità di memoria fisica allocabile alla cache CSV, meglio funzione diagnostica e interoperabilità ottimizzata che include il supporto per ReFS e deduplicazione. Per altre informazioni, vedere [What ' s New in Failover Clustering](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

>[!NOTE]
>Per informazioni sull'uso della deduplicazione dati in CSV per scenari VDI (Virtual Desktop Infrastructure), vedere i post di blog sulla [distribuzione della deduplicazione dati per l'archiviazione VDI in Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) e su come [estendere la deduplicazione dati a nuovi carichi di lavoro in Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx).

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>Verificare i requisiti e le considerazioni per l'uso di CSV in un cluster di failover

Prima di usare un volume CSV in un cluster di failover, verificare i requisiti e le considerazioni per rete, archiviazione e di altro tipo disponibili in questa sezione.

### <a name="network-configuration-considerations"></a>Considerazioni sulla configurazione di rete

Quando si configurano le reti che supportano CSV, prendere in considerazione gli elementi seguenti.

- **Più reti e più schede di rete**. Per abilitare la tolleranza di errore in caso di errore di rete, è consigliabile fare in modo che più reti di cluster trasportino il traffico CSV oppure configurare gruppi di schede di rete.
    
    Se i nodi del cluster sono connessi a reti che non devono essere usate dal cluster, è consigliabile disabilitarli. È ad esempio consigliabile disabilitare l'uso delle reti iSCSI da parte del cluster, in modo da impedire il traffico CSV in tali reti. Per disabilitare una rete, in Gestione Cluster di Failover, selezionare **reti**, selezionare la rete, selezionare la **delle proprietà** azione e quindi selezionare **non consentire comunicazioni di rete cluster in Questa rete**. In alternativa, è possibile configurare il **ruolo** proprietà di rete usando la [Get-ClusterNetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) cmdlet di Windows PowerShell.
- **Proprietà delle schede di rete**. Nelle proprietà per tutte le schede che trasportano le comunicazioni del cluster verificare che siano abilitate le impostazioni seguenti:

  - **Client per reti Microsoft** e **Condivisione file e stampanti per reti Microsoft**. Queste impostazioni supportano Server Message Block (SMB) 3.0, usato per impostazione predefinita per il trasporto del traffico CSV tra i nodi. Per abilitare SMB, assicurarsi anche che il servizio Server e il servizio Workstation siano in esecuzione e che siano configurati per l'avvio automatico in ogni nodo del cluster.

    >[!NOTE]
    >In Windows Server 2012 R2, sono presenti più istanze del servizio Server per ogni nodo del cluster di failover. Il traffico in arrivo dai client SMB che accedono alle condivisioni file normali è gestito dall'istanza predefinita e una seconda istanza CSV gestisce solo il traffico CSV tra i nodi. Se il servizio Server in un nodo risulta non integro, la proprietà del volume CSV sarà trasferita automaticamente a un altro nodo.

    In SMB 3.0 sono incluse le funzionalità SMB multicanale e SMB diretto, che permettono il flusso del traffico dei volumi condivisi del cluster su più reti nel cluster e l'uso delle schede di rete che supportano Accesso diretto a memoria remota (RDMA). Per impostazione predefinita, SMB multicanale è usato per il traffico CSV. Per altre informazioni, vedere [Panoramica di SMB (Server Message Block)](../storage/file-server/file-server-smb-overview.md).
  - **Microsoft Failover Cluster Virtual Adapter Performance Filter**. Questa impostazione permette di migliorare la capacità dei nodi di eseguire il reindirizzamento I/O quando necessario per raggiungere il volume CSV, ad esempio quando un errore di connettività impedisce la connessione diretta di un nodo al disco CSV. Per altre informazioni, vedere [sincronizzazione About i/o e reindirizzamento i/o nelle comunicazioni CSV](#about-io-synchronization-and-io-redirection-in-csv-communication) più avanti in questo argomento.
- **Definizione delle priorità delle reti del cluster**. È in genere consigliabile non modificare le preferenze configurate dal cluster per le reti.
- **Configurazione della subnet IP**. Per i nodi in una rete che usa CSV non è richiesta alcuna configurazione specifica della subnet. Nel volume CSV sono supportati cluster con più subnet.
- **Qualità del servizio (QoS) basata su criteri**. Quando si usa CSV, è consigliabile configurare un criterio di priorità QoS e un criterio minimo per la larghezza di banda per il traffico di rete in ogni nodo. Per altre informazioni, vedere [Quality of Service (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>).
- **Rete di archiviazione**. Per consigli sulla rete usata per l'archiviazione, esaminare le indicazioni rese disponibili dal fornitore del sistema di archiviazione. Per altre considerazioni sull'archiviazione per CSV, vedere [requisiti di configurazione di archiviazione e disco](#storage-and-disk-configuration-requirements) più avanti in questo argomento.

Per una panoramica dei requisiti hardware, di rete e di archiviazione per i cluster di failover, vedere [Failover Clustering Hardware Requirements and Storage Options](clustering-requirements.md).

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>Informazioni sulla sincronizzazione I/O e sulla modalità I/O reindirizzata nelle comunicazioni CSV

- **Sincronizzazione i/o**: TALE funzionalità consente a più nodi di accedere contemporaneamente in lettura / scrittura alla stessa archiviazione condivisa. Quando un nodo esegue operazioni di input/output (I/O) del disco in un volume CSV, il nodo comunica direttamente con l'archiviazione, ad esempio tramite una rete di archiviazione (SAN, Storage Area Network). Tuttavia, in qualsiasi momento, un singolo nodo, definito nodo coordinatore, è "proprietario" della risorsa Disco fisico associata al LUN. Il nodo coordinatore per un volume CSV è visualizzato in Gestione cluster di failover come **Nodo proprietario** in **Dischi**. Viene inoltre visualizzato nell'output del [Get-ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) cmdlet di Windows PowerShell.

  >[!NOTE]
  >In Windows Server 2012 R2, la proprietà CSV è distribuita uniformemente tra i nodi del cluster di failover in base al numero di volumi CSV di cui è proprietario ogni nodo. La proprietà viene anche ribilanciata automaticamente in caso di condizioni quali il failover del volume CSV, il reinserimento di un nodo nel cluster, l'aggiunta di un nuovo nodo al cluster, il riavvio di un nodo del cluster o l'avvio del cluster di failover dopo l'arresto.

  Quando si verificano determinate modifiche minori nel file system in un volume CSV, questi metadati devono essere sincronizzati in ogni nodo fisico che accede a LUN, non solo nel singolo nodo coordinatore. Ad esempio, quando una macchina virtuale in un volume CSV viene avviata, creata o eliminata o quando si esegue la migrazione di una VM, queste informazioni devono essere sincronizzate in ogni nodo fisico che accede alla macchina virtuale. Queste operazioni di aggiornamento dei metadati sono eseguite in parallelo nelle reti di cluster tramite SMB 3.0. Per queste operazioni non è necessario che tutti i nodi fisici comunichino con l'archiviazione condivisa.

- **Reindirizzamento i/o**: Gli errori di connettività dell'archiviazione e alcune operazioni di archiviazione possono impedire a un nodo specifico di comunicare direttamente con l'archiviazione. Per mantenere il funzionamento durante l'interruzione delle comunicazioni del nodo con l'archiviazione, il nodo reindirizza le operazioni I/O del disco tramite una rete di cluster al nodo coordinatore, in cui è attualmente montato il disco. In caso di errore di connettività dell'archiviazione nel nodo coordinatore corrente, tutte le operazioni I/O del disco saranno accodate temporaneamente, mentre si definisce un nuovo nodo come nodo coordinatore.

Il server usa una delle modalità di reindirizzamento I/O seguenti, in base alla situazione:

- **Reindirizzamento del file system** Il reindirizzamento è relativo ai singoli volumi, ad esempio in caso di creazione di snapshot del volume CSV da parte di un'applicazione di backup quando a un volume CSV viene applicata manualmente la modalità I/O reindirizzato.
- **Reindirizzamento blocchi** Il reindirizzamento è eseguito a livello di blocco di file, ad esempio in caso di perdita della connettività dell'archiviazione per un volume. Il reindirizzamento blocchi è significativamente più veloce rispetto al reindirizzamento del file system.

In Windows Server 2012 R2, è possibile visualizzare lo stato di un volume CSV in base al nodo. Ad esempio, è possibile verificare se I/O è diretto o reindirizzato o se il volume CSV non è disponibile. Se un volume CSV è in modalità I/O reindirizzato, sarà visualizzato anche il motivo. Usare il cmdlet **Get-ClusterSharedVolumeState** di Windows PowerShell per visualizzare queste informazioni.

>[!NOTE]
> * In Windows Server 2012, grazie a miglioramenti nella progettazione di CSV, i volumi CSV eseguono più operazioni in modalità i/o diretta rispetto a Windows Server 2008 R2 durante l'esecuzione.
> * A causa dell'integrazione di CSV con funzionalità di SMB 3.0 quali SMB multicanale e SMB diretto, è possibile eseguire lo streaming del traffico I/O reindirizzato in più reti di cluster.
> * È consigliabile pianificare le reti di cluster in modo da permettere il potenziale incremento in traffico di rete verso il nodo coordinatore durante il reindirizzamento I/O.

### <a name="storage-and-disk-configuration-requirements"></a>Requisiti per l'archiviazione e la configurazione del disco

Per usare CSV, l'archiviazione e i dischi devono soddisfare i requisiti seguenti:

- **Formato del file system**. In Windows Server 2012 R2, uno spazio di archiviazione o del disco per un volume CSV deve essere un disco di base partizionato con NTFS o ReFS. In Windows Server 2012, uno spazio di archiviazione o del disco per un volume CSV deve essere un disco di base partizionato con NTFS.

  Per un volume CSV è necessario soddisfare i requisiti aggiuntivi seguenti:
    
  - In Windows Server 2012 R2, è possibile usare un disco per un volume CSV formattato con FAT o FAT32.
  - In Windows Server 2012, è possibile usare un disco per un volume CSV formattato con FAT, FAT32 o ReFS.
  - Se si vuole usare uno spazio di archiviazione per un volume CSV, è possibile configurare uno spazio semplice o uno spazio con mirroring. In Windows Server 2012 R2, è anche possibile configurare uno spazio di parità. (In Windows Server 2012, CSV non supporta spazi parità.)
  - Non è possibile usare un volume CSV come disco di controllo del quorum. Per altre informazioni sul quorum del cluster, vedere [Quorum comprensione in spazi di archiviazione diretta](../storage/storage-spaces/understand-quorum.md).
  - Dopo l'aggiunta di un disco come volume CSV, il disco sarà designato nel formato CSVFS (CSV File System). Ciò permette al cluster e ad altro software di differenziare l'archiviazione CSV da altra archiviazione NTFS o ReFS. In genere, CSVFS supporta la stessa funzionalità come NTFS o ReFS. Alcune funzionalità, tuttavia, non sono supportate. Ad esempio, in Windows Server 2012 R2, è possibile abilitare la compressione su un volume CSV. In Windows Server 2012, è possibile abilitare la deduplicazione dei dati o la compressione su un volume CSV.
- **Tipo di risorsa nel cluster**. Per un volume CSV è necessario usare il tipo di risorsa Disco fisico. Per impostazione predefinita, un disco o uno spazio di archiviazione aggiunto a un'archiviazione cluster è configurato automaticamente in questo modo.
- **Scelta di dischi CSV o altri dischi nell'archiviazione cluster**. Quando si scelgono uno o più dischi per una macchina virtuale in cluster, è necessario valutare il modo in cui ogni disco sarà usato. Se un disco sarà usato per l'archiviazione di file creati da Hyper-V, ad esempio file VHD o file di configurazione, sarà possibile scegliere un disco CSV o uno degli altri dischi disponibili nell'archiviazione cluster. Se un disco sarà un disco fisico collegato direttamente alla macchina virtuale (definito anche disco pass-through), non sarà possibile scegliere un disco CSV e sarà necessario scegliere uno degli altri dischi disponibili nell'archiviazione cluster.
- **Nome del percorso per identificare i dischi**. I dischi in CSV sono identificati tramite un nome di percorso. Ogni percorso risulta essere nell'unità del sistema del nodo come volume numerato nella  **\\ClusterStorage** cartella. Questo percorso è lo stesso quando viene visualizzato da qualsiasi nodo nel cluster. Se necessario, è possibile rinominare i volumi.

Per i requisiti di archiviazione relativi a CSV, esaminare le indicazioni rese disponibili dal fornitore del sistema di archiviazione. Per considerazioni aggiuntive sull'archiviazione per CSV, vedere [Pianificare l'uso di CSV in un cluster di failover](#plan-to-use-csv-in-a-failover-cluster) più avanti in questo argomento.

### <a name="node-requirements"></a>Requisiti del nodo

Per usare CSV, i nodi devono soddisfare i requisiti seguenti:

- **Lettera di unità del disco di sistema**. La lettera di unità del disco di sistema deve essere la stessa per tutti i nodi.
- **Protocollo di autenticazione**. Il protocollo NTLM deve essere abilitato su tutti i nodi. L'impostazione è abilitata per impostazione predefinita.

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>Pianificare l'uso di CSV in un cluster di failover

Questa sezione elenca pianificazione considerazioni e consigli per l'uso di CSV in un cluster di failover che esegue Windows Server 2012 R2 o Windows Server 2012.

>[!IMPORTANT]
>Per indicazioni sulla configurazione dell'unità di archiviazione specifica in uso per CSV, rivolgersi al fornitore del sistema di archiviazione. Se le indicazioni del fornitore del sistema di archiviazione sono diverse dalle informazioni disponibili in questo argomento, usare le indicazioni del fornitore.

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Disposizione di LUN, volumi e file VHD

Per usare CSV in modo ottimale per offrire archiviazione per VM in cluster, è utile esaminare la disposizione di LUN (dischi) quando si configurano i server fisici. Quando si configurano le macchine virtuali corrispondenti, è necessario provare a disporre i file VHD in modo analogo.

Prendere in considerazione un server fisico in cui organizzare i dischi e i file come segue:

- File di sistema, incluso un file di paging, in un disco fisico.
- File di dati in un altro disco fisico.

Per una VM in cluster equivalente, è consigliabile organizzare i volumi e i file in modo analogo:

- File di sistema, incluso un file di paging, in un file VHD in un volume CSV.
- File di dati in un file VHD in un altro volume CSV.

Se si aggiunge un'altra macchina virtuale, è consigliabile, se possibile, mantenere la stessa disposizione per i file VHD nella VM aggiuntiva.

### <a name="number-and-size-of-luns-and-volumes"></a>Numero e dimensioni di LUN e volumi

Quando si pianifica la configurazione dell'archiviazione per un cluster di failover che usa CSV, tenere in considerazione i suggerimenti seguenti:

- Per definire il numero di LUN da configurare, rivolgersi al fornitore del sistema di archiviazione. Ad esempio, è possibile che il fornitore del sistema di archiviazione suggerisca di configurare ogni LUN con una partizione in cui posizionare un volume CSV.
- Non sono previste limitazioni per il numero di macchine virtuali che possono essere supportate in un singolo volume CSV. È tuttavia consigliabile esaminare il numero di VM che si prevede di includere in un cluster e il carico di lavoro (operazioni I/O al secondo) per ogni macchina virtuale. Considera gli esempi che seguono:

  - Un'organizzazione distribuisce macchine virtuali che supporteranno un'infrastruttura VDI (Virtual Desktop Infrastructure), che costituisce un carico di lavoro relativamente leggero. Per il cluster si usa l'archiviazione a prestazioni elevate. L'amministratore del cluster, dopo avere consultato il fornitore del sistema di archiviazione, decide di posizionare un numero relativamente elevato di VM per ogni volume CSV.
  - Un'altra organizzazione distribuisce un numero elevato di macchine virtuali che supporteranno un'applicazione database molto usata, che rappresenta un carico di lavoro più pesante. Nel cluster si usa l'archiviazione a prestazioni inferiori. L'amministratore del cluster, dopo avere consultato il fornitore del sistema di archiviazione, decide di posizionare un numero relativamente ridotto di VM per ogni volume CSV.
- Quando si pianifica la configurazione dell'archiviazione per una macchina virtuale specifica, è necessario tenere in considerazione i requisiti del disco del servizio, dell'applicazione o del ruolo che sarà supportato dalla macchina virtuale. La verifica di questi requisiti permette di evitare il conflitto tra dischi, che potrebbe influire negativamente sulle prestazioni. È consigliabile che la configurazione di archiviazione per la macchina virtuale sia il più possibile analoga alla configurazione di archiviazione che sarebbe usata per un server fisico che esegue lo stesso servizio, la stessa applicazione o lo stesso ruolo. Per altre informazioni, vedere [disposizione di LUN, volumi e VHD file](#arrangement-of-luns-volumes-and-vhd-files) più indietro in questo argomento.

    È anche possibile attenuare il conflitto tra dischi configurando l'archiviazione con un numero elevato di dischi rigidi fisici indipendenti. Scegliere l'hardware di archiviazione di conseguenza e rivolgersi al fornitore per ottimizzare le prestazioni del sistema di archiviazione.
- In base ai carichi di lavoro del cluster e alla rispettiva necessità di operazioni I/O, è possibile prendere in considerazione la configurazione solo di una percentuale di VM per l'accesso a ogni LUN, mentre le altre macchine virtuali non dispongono di connettività e sono invece dedicate alle operazioni di calcolo.

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>Aggiungere un disco a CSV in un cluster di failover

La funzionalità CSV è abilitata per impostazione predefinita nel Clustering di failover. Per aggiungere un disco a CSV, è necessario aggiungere un disco al gruppo **Archiviazione disponibile** del cluster, se non è già stato aggiunto, quindi aggiungere il disco a CSV nel cluster. È possibile utilizzare Gestione Cluster di Failover o i cmdlet di PowerShell di Windows Failover cluster per eseguire queste procedure.

### <a name="add-a-disk-to-available-storage"></a>Aggiungere un disco in archiviazione disponibile

1. In Gestione cluster di failover espandere il nome del cluster nell'albero della console e quindi espandere **Archiviazione**.
2. Fare doppio clic su **Disks**, quindi selezionare **Aggiungi disco**. Nell'elenco visualizzato sono inclusi i dischi che è possibile aggiungere per l'uso in un cluster di failover.
3. Selezionare il disco o i dischi da aggiungere e quindi selezionare **OK**.

    I dischi sono ora assegnati al gruppo **Archiviazione disponibile** .

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Comandi equivalenti di Windows PowerShell (aggiungere un disco in archiviazione disponibile)

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.

Nell'esempio seguente vengono identificati i dischi pronti per l'aggiunta al cluster e i dischi vengono aggiunti al gruppo **Archiviazione disponibile**.

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>Aggiungere un disco in archiviazione disponibile in volumi condivisi cluster

1. In Gestione Cluster di Failover, nell'albero della console, espandere il nome del cluster, espandere **memorizzazione**, quindi selezionare **dischi**.
2. Selezionare uno o più dischi assegnati al **spazio di archiviazione disponibile**, fare doppio clic la selezione e quindi selezionare **aggiungere ai volumi condivisi Cluster**.

    I dischi sono ora assegnati al gruppo **Volume condiviso cluster** nel cluster. I dischi sono esposti in ogni nodo del cluster come volumi numerati (punti di montaggio) nella cartella %SystemDisk%ClusterStorage. I volumi sono visualizzati nel file system CSVFS.

>[!NOTE]
>È possibile rinominare i volumi CSV nella cartella %SystemDisk%ClusterStorage.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Comandi equivalenti di Windows PowerShell (aggiungere un disco a CSV)

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.

Nell'esempio seguente si aggiunge il *Cluster Disk 1* in **Archiviazione disponibile** a CSV nel cluster locale.

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>Abilitare la cache CSV per carichi di lavoro a utilizzo intensivo di operazioni di lettura (facoltativo)

La cache CSV offre la memorizzazione nella cache a livello di blocco di operazioni I/O senza buffer di sola lettura tramite l'allocazione di memoria di sistema (RAM) come cache write-through. Le operazioni I/O senza buffer non sono memorizzate nella cache da Gestione cache. Ciò può migliorare le prestazioni per applicazioni quali Hyper-V, che esegue operazioni I/O senza buffer durante l'accesso a VHD. Grazie alla cache CSV è possibile migliorare le prestazioni delle richieste di lettura senza memorizzare nella cache le richieste di scrittura. L'abilitazione della cache CSV è utile anche per scenari di file server di scalabilità orizzontale.

>[!NOTE]
>È consigliabile abilitare la cache CSV per tutte le distribuzioni Hyper-V e di file server di scalabilità orizzontale.

Per impostazione predefinita in Windows Server 2012, la cache CSV è disabilitata. In Windows Server 2012 R2, la cache CSV è abilitata per impostazione predefinita. È tuttavia necessario allocare comunque le dimensioni della cache a blocchi da riservare.

La tabella seguente descrive le due impostazioni di configurazione che controllano la cache CSV.

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
<td>Si tratta di una proprietà comune del cluster che permette di definire la quantità di memoria (in megabyte) da riservare per la cache CSV in ogni nodo nel cluster. Ad esempio, se si definisce un valore pari a 512, in ogni nodo saranno riservati 512 MB di memoria di sistema. (In molti cluster 512 MB è un valore consigliato). L'impostazione predefinita è 0 (disabilitata).</td>
</tr>
<tr class="even">
<td><strong>EnableBlockCache</strong></td>
<td><strong>CsvEnableBlockCache</strong></td>
<td>Si tratta di una proprietà privata della risorsa Disco fisico del cluster. Permette di abilitare la cache CSV in un singolo disco aggiunto a CSV. In Windows Server 2012, l'impostazione predefinita è 0 (disabilitata). Per abilitare la cache CSV su un disco, configurare un valore pari a 1. Per impostazione predefinita, in Windows Server 2012 R2, questa impostazione è abilitata.</td>
</tr>
</tbody>
</table>

È possibile monitorare la cache CSV in Monitoraggio delle prestazioni aggiungendo i contatori in **Cache del volume condiviso del cluster**.

#### <a name="configure-the-csv-cache"></a>Configurare la cache CSV

1. Avviare Windows PowerShell come amministratore.
2. Per definire una cache di *512* MB da riservare in ogni nodo, digitare quanto segue:

    - Per Windows Server 2012 R2:

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - Per Windows Server 2012:

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. In Windows Server 2012, per abilitare la cache CSV in un volume CSV denominato *Cluster Disk 1*, immettere quanto segue:

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * In Windows Server 2012, è possibile allocare solo il 20% della RAM fisica totale alla cache CSV. In Windows Server 2012 R2, è possibile allocare fino all'80%. Poiché per i server di scalabilità orizzontale non sono in genere previsti vincoli di memoria, è possibile ottenere notevoli vantaggi a livelli di prestazioni usando la memoria extra per la cache CSV.
> * Per evitare conflitti di risorse, è consigliabile riavviare ogni nodo del cluster dopo aver modificato la memoria allocata alla cache CSV. In Windows Server 2012 R2, è più necessario un riavvio.
> * Dopo l'abilitazione o disabilitazione della cache CSV in un singolo disco, per applicare l'impostazione sarà necessario portare offline la risorsa Disco fisico e quindi riportarla online. (Per impostazione predefinita, in Windows Server 2012 R2, la cache CSV è abilitata.) 
> * Per altre informazioni sulla cache CSV e sui contatori delle prestazioni, vedere il post di blog su [come abilitare la cache CSV](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/).

## <a name="back-up-csv"></a>Eseguire il backup del volume CSV

È possibile eseguire in molti modi il backup delle informazioni archiviate in CSV in un cluster di failover. Si può usare un'applicazione di backup Microsoft oppure un'applicazione non Microsoft. In generale, per CSV non sono previsti requisiti speciali di backup oltre a quelli relativi all'archiviazione in cluster formattata con NTFS o ReFS. I backup di CSV non influiscono inoltre sulle altre operazioni di archiviazione CSV.

Quando si seleziona un'applicazione di backup e una pianificazione di backup per CSV, è consigliabile tenere in considerazione i fattori seguenti:

- Il backup a livello di volume di un volume CSV può essere eseguito da qualsiasi nodo che si connette al volume CSV.
- L'applicazione di backup può usare snapshot software o snapshot hardware. In base alla capacità dell'applicazione di backup di supportarli, per i backup è possibile usare snapshot del Servizio Copia Shadow del volume coerenti con l'applicazione e con gli arresti anomali.
- Se si esegue un backup di un volume CSV in cui sono in esecuzione più macchine virtuali, è in genere consigliabile scegliere un metodo di gestione del backup basato su sistema operativo. Se questa operazione è supportata dall'applicazione di backup, è possibile eseguire contemporaneamente il backup di più macchine virtuali.
- CSV supporta i richiedenti di backup che eseguono Windows Server 2012 R2 Backup, Backup di Windows Server 2012 o Windows Server 2008 R2 Backup. In Windows Server Backup è in genere disponibile solo una soluzione di backup di base che potrebbe non essere adatta per organizzazioni con cluster di grandi dimensioni. Il backup su CSV di macchine virtuali coerenti con l'applicazione non è supportato in Windows Server Backup. È supportato solo il backup a livello di volume coerente con gli arresti anomali. Se si ripristina un backup coerente con gli arresti anomali, lo stato della macchina virtuale sarà uguale a quello di una macchina virtuale arrestata in modo anomalo nel momento in cui è stato eseguito il backup. Il backup di una macchina virtuale in un volume CSV avrà esito positivo, ma sarà registrato un evento di errore per indicare che questa operazione non è supportata.
- Quando si esegue il backup di un cluster di failover potrebbero essere necessarie credenziali di amministratore.

>[!IMPORTANT]
>Assicurarsi di esaminare con attenzione i dati di cui l'applicazione di backup esegue il backup e il ripristino, le funzionalità CSV supportate e i requisiti di risorse per l'applicazione in ogni nodo del cluster.

>[!WARNING]
>Se è necessario ripristinare i dati di backup in un volume CSV, prestare attenzione alle capacità e alle limitazioni dell'applicazione di backup in merito al mantenere e ripristinare i dati coerenti con l'applicazione nei nodi del cluster. In alcune applicazioni, ad esempio, se si ripristina CSV in un nodo diverso dal nodo in cui è stato eseguito il backup del volume CSV, è possibile che siano accidentalmente sovrascritti dati importanti sullo stato dell'applicazione nel nodo in cui si sta eseguendo il ripristino.

## <a name="more-information"></a>Altre informazioni

- [Clustering di failover](failover-clustering.md)
- [Distribuire spazi di archiviazione in cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)