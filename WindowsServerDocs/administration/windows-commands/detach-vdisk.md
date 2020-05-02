---
title: detach vdisk
description: Argomento di riferimento per detach vdisk, che consente di arrestare il disco rigido virtuale (VHD) selezionato come unità disco rigido locale nel computer host.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5e64559650597eb8d15e28075f74704fdf338a6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716664"
---
# <a name="detach-vdisk"></a>detach vdisk

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arresta la visualizzazione del disco rigido virtuale (VHD) selezionato come unità disco rigido locale nel computer host. Quando viene reso non visibile, il VHD può essere copiato in percorsi diversi.  
  
> [!NOTE]  
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintassi  
  
```  
detach vdisk [noerr]  
```  
  
#### <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|NOERR|Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="remarks"></a>Osservazioni  
  
-   Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.  
  
## <a name="examples"></a>Esempi  
Per scollegare il disco rigido Virtuale selezionato, digitare:  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [compatta vdisk](compact-vdisk.md)  

-   [Dettagli vdisk](detail-vdisk.md)  
  
-   [Espandi vdisk](expand-vdisk.md)  
  
-   [Disco virtuale di tipo merge](merge-vdisk.md)  
  
-   [Seleziona vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

