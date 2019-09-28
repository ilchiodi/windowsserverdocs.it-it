---
title: Spazi di archiviazione diretta cache di lettura in memoria
ms.prod: windows-server
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 83fc923f505531f955fc0131d7dcc1ce98974daa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394094"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>Uso di Spazi di archiviazione diretta con la cache di lettura in memoria CSV
> Si applica a: Windows Server 2016, Windows Server 2019

Questo argomento descrive come usare la memoria di sistema per migliorare le prestazioni di [spazi di archiviazione diretta](storage-spaces-direct-overview.md).

Spazi di archiviazione diretta è compatibile con la cache di lettura in memoria del Volume condiviso cluster (CSV). L'utilizzo della memoria di sistema per la memorizzazione nella cache può migliorare le prestazioni di applicazioni come Hyper-V, che utilizzano I/O senza buffer per accedere ai file VHD o VHDX. (IOs non memorizzato nel buffer sono operazioni che non vengono memorizzate nella cache da Gestione cache di Windows).

Poiché la cache in memoria è Server-Local, migliora la posizione dei dati per le distribuzioni di Spazi di archiviazione diretta iperconvergenti: le letture recenti vengono memorizzate nella cache nello stesso host in cui è in esecuzione la macchina virtuale, riducendo la frequenza con cui le letture passano attraverso la rete. Ciò comporta una latenza più bassa e prestazioni di archiviazione migliori.

## <a name="planning-considerations"></a>Considerazioni sulla pianificazione

La cache di lettura in memoria è particolarmente efficace per i carichi di lavoro a elevato utilizzo di lettura, ad esempio Virtual Desktop Infrastructure (VDI). Viceversa, se il carico di lavoro è estremamente impegnativo per la scrittura, la cache può comportare un sovraccarico maggiore rispetto al valore e deve essere disabilitato.

È possibile utilizzare fino al 80% della memoria fisica totale per la cache di lettura CSV in memoria.

  > [!TIP]
  > Per le distribuzioni iperconvergenti, in cui le risorse di calcolo e di archiviazione vengono eseguite sugli stessi server, prestare attenzione a lasciare memoria sufficiente per le macchine virtuali. Per le distribuzioni con File server di scalabilità orizzontale convergenti (SoFS), con minore contesa per la memoria, questa operazione non è applicabile.

  > [!NOTE]
  > Alcuni strumenti di microbenchmarking come DISKSPD e [VM Fleet](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) possono produrre risultati peggiori con la cache di lettura in memoria CSV abilitata che senza di essa. Per impostazione predefinita, la flotta VM crea 1 10 GiB VHDX per macchina virtuale: circa 1 TiB per le VM 100, quindi esegue letture e scritture *casuali in modo uniforme* . A differenza dei carichi di lavoro reali, le letture non seguono alcun modello prevedibile o ripetitivo, quindi la cache in memoria non è efficace e comporta solo un sovraccarico.

## <a name="configuring-the-in-memory-read-cache"></a>Configurazione della cache di lettura in memoria

La cache di lettura CSV in memoria è disponibile in Windows Server 2016 e Windows Server 2019 con la stessa funzionalità. In Windows Server 2016 è disattivato per impostazione predefinita. Per impostazione predefinita, in Windows Server 2019, con 1 GB allocato.

| Versione del sistema operativo          | Dimensioni predefinite della cache CSV |
|---------------------|------------------------|
| Windows Server 2016 | 0 (disabilitato)           |
| Windows Server 2019 | 1 GiB                   |

Per visualizzare la quantità di memoria allocata tramite PowerShell, eseguire:

```PowerShell
(Get-Cluster).BlockCacheSize
```

Il valore restituito è in mebibytes (MiB) per server. Ad esempio, `1024` rappresenta 1 gibibyte (GiB).

Per modificare la quantità di memoria allocata, modificare questo valore usando PowerShell. Ad esempio, per allocare 2 GiB per server, eseguire:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Affinché le modifiche abbiano effetto immediato, sospendere e riprendere i volumi CSV o spostarli tra i server. Usare, ad esempio, questo frammento di PowerShell per spostare ogni volume condiviso cluster in un altro nodo del server e viceversa:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>Vedere anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
