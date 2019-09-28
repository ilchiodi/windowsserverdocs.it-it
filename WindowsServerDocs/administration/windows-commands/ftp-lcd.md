---
title: LCD FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47058cc62e4e87d1fcd951ec3b4a7885eeef2ae2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376312"
---
# <a name="ftp-lcd"></a>FTP: LCD

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica la directory di lavoro nel computer locale. Per impostazione predefinita, la directory di lavoro è la directory in cui **ftp** è stato avviato.   
## <a name="syntax"></a>Sintassi  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<directory>]|Specifica la directory del computer locale a cui si desidera modificare. Se la *directory* non è specificata, la directory di lavoro corrente viene modificata nella directory predefinita.|  
## <a name="BKMK_Examples"></a>Esempi  
modificare la directory di lavoro nel computer locale in **C:\Dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
