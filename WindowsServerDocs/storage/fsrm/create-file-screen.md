---
title: Creare uno screening dei file
description: Questo articolo descrive come creare uno screening dei file
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c1f261eb926eca3ead58b87aeb00a5060b9d957c
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-file-screen"></a>Creare uno screening dei file

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando si crea un nuovo screening dei file, è possibile salvare un modello di screening dei file basato sulle proprietà dello screening dei file personalizzato definito dall'utente. Il vantaggio è che viene mantenuto un collegamento tra gli screening dei file e il modello che viene utilizzato per crearli, in modo che in futuro modifiche apportate al modello possano essere applicate a tutti gli screening dei file derivanti da tale modello. Si tratta di una funzionalità che semplifica l'implementazione delle modifiche ai criteri di archiviazione offrendo un'unica posizione centrale in cui eseguire tutti gli aggiornamenti.

## <a name="to-create-a-file-screen-with-custom-properties"></a>Per creare uno screening dei file con le proprietà personalizzate

1.  In **Gestione screening dei File**, fare clic sul nodo **Screening dei file**.

2.  Fare clic con il pulsante destro su **Screening dei File** e fare clic su **Crea screening dei file** (o selezionare **Crea screening dei file** dal riquadro **Azioni**). Verrà visualizzata la finestra di dialogo **Crea screening dei file**.

3.  In **Percorso screening dei file**, digitare il nome della o accedere alla cartella padre alla quale verrà applicato lo screening dei file. Lo screening dei file verrà applicato alla cartella selezionata e tutte le relative sottocartelle.

4.  In **Scegliere la modalità di configurazione delle proprietà per lo screening dei file.**, fare clic su **Definisci proprietà personalizzate per lo screening dei file**, quindi fare clic su **Proprietà personalizzate**. Verrà visualizzata la finestra di dialogo **Proprietà screening dei file**.

5.  Se si desidera copiare le proprietà di un modello esistente da utilizzare come base per lo screening dei file, selezionare un modello nell'elenco a discesa **Copia proprietà dal modello**. Quindi, fare clic su **Copia**.

    Nella finestra di dialogo **Proprietà screening dei file**, modificare o impostare i valori seguenti nella scheda **Impostazioni**:

6.  In **Tipo di screening**, fare clic sull'opzione **Screening attivo** o **Screening passivo**. (Lo screening attivo impedisce il salvataggio di file che sono membri dei gruppi di file bloccati e generano notifiche quando gli utenti tentano di salvare i file non autorizzati. Lo screening passivo invia le notifiche configurate, ma impedisce il salvataggio dei file).

7.  In **Gruppi di file**, selezionare ogni gruppo di file che si desidera includere nello screening dei file. (Per selezionare la casella di controllo per il gruppo di file, fare doppio clic sull'etichetta del gruppo di file).

    Se si desidera visualizzare i tipi di file che un gruppo di file include ed esclude, fare clic sull'etichetta del gruppo di file e fare clic su **Modifica**. Per creare un nuovo gruppo di file, fare clic su **Crea**.

8.  Inoltre, è possibile configurare **Gestione risorse file server** per generare una o più notifiche impostando le opzioni seguenti nelle schede **Messaggio posta elettronica**, **Registro eventi**, **Comando** e **Report**. Per ulteriori informazioni sulle opzioni di notifica dello screening dei file, vedere [Creare un modello di screening dei file](create-file-screen-template.md).

9.  Dopo aver selezionato tutte le proprietà di screening dei file che si desidera utilizzare, fare clic su **OK** per chiudere la finestra di dialogo **Proprietà screening dei file**.

10. Nella finestra di dialogo **Crea screening dei file**, fare clic su **Crea** per salvare lo screening dei file. Verrà visualizzata la finestra di dialogo **Salva le proprietà personalizzate come modello**.

11. Selezionare il tipo di screening dei file personalizzato che si desidera creare:

    -   Per salvare un modello che si basa su queste proprietà personalizzate (scelta consigliata), fare clic su **Salva le proprietà personalizzate come modello** e immettere un nome per il modello. Questa opzione applicherà il modello al nuovo screening dei file ed è possibile utilizzare il modello per creare screening dei file aggiuntivi in futuro. Questo consentirà di aggiornare automaticamente lo screening dei file in un secondo momento aggiornando il modello.
    -   Se non si desidera salvare un modello quando si salva lo screening dei file, fare clic su **Salva lo screening dei file personalizzato senza creare un modello**.

12. Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Gestione screening dei file](file-screening-management.md)
-   [Definire gruppi di file per lo screening](define-file-groups-for-screening.md)
-   [Creare un modello di screening dei file](create-file-screen-template.md)
-   [Modificare le proprietà dei modelli di screening dei file](edit-file-screen-template-properties.md)


