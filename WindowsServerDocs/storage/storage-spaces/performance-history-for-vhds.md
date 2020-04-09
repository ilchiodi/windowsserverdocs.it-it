---
title: Cronologia delle prestazioni dei dischi rigidi virtuali
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: d917c2d75c1e4078438b94e8aa4a6f921019af5a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856164"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Cronologia delle prestazioni dei dischi rigidi virtuali

> Si applica a: Windows Server 2019

Questo argomento secondario della [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per i file del disco rigido virtuale (VHD). La cronologia delle prestazioni è disponibile per ogni disco rigido virtuale collegato a una macchina virtuale in esecuzione in cluster. La cronologia delle prestazioni è disponibile per i formati VHD e VHDX, ma non è disponibile per i file VHDX condivisi.

   > [!NOTE]
   > Potrebbero essere necessari alcuni minuti per iniziare la raccolta per i file VHD appena creati o spostati.

## <a name="series-names-and-units"></a>Unità e nomi di serie

Queste serie vengono raccolte per ogni disco rigido virtuale idoneo:

| Serie                    | Unità             |
|---------------------------|------------------|
| `vhd.iops.read`           | al secondo       |
| `vhd.iops.write`          | al secondo       |
| `vhd.iops.total`          | al secondo       |
| `vhd.throughput.read`     | byte al secondo |
| `vhd.throughput.write`    | byte al secondo |
| `vhd.throughput.total`    | byte al secondo |
| `vhd.latency.average`     | secondi          |
| `vhd.size.current`        | byte            |
| `vhd.size.maximum`        | byte            |

## <a name="how-to-interpret"></a>Come interpretare

| Serie                    | Come interpretare                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Numero di operazioni di lettura al secondo completate dal disco rigido virtuale.                                         |
| `vhd.iops.write`          | Numero di operazioni di scrittura al secondo completate dal disco rigido virtuale.                                        |
| `vhd.iops.total`          | Numero totale di operazioni di lettura o scrittura al secondo completate dal disco rigido virtuale.                          |
| `vhd.throughput.read`     | Quantità di dati letti dal disco rigido virtuale al secondo.                                                     |
| `vhd.throughput.write`    | Quantità di dati scritti nel disco rigido virtuale al secondo.                                                    |
| `vhd.throughput.total`    | Quantità totale di dati letti o scritti nel disco rigido virtuale al secondo.                                 |
| `vhd.latency.average`     | Latenza media di tutte le operazioni da o verso il disco rigido virtuale.                                              |
| `vhd.size.current`        | Dimensioni correnti del file del disco rigido virtuale, se ad espansione dinamica. Se è fissa, la serie non viene raccolta. |
| `vhd.size.maximum`        | Dimensione massima del disco rigido virtuale, se ad espansione dinamica. Se è fisso, è la dimensione.                  |

## <a name="where-they-come-from"></a>Da dove provengono

Le serie `iops.*`, `throughput.*`e `latency.*` vengono raccolte dal set di contatori delle prestazioni `Hyper-V Virtual Storage Device` nel server in cui è in esecuzione la macchina virtuale, un'istanza per ogni disco rigido virtuale o VHDX.

| Serie                    | Contatore di origine         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *somma dei precedenti*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *somma dei precedenti*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > I contatori sono misurati nell'intero intervallo, non campionati. Ad esempio, se il disco rigido virtuale è inattivo per 9 secondi, ma completa 30 IOs nel decimo secondo, la `vhd.iops.total` verrà registrata come 3 IOs al secondo in media durante questo intervallo di 10 secondi. Ciò garantisce che la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile per il rumore.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare il cmdlet [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) :

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Per ottenere il percorso di ogni disco rigido virtuale dalla macchina virtuale:

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > Il cmdlet Get-VHD richiede un percorso di file da fornire. Non supporta l'enumerazione.

## <a name="see-also"></a>Vedere anche

- [Cronologia prestazioni per Spazi di archiviazione diretta](performance-history.md)
