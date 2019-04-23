---
title: auditpol restore
description: Argomento i comandi di Windows per **auditpol ripristino** -Ripristina le impostazioni di Criteri controllo del sistema, impostazioni di criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file che è sintatticamente coerenza con le virgole formato di file CSV (valori) utilizzato da /backup l'opzione.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f1961387083a8a61b27f3e44a2380a6060a02f98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868982"
---
# <a name="auditpol-restore"></a>auditpol restore

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ripristina impostazioni dei criteri di controllo del sistema, impostazioni di criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file che è sintatticamente coerenza con il formato di file con valori delimitati da virgole (CSV) usato dal /backup opzione.

## <a name="syntax"></a>Sintassi
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/file|Specifica il file da cui verranno ripristinati i criteri di controllo. Il file deve essere stato creato utilizzando il /backup opzione o deve essere sintatticamente coerente con il formato di file CSV usato dal /backup opzione.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Note
per le operazioni di ripristino per il criterio per ogni utente e criteri di sistema, è necessario avere scrivere o dell'autorizzazione controllo completo su tale oggetto impostato nel descrittore di sicurezza. È anche possibile eseguire l'operazione di ripristino da cui appartiene il **gestione file registro di controllo e protezione** diritto utente (SeSecurityPrivilege). SeSecurityPrivilege è utile quando si ripristina il descrittore di sicurezza in caso di errori accidentali o dannose.
## <a name="BKMK_examples"></a>Esempi
Per ripristinare le impostazioni di Criteri controllo del sistema, impostazioni di criteri di controllo per utente per tutti gli utenti e tutte le opzioni di controllo da un file denominato auditpolicy.csv che è stato creato utilizzando il /backup comando, digitare:
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[auditpol backup](auditpol-backup.md)
