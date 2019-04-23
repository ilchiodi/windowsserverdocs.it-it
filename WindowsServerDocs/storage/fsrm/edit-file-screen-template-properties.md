---
title: Modificare le proprietà dei modelli per lo screening dei file
description: In questo articolo viene descritto come modificare le proprietà dei modelli per lo screening dei file
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 31ca46707a32d23a5dd9606c57bcaec5d6e53a80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846892"
---
# <a name="edit-file-screen-template-properties"></a>Modificare le proprietà dei modelli per lo screening dei file

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando si apportano modifiche a un modello per lo screening dei file, si ha la possibilità di estendere tali modifiche agli screening dei file creati con il modello per lo screening dei file originale. È possibile scegliere di modificare solo gli screening dei file che corrispondono al modello originale o tutti gli screening dei file che derivano dal modello originale, indipendentemente da eventuali modifiche apportate agli screening dei file al momento dalla loro creazione. Questa funzionalità semplifica il processo di aggiornamento delle proprietà degli screening dei file offrendo un'unica posizione centrale da cui apportare tutte le modifiche.

> [!Note]
> Se le modifiche vengono applicate a tutti gli screening dei file che derivano dal modello originale, verranno sovrascritte tutte le proprietà personalizzate create per lo screening dei file.

## <a name="to-edit-file-screen-template-properties"></a>Per modificare le proprietà dei modelli per lo screening dei file

1.  In **Modelli per lo screening dei file** selezionare il modello che si desidera modificare.

2.  Fare clic sul modello di schermata del file e fare clic su **Modifica proprietà modello** (o nel **azioni** riquadro, sotto **modelli di schermata del File selezionato**, selezionare  **Modificare le proprietà dei modelli**.) Verrà visualizzata la **proprietà modello di schermata** nella finestra di dialogo.

3.  Se si desidera copiare le proprietà di un altro modello come base per il modello modificato, selezionare un modello nell'elenco a discesa **Copia proprietà dal modello**. Quindi, fare clic su **Copia**.

4.  Eseguire tutte le modifiche necessarie. Le impostazioni e le opzioni di notifica sono identiche a quelle disponibili durante la creazione di un modello per lo screening dei file.

5.  Una volta completata la modifica delle proprietà del modello, fare clic su **OK**. Verrà visualizzata la finestra di dialogo **Aggiorna screening dei file derivati dal modello**.

6.  Selezionare il tipo di aggiornamento che si desidera applicare:

    -   Se si dispone di screening dei file modificati al momento della loro creazione utilizzando il modello originale e non si desidera modificarli, fare clic su **Applica il modello solo agli screening dei file derivati corrispondenti al modello originale**. Questa opzione consente di aggiornare solo gli screening dei file non modificati al momento della creazione con le proprietà del modello originale.
    -   Se si desidera modificare tutti gli screening dei file esistenti creati usando il modello originale, fare clic su **Applica il modello a tutti gli screening dei file derivati**.
    -   Se si desidera mantenere inalterati gli screening dei file esistenti, fare clic su **Non applicare il modello agli screening dei file derivati**.

7.  Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Gestione di screening dei file](file-screening-management.md)
-   [Creare un modello di schermata del File](create-file-screen-template.md)


