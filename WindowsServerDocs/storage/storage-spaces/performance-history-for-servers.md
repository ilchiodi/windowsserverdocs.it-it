---
title: Cronologia delle prestazioni per i server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820592"
---
# <a name="performance-history-for-servers"></a>Cronologia delle prestazioni per i server

> Si applica a: Anteprima Windows Server Insider

Questo argomento secondario di [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per i server. Cronologia delle prestazioni è disponibile per tutti i server del cluster.

   > [!NOTE]
   > Cronologia delle prestazioni non possa essere raccolti per un server che non è attivo. Raccolta riprenderà automaticamente quando il server torna allo stato attivo.

## <a name="series-names-and-units"></a>Unità e i nomi delle serie

Queste serie vengono raccolti per tutti i server idonei:

| serie                           | Unità    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | Percentuale |
| `clusternode.cpu.usage.guest`    | Percentuale |
| `clusternode.cpu.usage.host`     | Percentuale |
| `clusternode.memory.total`       | Byte   |
| `clusternode.memory.available`   | Byte   |
| `clusternode.memory.usage`       | Byte   |
| `clusternode.memory.usage.guest` | Byte   |
| `clusternode.memory.usage.host`  | Byte   |

Inoltre, unità, ad esempio serie `physicaldisk.size.total` vengono aggregati per tutte le unità idonee collegate al server e serie di schede di rete, ad esempio `networkadapter.bytes.total` vengono aggregati per tutte le schede di rete idonea al server collegate.

## <a name="how-to-interpret"></a>Come interpretare

| serie                           | Come interpretare                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Percentuale del tempo del processore non inattivo.                        |
| `clusternode.cpu.usage.guest`    | Percentuale di tempo processore utilizzato per la domanda guest (macchina virtuale). |
| `clusternode.cpu.usage.host`     | Percentuale di tempo processore utilizzato per la richiesta di host.                    |
| `clusternode.memory.total`       | La memoria fisica totale del server.                              |
| `clusternode.memory.available`   | La memoria disponibile del server.                                   |
| `clusternode.memory.usage`       | La memoria allocata (non disponibile) del server.                   |
| `clusternode.memory.usage.guest` | La memoria allocata alla richiesta guest (macchina virtuale).               |
| `clusternode.memory.usage.host`  | La memoria allocata alla richiesta di host.                                  |

## <a name="where-they-come-from"></a>Da dove provengono

Il `cpu.*` serie vengono raccolti dai contatori di prestazioni diverso a seconda se è abilitato Hyper-V.

Se è abilitato Hyper-V:

| serie                           | Contatore di origine |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

Uso di `% Total Run Time` contatori assicura che la cronologia delle prestazioni attributi tutto l'utilizzo.

Se non è abilitato Hyper-V:

| serie                           | Contatore di origine |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *uguali ai fini dell'utilizzo totale* |

Nonostante la sincronizzazione imperfetta `clusternode.cpu.usage` è sempre `clusternode.cpu.usage.host` plus `clusternode.cpu.usage.guest`.

Con la stessa avvertenza `clusternode.cpu.usage.guest` è sempre la somma di `vm.cpu.usage` per tutte le macchine virtuali nel server host.

Il `memory.*` serie sono (presto disponibile).

  > [!NOTE]
  > I contatori vengono misurati su tutto l'intervallo, non campionato. Ad esempio, se il server è inattivo per 9 secondi ma picchi al 100% della CPU nella seconda 10th, relativo `clusternode.cpu.usage` saranno registrate come il 10% in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività e affidabile al rumore.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare la [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) cmdlet:

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md)
