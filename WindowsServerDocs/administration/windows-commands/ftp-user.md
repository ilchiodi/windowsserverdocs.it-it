---
title: utente FTP
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef3b943491a90078dab453aaf3a037bd4ccf1825
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887492"
---
# <a name="ftp-user"></a>FTP: utente

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Specifica un utente al computer remoto.   
## <a name="syntax"></a>Sintassi  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|<UserName>|Specifica un nome utente con cui si accede al computer remoto.|  
|[<Password>]|Specifica la password per *UserName*. Se una password non è specificata, ma è obbligatoria,  **ftp** richiesta la password.|  
|[<Account>]|Specifica un account con cui si accede al computer remoto. Se un *Account* non è specificato, ma è obbligatorio,  **ftp** richiesto per l'account.|  
## <a name="BKMK_Examples"></a>Esempi  
Specificare User1 con la password Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
