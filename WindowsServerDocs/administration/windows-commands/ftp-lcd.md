---
title: FTP lcd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: eac028c8aa675e680dedefcfe9f0b8da18ce7179
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826892"
---
# <a name="ftp-lcd"></a>FTP: lcd

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica la directory di lavoro nel computer locale. Per impostazione predefinita, la directory di lavoro è la directory in cui **ftp** è stato avviato.   
## <a name="syntax"></a>Sintassi  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<directory>]|Specifica la directory del computer locale a cui si desidera modificare. Se *directory* non viene specificato, la directory di lavoro corrente viene modificata nella directory predefinita.|  
## <a name="BKMK_Examples"></a>Esempi  
modificare la directory di lavoro nel computer locale a **C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
