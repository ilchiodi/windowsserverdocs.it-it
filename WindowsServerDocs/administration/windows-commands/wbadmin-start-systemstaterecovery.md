---
title: comando Wbadmin start systemstaterecovery
description: Argomento di riferimento per Wbadmin start systemstaterecovery, che esegue un ripristino dello stato del sistema in un percorso e da un backup specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edbd6acefe2ef921b9325de4808753d5929efd1e
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821381"
---
# <a name="wbadmin-start-systemstaterecovery"></a>comando Wbadmin start systemstaterecovery



Esegue un ripristino dello stato di sistema in una posizione e da un backup specificato.

> [!NOTE]
> Windows Server Backup non eseguire il backup o ripristinare l'hive utente (HKEY_CURRENT_USER) come parte del backup dello stato del sistema o il ripristino dello stato del sistema.

Per eseguire un ripristino dello stato di sistema con il sottocomando, è necessario essere un membro del **Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)



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
Sintassi per Windows Server 2008 R2 o versione successiva:
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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-version|Specifica l'identificatore di versione per il backup da ripristinare nella MM/GG/AAAA-formato hh: mm. Se non si conosce l'identificatore di versione, digitare **wbadmin ottenere versioni**.|
|-showsummary|Segnala il riepilogo dell'ultimo ripristino dello stato di sistema (dopo il riavvio necessario per completare l'operazione). Questo parametro non può essere accompagnato da altri parametri.|
|-backupTarget|Specifica il percorso di archiviazione che contiene il backup o il backup da ripristinare. Questo parametro è utile quando il percorso di archiviazione è diverso da in cui sono in genere archiviati i backup del computer.|
|-machine|Specifica il nome del computer in cui si desidera ripristinare. Questo parametro è utile quando più computer sottoposte a backup nello stesso percorso. Deve essere utilizzato quando il **- backupTarget** parametro specificato.|
|-recoveryTarget|Specifica la directory da ripristinare. Questo parametro è utile se il backup viene ripristinato in un percorso alternativo.|
|-authsysvol|Se usato, esegue un ripristino autorevole di SYSVOL (la directory condivisa Volume di sistema).|
|-autoReboot|Consente di riavviare il sistema alla fine dell'operazione di ripristino dello stato del sistema. Questo parametro è valido solo per il ripristino nel percorso originale. Non è consigliabile che utilizzare questo parametro se è necessario eseguire passaggi dopo l'operazione di ripristino.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="examples"></a>Esempi

- Per eseguire un ripristino dello stato di sistema di backup da 31/03/2013 alle 9:00, digitare:
  ```
  wbadmin start systemstaterecovery -version:03/31/2013-09:00
  ```
- Per eseguire un ripristino del backup dello stato del sistema da 30/04/2013 alle 9:00. archiviati nella risorsa condivisa \\ \\ servername\share per Server01, digitare:
  ```
  wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
  ```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Inizio WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet