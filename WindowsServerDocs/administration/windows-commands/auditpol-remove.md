---
title: auditpol rimuovere
description: Argomento di riferimento per il comando Remove di auditpol, che rimuove i criteri di controllo per utente per un account specificato o per tutti gli account.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9aedde39d44c7640e6aa2516465e1c8ec7d022c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719083"
---
# <a name="auditpol-remove"></a>auditpol rimuovere

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove i criteri di controllo per utente per un account specificato o per tutti gli account.

Per eseguire operazioni di *rimozione* sui criteri *per utente* , è necessario disporre delle autorizzazioni di **controllo completo** o di **scrittura** per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di *rimozione* se si dispone del diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire le operazioni di *rimozione* generali.

## <a name="syntax"></a>Sintassi

```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| /User | Specifica l'ID di sicurezza (SID) o il nome utente per l'utente per il quale devono essere eliminati i criteri di controllo per utente. |
| /allusers | Rimuove i criteri di controllo per utente per tutti gli utenti. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comandi auditpol](auditpol.md)
