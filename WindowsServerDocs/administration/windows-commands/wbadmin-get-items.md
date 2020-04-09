---
title: wbadmin Ottieni elementi
description: Argomento dei comandi di Windows per Wbadmin get items, che elenca gli elementi inclusi in un backup specifico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 781057bb450ab4b692c793f67d807e008efcf5d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829754"
---
# <a name="wbadmin-get-items"></a>wbadmin Ottieni elementi



Elenca gli elementi inclusi in un backup specifico.

Per utilizzare questo sottocomando, è necessario essere un membro del gruppo **Backup Operators** o **Administrators** oppure avere ricevuto in delega le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt** e quindi fare clic su **Esegui come amministratore**.)

Per esempi di come utilizzare questo sottocomando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-versione|Specifica la versione del backup nel formato MM/GG/AAAA-HH: MM. Se non si conoscono le informazioni sulla versione, digitare **Wbadmin get versions**.|
|-backupTarget|Specifica il percorso di archiviazione che contiene i backup per i quali si desiderano i dettagli. Utilizzare per l'elenco dei backup archiviati in tale percorso di destinazione. I percorsi di destinazione di backup possono essere un'unità disco collegata localmente o una cartella condivisa remota. Se **Wbadmin get items**viene eseguito nello stesso computer in cui è stato creato il backup, questo parametro non è necessario. Tuttavia, questo parametro è obbligatorio per ottenere informazioni su un backup creato da un altro computer.|
|-machine|Specifica il nome del computer per il quale si desiderano i dettagli di backup. Utile quando è stato eseguito il backup di più computer nello stesso percorso. Deve essere utilizzato quando **- backupTarget** specificato.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per elencare gli elementi del backup eseguito il 31 marzo 2013 alle 9:00, digitare:
```
wbadmin get items -version:03/31/2013-09:00
```
Per elencare gli elementi del backup di Server01 eseguito il 30 aprile 2013 alle 9:00. e archiviato in \\\\servername\share, digitare:
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx)