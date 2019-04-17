---
title: Cronologia delle prestazioni per le macchine virtuali
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239218"
---
# Cronologia delle prestazioni per le macchine virtuali

> Si applica a: Windows Server Insider Preview

Questo argomento secondario della [cronologia delle prestazioni di spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolti per le macchine virtuali (VM). Cronologia delle prestazioni è disponibile per ogni in esecuzione, VM in cluster.

   > [!NOTE]
   > Potrebbero richiedere alcuni minuti iniziare per le macchine virtuali appena create o rinominate raccolta.

## Le unità e i nomi di serie

Queste serie vengono raccolti per ogni macchina virtuale idonea:

| Serie                            | Unit             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | %          |
| `vm.memory.assigned`              |  byte            |
| `vm.memory.available`             |  byte            |
| `vm.memory.maximum`               |  byte            |
| `vm.memory.minimum`               |  byte            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               |  byte            |
| `vm.memory.total`                 |  byte            |
| `vmnetworkadapter.bandwidth.inbound`  | bit al secondo |
| `vmnetworkadapter.bandwidth.outbound` | bit al secondo |
| `vmnetworkadapter.bandwidth.total`    | bit al secondo |

Inoltre, tutte le serie di disco rigido virtuale (VHD), ad esempio `vhd.iops.total`, vengono aggregati per ogni file VHD collegato alla macchina virtuale.

## Come interpretare


| Serie                            | Descrizione                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Percentuale della macchina virtuale è l'utilizzo di processori del server il relativo host.                                   |
| `vm.memory.assigned`              | La quantità di memoria assegnata alla macchina virtuale.                                                      |
| `vm.memory.available`             | La quantità di memoria che rimane disponibile, della quantità assegnata.                                       |
| `vm.memory.maximum`               | Se usi la memoria dinamica, questa è la quantità massima di memoria che può essere assegnata alla macchina virtuale. |
| `vm.memory.minimum`               | Se usi la memoria dinamica, questa è la quantità minima di memoria che può essere assegnata alla macchina virtuale. |
| `vm.memory.pressure`              | Il rapporto di memoria richiesta dalla macchina virtuale su memoria allocata per la macchina virtuale.            |
| `vm.memory.startup`               | La quantità di memoria necessaria per la macchina virtuale iniziare.                                            |
| `vm.memory.total`                 | Memoria totale. |
| `vmnetworkadapter.bandwidth.inbound`  | Tasso di dati ricevuti su tutti i relativi schede di rete virtuale dalla macchina virtuale.                        |
| `vmnetworkadapter.bandwidth.outbound` | Frequenza di dati inviati dalla macchina virtuale su tutti i relativi schede di rete virtuale.                            |
| `vmnetworkadapter.bandwidth.total`    | Frequenza totale dei dati ricevuti o inviati dalla macchina virtuale su tutti i relativi schede di rete virtuale.          |

   > [!NOTE]
   > Contatori vengono misurati nell'intero intervallo, non campionati. Ad esempio, se la macchina virtuale è inattiva per 9 secondi, ma picchi di usare 50% delle CPU host nella seconda 10, il relativo `vm.cpu.usage` verrà registrato come 5% in Media durante questo intervallo di 10 secondi. Ciò garantisce la cronologia delle prestazioni acquisisce tutte le attività e robusto al rumore.

## Utilizzo di PowerShell

Usa il cmdlet [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) :

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > Il cmdlet Get-VM restituisce solo le macchine virtuali nel server locale (o specificato), non all'interno del cluster.

## Vedi anche

- [Cronologia delle prestazioni di spazi di archiviazione diretta](performance-history.md)
