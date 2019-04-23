---
title: Creare un'eccezione screening dei file
description: Questo articolo descrive come creare un'eccezione screening dei file
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1f0e93cb2535862b9259d438de00c3b769c2282c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866302"
---
# <a name="create-a-file-screen-exception"></a>Creare un'eccezione screening dei file

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

In alcuni casi, è necessario consentire le eccezioni screening dei file. Ad esempio, si potrebbe decidere di bloccare i file video da un file server, ma è necessario consentire al gruppo di formazione di salvare i file video per i corsi su computer. Per consentire l'utilizzo di file bloccati da altri screening file, creare un'*eccezione per screening dei file*.

Un'eccezione screening dei file è un tipo speciale di screening dei file che ignora qualsiasi screening dei file che verrebbe altrimenti applicato a una cartella e a tutte le sottocartelle in un percorso di eccezione designato. Vale a dire, crea un'eccezione alle eventuali regole derivate da una cartella padre.

> [!Note]
> Non è possibile creare un'eccezione screening dei file in una cartella padre in cui è già definito uno screening dei file. È necessario assegnare l'eccezione a una sottocartella o apportare modifiche allo screening dei file esistente.

<br />
Assegnare gruppi di file per determinare quali tipi di file saranno consentiti nell'eccezione screening dei file.

## <a name="to-create-a-file-screen-exception"></a>Per creare un'eccezione screening dei file

1.  In **Gestione screening dei File**, fare clic sul nodo **Screening dei file**.

2.  Fare clic con il pulsante destro su **Screening dei File** e fare clic su **Crea eccezione per screening dei file** (o selezionare **Crea eccezione per screening dei file** dal riquadro **Azioni**). Verrà visualizzata la finestra di dialogo **Crea eccezione per screening dei file**.

3.  Nella casella di testo **Percorso eccezione**, digitare o selezionare il percorso in cui l'eccezione verrà applicata. L'eccezione verrà applicata alla cartella selezionata e tutte le relative sottocartelle.

4.  Per specificare i file da escludere dallo screening dei file:

    -   In **Gruppi di file**, selezionare ogni gruppo di file che si desidera escludere dallo screening dei file. (Per selezionare la casella di controllo per il gruppo di file, fare doppio clic sull'etichetta del gruppo di file).
    -   Se si desidera visualizzare i tipi di file da un gruppo di file inclusi ed esclusi, fare clic sull'etichetta di gruppo di file e fare clic su **modifica**.
    -   Per creare un nuovo gruppo di file, fare clic su **Crea**.

5.  Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Gestione di screening dei file](file-screening-management.md)
-   [Definire gruppi di File per Screening](define-file-groups-for-screening.md)


