---
title: Modificare le proprietà dei modelli per lo screening dei file
description: In questo articolo viene descritto come modificare le proprietà dei modelli per lo screening dei file
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9e84545be86bdb8fcba09d0ff49ac98b44cd7bdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403131"
---
# <a name="edit-file-screen-template-properties"></a>Modificare le proprietà dei modelli per lo screening dei file

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando si apportano modifiche a un modello per lo screening dei file, si ha la possibilità di estendere tali modifiche agli screening dei file creati con il modello per lo screening dei file originale. È possibile scegliere di modificare solo gli screening dei file che corrispondono al modello originale o tutti gli screening dei file che derivano dal modello originale, indipendentemente da eventuali modifiche apportate agli screening dei file al momento dalla loro creazione. Questa funzionalità semplifica il processo di aggiornamento delle proprietà degli screening dei file offrendo un'unica posizione centrale da cui apportare tutte le modifiche.

> [!Note]
> Se le modifiche vengono applicate a tutti gli screening dei file che derivano dal modello originale, verranno sovrascritte tutte le proprietà personalizzate create per lo screening dei file.

## <a name="to-edit-file-screen-template-properties"></a>Per modificare le proprietà dei modelli per lo screening dei file

1.  In **Modelli per lo screening dei file** selezionare il modello che si desidera modificare.

2.  Fare clic con il pulsante destro del mouse sul modello di schermata file e scegliere **modifica proprietà modello** . in alternativa, nel riquadro **Azioni** selezionare **modifica proprietà modello**in **modelli di schermata file selezionati**. Verrà visualizzata la finestra di dialogo **Proprietà modello schermata file** .

3.  Se si desidera copiare le proprietà di un altro modello come base per il modello modificato, selezionare un modello nell'elenco a discesa **Copia proprietà dal modello**. Quindi, fare clic su **Copia**.

4.  Eseguire tutte le modifiche necessarie. Le impostazioni e le opzioni di notifica sono identiche a quelle disponibili durante la creazione di un modello per lo screening dei file.

5.  Una volta completata la modifica delle proprietà del modello, fare clic su **OK**. Verrà visualizzata la finestra di dialogo **Aggiorna screening dei file derivati dal modello**.

6.  Selezionare il tipo di aggiornamento che si desidera applicare:

    -   Se si dispone di screening dei file modificati al momento della loro creazione utilizzando il modello originale e non si desidera modificarli, fare clic su **Applica il modello solo agli screening dei file derivati corrispondenti al modello originale**. Questa opzione consente di aggiornare solo gli screening dei file non modificati al momento della creazione con le proprietà del modello originale.
    -   Se si desidera modificare tutti gli screening dei file esistenti creati usando il modello originale, fare clic su **Applica il modello a tutti gli screening dei file derivati**.
    -   Se si desidera mantenere inalterati gli screening dei file esistenti, fare clic su **Non applicare il modello agli screening dei file derivati**.

7.  Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Gestione screening dei file](file-screening-management.md)
-   [Creare un modello di screening dei file](create-file-screen-template.md)


