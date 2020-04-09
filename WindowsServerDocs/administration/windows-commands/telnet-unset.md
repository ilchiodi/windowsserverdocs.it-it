---
title: Telnet non impostato
description: Argomento comandi di Windows per Telnet non impostato, che disattiva le opzioni impostate in precedenza.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34d52eedb2a5547ad0e3f2912dbe2a250eaf8fc5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833144"
---
# <a name="telnet-unset"></a>Telnet: Annulla impostazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Disattivare la registrazione.  
```  
u logging  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
