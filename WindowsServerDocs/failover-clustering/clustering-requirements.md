---
title: Requisiti hardware di clustering failover e opzioni di archiviazione
description: Requisiti hardware e le opzioni di archiviazione per la creazione di un cluster di failover.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: e6eb6f2acd420ae657a5c1b698e9733751378552
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476087"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Requisiti hardware di clustering failover e opzioni di archiviazione

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per creare un cluster di failover, è necessario disporre dell'hardware seguente. Per essere supportato da Microsoft, tutto l'hardware deve essere certificato per la versione di Windows Server in esecuzione e la soluzione completa del cluster di failover deve superare tutti i test della Convalida guidata configurazione. Per altre informazioni sulla convalida del cluster di failover, vedere [Convalidare l'hardware per un cluster di failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Server**: è consigliabile utilizzare un insieme di computer corrispondenti che contengono gli stessi componenti o componenti simili.
- **Cavo e adattatori di rete (per comunicazione di rete)** : In caso di uso di iSCSI, ogni scheda di rete deve essere dedicata alle comunicazioni di rete o a iSCSI, non a entrambi.

    Evitare singoli punti di errore nell'infrastruttura di rete utilizzata per la connessione dei nodi del cluster. È ad esempio possibile connettere i nodi del cluster con più reti distinte. In alternativa, è possibile connettere i nodi del cluster con una rete che viene costruita con schede di rete, commutatori ridondanti, router ridondanti o hardware simile che rimuova singoli punti di errore.

    >[!NOTE]
    >Se si connettono nodi cluster con una sola rete, la rete supererà il requisito di ridondanza nella Convalida configurazione guidata. Il rapporto della procedura guidata includerà tuttavia un avviso a indicare che la rete deve essere priva di singoli punti di errore.

- **Controller di dispositivi o adattatori appropriati per l'archiviazione**:

  - **SAS o Fibre Channel**: se si usa SAS o Fibre Channel, in tutti i server del cluster, è preferibile che tutti gli elementi dello stack di archiviazione siano identici. È necessario che il software multipath i/o (MPIO) siano identiche e che il software del modulo specifico dispositivo (DSM) siano identici. È consigliabile che i controller del dispositivo di archiviazione di massa, host bus adapter (HBA), i relativi driver e firmware, collegati all'archiviazione cluster siano identici. Se si utilizzano schede HBA diverse, è opportuno verificare con il fornitore dell'archiviazione che vengano eseguite le configurazioni supportate o consigliate.
  - **iSCSI**: se si usa iSCSI, ogni server del cluster deve disporre di una o più schede di rete o HBA dedicate all'archiviazione cluster. La rete utilizzata per i dispositivi iSCSI non deve essere utilizzata per la comunicazione di rete. In tutti i server del cluster, le schede di rete utilizzate per eseguire la connessione alla destinazione di archiviazione iSCSI devono essere identiche ed è consigliabile utilizzare Gigabit Ethernet o versione successiva.
- **Archiviazione**: È necessario utilizzare [spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) o condiviso di archiviazione compatibile con Windows Server 2012 R2 o Windows Server 2012. È possibile usare l'archiviazione condivisa associata ed è anche possibile usare condivisioni file SMB 3.0 come archiviazione condivisa per i server che eseguono Hyper-V configurati in un cluster di failover. Per altre informazioni, vedere [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    Nella maggior parte dei casi, l'archiviazione collegata deve contenere più dischi separati (LUN, Logical Unit Number) configurati a livello hardware. Per alcuni cluster, un disco viene utilizzato come disco di controllo (descritto alla fine di questa sottosezione). Altri dischi contengono i file necessari per i ruoli del cluster (in precedenza denominati servizi o applicazioni del cluster). Di seguito sono elencati i requisiti di archiviazione:

  - Per usare il supporto del disco nativo incluso nel clustering di failover, usare dischi di base, non dinamici.
  - È consigliabile formattare le partizioni con NTFS. Se si usano volumi condivisi (CSV,Cluster Shared Volumes) del cluster, la partizione per ogni disco deve essere NTFS.

    >[!NOTE]
    >Se si usa un disco di controllo per la configurazione quorum, sarà possibile formattare il disco con NTFS o ReFS (Resilient File System).

  - È possibile utilizzare MBR (Record di avvio principale, Master Boot Record) o GPT (Tabella di partizione GUID, GUID Partition Table) come stile di partizione del disco.

    Un disco di controllo è un disco nell'archiviazione del cluster che è progettato per contenere una copia del database di configurazione del cluster. Un cluster di failover dispone di un disco di controllo solo se tale funzionalità è stata specificata durante la configurazione quorum. Per altre informazioni, vedere [Quorum comprensione in spazi di archiviazione diretta](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Requisiti hardware per Hyper-V

Se si crea un cluster di failover che include macchine virtuali in cluster, è necessario che i server del cluster supportino i requisiti hardware per il ruolo Hyper-V. Hyper-V richiede un processore a 64 bit che include quanto indicato di seguito:

- Virtualizzazione assistita mediante hardware. Disponibile nei processori che includono un'opzione di virtualizzazione, in particolare i processori con tecnologia di virtualizzazione Intel (Intel VT) o la virtualizzazione AMD (AMD-V).
- È necessario rendere disponibile e abilitare Protezione esecuzione programmi applicata a livello di hardware. In particolare, è necessario abilitare Intel XD Bit (eXecute Disable Bit) o AMD NX Bit (No eXecute Bit).

Per altre informazioni sul ruolo Hyper-V, vedere [Panoramica di Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Distribuzione di reti SAN con cluster di failover

In fase di distribuzione di una rete SAN con un cluster di failover, attenersi alle linee guida seguenti:

- **Confermare la compatibilità della soluzione di archiviazione**: verificare con i produttori e i fornitori che la soluzione di archiviazione, inclusi driver, firmware e software utilizzati per l'archiviazione, siano compatibili con i cluster di failover nella versione di Windows Server in esecuzione.
- **Isolare i dispositivi di archiviazione, un cluster per dispositivo**: i server di cluster diversi non devono essere in grado di accedere agli stessi dispositivi di archiviazione. Nella maggior parte dei casi, è consigliabile isolare un LUN utilizzato da un insieme di server del cluster da tutti gli altri server mediante il mascheramento dei LUN o la suddivisione in zone.
- **Valutare l'uso di software Multipath I/O o di gruppi di schede di rete**: in un fabric di archiviazione a disponibilità elevata è possibile distribuire cluster di failover con più schede bus host usando software Multipath I/O o un gruppo di schede di rete (detto anche bilanciamento del carico e failover). In questo modo viene fornito il livello più elevato di ridondanza e disponibilità. Per Windows Server 2012 R2 o Windows Server 2012, la soluzione a percorsi multipli deve essere basata su Microsoft Multipath i/o (MPIO). Il fornitore dell'hardware offre in genere un modulo DSM MPIO per l'hardware, sebbene Windows Server includa uno o più moduli DSM all'interno del sistema operativo.

    Per altre informazioni sulle LBFO, vedere [Panoramica di gruppo NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) nella libreria tecnica di Windows Server.

    >[!IMPORTANT]
    >Le schede HBA e il software Multipath I/O possono variare significativamente in base alle versioni. In caso di implementazione di una soluzione a percorsi multipli per il cluster, è opportuno scegliere attentamente con il fornitore dell'hardware le schede, il firmware e il software corretti per la versione di Windows Server in esecuzione.

- **Valutare l'uso di Spazi di archiviazione**: Se si prevede di distribuire serie collegata di archiviazione SCSI (SAS) in cluster configurata in modo usando spazi di archiviazione, vedere [distribuire spazi di archiviazione in cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) per soddisfare i requisiti.

## <a name="more-information"></a>Altre informazioni

- [Clustering di failover](failover-clustering.md)
- [Spazi di archiviazione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Uso di cluster Guest per la disponibilità elevata](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)