---
title: Cronologia prestazioni per le unità
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: a6c6065b8d7963ada5d80844b270fe088eaa6e56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859454"
---
# <a name="performance-history-for-drives"></a>Cronologia prestazioni per le unità

> Si applica a: Windows Server 2019

Questo argomento secondario della [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per le unità. La cronologia delle prestazioni è disponibile per ogni unità del sottosistema di archiviazione del cluster, indipendentemente dal bus o dal tipo di supporto. Tuttavia, non è disponibile per le unità di avvio del sistema operativo.

   > [!NOTE]
   > Non è possibile raccogliere la cronologia delle prestazioni per le unità in un server che non è attivo. La raccolta verrà ripresa automaticamente quando il server verrà riavviato.

## <a name="series-names-and-units"></a>Unità e nomi di serie

Queste serie vengono raccolte per ogni unità idonea:

| Serie                          | Unità             |
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
| `physicaldisk.size.total`       | byte            |
| `physicaldisk.size.used`        | byte            |

## <a name="how-to-interpret"></a>Come interpretare

| Serie                          | Come interpretare                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Numero di operazioni di lettura al secondo completate dall'unità.                |
| `physicaldisk.iops.write`       | Numero di operazioni di scrittura al secondo completate dall'unità.               |
| `physicaldisk.iops.total`       | Numero totale di operazioni di lettura o scrittura al secondo completate dall'unità. |
| `physicaldisk.throughput.read`  | Quantità di dati letti dall'unità al secondo.                            |
| `physicaldisk.throughput.write` | Quantità di dati scritti nell'unità al secondo.                           |
| `physicaldisk.throughput.total` | Quantità totale di dati letti o scritti nell'unità al secondo.        |
| `physicaldisk.latency.read`     | Latenza media delle operazioni di lettura dall'unità.                          |
| `physicaldisk.latency.write`    | Latenza media delle operazioni di scrittura nell'unità.                           |
| `physicaldisk.latency.average`  | Latenza media di tutte le operazioni da o verso l'unità.                     |
| `physicaldisk.size.total`       | Capacità di archiviazione totale dell'unità.                                    |
| `physicaldisk.size.used`        | Capacità di archiviazione utilizzata dell'unità.                                     |

## <a name="where-they-come-from"></a>Da dove provengono

Le serie `iops.*`, `throughput.*`e `latency.*` vengono raccolte dal contatore delle prestazioni `Physical Disk` impostato sul server in cui è connessa l'unità, un'istanza per unità. Questi contatori sono misurati in base `partmgr.sys` e non includono Gran parte dello stack di software Windows né gli hop di rete. Sono rappresentativi delle prestazioni hardware del dispositivo.

| Serie                          | Contatore di origine           |
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
   > I contatori sono misurati nell'intero intervallo, non campionati. Se, ad esempio, l'unità è inattiva per 9 secondi, ma completa 30 IOs nel decimo secondo, la `physicaldisk.iops.total` verrà registrata come 3 IOs al secondo in media durante questo intervallo di 10 secondi. Ciò garantisce che la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile per il rumore.

La serie `size.*` viene raccolta dalla classe `MSFT_PhysicalDisk` in WMI, un'istanza per unità.

| Serie                          | proprietà Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare il cmdlet [Get-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) :

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia prestazioni per Spazi di archiviazione diretta](performance-history.md)
