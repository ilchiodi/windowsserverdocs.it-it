---
title: Cronologia delle prestazioni per i dischi rigidi virtuali
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880402"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Cronologia delle prestazioni per i dischi rigidi virtuali

> Si applica a: Anteprima Windows Server Insider

Questo argomento secondario di [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per i file di disco rigido virtuale (VHD). Cronologia delle prestazioni è disponibile per ogni disco rigido virtuale collegato a una macchina virtuale in esecuzione e in cluster. Cronologia delle prestazioni è disponibile per i formati sia VHD e VHDX, tuttavia, non è disponibile per i file VHDX condiviso.

   > [!NOTE]
   > Potrebbero occorrere alcuni minuti per la raccolta iniziare a per i file di disco rigido virtuale appena creati o spostati.

## <a name="series-names-and-units"></a>Unità e i nomi delle serie

Queste serie vengono raccolti per ogni disco rigido virtuale idonei:

| serie                    | Unità             |
|---------------------------|------------------|
| `vhd.iops.read`           | al secondo       |
| `vhd.iops.write`          | al secondo       |
| `vhd.iops.total`          | al secondo       |
| `vhd.throughput.read`     | byte al secondo |
| `vhd.throughput.write`    | byte al secondo |
| `vhd.throughput.total`    | byte al secondo |
| `vhd.latency.average`     | secondi          |
| `vhd.size.current`        | Byte            |
| `vhd.size.maximum`        | Byte            |

## <a name="how-to-interpret"></a>Come interpretare

| serie                    | Come interpretare                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Numero di operazioni di lettura al secondo completate dal disco rigido virtuale.                                         |
| `vhd.iops.write`          | Numero di operazioni di scrittura al secondo completate dal disco rigido virtuale.                                        |
| `vhd.iops.total`          | Numero totale di leggere o scrittura operazioni al secondo completate dal disco rigido virtuale.                          |
| `vhd.throughput.read`     | Quantità di dati letti dal disco rigido virtuale al secondo.                                                     |
| `vhd.throughput.write`    | Quantità di dati scritti sul disco rigido virtuale al secondo.                                                    |
| `vhd.throughput.total`    | Quantità totale di dati lette o scritte sul disco rigido virtuale al secondo.                                 |
| `vhd.latency.average`     | Latenza media di tutte le operazioni da o verso il disco rigido virtuale.                                              |
| `vhd.size.current`        | La dimensione corrente del file del disco rigido virtuale, se a espansione dinamica. Se viene risolto, la serie non vengono raccolti. |
| `vhd.size.maximum`        | Le dimensioni massime del disco rigido virtuale, se a espansione dinamica. Se viene risolto, le dimensioni.                  |

## <a name="where-they-come-from"></a>Da dove provengono

Il `iops.*`, `throughput.*`, e `latency.*` serie vengono raccolti dal `Hyper-V Virtual Storage Device` insieme di contatori delle prestazioni nel server in cui la macchina virtuale è in esecuzione, un'istanza per ogni VHD o VHDX.

| serie                    | Contatore di origine         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *somma dei valori precedenti*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *somma dei valori precedenti*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > I contatori vengono misurati su tutto l'intervallo, non campionato. Ad esempio, se il disco rigido virtuale è attivo per il 9 secondi ma viene completato 30 IOs nella seconda 10th, relativo `vhd.iops.total` saranno registrate come 3 IOs al secondo in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività e affidabile al rumore.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare la [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) cmdlet:

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Per ottenere il percorso di ogni disco rigido virtuale dalla macchina virtuale:

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > Il cmdlet Get-VHD richiede un percorso di file devono essere fornite. Non supporta l'enumerazione.

## <a name="see-also"></a>Vedere anche

- [Cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md)
