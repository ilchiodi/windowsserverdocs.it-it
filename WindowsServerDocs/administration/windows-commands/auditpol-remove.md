---
title: auditpol rimuovere
description: Argomento dei comandi di Windows per **auditpol Remove**, che rimuove i criteri di controllo per utente per un account specificato o per tutti gli account.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1eda43d6708a31b2966022d2ae2c162bbfc888cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851174"
---
# <a name="auditpol-remove"></a>auditpol rimuovere

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove i criteri di controllo per utente per un account specificato o per tutti gli account.

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

## <a name="remarks"></a>Note

Per le operazioni di rimozione per i criteri per utente, è necessario disporre dell'autorizzazione di controllo completo o di scrittura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di rimozione possedendo il diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire l'operazione di rimozione.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
