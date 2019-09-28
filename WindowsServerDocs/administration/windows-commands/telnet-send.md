---
title: invio Telnet
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 958e0e507e5a0ae836da98de8d677a116dbb38bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383629"
---
# <a name="telnet-send"></a>Telnet: invio

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia comandi Telnet al server Telnet.   
## <a name="syntax"></a>Sintassi  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Parametri  

| Parametro |                     Descrizione                      |
|-----------|------------------------------------------------------|
|    Ao     |       Invia l'output di interruzione del comando Telnet.        |
|    AYT    |       Invia il comando Telnet.       |
|    BRK    |            Invia il comando Telnet brk.            |
|    ESC    |      Invia il carattere di escape Telnet corrente.      |
|    IP     |     Invia il processo di interrupt del comando Telnet.     |
|   Synch   |           Invia la sincronizzazione del comando Telnet.           |
| <string>  | Invia qualsiasi stringa digitata al server Telnet. |
|     ?     |     Visualizza la Guida associata a questo comando.      |

## <a name="BKMK_Examples"></a>Esempi  
Inviare il server Telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
