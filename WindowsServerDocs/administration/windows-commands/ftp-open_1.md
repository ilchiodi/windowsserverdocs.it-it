---
title: open_1 FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c5da1c73362c0396300f712b2e45b906d1652604
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376194"
---
# <a name="ftp-open_1"></a>FTP: open_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Stabilisce la connessione al server FTP specificato.   
## <a name="syntax"></a>Sintassi  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Parametri  

| Parametro  |                                           Descrizione                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Specifica il computer remoto a cui si sta tentando di connettersi.                 |
|  [<Port>]  | Specifica un numero di porta TCP da utilizzare per la connessione a un server FTP. Per impostazione predefinita, viene usata la porta TCP 21. |

## <a name="remarks"></a>Note  
Per specificare il **computer**, è possibile usare un indirizzo IP o un nome computer, nel qual caso è necessario che sia disponibile un file host o un server DNS.  
## <a name="BKMK_Examples"></a>Esempi  
Connettersi al server FTP in **FTP.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Connettersi al server FTP in **FTP.Microsoft.com** che è in ascolto sulla porta TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
