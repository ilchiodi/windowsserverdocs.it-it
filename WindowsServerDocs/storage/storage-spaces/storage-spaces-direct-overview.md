---
title: Panoramica di Spazi di archiviazione diretta
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/26/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: Panoramica di spazi di archiviazione diretta, una funzionalità di Windows Server che ti consente di creare cluster di server con archiviazione interna in una soluzione di archiviazione software-defined.
ms.localizationpriority: medium
ms.openlocfilehash: d71e4fc404f760102a2b22f71bbeec680868e9b8
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262784"
---
# Panoramica di Spazi di archiviazione diretta

>Si applica a: Windows Server 2019, Windows Server 2016

Spazi di archiviazione diretta usa server standard di settore con unità collegate in locale per creare un'archiviazione software-defined a disponibilità elevata altamente scalabile a un costo minimo rispetto agli array SAN o NAS tradizionali. La sua architettura convergente o iperconvergente semplifica notevolmente l'approvvigionamento e la distribuzione, mentre le funzionalità, ad esempio la memorizzazione nella cache, livelli di archiviazione e codifica di cancellazione, insieme a innovazioni hardware più recenti, ad esempio funzionalità di rete RDMA e le unità NVMe, offrire senza confronti efficienza e le prestazioni.

Spazi di archiviazione diretta è incluso in Windows Server 2019 Datacenter, Windows Server 2016 Datacenter e [Build di Windows Server Insider Preview](https://insider.windows.com/for-business-getting-started-server/). 

Per altre applicazioni di spazi di archiviazione, ad esempio i cluster Shared SAS e server autonomi, vedi [la panoramica di spazi di archiviazione](overview.md). Se stai cercando info sull'uso di spazi di archiviazione in un PC Windows 10, vedi [Spazi di archiviazione in Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Esaminare con attenzione</a></strong>
            <ul>
              <li>Panoramica (sei in questa fase)</li>
              <li><a href="understand-the-cache.md">Informazioni sulla cache</a></li>
              <li><a href="storage-spaces-fault-tolerance.md">Tolleranza di errore ed efficienza di archiviazione</a></li>
              <li><a href="drive-symmetry-considerations.md">Considerazioni sulla simmetria dell'unità</a></li>
              <li><a href="understand-storage-resync.md">Comprendere e monitorare risincronizzazione di archiviazione</a></li>
              <li><a href="understand-quorum.md">Quorum di cluster e pool conoscenza</a></li>
              <li><a href="cluster-sets.md">Set di cluster</a></li>
            </ul>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Pianificazione</a></strong>
            <ul>
              <li><a href="storage-spaces-direct-hardware-requirements.md">Requisiti hardware</a></li>
              <li><a href="csv-cache.md">Uso della cache di lettura in memoria Volume condiviso cluster.</li>
              <li><a href="choosing-drives.md">Scegliere le unità</a></li>
              <li><a href="plan-volumes.md">Pianificare i volumi</a></li>
              <li><a href="storage-spaces-direct-in-vm.md">Uso dei cluster della macchina virtuale guest</a></li>
              <li><a href="storage-spaces-direct-disaster-recovery.md">Ripristino di emergenza</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Distribuire</a></strong>
            <ul>
                    <li><a href="deploy-storage-spaces-direct.md">Distribuire spazi di archiviazione diretta</a></li>
                    <li><a href="create-volumes.md">Creare volumi</a></li>
              <li><a href="nested-resiliency.md">Resilienza annidata</a></li>
              <li><a href="../../failover-clustering/manage-cluster-quorum.md">Configurare quorum</a></li>
              <li><a href="upgrade-storage-spaces-direct-to-windows-server-2019.md">Eseguire l'aggiornamento di un cluster di spazi di archiviazione diretta a Windows Server 2019</a></li>
            </ul>
        </td>        
        <td style="padding: 5px; border: 0;">
            <strong>Gestisci</a></strong>
            <ul>
              <li><a href="../../manage/windows-admin-center/use/manage-hyper-converged.md">Gestire con Windows Admin Center</a></li>
              <li><a href="add-nodes.md">Aggiungere server o unità</a></li>
              <li><a href="maintain-servers.md">Disconnessione del server a scopo di manutenzione</li>
              <li><a href="remove-servers.md">Rimuovere server</a></li>
              <li><a href="resize-volumes.md">Estendere i volumi</a></li>
              <li><a href="../update-firmware.md">Aggiornare il firmware delle unità</a></li>
              <li><a href="performance-history.md">Cronologia delle prestazioni</a></li>
              <li><a href="delimit-volume-allocation.md">Delimitare l'allocazione di volumi</a></li>
              <li><a href="configure-azure-monitor.md">Utilizzare Azure Monitor in un cluster iper-convergenti</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
         <td style="padding: 5px; border: 0;">
            <strong>Risoluzione dei problemi</a></strong>
            <ul>
              <li><a href="storage-spaces-states.md">Risolvere i problemi di stati di integrità e operativi</a></li>
              <li><a href="data-collection.md">Raccogliere dati di diagnostica con spazi di archiviazione diretta</a></li>
            </ul>
         <td style="padding: 5px; border: 0;">
            <strong>Post di blog recenti</a></strong>
            <ul>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/">13.7 milione di IOP con spazi di archiviazione diretta: il nuovo record di settore per l'infrastruttura iperconvergente</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/">Infrastruttura iperconvergente in Windows Server 2019 - l'orologio conto alla rovescia inizia ora!</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/">Annunci di grandi cinque dal vertice di Server di Windows</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/">Conteggio e 10.000 dei cluster di spazi di archiviazione diretta...</a></li>
            </ul>
</table>

## Video

**Panoramica Video rapida (5 minuti)**

<iframe src="https://www.youtube-nocookie.com/embed/raeUiNtMk0E" width="560" height="315" allowfullscreen></iframe>

**Spazi di archiviazione diretta al Microsoft Ignite 2018 (1 ora)**

[Guarda YouTube](https://www.youtube.com/watch?v=5kaUiW3qo30)

**Spazi di archiviazione diretta da Microsoft Ignite 2017 (1 ora)**

[Guarda YouTube](https://www.youtube.com/watch?v=YDr2sqNB-3c)

**Avviare l'evento al Microsoft Ignite 2016 (1 ora)**

[Guarda YouTube](https://www.youtube.com/watch?v=-LK2ViRGbWs)

## Principali vantaggi

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Semplicità.</b> Puoi passare dai server standard di settore che eseguono Windows Server 2016 al tuo primo cluster di Spazi di archiviazione diretta in meno di 15 minuti. Per gli utenti di System Center, la distribuzione avviene semplicemente selezionando una casella di controllo.
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
            <b>Tolleranza di errore. </b> La resilienza predefinita gestisce gli errori delle unità, dei server o dei componenti con disponibilità continua. Puoi anche configurare distribuzioni di dimensioni maggiori per la <a href="../../failover-clustering/fault-domains.md">tolleranza di errore di chassis e rack</a>. Quando si verifica un errore hardware, è sufficiente effettuare lo swapping. Il software si ripara da solo senza richiedere operazioni di gestione complesse.
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

## Opzioni di distribuzione

Spazi di archiviazione diretta è stato progettato per due opzioni di distribuzione distinte:

### Con convergenza

**Archiviazione e calcolo in cluster separati.** L'opzione di distribuzione convergente, chiamata anche disaggregata, sovrappone un file server di scalabilità orizzontale (SoFS) a Spazi di archiviazione diretta per offrire un dispositivo NAS in condivisioni file SMB3. In questo modo si ottiene un ridimensionamento del calcolo/carico di lavoro indipendentemente dal cluster di archiviazione, fondamentale per le distribuzioni su larga scala come l'infrastruttura distribuita come servizio (IaaS) Hyper-V per i provider di servizi e le aziende.

![Spazi di archiviazione diretta fornisce l'archiviazione tramite la funzionalità File server di scalabilità orizzontale alle macchine virtuali Hyper-V in un altro server o cluster](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### Iperconvergente

**Un solo cluster per calcolo e archiviazione** L'opzione di distribuzione iperconvergente esegue macchine virtuali Hyper-V o database di SQL Server direttamente nei server fornendo archiviazione e archiviando i file nei volumi locali. In questo modo, non è più necessario configurare l'accesso e le autorizzazioni per il file server e si riducono i costi per l'hardware per le distribuzioni di piccole e medie imprese o uffici remoti/succursali. Vedi [distribuire spazi di archiviazione diretta](deploy-storage-spaces-direct.md).

![Spazi di archiviazione diretta fornisce archiviazione alle macchine virtuali Hyper-V VMs nello stesso cluster](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## Come funziona

Spazi di archiviazione diretta è l'evoluzione di Spazi di archiviazione, funzionalità disponibile per la prima volta in Windows Server 2012. Usa molte delle funzionalità di Windows Server, ad esempio il clustering di failover, il file system del volume condiviso cluster, Server Message Block (SMB) 3 e naturalmente Spazi di archiviazione. Offre anche una nuova tecnologia, il bus di archiviazione software.

L'immagine seguente offre una panoramica dello stack di Spazi di archiviazione diretta:

![Stack di Spazi di archiviazione diretta](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**Hardware di rete.** Spazi di archiviazione diretta usa SMB3, inclusi SMB diretto e SMB multicanale, su Ethernet per la comunicazione tra server. Si consiglia di disporre di almeno 10 GbE con accesso diretto a memoria remota (RDMA), iWARP o RoCE.

**Hardware di archiviazione.** Da 2 a 16 server con unità SATA, SAS o NVMe collegate in locale. Ogni server deve avere almeno 2 unità SSD e almeno 4 unità aggiuntive. I dispositivi SATA e SAS devono essere dietro a una scheda bus host (HBA) e un espansore SAS. Si consiglia di usare le piattaforme accuratamente progettate e ampiamente convalidate dei partner Microsoft (presto disponibili).

**Clustering di failover.** La funzionalità di clustering predefinita di Windows Server viene usata per connettere i server.

**Bus di archiviazione software.** Il bus di archiviazione software è una nuova funzionalità di Spazi di archiviazione diretta. Estende il cluster e imposta un'infrastruttura di archiviazione software-defined per mezzo della quale tutti i server possono vedere tutte le rispettive unità locali. Può essere considerato un'alternativa ai cavi più costosi e limitati Fibre Channel o Shared SAS.

**Cache a livello del bus di archiviazione.** Il bus di archiviazione software associa dinamicamente le unità più veloci (ad esempio, SSD) a quelle più lente (ad esempio, HDDs) per offrire il caching di lettura/scrittura sul lato server che accelera l'IO e incrementa la velocità effettiva.

**Pool di archiviazione.** Il pool di archiviazione è la raccolta di unità che costituiscono la base di Spazi di archiviazione. Il pool viene creato automaticamente e tutte le unità idonee vengono individuate e aggiunte automaticamente al pool. Si consiglia di usare un pool per cluster con le impostazioni predefinite. Per altre informazioni leggi l'[approfondimento sul pool di archiviazione](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

**Spazi di archiviazione.** Spazi di archiviazione fornisce tolleranza di errore ai "dischi" virtuali mediante [mirroring, codifica di cancellazione o entrambi](storage-spaces-fault-tolerance.md). Può essere considerato un RAID software-defined che usa le unità nel pool. In Spazi di archiviazione diretta i dischi virtuali hanno in genere resilienza a due errori simultanei di server o unità (ad esempio, mirroring a tre vie con ciascuna copia dei dati in un server diverso) sebbene sia disponibile anche la tolleranza di errore di chassis e rack.

**Resilient File System (ReFS)** ReFS è il file system specificatamente progettato per la virtualizzazione. Include accelerazioni significative per le operazioni di file con estensione vhdx, ad esempio creazione, espansione, unione di checkpoint e checksum predefiniti per rilevare e correggere gli errori di bit. Offre anche suddivisioni in livelli in tempo reale che ruotano i dati tra livelli di archiviazione "a caldo" e "a freddo" in tempo reale in base all'utilizzo.

**Volumi condivisi cluster.** Il file system CSV unisce tutti i volumi ReFS in un unico spazio dei nomi accessibile tramite qualsiasi server, in modo che in ogni server ogni volume agisce come se fosse montato in locale.

**File server di scalabilità orizzontale.** Questo livello finale è necessario solo per le distribuzioni convergenti. Offre accesso remoto ai file tramite il protocollo di accesso SMB3 ai client, ad esempio a un altro cluster che esegue Hyper-V in rete, trasformando Spazi di archiviazione diretta in dispositivo NAS.

## Study

Ci sono [più di 10.000 cluster](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/) tutto il mondo in esecuzione spazi di archiviazione diretta. Le organizzazioni di tutte le dimensioni, da piccole aziende distribuzione solo due nodi, per le aziende di grandi dimensioni e governi distribuzione centinaia di nodi, dipendono da spazi di archiviazione diretta per applicazioni critiche e dell'infrastruttura.

Visita [Microsoft.com/HCI](https://www.microsoft.com/hci) per leggere le storie:

[![Geliminare i logo cliente](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## Strumenti di gestione

Gli strumenti seguenti possono essere usati per gestire e/o monitora spazi di archiviazione diretta:

| Nome | Grafica o della riga di comando? | A pagamento o inclusa? |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | Grafica    | Inclusa |
| Gestione Cluster di Failover & Server Manager                                 | Grafica    | Inclusa |
| Windows PowerShell                                                        | Riga di comando | Inclusa |
| [System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) & [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | Grafica    | A pagamento     |

## Per iniziare

Prova Spazi di archiviazione diretta [in Microsoft Azure](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/) o scarica una copia di valutazione con licenza di 180 giorni di Windows Server da [Windows Server Valutazioni](https://go.microsoft.com/fwlink/?linkid=842602).

## Vedi anche

-   [Tolleranza di errore ed efficienza di archiviazione](storage-spaces-fault-tolerance.md)
-   [Replica di archiviazione](../storage-replica/storage-replica-overview.md)
-   [Archiviazione blog Microsoft](https://blogs.technet.microsoft.com/filecab/)
-   [Storage Spaces Direct throughput with iWARP](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp) (Velocità effettiva di Spazi di archiviazione diretta con iWARP) (blog di TechNet)
-   [Novità del clustering di failover in Windows Server](../../failover-clustering/whats-new-in-failover-clustering.md)  
-   [QoS di archiviazione](../storage-qos/storage-qos-overview.md)
-   [Supporto per Windows IT Pro](https://www.microsoft.com/itpro/windows/support)
