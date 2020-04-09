---
title: Cronologia prestazioni per i cluster
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7a5eec986d6e7d633f1917c599ab6fcd244c7008
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856204"
---
# <a name="performance-history-for-clusters"></a>Cronologia prestazioni per i cluster

> Si applica a: Windows Server 2019

Questo argomento secondario della [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive la cronologia delle prestazioni raccolta per i cluster.

Non sono presenti serie originate a livello di cluster. Al contrario, le serie di server, ad esempio `clusternode.cpu.usage`, vengono aggregate per tutti i server nel cluster. Le serie di volumi, ad esempio `volume.iops.total`, vengono aggregate per tutti i volumi del cluster. E le serie di unità, ad esempio `physicaldisk.size.total`, vengono aggregate per tutte le unità del cluster.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare il cmdlet [Get-cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia prestazioni per Spazi di archiviazione diretta](performance-history.md)
