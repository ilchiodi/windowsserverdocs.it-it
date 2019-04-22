---
title: Rimuovi auditpol
description: Argomento i comandi di Windows per **auditpol rimuovere** -rimuove il criterio di controllo per utente per un account specificato o tutti gli account.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 566827e93d9f8c9dc0d00f4f704513369fbb44ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818302"
---
# <a name="auditpol-remove"></a>Rimuovi auditpol

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove i criteri di controllo per utente per un account specificato o tutti gli account.

## <a name="syntax"></a>Sintassi
```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/User|Specifica l'ID di sicurezza (SID) o nome utente per l'utente per il quale l'utente per ogni criterio di controllo deve essere eliminato.|
|/allusers|Rimuove i criteri di controllo per utente per tutti gli utenti.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Note
per le operazioni di rimozione per il criterio per ogni utente, è necessario disporre dell'autorizzazione di scrittura o controllo completo su tale oggetto impostato nel descrittore di sicurezza. È anche possibile eseguire operazioni di eliminazione che presenta il **gestione file registro di controllo e protezione** diritto utente (SeSecurityPrivilege). Tuttavia, questo diritto consente accesso aggiuntivo che non è necessario eseguire l'operazione di rimozione.
## <a name="BKMK_examples"></a>Esempi
Per rimuovere il criterio di controllo per utente per utente bianchi in base al nome, digitare:
```
auditpol /remove /user:mikedan
```
Per rimuovere il criterio di controllo per utente per utente bianchi dal SID, digitare:
```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```
Per rimuovere il criterio di controllo per utente per tutti gli utenti, digitare:
```
auditpol /remove /allusers
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
