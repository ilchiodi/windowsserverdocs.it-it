---
title: Panoramica di Spazi di archiviazione diretta
ms.prod: windows-server
ms.author: cosdar
manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/26/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: Panoramica di Spazi di archiviazione diretta, una funzionalità di Windows Server che consente di raggruppare i server con archiviazione interna in una soluzione di archiviazione definita dal software.
ms.localizationpriority: medium
ms.openlocfilehash: b032d286398b3c1719d290ca83da8bbc9c6b9f85
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859134"
---
# <a name="storage-spaces-direct-overview"></a>Panoramica di Spazi di archiviazione diretta

>Si applica a: Windows Server 2019, Windows Server 2016

Spazi di archiviazione diretta usa server standard di settore con unità collegate in locale per creare un'archiviazione software-defined a disponibilità elevata altamente scalabile a un costo minimo rispetto agli array SAN o NAS tradizionali. L'architettura convergente o iperconvergente semplifica radicalmente l'approvvigionamento e la distribuzione, mentre funzionalità come la memorizzazione nella cache, i livelli di archiviazione e la codifica di cancellazione, insieme alle innovazioni hardware più recenti, come la rete RDMA e le unità NVMe, offrono efficienza e prestazioni senza precedenti.

Spazi di archiviazione diretta è incluso nelle compilazioni Windows Server 2019 datacenter, Windows Server 2016 datacenter e [Windows Server Insider Preview](https://insider.windows.com/for-business-getting-started-server/). 

Per altre applicazioni di spazi di archiviazione, ad esempio i cluster SAS condivisi e i server autonomi, vedere [Panoramica di spazi di archiviazione](overview.md). Per informazioni sull'uso di spazi di archiviazione in un PC Windows 10, vedere [spazi di archiviazione in Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

|       |       |
|   -   |   -   |
| **Comprensione**<br><ul><li>Panoramica (sei in questa fase)</li><li>[Informazioni sulla cache](understand-the-cache.md)</li><li>[Tolleranza di errore ed efficienza di archiviazione](storage-spaces-fault-tolerance.md)<li>[Considerazioni sulla simmetria dell'unità](drive-symmetry-considerations.md)</li><li>[Comprendere e monitorare risincronizzazione di archiviazione](understand-storage-resync.md)</li><li>[Informazioni sul quorum di cluster e pool](understand-quorum.md)</li><li>[Set di cluster](cluster-sets.md)</li> | **Pianificazione**<br><ul><li>[Requisiti hardware](storage-spaces-direct-hardware-requirements.md)</li><li>[Uso della cache di lettura in memoria Volume condiviso cluster](csv-cache.md)</li><li>[Scegliere le unità](choosing-drives.md)</li><li>[Pianificare i volumi](plan-volumes.md)</li><li>[Uso di cluster di macchine virtuali guest](storage-spaces-direct-in-vm.md)</li><li>[Ripristino di emergenza](storage-spaces-direct-disaster-recovery.md)</li> |
| **Distribuzione**<br><ul><li>[Distribuire Spazi di archiviazione diretta](deploy-storage-spaces-direct.md)</li><li>[Creare volumi](create-volumes.md)</li><li>[Resilienza annidata](nested-resiliency.md)</li><li>[Configurare quorum](../../failover-clustering/manage-cluster-quorum.md)</li><li>[Eseguire l'aggiornamento di un cluster di Spazi di archiviazione diretta a Windows Server 2019](upgrade-storage-spaces-direct-to-windows-server-2019.md)</li><li>[Concetti e distribuzione della memoria persistente](deploy-pmem.md)</li> | **Gestione**<br><ul><li>[Gestire con Windows Admin Center](../../manage/windows-admin-center/use/manage-hyper-converged.md)</li><li>[Aggiungere server o unità](add-nodes.md)</li><li>[Disconnessione del server a scopo di manutenzione](maintain-servers.md)</li><li>[Rimuovere server](remove-servers.md)</li><li>[Estendere volumi](resize-volumes.md)</li><li>[Eliminare volumi](delete-volumes.md)</li><li>[Aggiornare il firmware delle unità](../update-firmware.md)</li><li>[Cronologia delle prestazioni](performance-history.md)</li><li>[Delimitare l'allocazione di volumi](delimit-volume-allocation.md)</li><li>[Usare monitoraggio di Azure in un cluster iperconvergente](configure-azure-monitor.md)</li> |
| **Risoluzione dei problemi**<br><ul><li>[Scenari di risoluzione dei problemi](troubleshooting-storage-spaces.md)</li><li>[Risolvere i problemi relativi a stato e stato operativo](storage-spaces-states.md)</li><li>[Raccogliere i dati di diagnostica con Spazi di archiviazione diretta](data-collection.md)</li><li>[Gestione dell'integrità di memoria della classe di archiviazione](Storage-class-memory-health.md)</li> | **Post di Blog recenti**<br><ul><li>[13,7 milioni IOPS con Spazi di archiviazione diretta: il nuovo record di settore per l'infrastruttura iperconvergente](https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/)</li><li>[Infrastruttura iperconvergente in Windows Server 2019: il clock del conto alla rovescia inizia subito.](https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/)</li><li>[Cinque annunci di grandi dimensioni da Windows Server Summit](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap)</li><li>[10.000 Spazi di archiviazione diretta cluster e conteggio...](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/)</li> |

## <a name="videos"></a>Videos

**Panoramica video veloce (5 minuti)**

> [!Video https://www.youtube-nocookie.com/embed/raeUiNtMk0E]

**Spazi di archiviazione diretta a Microsoft Ignite 2018 (1 ora)**

> [!Video https://www.youtube-nocookie.com/embed/5kaUiW3qo30]

**Spazi di archiviazione diretta a Microsoft Ignite 2017 (1 ora)**

> [!Video https://www.youtube-nocookie.com/embed/YDr2sqNB-3c]

**Evento di avvio in Microsoft Ignite 2016 (1 ora)**

> [!Video https://www.youtube-nocookie.com/embed/LK2ViRGbWs]

## <a name="key-benefits"></a>Principali vantaggi

|       |       |
|   -   |   -   |
| ![Semplicità](media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png)   | **Semplicità.** Puoi passare dai server standard del settore che eseguono Windows Server 2016 al tuo primo cluster di Spazi di archiviazione diretta in meno di 15 minuti. Per gli utenti di System Center, la distribuzione avviene semplicemente selezionando una casella di controllo.       |
| ![Prestazioni senza precedenti](media/storage-spaces-direct-in-windows-server-2016/performance-icon.png)   | **Prestazioni senza precedenti.** All-flash o ibrido, Spazi di archiviazione diretta supera facilmente le [150.000 operazioni di I/O al secondo casuali 4K miste per server](https://blogs.technet.microsoft.com/filecab/2016/07/26/storage-iops-update-with-storage-spaces-direct/) con bassa latenza coerente grazie all'architettura con hypervisor predefinito, cache di lettura/scrittura predefinita e supporto di unità NVMe all'avanguardia montate direttamente sul bus PCIe.      |
| ![Tolleranza di errore](media/storage-spaces-direct-in-windows-server-2016/fault-tolerance-icon.png)   | **Tolleranza di errore.** La resilienza predefinita gestisce gli errori delle unità, dei server o dei componenti con disponibilità continua. Puoi anche configurare distribuzioni di dimensioni maggiori per la [tolleranza di errore di chassis e rack](../../failover-clustering/fault-domains.md). Quando si verifica un errore hardware, è sufficiente effettuare lo swapping. Il software si ripara da solo senza richiedere operazioni di gestione complesse.       |
| ![Efficienza delle risorse](media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png)   | **Efficienza delle risorse.** La codifica di cancellazione offre un'efficienza di archiviazione fino a 2,4 volte maggiore con innovazioni esclusive come i codici di ricostruzione locale e i livelli in tempo reale ReFS per estendere questi vantaggi alle unità disco rigido e ai carichi di lavoro a freddo/caldo misti, riducendo al contempo l'utilizzo della CPU per restituire le risorse dove sono più necessarie, ovvero nelle macchine virtuali.       |
| ![Gestibilità](media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png)   | **Gestione**. Usare i [controlli QoS di archiviazione](../storage-qos/storage-qos-overview.md) per mantenere sotto controllo le macchine virtuali eccessivamente occupate con limiti minimi e massimi di operazioni di I/O per macchina virtuale. [Servizio integrità](../../failover-clustering/health-service-overview.md) offre monitoraggio e avvisi predefiniti continui, mentre le nuove API migliorano le prestazioni e le metriche di capacità dell'intero cluster.      |
| ![Scalabilità](media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png)   | **Scalabilità**. Fino a 16 server e oltre 400 unità, per un massimo di 1 petabyte (1.000 terabyte) di archiviazione per cluster. Per la scalabilità orizzontale, è sufficiente aggiungere unità o server. Spazi di archiviazione diretta caricherà automaticamente le nuove unità e inizierà a usarle. L'efficienza e le prestazioni di archiviazione migliorano la prevedibilità su vasta scala.       |

## <a name="deployment-options"></a>Opzioni di distribuzione

Spazi di archiviazione diretta è stato progettato per due opzioni di distribuzione distinte:

### <a name="converged"></a>Convergenza eseguita

**Archiviazione e calcolo in cluster distinti.** L'opzione di distribuzione convergente, chiamata anche disaggregata, sovrappone un file server di scalabilità orizzontale (SoFS) a Spazi di archiviazione diretta per offrire un dispositivo NAS in condivisioni file SMB3. In questo modo si ottiene un ridimensionamento del calcolo/carico di lavoro indipendentemente dal cluster di archiviazione, fondamentale per le distribuzioni su larga scala come l'infrastruttura distribuita come servizio (IaaS) Hyper-V per i provider di servizi e le aziende.

![Spazi di archiviazione diretta fornisce l'archiviazione tramite la funzionalità File server di scalabilità orizzontale alle macchine virtuali Hyper-V in un altro server o cluster](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### <a name="hyper-converged"></a>Con iperconvergenza

**Un cluster per il calcolo e l'archiviazione.** L'opzione di distribuzione iperconvergente esegue macchine virtuali Hyper-V o database di SQL Server direttamente nei server fornendo archiviazione e archiviando i file nei volumi locali. In questo modo, non è più necessario configurare l'accesso e le autorizzazioni per il file server e si riducono i costi per l'hardware per le distribuzioni di piccole e medie imprese o uffici remoti/succursali. Vedere [Deploy spazi di archiviazione diretta](deploy-storage-spaces-direct.md).

![Spazi di archiviazione diretta fornisce archiviazione alle macchine virtuali Hyper-V VMs nello stesso cluster](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## <a name="how-it-works"></a>Come funziona

Spazi di archiviazione diretta è l'evoluzione di Spazi di archiviazione, funzionalità disponibile per la prima volta in Windows Server 2012. Usa molte delle funzionalità di Windows Server, ad esempio il clustering di failover, il file system del volume condiviso cluster, Server Message Block (SMB) 3 e naturalmente Spazi di archiviazione. Offre anche una nuova tecnologia, il bus di archiviazione software.

L'immagine seguente offre una panoramica dello stack di Spazi di archiviazione diretta:

![Stack di Spazi di archiviazione diretta](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**Hardware di rete.** Spazi di archiviazione diretta usa SMB3, inclusi SMB diretto e SMB multicanale, su Ethernet per la comunicazione tra server. Si consiglia di disporre di almeno 10 GbE con accesso diretto a memoria remota (RDMA), iWARP o RoCE.

**Hardware di archiviazione.** Da 2 a 16 server con unità SATA, SAS o NVMe collegate in locale. Ogni server deve avere almeno 2 unità SSD e almeno 4 unità aggiuntive. I dispositivi SATA e SAS devono essere dietro a una scheda bus host (HBA) e un espansore SAS. Si consiglia di usare le piattaforme accuratamente progettate e ampiamente convalidate dei partner Microsoft (presto disponibili).

**Clustering di failover.** La funzionalità di clustering predefinita di Windows Server viene usata per connettere i server.

**Bus di archiviazione software.** Il bus di archiviazione software è una nuova funzionalità di Spazi di archiviazione diretta. Estende il cluster e imposta un'infrastruttura di archiviazione software-defined per mezzo della quale tutti i server possono vedere tutte le rispettive unità locali. Può essere considerato un'alternativa ai cavi più costosi e limitati Fibre Channel o Shared SAS.

**Cache del livello del bus di archiviazione.** Il bus di archiviazione software associa dinamicamente le unità più veloci presenti (ad esempio unità SSD) a unità più lente (ad esempio HDD) per fornire la memorizzazione nella cache di lettura/scrittura sul lato server che accelera l'i/o e aumenta la velocità effettiva.

**Pool di archiviazione.** Il pool di archiviazione è la raccolta di unità che costituiscono la base di Spazi di archiviazione. Il pool viene creato automaticamente e tutte le unità idonee vengono individuate e aggiunte automaticamente al pool. Si consiglia di usare un pool per cluster con le impostazioni predefinite. Per altre informazioni leggi l'[approfondimento sul pool di archiviazione](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

**Spazi di archiviazione.** Spazi di archiviazione fornisce tolleranza di errore ai "dischi" virtuali mediante [mirroring, codifica di cancellazione o entrambi](storage-spaces-fault-tolerance.md). Può essere considerato un RAID software-defined che usa le unità nel pool. In Spazi di archiviazione diretta i dischi virtuali hanno in genere resilienza a due errori simultanei di server o unità (ad esempio, mirroring a tre vie con ciascuna copia dei dati in un server diverso) sebbene sia disponibile anche la tolleranza di errore di chassis e rack.

**Resilient file System (ReFS).** ReFS è il file system specificatamente progettato per la virtualizzazione. Include accelerazioni significative per le operazioni di file con estensione vhdx, ad esempio creazione, espansione, unione di checkpoint e checksum predefiniti per rilevare e correggere gli errori di bit. Offre anche suddivisioni in livelli in tempo reale che ruotano i dati tra livelli di archiviazione "a caldo" e "a freddo" in tempo reale in base all'utilizzo.

**Volumi condivisi cluster.** Il file system CSV unisce tutti i volumi ReFS in un unico spazio dei nomi accessibile tramite qualsiasi server, in modo che in ogni server ogni volume agisce come se fosse montato in locale.

**File server di scalabilità orizzontale.** Questo livello finale è necessario solo per le distribuzioni convergenti. Offre accesso remoto ai file tramite il protocollo di accesso SMB3 ai client, ad esempio a un altro cluster che esegue Hyper-V in rete, trasformando Spazi di archiviazione diretta in dispositivo NAS.

## <a name="customer-stories"></a>Storie dei clienti

Sono presenti [più cluster 10.000](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/) in tutto il mondo che eseguono spazi di archiviazione diretta. Le organizzazioni di qualsiasi dimensione, dalle piccole aziende che distribuiscono solo due nodi, alle grandi aziende e ai governi che distribuiscono centinaia di nodi, dipendono da Spazi di archiviazione diretta per le applicazioni e l'infrastruttura critiche.

Visitare [Microsoft.com/HCI](https://www.microsoft.com/hci) per leggere le storie:

[Griglia di ![dei logo dei clienti](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## <a name="management-tools"></a>Strumenti di gestione

Per gestire e/o monitorare Spazi di archiviazione diretta, è possibile usare gli strumenti seguenti:

| Name | Grafica o riga di comando? | Pagata o inclusa? |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | Grafica    | Inclusione |
| Server Manager & Gestione cluster di failover                                 | Grafica    | Inclusione |
| Windows PowerShell                                                        | Riga di comando | Inclusione |
| [System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) <br>& [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | Grafica    | A pagamento     |

## <a name="get-started"></a>Attività iniziali

Prova Spazi di archiviazione diretta [in Microsoft Azure](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/) o scarica una copia di valutazione con licenza di 180 giorni di Windows Server da [Windows Server Valutazioni](https://go.microsoft.com/fwlink/?linkid=842602).

## <a name="see-also"></a>Vedere anche

- [Tolleranza di errore ed efficienza di archiviazione](storage-spaces-fault-tolerance.md)
- [Replica di archiviazione](../storage-replica/storage-replica-overview.md)
- [Blog di archiviazione in Microsoft](https://blogs.technet.microsoft.com/filecab/)
- [Storage Spaces Direct throughput with iWARP](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp) (Velocità effettiva di Spazi di archiviazione diretta con iWARP) (blog di TechNet)
- [Novità del clustering di failover in Windows Server](../../failover-clustering/whats-new-in-failover-clustering.md)  
- [Qualità del servizio di archiviazione](../storage-qos/storage-qos-overview.md)
- [Supporto di Windows IT Pro](https://www.microsoft.com/itpro/windows/support)
