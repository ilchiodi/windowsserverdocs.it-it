---
title: Crea Stripe del volume
description: Argomento di riferimento per il comando Crea Stripe volume, che consente di creare un volume con striping utilizzando due o più dischi dinamici specificati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 315d7a08dfcf64ae09501975b5f5bdb72c37754e
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993211"
---
# <a name="create-volume-stripe"></a>Crea Stripe del volume

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un volume con striping utilizzando due o più dischi dinamici specificati. Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.

## <a name="syntax"></a>Sintassi

```
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |  -----------|
| dimensioni =`<n>` | Quantità di spazio su disco, in megabyte (MB), che il volume occuperà in ogni disco. Se si specifica alcuna dimensione, il nuovo volume occuperà lo spazio libero rimanente nel disco più piccolo e un'uguale quantità di spazio su ogni disco successivo. |
| disco =`<n>,<n>[,<n>,...]` | I dischi dinamici in cui viene creato il volume con striping. Sono necessari almeno due dischi dinamici per creare un volume con striping. In ogni disco viene allocata `size=<n>` una quantità di spazio pari a. |
| Allinea =`<n>` | Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. In genere utilizzata con le matrici numero RAID unità logica (LUN) hardware per migliorare le prestazioni. `<n>`numero di kilobyte (KB) dall'inizio del disco al limite di allineamento più vicino. |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per creare un volume con striping di 1000 MB di dimensioni, sui dischi 1 e 2, digitare:

```
create volume stripe size=1000 disk=1,2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Crea comando](create.md)
