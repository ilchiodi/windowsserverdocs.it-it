---
title: Cronologia delle prestazioni per le unità
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879192"
---
# <a name="performance-history-for-drives"></a>Cronologia delle prestazioni per le unità

> Si applica a: Anteprima Windows Server Insider

Questo argomento secondario di [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per le unità. Cronologia delle prestazioni è disponibile per ogni unità nel sottosistema di archiviazione del cluster, indipendentemente dal bus o tipo di supporto. Non è tuttavia disponibile per le unità di avvio del sistema operativo.

   > [!NOTE]
   > Cronologia delle prestazioni non può essere raccolti per le unità in un server che non è attivo. Raccolta riprenderà automaticamente quando il server torna allo stato attivo.

## <a name="series-names-and-units"></a>Unità e i nomi delle serie

Queste serie vengono raccolti per tutte le unità idonee:

| serie                          | Unità             |
|---------------------------------|------------------|
| `physicaldisk.iops.read`        | al secondo       |
| `physicaldisk.iops.write`       | al secondo       |
| `physicaldisk.iops.total`       | al secondo       |
| `physicaldisk.throughput.read`  | byte al secondo |
| `physicaldisk.throughput.write` | byte al secondo |
| `physicaldisk.throughput.total` | byte al secondo |
| `physicaldisk.latency.read`     | secondi          |
| `physicaldisk.latency.write`    | secondi          |
| `physicaldisk.latency.average`  | secondi          |
| `physicaldisk.size.total`       | Byte            |
| `physicaldisk.size.used`        | Byte            |

## <a name="how-to-interpret"></a>Come interpretare

| serie                          | Come interpretare                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Numero di operazioni di lettura al secondo completate dall'unità.                |
| `physicaldisk.iops.write`       | Numero di operazioni di scrittura al secondo completate dall'unità.               |
| `physicaldisk.iops.total`       | Numero totale di leggere o scrittura operazioni al secondo completate dall'unità. |
| `physicaldisk.throughput.read`  | Quantità di dati letti dal disco al secondo.                            |
| `physicaldisk.throughput.write` | Quantità di dati scritti sul disco al secondo.                           |
| `physicaldisk.throughput.total` | Quantità totale di dati lette o scritte sul disco al secondo.        |
| `physicaldisk.latency.read`     | Latenza media delle operazioni di lettura dal disco.                          |
| `physicaldisk.latency.write`    | Latenza media delle operazioni di scrittura sul disco.                           |
| `physicaldisk.latency.average`  | Latenza media di tutte le operazioni da o verso l'unità.                     |
| `physicaldisk.size.total`       | La capacità di archiviazione totale dell'unità.                                    |
| `physicaldisk.size.used`        | La capacità di spazio di archiviazione utilizzato dell'unità.                                     |

## <a name="where-they-come-from"></a>Da dove provengono

Il `iops.*`, `throughput.*`, e `latency.*` serie vengono raccolti dal `Physical Disk` contatore delle prestazioni impostato nel server in cui è collegata l'unità, un'istanza per disco. Questi contatori vengono misurati da `partmgr.sys` e non includono gli hop di rete che misura lo stack di software di Windows né qualsiasi. Sono rappresentativi di prestazioni dell'hardware del dispositivo.

| serie                          | Contatore di origine           |
|---------------------------------|--------------------------|
| `physicaldisk.iops.read`        | `Disk Reads/sec`         |
| `physicaldisk.iops.write`       | `Disk Writes/sec`        |
| `physicaldisk.iops.total`       | `Disk Transfers/sec`     |
| `physicaldisk.throughput.read`  | `Disk Read Bytes/sec`    |
| `physicaldisk.throughput.write` | `Disk Write Bytes/sec`   |
| `physicaldisk.throughput.total` | `Disk Bytes/sec`         |
| `physicaldisk.latency.read`     | `Avg. Disk sec/Read`     |
| `physicaldisk.latency.write`    | `Avg. Disk sec/Writes`   |
| `physicaldisk.latency.average`  | `Avg. Disk sec/Transfer` |

   > [!NOTE]
   > I contatori vengono misurati su tutto l'intervallo, non campionato. Ad esempio, se l'unità è inattivo per 9 secondi ma viene completata 30 IOs nella seconda 10th, relativo `physicaldisk.iops.total` saranno registrate come 3 IOs al secondo in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività e affidabile al rumore.

Il `size.*` serie vengono raccolti dal `MSFT_PhysicalDisk` classe in WMI, un'istanza per ogni unità.

| serie                          | Proprietà Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare la [Get-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) cmdlet:

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md)
