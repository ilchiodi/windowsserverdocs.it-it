---
title: Cronologia delle prestazioni per i gruppi
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894320"
---
# <a name="performance-history-for-clusters"></a>Cronologia delle prestazioni per i gruppi

> Si applica a: Windows Server persona interna Preview

Secondaria di [cronologia delle prestazioni di archiviazione spazi diretta](performance-history.md) viene descritta la cronologia delle prestazioni raccolta per i gruppi.

Non esistono nessuna serie che hanno origine a livello di cluster. In realtà, serie di server, ad esempio `clusternode.cpu.usage`, vengono aggregati per tutti i server del cluster. Serie di volume, ad esempio `volume.iops.total`, vengono aggregati per tutti i volumi del cluster. E unità serie, ad esempio `physicaldisk.size.total`, vengono aggregati per tutte le unità del cluster.

## <a name="usage-in-powershell"></a>Utilizzo di PowerShell

Utilizzare il cmdlet [Get-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Vedi anche

- [Cronologia delle prestazioni di archiviazione spazi diretto](performance-history.md)
