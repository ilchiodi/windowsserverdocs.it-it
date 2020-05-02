---
title: Telnet aperto
description: Argomento di riferimento per Telnet aperto, che si connette a un server Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c4529670ef934cfa19c9864ac59f5317eb2887a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721508"
---
# <a name="telnet-open"></a>Telnet: Apri

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Stabilisce la connessione a un server Telnet.    

## <a name="syntax"></a>Sintassi  
```  
o[pen] <hostname> [<Port>]  
```  
#### <a name="parameters"></a>Parametri  

| Parametro  |                                        Descrizione                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Specifica il nome del computer o l'indirizzo IP.                         |
|  [<Port>]  | Specifica la porta TCP su cui è in ascolto il server Telnet. Il valore predefinito è la porta TCP 23. |

## <a name="examples"></a>Esempi  
Connettersi a un server Telnet in telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
