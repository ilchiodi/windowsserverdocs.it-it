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
ms.openlocfilehash: 93da32a204c42b3f4fb503349ff732c9c050db31
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192634"
---
# <a name="change-a-drive-letter"></a>Modificare una lettera di unità

> **Si applica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se si vuole omettere la lettera di unità assegnata a un'unità, o se hai un'unità che non dispone ancora di una lettera di unità, è possibile utilizzare Gestione disco per modificarlo.

> [!IMPORTANT]
> Se si modifica la lettera di unità di un'unità in cui sono installate Windows o App, App potevano avere problemi in esecuzione o la ricerca di tale unità. Per questo motivo è consigliabile non modificare la lettera di unità di un'unità in cui sono installate Windows o App.

Di seguito viene illustrato come modificare la lettera di unità (per invece per montare l'unità in una classe vuota cartella in modo che venga visualizzato come solo un'altra cartella, vedere [assegnare un percorso cartella del punto di montaggio in un'unità](assign-a-mount-point-folder-path-to-a-drive.md)).

1. Aprire Gestione disco con le autorizzazioni di amministratore. 
    A tale scopo, nella casella di ricerca della barra delle applicazioni, digitare **Gestione disco**, selezionare e tenere premuto (o destro) **Gestione disco**, quindi selezionare **Esegui come amministratore**  >  **Sì**. Se non è possibile aprirlo come amministratore, digitare **Gestione Computer** invece e quindi aprire **archiviazione** > **Gestione disco**.
1. In Gestione disco, fare doppio clic su unità per il quale si desidera modificare o aggiungere una lettera di unità e quindi selezionare **Cambia lettera di unità e percorsi**.

    ![Gestione disco che mostra un'unità](media/change-drive-letter.png)
    > [!TIP]
    > Se non viene visualizzato il **Cambia lettera di unità e percorso** opzione oppure è disabilitata, è possibile il volume non è pronto per ricevere una lettera di unità, che può essere il caso se l'unità non allocato e deve essere [inizializzato](initialize-new-disks.md). In alternativa, forse e non mira a accessibile, nel caso delle partizioni di sistema EFI e delle partizioni di ripristino. Se dopo la conferma di avere un volume formattato con una lettera di unità che è possibile accedere e non è ancora possibile modificarla, purtroppo in questo argomento probabilmente non può aiutarti a, pertanto è consigliabile [contattando Microsoft](https://support.microsoft.com/contactus/) o il produttore del PC per ricevere assistenza.

1. Per modificare la lettera di unità, selezionare **modificare**. Per aggiungere una lettera di unità se l'unità non la contiene già uno, selezionare **Add**.

    ![La finestra di dialogo Cambia lettera di unità e percorso](media/change-drive-letter2.png)
1. Selezionare la nuova lettera di unità, selezionare **OK**, quindi selezionare **Yes** quando viene chiesto come programmi che si basano sulla lettera di unità potrebbero non funzionare correttamente.

    ![La finestra di dialogo Cambia lettera di unità o percorso che mostra la modifica della lettera di unità](media/change-drive-letter3.png)