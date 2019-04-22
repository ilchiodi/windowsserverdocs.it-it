---
title: Creare una regola di classificazione automatica
description: Questo articolo descrive come creare una regola di classificazione per una proprietà.
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c472949228184c6202681d257412c046bbc90d37
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812302"
---
# <a name="create-an-automatic-classification-rule"></a>Creare una regola di classificazione automatica

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

La seguente procedura descrive il processo di creazione di una regola di classificazione. Ogni regola imposta il valore di una singola proprietà. Per impostazione predefinita, una regola viene eseguita una sola volta e ignora i file che hanno già un valore di proprietà assegnato. Tuttavia, è possibile configurare una regola per valutare i file indipendentemente dal fatto che un valore sia già assegnato alla proprietà.

## <a name="to-create-a-classification-rule"></a>Per creare una regola di classificazione

1.  In **Gestione classificazioni** fare clic sul nodo **Regole di classificazione**.

2.  Fare clic con il pulsante destro del mouse su **Regole di classificazione**, quindi fare clic su **Crea una nuova regola** (oppure fare clic su **Creare una nuova regola** nel riquadro **Azioni**). Verrà visualizzata la finestra di dialogo **Definizioni regole classificazione**.

3.  Nella scheda **Impostazioni regole** immettere le informazioni seguenti:

    -   **Nome regola**. Digitare un nome per la regola.
    -   **Attivata**. La regola verrà applicata solo se la casella di controllo Attivata è selezionata. Per disabilitare la regola, deselezionare la casella di controllo.
    -   **Descrizione**. Digitare una descrizione facoltativa per questa regola.
    -   **Ambito**. Fare clic su **Aggiungi** per selezionare un percorso in cui si applica questa regola. È possibile aggiungere più posizioni o rimuovere un percorso facendo clic su **Rimuovi**. La regola di classificazione verrà applicata a tutte le cartelle e sottocartelle in questo elenco.

4.  Nella scheda **Classificazione** immettere le informazioni seguenti:

    -   **Meccanismo di classificazione**. Scegliere un metodo per assegnare il valore della proprietà.
    -   **Nome proprietà** Selezionare la proprietà che verrà assegnata da questa regola.
    -   **Valore della proprietà** Selezionare il valore della proprietà che verrà assegnato da questa regola.

5.  Facoltativamente, fare clic sul pulsante **Avanzate** per selezionare ulteriori opzioni. Nella scheda **Tipo valutazione**, la casella di controllo **Rivaluta file** non è selezionata per impostazione predefinita. Le opzioni che possono essere selezionate in questo punto sono le seguenti:

    -   **Valutare nuovamente i file** deselezionata: Se e solo se la proprietà specificata dalla regola non è stata impostata su qualsiasi valore sul file, viene applicata una regola in un file.
    -   **Rivaluta file** selezionata e opzione **Sovrascrivi il valore esistente** selezionata: la regola verrà applicata ai file di ogni volta che viene eseguito il processo di classificazione automatica. Ad esempio, se un file ha una proprietà booleana impostata su **Sì**, una regola che utilizza il classificatore di cartelle per impostare tutti i file su **No** con questo set di opzioni lascerà la proprietà impostata su **No**.
    -   **Valutare nuovamente i file** controllati e il **aggregare i valori** opzione selezionata: La regola verrà applicata ai file ogni volta che viene eseguito il processo di classificazione automatica. Tuttavia, quando la regola ha deciso quale valore impostare al file della proprietà, aggrega tale valore con quello nel file. Ad esempio, se un file ha una proprietà booleana impostata su **Sì**, una regola che utilizza il classificatore di cartelle per impostare tutti i file su **No** con questo set di opzioni lascerà la proprietà impostata su **Sì**.

    Nella scheda **Parametri di classificazione aggiuntivi**, è possibile specificare parametri aggiuntivi riconosciuti dal metodo di classificazione selezionato immettendo il nome e il valore e facendo clic sul pulsante **Inserisci**.

6.  Fare clic su **OK** o su **Annulla** per chiudere la finestra di dialogo **Avanzate**.

7.  Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Creare una proprietà di classificazione](create-classification-property.md)
-   [Gestione classificazioni](classification-management.md)