---
title: Cronologia delle prestazioni dei dischi rigidi virtuali
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589722"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Cronologia delle prestazioni dei dischi rigidi virtuali

> Si applica a: Windows Server persona interna Preview

In questo argomento secondaria di [cronologia delle prestazioni di archiviazione spazi diretto](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolti per i file su disco rigido virtuale (VHD). La cronologia delle prestazioni è disponibile per ogni disco rigido virtuale collegato a una macchina virtuale in esecuzione, non in pila. La cronologia delle prestazioni è disponibile per i formati di file VHD sia VHDX, tuttavia non è disponibile per i file condivisi VHDX.

   > [!NOTE]
   > Potrebbe richiedere alcuni minuti per la raccolta iniziare per i file di file VHD appena creati o spostati.

## <a name="series-names-and-units"></a>Unità di misura e i nomi delle serie

Le serie vengono raccolti per ogni disco rigido virtuale idoneo:

| Serie                    | Unit             |
|---------------------------|------------------|
| `vhd.iops.read`           | al secondo       |
| `vhd.iops.write`          | al secondo       |
| `vhd.iops.total`          | al secondo       |
| `vhd.throughput.read`     | byte al secondo |
| `vhd.throughput.write`    | byte al secondo |
| `vhd.throughput.total`    | byte al secondo |
| `vhd.latency.average`     | secondi          |
| `vhd.size.current`        |  byte            |
| `vhd.size.maximum`        |  byte            |

## <a name="how-to-interpret"></a>In che modo interpretare

| Serie                    | In che modo interpretare                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Numero di operazioni di lettura al secondo completate manualmente il disco rigido virtuale.                                         |
| `vhd.iops.write`          | Numero di operazioni di scrittura al secondo completate manualmente il disco rigido virtuale.                                        |
| `vhd.iops.total`          | Numero totale di lettura o scrittura opérations par seconde completate manualmente il disco rigido virtuale.                          |
| `vhd.throughput.read`     | Quantità di dati XML letti da disco rigido virtuale al secondo.                                                     |
| `vhd.throughput.write`    | Quantità di dati vengano scritti nel disco rigido virtuale al secondo.                                                    |
| `vhd.throughput.total`    | Quantità totale di dati leggere o scrivere nel disco rigido virtuale al secondo.                                 |
| `vhd.latency.average`     | Latenza media di tutte le operazioni da o verso il disco rigido virtuale.                                              |
| `vhd.size.current`        | Dimensioni del file corrente del disco rigido virtuale, se l'espansione in modo dinamico. Se uno spazio non vengono raccolti della serie. |
| `vhd.size.maximum`        | La dimensione massima del disco rigido virtuale, se l'espansione in modo dinamico. Se uno spazio di è la dimensione.                  |

## <a name="where-they-come-from"></a>Dove provengono da

Il `iops.*`, `throughput.*`, e `latency.*` serie vengono raccolti dal `Hyper-V Virtual Storage Device` contatore delle prestazioni impostato sul server dove la macchina virtuale è in esecuzione, un'istanza per ogni disco rigido virtuale o VHDX.

| Serie                    | Contatori di origine         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *Somma le precedenti*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *Somma le precedenti*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Contatori vengono espressi in tutto l'intervallo, non viene eseguito. Ad esempio, se il disco rigido virtuale è inattivo per 9 secondi completa ma 30 IOs nella seconda 10, il relativo `vhd.iops.total` verrà registrato come 3 IOs al secondo in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile di rumore.

## <a name="usage-in-powershell"></a>Utilizzo di PowerShell

Utilizzare il cmdlet [Get-disco rigido virtuale](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) :

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Per ottenere il percorso di ogni disco rigido virtuale dalla macchina virtuale:

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > Il cmdlet Get-VHD richiede un percorso di file devono essere fornite. Non supporta l'enumerazione.

## <a name="see-also"></a>Vedi anche

- [Cronologia delle prestazioni di archiviazione spazi diretto](performance-history.md)
