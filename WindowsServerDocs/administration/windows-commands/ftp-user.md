---
title: utente FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29081bd8c5d537e1f060e4c848b720a60b4c8aea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842854"
---
# <a name="ftp-user"></a>FTP: utente

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Specificare User1 con la password Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
