---
title: Wbadmin delete systemstatebackup
description: Argomento di riferimento per Wbadmin delete systemstatebackup, che consente di eliminare i backup dello stato del sistema specificati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a84112191ad1b5873ad09c467fb3668107f2b24e
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821431"
---
# <a name="wbadmin-delete-systemstatebackup"></a>Wbadmin delete systemstatebackup



Elimina i backup dello stato del sistema specificati. Se il volume specificato contiene backup diversi dai backup dello stato del sistema del server locale, tali backup non verranno eliminati.

> [!NOTE]
> Windows Server Backup non eseguire il backup o ripristinare l'hive utente (HKEY_CURRENT_USER) come parte del backup dello stato del sistema o il ripristino dello stato del sistema.

Per eliminare un backup dello stato del sistema con questo sottocomando, è necessario essere un membro del gruppo **Backup Operators** o **Administrators** oppure avere ricevuto in delega le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)



## <a name="syntax"></a>Sintassi

```
wbadmin delete systemstatebackup
{-keepVersions:<NumberofCopies> | -version:<VersionIdentifier> | -deleteOldest}
[-backupTarget:<VolumeName>]
[-machine:<BackupMachineName>]
[-quiet]
```

> [!IMPORTANT]
> È necessario specificare solo uno di questi parametri: **-keepVersions**, **-Version**o **-deleteOldest**.

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-keepVersions|Specifica il numero di backup dello stato del sistema più recenti da contenere. Il valore deve essere un numero intero positivo. Il valore del parametro **-keepVersions: 0** Elimina tutti i backup dello stato del sistema.|
|-version|Specifica l'identificatore di versione del backup nel formato MM/GG/AAAA-HH: MM. Se non si conosce l'identificatore di versione, digitare **wbadmin ottenere versioni**.</br>Con questo comando è possibile eliminare le versioni esclusivamente di backup dello stato del sistema. Usare **Wbadmin get items** per visualizzare il tipo di versione.|
|-deleteOldest|Elimina il backup dello stato del sistema meno recente.|
|-backupTarget|Specifica il percorso di archiviazione per il backup che si desidera eliminare. Il percorso di archiviazione per i backup dei dischi può essere una lettera di unità, un punto di montaggio o un percorso del volume basato su GUID. Questo valore deve essere specificato solo per l'individuazione dei backup che non sono del computer locale. Le informazioni sui backup per il computer locale saranno disponibili nel catalogo di backup nel computer locale.|
|-machine|Specifica il computer di cui si desidera eliminare il backup dello stato del sistema. Utile quando è stato eseguito il backup di più computer nello stesso percorso. Deve essere utilizzato quando il **- backupTarget** parametro specificato.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="examples"></a>Esempi

Per eliminare il backup dello stato del sistema creato il 31 marzo 2013 alle 10:00 AM, digitare:
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
Per eliminare tutti i backup dello stato del sistema, ad eccezione dei tre più recenti, digitare:
```
wbadmin delete systemstatebackup -keepVersions:3
```
Per eliminare il backup dello stato del sistema meno recente archiviato sul disco f, digitare:
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)