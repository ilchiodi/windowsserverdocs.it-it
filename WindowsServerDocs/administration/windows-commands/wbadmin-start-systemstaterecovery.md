---
title: Comando Wbadmin start systemstaterecovery
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4282da2011c39daec0315a7f3836d5517f29debb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440201"
---
# <a name="wbadmin-start-systemstaterecovery"></a>Comando Wbadmin start systemstaterecovery



Esegue un ripristino dello stato di sistema in una posizione e da un backup specificato.

> [!NOTE]
> Windows Server Backup non eseguire il backup o ripristinare l'hive utente (HKEY_CURRENT_USER) come parte del backup dello stato del sistema o il ripristino dello stato del sistema.

Per eseguire un ripristino dello stato di sistema con il sottocomando, è necessario essere un membro del **Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

Sintassi per Windows Server 2008:
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-quiet]
```
Sintassi per Windows Server 2008 R2 o versioni successive:
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-autoReboot]
[-quiet]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-versione|Specifica l'identificatore di versione per il backup da ripristinare nella MM/GG/AAAA-formato hh: mm. Se non si conosce l'identificatore di versione, digitare **wbadmin ottenere versioni**.|
|-showsummary|Segnala il riepilogo dell'ultimo ripristino dello stato di sistema (dopo il riavvio necessario per completare l'operazione). Questo parametro non può essere accompagnato da altri parametri.|
|-backupTarget|Specifica il percorso di archiviazione che contiene il backup o il backup da ripristinare. Questo parametro è utile quando il percorso di archiviazione è diverso da in cui sono in genere archiviati i backup del computer.|
|-machine|Specifica il nome del computer in cui si desidera ripristinare. Questo parametro è utile quando più computer sottoposte a backup nello stesso percorso. Deve essere utilizzato quando il **- backupTarget** parametro specificato.|
|-recoveryTarget|Specifica la directory da ripristinare. Questo parametro è utile se il backup viene ripristinato in un percorso alternativo.|
|-authsysvol|Se usato, esegue un ripristino autorevole di SYSVOL (la directory condivisa Volume di sistema).|
|-autoReboot|Consente di riavviare il sistema alla fine dell'operazione di ripristino dello stato del sistema. Questo parametro è valido solo per il ripristino nel percorso originale. Non è consigliabile che utilizzare questo parametro se è necessario eseguire passaggi dopo l'operazione di ripristino.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="BKMK_examples"></a>Esempi

- Per eseguire un ripristino dello stato di sistema di backup da 31/03/2013 alle 9:00, digitare:  
  ```
  wbadmin start systemstaterecovery -version:03/31/2013-09:00
  ```  
- Per eseguire un ripristino del backup dello stato del sistema da 30/04/2013 alle 9:00. che viene archiviato nella risorsa condivisa \\ \\servername\share per server01, tipo:  
  ```
  wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
  ```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Inizio WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet