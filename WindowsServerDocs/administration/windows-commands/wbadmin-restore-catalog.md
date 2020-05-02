---
title: Wbadmin restore catalog
description: Argomento di riferimento per Wbadmin restore catalog, che consente di recuperare un catalogo di backup per il computer locale da un percorso di archiviazione specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de9ce6b64f996e50fb85a8c612104bc6851ebdfd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720136"
---
# <a name="wbadmin-restore-catalog"></a>Wbadmin restore catalog

Consente di recuperare un catalogo di backup per il computer locale da un percorso di archiviazione specificato.

Per ripristinare un catalogo di backup con questo sottocomando, è necessario essere un membro del **gruppo Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

## <a name="syntax"></a>Sintassi

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-backupTarget|Specifica il percorso del catalogo di backup del sistema è stato in corrispondenza del punto dopo la creazione del backup.|
|-machine|Specifica il nome del computer in cui si desidera ripristinare il catalogo di backup per. Utilizzare backup per più computer sono stati archiviati nella stessa posizione. Deve essere utilizzato quando **- backupTarget** specificato.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="remarks"></a>Osservazioni

Se il percorso in cui vengono archiviati i backup (disco, DVD o cartella condivisa remota) è danneggiato o perso e non può essere utilizzato per ripristinare il catalogo di backup, utilizzare **wbadmin delete catalogo** per eliminare il catalogo danneggiato. In questo caso, è necessario creare un nuovo backup una volta eliminato il catalogo di backup.

## <a name="examples"></a>Esempi

Per ripristinare un catalogo da un backup archiviato su disco d, digitare:
```
wbadmin restore catalog -backupTarget:d
```
Per ripristinare un catalogo da un backup archiviato nella cartella \\ \\condivisa servername\share di Server01, digitare:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Ripristino-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) cmdlet