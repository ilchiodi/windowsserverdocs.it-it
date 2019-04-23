---
title: creare volumi semplici
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d754a6b5788656132459a0b4a42954e9084f26bb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836502"
---
# <a name="create-volume-simple"></a>creare volumi semplici

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un volume semplice sul disco dinamico specificato.  
  
> [!IMPORTANT]  
> per Windows Vista, questo comando DiskPart è solo disponibile nelle edizioni di Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|size\=<n>|Le dimensioni del volume in megabyte \(MB\). Se si specifica alcuna dimensione, il nuovo volume occuperà lo spazio rimanente sul disco.|  
|disk\=<n>|Disco dinamico in cui viene creato il volume. Se non viene specificato alcun disco, viene utilizzato il disco corrente.|  
|align\=<n>|Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. *n* è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino.|  
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="remarks"></a>Note  
  
-   Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare un volume di 1000 MB, le dimensioni su disco 1, digitare:  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

  
