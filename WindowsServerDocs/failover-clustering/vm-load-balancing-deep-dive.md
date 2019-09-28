---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Approfondimento sul bilanciamento del carico della macchina virtuale
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 972e86d5f49f92d090eed1d4130544d0269c1309
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392224"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Approfondimento sul bilanciamento del carico della macchina virtuale

> Si applica a: Windows Server 2019, Windows Server 2016

La [funzionalità di bilanciamento del carico della macchina virtuale](vm-load-balancing-overview.md) consente di ottimizzare l'utilizzo dei nodi in un cluster di failover. Questo documento descrive come configurare e controllare il bilanciamento del carico delle macchine virtuali. 

## <a id="heuristics-for-balancing"></a>Euristica per il bilanciamento
Il bilanciamento del carico della macchina virtuale valuta il carico di un nodo in base all'euristica seguente:
1. Utilizzo corrente della **memoria**: La memoria è il vincolo di risorse più comune in un host Hyper-V
2. **Utilizzo** della CPU del nodo medio in un intervallo di 5 minuti: Attenua un nodo nel cluster che sta per essere sottoposta a overcommit

## <a id="controlling-aggressiveness-of-balancing"></a>Controllo dell'aggressività del bilanciamento
L'aggressività del bilanciamento basato sulla memoria e sull'euristica della CPU può essere configurata usando la proprietà comune del cluster ' AutoBalancerLevel '. Per controllare l'aggressività, eseguire quanto segue in PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Aggressività | Comportamento |
|-------------------|----------------|----------|
| 1 (predefinito) | Bassa | Sposta quando l'host è caricato più del 80% |
| 2 | Medio | Sposta quando l'host è caricato più del 70% |
| 3 | Alto | Media dei nodi e spostamento quando l'host supera la media del 5% | 

![Rappresentazione grafica di un PowerShell di configurazione dell'aggressività del bilanciamento](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>Controllo del bilanciamento del carico delle VM
Il bilanciamento del carico della macchina virtuale è abilitato per impostazione predefinita e quando si verifica il bilanciamento del carico può essere configurato dalla proprietà comune del cluster ' AutoBalancerMode '. Per controllare quando l'equità del nodo bilancia il cluster:

### <a name="using-failover-cluster-manager"></a>Utilizzando Gestione cluster di failover:
1. Fare clic con il pulsante destro del mouse sul nome del cluster e selezionare l'opzione "proprietà"  
    ![Rappresentazione grafica della selezione della proprietà per il cluster tramite Gestione cluster di failover](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Selezionare il riquadro "bilanciamento"  
    ![Rappresentazione grafica della selezione dell'opzione di bilanciamento tramite Gestione cluster di failover](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Usando PowerShell:
Eseguire quanto segue:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamento| 
|:----------------:|:----------:|
|0| Disabilitata| 
|1| Bilanciamento del carico sul join del nodo| 
|2 (impostazione predefinita)| Bilanciamento del carico sul join del nodo e ogni 30 minuti |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>Bilanciamento del carico delle VM rispetto a Ottimizzazione dinamica System Center Virtual Machine Manager
La funzionalità di equità dei nodi fornisce funzionalità predefinite, destinate a distribuzioni senza System Center Virtual Machine Manager (SCVMM). L'ottimizzazione dinamica SCVMM è il meccanismo consigliato per il bilanciamento del carico delle macchine virtuali nel cluster per le distribuzioni SCVMM. SCVMM Disabilita automaticamente il bilanciamento del carico delle macchine virtuali Windows Server quando è abilitata l'ottimizzazione dinamica.

## <a name="see-also"></a>Vedere anche
* [Panoramica del bilanciamento del carico della macchina virtuale](vm-load-balancing-overview.md)
* [Clustering di failover](failover-clustering-overview.md)
* [Panoramica di Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
