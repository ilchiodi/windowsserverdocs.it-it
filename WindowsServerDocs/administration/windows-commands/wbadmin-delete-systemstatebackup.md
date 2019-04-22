---
title: comando Wbadmin delete systemstatebackup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6801ca5985af626ccb7f6170fbcd6f8fc8305ba1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813152"
---
# <a name="wbadmin-delete-systemstatebackup"></a>comando Wbadmin delete systemstatebackup



Elimina i backup dello stato di sistema specificato. Se il volume specificato contiene i backup diverso dal backup dello stato del sistema del server locale, non tali backup verranno eliminati.

> [!NOTE]
> Windows Server Backup non eseguire il backup o ripristinare l'hive utente (HKEY_CURRENT_USER) come parte del backup dello stato del sistema o il ripristino dello stato del sistema.

Per eliminare un backup dello stato del sistema con il sottocomando, è necessario essere un membro del **al gruppo Backup Operators** gruppo o il **gli amministratori** gruppo, oppure avere ricevuto delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
wbadmin delete systemstatebackup
{-keepVersions:<NumberofCopies> | -version:<VersionIdentifier> | -deleteOldest}
[-backupTarget:<VolumeName>]
[-machine:<BackupMachineName>]
[-quiet]
```

> [!IMPORTANT]
> È necessario specificare solo uno di questi parametri: **- keepVersions**, **-versione**, o **- deleteOldest**.

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-keepVersions|Specifica il numero per mantenere i backup dello stato sistema più recente. Il valore deve essere un numero intero positivo. Il valore del parametro **- keepVersions: 0** Elimina tutti i backup dello stato del sistema.|
|-versione|Specifica l'identificatore di versione del backup in MM/GG/AAAA-formato hh: mm. Se non si conosce l'identificatore di versione, digitare **wbadmin ottenere versioni**.</br>Le versioni che sono esclusivamente i backup dello stato di sistema possono essere eliminate usando questo comando. Uso **wbadmin ottenere elementi** per visualizzare il tipo di versione.|
|-deleteOldest|Elimina il meno recente backup dello stato del sistema.|
|-backupTarget|Specifica il percorso di archiviazione per il backup che si desidera eliminare. Il percorso di archiviazione per i backup dei dischi può essere una lettera di unità, un punto di montaggio o un percorso basato su GUID volume. Questo valore deve solo essere specificata per individuare i backup che non sono del computer locale. Informazioni sui backup per il computer locale sarà disponibile nel catalogo di backup nel computer locale.|
|-machine|Specifica il computer il cui backup dello stato del sistema che si desidera eliminare. È utile quando più computer è stato eseguito il nello stesso percorso. Deve essere utilizzato quando il **- backupTarget** parametro specificato.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="BKMK_examples"></a>Esempi

Per eliminare i backup dello stato del sistema creato in al 31 marzo 2013 alle 10:00, digitare:
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
Per eliminare tutti i backup dello stato di sistema, tranne le tre più recenti, digitare:
```
wbadmin delete systemstatebackup -keepVersions:3
```
Per eliminare il backup dello stato sistema meno recente archiviato nel disco f, digitare:
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)