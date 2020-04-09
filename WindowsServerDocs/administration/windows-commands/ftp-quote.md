---
title: virgolette FTP
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bf13704150d602fbfa4e3b1a3fb1774d3bf7363
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843034"
---
# <a name="ftp-quote"></a>FTP: virgolette

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia argomenti Verbatim al server FTP remoto. Viene restituito un singolo codice di risposta FTP.   
## <a name="syntax"></a>Sintassi  
```  
quote <Argument>[ ]  
```  
#### <a name="parameters"></a>Parametri  

| Parametro  |                    Descrizione                    |
|------------|---------------------------------------------------|
| <Argument> | Specifica l'argomento da inviare al server FTP. |

## <a name="remarks"></a>Note  
Il comando **quote** è identico al comando **literal** .  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Inviare un comando **Quit** al server FTP remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   [FTP: literal_1](ftp-literal_1.md)  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
