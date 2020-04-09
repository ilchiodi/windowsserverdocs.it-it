---
title: Cronologia prestazioni per i volumi
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5f6acf062d2dba7c2a1a04d8a3f7cb4d7bd51a4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856134"
---
# <a name="performance-history-for-volumes"></a>Cronologia prestazioni per i volumi

> Si applica a: Windows Server 2019

Questo argomento secondario della [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per i volumi. La cronologia delle prestazioni è disponibile per ogni Volume condiviso cluster (CSV) del cluster. Tuttavia, non è disponibile per i volumi di avvio del sistema operativo e per qualsiasi altra risorsa di archiviazione non CSV.

   > [!NOTE]
   > L'inizio della raccolta può richiedere alcuni minuti per i volumi appena creati o rinominati.

## <a name="series-names-and-units"></a>Unità e nomi di serie

Queste serie vengono raccolte per ogni volume idoneo:

| Serie                    | Unità             |
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
| `volume.size.total`       | byte            |
| `volume.size.available`   | byte            |

## <a name="how-to-interpret"></a>Come interpretare

| Serie                    | Come interpretare                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Numero di operazioni di lettura al secondo completate da questo volume.                |
| `volume.iops.write`       | Numero di operazioni di scrittura al secondo completate da questo volume.               |
| `volume.iops.total`       | Numero totale di operazioni di lettura o scrittura al secondo completate da questo volume. |
| `volume.throughput.read`  | Quantità di dati letti da questo volume al secondo.                            |
| `volume.throughput.write` | Quantità di dati scritti in questo volume al secondo.                           |
| `volume.throughput.total` | Quantità totale di dati letti da o scritti in questo volume al secondo.        |
| `volume.latency.read`     | Latenza media delle operazioni di lettura da questo volume.                          |
| `volume.latency.write`    | Latenza media delle operazioni di scrittura in questo volume.                           |
| `volume.latency.average`  | Latenza media di tutte le operazioni da o verso questo volume.                     |
| `volume.size.total`       | Capacità di archiviazione totale del volume.                                     |
| `volume.size.available`   | Capacità di archiviazione disponibile del volume.                                 |

## <a name="where-they-come-from"></a>Da dove provengono

Le serie `iops.*`, `throughput.*`e `latency.*` vengono raccolte dal set di contatori delle prestazioni `Cluster CSVFS`. Ogni server del cluster dispone di un'istanza per ogni volume CSV, indipendentemente dalla proprietà. La cronologia delle prestazioni registrata per volume `MyVolume` è l'aggregazione delle istanze di `MyVolume` in ogni server del cluster.

| Serie                    | Contatore di origine         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *somma dei precedenti*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *somma dei precedenti*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *media dei precedenti* |

   > [!NOTE]
   > I contatori sono misurati nell'intero intervallo, non campionati. Ad esempio, se il volume è inattivo per 9 secondi, ma completa 30 IOs nel decimo secondo, la `volume.iops.total` verrà registrata come 3 IOs al secondo in media durante questo intervallo di 10 secondi. Ciò garantisce che la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile per il rumore.

   > [!TIP]
   > Si tratta degli stessi contatori usati dal comune Framework di benchmark di macchine [virtuali](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) .

La serie `size.*` viene raccolta dalla classe `MSFT_Volume` in WMI, un'istanza per ogni volume.

| Serie                    | proprietà Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare il cmdlet [Get-volume](https://docs.microsoft.com/powershell/module/storage/get-volume) :

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia prestazioni per Spazi di archiviazione diretta](performance-history.md)
