---
title: Gestire i cluster di Failover con Windows Admin Center
description: Gestire i cluster di Failover con Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7e14581f7f6b14b0cf39308de236b68a07e8c9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824072"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>Gestire i cluster di Failover con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## <a name="managing-failover-clusters"></a>Gestione dei cluster di failover
[Clustering di failover](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) è una funzionalità di Windows Server che consente di raggruppare più server in un cluster a tolleranza di errore per aumentare la disponibilità e scalabilità delle applicazioni e servizi, ad esempio Scale-Out File Server, Hyper-V e Microsoft SQL Server.

Anche se è possibile gestire i nodi del cluster come server singoli aggiungendole come [le connessioni al Server](manage-servers.md) in Windows Admin Center, è anche possibile aggiungere tali come cluster di Failover per visualizzare e gestire le risorse del cluster, archiviazione, rete, i nodi, i ruoli, le macchine virtuali e commutatori virtuali.

![Schermata di panoramica del cluster di failover](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>Aggiunta di un cluster di failover di Windows Admin Center
Per aggiungere un cluster a Windows Admin Center:

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere un **connessione di Failover**.
3. Digitare il nome del cluster e, se richiesto, le credenziali da utilizzare.
4. Si avrà la possibilità di aggiungere i nodi del cluster come connessioni a server singolo in Windows Admin Center.
5. Fare clic su **Submit** alla fine.

Il cluster verrà aggiunto all'elenco di connessione nella pagina di panoramica. Fare clic per connettersi al cluster.

> [!NOTE]
> È anche possibile gestire iperconvergente in cluster mediante l'aggiunta del cluster come un [connessione Cluster Hyper-Converged](manage-hyper-converged.md) in Windows Admin Center.

## <a name="tools"></a>Strumenti

Gli strumenti seguenti sono disponibili per le connessioni del cluster di failover:

| Strumento | Descrizione |
| ---- | ----------- |
| Panoramica | Visualizzare i dettagli dei cluster di failover e gestire le risorse del cluster |
| Dischi | Condivisi del cluster Visualizza i dischi e volumi |
| Reti | Visualizzare le reti del cluster |
| Nodi | Visualizzare e gestire i nodi del cluster |
| Ruoli | Gestire i ruoli del cluster o creare un ruolo vuoto |
| Aggiornamenti | Gestire gli aggiornamenti compatibile con Cluster |
| [Macchine virtuali](manage-virtual-machines.md) | Visualizzare e gestire le macchine virtuali |
| Commutatori virtuali | Visualizzare e gestire i commutatori virtuali |

## <a name="more-coming"></a>Ulteriori provenienti

Gestione cluster di failover in Windows Admin Center è attivamente in fase di sviluppo e verranno aggiunte nuove funzionalità nel prossimo futuro. In UserVoice, è possibile visualizzare lo stato e votare funzionalità:

|Richiesta di funzionalità|
|-------|
| [Mostra più informazioni sul disco del cluster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Supportare le azioni cluster aggiuntivi](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Supporto per i cluster convergente che eseguono Hyper-V e File Server di scalabilità orizzontale in cluster diversi](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Cache del blocco di visualizzazione CSV](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Vedi tutto o proporre nuove funzionalità](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |