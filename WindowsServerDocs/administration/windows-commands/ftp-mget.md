---
title: mget FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 666025c92b6fb1a612cbe7b83833557a8a7d5017
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376304"
---
# <a name="ftp-mget"></a>FTP: mget

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia i file remoti nel computer locale utilizzando il tipo di trasferimento di file corrente.   
## <a name="syntax"></a>Sintassi  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |                        Descrizione                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Specifica i file remoti da copiare nel computer locale. |

## <a name="BKMK_Examples"></a>Esempi  
copiare i file remoti **a. exe** e **b. exe** nel computer locale usando il tipo di trasferimento di file corrente.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
