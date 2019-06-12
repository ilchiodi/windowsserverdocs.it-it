---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Clustering di failover
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: high
ms.openlocfilehash: a7f6dd847d2762dbc616189ed729449479788f98
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810907"
---
# <a name="failover-clustering-in-windows-server"></a>Clustering di failover in Windows Server

> Si applica a: Windows Server 2019, Windows Server 2016

Un cluster di failover è costituito da un gruppo di computer indipendenti che interagiscono tra di loro per migliorare la disponibilità e la scalabilità dei ruoli cluster, noti in precedenza come servizi e applicazioni nel cluster. I server inclusi nel cluster (detti nodi) sono connessi mediante cavi fisici e software. Se in uno o più nodi del cluster si verifica un errore, il servizio verrà garantito dagli altri nodi (un processo noto come failover). I ruoli cluster, inoltre, vengono monitorati in modo proattivo per verificare che funzionino correttamente. Se non funzionano, vengono riavviati o spostati in un altro nodo.

I cluster di failover includono inoltre la funzionalità Volume condiviso del cluster, che fornisce uno spazio dei nomi uniforme e distribuito che può essere utilizzato dai ruoli del cluster per accedere allo spazio di archiviazione condiviso da tutti i nodi. Con la funzionalità Clustering di failover, per gli utenti l'interruzione del servizio risulterà minima.

Clustering di failover offre numerose applicazioni pratiche, tra cui:

* Archiviazione su condivisione file con disponibilità continua ed elevata per applicazioni come Microsoft SQL Server e macchine virtuali Hyper-V
* Ruoli cluster a disponibilità elevata eseguiti su server fisici o su macchine virtuali installate in server che eseguono Hyper-V

| **Comprendere**                                                               |  **Pianificazione**                          |  **Distribuzione**       |
| -------------                                                                |  --------------                        | --------------------- |
| [Novità del clustering di failover](whats-new-in-failover-clustering.md)    | [Pianificazione Failover Clustering Hardware Requirements and Storage Options](clustering-requirements.md)  | [Creazione di un Cluster di Failover](create-failover-cluster.md) |
| [File server di scalabilità orizzontale per dati delle applicazioni](sofs-overview.md)               | [Usare Volumi condivisi cluster](failover-cluster-csvs.md) | [Distribuire un file server a due nodi](../storage/storage-spaces/storage-spaces-direct-in-vm.md) |
|  [Quorum di cluster e pool](../storage/storage-spaces/understand-quorum.md)   |  [Uso di cluster di macchine virtuali guest con spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-in-vm.md)       | [Pre-installare oggetti computer del cluster in servizi di dominio Active Directory](prestage-cluster-adds.md) |
| [Riconoscimento dei domini di errore](fault-domains.md)                                 |                                 | [Configurazione di account di cluster in Active Directory](configure-ad-accounts.md) |
| [SMB multicanale semplificato e reti di cluster Multi-NIC](smb-multichannel.md) |                       | [Gestire il quorum e i server di controllo](manage-cluster-quorum.md) |
| [Bilanciamento del carico delle macchine virtuali](vm-load-balancing-overview.md)                         |                             | [Distribuire un cloud di controllo](deploy-cloud-witness.md) |
| [Set di cluster](../storage/storage-spaces/cluster-sets.md)                  |                             |[Distribuire una condivisione file di controllo](file-share-witness.md) |
| [Affinità del cluster](cluster-affinity.md)                                     |                            | [Aggiornamenti in sequenza del sistema operativo del cluster](cluster-operating-system-rolling-upgrade.md) |
|                                                                             |                            | [Aggiornamento di un cluster di failover nello stesso hardware](upgrade-option-same-hardware.md) |
|                                                                            |                             | [Distribuire un Cluster scollegato Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\))

|**Gestione**  |  **Strumenti e impostazioni**  |  **Risorse della community**       |
| ------------- |  -------------- | --------------------- |
| [Aggiornamento compatibile con cluster](cluster-aware-updating.md)    |   [Cmdlet di PowerShell di Clustering di failover](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)      |  [Forum dedicato alla disponibilità elevata (clustering)](https://go.microsoft.com/fwlink/p/?LinkId=230641)       |
|  [Servizio integrità](health-service-overview.md)   |   [Cmdlet di PowerShell ad aggiornamento compatibile con cluster](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)      | [Clustering di failover e carico di rete blog del Team di bilanciamento del carico](http://blogs.msdn.com/b/clustering/)        |
|  [Migrazione del dominio del cluster](cluster-domain-migration.md)   |         |         |
|  [Risoluzione dei problemi con Segnalazione errori Windows](troubleshooting-using-wer-reports.md)   |         |         |
