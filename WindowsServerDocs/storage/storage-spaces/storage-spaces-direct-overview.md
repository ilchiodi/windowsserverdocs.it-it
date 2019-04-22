---
title: Panoramica di Spazi di archiviazione diretta
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/06/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: Panoramica di spazi di archiviazione diretta, una funzionalità di Windows Server che consente all'utente di server del cluster con spazio di archiviazione interno in una soluzione di archiviazione software-defined.
ms.localizationpriority: medium
ms.openlocfilehash: 25de20b398f780f5da07b6b6cf4d396a7d12204a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823892"
---
# <a name="storage-spaces-direct-overview"></a>Panoramica di Spazi di archiviazione diretta

>Si applica a: Windows Server 2019, Windows Server 2016

Spazi di archiviazione diretta usa server standard di settore con unità collegate in locale per creare un'archiviazione software-defined a disponibilità elevata altamente scalabile a un costo minimo rispetto agli array SAN o NAS tradizionali. L'architettura convergente o iperconvergente semplifica notevolmente approvvigionamento e la distribuzione, mentre le funzionalità, ad esempio la memorizzazione nella cache, i livelli di archiviazione e la codifica di cancellazione, con le innovazioni hardware più recenti, ad esempio la rete RDMA e le unità NVMe, potrai distribuire efficienza senza confronti e le prestazioni.

