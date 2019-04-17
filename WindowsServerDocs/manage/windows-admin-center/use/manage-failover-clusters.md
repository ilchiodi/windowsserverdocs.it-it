---
title: Gestire i cluster di Failover con Windows Admin Center
description: Gestire i cluster di Failover con Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 63f5fca8e7ef63200e01cfc6e00a979e7f045b51
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296673"
---
# Gestire i cluster di Failover con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## Gestione dei cluster di failover
[Clustering di failover](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) è una funzionalità di Windows Server che consente di raggruppare più server in un cluster a tolleranza di errore per aumentare la disponibilità e scalabilità di applicazioni e servizi, ad esempio File Server di scalabilità orizzontale, Hyper-V e Microsoft SQL Server.

Anche se è possibile gestire i nodi del cluster di failover come server singoli aggiungendoli come [le connessioni Server](manage-servers.md) in Windows Admin Center, puoi anche aggiungere loro come cluster di Failover per visualizzare e gestire le risorse del cluster, archiviazione, rete, i nodi, ruoli, virtuali computer e commutatori virtuali.

![Schermata di panoramica del cluster di failover](../media/manage-failover-clusters/fcm-overview.png)

## Aggiunta di un cluster di failover di Windows Admin Center
Per aggiungere un cluster a Windows Admin Center:

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere una **Connessione di Failover**.
3. Digita il nome del cluster e, se richiesto, le credenziali da utilizzare.
4. Hai la possibilità di aggiungere i nodi del cluster come le connessioni server singoli in Windows Admin Center.
5. Fai clic su **Invia** alla fine.

Il cluster verrà aggiunto all'elenco di connessione nella pagina della panoramica. Fai clic per connettersi al cluster.

> [!NOTE]
> È inoltre possibile gestire hyper-converged cluster tramite l'aggiunta al cluster come [connessione Cluster iperconvergente](manage-hyper-converged.md) in Windows Admin Center.

## Strumenti

Gli strumenti seguenti sono disponibili per le connessioni cluster di failover:

| Strumento | Descrizione |
| ---- | ----------- |
| Panoramica | Visualizzare i dettagli del cluster di failover e gestire le risorse del cluster |
| Dischi | I dischi e volumi condivisi del cluster di visualizzazione |
| Reti | Visualizzare le reti del cluster |
| Nodi | Visualizzare e gestire i nodi del cluster |
| Ruoli | Gestire i ruoli del cluster o creare un ruolo vuoto |
| Aggiornamenti | Gestire gli aggiornamenti compatibile con Cluster (richiede [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)) |
| [Macchine virtuali](manage-virtual-machines.md) | Visualizzare e gestire le macchine virtuali |
| Switch virtuali | Visualizzazione e gestione commutatori virtuali |

## Altre prossime

Gestione cluster di failover in Windows Admin Center è attivamente in fase di sviluppo e verranno aggiunte nuove funzionalità in futuro. Visualizzare lo stato e vota per funzionalità UserVoice:

|Richiesta di funzionalità|
|-------|
| [Visualizzare più informazioni disco del cluster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Supporta azioni aggiuntive del cluster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Supporto per la convergenza dei cluster che esegue Hyper-V e File Server di scalabilità orizzontale in cluster diversi](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Visualizzazione CSV blocco cache](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Vedi tutti o proporre nuove funzionalità](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |