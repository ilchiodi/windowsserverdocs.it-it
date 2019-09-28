---
title: Pianificare una serie di rapporti
description: In questo articolo viene descritto come generare una serie di rapporti sulla base di una pianificazione regolare
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b718ceed7a378649c51e1ca64bffaaddf051c292
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394056"
---
# <a name="schedule-a-set-of-reports"></a>Pianificare una serie di rapporti

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Per generare una serie di rapporti in una pianificazione regolare, è necessario pianificare un'*attività rapporto*. L'attività rapporto consente di specificare i rapporti da generare e i parametri da utilizzare, i volumi e le cartelle su cui eseguire rapporti, la frequenza con cui tali rapporti devono essere generati e i formati di file in cui verranno salvati.

I rapporti pianificati vengono salvati in un percorso predefinito, che è possibile specificare nella finestra di dialogo **Opzioni Gestione risorse file server**. È, inoltre, possibile recapitare i rapporti tramite posta elettronica a un gruppo di amministratori.

> [!Note]
> Per ridurre al minimo l'impatto che l'elaborazione dei rapporti potrebbe avere sulle prestazioni, è consigliabile generare più rapporti nella stessa pianificazione affinché i dati vengano raccolti una sola volta. Per aggiungere rapidamente rapporti alle attività rapporto, è possibile utilizzare l'azione **Aggiungi o rimuovi rapporti per un'attività rapporto**. Questa azione consente di aggiungere o rimuovere rapporti da più attività rapporto e di modificare i parametri dei rapporti. Per modificare le pianificazioni o gli indirizzi di recapito, è necessario modificare le singole attività rapporto.

## <a name="to-schedule-a-report-task"></a>Per pianificare un'attività rapporto

1. Fare clic sul nodo **Gestione rapporti di archiviazione**.

2. Fare clic con il pulsante destro del mouse su **Gestione rapporti di archiviazione**, quindi scegliere **Pianifica una nuova attività rapporto**. In alternativa, è possibile selezionare **Pianifica una nuova attività rapporto** nel riquadro **Azioni**. Verrà visualizzata la finestra di dialogo **Proprietà attività rapporto archiviazione**.

3. Per selezionare volumi o cartelle su cui si desidera generare rapporti:

   -   In **Ambito** fare clic su **Aggiungi**.
   -   Visualizzare il volume o la cartella in cui si desidera generare i rapporti, selezionarla, quindi fare clic su **OK** per aggiungere il percorso all'elenco.
   -   Aggiungere tutti i volumi o tutte le cartelle che si desidera includere nei rapporti. Per rimuovere un volume o una cartella, fare clic sul percorso, quindi fare clic su **Rimuovi**.

4. Per specificare i rapporti da generare:

   -  In **Dati rapporto** selezionare tutti i rapporti che si desidera includere. Per impostazione predefinita, tutti i rapporti vengono generati per un'attività rapporto pianificata.

   Per modificare i parametri di un rapporto:

   -   Fare clic sull'etichetta del rapporto, quindi fare clic su **Modifica parametri**.
   -   Nella finestra di dialogo **Parametri rapporto** modificare i parametri in base alle esigenze e fare clic su **OK**.

   -   Per visualizzare un elenco di parametri per tutti i rapporti selezionati, fare clic su **Visualizza rapporti selezionati**. e quindi fare clic su **Chiudi**.

5. Per specificare i formati in cui salvare i rapporti:

   -  In **Formati rapporto** selezionare uno o più formati per i rapporti pianificati. Per impostazione predefinita, i rapporti vengono generati in HTML dinamico (DHTML). È possibile selezionare anche HTML, XML, CSV e formati di testo. I rapporti vengono salvati nel percorso predefinito dei rapporti pianificati.

6. Per recapitare copie dei rapporti agli amministratori tramite posta elettronica:

   - Nella scheda **Recapito** selezionare la casella di controllo **Invia rapporti agli amministratori seguenti**, quindi immettere i nomi degli account amministrativi a cui verranno inviati i rapporti. 
   - Usare il formato <em>account@domain</em> e separare gli account con un punto e virgola.

7. Per pianificare i rapporti:

   Nella scheda **Pianificazione** fare clic su **Crea pianificazione**, quindi, nella finestra di dialogo **Pianificazione**, fare clic su **Nuovo**. Verrà visualizzata una pianificazione predefinita impostata per ogni giorno alle 09:00, ma è possibile modificarla.

   -   Per specificare una frequenza di generazione dei rapporti, selezionare un intervallo dall'elenco a discesa **Attività pianificazione**.
       È possibile pianificare rapporti giornalmente, settimanalmente o mensilmente oppure generarli una sola volta. I rapporti possono essere generati anche all'avvio del sistema o al momento dell'accesso oppure dopo che il computer è rimasto inattivo per un periodo di tempo specificato.
   -   Per fornire informazioni di pianificazione aggiuntive per l'intervallo selezionato, modificare o impostare i valori delle opzioni **Attività pianificazione**.
       Queste opzioni variano in base all'intervallo scelto. Per un rapporto settimanale, ad esempio, è possibile specificare il numero di settimane tra i rapporti e i giorni della settimana in cui generare i rapporti.
   -   Per specificare l'ora del giorno in cui si desidera generare il rapporto, digitare o selezionare il valore nella casella **Ora di inizio**.
   -   Per accedere a opzioni di pianificazione aggiuntive, incluse una data di inizio e una data di fine per l'attività, fare clic su **Avanzate**.
   -   Per salvare la pianificazione, fare clic su **OK**.
   -  Per creare una pianificazione aggiuntiva per un'attività o per modificare una pianificazione esistente, fare clic su **Modifica pianificazione** nella scheda **Pianificazione**.

8. Per salvare l'attività rapporto, fare clic su **OK**.

L'attività rapporto viene aggiunta al nodo **Gestione rapporti di archiviazione**. Le attività vengono identificate dai rapporti che devono essere generati, dallo spazio dei nomi da includere nel rapporto e dalla pianificazione dei rapporti.

Inoltre, è possibile visualizzare lo stato corrente del rapporto, indipendentemente dal fatto che sia in esecuzione o meno, l'ora e il risultato dell'ultima esecuzione e l'ora della successiva esecuzione pianificata.

## <a name="see-also"></a>Vedere anche

-   [Gestione rapporti di archiviazione](storage-reports-management.md)
-   [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)


