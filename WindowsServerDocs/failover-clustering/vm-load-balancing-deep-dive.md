---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Il bilanciamento del carico di macchine virtuali-approfondimenti
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 50213cf47c2c59f1775ae704e82ed51794715ac0
ms.sourcegitcommit: 276a480b470482cba4682caa3df4cd07ba5b7801
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66198556"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Il bilanciamento del carico di macchine virtuali-approfondimenti

> Si applica a: Windows Server 2019, Windows Server 2016

Il [funzionalità di bilanciamento del carico macchina virtuale](vm-load-balancing-overview.md) Ottimizza l'utilizzo di nodi in un Cluster di Failover. Questo documento descrive come configurare e controllare il bilanciamento del carico della macchina virtuale. 

## <a id="heuristics-for-balancing"></a>Euristica per bilanciamento del carico
Il bilanciamento del carico macchina virtuale valuta il carico di un nodo in base l'euristica seguente:
1. Corrente **richieste di memoria**: La memoria è la limitazione delle risorse più comune in un host Hyper-V
2. CPU **utilizzo** del nodo come medio di una finestra di 5 minuti: Consente di ridurre un nodo del cluster sta diventando overcommit

## <a id="controlling-aggressiveness-of-balancing"></a>Controllare l'aggressività di bilanciamento del carico
L'aggressività di bilanciamento del carico in base all'euristica di memoria e CPU può essere configurato usando il riferimento la proprietà comuni di cluster 'AutoBalancerLevel'. Per controllare l'aggressività eseguire il comando seguente in PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Aggressività | Comportamento |
|-------------------|----------------|----------|
| 1 (predefinito) | Bassa | Spostare quando l'host è caricata più di 80% |
| 2 | Medio | Sposta quando l'host è caricato più di 70% |
| 3 | Alto | Medio di nodi e spostare quando l'host si trova più del 5% maggiore della media | 

![Rappresentazione grafica di un PowerShell di configurazione l'aggressività di bilanciamento del carico](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>Controllo di bilanciamento del carico della macchina virtuale
Il bilanciamento del carico della macchina virtuale è abilitata per impostazione predefinita e quando si verifica il bilanciamento del carico può essere configurato per la proprietà comune del cluster 'AutoBalancerMode'. Per controllare quando l'equità dei nodi consente di bilanciare il cluster:

### <a name="using-failover-cluster-manager"></a>Usando Gestione Cluster di Failover:
1. Fare clic sul nome del cluster e selezionare l'opzione "Proprietà"  
    ![Rappresentazione grafica della selezione di proprietà per il cluster tramite Gestione Cluster di Failover](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Selezionare il riquadro "Servizio di bilanciamento"  
    ![Grafico di selezione dell'opzione di bilanciamento del carico tramite Gestione Cluster di Failover](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Utilizzo di PowerShell:
Eseguire il comando seguente:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamento| 
|:----------------:|:----------:|
|0| Disabled| 
|1| Bilanciare il carico sull'aggiunta di nodi| 
|2 (predefinito)| Bilanciare sull'aggiunta di nodo e ogni 30 minuti |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>Visual Studio il bilanciamento del carico della macchina virtuale. System Center Virtual Machine Manager Dynamic Optimization
La funzionalità di equità di nodo, fornisce funzionalità in arrivo, che viene indirizzata verso le distribuzioni senza System Center Virtual Machine Manager (SCVMM). Ottimizzazione dinamica SCVMM è il metodo consigliato per il bilanciamento del carico delle macchine virtuali nel cluster per le distribuzioni di SCVMM. SCVMM si disabilita il bilanciamento di carico di macchine Virtuali Server Windows quando è abilitata l'ottimizzazione dinamica.

## <a name="see-also"></a>Vedere anche
* [Panoramica del bilanciamento del carico delle macchine virtuali](vm-load-balancing-overview.md)
* [Clustering di failover](failover-clustering-overview.md)
* [Panoramica di Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
