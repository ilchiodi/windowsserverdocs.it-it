---
title: backup auditpol
description: Windows Commands argomento for **auditpol backup**, che consente di eseguire il backup delle impostazioni dei criteri di controllo del sistema, delle impostazioni dei criteri di controllo per utente per tutti gli utenti e di tutte le opzioni di controllo in un file di testo con valori delimitati da virgole (CSV).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8895f312606a6a6c45a77c659a1cd98d115babe3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851214"
---
# <a name="auditpol-backup"></a>backup auditpol

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il backup delle impostazioni dei criteri di controllo del sistema, delle impostazioni dei criteri di controllo per utente per tutti gli utenti e di tutte le opzioni di controllo in un file di testo con valori delimitati da virgole (CSV).

## <a name="syntax"></a>Sintassi

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
|-----------|------------- |
| /file | Specifica il nome del file in cui verrà eseguito il backup dei criteri di controllo. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Note

Per le operazioni di backup per i criteri per utente e i criteri di sistema, è necessario disporre dell'autorizzazione di controllo completo o di scrittura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di backup possedendo il diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire l'operazione di elenco.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per eseguire il backup delle impostazioni dei criteri di controllo per utente per tutti gli utenti, le impostazioni dei criteri di controllo del sistema e tutte le opzioni di controllo in un file di testo in formato CSV denominato AuditPolicy. csv, digitare:

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> Se non viene specificata alcuna unità, viene utilizzata la directory corrente.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
- [ripristino auditpol](auditpol-restore.md)
