---
title: Creare una quota automatica
description: Questo articolo descrive come creare quote automatiche in base a un modello quota
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e2837df448434252470d783a6c06f0690ba09021
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="create-an-auto-apply-quota"></a>Creare una quota automatica

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Utilizzando le quote automatiche, è possibile assegnare un modello quota a un volume o cartella padre. Quindi Gestione risorse file server genera automaticamente le quote basate su tale modello. Le quote vengono generate per tutte le sottocartelle esistenti e per le sottocartelle che verranno create in futuro.

Ad esempio, è possibile definire una quota automatica per le sottocartelle create su richiesta, per utenti con profilo roaming o per i nuovi utenti. Ogni volta che viene creata una sottocartella, viene generata automaticamente una nuova voce di quota utilizzando il modello dalla cartella padre. Queste voci quota generate automaticamente possono essere visualizzate come singole quote nel nodo **Quote**. Ogni voce di quota può essere gestita separatamente.

## <a name="to-create-an-auto-apply-quota"></a>Per creare una quota automatica

1.  In **Gestione quote** fare clic sul nodo **Quote**.

2.  Fare clic con il pulsante destro del mouse su **Quote** e scegliere **Crea quota** (o selezionare **Crea quota** nel riquadro **Azioni**). Verrà visualizzata la finestra di dialogo **Crea quota**.

3.  In **Percorso quota**, digitare il nome della o accedere alla cartella padre alla quale verrà applicato il profilo della quota. La quota automatica verrà applicata a tutte le sottocartelle (attuali e future) in questa cartella.

4.  Fare clic su **Applica modello e crea quote nelle sottocartelle nuove ed esistenti**.

5.  In **Deriva proprietà dal modello quota seguente**, selezionare il modello quota che si desidera applicare dall'elenco a discesa. Si noti che vengono visualizzate le proprietà di ciascun modello in **Riepilogo proprietà quota**.

6.  Fai clic su **Crea**.

> [!Note]
> È possibile verificare tutte le quote generate automaticamente selezionando il nodo **Quote** e quindi selezionando **Aggiorna**. Vengono elencate una singola quota per ogni sottocartella e il profilo della quota automatica nel volume o nella cartella padre.

## <a name="see-also"></a>Vedere anche

-   [Gestione delle quote](quota-management.md)
-   [Modificare le proprietà delle quote automatiche](edit-auto-apply-quota-properties.md)