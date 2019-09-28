---
title: virgolette FTP
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65660cf7311713295dae8a94c9174229f5ee44be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376084"
---
# <a name="ftp-quote"></a>FTP: virgolette

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia argomenti Verbatim al server FTP remoto. Viene restituito un singolo codice di risposta FTP.   
## <a name="syntax"></a>Sintassi  
```  
quote <Argument>[ ]  
```  
### <a name="parameters"></a>Parametri  

| Parametro  |                    Descrizione                    |
|------------|---------------------------------------------------|
| <Argument> | Specifica l'argomento da inviare al server FTP. |

## <a name="remarks"></a>Note  
Il comando **quote** è identico al comando **literal** .  
## <a name="BKMK_Examples"></a>Esempi  
Inviare un comando **Quit** al server FTP remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [FTP: literal_1](ftp-literal_1.md)  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
