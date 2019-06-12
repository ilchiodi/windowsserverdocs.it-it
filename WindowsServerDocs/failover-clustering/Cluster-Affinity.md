---
title: Affinità di cluster
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 03/07/2019
description: Questo articolo descrive i livelli di affinità e antiAffinity cluster di failover
ms.openlocfilehash: 67929e6d3399633ebfec0b908463131973aecaf7
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453032"
---
# <a name="cluster-affinity"></a>Affinità di cluster

> Si applica a: Windows Server 2019, Windows Server 2016

Un cluster di failover può contenere numerosi ruoli che possono spostarsi tra i nodi ed eseguite.  Esistono situazioni in cui determinati ruoli (ad esempio le macchine virtuali, i gruppi di risorse e così via) non deve essere eseguita nello stesso nodo.  È possibile a causa dell'utilizzo delle risorse, utilizzo della memoria e così via.  Ad esempio, sono disponibili due macchine virtuali che sono a elevato utilizzo di CPU e memoria e se le due macchine virtuali sono in esecuzione nello stesso nodo, una o entrambe le macchine virtuali potrebbe avere problemi di impatto sulle prestazioni.  Questo articolo vengono descritti i livelli di antiaffinity e come è possibile usarli cluster.

## <a name="what-is-affinity-and-antiaffinity"></a>Che cos'è l'affinità e AntiAffinity?

L'affinità è una regola si impostano di solito che stabilisce una relazione tra due o più ruoli (i, e, le macchine virtuali, i gruppi di risorse, e così via) per mantenerli.  AntiAffinity è lo stesso ma serve a provare e mantenere i ruoli specificati separatamente tra loro.  I cluster di failover usano AntiAffinity per relativi ruoli.  In particolare, il [AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames) parametro definito nei ruoli in modo che non vengono eseguite nello stesso nodo.  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

Quando si esaminano le proprietà di un gruppo, è disponibile il parametro AntiAffinityClassNames ed è vuota per impostazione predefinita.  Negli esempi seguenti, Gruppo1 e gruppo2 devono essere separate dall'esecuzione nello stesso nodo.  Per visualizzare le proprietà, il comando di PowerShell e il risultato sarà:

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

Poiché AntiAffinityClassNames non sono definiti per impostazione predefinita, questi possono ruoli eseguiti insieme o distanti.  L'obiettivo è mantenere la separazione.  Il valore per AntiAffinityClassNames può essere ciò che si vuole che siano, hanno solo essere lo stesso.  Si supponga che GRUPPO1 e gruppo2 sono controller di dominio in esecuzione in macchine virtuali e potrebbe essere soddisfatti in modo ottimale in esecuzione in nodi diversi.  Poiché questi sono i controller di dominio, si utilizzerà controller di dominio per il nome della classe.  Per impostare il valore, il comando di PowerShell e i risultati sarebbero:

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

Ora che sono impostati, verrà effettuato un tentativo di mantenerli distanti clustering di failover.  

Il parametro di AntiAffinityClassName è un blocco "soft".  Vale a dire, verrà effettuato un tentativo per mantenerli distanti, ma se non è possibile, comunque consentirà loro di eseguire sullo stesso nodo.  Ad esempio, i gruppi sono in esecuzione in un cluster di failover a due nodi.  Se un nodo deve passare a discesa per la manutenzione, ciò significherebbe che entrambi i gruppi sarà attivo e in esecuzione nello stesso nodo.  In questo caso, sarebbe accettabile per questa.  Potrebbe non essere la soluzione più ideale, ma entrambi i computer virtial saranno ancora in esecuzione all'interno degli intervalli di prestazioni accettabili.

## <a name="i-need-more"></a>È necessario altro ancora

Come accennato, AntiAffinityClassNames è un blocco soft.  Ma cosa accade se è necessario un blocco hard?  Le macchine virtuali non può essere eseguite su egli stesso nodo. impatto sulle prestazioni verrà in caso contrario, si verificano e causare alcuni servizi eventualmente diminuisce.

Per questi casi, è disponibile una proprietà di cluster aggiuntiva di ClusterEnforcedAntiAffinity.  Questo livello antiaffinity impedirà a tutti i costi di uno qualsiasi degli stessi valori AntiAffinityClassNames in esecuzione nello stesso nodo.

Per visualizzare le proprietà e il valore, il comando di PowerShell (e result) sarebbe:

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

Il valore "0" indica che è disabilitato e non possano essere applicate.  Il valore "1" lo abilita ed è il blocco di disco rigido.  Per abilitare questo blocco di disco rigido, il comando (e result) è:

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

Quando entrambi sono impostate, il gruppo verrà impedito assemblati in linea.  Se sono nello stesso nodo, si tratta di quella che si vedrebbe in Gestione Cluster di Failover.

![Affinità di cluster](media/Cluster-Affinity/Cluster-Affinity-1.png)

In un elenco di PowerShell dei gruppi, si vedrà in questo:

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>Commenti aggiuntivi

- Assicurarsi di usare l'impostazione AntiAffinity appropriata a seconda delle esigenze.
- Tenere presente che in uno scenario di due nodi e ClusterEnforcedAntiAffinity, se un nodo è inattivo, entrambi i gruppi non verrà eseguita.  

- L'uso di proprietari preferiti i gruppi può essere combinato con AntiAffinity in un cluster a tre o più nodi.
- Le impostazioni AntiAffinityClassNames e ClusterEnforcedAntiAffinity avrà luogo solo dopo un riciclo delle risorse. I.E. è possibile impostare, ma se entrambi i gruppi siano online nello stesso nodo se è impostata, entrambi continueranno a rimanere online.



