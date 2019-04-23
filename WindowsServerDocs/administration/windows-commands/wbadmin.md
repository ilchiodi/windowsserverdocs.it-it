---
title: wbadmin
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ce8109ef6a0885abd02ef1dee9f11d21b7d7e9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858882"
---
# <a name="wbadmin"></a>wbadmin



Consente di eseguire il backup e ripristinare il sistema operativo, volumi, file, cartelle e le applicazioni da un prompt dei comandi.

Per configurare un backup pianificato, è necessario essere un membro del **amministratori** gruppo. Per eseguire tutte le altre attività con questo comando, è necessario essere un membro del **Backup Operators** o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate.

È necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati, fare doppio clic su **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

## <a name="subcommands"></a>Sottocomandi

|Sottocomando|Descrizione|
|----------|-----------|
|[Abilita backup Wbadmin](wbadmin-enable-backup.md)|Configura e abilita il backup regolare.|
|[disabilitare il backup Wbadmin](wbadmin-disable-backup.md)|Disabilita i backup giornalieri.|
|[Comando Wbadmin start backup](wbadmin-start-backup.md)|Viene eseguito un backup unico. Se utilizzata senza parametri, Usa le impostazioni di pianificazione del backup giornaliero.|
|[Processo di arresto Wbadmin](wbadmin-stop-job.md)|Arresta l'esecuzione del backup o l'operazione di ripristino.|
|[Wbadmin get versioni](wbadmin-get-versions.md)|Elenca i dettagli di backup recuperabili dal computer locale o, se viene specificato un altro percorso, da un altro computer.|
|[Wbadmin get elementi](wbadmin-get-items.md)|Elenca gli elementi inclusi in un backup.|
|[Comando Wbadmin start recovery](wbadmin-start-recovery.md)|Esegue un ripristino di volumi, applicazioni, file o cartelle specificate.|
|[Comando Wbadmin ottenere lo stato](wbadmin-get-status.md)|Mostra lo stato di esecuzione del backup o di operazione di ripristino.|
|[Wbadmin get dischi](wbadmin-get-disks.md)|Vengono elencati i dischi attualmente online.|
|[Comando Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Esegue un ripristino dello stato del sistema.|
|[Comando Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Viene eseguito un backup dello stato del sistema.|
|[comando Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Elimina uno o più backup dello stato del sistema.|
|[Comando Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Esegue un ripristino del sistema completo (almeno tutti i volumi che contengono lo stato del sistema operativo). Il sottocomando è disponibile solo se si utilizza l'ambiente ripristino Windows.|
|[Catalogo di ripristino Wbadmin](wbadmin-restore-catalog.md)|Consente di recuperare un catalogo di backup da un percorso di archiviazione specificato nel caso in cui il catalogo di backup nel computer locale è stato danneggiato.|
|[Comando Wbadmin delete catalogo](wbadmin-delete-catalog.md)|Elimina il catalogo di backup del computer locale. Utilizzare questo sottocomando solo se il catalogo di backup nel computer in uso è danneggiato e non sono disponibili backup archiviato in un altro percorso che è possibile utilizzare per ripristinare il catalogo.|

#### <a name="additional-references"></a>Altri riferimenti

-   [Backup e ripristino](https://go.microsoft.com/fwlink/?LinkID=195054)
-   [Cmdlet di Windows Server Backup in Windows PowerShell](https://technet.microsoft.com/library/jj902428.aspx)