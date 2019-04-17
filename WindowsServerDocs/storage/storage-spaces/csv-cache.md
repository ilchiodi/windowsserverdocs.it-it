---
title: Cache di lettura di archiviazione diretta spazi in memoria
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122054"
---
# Uso di spazi di archiviazione diretta con CSV in memoria cache di lettura
> Si applica a: Windows Server 2016, Windows Server 2019

In questo argomento viene descritto come utilizzare la memoria di sistema per migliorare le prestazioni di [Spazi di archiviazione diretta](storage-spaces-direct-overview.md).

Spazi di archiviazione diretta è compatibili con il Volume condiviso Cluster (CSV) in memoria cache di lettura. Utilizzo della memoria di sistema cache effettui le letture può migliorare le prestazioni per le applicazioni come Hyper-V, che usa privo di buffer dei / o per accedere ai file VHD o VHDX. (Sequenzialmente IOs sono tutte le operazioni che non sono memorizzati nella cache per il gestore della Cache di Windows).

Poiché la cache in memoria è server locale, migliora la località dei dati per le distribuzioni di spazi di archiviazione diretta con iperconvergenza: letture recenti vengono memorizzate nella cache in memoria nello stesso host in cui è in esecuzione nella macchina virtuale, riducendo la frequenza Vai che le letture attraverso la rete. Ciò produce una latenza minore e migliori prestazioni di archiviazione.

## Considerazioni sulla pianificazione

Cache più efficace per la lettura i carichi di lavoro, ad esempio Virtual Desktop Infrastructure (VDI) lettura in memoria. Al contrario, se il carico di lavoro è estremamente con uso intensivo di scrittura, la cache può introdurre un overhead maggiore rispetto al valore e deve essere disabilitata.

È possibile utilizzare fino all'80% della memoria fisica totale per la cache di lettura CSV in memoria.

  > [!TIP]
  > Per le distribuzioni iperconvergenti, in cui di calcolo e archiviazione eseguire sugli stessi server, presta attenzione a lasciare memoria sufficiente per le macchine virtuali. Per le distribuzioni convergenti di File Server di scalabilità orizzontale (SoFS), con meno conflitti di memoria, ciò non è valido.

  > [!NOTE]
  > Alcuni strumenti microbenchmarking come DISKSPD e [Flotta macchina virtuale](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) può produrre peggio risultati con CSV in memoria leggono cache abilitata rispetto a senza di esso. Per impostazione predefinita della macchina virtuale flotta crea un GiB 10 VHDX per ogni macchina virtuale: circa 1 TiB totale per le macchine 100 virtuali- e quindi esegue letture *casuali in modo uniforme* e scrive su di essi. A differenza dei carichi di lavoro reale, le letture non seguono qualsiasi modello prevedibile o ripetitivo, in modo che la cache in memoria non viene applicata e semplicemente comporta un sovraccarico.

## Configurazione in memoria cache di lettura

La cache è disponibile nel Windows Server 2016 e Windows Server 2019 con le stesse funzionalità di lettura la CSV in memoria. In Windows Server 2016, è per impostazione predefinita. In Windows Server 2019, è per impostazione predefinita con 1 GB allocate.

| Versione del sistema operativo          | Dimensioni della cache CSV predefinite |
|---------------------|------------------------|
| WindowsServer 2016 | 0 (disabilitato)           |
| Windows Server 2019 | 1 giB                   |

Per visualizzare la quantità di memoria allocata con PowerShell, eseguire:

```PowerShell
(Get-Cluster).BlockCacheSize
```

Il valore restituito è in mebibytes (MiB) per ogni server. Ad esempio, `1024` rappresenta 1 gibibyte (GiB).

Per modificare la quantità di memoria allocata, modificare questo valore con PowerShell. Ad esempio, per allocare GiB 2 per ogni server, eseguire:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Per le modifiche avranno effetto immediatamente, sospendere, quindi riprendere i volumi condivisi del cluster o li si sposta tra i server. Ad esempio, utilizzare questo frammento di PowerShell per spostare ogni CSV in un altro nodo server e nuovamente:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## Vedi anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
