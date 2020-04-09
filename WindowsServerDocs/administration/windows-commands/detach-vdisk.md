---
title: detach vdisk
description: Windows Commands Topic for detach vdisk, che interrompe la visualizzazione del disco rigido virtuale (VHD) selezionato come unità disco rigido locale nel computer host.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14eb66031841624156afb03f492e2afce5bc56f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846504"
---
# <a name="detach-vdisk"></a>detach vdisk

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
  
## <a name="remarks"></a>Note  
  
-   Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Per scollegare il disco rigido Virtuale selezionato, digitare:  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [Connetti vdisk](attach-vdisk.md)  
  
-   [compatta vdisk](compact-vdisk.md)  

-   [Dettagli vdisk](detail-vdisk.md)  
  
-   [Espandi vdisk](expand-vdisk.md)  
  
-   [Unisci vdisk](merge-vdisk.md)  
  
-   [Seleziona vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

