---
title: eliminazione FTP
description: Argomento dei comandi di Windows per l'eliminazione FTP
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 067c45f3-e4e8-4450-b8b6-836994f6adfe vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b566da14751921e2f38acd922ececbbc972daa6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376450"
---
# <a name="ftp-delete"></a>FTP: Elimina

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i file nei computer remoti.   
## <a name="syntax"></a>Sintassi  
```  
delete <remoteFile>  
```  
### <a name="parameters"></a>Parametri  

|  Parametro   |          Descrizione          |
|--------------|-------------------------------|
| <remoteFile> | Specifica il file da eliminare. |

## <a name="BKMK_Examples"></a>Esempi  
eliminare il file test. txt nel computer remoto.  
```  
delete test.txt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
