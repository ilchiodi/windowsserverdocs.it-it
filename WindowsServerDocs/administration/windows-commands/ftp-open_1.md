---
title: ftp open_1
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45de8b3c210fe0925ac3cc43c41d3e092d5dfe16
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438501"
---
# <a name="ftp-open1"></a>ftp: open_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si connette al server ftp specificata.   
## <a name="syntax"></a>Sintassi  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Parametri  

| Parametro  |                                           Descrizione                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Specifica il computer remoto a cui si sta provando a connettersi.                 |
|  [<Port>]  | Specifica un numero di porta TCP da utilizzare per connettersi a un server ftp. Per impostazione predefinita, viene utilizzata la porta TCP 21. |

## <a name="remarks"></a>Note  
È possibile usare un nome di computer o indirizzo IP (nel qual caso un file di host o server DNS deve essere disponibile) per specificare **computer**.  
## <a name="BKMK_Examples"></a>Esempi  
Connettersi al server ftp in **ftp.microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Connettersi al server ftp in **ftp.microsoft.com** che è in ascolto sulla porta TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
