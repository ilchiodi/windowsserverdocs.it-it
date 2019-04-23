---
title: Catalogo di ripristino Wbadmin
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5876a44b178025baac7ee5901cdc32c1b5d33dad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851712"
---
# <a name="wbadmin-restore-catalog"></a>Catalogo di ripristino Wbadmin



Consente di recuperare un catalogo di backup per il computer locale da un percorso di archiviazione specificato.

Per ripristinare un catalogo di backup con questo sottocomando, è necessario essere un membro del **gruppo Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

Per esempi di come utilizzare questo sottocomando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-backupTarget|Specifica il percorso del catalogo di backup del sistema è stato in corrispondenza del punto dopo la creazione del backup.|
|-machine|Specifica il nome del computer in cui si desidera ripristinare il catalogo di backup per. Utilizzare backup per più computer sono stati archiviati nella stessa posizione. Deve essere utilizzato quando **- backupTarget** specificato.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="remarks"></a>Note

Se il percorso in cui vengono archiviati i backup (disco, DVD o cartella condivisa remota) è danneggiato o perso e non può essere utilizzato per ripristinare il catalogo di backup, utilizzare **wbadmin delete catalogo** per eliminare il catalogo danneggiato. In questo caso, è necessario creare un nuovo backup una volta eliminato il catalogo di backup.

## <a name="BKMK_examples"></a>Esempi

Per ripristinare un catalogo da un backup archiviato su disco d, digitare:
```
wbadmin restore catalog -backupTarget:d
```
Per ripristinare un catalogo da un backup archiviato nella cartella condivisa \\ \\servername\share di server01, tipo:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Ripristino-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) cmdlet