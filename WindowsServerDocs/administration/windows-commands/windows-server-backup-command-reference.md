---
title: Riferimenti ai comandi di Windows Server Backup
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c05a44d3390e110fbaf276dfb9b40c1f0adc1dd5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362054"
---
# <a name="windows-server-backup-command-reference"></a>Riferimenti ai comandi di Windows Server Backup



I sottocomandi seguenti per **Wbadmin** forniscono funzionalità di backup e ripristino da un prompt dei comandi.

Per configurare una pianificazione di backup, è necessario essere un membro del gruppo **Administrators** . Per eseguire tutte le altre attività con questo comando, è necessario essere un membro del **Backup Operators** o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate.

È necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati, fare clic su **Avviare**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.)

|Sottocomando|Descrizione|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|Configura e Abilita una pianificazione di backup giornaliera.|
|[Wbadmin disable backup](wbadmin-disable-backup.md)|Disabilita i backup giornalieri.|
|[Wbadmin start backup](wbadmin-start-backup.md)|Esegue un backup unico. Se usato senza parametri, USA le impostazioni della pianificazione del backup giornaliero.|
|[Wbadmin stop job](wbadmin-stop-job.md)|Arresta l'operazione di backup o ripristino attualmente in esecuzione.|
|[Wbadmin get versions](wbadmin-get-versions.md)|Elenca i dettagli dei backup ripristinabili dal computer locale o, se viene specificato un altro percorso, da un altro computer.|
|[Wbadmin get items](wbadmin-get-items.md)|Elenca gli elementi inclusi in un backup specifico.|
|[Wbadmin start recovery](wbadmin-start-recovery.md)|Esegue un ripristino dei volumi, delle applicazioni, dei file o delle cartelle specificate.|
|[Wbadmin get status](wbadmin-get-status.md)|Mostra lo stato dell'operazione di backup o ripristino attualmente in esecuzione.|
|[Wbadmin get disks](wbadmin-get-disks.md)|Elenca i dischi attualmente in linea.|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Esegue un ripristino dello stato del sistema.|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Esegue un backup dello stato del sistema.|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Elimina uno o più backup dello stato del sistema.|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Esegue un ripristino del sistema completo (almeno tutti i volumi che contengono lo stato del sistema operativo). Questo sottocomando è disponibile solo se si utilizza ambiente ripristino Windows.|
|[Wbadmin restore catalog](wbadmin-restore-catalog.md)|Recupera un catalogo di backup da una posizione di archiviazione specificata nel caso in cui il catalogo di backup nel computer locale sia danneggiato.|
|[Wbadmin delete catalog](wbadmin-delete-catalog.md)|Elimina il catalogo di backup del computer locale. Usare questo comando solo se il catalogo di backup nel computer in uso è danneggiato e non sono presenti backup archiviati in un'altra posizione che è possibile usare per ripristinare il catalogo.|