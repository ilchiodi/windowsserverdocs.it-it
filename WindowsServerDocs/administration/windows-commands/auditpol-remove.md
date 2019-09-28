---
title: auditpol rimuovere
description: 'Argomento dei comandi di Windows per **auditpol Remove** : rimuove i criteri di controllo per utente per un account specificato o per tutti gli account.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2d652cb3bf7156bc1b1198616c9f6e57f588acd8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382483"
---
# <a name="auditpol-remove"></a>auditpol rimuovere

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

rimuove i criteri di controllo per utente per un account specificato o per tutti gli account.

## <a name="syntax"></a>Sintassi
```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/User|Specifica l'ID di sicurezza (SID) o il nome utente per l'utente per il quale devono essere eliminati i criteri di controllo per utente.|
|/allusers|rimuove i criteri di controllo per utente per tutti gli utenti.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Note
per le operazioni di rimozione per i criteri per utente, è necessario disporre dell'autorizzazione di controllo completo o di scrittura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di rimozione possedendo il diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire l'operazione di rimozione.
## <a name="BKMK_examples"></a>Esempi
Per rimuovere i criteri di controllo per utente per l'utente Zaffaronif in base al nome, digitare:
```
auditpol /remove /user:mikedan
```
Per rimuovere i criteri di controllo per utente per l'utente Zaffaronif in base al SID, digitare:
```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```
Per rimuovere i criteri di controllo per utente per tutti gli utenti, digitare:
```
auditpol /remove /allusers
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
