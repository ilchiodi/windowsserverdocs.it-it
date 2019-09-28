---
title: Panoramica di Gestione disco
description: Gestione disco è un'utilità di sistema in Windows che permette di eseguire attività di archiviazione avanzate, tra cui l'inizializzazione di una nuova unità, l'estensione dei volumi, la compressione delle partizioni e la modifica delle lettere di unità.
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 46ed1256ed9039311939f9de12ea46416443be9c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402146"
---
# <a name="overview-of-disk-management"></a>Panoramica di Gestione disco

> **Si applica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestione disco è un'utilità di sistema in Windows che permette di eseguire attività di archiviazione avanzate. Ecco alcuni degli aspetti per cui Gestione disco è utile:

- Per impostare una nuova unità. Vedi [inizializzazione di una nuova unità](initialize-new-disks.md).
- Per estendere un volume nello spazio che non sia già parte di un volume nella stessa unità. Vedi [Estendere un volume di base](extend-a-basic-volume.md).
- Per ridurre una partizione, in genere in modo da estendere una partizione adiacente. Vedi [Ridurre un volume di base](shrink-a-basic-volume.md).
- Per modificare una lettera di unità o assegnarne una nuova. Vedi [Modificare una lettera di unità](change-a-drive-letter.md).

![Gestione disco che mostra una tipica unità con tre partizioni: una partizione di sistema da 499 MB, un'unità C di dimensioni maggiori per Windows e un'altra partizione da 499 MB per il ripristino](media/disk-management.png)

> [!TIP]
>  Se ricevi un errore o qualcosa non funziona nel seguire queste procedure, vedi l'argomento [Risoluzione dei problemi relativi a Gestione disco](troubleshooting-disk-management.md). Se l'articolo non è utile, non lasciarti prendere dal panico. Nel sito della [community Microsoft](https://answers.microsoft.com/en-us/windows) sono disponibili tantissime informazioni. Prova a eseguire una ricerca nella sezione [File, cartelle e archiviazione in linea](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) e se ti serve altro aiuto, inserisci una domanda perché Microsoft o altri membri della community possano provare a rispondere. Se hai commenti e suggerimenti su come migliorare questi argomenti, siamo ansiosi di conoscerli. Rispondi alla richiesta *Questa pagina è stata utile?* e lascia i tuoi commenti qui o nel thread dei commenti pubblici alla fine di questo argomento.

Ecco alcune attività comuni che potresti voler eseguire, ma che usano altri strumenti in Windows:

- Per liberare spazio su disco, vedi [Liberare spazio nell'unità in Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space).
- Per deframmentare le unità, vedi [Deframmentare il PC Windows 10](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc).
- Per riunire più unità disco rigido, analogamente a una configurazione RAID, vedi [Spazi di archiviazione](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

## <a name="about-those-extra-recovery-partitions"></a>Informazioni sulle partizioni di ripristino aggiuntive

Per i più curiosi (abbiamo letto i vostri commenti!) Windows include in genere tre partizioni nell'unità principale (in genere l'unità C:\):

![Disco 0 che mostra tre partizioni: una partizione di sistema EFI, la partizione di Windows e una partizione di ripristino del disco](media/windows-partitions.png)

- **Partizione di sistema EFI**: viene usata dai PC moderni per avviare il PC e il sistema operativo.
- **Unità del sistema operativo Windows (C:)** : unità in cui viene installato Windows e in cui normalmente posizioni il resto delle app e dei file.
- **Partizione di ripristino**: unità in cui vengono archiviati strumenti speciali per semplificare il ripristino di Windows in caso di problemi di avvio o esecuzione o di altri problemi gravi.

Anche se Gestione disco può indicare che la partizione di sistema EFI e la partizione di ripristino sono libere al 100%, non è vero. Le partizioni sono in genere piuttosto piene di file molto importanti, necessari al PC per il corretto funzionamento. È consigliabile lasciarle separate in modo che possano svolgere il proprio lavoro durante l'avvio del PC e il ripristino da eventuali problemi.

## <a name="see-also"></a>Vedi anche

- [Gestire dischi](manage-disks.md)
- [Gestire volumi di base](manage-basic-volumes.md)
- [Risoluzione dei problemi relativi a Gestione disco](troubleshooting-disk-management.md)
- [Opzioni di ripristino in Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [Trovare i file perduti dopo l'aggiornamento a Windows 10](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [Eseguire il backup e il ripristino dei file](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [Creare un'unità di ripristino](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [Creare un punto di ripristino del sistema](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [Trovare la chiave di ripristino di BitLocker](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
