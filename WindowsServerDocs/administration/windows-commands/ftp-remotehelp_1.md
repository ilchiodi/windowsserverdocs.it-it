---
title: remotehelp_1 FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bac6fbe4a55c3fed4caab4e30ba848ec9ea68e21
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376035"
---
# <a name="ftp-remotehelp_1"></a>FTP: remotehelp_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza la guida per i comandi remoti.   
## <a name="syntax"></a>Sintassi  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<Command>]|Specifica il nome del comando su cui si desidera visualizzare la guida. Se *Command* non è specificato, **FTP** Visualizza un elenco di tutti i comandi remoti.|  
## <a name="remarks"></a>Note  
È possibile eseguire comandi remoti usando **virgolette** o **valori letterali**.  
## <a name="BKMK_Examples"></a>Esempi  
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
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
