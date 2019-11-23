---
title: comando Wbadmin start systemstatebackup
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0244f984d29c8a802475d2dc08f1cdfe4495f0b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362235"
---
# <a name="wbadmin-start-systemstatebackup"></a>comando Wbadmin start systemstatebackup



Crea un backup dello stato del sistema del computer locale e viene archiviato nel percorso specificato.

> [!NOTE]
> Windows Server Backup non eseguire il backup o ripristinare l'hive utente (HKEY_CURRENT_USER) come parte del backup dello stato del sistema o il ripristino dello stato del sistema.

Per eseguire un backup dello stato del sistema con il sottocomando, è necessario essere un membro del **gruppo Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

Per esempi di come utilizzare questo sottocomando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

## <a name="parameters"></a>Parametri

|   Parametro   |                                                                                                                                                                                                                      Descrizione                                                                                                                                                                                                                      |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -backupTarget | Specifica il percorso in cui si desidera archiviare il backup. Il percorso di archiviazione richiede una lettera di unità o un volume basato su GUID nel formato: \\\\? \Volume{*GUID*}.</br>Un backup dello stato del sistema in una cartella di rete condivisa non è supportato in un computer che esegue Windows Server 2008. Se nel server è in esecuzione Windows Server 2008 R2 o versione successiva, è possibile usare il comando **-backupTarget:\\\\servername\sharedFolder\\** per archiviare i backup dello stato del sistema. |
|    -quiet     |                                                                                                                                                                                                   Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.                                                                                                                                                                                                    |

## <a name="remarks"></a>Osservazioni

Per informazioni sul salvataggio di un backup dello stato del sistema in un volume che, a sua volta, contiene i file di stato del sistema, vedere l'articolo 944530 della Microsoft Knowledge base ([https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439)).

## <a name="BKMK_examples"></a>Esempi

Per creare un backup dello stato del sistema e archiviarlo nel volume f, digitare:
```
wbadmin start systemstatebackup -backupTarget:f:
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBBackup](https://technet.microsoft.com/library/jj902459.aspx) cmdlet