Spazi di archiviazione diretta è incluso in Windows Server 2019 Datacenter, Windows Server 2016 Datacenter, e [build di Windows Server Insider Preview](https://insider.windows.com/for-business-getting-started-server/). 

Per altre applicazioni di spazi di archiviazione, ad esempio cluster condiviso e i server autonomi, vedere [Panoramica di spazi di archiviazione](overview.md). Se stanno cercando informazioni sulle usando spazi di archiviazione in un PC Windows 10, vedere [spazi di archiviazione in Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Comprendere</a></strong>
            <ul>
              <li>Panoramica (sei in questa fase)</li>
              <li><a href="understand-the-cache.md">Comprendere la cache</a></li>
              <li><a href="storage-spaces-fault-tolerance.md">Efficienza di archiviazione e la tolleranza di errore</a></li>
              <li><a href="drive-symmetry-considerations.md">Considerazioni sulla simmetria di unità</a></li>
              <li><a href="understand-storage-resync.md">Comprendere e monitorare la risincronizzazione di archiviazione</a></li>
              <li><a href="understand-quorum.md">Quorum del cluster e pool di comprensione</a></li>
              <li><a href="cluster-sets.md">Set di cluster</a></li>
            </ul>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Piano</a></strong>
            <ul>
              <li><a href="storage-spaces-direct-hardware-requirements.md">Requisiti hardware</a></li>
              <li><a href="csv-cache.md">Uso di CSV in memoria cache di lettura</li>
              <li><a href="choosing-drives.md">Scegliere le unità</a></li>
              <li><a href="plan-volumes.md">Pianificare i volumi</a></li>
              <li><a href="storage-spaces-direct-in-vm.md">Uso dei cluster di macchine Virtuali guest</a></li>
              <li><a href="storage-spaces-direct-disaster-recovery.md">Ripristino di emergenza</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Distribuire</a></strong>
            <ul>
                    <li><a href="deploy-storage-spaces-direct.md">Distribuire spazi di archiviazione diretta</a></li>
                    <li><a href="create-volumes.md">Creare i volumi</a></li>
              <li><a href="nested-resiliency.md">Resilienza annidata</a></li>
              <li><a href="../../failover-clustering/manage-cluster-quorum.md">Configurazione del quorum</a></li>
              <li><a href="upgrade-storage-spaces-direct-to-windows-server-2019.md">Aggiornare un cluster di spazi di archiviazione diretta per Windows Server 2019</a></li>
            </ul>
        </td>        
        <td style="padding: 5px; border: 0;">
            <strong>gestire</a></strong>
            <ul>
              <li><a href="../../manage/windows-admin-center/use/manage-hyper-converged.md">Gestire con Windows Admin Center</a></li>
              <li><a href="add-nodes.md">Aggiungere server o le unità</a></li>
              <li><a href="maintain-servers.md">Portare un server in modalità offline per manutenzione</li>
              <li><a href="remove-servers.md">Rimuovere i server</a></li>
              <li><a href="resize-volumes.md">Estendere i volumi</a></li>
              <li><a href="../update-firmware.md">Aggiornare il firmware dell'unità</a></li>
              <li><a href="performance-history.md">Cronologia delle prestazioni</a></li>
              <li><a href="delimit-volume-allocation.md">Consente di delimitare l'allocazione dei volumi</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
         <td style="padding: 5px; border: 0;">
            <strong>Risoluzione dei problemi</a></strong>
            <ul>
              <li><a href="storage-spaces-states.md">Risolvere i problemi di integrità e gli stati operativi</a></li>
              <li><a href="data-collection.md">Raccogliere dati diagnostici con spazi di archiviazione diretta</a></li>
            </ul>
         <td style="padding: 5px; border: 0;">
            <strong>Post di blog recenti</a></strong>
            <ul>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/">13.7 milione di IOPS con spazi di archiviazione diretta: nuovo record del settore per l'infrastruttura iperconvergente</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/">Infrastruttura iperconvergente in Windows Server 2019 - l'orologio del conto alla rovescia inizia ora!</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/">Annunci di big data cinque dal Summit Windows Server</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/">I cluster di spazi di archiviazione diretta 10.000 e il conteggio di...</a></li>
            </ul>
</table>

## <a name="videos"></a>Video

**Rapida panoramica Video (5 minuti)**

<iframe src="https://www.youtube-nocookie.com/embed/raeUiNtMk0E" width="560" height="315" allowfullscreen></iframe>

**Spazi di archiviazione diretta presso Microsoft Ignite 2018 (1 ora)**

[Guarda su YouTube](https://www.youtube.com/watch?v=5kaUiW3qo30)

**Spazi di archiviazione diretta a Microsoft Ignite 2017 (1 ora)**

[Guarda su YouTube](https://www.youtube.com/watch?v=YDr2sqNB-3c)

**Avviare l'evento Microsoft Ignite 2016 (1 ora)**

[Guarda su YouTube](https://www.youtube.com/watch?v=-LK2ViRGbWs)

## <a name="key-benefits"></a>Principali vantaggi

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Semplicità.</b> Puoi passare dai server standard del settore che eseguono Windows Server 2016 al tuo primo cluster di Spazi di archiviazione diretta in meno di 15 minuti. Per gli utenti di System Center, la distribuzione avviene semplicemente selezionando una casella di controllo.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/performance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Prestazioni senza confronti.</b> All-flash o ibrido, Spazi di archiviazione diretta supera facilmente le <a href="https://blogs.technet.microsoft.com/filecab/2016/07/26/storage-iops-update-with-storage-spaces-direct/">150.000 operazioni di I/O al secondo casuali 4K miste per server</a> con bassa latenza coerente grazie all'architettura con hypervisor predefinito, cache di lettura/scrittura predefinita e supporto di unità NVMe all'avanguardia montate direttamente sul bus PCIe.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/fault-tolerance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Tolleranza di errore. </b> La resilienza predefinita gestisce unità, server o guasti dei componenti con disponibilità continua. Puoi anche configurare distribuzioni di dimensioni maggiori per la <a href="../../failover-clustering/fault-domains.md">tolleranza di errore di chassis e rack</a>. Quando si verifica un errore hardware, è sufficiente effettuare lo swapping. Il software si ripara da solo senza richiedere operazioni di gestione complesse.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Efficienza delle risorse.</b> La codifica di cancellazione offre un'efficienza di archiviazione fino a 2,4 volte maggiore con innovazioni esclusive come i codici di ricostruzione locale e i livelli in tempo reale ReFS per estendere questi vantaggi alle unità disco rigido e ai carichi di lavoro a freddo/caldo misti, riducendo al contempo l'utilizzo della CPU per restituire le risorse dove sono più necessarie, ovvero nelle macchine virtuali.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Facilità di gestione.</b> Usare i <a href="../storage-qos/storage-qos-overview.md">controlli QoS di archiviazione</a> per mantenere sotto controllo le macchine virtuali eccessivamente occupate con limiti minimi e massimi di operazioni di I/O per macchina virtuale. <a href="../../failover-clustering/health-service-overview.md">Servizio integrità</a> offre monitoraggio e avvisi predefiniti continui, mentre le nuove API migliorano le prestazioni e le metriche di capacità dell'intero cluster.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Scalabilità.</b> Fino a 16 server e oltre 400 unità, per un massimo di 1 petabyte (1.000 terabyte) di archiviazione per cluster. Per la scalabilità orizzontale, è sufficiente aggiungere unità o server. Spazi di archiviazione diretta caricherà automaticamente le nuove unità e inizierà a usarle. L'efficienza e le prestazioni di archiviazione migliorano la prevedibilità su vasta scala.
        </td>
    </tr>
</table>

## <a name="deployment-options"></a>Opzioni di distribuzione

Spazi di archiviazione diretta è stato progettato per due opzioni di distribuzione distinte:

### <a name="converged"></a>Con convergenza

**L'archiviazione e risorse di calcolo in cluster distinti.** L'opzione di distribuzione convergente, chiamata anche disaggregata, sovrappone un file server di scalabilità orizzontale (SoFS) a Spazi di archiviazione diretta per offrire un dispositivo NAS in condivisioni file SMB3. In questo modo si ottiene un ridimensionamento del calcolo/carico di lavoro indipendentemente dal cluster di archiviazione, fondamentale per le distribuzioni su larga scala come l'infrastruttura distribuita come servizio (IaaS) Hyper-V per i provider di servizi e le aziende.

![Spazi di archiviazione diretta fornisce l'archiviazione tramite la funzionalità File server di scalabilità orizzontale alle macchine virtuali Hyper-V in un altro server o cluster](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### <a name="hyper-converged"></a>Con iperconvergenza

**Un cluster di calcolo e archiviazione.** L'opzione di distribuzione iperconvergente esegue macchine virtuali Hyper-V o database di SQL Server direttamente nei server fornendo archiviazione e archiviando i file nei volumi locali. In questo modo, non è più necessario configurare l'accesso e le autorizzazioni per il file server e si riducono i costi per l'hardware per le distribuzioni di piccole e medie imprese o uffici remoti/succursali. Visualizzare [distribuire spazi di archiviazione diretta](deploy-storage-spaces-direct.md).

![Spazi di archiviazione diretta fornisce archiviazione alle macchine virtuali Hyper-V VMs nello stesso cluster](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## <a name="how-it-works"></a>Come funziona

Spazi di archiviazione diretta è l'evoluzione di Spazi di archiviazione, funzionalità disponibile per la prima volta in Windows Server 2012. Usa molte delle funzionalità di Windows Server, ad esempio il clustering di failover, il file system del volume condiviso cluster, Server Message Block (SMB) 3 e naturalmente Spazi di archiviazione. Offre anche una nuova tecnologia, il bus di archiviazione software.

L'immagine seguente offre una panoramica dello stack di Spazi di archiviazione diretta:

![Stack di Spazi di archiviazione diretta](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**Hardware di rete.** Spazi di archiviazione diretta usa SMB3, inclusi SMB diretto e SMB multicanale, su Ethernet per la comunicazione tra server. Si consiglia di disporre di almeno 10 GbE con accesso diretto a memoria remota (RDMA), iWARP o RoCE.

**Hardware di archiviazione.** Da 2 a 16 server con unità SATA, SAS o NVMe collegate in locale. Ogni server deve avere almeno 2 unità SSD e almeno 4 unità aggiuntive. I dispositivi SATA e SAS devono essere dietro a una scheda bus host (HBA) e un espansore SAS. Si consiglia di usare le piattaforme accuratamente progettate e ampiamente convalidate dei partner Microsoft (presto disponibili).

**Clustering di failover.** La funzionalità di clustering predefinita di Windows Server viene usata per connettere i server.

**Bus di archiviazione software.** Il bus di archiviazione software è una nuova funzionalità di Spazi di archiviazione diretta. Estende il cluster e imposta un'infrastruttura di archiviazione software-defined per mezzo della quale tutti i server possono vedere tutte le rispettive unità locali. Può essere considerato un'alternativa ai cavi più costosi e limitati Fibre Channel o Shared SAS.

**Cache di livello del Bus di archiviazione.** Il Bus di archiviazione Software associa dinamicamente le unità più veloci presente (ad esempio, SSD) a unità più lente (ad esempio HDD) per fornire la cache sul lato server di lettura/scrittura che accelera l'IO e incrementa la velocità effettiva.

**Pool di archiviazione.** Il pool di archiviazione è la raccolta di unità che costituiscono la base di Spazi di archiviazione. Il pool viene creato automaticamente e tutte le unità idonee vengono individuate e aggiunte automaticamente al pool. Si consiglia di usare un pool per cluster con le impostazioni predefinite. Per altre informazioni leggi l'[approfondimento sul pool di archiviazione](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

**Spazi di archiviazione.** Spazi di archiviazione fornisce tolleranza di errore ai "dischi" virtuali mediante [mirroring, codifica di cancellazione o entrambi](storage-spaces-fault-tolerance.md). Può essere considerato un RAID software-defined che usa le unità nel pool. In Spazi di archiviazione diretta i dischi virtuali hanno in genere resilienza a due errori simultanei di server o unità (ad esempio, mirroring a tre vie con ciascuna copia dei dati in un server diverso) sebbene sia disponibile anche la tolleranza di errore di chassis e rack.

**Resilient File System (ReFS).** ReFS è il file system specificatamente progettato per la virtualizzazione. Include accelerazioni significative per le operazioni di file con estensione vhdx, ad esempio creazione, espansione, unione di checkpoint e checksum predefiniti per rilevare e correggere gli errori di bit. Offre anche suddivisioni in livelli in tempo reale che ruotano i dati tra livelli di archiviazione "a caldo" e "a freddo" in tempo reale in base all'utilizzo.

**Volumi condivisi del cluster.** Il file system CSV unisce tutti i volumi ReFS in un unico spazio dei nomi accessibile tramite qualsiasi server, in modo che in ogni server ogni volume agisce come se fosse montato in locale.

**Scale-Out File Server.** Questo livello finale è necessario solo per le distribuzioni convergenti. Offre accesso remoto ai file tramite il protocollo di accesso SMB3 ai client, ad esempio a un altro cluster che esegue Hyper-V in rete, trasformando Spazi di archiviazione diretta in dispositivo NAS.

## <a name="customer-stories"></a>Esperienze dei clienti

Esistono [cluster oltre 10.000](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/) in tutto il mondo che esegue spazi di archiviazione diretta. Le organizzazioni di qualsiasi dimensione, da aziende di piccole dimensioni distribuzione solo due nodi, in aziende di grandi dimensioni e strutture governative statali distribuiscono centinaia di nodi, dipendono da spazi di archiviazione diretta per le applicazioni critiche e infrastruttura.

Visita [Microsoft.com/HCI](https://www.microsoft.com/hci) a leggi i casi:

[![Griglia di logo dei clienti](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## <a name="management-tools"></a>Strumenti di gestione

I seguenti strumenti sono utilizzabile per gestire e/o monitorare gli spazi di archiviazione diretta:

| Nome | Con interfaccia grafica o della riga di comando? | Versione a pagamento o incluso? |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | Con interfaccia grafica    | Inclusa |
| Server Manager e gestione Cluster di Failover                                 | Con interfaccia grafica    | Inclusa |
| Windows PowerShell                                                        | Riga di comando | Inclusa |
| [System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) & [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | Con interfaccia grafica    | A pagamento     |

## <a name="get-started"></a>Informazioni di base

Prova Spazi di archiviazione diretta [in Microsoft Azure](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/) o scarica una copia di valutazione con licenza di 180 giorni di Windows Server da [Windows Server Valutazioni](https://go.microsoft.com/fwlink/?linkid=842602).

## <a name="see-also"></a>Vedere anche

-   [Efficienza di archiviazione e la tolleranza di errore](storage-spaces-fault-tolerance.md)
-   [Replica di archiviazione](../storage-replica/storage-replica-overview.md)
-   [Spazio di archiviazione nel blog di Microsoft](https://blogs.technet.microsoft.com/filecab/)
-   [Storage Spaces Direct throughput with iWARP](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp) (Velocità effettiva di Spazi di archiviazione diretta con iWARP) (blog di TechNet)
-   [What ' s New in Failover Clustering in Windows Server](../../failover-clustering/whats-new-in-failover-clustering.md)  
-   [QoS di archiviazione](../storage-qos/storage-qos-overview.md)
-   [Windows IT Pro supporto](https://www.microsoft.com/itpro/windows/support)
