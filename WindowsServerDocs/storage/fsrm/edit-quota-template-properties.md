---
title: "Modificare le proprietà dei modelli quota"
description: "In questo articolo viene descritto come modificare le proprietà del modello quota per estendere le modifiche apportate alle quote create dal modello quota originale"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0362b30e16dacb354220c770899195240f3e19ee
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="edit-quota-template-properties"></a>Modificare le proprietà dei modelli quota

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando si apportano modifiche a un modello quota, si ha la possibilità di estenderle alle quote create dal modello quota originale. È possibile scegliere di modificare solo le quote che ancora corrispondono al modello originale o tutte le quote derivate dal modello originale, indipendentemente da eventuali modifiche apportate alle quote fin dalla loro creazione. Questa funzionalità semplifica il processo di aggiornamento delle proprietà delle quote offrendo un'unica posizione centrale da cui è possibile apportare tutte le modifiche.

> [!Note]
> Se si sceglie di applicare le modifiche a tutte le quote derivate dal modello originale, verranno sovrascritte tutte le proprietà personalizzate create per le quote.

## <a name="to-edit-quota-template-properties"></a>Per modificare le proprietà dei modelli quota

1.  In **Modelli quote** selezionare il modello che si desidera modificare.

2.  Fare clic con il pulsante destro del mouse sul modello quota, quindi scegliere **Modifica proprietà modello** (o nel riquadro **Azioni**, in **Modelli quota selezionati**, selezionare **Modifica proprietà modello**). Verrà visualizzata la finestra di dialogo **Proprietà modello quota**.

3.  Eseguire tutte le modifiche necessarie. Le impostazioni e le opzioni di notifica sono identiche a quelle che è possibile impostare durante la creazione di un modello quota. Facoltativamente, è possibile copiare le proprietà da un modello diverso e modificarle per questo.

4.  Una volta completata la modifica delle proprietà del modello, fare clic su **OK**. Verrà visualizzata la finestra di dialogo **Aggiorna le quote derivate dal modello**.

5.  Selezionare il tipo di aggiornamento che si desidera applicare:

    -   Se si dispone di quote modificate al momento della loro creazione con il modello originale e non si desidera modificarle, selezionare **Applica il modello solo alle quote derivate corrispondenti al modello originale**. Questa opzione consente di aggiornare solo le quote non modificate al momento della creazione con il modello originale.
    -   Se si desidera modificare tutte le quote esistenti create dal modello originale, selezionare **Applica il modello a tutte le quote derivate**.
    -   Se si desidera mantenere inalterate le quote esistenti, selezionare **Non applicare il modello alle quote derivate**.

6.  Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Gestione delle quote](quota-management.md)
-   [Creare un modello quota](create-quota-template.md)


