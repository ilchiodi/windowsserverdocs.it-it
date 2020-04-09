---
title: Cronologia prestazioni per le macchine virtuali
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: aefc9c3c33cb93be241aae4ef18d815a9f8defef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856144"
---
# <a name="performance-history-for-virtual-machines"></a>Cronologia prestazioni per le macchine virtuali

> Si applica a: Windows Server 2019

Questo argomento secondario della [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per le macchine virtuali (VM). La cronologia delle prestazioni è disponibile per ogni macchina virtuale in esecuzione in cluster.

   > [!NOTE]
   > Potrebbero essere necessari alcuni minuti per iniziare la raccolta per le macchine virtuali appena create o rinominate.

## <a name="series-names-and-units"></a>Unità e nomi di serie

Queste serie vengono raccolte per ogni macchina virtuale idonea:

| Serie                            | Unità             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | percent          |
| `vm.memory.assigned`              | byte            |
| `vm.memory.available`             | byte            |
| `vm.memory.maximum`               | byte            |
| `vm.memory.minimum`               | byte            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | byte            |
| `vm.memory.total`                 | byte            |
| `vmnetworkadapter.bandwidth.inbound`  | bit al secondo |
| `vmnetworkadapter.bandwidth.outbound` | bit al secondo |
| `vmnetworkadapter.bandwidth.total`    | bit al secondo |

Inoltre, tutte le serie di dischi rigidi virtuali (VHD), ad esempio `vhd.iops.total`, vengono aggregate per ogni disco rigido virtuale collegato alla macchina virtuale.

## <a name="how-to-interpret"></a>Come interpretare


| Serie                            | Descrizione                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Percentuale di utilizzo del processore del server host da parte della macchina virtuale.                                   |
| `vm.memory.assigned`              | Quantità di memoria assegnata alla macchina virtuale.                                                      |
| `vm.memory.available`             | Quantità di memoria che rimane disponibile, della quantità assegnata.                                       |
| `vm.memory.maximum`               | Se si usa la memoria dinamica, questa è la quantità massima di memoria che può essere assegnata alla macchina virtuale. |
| `vm.memory.minimum`               | Se si usa la memoria dinamica, si tratta della quantità minima di memoria che può essere assegnata alla macchina virtuale. |
| `vm.memory.pressure`              | Rapporto tra la memoria richiesta dalla macchina virtuale e la memoria allocata alla macchina virtuale.            |
| `vm.memory.startup`               | Quantità di memoria necessaria per l'avvio della macchina virtuale.                                            |
| `vm.memory.total`                 | Memoria totale. |
| `vmnetworkadapter.bandwidth.inbound`  | Frequenza dei dati ricevuti dalla macchina virtuale in tutte le schede di rete virtuali.                        |
| `vmnetworkadapter.bandwidth.outbound` | Frequenza dei dati inviati dalla macchina virtuale in tutte le schede di rete virtuali.                            |
| `vmnetworkadapter.bandwidth.total`    | Velocità totale dei dati ricevuti o inviati dalla macchina virtuale in tutte le schede di rete virtuali.          |

   > [!NOTE]
   > I contatori sono misurati nell'intero intervallo, non campionati. Se, ad esempio, la macchina virtuale è inattiva per 9 secondi, ma i picchi utilizzano il 50% della CPU dell'host nel decimo secondo, la `vm.cpu.usage` verrà registrata come media del 5% durante questo intervallo di 10 secondi. Ciò garantisce che la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile per il rumore.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare il cmdlet [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) :

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > Il cmdlet Get-VM restituisce solo le macchine virtuali nel server locale o specificato, non nel cluster.

## <a name="see-also"></a>Vedere anche

- [Cronologia prestazioni per Spazi di archiviazione diretta](performance-history.md)
