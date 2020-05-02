---
title: invio Telnet
description: Argomento di riferimento per l'invio Telnet, che invia comandi Telnet al server Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 432401bbe2050a7954967a73b5ba8abeee5bb1d3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721494"
---
# <a name="telnet-send"></a>Telnet: invio

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia comandi Telnet al server Telnet.   

## <a name="syntax"></a>Sintassi  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
#### <a name="parameters"></a>Parametri  

| Parametro |                     Descrizione                      |
|-----------|------------------------------------------------------|
|    Ao     |       Invia l'output di interruzione del comando Telnet.        |
|    AYT    |       Invia il comando Telnet.       |
|    BRK    |            Invia il comando Telnet brk.            |
|    ESC    |      Invia il carattere di escape Telnet corrente.      |
|    IP     |     Invia il processo di interrupt del comando Telnet.     |
|   sincronizzazione   |           Invia la sincronizzazione del comando Telnet.           |
| <string>  | Invia qualsiasi stringa digitata al server Telnet. |
|     ?     |     Visualizza la Guida associata a questo comando.      |

## <a name="examples"></a>Esempi  
Inviare il server Telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
