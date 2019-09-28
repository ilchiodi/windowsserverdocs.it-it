---
title: Seleziona vdisk
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bfa6450d1704cde1e5ff2a50a8e3b61a30d0766
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384187"
---
# <a name="select-vdisk"></a>Seleziona vdisk

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleziona il disco rigido virtuale specificato \(VHD @ no__t-1 e sposta lo stato attivo a esso.  
  
> [!NOTE]  
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.  
  
## <a name="syntax"></a>Sintassi  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|file @ no__t-0 @ no__t-1|Specifica il nome di file e percorso completo di un file di disco rigido Virtuale esistente.|  
|NOERR|Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="BKMK_examples"></a>Esempi  
Per spostare lo stato attivo per il disco rigido Virtuale denominato test. vhd, digitare:  
  
```  
select vdisk file="c:\test\test.vhd"  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
-   [Connetti vdisk](attach-vdisk.md)  
  
-   [compatta vdisk](compact-vdisk.md)  
  
  
  
-   [Scollega vdisk](detach-vdisk.md)  
  
-   [Dettagli vdisk](detail-vdisk.md)  
  
-   [Espandi vdisk](expand-vdisk.md)  
  
-   [Unisci vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

