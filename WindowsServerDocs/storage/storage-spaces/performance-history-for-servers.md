---
title: Cronologia prestazioni per i server
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: cf4bdabb132c832370e5dffec215c24b54aebdd7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856194"
---
# <a name="performance-history-for-servers"></a>Cronologia prestazioni per i server

> Si applica a: Windows Server 2019

Questo argomento secondario della [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per i server. La cronologia delle prestazioni è disponibile per tutti i server del cluster.

   > [!NOTE]
   > Non è possibile raccogliere la cronologia delle prestazioni per un server che non è attivo. La raccolta verrà ripresa automaticamente quando il server verrà riavviato.

## <a name="series-names-and-units"></a>Unità e nomi di serie

Queste serie vengono raccolte per ogni server idoneo:

| Serie                           | Unità    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | percent |
| `clusternode.cpu.usage.guest`    | percent |
| `clusternode.cpu.usage.host`     | percent |
| `clusternode.memory.total`       | byte   |
| `clusternode.memory.available`   | byte   |
| `clusternode.memory.usage`       | byte   |
| `clusternode.memory.usage.guest` | byte   |
| `clusternode.memory.usage.host`  | byte   |

Inoltre, le serie di unità, ad esempio `physicaldisk.size.total` vengono aggregate per tutte le unità idonee collegate al server e le serie di schede di rete, ad esempio `networkadapter.bytes.total`, vengono aggregate per tutte le schede di rete idonee collegate al server.

## <a name="how-to-interpret"></a>Come interpretare

| Serie                           | Come interpretare                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Percentuale del tempo del processore non inattiva.                        |
| `clusternode.cpu.usage.guest`    | Percentuale di tempo del processore utilizzata per la richiesta Guest (macchina virtuale). |
| `clusternode.cpu.usage.host`     | Percentuale del tempo del processore usato per la richiesta dell'host.                    |
| `clusternode.memory.total`       | Memoria fisica totale del server.                              |
| `clusternode.memory.available`   | Memoria disponibile del server.                                   |
| `clusternode.memory.usage`       | Memoria allocata (non disponibile) del server.                   |
| `clusternode.memory.usage.guest` | Memoria allocata alla richiesta Guest (macchina virtuale).               |
| `clusternode.memory.usage.host`  | Memoria allocata alla richiesta host.                                  |

## <a name="where-they-come-from"></a>Da dove provengono

La serie `cpu.*` viene raccolta da contatori delle prestazioni diversi a seconda che Hyper-V sia abilitato o meno.

Se Hyper-V è abilitato:

| Serie                           | Contatore di origine |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

L'utilizzo dei contatori `% Total Run Time` garantisce la cronologia delle prestazioni per tutti gli attributi.

Se Hyper-V non è abilitato:

| Serie                           | Contatore di origine |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *uguale all'utilizzo totale* |

Indipendentemente dalla sincronizzazione imperfetta, `clusternode.cpu.usage` è sempre `clusternode.cpu.usage.host` più `clusternode.cpu.usage.guest`.

Con la stessa avvertenza, `clusternode.cpu.usage.guest` è sempre la somma di `vm.cpu.usage` per tutte le macchine virtuali nel server host.

La serie `memory.*` è disponibile a breve.

  > [!NOTE]
  > I contatori sono misurati nell'intero intervallo, non campionati. Se, ad esempio, il server è inattivo per 9 secondi ma viene raggiunto il 100% della CPU nel decimo secondo, la relativa `clusternode.cpu.usage` verrà registrata come media 10% durante questo intervallo di 10 secondi. Ciò garantisce che la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile per il rumore.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare il cmdlet [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia prestazioni per Spazi di archiviazione diretta](performance-history.md)
