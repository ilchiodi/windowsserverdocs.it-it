---
title: Modificare una lettera di unità
description: Come modificare o assegnare una lettera di unità in Windows tramite Gestione disco.
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b972ab05c192dca9a9a0a2bda4f083d2906acadb
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812478"
---
# <a name="change-a-drive-letter"></a>Modificare una lettera di unità

> **Si applica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se vuoi omettere la lettera di unità assegnata a un'unità o se un'unità non è ancora associata a una lettera di unità, puoi usare Gestione disco per modificare la lettera.

> [!IMPORTANT]
> Se modifichi la lettera di un'unità in cui sono installati Windows o app, le app possono avere problemi di esecuzione o non riuscire a individuare l'unità. Per questo motivo, ti consigliamo di non modificare la lettera di un'unità in cui sono installati Windows o app.

Di seguito viene descritto come modificare la lettera di unità. Per montare invece l'unità in una cartella vuota in modo che venga visualizzata proprio come un'altra cartella, vedi [Assegnare un percorso di cartella di un punto di montaggio a un'unità](assign-a-mount-point-folder-path-to-a-drive.md).

1. Apri Gestione disco con autorizzazioni di amministratore. 
    A questo scopo, nella casella di ricerca sulla barra delle applicazioni digita **Gestione disco**, tieni selezionato (o fai clic con il pulsante destro del mouse) **Gestione disco** e quindi scegli **Esegui come amministratore** > **Sì**. Se non riesci ad aprire lo strumento come amministratore, digita invece **Gestione computer** e quindi passa ad **Archiviazione** > **Gestione disco**.
1. In Gestione disco fai doppio clic sull'unità per la quale vuoi modificare o aggiungere una lettera di unità e quindi scegli **Cambia lettera e percorso di unità**.

    ![Gestione disco con un'unità visualizzata](media/change-drive-letter.png)
    > [!TIP]
    > Se l'opzione **Cambia lettera e percorso di unità** non è visualizzata o è disattivata, è possibile che il volume non sia pronto per ricevere una lettera di unità, ad esempio se l'unità non è allocata e deve essere [inizializzata](initialize-new-disks.md). È anche possibile che l'unità non debba essere accessibile, nel caso di partizioni di sistema EFI e partizioni di ripristino. Se il problema persiste anche se hai verificato che si tratta di un volume formattato con una lettera di unità cui puoi accedere, sfortunatamente questo argomento non ti sarà utile e ti suggeriamo di [contattare Microsoft](https://support.microsoft.com/contactus/) o il produttore del PC per altre informazioni.

1. Per modificare la lettera di unità, seleziona **Cambia**. Per aggiungere una lettera di unità se l'unità non ne ha già una, seleziona **Aggiungi**.

    ![Finestra di dialogo Cambia lettera e percorso di unità](media/change-drive-letter2.png)
1. Seleziona la nuova lettera di unità, seleziona **OK** e quindi **Sì** quando ti viene chiesto di confermare il fatto che alcuni programmi che si basano sulla lettera di unità potrebbero non funzionare correttamente.

    ![Finestra di dialogo Cambia lettera e percorso di unità che mostra la modifica della lettera di unità](media/change-drive-letter3.png)