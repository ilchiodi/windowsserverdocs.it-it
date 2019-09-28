---
title: Telnet aperto
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4528b728c89bbdfc99de94c7fefebb18c8e1ad97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383650"
---
# <a name="telnet-open"></a>Telnet: Apri

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Stabilisce la connessione a un server Telnet.    
## <a name="syntax"></a>Sintassi  
```  
o[pen] <hostname> [<Port>]  
```  
### <a name="parameters"></a>Parametri  

| Parametro  |                                        Descrizione                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Specifica il nome del computer o l'indirizzo IP.                         |
|  [<Port>]  | Specifica la porta TCP su cui è in ascolto il server Telnet. Il valore predefinito è la porta TCP 23. |

## <a name="BKMK_Examples"></a>Esempi  
Connettersi a un server Telnet in telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
