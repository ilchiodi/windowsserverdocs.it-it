---
title: Crea RAID del volume
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a3c13cb5b78ae3e771b461a35a7130a48e7ec01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378858"
---
# <a name="create-volume-raid"></a>Crea RAID del volume

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

consente di creare un volume RAID @ no__t-05 utilizzando tre o più dischi dinamici specificati.  
  
> [!IMPORTANT]  
> Questo comando di DiskPart non è disponibile in qualsiasi edizione di Windows Vista.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|           Parametro           |                                                                                                                                                                                                                                              Descrizione                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           dimensioni @ no__t-0 @ no__t-1           | La quantità di spazio su disco, in megabyte \(MB\), che verrà occupata dal volume in ciascun disco. Se non viene specificata alcuna dimensione, verrà creato il volume RAID @ no__t-05 più grande possibile. Il disco con lo spazio libero contiguo disponibile più piccolo determina le dimensioni per il volume RAID @ no__t-05 e viene allocata la stessa quantità di spazio da ogni disco. La quantità effettiva di spazio su disco utilizzabile nel volume RAID @ no__t-05 è inferiore alla quantità combinata di spazio su disco, perché per parità è necessario uno spazio su disco. |
| Disk @ no__t-0 @ no__t-1, <n>, <n> @ no__t-4, <n>,... \] |                                                                                                                                               Dischi dinamici su cui creare il volume RAID @ no__t-05. Sono necessari almeno tre dischi dinamici per creare un volume RAID @ no__t-05. In ogni disco viene allocata una quantità di spazio uguale a **size @ no__t-1 @ no__t-2** .                                                                                                                                                |
|          align @ no__t-0 @ no__t-1           |                                                                                                                   Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. *n* è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino.                                                                                                                   |
|             NOERR             |                                                                                                                                                 Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                                                  |
  
## <a name="remarks"></a>Note  
  
-   Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare un volume RAID @ no__t-05 di 1000 MB di dimensioni, usando i dischi 1, 2 e 3, digitare:  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

