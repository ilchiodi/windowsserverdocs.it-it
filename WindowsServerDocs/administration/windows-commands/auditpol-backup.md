---
title: backup auditpol
description: Argomento i comandi di Windows per **auditpol backup** -esegue il backup di sistema di controllo delle impostazioni di criteri, impostazioni di criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo per un file di testo con valori delimitati da virgole (CSV).
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7de5e6dc6d205b7e6749d38ac822e31a78788c6e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435216"
---
# <a name="auditpol-backup"></a>backup auditpol

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il backup di sistema di controllo delle impostazioni di criteri, impostazioni di criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo per un file di testo con valori delimitati da virgole (CSV).

## <a name="syntax"></a>Sintassi
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Parametri

| Parametro |                                 Descrizione                                 |
|-----------|-----------------------------------------------------------------------------|
|   /file   | Specifica il nome del file a cui il criterio di controllo verrà sottoposti a backup. |
|    /?     |                    Visualizza la guida al prompt dei comandi.                     |

## <a name="remarks"></a>Note
per operazioni di backup per il criterio per ogni utente e criteri di sistema, è necessario avere scrivere o dell'autorizzazione controllo completo su tale oggetto impostato nel descrittore di sicurezza. È anche possibile eseguire operazioni di backup da cui appartiene il **gestione file registro di controllo e protezione** diritto utente (SeSecurityPrivilege). Tuttavia, questo diritto consente accesso aggiuntivo che non è necessario eseguire l'operazione di elenco.
## <a name="BKMK_examples"></a>Esempi
Per eseguire il backup di controllo per utente impostazioni dei criteri per tutti gli utenti, sistema di controllo impostazioni di criteri e le opzioni di controllo di tutto in un file di testo in formato CSV denominato auditpolicy.csv, tipo:
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> Se viene specificata alcuna unità, viene usata la directory corrente.
> #### <a name="additional-references"></a>Riferimenti aggiuntivi
> [Chiave sintassi della riga di comando](command-line-syntax-key.md)
> [auditpol ripristino](auditpol-restore.md)
