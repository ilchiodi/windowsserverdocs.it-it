---
title: Requisiti hardware per il clustering di failover e opzioni di archiviazione
description: Requisiti hardware e opzioni di archiviazione per la creazione di un cluster di failover.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: dec242282b0d7a07b8f244a1044134bd8af03f6f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827854"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Requisiti hardware per il clustering di failover e opzioni di archiviazione

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per creare un cluster di failover, è necessario disporre dell'hardware seguente. Per essere supportato da Microsoft, tutto l'hardware deve essere certificato per la versione di Windows Server in esecuzione e la soluzione completa del cluster di failover deve superare tutti i test della Convalida guidata configurazione. Per altre informazioni sulla convalida del cluster di failover, vedere [Convalidare l'hardware per un cluster di failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Server**: è consigliabile utilizzare un insieme di computer corrispondenti che contengono componenti identici o simili.
- **Schede di rete e cavo (per la comunicazione di rete)** : se si utilizza iSCSI, ogni scheda di rete deve essere dedicata alla comunicazione di rete o alla configurazione iSCSI, non a entrambe.

    Evitare singoli punti di errore nell'infrastruttura di rete utilizzata per la connessione dei nodi del cluster. È ad esempio possibile connettere i nodi del cluster con più reti distinte. In alternativa, è possibile connettere i nodi del cluster con una rete costruita con schede di rete raggruppate, commutatori ridondanti, router ridondanti o hardware analogo che rimuove i singoli punti di errore.

    >[!NOTE]
    >Se si connettono nodi cluster con una sola rete, la rete supererà il requisito di ridondanza nella Convalida configurazione guidata. Il rapporto della procedura guidata includerà tuttavia un avviso a indicare che la rete deve essere priva di singoli punti di errore.

- **Controller di dispositivi o adattatori appropriati per l'archiviazione**:

  - **Serial Attached SCSI o Fibre Channel**: se si utilizza Serial Attached SCSI o Fibre Channel, in tutti i server del cluster tutti gli elementi dello stack di archiviazione devono essere identici. È necessario che il software Multipath I/O (MPIO) sia identico e che il software del modulo specifico del dispositivo (DSM) sia identico. È consigliabile che i controller dei dispositivi di archiviazione di massa, ovvero la scheda bus host (HBA), i driver HBA e il firmware HBA, collegati all'archiviazione del cluster siano identici. Se si utilizzano schede HBA diverse, è opportuno verificare con il fornitore dell'archiviazione che vengano eseguite le configurazioni supportate o consigliate.
  - **iSCSI**: se si utilizza iSCSI, ogni server del cluster deve disporre di una o più schede di rete o HBA dedicate all'archiviazione cluster. La rete utilizzata per i dispositivi iSCSI non deve essere utilizzata per la comunicazione di rete. In tutti i server del cluster, le schede di rete utilizzate per eseguire la connessione alla destinazione di archiviazione iSCSI devono essere identiche ed è consigliabile utilizzare Gigabit Ethernet o versione successiva.
- **Archiviazione**: è necessario usare [spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) o lo spazio di archiviazione condiviso compatibile con Windows Server 2012 R2 o Windows Server 2012. È possibile utilizzare lo spazio di archiviazione condiviso collegato ed è inoltre possibile utilizzare le condivisioni file SMB 3,0 come archiviazione condivisa per i server che eseguono Hyper-V configurati in un cluster di failover. Per ulteriori informazioni, vedere [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    Nella maggior parte dei casi, l'archiviazione collegata deve contenere più dischi separati (LUN, Logical Unit Number) configurati a livello hardware. Per alcuni cluster, un disco viene utilizzato come disco di controllo (descritto alla fine di questa sottosezione). Altri dischi contengono i file necessari per i ruoli del cluster (in precedenza denominati servizi o applicazioni del cluster). Di seguito sono elencati i requisiti di archiviazione:

  - Per usare il supporto del disco nativo incluso nel clustering di failover, usare dischi di base, non dinamici.
  - È consigliabile formattare le partizioni con NTFS. Se si usano volumi condivisi (CSV,Cluster Shared Volumes) del cluster, la partizione per ogni disco deve essere NTFS.

    >[!NOTE]
    >Se si usa un disco di controllo per la configurazione quorum, sarà possibile formattare il disco con NTFS o ReFS (Resilient File System).

  - È possibile utilizzare MBR (Record di avvio principale, Master Boot Record) o GPT (Tabella di partizione GUID, GUID Partition Table) come stile di partizione del disco.

    Un disco di controllo è un disco nell'archivio cluster designato per conservare una copia del database di configurazione del cluster. Un cluster di failover dispone di un disco di controllo solo se tale funzionalità è stata specificata durante la configurazione quorum. Per ulteriori informazioni, vedere informazioni [sul quorum in spazi di archiviazione diretta](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Requisiti hardware per Hyper-V

Se si crea un cluster di failover che include macchine virtuali in cluster, è necessario che i server del cluster supportino i requisiti hardware per il ruolo Hyper-V. Hyper-V richiede un processore a 64 bit che include quanto indicato di seguito:

- Virtualizzazione assistita mediante hardware. Disponibile nei processori che includono un'opzione di virtualizzazione, in particolare i processori con tecnologia di virtualizzazione Intel (Intel VT) o la virtualizzazione AMD (AMD-V).
- È necessario rendere disponibile e abilitare Protezione esecuzione programmi applicata a livello di hardware. In particolare, è necessario abilitare Intel XD Bit (eXecute Disable Bit) o AMD NX Bit (No eXecute Bit).

Per ulteriori informazioni sul ruolo Hyper-V, vedere [Panoramica di Hyper-v](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Distribuzione di reti SAN con cluster di failover

In fase di distribuzione di una rete SAN con un cluster di failover, attenersi alle linee guida seguenti:

- **Confermare la compatibilità dell'archiviazione**: verificare con i produttori e i fornitori che lo spazio di archiviazione, inclusi i driver, il firmware e il software utilizzati per l'archiviazione, siano compatibili con i cluster di failover nella versione di Windows Server in esecuzione.
- **Isolare i dispositivi di archiviazione, un cluster per dispositivo**: i server di cluster diversi non devono essere in grado di accedere agli stessi dispositivi di archiviazione. Nella maggior parte dei casi, è consigliabile isolare un LUN utilizzato da un insieme di server del cluster da tutti gli altri server mediante il mascheramento dei LUN o la suddivisione in zone.
- **Valutare l'utilizzo di software Multipath I/O o di gruppi di schede di rete**: in un fabric di archiviazione a disponibilità elevata è possibile distribuire cluster di failover con più schede bus host utilizzando software Multipath I/O o un gruppo di schede di rete (detto anche bilanciamento del carico e failover). In questo modo viene fornito il livello più elevato di ridondanza e disponibilità. Per Windows Server 2012 R2 o Windows Server 2012, la soluzione a percorsi multipli deve essere basata su Microsoft Multipath I/O (MPIO). Il fornitore dell'hardware offre in genere un modulo DSM MPIO per l'hardware, sebbene Windows Server includa uno o più moduli DSM all'interno del sistema operativo.

    Per ulteriori informazioni su LBFO, vedere la pagina relativa alla [Panoramica del gruppo NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) nella raccolta di documentazione tecnica su Windows Server.

    >[!IMPORTANT]
    >Le schede HBA e il software Multipath I/O possono variare significativamente in base alle versioni. In caso di implementazione di una soluzione a percorsi multipli per il cluster, è opportuno scegliere attentamente con il fornitore dell'hardware le schede, il firmware e il software corretti per la versione di Windows Server in esecuzione.

- **Provare a usare spazi di archiviazione**: se si prevede di distribuire l'archiviazione in cluster SAS (Serial Attached SCSI) configurata usando spazi di archiviazione, vedere [distribuire spazi di archiviazione in cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) per i requisiti.

## <a name="more-information"></a>Altre informazioni

- [Clustering di failover](failover-clustering.md)
- [Spazi di archiviazione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Uso del clustering guest per la disponibilità elevata](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)