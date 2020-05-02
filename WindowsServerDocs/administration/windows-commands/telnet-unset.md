---
title: Telnet non impostato
description: Argomento di riferimento per Telnet non impostato, che disattiva le opzioni impostate in precedenza.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d88f5e479564cad8e2ec7e82dbb4f4cef35cd012
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721457"
---
# <a name="telnet-unset"></a>Telnet: Annulla impostazione

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disattiva le opzioni impostate in precedenza.   

## <a name="syntax"></a>Sintassi  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|bsasdel|Invia **BACKSPACE** come **BACKSPACE**.|  
|CRLF|Invia il tasto **invio** come CR. Nota anche come modalità di avanzamento riga.|  
|delasbs|Invia **Delete** come **Delete**.|  
|escape|Rimuove l'impostazione del carattere di escape.|  
|LOCALECHO|Disattiva localecho.|  
|registrazione|Disattiva la registrazione.|  
|ntlm|Disattiva l'autenticazione NTLM.|  
|?|Visualizza la guida per questo comando.|  
## <a name="examples"></a>Esempi  
Disattivare la registrazione.  
```  
u logging  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
