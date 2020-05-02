---
title: backup auditpol
description: Argomento di riferimento per il comando backup auditpol, che consente di eseguire il backup delle impostazioni dei criteri di controllo del sistema, delle impostazioni dei criteri di controllo per utente per tutti gli utenti e di tutte le opzioni di controllo in un file di testo con valori delimitati da virgole (CSV).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddc6bbbc379453c86df27674b57f29f7c0960772
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719165"
---
# <a name="auditpol-backup"></a>backup auditpol

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il backup delle impostazioni dei criteri di controllo del sistema, delle impostazioni dei criteri di controllo per utente per tutti gli utenti e di tutte le opzioni di controllo in un file di testo con valori delimitati da virgole (CSV).

Per eseguire operazioni di *backup* nei criteri *per utente* e di *sistema* , è necessario disporre dell'autorizzazione di **controllo completo** o di **scrittura** per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di *backup* se si dispone del diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire le operazioni di *backup* complessive.

## <a name="syntax"></a>Sintassi

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
|-----------|------------- |
| /file | Specifica il nome del file in cui verrà eseguito il backup dei criteri di controllo. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per eseguire il backup delle impostazioni dei criteri di controllo per utente per tutti gli utenti, le impostazioni dei criteri di controllo del sistema e tutte le opzioni di controllo in un file di testo in formato CSV denominato AuditPolicy. csv, digitare:

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> Se non viene specificata alcuna unità, viene utilizzata la directory corrente.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [ripristino auditpol](auditpol-restore.md)

- [comandi auditpol](auditpol.md)
