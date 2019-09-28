---
title: utente FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63281a0ffdd646d3652eb3a442a8edd9acec9cce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375874"
---
# <a name="ftp-user"></a>FTP: utente

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Specifica un utente al computer remoto.   
## <a name="syntax"></a>Sintassi  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |                                                                      Descrizione                                                                      |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <UserName>  |                                          Specifica un nome utente con cui si accede al computer remoto.                                           |
| [<Password>] |               Specifica la password per *UserName*. Se una password non è specificata, ma è obbligatoria,  **ftp** richiesta la password.               |
| [<Account>]  | Specifica un account con cui si accede al computer remoto. Se un *Account* non è specificato, ma è obbligatorio,  **ftp** richiesto per l'account. |

## <a name="BKMK_Examples"></a>Esempi  
Specificare User1 con la password Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
