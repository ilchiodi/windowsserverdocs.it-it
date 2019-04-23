---
title: Riferimenti ai comandi di Windows Server Backup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03de0a65-21f0-4dd7-a3ae-251c98bbf6eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ded5039e122832c95eda864bcdcc76f580ca7108
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839812"
---
# <a name="windows-server-backup-command-reference"></a>Riferimenti ai comandi di Windows Server Backup



I sottocomandi seguenti per **wbadmin** forniscono funzionalità di backup e ripristino da un prompt dei comandi.

Per configurare una pianificazione di backup, è necessario essere un membro del **gli amministratori** gruppo. Per eseguire tutte le altre attività con questo comando, è necessario essere un membro del **Backup Operators** o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate.

È necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati, fare clic su **Avviare**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.)

|Sottocomando|Descrizione|
|----------|-----------|
|[Abilita backup Wbadmin](wbadmin-enable-backup.md)|Consente di configurare e consente a una pianificazione del backup giornaliero.|
|[disabilitare il backup Wbadmin](wbadmin-disable-backup.md)|Disabilita i backup giornalieri.|
|[Comando Wbadmin start backup](wbadmin-start-backup.md)|Viene eseguito un backup unico. Se utilizzata senza parametri, Usa le impostazioni di pianificazione del backup giornaliero.|
|[Processo di arresto Wbadmin](wbadmin-stop-job.md)|Arresta l'esecuzione del backup o l'operazione di ripristino.|
|[Wbadmin get versioni](wbadmin-get-versions.md)|Elenca i dettagli di backup recuperabili dal computer locale o, se viene specificato un altro percorso, da un altro computer.|
|[Wbadmin get elementi](wbadmin-get-items.md)|Elenca gli elementi inclusi in un backup specifico.|
|[Comando Wbadmin start recovery](wbadmin-start-recovery.md)|Esegue un ripristino di volumi, applicazioni, file o cartelle specificate.|
|[Comando Wbadmin ottenere lo stato](wbadmin-get-status.md)|Mostra lo stato di esecuzione del backup o di operazione di ripristino.|
|[Wbadmin get dischi](wbadmin-get-disks.md)|Vengono elencati i dischi attualmente online.|
|[Comando Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Esegue un ripristino dello stato del sistema.|
|[Comando Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Viene eseguito un backup dello stato del sistema.|
|[comando Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Elimina uno o più backup dello stato del sistema.|
|[Comando Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Esegue un ripristino del sistema completo (almeno tutti i volumi che contengono lo stato del sistema operativo). Il sottocomando è disponibile solo se si usa l'ambiente ripristino Windows.|
|[Catalogo di ripristino Wbadmin](wbadmin-restore-catalog.md)|Consente di recuperare un catalogo di backup da un percorso di archiviazione specificato nel caso in cui il catalogo di backup nel computer locale è stato danneggiato.|
|[Comando Wbadmin delete catalogo](wbadmin-delete-catalog.md)|Elimina il catalogo di backup del computer locale. Usare questo comando solo se il catalogo di backup in questo computer è danneggiato e non sono disponibili backup archiviato in un altro percorso che è possibile usare per ripristinare il catalogo.|