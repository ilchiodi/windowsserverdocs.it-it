---
title: Cronologia delle prestazioni per le unità
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589756"
---
# <a name="performance-history-for-drives"></a>Cronologia delle prestazioni per le unità

> Si applica a: Windows Server persona interna Preview

In questo argomento secondaria di [cronologia delle prestazioni di archiviazione spazi diretto](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolti per le unità. La cronologia delle prestazioni è disponibile per ogni unità nel sottosistema di archiviazione cluster, indipendentemente dal bus o tipo di supporto. Non è invece disponibile per le unità di avvio del sistema operativo.

   > [!NOTE]
   > Non è possibile raccogliere la cronologia delle prestazioni per le unità di un server che non è attivo. Insieme riprenderà automaticamente quando il server verrà ripristinato.

## <a name="series-names-and-units"></a>Unità di misura e i nomi delle serie

Le serie vengono raccolti per ogni unità idonei:

| Serie                          | Unit             |
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
| `physicaldisk.size.total`       |  byte            |
| `physicaldisk.size.used`        |  byte            |

## <a name="how-to-interpret"></a>In che modo interpretare

| Serie                          | In che modo interpretare                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Numero di operazioni di lettura al secondo completato dall'unità.                |
| `physicaldisk.iops.write`       | Numero di operazioni di scrittura al secondo completato dall'unità.               |
| `physicaldisk.iops.total`       | Numero totale di lettura o scrittura opérations par seconde completato dall'unità. |
| `physicaldisk.throughput.read`  | Quantità di dati XML letti da disco al secondo.                            |
| `physicaldisk.throughput.write` | Quantità di dati scritti nell'unità al secondo.                           |
| `physicaldisk.throughput.total` | Quantità totale di dati leggere o scrivere nell'unità al secondo.        |
| `physicaldisk.latency.read`     | Latenza media delle operazioni di lettura dall'unità.                          |
| `physicaldisk.latency.write`    | Latenza media delle operazioni di scrittura nell'unità.                           |
| `physicaldisk.latency.average`  | Latenza media di tutte le operazioni a o dall'unità.                     |
| `physicaldisk.size.total`       | La capacità totale dell'unità.                                    |
| `physicaldisk.size.used`        | La capacità di spazio di archiviazione utilizzato dell'unità.                                     |

## <a name="where-they-come-from"></a>Dove provengono da

Il `iops.*`, `throughput.*`, e `latency.*` serie vengono raccolti dal `Physical Disk` contatore delle prestazioni impostato sul server in cui è connesso l'unità, un'istanza per ogni unità. Questi contatori sono misurati in base alla `partmgr.sys` e non includere quantità dello stack di software Windows né qualsiasi hop di rete. Sono rappresentante delle prestazioni di dispositivi hardware.

| Serie                          | Contatori di origine           |
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
   > Contatori vengono espressi in tutto l'intervallo, non viene eseguito. Ad esempio, se l'unità è inattivo per 9 secondi ma al termine 30 IOs nella seconda 10, il relativo `physicaldisk.iops.total` verrà registrato come 3 IOs al secondo in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile di rumore.

Il `size.*` serie vengono raccolti dal `MSFT_PhysicalDisk` classe in WMI, un'istanza per ogni unità.

| Serie                          | Proprietà Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Utilizzo di PowerShell

Utilizzare il cmdlet [Get-disco fisico](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) :

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedi anche

- [Cronologia delle prestazioni di archiviazione spazi diretto](performance-history.md)
