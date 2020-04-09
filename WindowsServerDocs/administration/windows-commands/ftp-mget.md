---
title: mget FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72b45f84622fcd6abf7a743f606fb514def584cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843354"
---
# <a name="ftp-mget"></a>FTP: mget

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia i file remoti nel computer locale utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
mget <remoteFile>[ ]  
```  
#### <a name="parameters"></a>Parametri  

|  Parametro   |                        Descrizione                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Specifica i file remoti da copiare nel computer locale. |

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
copiare i file remoti **a. exe** e **b. exe** nel computer locale usando il tipo di trasferimento di file corrente.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
