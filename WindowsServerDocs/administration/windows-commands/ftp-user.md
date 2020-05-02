---
title: utente FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb4f0f47f23bec312c57266479c261c96535e8cc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725074"
---
# <a name="ftp-user"></a>FTP: utente

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Specifica un utente al computer remoto.   
## <a name="syntax"></a>Sintassi  
```  
user <UserName> [<Password>] [<Account>]  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro   |                                                                      Descrizione                                                                      |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <UserName>  |                                          Specifica un nome utente con cui si accede al computer remoto.                                           |
| [<Password>] |               Specifica la password per *UserName*. Se una password non è specificata, ma è obbligatoria,  **ftp** richiesta la password.               |
| [<Account>]  | Specifica un account con cui si accede al computer remoto. Se un *Account* non è specificato, ma è obbligatorio,  **ftp** richiesto per l'account. |

## <a name="examples"></a>Esempi  
Specificare User1 con la password Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
