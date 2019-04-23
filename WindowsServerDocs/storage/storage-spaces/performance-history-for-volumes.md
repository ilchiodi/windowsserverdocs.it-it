---
title: Cronologia delle prestazioni per i volumi
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882952"
---
# <a name="performance-history-for-volumes"></a>Cronologia delle prestazioni per i volumi

> Si applica a: Anteprima Windows Server Insider

Questo argomento secondario di [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per i volumi. Cronologia delle prestazioni è disponibile per ogni Volume condiviso Cluster (CSV) nel cluster. Non è tuttavia disponibile per il sistema operativo di avvio volumi né qualsiasi altro dispositivo di archiviazione non CSV.

   > [!NOTE]
   > Potrebbero occorrere alcuni minuti per la raccolta iniziare a per i volumi appena creati o rinominati.

## <a name="series-names-and-units"></a>Unità e i nomi delle serie

Queste serie vengono raccolti per ogni volume idonei:

| serie                    | Unità             |
|---------------------------|------------------|
| `volume.iops.read`        | al secondo       |
| `volume.iops.write`       | al secondo       |
| `volume.iops.total`       | al secondo       |
| `volume.throughput.read`  | byte al secondo |
| `volume.throughput.write` | byte al secondo |
| `volume.throughput.total` | byte al secondo |
| `volume.latency.read`     | secondi          |
| `volume.latency.write`    | secondi          |
| `volume.latency.average`  | secondi          |
| `volume.size.total`       | Byte            |
| `volume.size.available`   | Byte            |

## <a name="how-to-interpret"></a>Come interpretare

| serie                    | Come interpretare                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Numero di operazioni di lettura al secondo completate per questo volume.                |
| `volume.iops.write`       | Numero di operazioni di scrittura al secondo completate per questo volume.               |
| `volume.iops.total`       | Numero totale di leggere o scrittura operazioni al secondo completate per questo volume. |
| `volume.throughput.read`  | Quantità di dati letti da questo volume al secondo.                            |
| `volume.throughput.write` | Quantità di dati scritti da questo volume al secondo.                           |
| `volume.throughput.total` | Quantità totale di dati letti da o scritti in questo volume al secondo.        |
| `volume.latency.read`     | Latenza media delle operazioni di lettura da questo volume.                          |
| `volume.latency.write`    | Latenza media delle operazioni di scrittura per questo volume.                           |
| `volume.latency.average`  | Latenza media di tutte le operazioni da o verso questo volume.                     |
| `volume.size.total`       | La capacità di archiviazione totale del volume.                                     |
| `volume.size.available`   | La capacità di archiviazione disponibile del volume.                                 |

## <a name="where-they-come-from"></a>Da dove provengono

Il `iops.*`, `throughput.*`, e `latency.*` serie vengono raccolti dal `Cluster CSVFS` insieme di contatori delle prestazioni. Tutti i server del cluster dispone di un'istanza per ogni volume CSV, indipendentemente dal proprietario. La cronologia delle prestazioni registrate per il volume `MyVolume` è l'aggregazione del `MyVolume` istanze in ogni server del cluster.

| serie                    | Contatore di origine         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *somma dei valori precedenti*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *somma dei valori precedenti*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *Media dei valori precedenti* |

   > [!NOTE]
   > I contatori vengono misurati su tutto l'intervallo, non campionato. Ad esempio, se il volume è inattivo per 9 secondi ma viene completato 30 IOs nella seconda 10th, relativo `volume.iops.total` saranno registrate come 3 IOs al secondo in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività e affidabile al rumore.

   > [!TIP]
   > Questi sono gli stessi contatori usati per popolare [Fleet VM](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) framework benchmark.

Il `size.*` serie vengono raccolti dal `MSFT_Volume` classe in WMI, un'istanza per ogni volume.

| serie                    | Proprietà Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare la [Get-Volume](https://docs.microsoft.com/powershell/module/storage/get-volume) cmdlet:

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md)
