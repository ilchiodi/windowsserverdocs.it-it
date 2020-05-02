---
title: virgolette FTP
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1101dd6a5fa163df8d43d182e9d0dfe66e340b60
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725146"
---
# <a name="ftp-quote"></a>FTP: virgolette

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia argomenti Verbatim al server FTP remoto. Viene restituito un singolo codice di risposta FTP.   
## <a name="syntax"></a>Sintassi  
```  
quote <Argument>[ ]  
```  
#### <a name="parameters"></a>Parametri  

| Parametro  |                    Descrizione                    |
|------------|---------------------------------------------------|
| <Argument> | Specifica l'argomento da inviare al server FTP. |

## <a name="remarks"></a>Osservazioni  
Il comando **quote** è identico al comando **literal** .  
## <a name="examples"></a>Esempi  
Inviare un comando **Quit** al server FTP remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [FTP: literal_1](ftp-literal_1.md)  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
