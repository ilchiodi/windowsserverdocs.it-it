---
title: Crea RAID del volume
description: Argomento di riferimento per create volume RAID, che consente di creare un volume RAID-5 utilizzando tre o più dischi dinamici specificati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15a165439d0b19023fa5270eb372a17af8d558a4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716941"
---
# <a name="create-volume-raid"></a>Crea RAID del volume

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di creare un volume RAID-5 utilizzando tre o più dischi dinamici specificati.  

> [!IMPORTANT]  
> Questo comando di DiskPart non è disponibile in qualsiasi edizione di Windows Vista.

## <a name="syntax"></a>Sintassi  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
|           Parametro           |                                                                                                                                                                                                                                              Descrizione                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           dimensioni\=<n>           | La quantità di spazio su disco, in megabyte \(MB\), che verrà occupata dal volume in ciascun disco. Se non si specifica alcuna dimensione, verrà creato il\-volume RAID 5 massimo possibile. Il disco con lo spazio libero contiguo disponibile più piccolo determina le dimensioni per il\-volume RAID 5 e viene allocata la stessa quantità di spazio da ogni disco. La quantità effettiva di spazio su disco utilizzabile nel\-volume RAID 5 è inferiore alla quantità combinata di spazio su disco, perché per parità è necessario uno spazio su disco. |
| disco\=<n>,<n><n>,\[,<n>,...\] |                                                                                                                                               Dischi dinamici su cui creare il volume RAID\-5. Sono necessari almeno tre dischi dinamici per creare un volume RAID\-5. In ogni disco viene allocata una quantità di spazio uguale a **size\= ** .                                                                                                                                                |
|          allineare\=<n>           |                                                                                                                   Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. *n* è il numero di kilobyte \(KB\) dall'inizio del disco al limite di allineamento più vicino.                                                                                                                   |
|             NOERR             |                                                                                                                                                 solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                                                  |
  
## <a name="remarks"></a>Osservazioni  
  
-   Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.  
  
## <a name="examples"></a>Esempi  
Per creare un volume\-RAID 5 di 1000 MB di dimensioni, usando i dischi 1, 2 e 3, digitare:  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

