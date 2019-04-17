---
title: Requisiti hardware clustering di failover e le opzioni di archiviazione
description: Requisiti hardware e le opzioni di archiviazione per la creazione di un cluster di failover.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4706372b06d0554196b692c3ddcda145dee5bae5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082291"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Requisiti hardware clustering di failover e le opzioni di archiviazione

Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

È necessario l'hardware per creare un cluster di failover. Per essere supportata da Microsoft, deve essere certificate tutti i componenti hardware per la versione di Windows Server in esecuzione e la soluzione di cluster di failover completo deve superare tutti i test in Convalida configurazione guidata. Per ulteriori informazioni sulla convalida di un cluster di failover, vedere [Convalida Hardware per un Cluster di Failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Server**: È consigliabile utilizzare un insieme di computer che contengono componenti uguali o simili.
- **Schede di rete e cavo (per le comunicazioni di rete)**: se si usa iSCSI, ogni scheda di rete deve essere dedicato per le comunicazioni di rete o iSCSI, non entrambi.

    Nell'infrastruttura di rete che connette i nodi del cluster, evitare di singoli punti di errore. Ad esempio, è possibile connettere i nodi del cluster utilizzando più reti distinte. In alternativa, è possibile connettere i nodi del cluster con una rete che viene creata con schede di rete ridondanti, commutatori ridondanti, router ridondanti o hardware simile che rimuove i singoli punti di errore.

    >[!NOTE]
    >Se ci si connettono i nodi del cluster con un'unica rete, la rete passerà i requisiti di ridondanza in Convalida configurazione guidata. Tuttavia, un avviso indicante che la rete non dovrebbe includere singoli punti di errore da includere nel report dalla procedura guidata.

- **Controller di dispositivi o schede appropriate per l'archiviazione**:

  - **Serial Attached SCSI o Fibre Channel**: se si utilizza Serial Attached SCSI o Fibre Channel, in tutti i cluster di server, tutti gli elementi dello stack di archiviazione devono essere identici. È necessario che il software dei / o (MPIO) più percorsi siano identiche e che il software specifico modulo DSM (Device) essere identici. È consigliabile che i controller di periferica di memorizzazione, ovvero l'host del bus scheda (HBA), driver HBA e firmware HBA, che devono essere collegati alla archiviazione cluster essere identici. Se si utilizzano HBA diversi, è consigliabile verificare con il fornitore di archiviazione che si siano seguendo le configurazioni supportate o consigliati.
  - **iSCSI**: se si utilizza iSCSI, ogni server del cluster deve disporre di uno o più schede di rete o HBA dedicati all'archiviazione cluster. La rete utilizzata per iSCSI non deve essere utilizzata per le comunicazioni di rete. Nel server di tutti i cluster, le schede di rete utilizzate per la connessione alla destinazione di archiviazione iSCSI devono essere identiche ed è consigliabile utilizzare Gigabit Ethernet o versione successiva.
