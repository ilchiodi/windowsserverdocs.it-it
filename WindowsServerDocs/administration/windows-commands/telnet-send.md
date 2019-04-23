---
title: trasmissione Telnet
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32345b22395107f4a2c3d88894126d4e5e0875a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842362"
---
# <a name="telnet-send"></a>telnet: send

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia comandi telnet per server telnet.   
## <a name="syntax"></a>Sintassi  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|ao|Invia il comando telnet interrompere uscita.|  
|ayt|Invia il comando telnet è sono presenti.|  
|brk|Invia brk il comando telnet.|  
|esc|Invia il carattere di escape telnet corrente.|  
|ip|Invia il comando telnet Interrupt processo.|  
|sincronizzazione delle directory|Invia la sincronizzazione di comandi telnet.|  
|<string>|Invia qualsiasi stringa digitata per il server telnet.|  
|?|Visualizza la Guida associata con questo comando.|  
## <a name="BKMK_Examples"></a>Esempi  
Trasmissione sono è presente nel server telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
