---
title: CD FTP
description: Argomento dei comandi di Windows per il CD FTP
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a574855a-31b4-45c6-bce2-581c7231c99b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 891b144b20ebbef6c7e8058771d8249f4bace1cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376489"
---
# <a name="ftp-cd"></a>FTP: CD

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica la directory di lavoro nel computer remoto.   
## <a name="syntax"></a>Sintassi  
```  
cd <remotedirectory>  
```  
### <a name="parameters"></a>Parametri  

|     Parametro     |                                 Descrizione                                 |
|-------------------|-----------------------------------------------------------------------------|
| <remotedirectory> | Specifica la directory sul computer remoto a cui si desidera modificare. |

## <a name="BKMK_Examples"></a>Esempi  
modificare la directory nel computer remoto in **docs**.  
```  
cd Docs  
```  
modificare la directory nel computer remoto in **video di maggio**.  
```  
cd  May Videos  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