- **Archiviazione**: È necessario utilizzare [Spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) o condivisi archiviazione compatibile con Windows Server 2012 R2 o Windows Server 2012. È possibile utilizzare un'archiviazione condivisa collegato ed è inoltre possibile utilizzare le condivisioni di file SMB 3.0 come un'archiviazione condivisa per i server che esegue Hyper-V configurate in un cluster di failover. Per ulteriori informazioni, vedere [Distribuzione di Hyper-V su SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    Nella maggior parte dei casi, archiviazione collegate deve contenere più, separare i dischi (numeri di unità logica o LUN) che vengono configurati a livello hardware. Per alcuni cluster, un disco funziona come server di controllo del disco (descritto alla fine della presente sottosezione). Altri dischi contengono i file necessari per i ruoli del cluster (precedentemente denominati applicazioni o servizi del cluster). Requisiti di archiviazione includono:

  - Per utilizzare il supporto nativo disco incluso nel Clustering di Failover, utilizzare dischi di base, i dischi non dinamici.
  - È consigliabile formattare le partizioni con NTFS. Se si utilizza Cluster condiviso volumi (con estensione CSV), la partizione per ognuna di esse deve essere NTFS.

    >[!NOTE]
    >Se si dispone di un controllo su disco per la configurazione di quorum, è possibile formattare il disco con NTFS o resiliente File System (ReFS).

  - Per lo stile di partizione del disco, è possibile utilizzare record avvio principale () o tabella di partizione GUID (GPT).

    Un controllo del mirroring del disco è un disco nell'archivio cluster che ha designato per mantenere una copia del database di configurazione del cluster. Un cluster di failover è un'istanza di controllo su disco solo se viene specificato come parte della configurazione del quorum. Per ulteriori informazioni, vedere [Understanding Quorum in spazi di archiviazione diretta](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Requisiti hardware per Hyper-V

Se si sta creando un cluster di failover che include le macchine virtuali in cluster, i server del cluster devono supportare i requisiti hardware per il ruolo Hyper-V. Hyper-V è necessario un processore a 64 bit che include le operazioni seguenti:

- Virtualizzazione basata su hardware. Questa opzione è disponibile nei processori che includono un'opzione di virtualizzazione, nello specifico i processori Intel Virtualization Technology (Intel VT) o tecnologia AMD Virtualization (AMD-V).
- Applicata da hardware esecuzione programmi (DEP, Data) deve essere disponibile e abilitata. In particolare, è necessario attivare Intel XD bit (execute disable bit) o AMD NX bit (no execute).

Per ulteriori informazioni sul ruolo Hyper-V, vedere [Panoramica di Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Distribuzione di reti di archiviazione con i cluster di failover

Quando si distribuisce una rete di archiviazione (SAN) con un cluster di failover, seguire queste linee guida:

- **Verificare la compatibilità dello spazio di archiviazione**: conferma con produttori e dai fornitori di archiviazione, inclusi i driver, firmware e software utilizzato per l'archiviazione, sono compatibili con i cluster di failover nella versione di Windows Server in esecuzione.
- **Isolare i dispositivi di archiviazione, un cluster per dispositivo**: server dal cluster diversi non deve essere in grado di accedere ai dispositivi di archiviazione stesso. Nella maggior parte dei casi, un LUN utilizzato da un insieme di server cluster deve essere isolato da tutti gli altri server tramite LUN per il mascheramento o zoning.
- **Si consiglia di utilizzare percorsi multipli i/o software o collaborato schede di rete**: In un'infrastruttura di archiviazione a disponibilità elevata, è possibile distribuire i cluster di failover con più schede bus host mediante un software dei / o più percorsi o raggruppamento delle schede (acronimo di carico di rete bilanciamento del carico e failover o LBFO). In tal modo il livello massimo di ridondanza e disponibilità. Per Windows Server 2012 R2 o Windows Server 2012, la soluzione più percorsi deve essere basata su Microsoft percorsi multipli i/o (MPIO). Il fornitore dell'hardware in genere fornirà un modulo specifici del dispositivo MPIO (DSM) per l'hardware, anche se Windows Server include uno o più DSM come parte del sistema operativo.

    Per ulteriori informazioni su LBFO, vedere [Panoramica della scheda di rete che si integra](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) nella raccolta di documentazione tecnica di Windows Server.

    >[!IMPORTANT]
    >Software dei / o più percorsi e delle schede bus host può essere molto versione riservati. Se si stanno implementando una soluzione più percorsi per il cluster, lavorano con il fornitore dell'hardware per scegliere le schede corrette, firmware e software per la versione di Windows Server in esecuzione.

- **Si consiglia di utilizzare spazi di archiviazione**: se si prevede di distribuire seriale archiviazione collegate SCSI (SAS) cluster configurato con spazi di archiviazione, vedere [Distribuire cluster spazi di archiviazione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) per i requisiti.

## <a name="more-information"></a>Ulteriori informazioni

- [Clustering di failover](failover-clustering.md)
- [Spazi di archiviazione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Utilizzo del clustering guest per la disponibilità elevata](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)