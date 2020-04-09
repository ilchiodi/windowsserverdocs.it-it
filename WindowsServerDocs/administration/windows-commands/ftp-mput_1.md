---
title: mput_1 FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 489e18da937e12a1fc69e0ee84d9dda00309ccd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843234"
---
# <a name="ftp-mput_1"></a>FTP: mput_1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia i file locali nel computer remoto utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
mput <LocalFile>[ ]  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro  |                       Descrizione                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Specifica il file locale da copiare nel computer remoto. |

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
copiare **Program1. exe** e **Program2. exe** nel computer remoto usando il tipo di trasferimento di file corrente.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
