---
title: tipo FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb254d1c9b17ac6baadf6b84702d2812f1117a93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375903"
---
# <a name="ftp-type"></a>FTP: tipo

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta o Visualizza il tipo di trasferimento di file.   
## <a name="syntax"></a>Sintassi  
```  
type [<typeName>]  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |            Descrizione            |
|--------------|-----------------------------------|
| [<typeName>] | Specifica il tipo di trasferimento di file. |

## <a name="remarks"></a>Note  
- Se *typeName* non è specificato, viene visualizzato il tipo corrente.  
- **FTP** supporta due tipi di trasferimento di file, ASCII e Binary.  
  Il tipo di trasferimento di file predefinito è ASCII.  Per il trasferimento di file di testo, è necessario usare il comando **ASCII** . In QUESTA modalità, vengono eseguite le conversioni di caratteri da e verso il set di caratteri standard di rete. I caratteri di fine riga, ad esempio, vengono convertiti come necessari, in base al sistema operativo nella destinazione.  
  Per il trasferimento dei file eseguibili, è necessario usare il comando **binario** . In modalità binaria, il file viene spostato in unità a un byte.  
  ## <a name="BKMK_Examples"></a>Esempi  
  Impostare il tipo di trasferimento di file ASCII.  
  ```  
  type ascii  
  ```  
  Impostare il tipo di file di trasferimento su Binary.  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
