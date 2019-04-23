---
title: Telnet non impostato
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37c4d84d1664fdc13ea7ffec60bf981b264dba00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853892"
---
# <a name="telnet-unset"></a>telnet: unset

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disattiva l'impostazione delle opzioni precedentemente.   
## <a name="syntax"></a>Sintassi  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|bsasdel|Invia **Backspace** come una **Backspace**.|  
|crlf|Invia il **invio** chiave come un CR. Noto anche come modalità di avanzamento riga.|  
|delasbs|Invia **eliminare** come **eliminare**.|  
|escape|Rimuove l'impostazione di caratteri di escape.|  
|eco locale|Disattiva l'eco locale.|  
|logging|Disattiva la registrazione.|  
|ntlm|Disattiva l'autenticazione NTLM.|  
|?|Visualizza la Guida per questo comando.|  
## <a name="BKMK_Examples"></a>Esempi  
Disabilitare la registrazione.  
```  
u logging  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
