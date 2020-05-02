---
title: tipo FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5531da30118914599ed0f85bfd10bd02ae89ffcf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725083"
---
# <a name="ftp-type"></a>FTP: tipo

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta o Visualizza il tipo di trasferimento di file.   
## <a name="syntax"></a>Sintassi  
```  
type [<typeName>]  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro   |            Descrizione            |
|--------------|-----------------------------------|
| [<typeName>] | Specifica il tipo di trasferimento di file. |

## <a name="remarks"></a>Osservazioni  
- Se *typeName* non è specificato, viene visualizzato il tipo corrente.  
- **FTP** supporta due tipi di trasferimento di file, ASCII e Binary.  
  Il tipo di trasferimento di file predefinito è ASCII.  Per il trasferimento di file di testo, è necessario usare il comando **ASCII** . In QUESTA modalità, vengono eseguite le conversioni di caratteri da e verso il set di caratteri standard di rete. I caratteri di fine riga, ad esempio, vengono convertiti come necessari, in base al sistema operativo nella destinazione.  
  Per il trasferimento dei file eseguibili, è necessario usare il comando **binario** . In modalità binaria, il file viene spostato in unità a un byte.  
  ## <a name="examples"></a>Esempi  
  Impostare il tipo di trasferimento di file ASCII.  
  ```  
  type ascii  
  ```  
  Impostare il tipo di file di trasferimento su Binary.  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
