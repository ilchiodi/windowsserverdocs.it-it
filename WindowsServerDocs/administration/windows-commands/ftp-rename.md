---
title: ridenominazione FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 977baa042a6b0d9c23db7cb398bee997c2049227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376019"
---
# <a name="ftp-rename"></a>FTP: Rinomina

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rinomina i file remoti.   
## <a name="syntax"></a>Sintassi  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Parametri  

|   Parametro   |                 Descrizione                 |
|---------------|---------------------------------------------|
|  <FileName>   | Specifica il file che si desidera rinominare. |
| <NewFileName> |        Specifica il nuovo nome del file.         |

## <a name="BKMK_Examples"></a>Esempi  
Rinominare il file remoto **example. txt** in **Example1. txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
