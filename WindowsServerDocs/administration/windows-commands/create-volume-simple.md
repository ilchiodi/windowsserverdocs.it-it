---
title: Crea volume semplice
description: Windows Commands Topic for create volume simple, che consente di creare un volume semplice sul disco dinamico specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45c707a92692c5531a0e33c9537705558f2ac309
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846894"
---
# <a name="create-volume-simple"></a>Crea volume semplice

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un volume semplice sul disco dinamico specificato.  
  
> [!IMPORTANT]  
> per Windows Vista, questo comando DiskPart è disponibile solo in Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business Edition.
  
## <a name="syntax"></a>Sintassi  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
| Parametro  |                                                                                                                            Descrizione                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dimensioni\=<n>  |                                                                  Le dimensioni del volume in megabyte \(MB\). Se si specifica alcuna dimensione, il nuovo volume occuperà lo spazio rimanente sul disco.                                                                   |
| \=disco <n>  |                                                                                Disco dinamico in cui viene creato il volume. Se non viene specificato alcun disco, viene utilizzato il disco corrente.                                                                                |
| Allinea\=<n> | Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. *n* è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino. |
|   NOERR    |                               solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                |
  
## <a name="remarks"></a>Note  
  
-   Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Per creare un volume di 1000 MB, le dimensioni su disco 1, digitare:  
  
```  
create volume simple size=1000 disk=1  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

