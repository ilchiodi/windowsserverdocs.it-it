---
title: mput_1 FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3fb654d5a2f44b9b63238abdbaee8d6a0294861
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725224"
---
# <a name="ftp-mput_1"></a>FTP: mput_1

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia i file locali nel computer remoto utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
mput <LocalFile>[ ]  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro  |                       Descrizione                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Specifica il file locale da copiare nel computer remoto. |

## <a name="examples"></a>Esempi  
copiare **Program1. exe** e **Program2. exe** nel computer remoto usando il tipo di trasferimento di file corrente.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
