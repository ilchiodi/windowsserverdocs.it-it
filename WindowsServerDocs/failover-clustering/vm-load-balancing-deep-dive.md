---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Approfondimento il bilanciamento del carico macchina virtuale
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 6ee092a2e51e2181a2203bc263377c4a8ec60246
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Approfondimento il bilanciamento del carico macchina virtuale

> Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Windows Server 2016 introduce il [funzionalità di bilanciamento del carico macchina virtuale](vm-load-balancing-overview.md) per ottimizzare l'utilizzo di nodi in un Cluster di Failover. Questo documento viene descritto come configurare e controllare <abbr title="macchina virtuale">VM</abbr> il bilanciamento del carico. 

## <a id="heuristics-for-balancing"></a>Euristica per bilanciamento del carico
<abbr title="Macchina virtuale">VM</abbr> il bilanciamento del carico macchina virtuale valuta il caricamento di un nodo in base all'euristica seguenti:
1. Corrente **utilizzo di memoria**: la memoria è la limitazione delle risorse più comune in un host Hyper-V
2. <abbr title="CPU">CPU</abbr> **utilizzo** del nodo Media di una finestra di 5 minuti: consente di prevenire un nodo del cluster diventare overcommit

## <a id="controlling-aggressiveness-of-balancing"></a>Controllare l'aggressività di bilanciamento del carico
L'aggressività di bilanciamento del carico in base all'euristica di memoria e CPU possono essere configurate tramite dalla proprietà comune cluster 'AutoBalancerLevel'. Per controllare l'aggressività eseguire il comando seguente in PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Aggressività | Comportamento |
|-------------------|----------------|----------|
| 1 (impostazione predefinita) | Basso | Spostare quando host superiore all'80% caricata |
| 2 | Media | Spostare quando l'host è superiore al 70% caricato |
| 3 | Elevata | Spostare quando l'host è oltre il 60% caricato | 

![Immagine di un PowerShell di configurazione di bilanciamento del carico l'aggressività](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-abbr-titlevirtual-machinevmabbr-load-balancing"></a>Controllo <abbr title="macchina virtuale">VM</abbr> il bilanciamento del carico
<abbr title="Macchina virtuale">VM</abbr> il bilanciamento del carico è abilitata per impostazione predefinita e quando si verifica il bilanciamento del carico può essere configurato per la proprietà comune cluster 'AutoBalancerMode'. Per controllare quando nodo equità bilancia cluster:

### <a name="using-failover-cluster-manager"></a>Utilizzando Gestione Cluster di Failover:
1. Fare doppio clic sul nome del cluster e selezionare l'opzione "Proprietà"  
    ![Grafico di selezione delle proprietà per il cluster tramite Gestione Cluster di Failover](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Selezionare il riquadro "Bilanciamento"  
    ![Grafico di selezione dell'opzione di bilanciamento del carico tramite Gestione Cluster di Failover](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Utilizzo di PowerShell:
Eseguire le operazioni seguenti:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamento| 
|:----------------:|:----------:|
|0| Disabilitato| 
|1| Bilanciamento del carico in aggiunta al nodo| 
|2 (predefinito)| Bilanciare in aggiunta al nodo e ogni 30 minuti |

## <a name="abbr-titlevirtual-machinevmabbr-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a><abbr title="Macchina virtuale">VM</abbr> il bilanciamento del carico e System Center Virtual Machine Manager dinamico ottimizzazione
La funzionalità equità nodo, fornisce funzionalità nella casella che è destinata agli distribuzioni senza System Center Virtual Machine Manager (<abbr title="System Center Virtual Machine Manager">SCVMM</abbr>). <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> ottimizzazione dinamica è il metodo consigliato per il bilanciamento del carico delle macchine virtuali del cluster per <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> le distribuzioni. <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> disattiva automaticamente il Server Windows <abbr title="macchina virtuale">VM</abbr> quando è abilitata l'ottimizzazione dinamica il bilanciamento del carico.

## <a name="see-also"></a>Vedere anche
* [Panoramica di bilanciamento carico di macchina virtuale](vm-load-balancing-overview.md)
* [Clustering di failover](failover-clustering-overview.md)
* [Panoramica di Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
