---
title: Definire gruppi di file per lo screening
description: Questo articolo descrive come definire gruppi di file per creare uno spazio dei nomi per lo screening dei file, l'eccezione screening dei file o i rapporti di archiviazione File in base a gruppo di file
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6e043692500370b6c084a4db068027d13afc957f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838432"
---
# <a name="define-file-groups-for-screening"></a>Definire gruppi di file per lo screening

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Un *gruppo di file* viene utilizzato per definire uno spazio dei nomi per uno screening dei file, l'eccezione screening dei file o il rapporto di archiviazione **File in base a gruppo di file** È costituito da un set di modelli di nomi di file, che vengono raggruppati in base a quanto segue:

-   **File da includere**: i file che appartengono al gruppo
-   **File da escludere**: i file che non appartengono al gruppo

> [!Note]
> Per comodità, è possibile creare e modificare i gruppi di file mentre si modificano le proprietà di screening dei file, le eccezioni screening dei file, i modelli di screening dei file e i rapporti **File in base a gruppo di file**. Le modifiche ai gruppi di file apportate da queste pagine delle proprietà non sono limitate all'elemento corrente su cui si sta lavorando.

## <a name="to-create-a-file-group"></a>Per creare un gruppo di file

1.  In **Gestione screening dei File**, fare clic sul nodo **Gruppi di file**.

2.  Nel riquadro **Azioni** fare clic su **Crea gruppo di file**. Verrà visualizzata la finestra di dialogo **Crea proprietà gruppo di file**.

    (In alternativa, mentre si modificano le proprietà di screening dei file, eccezioni screening dei file, modelli di screening dei file o rapporti **File in base a gruppo di file**, in **Gestisci gruppi di file**, fare clic su **Crea**).

3.  Nella finestra di dialogo **Crea proprietà gruppo di file**, digitare un nome per il gruppo di file.

4.  Aggiungere i file da escludere e i file da includere:

    -   Per ogni set di file che si desidera includere nel gruppo di file, nella casella **File da includere**, immettere un modello di nome file e quindi fare clic su **Aggiungi**.
    -   Per ogni set di file che si desidera escludere dal gruppo di file, nella casella **File da escludere**, immettere un modello di nome file e quindi fare clic su **Aggiungi**.
        Si noti che si applicano le regole con caratteri jolly standard, ad esempio,  **\*.exe** Seleziona tutti i file eseguibili.

5.  Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Gestione di screening dei file](file-screening-management.md)
-   [Creare uno screening dei File](create-file-screen.md)
-   [Creare un'eccezione per screening dei File](create-file-screen-exception.md)
-   [Creare un modello di schermata del File](create-file-screen-template.md)
-   [Gestione rapporti archiviazione](storage-reports-management.md)


