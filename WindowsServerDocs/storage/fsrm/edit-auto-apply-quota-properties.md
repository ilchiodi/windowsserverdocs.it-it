---
title: Modificare le proprietà delle quote automatiche
description: Questo articolo descrive come modificare le proprietà delle quote automatiche
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4b4fda5cdfeed8df02fee922c8dc5fddc75c56ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403116"
---
# <a name="edit-auto-apply-quota-properties"></a>Modificare le proprietà delle quote automatiche

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando si apportano modifiche a una quota automatica, si ha la possibilità di estenderle anche alle quote esistenti nel percorso della quota automatica. È possibile scegliere di modificare solo le quote che ancora corrispondono alla quota automatica originale o tutte le quote nel percorso della quota automatica, indipendentemente dalle modifiche apportate alle quote dal momento della loro creazione. Questa funzionalità semplifica il processo di aggiornamento delle proprietà di quote derivate da una quota automatica in quanto offre un'unica posizione centrale da cui è possibile apportare tutte le modifiche.

> [!Note]
> Se si sceglie di applicare le modifiche a tutte le quote nel percorso della quota automatica, verranno sovrascritte tutte le proprietà personalizzate create per la quota.

## <a name="to-edit-an-auto-apply-quota"></a>Per modificare una quota automatica

1.  In **Quote** selezionare la quota automatica che si desidera modificare. È possibile filtrare le quote in modo da visualizzare solo le quote automatiche.

2.  Fare clic con il pulsante destro del mouse sulla voce di quota, quindi scegliere **Modifica proprietà quote** (o nel riquadro **Azioni**, in **Quote selezionate**, selezionare **Modifica proprietà quote**). Verrà visualizzata la finestra di dialogo **Modifica quota automatica**.

3.  In **Deriva proprietà dal modello quota seguente** selezionare il modello quota che si desidera applicare. È possibile esaminare le proprietà di ogni modello quota nella casella di riepilogo.

4.  Fare clic su **OK**. Verrà visualizzata la finestra di dialogo **Aggiorna le quote derivate in base alla quota automatica**.

5.  Selezionare il tipo di aggiornamento che si desidera applicare:

    -   Se si dispone di quote che sono state modificate dopo essere state generate automaticamente, e non si desidera modificarle, selezionare **Applica la quota automatica solo alle quote derivate corrispondenti alla quota automatica originale**. Questa opzione aggiornerà solo le quote nel percorso della quota automatica che non sono state modificate da quando sono state automaticamente generate.
    -   Se si desidera modificare tutte le quote esistenti nel percorso della quota automatica, selezionare **Applica la quota automatica a tutte le quote derivate**.
    -   Se non si desidera modificare le quote esistenti ma rendere effettiva la quota automatica modificata per le nuove sottocartelle nel percorso della quota automatica, selezionare **Non applicare la quota automatica alle quote derivate**.

6.  Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Gestione delle quote](quota-management.md)
-   [Creare una quota automatica](create-auto-apply-quota.md)


