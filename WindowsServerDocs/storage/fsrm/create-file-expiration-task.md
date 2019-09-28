---
title: Creare un'attività di scadenza dei file
description: Questo articolo descrive il processo di creazione di un'attività di gestione dei file prossimi alla scadenza
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0901c17203252414a37ccc5205a0946b8bef0d41
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394234"
---
# <a name="create-a-file-expiration-task"></a>Creare un'attività di scadenza dei file

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

La procedura indicata di seguito illustra il processo di creazione di un'attività di gestione dei file in scadenza. Le attività di gestione dei file in scadenza consentono di spostare tutti i file corrispondenti a determinati criteri in una directory di scadenza specificata, in cui un amministratore può quindi eseguire il backup di questi file ed eliminarli.

Quando viene eseguita un'attività di gestione dei file in scadenza, viene creata una nuova directory nella directory dei file in scadenza, raggruppata in base al nome del server sul quale è stata eseguita l'attività.

Il nome della nuova directory è basato sul nome dell'attività di gestione dei file e sull'ora in cui è stata eseguita. Quando si trova un file scaduto, viene spostato nella nuova directory, preservando la struttura originale della directory.

## <a name="to-create-a-file-expiration-task"></a>Per creare un'attività di gestione dei file in scadenza

1. Fare clic sul nodo **Attività di gestione file**.

2. Fare clic con il pulsante destro del mouse su **Attività di gestione file**, quindi fare clic su **Crea attività di gestione file** (oppure fare clic su **Crea attività di gestione file** nel riquadro **Azioni**). Verrà visualizzata la finestra di dialogo **Crea attività di gestione file**.

3. Nella scheda **Generale** immettere le informazioni seguenti:

   -   **Nome**. Immettere un nome per la nuova attività.  

   -   **Descrizione**. Immettere una descrizione facoltativa per l'attività.  
    
   -   **Ambito**. Aggiungere le directory in cui questa attività dovrebbe essere eseguita utilizzando il pulsante **Aggiungi**. Se lo si desidera, le directory possono essere rimosse dall'elenco utilizzando il pulsante **Rimuovi**. L'attività di gestione dei file verrà applicata a tutte le cartelle e sottocartelle in questo elenco.

4. Nella scheda **Azione** immettere le informazioni seguenti:

   - **Tipo**. Selezionare **Scadenza file** nella casella di riepilogo a discesa.

   - **Directory file in scadenza**. Selezionare una directory in cui posizionare i file in scadenza.

     > [!Warning]
     > Non selezionare una directory nell'ambito dell'attività, in base a quanto definito nel passaggio precedente, altrimenti potrebbe verificarsi un ciclo iterativo con il rischio di instabilità del sistema e perdita di dati.

5. Facoltativamente, nella scheda **Notifica** fare clic su **Aggiungi** per inviare notifiche per posta elettronica, registrare un evento o eseguire un comando o uno script per un numero minimo di giorni specificato prima che l'attività esegua un'azione su un file.

   - Nella casella combinata **Giorni prima di eseguire l'attività per l'invio della notifica**, digitare o selezionare un valore per specificare il numero minimo di giorni prima di eseguire su un file l'attività di invio della notifica.

     > [!Note]
     > Le notifiche vengono inviate solo quando viene eseguita un'attività. Se il numero minimo di giorni specificato per inviare una notifica non coincide con un'attività pianificata, la notifica verrà inviata il giorno dell'attività pianificata precedente.

   - Per configurare le notifiche tramite posta elettronica, fare clic sulla scheda **Messaggio posta elettronica** e immettere le informazioni seguenti:

     - Per inviare notifiche agli amministratori quando viene raggiunta una soglia, selezionare la casella di controllo **Invia posta elettronica agli amministratori seguenti** e quindi immettere i nomi degli account amministrativi che riceveranno le notifiche. Usare il formato <em>account@domain</em> e separare gli account con un punto e virgola.  

     - Per inviare un messaggio di posta elettronica all'utente i cui file stanno per scadere, selezionare la casella di controllo **Invia messaggio di posta elettronica all'utente i cui file stanno per scadere**.

     - Per configurare il messaggio, modificare la riga dell'oggetto e il corpo del messaggio predefiniti forniti. Il testo tra parentesi quadre inserisce informazioni sulle variabili in relazione all'evento di quota che ha generato la notifica. Ad esempio, il **proprietario del file \[Source @ no__t-2** variabile inserisce il nome dell'utente il cui file sta per scadere. Per inserire altre variabili nel testo, fare clic su **Inserisci variabile**.

     - Per allegare un elenco dei file che stanno per scadere, fare clic su **Allega a elenco di file inviati per posta elettronica su cui verrà eseguita l'azione** e digitare o selezionare un valore per **Numero massimo di file nell'elenco**.

     - Per configurare le intestazioni aggiuntive (inclusi i campi Da, Cc, Ccn e Rispondi a), fare clic su **Intestazioni messaggio aggiuntive**.  

   - Per registrare un evento, fare clic sulla scheda **Registro eventi** e selezionare la casella di controllo **Invia avviso al registro eventi** e quindi modificare la voce di registro predefinita.  

   - Per eseguire un comando o uno script, fare clic sulla scheda **Comando** e selezionare la casella di controllo **Esegui il comando o lo script seguente**. Quindi, digitare il comando o fare clic su **Sfoglia** per cercare il percorso in cui è archiviato lo script. È inoltre possibile immettere argomenti comando, selezionare una directory di lavoro per il comando o script o modificare l'impostazione di sicurezza del comando.

6. Facoltativamente, utilizzare la scheda **Rapporto** per generare uno o più registri o rapporti di archiviazione.

   - Per generare i registri, selezionare la casella di controllo **Generazione registro** e quindi selezionare uno o più registri disponibili.  

   - Per generare rapporti, selezionare la casella di controllo **Genera rapporto** e quindi selezionare uno o più formati di rapporto disponibili.  

   - Per creare rapporti di archiviazione o registri generati tramite posta elettronica, selezionare la casella di controllo **Invia rapporti ai seguenti amministratori** e digitare uno o più destinatari di posta elettronica amministrativi usando il formato <em>account@domain</em>. Separare più indirizzi con un punto e virgola.

     > [!Note]
     > Il rapporto verrà salvato nel percorso predefinito dei rapporti di operazioni non consentite, che può essere modificato nella finestra di dialogo **Opzioni Gestione risorse file server**.
        
7. Facoltativamente, utilizzare la scheda **Condizione** per eseguire questa attività solo su file che soddisfano un insieme definito di condizioni. Sono disponibili le impostazioni seguenti:

    -   **Condizioni proprietà**. Fare clic su **Aggiungi** per creare una nuova condizione basata sulla classificazione del file. Verrà aperta la finestra di dialogo **Condizione proprietà** che consente di selezionare una proprietà, un operatore da eseguire sulla proprietà e il valore con cui confrontare la proprietà. Dopo aver fatto clic su **OK**, è quindi possibile creare condizioni aggiuntive o modificare o rimuovere una condizione esistente.

    -   **N. di giorni dall'ultima modifica al file**. Fare clic sulla casella di controllo e quindi immettere un numero di giorni nella casella di selezione. L'attività di gestione dei file verrà applicata di conseguenza solo ai file che non sono stati modificati per un numero di giorni maggiore di quello specificato.

    -   **N. di giorni dall'ultimo accesso al file**. Fare clic sulla casella di controllo e quindi immettere un numero di giorni nella casella di selezione. Se il server è configurato per tenere traccia dei timestamp relativi all'ultimo accesso ai file, l'attività di gestione dei file verrà applicata di conseguenza solo ai file a cui non è stato effettuato l'accesso per un numero di giorni maggiore di quello specificato. Se il server non è configurato per tenere traccia delle ore di accesso, questa condizione non sarà valida.

    -   **N. di giorni dalla creazione del file**. Fare clic sulla casella di controllo e quindi immettere un numero di giorni nella casella di selezione. L'attività verrà applicata di conseguenza solo ai file che sono stati creati almeno dal numero di giorni specificato.  

    -   **Effettivo a partire dal giorno**. Impostare la data in cui questa attività di gestione dei file deve iniziare l'elaborazione dei file. Questa opzione è utile per posticipare l'attività fino a quando non sarà possibile inviare notifiche agli utenti o effettuare altre attività preliminari di preparazione.

8. Nella scheda **Pianificazione** fare clic su **Crea pianificazione**, quindi, nella finestra di dialogo **Pianificazione**, fare clic su **Nuovo**. Verrà visualizzata una pianificazione predefinita impostata per ogni giorno alle 09:00, ma è possibile modificarla. Al termine della configurazione della pianificazione, fare clic su **OK** e quindi fare clic nuovamente su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Gestione delle classificazioni](classification-management.md)
-   [Attività di gestione file](file-management-tasks.md)