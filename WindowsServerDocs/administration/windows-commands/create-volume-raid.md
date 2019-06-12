---
title: creare volumi raid
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ca75dd9af441446081cb10743329eb8e42166c0c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434068"
---
# <a name="create-volume-raid"></a>creare volumi raid

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un sistema RAID\-specificato di volume 5 usando tre o più dischi dinamici.  
  
> [!IMPORTANT]  
> Questo comando di DiskPart non è disponibile in qualsiasi edizione di Windows Vista.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|           Parametro           |                                                                                                                                                                                                                                              Descrizione                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           size\=<n>           | La quantità di spazio su disco, in megabyte \(MB\), che verrà occupata dal volume in ciascun disco. Se si specifica alcuna dimensione, il più grande RAID possibili\-verrà creato un volume 5. Il disco con il più piccolo maggiore spazio libero contiguo disponibile determina le dimensioni massime per il RAID\-volume 5 e la stessa quantità di spazio allocata in base a ogni disco. La quantità effettiva di spazio su disco utilizzabile RAID\-volume 5 è minore rispetto alla quantità di spazio su disco combinata perché alcune dello spazio su disco è necessaria per la parità. |
| disk\=<n>,<n>,<n>\[,<n>,...\] |                                                                                                                                               I dischi dinamici in cui creare il RAID\-volume 5. Sono necessari almeno tre dischi dinamici per creare un sistema RAID\-volume 5. Una quantità di spazio pari a **dimensioni\= <n>**  viene allocato su ogni disco.                                                                                                                                                |
|          align\=<n>           |                                                                                                                   Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. *n* è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino.                                                                                                                   |
|             NOERR             |                                                                                                                                                 Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                                                  |
  
## <a name="remarks"></a>Note  
  
-   Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare un sistema RAID\-5 volume di 1000 MB di dimensioni, usando i dischi 1, 2 e 3, tipo:  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

