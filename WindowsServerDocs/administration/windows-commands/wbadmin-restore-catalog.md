---
title: Wbadmin restore catalog
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b0d646440ca9b30f9fa30fb1ac3ff08458b8e44d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362334"
---
# <a name="wbadmin-restore-catalog"></a>Wbadmin restore catalog



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

## <a name="remarks"></a>Osservazioni

Se il percorso in cui vengono archiviati i backup (disco, DVD o cartella condivisa remota) è danneggiato o perso e non può essere utilizzato per ripristinare il catalogo di backup, utilizzare **wbadmin delete catalogo** per eliminare il catalogo danneggiato. In questo caso, è necessario creare un nuovo backup una volta eliminato il catalogo di backup.

## <a name="BKMK_examples"></a>Esempi

Per ripristinare un catalogo da un backup archiviato su disco d, digitare:
```
wbadmin restore catalog -backupTarget:d
```
Per ripristinare un catalogo da un backup archiviato nella cartella condivisa \\\\servername\share di Server01, digitare:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Ripristino-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) cmdlet