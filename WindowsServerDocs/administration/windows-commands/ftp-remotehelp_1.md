---
title: remotehelp_1 FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc4affb3f04eadaa4e0005e5edce0f564156f64a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725138"
---
# <a name="ftp-remotehelp_1"></a>FTP: remotehelp_1

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza la guida per i comandi remoti.   
## <a name="syntax"></a>Sintassi  
```  
remotehelp [<Command>]  
```  
#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<Command>]|Specifica il nome del comando su cui si desidera visualizzare la guida. Se *Command* non è specificato, **FTP** Visualizza un elenco di tutti i comandi remoti.|  
## <a name="remarks"></a>Osservazioni  
È possibile eseguire comandi remoti usando **virgolette** o **valori letterali**.  
## <a name="examples"></a>Esempi  
Visualizza un elenco di comandi remoti.  
```  
remotehelp  
```  
Visualizza la sintassi del comando **feat** remote.  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [FTP: virgolette](ftp-quote.md)  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
