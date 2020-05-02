---
title: Seleziona vdisk
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68f60f8f43a33d498c40daa3b9994887f12037bb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721998"
---
# <a name="select-vdisk"></a>Seleziona vdisk

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleziona il VHD \(\) del disco rigido virtuale specificato e sposta lo stato attivo a esso.  
  
> [!NOTE]  
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintassi  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|file\=<full path>|Specifica il nome di file e percorso completo di un file di disco rigido Virtuale esistente.|  
|NOERR|Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="examples"></a>Esempi  
Per spostare lo stato attivo per il disco rigido Virtuale denominato test. vhd, digitare:  
  
```  
select vdisk file=c:\test\test.vhd  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [compatta vdisk](compact-vdisk.md)  
  
  
  
-   [Scollega disco virtuale](detach-vdisk.md)  
  
-   [Dettagli vdisk](detail-vdisk.md)  
  
-   [Espandi vdisk](expand-vdisk.md)  
  
-   [Disco virtuale di tipo merge](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

