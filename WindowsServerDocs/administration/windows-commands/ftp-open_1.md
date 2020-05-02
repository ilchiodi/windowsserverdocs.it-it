---
title: open_1 FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15a27d2f7512da352a0f4ddf02fa2511ffce7c1d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725190"
---
# <a name="ftp-open_1"></a>FTP: open_1

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Stabilisce la connessione al server FTP specificato.   
## <a name="syntax"></a>Sintassi  
```  
open <computer> [<Port>]  
```  
#### <a name="parameters"></a>Parametri  

| Parametro  |                                           Descrizione                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Specifica il computer remoto a cui si sta tentando di connettersi.                 |
|  [<Port>]  | Specifica un numero di porta TCP da utilizzare per la connessione a un server FTP. Per impostazione predefinita, viene usata la porta TCP 21. |

## <a name="remarks"></a>Osservazioni  
Per specificare il **computer**, è possibile usare un indirizzo IP o un nome computer, nel qual caso è necessario che sia disponibile un file host o un server DNS.  
## <a name="examples"></a>Esempi  
Connettersi al server FTP in **FTP.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Connettersi al server FTP in **FTP.Microsoft.com** che è in ascolto sulla porta TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
