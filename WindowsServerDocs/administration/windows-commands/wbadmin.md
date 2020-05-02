---
title: wbadmin
description: Argomento di riferimento per Wbadmin, che consente di eseguire il backup e il ripristino del sistema operativo, di volumi, file, cartelle e applicazioni da un prompt dei comandi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94f07d17d46dad4e5301ba3ea6be94b10f26a3af
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725830"
---
# <a name="wbadmin"></a>wbadmin



Consente di eseguire il backup e ripristinare il sistema operativo, volumi, file, cartelle e le applicazioni da un prompt dei comandi.

Per configurare un backup pianificato, è necessario essere un membro del **amministratori** gruppo. Per eseguire tutte le altre attività con questo comando, è necessario essere un membro del **Backup Operators** o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate.

È necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati, fare clic con il pulsante destro del mouse su **prompt dei comandi**e quindi scegliere **Esegui come amministratore**).

## <a name="subcommands"></a>Sottocomandi

|Sottocomando|Descrizione|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|Configura e abilita il backup regolare.|
|[Wbadmin disable backup](wbadmin-disable-backup.md)|Disabilita i backup giornalieri.|
|[Wbadmin start backup](wbadmin-start-backup.md)|Esegue un backup unico. Se usato senza parametri, USA le impostazioni della pianificazione del backup giornaliero.|
|[Wbadmin stop job](wbadmin-stop-job.md)|Arresta l'operazione di backup o ripristino attualmente in esecuzione.|
|[Wbadmin get versions](wbadmin-get-versions.md)|Elenca i dettagli dei backup ripristinabili dal computer locale o, se viene specificato un altro percorso, da un altro computer.|
|[Wbadmin get items](wbadmin-get-items.md)|Elenca gli elementi inclusi in un backup.|
|[Wbadmin start recovery](wbadmin-start-recovery.md)|Esegue un ripristino dei volumi, delle applicazioni, dei file o delle cartelle specificate.|
|[Wbadmin get status](wbadmin-get-status.md)|Mostra lo stato dell'operazione di backup o ripristino attualmente in esecuzione.|
|[Wbadmin get disks](wbadmin-get-disks.md)|Elenca i dischi attualmente in linea.|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Esegue un ripristino dello stato del sistema.|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Esegue un backup dello stato del sistema.|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Elimina uno o più backup dello stato del sistema.|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Esegue un ripristino del sistema completo (almeno tutti i volumi che contengono lo stato del sistema operativo). Il sottocomando è disponibile solo se si utilizza l'ambiente ripristino Windows.|
|[Wbadmin restore catalog](wbadmin-restore-catalog.md)|Recupera un catalogo di backup da una posizione di archiviazione specificata nel caso in cui il catalogo di backup nel computer locale sia danneggiato.|
|[Wbadmin delete catalog](wbadmin-delete-catalog.md)|Elimina il catalogo di backup del computer locale. Utilizzare questo sottocomando solo se il catalogo di backup nel computer in uso è danneggiato e non sono disponibili backup archiviato in un altro percorso che è possibile utilizzare per ripristinare il catalogo.|

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Backup e ripristino](https://go.microsoft.com/fwlink/?LinkID=195054)
-   [Cmdlet di Windows Server Backup in Windows PowerShell](https://technet.microsoft.com/library/jj902428.aspx)