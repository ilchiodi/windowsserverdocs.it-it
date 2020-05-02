---
title: LCD FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 646cbfe3feadb63388694218758dae165ffb49c4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725268"
---
# <a name="ftp-lcd"></a>FTP: LCD

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica la directory di lavoro nel computer locale. Per impostazione predefinita, la directory di lavoro è la directory in cui **ftp** è stato avviato.   
## <a name="syntax"></a>Sintassi  
```  
lcd [<directory>]  
```  
#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<directory>]|Specifica la directory del computer locale a cui si desidera modificare. Se la *directory* non è specificata, la directory di lavoro corrente viene modificata nella directory predefinita.|  
## <a name="examples"></a>Esempi  
modificare la directory di lavoro nel computer locale in **C:\Dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
