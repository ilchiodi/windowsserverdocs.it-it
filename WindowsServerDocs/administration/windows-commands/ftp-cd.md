---
title: CD FTP
description: Argomento dei comandi di Windows per il CD FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a574855a-31b4-45c6-bce2-581c7231c99b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9739a3dbfb2dcb350cf34e90ceaeb29360e53598
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843714"
---
# <a name="ftp-cd"></a>FTP: CD

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica la directory di lavoro nel computer remoto.   
## <a name="syntax"></a>Sintassi  
```  
cd <remotedirectory>  
```  
#### <a name="parameters"></a>Parametri  

|     Parametro     |                                 Descrizione                                 |
|-------------------|-----------------------------------------------------------------------------|
| <remotedirectory> | Specifica la directory sul computer remoto a cui si desidera modificare. |

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
modificare la directory nel computer remoto in **docs**.  
```  
cd Docs  
```  
modificare la directory nel computer remoto in **video di maggio**.  
```  
cd  May Videos  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
