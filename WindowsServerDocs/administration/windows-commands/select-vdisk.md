---
title: Seleziona vdisk
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65e186413bebbf467cd4c2033d274badd1fbea80
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834744"
---
# <a name="select-vdisk"></a>Seleziona vdisk

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleziona il disco rigido virtuale specificato \(\) VHD e sposta lo stato attivo a esso.  
  
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
  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Per spostare lo stato attivo per il disco rigido Virtuale denominato test. vhd, digitare:  
  
```  
select vdisk file=c:\test\test.vhd  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [Connetti vdisk](attach-vdisk.md)  
  
-   [compatta vdisk](compact-vdisk.md)  
  
  
  
-   [Scollega vdisk](detach-vdisk.md)  
  
-   [Dettagli vdisk](detail-vdisk.md)  
  
-   [Espandi vdisk](expand-vdisk.md)  
  
-   [Unisci vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

