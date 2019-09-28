---
title: mput_1 FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6308f7b47d58bc25d964944f96fbc83a26350962
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376212"
---
# <a name="ftp-mput_1"></a>FTP: mput_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia i file locali nel computer remoto utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Parametri  

|  Parametro  |                       Descrizione                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Specifica il file locale da copiare nel computer remoto. |

## <a name="BKMK_Examples"></a>Esempi  
copiare **Program1. exe** e **Program2. exe** nel computer remoto usando il tipo di trasferimento di file corrente.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
