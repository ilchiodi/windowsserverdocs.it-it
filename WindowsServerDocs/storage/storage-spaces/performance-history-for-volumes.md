---
title: Cronologia delle prestazioni per i volumi
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589705"
---
# <a name="performance-history-for-volumes"></a>Cronologia delle prestazioni per i volumi

> Si applica a: Windows Server persona interna Preview

In questo argomento secondaria di [cronologia delle prestazioni di archiviazione spazi diretto](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolti per i volumi. La cronologia delle prestazioni è disponibile per ogni Cluster condiviso Volume (con estensione CSV) del cluster. Tuttavia, non risulta disponibile per il sistema operativo di avvio volumi né da altri archivi non CSV.

   > [!NOTE]
   > Potrebbe richiedere alcuni minuti per la raccolta iniziare per i volumi appena creati o rinominati.

## <a name="series-names-and-units"></a>Unità di misura e i nomi delle serie

Le serie vengono raccolti per ogni volume idonei:

| Serie                    | Unit             |
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
| `volume.size.total`       |  byte            |
| `volume.size.available`   |  byte            |

## <a name="how-to-interpret"></a>In che modo interpretare

| Serie                    | In che modo interpretare                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Numero di operazioni di lettura al secondo completate manualmente questo volume.                |
| `volume.iops.write`       | Numero di operazioni di scrittura al secondo completate manualmente questo volume.               |
| `volume.iops.total`       | Numero totale di lettura o scrittura opérations par seconde completate manualmente questo volume. |
| `volume.throughput.read`  | Quantità di dati XML letti da questo volume al secondo.                            |
| `volume.throughput.write` | Quantità di dati scritti questo volume al secondo.                           |
| `volume.throughput.total` | Quantità totale di dati leggere o scrivere in questo volume al secondo.        |
| `volume.latency.read`     | Latenza media delle operazioni di lettura da questo volume.                          |
| `volume.latency.write`    | Latenza media delle operazioni di scrittura per il volume.                           |
| `volume.latency.average`  | Latenza media di tutte le operazioni da o verso il volume.                     |
| `volume.size.total`       | La capacità totale del volume.                                     |
| `volume.size.available`   | La capacità di archiviazione disponibili del volume.                                 |

## <a name="where-they-come-from"></a>Dove provengono da

Il `iops.*`, `throughput.*`, e `latency.*` serie vengono raccolti dal `Cluster CSVFS` insieme di contatori delle prestazioni. Ogni server del cluster dispone di un'istanza per ogni volume CSV, indipendentemente dalla proprietà. La cronologia delle prestazioni registrate per il volume `MyVolume` è l'aggregazione del `MyVolume` istanze in ogni server del cluster.

| Serie                    | Contatori di origine         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *Somma le precedenti*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *Somma le precedenti*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *Media di precedenti* |

   > [!NOTE]
   > Contatori vengono espressi in tutto l'intervallo, non viene eseguito. Ad esempio, se il volume è inattivo per 9 secondi completa ma 30 IOs nella seconda 10, il relativo `volume.iops.total` verrà registrato come 3 IOs al secondo in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile di rumore.

   > [!TIP]
   > Questi sono gli stessi utilizzata dal framework di benchmark [Flotta VM](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) più comuni.

Il `size.*` serie vengono raccolti dal `MSFT_Volume` classe in WMI, un'istanza per volume.

| Serie                    | Proprietà Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Utilizzo di PowerShell

Utilizzare il cmdlet [Get-Volume](https://docs.microsoft.com/powershell/module/storage/get-volume) :

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedi anche

- [Cronologia delle prestazioni di archiviazione spazi diretto](performance-history.md)
