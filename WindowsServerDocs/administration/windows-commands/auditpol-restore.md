---
title: ripristino auditpol
description: Windows Commands Topic for **auditpol restore** -Ripristina le impostazioni dei criteri di controllo del sistema, le impostazioni dei criteri di controllo per ogni utente per tutti gli utenti e tutte le opzioni di controllo da un file sintatticamente coerente con il formato di file con valori delimitati da virgole (CSV) usato dall'opzione/backup.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b91f3745354c695c4ab0c71b429718bff05d8098
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382407"
---
# <a name="auditpol-restore"></a>ripristino auditpol

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ripristina le impostazioni dei criteri di controllo del sistema, le impostazioni dei criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file sintatticamente coerente con il formato di file con valori delimitati da virgole (CSV) usato dall'opzione/backup.

## <a name="syntax"></a>Sintassi
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/file|Specifica il file da cui ripristinare i criteri di controllo. Il file deve essere stato creato usando l'opzione/backup o deve essere sintatticamente coerente con il formato di file CSV usato dall'opzione/backup.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Osservazioni
per le operazioni di ripristino per i criteri per utente e i criteri di sistema, è necessario disporre dell'autorizzazione di controllo completo o di scrittura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire l'operazione di ripristino possedendo il diritto utente **Gestione registro di controllo e protezione** (SeSecurityPrivilege). SeSecurityPrivilege è utile per il ripristino del descrittore di sicurezza in caso di errore accidentale o di attacchi dannosi.
## <a name="BKMK_examples"></a>Esempi
Per ripristinare le impostazioni dei criteri di controllo del sistema, le impostazioni dei criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file denominato AuditPolicy. csv creato con il comando/backup, digitare:
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[backup auditpol](auditpol-backup.md)
