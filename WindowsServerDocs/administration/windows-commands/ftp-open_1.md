---
title: open_1 FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8bd3063a52908d65f336afcda6b6982d5bc9bf94
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843184"
---
# <a name="ftp-open_1"></a>FTP: open_1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="remarks"></a>Note  
Per specificare il **computer**, è possibile usare un indirizzo IP o un nome computer, nel qual caso è necessario che sia disponibile un file host o un server DNS.  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Connettersi al server FTP in **FTP.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Connettersi al server FTP in **FTP.Microsoft.com** che è in ascolto sulla porta TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
