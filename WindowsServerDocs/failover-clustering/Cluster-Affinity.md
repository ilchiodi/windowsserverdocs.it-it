---
title: Affinità cluster
ms.prod: windows-server
manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 03/07/2019
description: Questo articolo descrive i livelli di affinità e affinità del cluster di failover
ms.openlocfilehash: c9910cac602802b753391fad1009fb7f1fa3d2f2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828284"
---
# <a name="cluster-affinity"></a>Affinità cluster

> Si applica a: Windows Server 2019, Windows Server 2016

Un cluster di failover può ospitare numerosi ruoli che possono spostarsi tra i nodi ed eseguire.  In alcuni casi alcuni ruoli, ad esempio macchine virtuali, gruppi di risorse e così via, non devono essere eseguiti nello stesso nodo.  Questo problema può essere dovuto all'utilizzo delle risorse, all'utilizzo della memoria e così via.  Esistono, ad esempio, due macchine virtuali con utilizzo intensivo di memoria e CPU e se le due macchine virtuali sono in esecuzione nello stesso nodo, una o entrambe le macchine virtuali potrebbero avere problemi di conseguenze sulle prestazioni.  In questo articolo vengono illustrati i livelli di antiaffinità del cluster e il modo in cui è possibile usarli.

## <a name="what-is-affinity-and-antiaffinity"></a>Che cos'è l'affinità e l'affinità?

L'affinità è una regola da configurare che stabilisce una relazione tra due o più ruoli (i, e, macchine virtuali, gruppi di risorse e così via) per mantenerli insieme.  L'affinità è la stessa, ma viene usata per provare a evitare che i ruoli specificati siano separati tra loro.  I cluster di failover usano l'affinità per i ruoli.  In particolare, il parametro [AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames) definito nei ruoli in modo che non vengano eseguiti nello stesso nodo.  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

Quando si esaminano le proprietà di un gruppo, è disponibile il parametro AntiAffinityClassNames ed è vuoto come valore predefinito.  Negli esempi seguenti, group1 e group2 devono essere separati dall'esecuzione nello stesso nodo.  Per visualizzare la proprietà, il comando di PowerShell e il risultato sono:

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

Poiché AntiAffinityClassNames non sono definiti come predefiniti, questi ruoli possono essere eseguiti insieme o separatamente.  L'obiettivo è mantenerli separati.  Il valore di AntiAffinityClassNames può essere quello desiderato, ma è necessario che sia lo stesso.  Si affermi che group1 e group2 sono controller di dominio in esecuzione in macchine virtuali e che verrebbero serviti in modo ottimale in altri nodi.  Poiché si tratta di controller di dominio, utilizzerò DC per il nome della classe.  Per impostare il valore, il comando e i risultati di PowerShell sono:

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

Ora che sono stati impostati, il clustering di failover tenterà di mantenerli separati.  

Il parametro AntiAffinityClassName è un blocco "soft".  Il che significa che tenta di mantenerli separati, ma se non è possibile, ne consentirà comunque l'esecuzione nello stesso nodo.  Ad esempio, i gruppi sono in esecuzione in un cluster di failover a due nodi.  Se un nodo deve essere disattivato per la manutenzione, significa che entrambi i gruppi sarebbero operativi nello stesso nodo.  In questo caso, sarebbe giusto.  Potrebbe non essere il più ideale, ma entrambe le macchine virtial continueranno a funzionare entro intervalli di prestazioni accettabili.

## <a name="i-need-more"></a>Sono necessarie altre informazioni

Come indicato in precedenza, AntiAffinityClassNames è un blocco soft.  Che cosa accade se è necessario un blocco rigido?  Le macchine virtuali non possono essere eseguite nello stesso nodo; in caso contrario, si verificherà un problema di prestazioni e alcuni servizi potrebbero risultare inattivi.

In questi casi, esiste una proprietà cluster aggiuntiva di ClusterEnforcedAntiAffinity.  Questo livello di antiaffinità impedirà a tutti i costi che tutti gli stessi valori di AntiAffinityClassNames vengano eseguiti nello stesso nodo.

Per visualizzare la proprietà e il valore, il comando di PowerShell (e il risultato) sarà:

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

Il valore "0" indica che è disabilitato e non deve essere applicato.  Il valore "1" lo Abilita ed è il blocco rigido.  Per abilitare questo blocco rigido, il comando (e il risultato) è:

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

Quando entrambe le impostazioni sono impostate, al gruppo verrà impedito di tornare online.  Se si trovano nello stesso nodo, questo è ciò che viene visualizzato in Gestione cluster di failover.

![Affinità cluster](media/Cluster-Affinity/Cluster-Affinity-1.png)

In un elenco di PowerShell dei gruppi si noterà quanto segue:

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>Commenti aggiuntivi

- Assicurarsi di usare l'impostazione di antiaffinità appropriata a seconda delle esigenze.
- Tenere presente che in uno scenario a due nodi e ClusterEnforcedAntiAffinity, se un nodo è inattivo, entrambi i gruppi non vengono eseguiti.  

- L'uso dei proprietari preferiti nei gruppi può essere combinato con l'affinità in un cluster di tre o più nodi.
- Le impostazioni AntiAffinityClassNames e ClusterEnforcedAntiAffinity verranno applicate solo dopo un riciclo delle risorse. I.E. è possibile impostarle, ma se entrambi i gruppi sono online nello stesso nodo quando sono impostati, continueranno a rimanere online.



