---
title: Impostare la priorità di destinazione per eseguire l'override dell'ordinamento dei riferimenti
description: Questo articolo descrive come impostare la priorità di destinazione per eseguire l'override dell'ordinamento dei riferimenti.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 59db08d5ef46b696f550a5fa0738c5c1f9375fda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826362"
---
# <a name="set-target-priority-to-override-referral-ordering"></a>Impostare la priorità di destinazione per eseguire l'override dell'ordinamento dei riferimenti

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Un riferimento è un elenco ordinato di destinazioni che un computer client riceve da un controller di dominio o un server dello spazio dei nomi quando l'utente accede a una directory radice dello spazio dei nomi o a una cartella con destinazioni nello spazio dei nomi. Tutte le destinazioni in un riferimento vengono ordinate in base al metodo impostato per la cartella o la directory radice dello spazio dei nomi. 

Per ridefinire l'ordinamento, puoi impostare la priorità delle singole destinazioni. Puoi ad esempio specificare una destinazione in modo che venga inserita prima o dopo tutte le altre oppure prima o dopo tutte quelle di costo uguale.

## <a name="to-set-target-priority-on-a-root-target-for-a-domain-based-namespace"></a>Per impostare la priorità di una destinazione directory radice di uno spazio dei nomi basato su dominio

Per impostare la priorità di una destinazione radice di uno spazio dei nomi basato su dominio, esegui questa procedura seguente.

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fai clic sullo spazio dei nomi basato su dominio delle destinazioni radice di cui si desidera impostare la priorità nel nodo **Spazi dei nomi**.

3.  Nella scheda **Server dello spazio dei nomi** del riquadro **Dettagli** fai clic con il pulsante destro del mouse sulla destinazione radice con la priorità da modificare, quindi fai clic su **Proprietà**.

4.  Nella scheda **Avanzate** fai clic su **Ignora ordinamento riferimenti**, quindi seleziona la priorità desiderata.

    -   **Prima tra tutte le destinazioni** Indica che gli utenti devono sempre fare riferimento a questa destinazione, se disponibile.
    -   **Ultima tra tutte le destinazioni** Indica che gli utenti devono fare riferimento a questa destinazione solo nel caso in cui non sia disponibile alcuna altra destinazione.
    -   **Prima tra le destinazioni con uguale costo** Indica che gli utenti devono fare riferimento a questa destinazione prima di altre destinazioni con costo uguale, che in genere sono altre destinazioni dello stesso sito.
    -   **Ultima tra le destinazioni con uguale costo** Indica che gli utenti non devono mai fare riferimento a questa destinazione se sono disponibili altre destinazioni con costo uguale, che in genere sono altre destinazioni dello stesso sito.

## <a name="to-set-target-priority-on-a-folder-target"></a>Per impostare la priorità di una destinazione cartella

Per impostare la priorità di una destinazione cartella, esegui questa procedura:

1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione DFS**.

2.  Nell'albero della console fai clic sulla cartella delle destinazioni per cui desideri impostare la priorità nel nodo **Spazio dei nomi**.

3.  Nella scheda **Destinazioni cartella** del riquadro **Dettagli** fai clic con il pulsante destro del mouse sulla destinazione cartella di cui desideri modificare la priorità, quindi scegli **Proprietà**.

4.  Nella scheda **Avanzate** fai clic su **Ignora ordinamento riferimenti**, quindi seleziona la priorità desiderata.

> [!NOTE]
> Per impostare le priorità di destinazione tramite Windows PowerShell, usa i cmdlet [Set-DfsnRootTarget](https://technet.microsoft.com/library/jj884266.aspx) e [Set-DfsnFolderTarget](https://technet.microsoft.com/library/jj884264.aspx) con i parametri **ReferralPriorityClass** e **ReferralPriorityRank**. Questi cmdlet sono stati introdotti in Windows Server 2012.

## <a name="see-also"></a>Vedere anche

-   [Ottimizzazione di spazi dei nomi DFS](tuning-dfs-namespaces.md)
-   [Delegare le autorizzazioni di gestione di spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)