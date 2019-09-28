---
title: Estensione dei volumi in Spazi di archiviazione diretta
description: Come ridimensionare i volumi in Spazi di archiviazione diretta usando l'interfaccia di amministrazione di Windows e PowerShell.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 05/07/2019
ms.openlocfilehash: 20482fe1728b12d4fe56dcfa397352fbb4b4f981
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366089"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>Estensione dei volumi in Spazi di archiviazione diretta
> Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento fornisce istruzioni per il ridimensionamento dei volumi in un cluster [spazi di archiviazione diretta](storage-spaces-direct-overview.md) usando l'interfaccia di amministrazione di Windows.

Guarda un breve video su come ridimensionare un volume.

> [!VIDEO https://www.youtube-nocookie.com/embed/hqyBzipBoTI]

## <a name="extending-volumes-using-windows-admin-center"></a>Estensione di volumi con l'interfaccia di amministrazione di Windows

1. Nell'interfaccia di amministrazione di Windows connettersi a un cluster Spazi di archiviazione diretta e quindi selezionare **volumi** dal riquadro **strumenti** .
2. Nella pagina volumi selezionare la scheda **inventario** , quindi selezionare il volume che si desidera ridimensionare.

    Nella pagina dettagli volume è indicata la capacità di archiviazione per il volume. È anche possibile aprire la pagina dei dettagli sui volumi direttamente dal dashboard. Nel riquadro avvisi del Dashboard selezionare l'avviso, che informa l'utente se la capacità di archiviazione di un volume è insufficiente e quindi selezionare **Vai al volume**.

4. Nella parte superiore della pagina dei dettagli volumi selezionare **Ridimensiona**.
5. Immettere una nuova dimensione maggiore e quindi fare clic su **Ridimensiona**.

    Nella pagina Dettagli volumi è indicata la capacità di archiviazione più ampia per il volume e l'avviso sul dashboard è stato cancellato.

## <a name="extending-volumes-using-powershell"></a>Estensione di volumi con PowerShell

### <a name="capacity-in-the-storage-pool"></a>Capacità nel pool di archiviazione

Prima di ridimensionare un volume, assicurati di disporre di capacità sufficiente nel pool di archiviazione per contenere il nuovo footprint di dimensioni maggiori. Ad esempio, nel ridimensionamento di un volume con mirroring a tre vie da 1 TB a 2 TB, il footprint aumenta da 3 TB a 6 TB. Per l'esito positivo del ridimensionamento, hai bisogno di almeno (6 - 3) = 3 TB di capacità disponibile nel pool di archiviazione.

### <a name="familiarity-with-volumes-in-storage-spaces"></a>Familiarità con i volumi in Spazi di archiviazione

In Spazi di archiviazione diretta, ogni volume è costituito da diversi oggetti in pila: il volume condiviso cluster, che è un volume, la partizione, il disco, che è un disco virtuale. e uno o più livelli di archiviazione (ove applicabili). Per ridimensionare un volume, dovrai ridimensionare diversi di questi oggetti.

![volumes-in-smapi](media/resize-volumes/volumes-in-smapi.png)

Per acquisire familiarità con tali oggetti, prova a eseguire **Get-** con il sostantivo corrispondente in PowerShell.

Esempio:

```PowerShell
Get-VirtualDisk
```

Per seguire le associazioni tra gli oggetti nello stack, invia pipe un cmdlet **Get-** nel successivo.

Ad esempio, ecco come arrivare da un disco virtuale fino al volume:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-Disk | Get-Partition | Get-Volume 
```

### <a name="step-1--resize-the-virtual-disk"></a>Passaggio 1: ridimensionare il disco virtuale

Il disco virtuale può utilizzare i livelli di archiviazione o meno, a seconda di come è stato creato.

Per verificare, esegui il cmdlet seguente:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier 
```

Se il cmdlet non restituisce valori, il disco virtuale non utilizza i livelli di archiviazione.

#### <a name="no-storage-tiers"></a>Nessun livello di archiviazione

Se il disco virtuale non dispone di livelli di archiviazione, puoi ridimensionarlo direttamente tramite il cmdlet **Resize-VirtualDisk**.

Fornisci la nuova dimensione nel parametro **-Size**.

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

Quando ridimensioni **VirtualDisk**, anche **Disk** segue automaticamente e viene ridimensionato.

![Resize-VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

#### <a name="with-storage-tiers"></a>Con livelli di archiviazione

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

### <a name="step-2--resize-the-partition"></a>Passaggio 2: ridimensionare la partizione

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
- [Pianificazione di volumi in Spazi di archiviazione diretta](plan-volumes.md)
- [Creazione di volumi in Spazi di archiviazione diretta](create-volumes.md)
- [Eliminazione di volumi in Spazi di archiviazione diretta](delete-volumes.md)
