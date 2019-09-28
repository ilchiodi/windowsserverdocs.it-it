---
title: Gestire i cluster di failover con l'interfaccia di amministrazione di Windows
description: Gestire i cluster di failover con l'interfaccia di amministrazione di Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 16e758f0a8746d41adcdafb2bc1be2d91a3fc29c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406808"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>Gestire i cluster di failover con l'interfaccia di amministrazione di Windows

>Si applica a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## <a name="managing-failover-clusters"></a>Gestione dei cluster di failover
Il [clustering di failover](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) è una funzionalità di Windows Server che consente di raggruppare più server in un cluster a tolleranza di errore per aumentare la disponibilità e la scalabilità di applicazioni e servizi, ad esempio file server di scalabilità orizzontale, Hyper-V e Microsoft SQL Server.

Sebbene sia possibile gestire i nodi del cluster di failover come singoli server aggiungendoli come [connessioni server](manage-servers.md) nell'interfaccia di amministrazione di Windows, è anche possibile aggiungerli come cluster di failover per visualizzare e gestire risorse cluster, archiviazione, rete, nodi, ruoli, virtuali computer e commutatori virtuali.

![Schermata di panoramica del cluster di failover](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>Aggiunta di un cluster di failover all'interfaccia di amministrazione di Windows
Per aggiungere un cluster al centro di amministrazione di Windows:

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere una **connessione di failover**.
3. Digitare il nome del cluster e, se richiesto, le credenziali da usare.
4. Si avrà la possibilità di aggiungere i nodi del cluster come singole connessioni server nell'interfaccia di amministrazione di Windows.
5. Fare clic su **Invia** per terminare.

Il cluster verrà aggiunto all'elenco di connessioni nella pagina panoramica. Fare clic su di esso per connettersi al cluster.

> [!NOTE]
> È anche possibile gestire il cluster iperconvergente aggiungendo il cluster come [connessione cluster iperconvergente nell'interfaccia](manage-hyper-converged.md) di amministrazione di Windows.

## <a name="tools"></a>Strumenti

Per le connessioni del cluster di failover sono disponibili gli strumenti seguenti:

| Strumento | Descrizione |
| ---- | ----------- |
| Panoramica | Visualizzare i dettagli del cluster di failover e gestire le risorse cluster |
| Dischi | Visualizzazione di volumi e dischi condivisi del cluster |
| Reti | Visualizzare le reti nel cluster |
| Nodi | Visualizzare e gestire i nodi del cluster |
| Ruoli | Gestire i ruoli del cluster o creare un ruolo vuoto |
| Aggiornamenti | Gestire gli aggiornamenti compatibili con cluster (richiede [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)) |
| [Macchine virtuali](manage-virtual-machines.md) | Visualizzare e gestire le macchine virtuali |
| Commutatori virtuali | Visualizzare e gestire i commutatori virtuali |

## <a name="more-coming"></a>Ulteriori informazioni

La gestione dei cluster di failover nell'interfaccia di amministrazione di Windows è attivamente in fase di sviluppo e le nuove funzionalità verranno aggiunte a breve in futuro. È possibile visualizzare lo stato e votare le funzionalità in UserVoice:

|Richiesta di funzionalità|
|-------|
| [Mostra altre informazioni sul disco del cluster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Supportare azioni cluster aggiuntive](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Supporto di cluster convergenti che eseguono Hyper-V e File server di scalabilità orizzontale in cluster diversi](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Visualizza cache di blocco CSV](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Visualizza tutto o Suggerisci nuova funzionalità](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |