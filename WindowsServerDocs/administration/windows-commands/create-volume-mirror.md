---
title: Crea mirror del volume
description: Argomento di riferimento per il comando crea mirror del volume, che consente di creare un mirror del volume utilizzando i due dischi dinamici specificati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: be6e4496876636351b6e0853626a9ff9bb421f18
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993253"
---
# <a name="create-volume-mirror"></a>Crea mirror del volume

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un mirror di volume utilizzando due dischi dinamici specificati. Una volta creato il volume, lo stato attivo passa automaticamente al nuovo volume.

## <a name="syntax"></a>Sintassi

```
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| dimensioni =`<n>` | Specifica la quantità di spazio su disco, in megabyte (MB), che il volume occuperà in ogni disco. Se si specifica alcuna dimensione, il nuovo volume occuperà lo spazio libero rimanente nel disco più piccolo e un'uguale quantità di spazio su ogni disco successivo. |
| disk =`<n>`,`<n>`[`,<n>,...`] | Specifica i dischi dinamici in cui viene creato il volume con mirroring. Sono necessari due dischi dinamici per creare un volume con mirroring. Una quantità di spazio che è uguale a quella specificata con il **dimensioni** parametro allocato su ogni disco. |
| Allinea =`<n>` | Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. Questo parametro viene in genere usato con matrici di numeri di unità logica (LUN) RAID hardware per migliorare le prestazioni. `<n>`numero di kilobyte (KB) dall'inizio del disco al limite di allineamento più vicino. |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un errore. |

## <a name="examples"></a>Esempi

Per creare un volume con mirroring di 1000 MB di dimensioni, sui dischi 1 e 2, digitare:

```
create volume mirror size=1000 disk=1,2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Crea comando](create.md)
