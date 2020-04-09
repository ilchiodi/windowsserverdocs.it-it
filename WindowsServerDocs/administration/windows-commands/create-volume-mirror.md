---
title: Crea mirror del volume
description: Windows Commands argomento per create volume mirror, che consente di creare un mirror del volume usando i due dischi dinamici specificati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa5ea72eb0edacb841f32126f31a257b0573dd6d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846974"
---
# <a name="create-volume-mirror"></a>Crea mirror del volume

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un mirror di volume utilizzando due dischi dinamici specificati.  
  
> [!NOTE]  
> Questo comando è disponibile solo in Windows 7 e Windows Server 2008 R2.

## <a name="syntax"></a>Sintassi  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
|         Parametro         |                                                                                                                                     Descrizione                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         dimensioni\=<n>         |                 Specifica la quantità di spazio su disco, in megabyte \(MB\), che verrà occupata dal volume in ciascun disco. Se si specifica alcuna dimensione, il nuovo volume occuperà lo spazio libero rimanente nel disco più piccolo e un'uguale quantità di spazio su ogni disco successivo.                 |
| disco\=<n>,<n>\[,<n>,...\] |                       Specifica i dischi dinamici in cui viene creato il volume con mirroring. Sono necessari due dischi dinamici per creare un volume con mirroring. Una quantità di spazio che è uguale a quella specificata con il **dimensioni** parametro allocato su ogni disco.                        |
|        Allinea\=<n>         | Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. Questo parametro viene in genere utilizzato con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. *n* è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino. |
|           NOERR           |                                        Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un errore.                                         |
  
## <a name="remarks"></a>Note  
  
-   Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Per creare un volume con mirroring di 1000 MB di dimensioni, sui dischi 1 e 2, digitare:  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

