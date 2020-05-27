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
ms.openlocfilehash: c121d6501f87ae1218381871bcbf536d9407ba31
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821041"
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
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
