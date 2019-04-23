---
title: Wbadmin get elementi
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eeb7c29ff552f968b4785612f626a86baf154ad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842882"
---
# <a name="wbadmin-get-items"></a>Wbadmin get elementi



Elenca gli elementi inclusi in un backup specifico.

Per utilizzare questo sottocomando, è necessario essere un membro del **Backup Operators** gruppo o il **gli amministratori** gruppo, oppure è necessario siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt** e quindi fare clic su **Esegui come amministratore**.)

Per esempi di come utilizzare questo sottocomando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-versione|Specifica la versione del backup in MM/GG/AAAA-formato hh: mm. Se non si conosce le informazioni sulla versione, digitare **wbadmin ottenere versioni**.|
|-backupTarget|Specifica il percorso di archiviazione che contiene i backup per il quale si desidera i dettagli. Utilizzare per l'elenco dei backup archiviati in tale percorso di destinazione. Percorsi di destinazione di backup possono essere un'unità disco collegata al computer locale o una cartella condivisa remota. Se **wbadmin ottenere elementi**viene eseguito nello stesso computer in cui è stato creato il backup, questo parametro non è necessaria. Tuttavia, questo parametro è obbligatorio per ottenere informazioni su un backup creato da un altro computer.|
|-machine|Specifica il nome del computer in cui si desidera che i dettagli sul backup per. È utile quando più computer sottoposte a backup nello stesso percorso. Deve essere utilizzato quando **- backupTarget** specificato.|

## <a name="BKMK_examples"></a>Esempi

Per elencare gli elementi dalla copia di backup è stato eseguito il 31 marzo 2013 alle 9:00, digitare:
```
wbadmin get items -version:03/31/2013-09:00
```
Per elencare gli elementi dal backup di server01 che è stato eseguito il 30 aprile 2013 alle 9:00 e archiviato nel \\ \\servername\share, tipo:
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx) cmdlet