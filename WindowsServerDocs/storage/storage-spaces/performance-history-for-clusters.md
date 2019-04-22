---
title: Cronologia delle prestazioni per i cluster
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818772"
---
# <a name="performance-history-for-clusters"></a>Cronologia delle prestazioni per i cluster

> Si applica a: Anteprima Windows Server Insider

Questo argomento secondario di [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive la cronologia delle prestazioni raccolta per i cluster.

Non esistono alcuna serie originate al livello del cluster. Al contrario, serie di server, ad esempio `clusternode.cpu.usage`, vengono aggregati per tutti i server del cluster. Serie di volume, ad esempio `volume.iops.total`, aggregate per tutti i volumi del cluster. E incrementa le serie, ad esempio `physicaldisk.size.total`, vengono aggregati per tutte le unità nel cluster.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare la [Get-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) cmdlet:

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md)
