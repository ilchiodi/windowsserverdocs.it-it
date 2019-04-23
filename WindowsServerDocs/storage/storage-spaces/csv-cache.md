---
title: Cache di lettura di archiviazione diretta di spazi in memoria
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850552"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>Uso di spazi di archiviazione diretta con i CSV in memoria cache di lettura
> Si applica a: Windows Server 2016, Windows Server 2019

In questo argomento viene descritto come utilizzare la memoria di sistema per migliorare le prestazioni di [spazi di archiviazione diretta](storage-spaces-direct-overview.md).

Spazi di archiviazione diretta è compatibili con il Volume condiviso Cluster (CSV) in memoria cache di lettura. Utilizzo della memoria di sistema e di letture della cache può migliorare le prestazioni per applicazioni quali Hyper-V, che utilizza non memorizzato nel buffer i/o per accedere ai file VHD o VHDX. (Non memorizzato nel buffer IOs sono operazioni che non vengono memorizzate nella cache per la gestione della Cache di Windows).

Perché la cache in memoria locale del server, è possibile migliorare la località dei dati per le distribuzioni di spazi di archiviazione diretta iperconvergente: recenti letture vengono memorizzate nella cache in memoria nello stesso host in cui è in esecuzione la macchina virtuale, riducendo la frequenza letture esaminato nella rete. Ciò comporta una latenza più bassa e prestazioni migliori di archiviazione.

## <a name="planning-considerations"></a>Considerazioni sulla pianificazione

In memoria di lettura della cache è più efficace per i carichi di lavoro a elevato utilizzo di lettura, ad esempio Virtual Desktop Infrastructure (VDI). Viceversa, se il carico di lavoro è estremamente elevato della scrittura, la cache può comportare un sovraccarico maggiore rispetto al valore e deve essere disabilitata.

È possibile usare fino all'80% della memoria fisica totale per cache di lettura di CSV in memoria.

  > [!TIP]
  > Per le distribuzioni iperconvergenti, in cui calcolo e archiviazione eseguiti negli stessi server, prestare attenzione a lasciare la memoria sufficiente per le macchine virtuali. Per le distribuzioni convergenti di File Server di scalabilità orizzontale (SoFS), con minore contesa per la memoria, ciò non si applica.

  > [!NOTE]
  > Alcuni strumenti microbenchmarking come DISKSPD e [parco macchine Virtuali](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) può produrre risultati peggiori con il volume condiviso cluster in memoria di lettura cache abilitata rispetto a senza di essa. Per impostazione predefinita parco macchine Virtuali viene creato uno VHDX per ogni macchina virtuale: circa 1 TiB di 10 GiB totale per 100 macchine virtuali ed esegue poi *casuale uniformemente* legge e scrive in questi. A differenza dei carichi di lavoro reali, le letture non seguono qualsiasi modello stimabile o ripetitivo, in modo che la cache in memoria non è efficace e appena comporta un sovraccarico.

## <a name="configuring-the-in-memory-read-cache"></a>Configurazione della memoria nella cache di lettura

CSV in memoria di lettura della cache è disponibile in Windows Server 2016 e Windows Server 2019 con la stessa funzionalità. In Windows Server 2016, è per impostazione predefinita. In Windows Server 2019, è per impostazione predefinita con 1 GB allocati.

| Versione sistema operativo          | Dimensione della cache CSV predefinito |
|---------------------|------------------------|
| Windows Server 2016 | 0 (disabilitato)           |
| Windows Server 2019 | 1 GiB                   |

Per visualizzare la quantità di memoria allocata tramite PowerShell, eseguire:

```PowerShell
(Get-Cluster).BlockCacheSize
```

Il valore restituito è in mebibytes (MiB) per ogni server. Ad esempio, `1024` rappresenta 1 gibibyte (GiB).

Per modificare la quantità di memoria viene allocata, modificare questo valore tramite PowerShell. Ad esempio, per allocare GiB 2 per ogni server, eseguire:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Le modifiche vengono applicate immediatamente, sospendere e riprendere i volumi condivisi del cluster o spostarli tra i server. Ad esempio, usare questo frammento di PowerShell per spostare ogni volume condiviso cluster in un altro nodo del server e quindi nuovamente:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>Vedere anche

- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)
