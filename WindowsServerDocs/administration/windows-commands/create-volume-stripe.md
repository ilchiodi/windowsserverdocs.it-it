---
title: Crea Stripe del volume
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46c1367b5667294a7a9df742861a011090e7a337
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379141"
---
# <a name="create-volume-stripe"></a>Crea Stripe del volume

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

consente di creare un volume con striping utilizzando due o più dischi dinamici specificati.  
  
> [!IMPORTANT]  
> per Windows Vista, questo comando DiskPart è disponibile solo in Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business Edition.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|         Parametro         |                                                                                                                            Descrizione                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         dimensioni @ no__t-0 @ no__t-1         |             La quantità di spazio su disco, in megabyte \(MB\), che verrà occupata dal volume in ciascun disco. Se si specifica alcuna dimensione, il nuovo volume occuperà lo spazio libero rimanente nel disco più piccolo e un'uguale quantità di spazio su ogni disco successivo.             |
| Disk @ no__t-0 @ no__t-1, <n> @ no__t-3, <n>,... \] |                                  I dischi dinamici in cui viene creato il volume con striping. Sono necessari almeno due dischi dinamici per creare un volume con striping. In ogni disco viene allocata una quantità di spazio uguale a **size @ no__t-1 @ no__t-2** .                                   |
|        align @ no__t-0 @ no__t-1         | Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. *n* è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino. |
|           NOERR           |                               Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                |
  
## <a name="remarks"></a>Note  
  
-   Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare un volume con striping di 1000 MB di dimensioni, sui dischi 1 e 2, digitare:  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

