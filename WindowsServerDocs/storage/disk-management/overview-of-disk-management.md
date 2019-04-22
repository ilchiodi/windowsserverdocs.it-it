---
title: Panoramica di Gestione disco
description: Gestione disco è un'utilità di sistema in Windows che consente di eseguire le attività di archiviazione avanzate, ad esempio l'inizializzazione di una nuova unità, estensione dei volumi, la compattazione di partizioni e la modifica delle lettere di unità.
ms.date: 4/2/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2a467c64a3e0ff38b5165b9e001fc2deb2d92148
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819282"
---
# <a name="overview-of-disk-management"></a>Panoramica di Gestione disco

> **Si applica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestione disco è un'utilità di sistema in Windows che consente di eseguire attività di archiviazione avanzate. Ecco alcune delle attività che Gestione disco è ideale per:

- Per impostare una nuova unità, vedere [inizializzazione di una nuova unità](initialize-new-disks.md).
- Per estendere un volume nello spazio che non sia già parte di un volume nella stessa unità, vedere [estendere un volume di base](extend-a-basic-volume.md).
- Per compattare una partizione, in genere in modo che sia possibile estendere una partizione adiacente, vedere [ridurre un volume base](shrink-a-basic-volume.md).
- Per modificare una lettera di unità o assegnare una lettera di unità, vedere [modifica di una lettera di unità](change-a-drive-letter.md).

![Gestione disco che mostra una tipica unità con tre partizioni: una partizione di sistema 499 MB, un'unità C di dimensioni maggiori per Windows e un'altra partizione 499 MB per il ripristino](media/disk-management.png)

> [!TIP]
>  Se si verifica un errore o qualcosa non funziona quando si seguono queste procedure, consultare il [risoluzione dei problemi di Gestione disco](troubleshooting-disk-management.md) argomento. Se il problema persiste, nessun problema È una considerevole quantità di informazioni [della community di Microsoft](https://answers.microsoft.com/en-us/windows) del sito - prova a cercare il [i file, cartelle e archiviazione](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) sezione e se è ancora necessaria assistenza, pubblicare una domanda e Microsoft o altri membri del community tenterà di Guida in linea. Se hai commenti e suggerimenti su come migliorare questi argomenti, saremo lieti di rispondere! Rispondere solo il *è utile questa pagina?* richiesto e lasciare eventuali commenti non esiste o nel thread di pubblico di commenti nella parte inferiore di questo argomento.

Ecco alcune attività comuni, che si potrebbe voler eseguire operazioni, ma che usano altri strumenti in Windows:

- Per liberare spazio su disco, vedere [liberare spazio nell'unità in Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space).
- Per deframmentare le unità, vedere [deframmentare i PC Windows 10](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc).
- Per sfruttare più dischi rigidi e pool loro insieme, in modo analogo a una configurazione RAID, vedere [spazi di archiviazione](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

## <a name="about-those-extra-recovery-partitions"></a>Su tali partizioni di ripristino aggiuntivi

Nel caso in cui curiosità (aver letto i commenti!), Windows include in genere tre partizioni sull'unità principale (in genere C:\ unità):

![0 che mostra tre partizioni: una partizione di sistema EFI, la partizione di Windows e una partizione di ripristino del disco](media/windows-partitions.png)

- **Partizione di sistema EFI** -viene utilizzato per i PC moderni per start (avvio) al computer e il sistema operativo.
- **Unità del sistema operativo Windows (c)** -si tratta in cui è installato Windows e in genere si inserita il resto delle App e dei file.
- **Partizione di ripristino** -si tratta in cui sono archiviati strumenti speciali per il ripristino di Windows nel caso in cui viene eseguito in altri problemi gravi o avere problemi di avvio.

Sebbene Gestione disco potrebbe mostrare la partizione di sistema EFI e la partizione di ripristino come 100% di spazio disponibile, lo si trova. Le partizioni sono in genere piuttosto complete con i file realmente importanti che il PC richiede per il corretto funzionamento. È consigliabile lasciarle da solo per svolgere il proprio lavoro iniziale del PC e alla possibilità di risolvere i problemi.

## <a name="see-also"></a>Vedere anche

- [Gestire i dischi](manage-disks.md)
- [Gestire volumi di base](manage-basic-volumes.md)
- [Risoluzione dei problemi relativi a Gestione disco](troubleshooting-disk-management.md)
- [Opzioni di ripristino in Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [Trovare i file persi dopo l'aggiornamento a Windows 10](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [Eseguire il backup e ripristino dei file](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [Creare un'unità di ripristino](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [Creare un punto di ripristino di sistema](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [Trovare la chiave di ripristino di BitLocker](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
