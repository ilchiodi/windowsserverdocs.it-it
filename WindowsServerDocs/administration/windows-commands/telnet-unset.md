---
title: Telnet non impostato
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e6f0eb98c4168d2f664780dad42ca1aea5463d24
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383476"
---
# <a name="telnet-unset"></a>Telnet: Annulla impostazione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disattiva le opzioni impostate in precedenza.   
## <a name="syntax"></a>Sintassi  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|bsasdel|Invia **BACKSPACE** come **BACKSPACE**.|  
|CRLF|Invia il tasto **invio** come CR. Nota anche come modalità di avanzamento riga.|  
|delasbs|Invia **Delete** come **Delete**.|  
|Fuga|Rimuove l'impostazione del carattere di escape.|  
|LOCALECHO|Disattiva localecho.|  
|logging|Disattiva la registrazione.|  
|NTLM|Disattiva l'autenticazione NTLM.|  
|?|Visualizza la guida per questo comando.|  
## <a name="BKMK_Examples"></a>Esempi  
Disattivare la registrazione.  
```  
u logging  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
