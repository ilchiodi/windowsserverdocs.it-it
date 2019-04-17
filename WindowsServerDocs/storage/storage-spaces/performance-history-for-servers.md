---
title: Cronologia delle prestazioni per i server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894250"
---
# <a name="performance-history-for-servers"></a>Cronologia delle prestazioni per i server

> Si applica a: Windows Server persona interna Preview

In questo argomento secondaria di [cronologia delle prestazioni di archiviazione spazi diretto](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolti per i server. La cronologia delle prestazioni è disponibile per tutti i server del cluster.

   > [!NOTE]
   > Non è possibile raccogliere la cronologia delle prestazioni per un server che non è attivo. Insieme riprenderà automaticamente quando il server verrà ripristinato.

## <a name="series-names-and-units"></a>Unità di misura e i nomi delle serie

Le serie vengono raccolti per ogni server idonei:

| Serie                           | Unit    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | % |
| `clusternode.cpu.usage.guest`    | % |
| `clusternode.cpu.usage.host`     | % |
| `clusternode.memory.total`       |  byte   |
| `clusternode.memory.available`   |  byte   |
| `clusternode.memory.usage`       |  byte   |
| `clusternode.memory.usage.guest` |  byte   |
| `clusternode.memory.usage.host`  |  byte   |

Inoltre, unità come serie `physicaldisk.size.total` aggregati per tutte le unità idonei collegate al server e serie di schede di rete, ad esempio `networkadapter.bytes.total` vengono aggregati per tutte le schede di rete idonee collegate al server.

## <a name="how-to-interpret"></a>In che modo interpretare

| Serie                           | In che modo interpretare                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Percentuale di tempo del processore non è inattivo.                        |
| `clusternode.cpu.usage.guest`    | Percentuale di tempo del processore utilizzata per la domanda guest (macchina virtuale). |
| `clusternode.cpu.usage.host`     | Percentuale di tempo del processore utilizzata per la richiesta di host.                    |
| `clusternode.memory.total`       | Memoria fisica totale del server.                              |
| `clusternode.memory.available`   | La memoria disponibile del server.                                   |
| `clusternode.memory.usage`       | La memoria allocata (non disponibile) del server.                   |
| `clusternode.memory.usage.guest` | La memoria allocata per richiesta guest (macchina virtuale).               |
| `clusternode.memory.usage.host`  | La memoria allocata per proposte di host.                                  |

## <a name="where-they-come-from"></a>Dove provengono da

Il `cpu.*` serie vengono raccolti dai contatori delle prestazioni diverso a seconda se è abilitata la tecnologia Hyper-V.

Se è abilitata la tecnologia Hyper-V:

| Serie                           | Contatori di origine |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

Utilizzo di `% Total Run Time` contatori garantisce che la cronologia delle prestazioni degli attributi di tutti i dati di utilizzo.

Se non è abilitato Hyper-V:

| Serie                           | Contatori di origine |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *come per utilizzo totale* |

Nonostante la sincronizzazione imperfetta, `clusternode.cpu.usage` è sempre `clusternode.cpu.usage.host` plus `clusternode.cpu.usage.guest`.

Con la stessa avvertenza, `clusternode.cpu.usage.guest` è sempre la somma di `vm.cpu.usage` per tutte le macchine virtuali sul server host.

Il `memory.*` serie vengono (disponibile a breve).

  > [!NOTE]
  > Contatori vengono espressi in tutto l'intervallo, non viene eseguito. Ad esempio, se il server è inattivo per 9 secondi ma picchi al 100% della CPU nel secondo 10, il relativo `clusternode.cpu.usage` verrà registrato come pari al 10% in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile di rumore.

## <a name="usage-in-powershell"></a>Utilizzo di PowerShell

Utilizzare il cmdlet [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedi anche

- [Cronologia delle prestazioni di archiviazione spazi diretto](performance-history.md)
