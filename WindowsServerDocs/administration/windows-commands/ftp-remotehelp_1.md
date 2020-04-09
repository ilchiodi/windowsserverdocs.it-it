---
title: remotehelp_1 FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c4a4ffec01fce5cde8b2aa9dd1fa0704f3a85ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843054"
---
# <a name="ftp-remotehelp_1"></a>FTP: remotehelp_1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza la guida per i comandi remoti.   
## <a name="syntax"></a>Sintassi  
```  
remotehelp [<Command>]  
```  
#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<Command>]|Specifica il nome del comando su cui si desidera visualizzare la guida. Se *Command* non è specificato, **FTP** Visualizza un elenco di tutti i comandi remoti.|  
## <a name="remarks"></a>Note  
È possibile eseguire comandi remoti usando **virgolette** o **valori letterali**.  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Visualizza un elenco di comandi remoti.  
```  
remotehelp  
```  
Visualizza la sintassi del comando **feat** remote.  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   [FTP: virgolette](ftp-quote.md)  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
