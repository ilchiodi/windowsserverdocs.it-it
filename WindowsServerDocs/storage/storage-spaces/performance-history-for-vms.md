---
title: Cronologia delle prestazioni per le macchine virtuali
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890872"
---
# <a name="performance-history-for-virtual-machines"></a>Cronologia delle prestazioni per le macchine virtuali

> Si applica a: Anteprima Windows Server Insider

Questo argomento secondario di [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per le macchine virtuali (VM). Cronologia delle prestazioni è disponibile per ogni esecuzione, cluster macchina virtuale.

   > [!NOTE]
   > Potrebbero occorrere alcuni minuti per la raccolta iniziare a per le macchine virtuali appena create o rinominate.

## <a name="series-names-and-units"></a>Unità e i nomi delle serie

Queste serie vengono raccolti per tutte le macchine Virtuali idonee:

| serie                            | Unità             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | Percentuale          |
| `vm.memory.assigned`              | Byte            |
| `vm.memory.available`             | Byte            |
| `vm.memory.maximum`               | Byte            |
| `vm.memory.minimum`               | Byte            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | Byte            |
| `vm.memory.total`                 | Byte            |
| `vmnetworkadapter.bandwidth.inbound`  | Bit al secondo |
| `vmnetworkadapter.bandwidth.outbound` | Bit al secondo |
| `vmnetworkadapter.bandwidth.total`    | Bit al secondo |

Inoltre, tutte le serie di disco rigido virtuale (VHD), ad esempio `vhd.iops.total`, vengono aggregati per ogni disco rigido virtuale collegato alla macchina virtuale.

## <a name="how-to-interpret"></a>Come interpretare


| serie                            | Descrizione                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Percentuale della macchina virtuale utilizza del processore o i processori del relativo server host.                                   |
| `vm.memory.assigned`              | La quantità di memoria assegnata alla macchina virtuale.                                                      |
| `vm.memory.available`             | La quantità di memoria rimanente disponibile, la quantità assegnato.                                       |
| `vm.memory.maximum`               | Se si usa la memoria dinamica, questa è la quantità massima di memoria che può essere assegnata alla macchina virtuale. |
| `vm.memory.minimum`               | Se si usa la memoria dinamica, questa è la quantità minima di memoria che può essere assegnata alla macchina virtuale. |
| `vm.memory.pressure`              | Il rapporto tra memoria richiesta dalla macchina virtuale su memoria allocata alla macchina virtuale.            |
| `vm.memory.startup`               | La quantità di memoria necessaria per la macchina virtuale da avviare.                                            |
| `vm.memory.total`                 | Memoria totale. |
| `vmnetworkadapter.bandwidth.inbound`  | Frequenza dei dati ricevuti dalla macchina virtuale tra tutte le schede di rete virtuale.                        |
| `vmnetworkadapter.bandwidth.outbound` | Frequenza dei dati inviati dalla macchina virtuale su tutte le schede di rete virtuale.                            |
| `vmnetworkadapter.bandwidth.total`    | Frequenza totale di dati ricevuti o inviati dalla macchina virtuale tra tutte le schede di rete virtuale.          |

   > [!NOTE]
   > I contatori vengono misurati su tutto l'intervallo, non campionato. Ad esempio, se la macchina virtuale è inattiva per 9 secondi, ma picchi 50% della CPU dell'host nella seconda 10th, relativo `vm.cpu.usage` saranno registrate come 5% in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività e affidabile al rumore.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare la [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet:

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > Il cmdlet Get-VM restituisce solo le macchine virtuali nel server locale (o specificato), non in cluster.

## <a name="see-also"></a>Vedere anche

- [Cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md)
