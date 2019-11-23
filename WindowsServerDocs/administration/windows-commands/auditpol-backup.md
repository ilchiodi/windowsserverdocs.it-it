---
title: backup auditpol
description: 'Argomento dei comandi di Windows per il **backup di auditpol** : esegue il backup delle impostazioni dei criteri di controllo del sistema, delle impostazioni dei criteri di controllo per utente per tutti gli utenti e di tutte le opzioni di controllo in un file di testo con valori delimitati da virgole (CSV).'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96b98a05740d3ce1bfe14eda4c5d97ba6c09ff32
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382447"
---
# <a name="auditpol-backup"></a>backup auditpol

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il backup delle impostazioni dei criteri di controllo del sistema, delle impostazioni dei criteri di controllo per utente per tutti gli utenti e di tutte le opzioni di controllo in un file di testo con valori delimitati da virgole (CSV).

## <a name="syntax"></a>Sintassi
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Parametri

| Parametro |                                 Descrizione                                 |
|-----------|-----------------------------------------------------------------------------|
|   /file   | Specifica il nome del file in cui verrà eseguito il backup dei criteri di controllo. |
|    /?     |                    Visualizza la guida al prompt dei comandi.                     |

## <a name="remarks"></a>Osservazioni
per le operazioni di backup per i criteri per utente e i criteri di sistema, è necessario disporre dell'autorizzazione di controllo completo o di scrittura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni di backup possedendo il diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire l'operazione di elenco.
## <a name="BKMK_examples"></a>Esempi
Per eseguire il backup delle impostazioni dei criteri di controllo per utente per tutti gli utenti, le impostazioni dei criteri di controllo del sistema e tutte le opzioni di controllo in un file di testo in formato CSV denominato AuditPolicy. csv, digitare:
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> Se non viene specificata alcuna unità, viene utilizzata la directory corrente.
> #### <a name="additional-references"></a>riferimenti aggiuntivi
> [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
> [Restore auditpol](auditpol-restore.md)
