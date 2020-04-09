---
title: LCD FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d7e2e6fc9f6af7655381bfb802dc190e79365bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843404"
---
# <a name="ftp-lcd"></a>FTP: LCD

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica la directory di lavoro nel computer locale. Per impostazione predefinita, la directory di lavoro è la directory in cui **ftp** è stato avviato.   
## <a name="syntax"></a>Sintassi  
```  
lcd [<directory>]  
```  
#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<directory>]|Specifica la directory del computer locale a cui si desidera modificare. Se la *directory* non è specificata, la directory di lavoro corrente viene modificata nella directory predefinita.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
modificare la directory di lavoro nel computer locale in **C:\Dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
