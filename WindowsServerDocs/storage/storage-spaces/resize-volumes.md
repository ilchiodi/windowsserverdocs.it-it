---
title: Estensione dei volumi in Spazi di archiviazione diretta
ms.assetid: fa48f8f7-44e7-4a0b-b32d-d127eff470f0
ms.prod: windows-server-threshold
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/23/2017
ms.localizationpriority: medium
ms.openlocfilehash: 51f58ec23c55a6cb1664d800d6f4a75dae545899
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824972"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>Estensione dei volumi in Spazi di archiviazione diretta
> Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento fornisce istruzioni per il ridimensionamento dei volumi in [Spazi di archiviazione diretta](storage-spaces-direct-overview.md).

## <a name="prerequisites"></a>Prerequisiti

### <a name="capacity-in-the-storage-pool"></a>Capacità nel pool di archiviazione

Prima di ridimensionare un volume, assicurati di disporre di capacità sufficiente nel pool di archiviazione per contenere il nuovo footprint di dimensioni maggiori. Ad esempio, nel ridimensionamento di un volume con mirroring a tre vie da 1 TB a 2 TB, il footprint aumenta da 3 TB a 6 TB. Per l'esito positivo del ridimensionamento, hai bisogno di almeno (6 - 3) = 3 TB di capacità disponibile nel pool di archiviazione.

### <a name="familiarity-with-volumes-in-storage-spaces"></a>Familiarità con i volumi in Spazi di archiviazione

In Spazi di archiviazione diretta, ogni volume è costituito da diversi oggetti in pila: il volume condiviso cluster, che è un volume, la partizione, il disco, che è un disco virtuale. e uno o più livelli di archiviazione (ove applicabili). Per ridimensionare un volume, dovrai ridimensionare diversi di questi oggetti.

![volumes-in-smapi](media/resize-volumes/volumes-in-smapi.png)

Per acquisire familiarità con tali oggetti, prova a eseguire **Get-** con il sostantivo corrispondente in PowerShell.

Ad esempio: 

```PowerShell
Get-VirtualDisk
```

Per seguire le associazioni tra gli oggetti nello stack, invia pipe un cmdlet **Get-** nel successivo.

Ad esempio, ecco come arrivare da un disco virtuale fino al volume:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-Disk | Get-Partition | Get-Volume 
```

## <a name="step-1--resize-the-virtual-disk"></a>Passaggio 1: ridimensionare il disco virtuale

Il disco virtuale può utilizzare i livelli di archiviazione o meno, a seconda di come è stato creato.

Per verificare, esegui il cmdlet seguente:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier 
```

Se il cmdlet non restituisce valori, il disco virtuale non utilizza i livelli di archiviazione.

### <a name="no-storage-tiers"></a>Nessun livello di archiviazione

Se il disco virtuale non dispone di livelli di archiviazione, puoi ridimensionarlo direttamente tramite il cmdlet **Resize-VirtualDisk**.

Fornisci la nuova dimensione nel parametro **-Size**.

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

Quando ridimensioni **VirtualDisk**, anche **Disk** segue automaticamente e viene ridimensionato.

![Resize-VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

### <a name="with-storage-tiers"></a>Con livelli di archiviazione

Se il disco virtuale utilizza livelli di archiviazione, puoi ridimensionare ogni livello separatamente mediante il cmdlet **Resize-StorageTier**.

Ottieni i nomi dei livelli di archiviazione seguendo le associazioni dal disco virtuale.

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier | Select FriendlyName
```

Quindi, per ogni livello, fornisci le nuove dimensioni nel parametro **-Size**.

```PowerShell
Get-StorageTier <FriendlyName> | Resize-StorageTier -Size <Size>
```

> [!TIP]
> Se i livelli sono tipi di supporti fisici diversi (ad esempio **MediaType = SSD** e **MediaType = HDD**) devi verificare di disporre di capacità di ogni tipo di supporto nel pool di archiviazione sufficiente a contenere il nuovo footprint di dimensioni maggiori di ogni livello.

Quando ridimensioni **StorageTier**, anche **VirtualDisk** e **Disk** seguono automaticamente e vengono ridimensionati.

![Resize-StorageTier](media/resize-volumes/Resize-StorageTier.gif)

## <a name="step-2--resize-the-partition"></a>Passaggio 2: ridimensionare la partizione

Successivamente, ridimensiona la partizione utilizzando il cmdlet **Resize-Partition**. Normalmente il disco virtuale dispone di due partizioni: la prima è riservata e non deve essere modificata; quella da ridimensionare ha **PartitionNumber = 2** e **Type = Basic**.

Fornisci la nuova dimensione nel parametro **-Size**. Ti consigliamo di usare la dimensione massima supportata, come illustrato di seguito.

```PowerShell
# Choose virtual disk
$VirtualDisk = Get-VirtualDisk <FriendlyName>

# Get its partition
$Partition = $VirtualDisk | Get-Disk | Get-Partition | Where PartitionNumber -Eq 2

# Resize to its maximum supported size 
$Partition | Resize-Partition -Size ($Partition | Get-PartitionSupportedSize).SizeMax
```

Quando ridimensioni **Partition**, anche **Volume** e **ClusterSharedVolume** seguono automaticamente e vengono ridimensionati.

![Resize-Partition](media/resize-volumes/Resize-Partition.gif)

La procedura è terminata.

> [!TIP]
> Puoi verificare che il volume ha la nuova dimensione eseguendo **Get-Volume**.

## <a name="see-also"></a>Vedere anche

- [Spazi di archiviazione diretta in Windows Server 2016](storage-spaces-direct-overview.md)
- [Pianificazione di volumi in spazi di archiviazione diretta](plan-volumes.md)
- [Creazione di volumi in spazi di archiviazione diretta](create-volumes.md)
