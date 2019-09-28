---
title: Crea mirror del volume
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72ecc4e0ede163857c47c5b7013aacdd49719ac8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378875"
---
# <a name="create-volume-mirror"></a>Crea mirror del volume

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

consente di creare un mirror del volume utilizzando i due dischi dinamici specificati.  
  
> [!NOTE]  
> Questo comando è disponibile solo in Windows 7 e Windows Server 2008 R2.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|         Parametro         |                                                                                                                                     Descrizione                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         dimensioni @ no__t-0 @ no__t-1         |                 Specifica la quantità di spazio su disco, in megabyte \(MB\), che verrà occupata dal volume in ciascun disco. Se si specifica alcuna dimensione, il nuovo volume occuperà lo spazio libero rimanente nel disco più piccolo e un'uguale quantità di spazio su ogni disco successivo.                 |
| Disk @ no__t-0 @ no__t-1, <n> @ no__t-3, <n>,... \] |                       Specifica i dischi dinamici in cui viene creato il volume con mirroring. Sono necessari due dischi dinamici per creare un volume con mirroring. Una quantità di spazio che è uguale a quella specificata con il **dimensioni** parametro allocato su ogni disco.                        |
|        align @ no__t-0 @ no__t-1         | Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. Questo parametro viene in genere utilizzato con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. *n* è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino. |
|           NOERR           |                                        Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un errore.                                         |
  
## <a name="remarks"></a>Note  
  
-   Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare un volume con mirroring di 1000 MB di dimensioni, sui dischi 1 e 2, digitare:  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

