---
title: Generare rapporti su richiesta
description: In questo articolo viene descritto come generare rapporti su richiesta per analizzare l'utilizzo del disco nel server
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9b3cde8a01c40a04df1bc433687029a2e0e7c394
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403003"
---
# <a name="generate-reports-on-demand"></a>Generare rapporti su richiesta

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Durante le operazioni quotidiane, è possibile utilizzare l'opzione **Genera rapporti** per generare uno o più rapporti su richiesta. Con questi rapporti è possibile analizzare diversi aspetti dell'utilizzo del disco corrente sul server. I dati correnti vengono raccolti prima della generazione dei rapporti.

Quando si generano rapporti su richiesta, questi vengono salvati in un percorso predefinito specificato nella finestra di dialogo **Gestione risorse file server**, ma non viene creata alcuna attività rapporto per l'utilizzo futuro. È possibile visualizzare i rapporti subito dopo la generazione o inviarli tramite posta elettronica a un gruppo di amministratori.

> [!Note]
> Se si sceglie di aprire i rapporti immediatamente, è necessario attendere che vengano generati. Il tempo necessario all'elaborazione varia a seconda dei tipi di rapporti e all'ambito dei dati.

## <a name="to-generate-reports-immediately"></a>Per generare rapporti immediatamente

1. Fare clic sul nodo **Gestione rapporti di archiviazione**.

2. Fare clic con il pulsante destro del mouse su **Gestione rapporti di archiviazione**, quindi scegliere **Genera rapporti** (o selezionare **Genera rapporti** nel riquadro **Azioni**). Verrà visualizzata la finestra di dialogo **Proprietà attività rapporto archiviazione**.

3. Per selezionare volumi o cartelle su cui si desidera generare rapporti:

   -   In **Ambito** fare clic su **Aggiungi**.
   -   Visualizzare il volume o la cartella in cui si desidera generare i rapporti, selezionarla, quindi fare clic su **OK** per aggiungere il percorso all'elenco.
   -   Aggiungere tutti i volumi o tutte le cartelle che si desidera includere nei rapporti. Per rimuovere un volume o una cartella, fare clic sul percorso, quindi fare clic su **Rimuovi**.

4. Per specificare i rapporti da generare:

    -   In **Dati rapporto** selezionare tutti i rapporti che si desidera includere.

   Per modificare i parametri di un rapporto:

   -   Fare clic sull'etichetta del rapporto, quindi fare clic su **Modifica parametri**.
   -   Nella finestra di dialogo **Parametri rapporto** modificare i parametri in base alle esigenze e fare clic su **OK**.
   -  Per visualizzare un elenco di parametri per tutti i rapporti selezionati, fare clic su **Visualizza rapporti selezionati**, quindi su **Chiudi**.
 
5. Per specificare i formati in cui salvare i rapporti:

   -  In **Formati rapporto** selezionare uno o più formati per i rapporti pianificati. Per impostazione predefinita, i rapporti vengono generati in HTML dinamico (DHTML). È possibile selezionare anche HTML, XML, CSV e formati di testo. I rapporti vengono salvati nel percorso predefinito per i rapporti su richiesta.

6. Per recapitare copie dei rapporti agli amministratori tramite posta elettronica:

   - Nella scheda **Recapito** selezionare la casella di controllo **Invia rapporti agli amministratori seguenti**, quindi immettere i nomi degli account amministrativi a cui verranno inviati i rapporti. 
   - Usare il formato <em>account@domain</em> e separare gli account con un punto e virgola.

7. Per raccogliere i dati e generare i rapporti, fare clic su **OK**. Verrà visualizzata la finestra di dialogo **Genera rapporti archiviazione**.

8. Selezionare la modalità di generazione dei rapporti su richiesta:

   -   Se si desidera visualizzare i rapporti subito dopo la loro generazione, fare clic su **Attendi che i rapporti vengano generati, quindi visualizzali**. Ogni rapporto si apre nella relativa finestra.
   -   Per visualizzare i rapporti in un secondo momento, fare clic su **Genera rapporti in background**.

   Entrambe le opzioni consentono di salvare i rapporti e, se è stato abilitato il recapito tramite posta elettronica, inviano i rapporti agli amministratori nei formati selezionati.

## <a name="see-also"></a>Vedere anche

-   [Gestione rapporti di archiviazione](storage-reports-management.md)
-   [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)

