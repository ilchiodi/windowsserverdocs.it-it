---
title: Creare un modello di screening dei file
description: Questo articolo descrive come creare un modello di screening dei file
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b06597bce0b88ed5a2e98ad45d0cbc355d1b13fc
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-file-screen-template"></a>Creare un modello di screening dei file

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Un *modello di screening dei file* definisce un set di gruppi di file di cui eseguire lo screening, il tipo di screening da eseguire (attivo o passivo) e, facoltativamente, un set di notifiche che verranno generate automaticamente quando un utente salva o tenta di salvare un file non autorizzato.

Gestione risorse file Server può inviare messaggi di posta elettronica agli amministratori o utenti specifici, registrare un evento, eseguire un comando o uno script o generare rapporti. È possibile configurare più di un tipo di notifica per un evento di screening dei file.

Se gli screening dei file vengono creati esclusivamente in base a modelli, è possibile gestirli in modo centralizzato aggiornando i modelli anziché i singoli screening dei file. Questa funzionalità semplifica l'implementazione delle modifiche ai criteri di archiviazione offrendo un'unica posizione centrale in cui eseguire tutti gli aggiornamenti.

> [!Important]
> Per inviare notifiche tramite posta elettronica e per configurare i report di archiviazione con i parametri appropriati per l'ambiente server, è innanzitutto necessario impostare le opzioni di Gestione risorse file server generali. Per ulteriori informazioni, vedere [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md).

## <a name="to-create-a-file-screen-template"></a>Per creare un modello di screening dei file

1.  In **Gestione screening dei File**, fare clic sul nodo **Modelli per lo screening dei file**.

2.  Fare clic con il pulsante destro su **Modelli per lo screening dei file** e fare clic su **Crea modello per lo screening dei file** (o selezionare **Crea modello per lo screening dei file** dal riquadro **Azioni**). Verrà visualizzata la finestra di dialogo **Crea modello per lo screening dei file**.

3.  Se si desidera copiare le proprietà di un modello esistente da utilizzare come base per il nuovo modello, selezionare un modello nell'elenco a discesa **Copia le proprietà dal modello quota**, quindi fare clic su **Copia**.

    Indipendentemente dal fatto che si sia scelto di utilizzare le proprietà di un modello esistente o si crei un nuovo modello, modificare o impostare i valori seguenti nella scheda **Impostazioni**:

4.  Nella casella di testo **Nome modello** immettere un nome per il nuovo modello.

5.  In **Tipo di screening**, fare clic sull'opzione **Screening attivo** o **Screening passivo**. (Lo screening attivo impedisce il salvataggio di file che sono membri dei gruppi di file bloccati e generano notifiche quando gli utenti tentano di salvare i file non autorizzati. Lo screening passivo invia le notifiche configurate, ma impedisce il salvataggio dei file).

6.  Per specificare su quali gruppi di file eseguire lo screening:

    In **Gruppi di file**, selezionare ogni gruppo di file che si desidera includere. (Per selezionare la casella di controllo per il gruppo di file, fare doppio clic sull'etichetta del gruppo di file).

    Se si desidera visualizzare i tipi di file che un gruppo di file include ed esclude, fare clic sull'etichetta del gruppo di file e fare clic su **Modifica**. Per creare un nuovo gruppo di file, fare clic su **Crea**.

    Inoltre, è possibile configurare Gestione risorse file server per generare una o più notifiche impostando le opzioni seguenti nelle schede **Messaggio posta elettronica**, **Registro eventi**, **Comando** e **Report**.

7.  Per configurare le notifiche tramite posta elettronica:

    Nella scheda **Messaggio posta elettronica** specificare le opzioni seguenti:

    -   Per inviare notifiche agli amministratori quando un utente o applicazione tenta di salvare un file non autorizzato, selezionare la casella di controllo **Invia posta elettronica agli amministratori seguenti** e quindi immettere i nomi degli account amministrativi che riceveranno le notifiche. Usare il formato *account*@*domain* e separare più account con un punto e virgola.
    -   Per inviare messaggi e-mail all'utente che ha tentato di salvare il file, selezionare la casella di controllo **Invia posta elettronica all'utente che ha cercato di salvare un file non autorizzato**.
    -   Per configurare il messaggio, modificare la riga dell'oggetto e il corpo del messaggio predefiniti forniti. Il testo tra parentesi quadre inserisce informazioni sulle variabili in relazione all'evento di screening dei file che ha generato la notifica. Ad esempio, la variabile \[**Source Io Owner**\] inserisce il nome dell'utente che ha tentato di salvare un file non autorizzato. Per inserire altre variabili nel testo, fare clic su **Inserisci variabile**.
    -   Per configurare le intestazioni aggiuntive (inclusi i campi Da, Cc, Ccn e Rispondi a), fare clic su **Intestazioni messaggio aggiuntive**.

8.  Per registrare un errore nel registro eventi quando un utente tenta di salvare un file non autorizzato:

    Nella scheda **Registro eventi**, selezionare la casella di controllo **Invia avviso al registro eventi** e quindi modificare la voce di registro predefinita.

9.  Per eseguire un comando o script quando un utente tenta di salvare un file non autorizzato:

    Nella scheda **Comando**, selezionare la casella di controllo **Esegui il comando o lo script seguente**. Quindi, digitare il comando o fare clic su **Sfoglia** per cercare il percorso in cui è archiviato lo script. È inoltre possibile immettere argomenti comando, selezionare una directory di lavoro per il comando o script o modificare l'impostazione di sicurezza del comando.

10. Per generare uno o più rapporti archiviazione quando un utente tenta di salvare un file non autorizzato:

    Nella scheda **Rapporto**, selezionare la casella di controllo **Genera rapporti** e quindi selezionare i rapporti da generare. (Puoi scegliere uno o più destinatari di posta elettronica amministrativi per il rapporto o inviare il rapporto tramite posta elettronica all'utente che ha tentato di salvare il file).

    Il rapporto verrà salvato nel percorso predefinito dei rapporti di operazioni non consentite, che può essere modificato nella finestra di dialogo **Opzioni di Gestione risorse file server**.

11. Dopo aver selezionato tutte le proprietà del modello di file che si desidera utilizzare, fare clic su **OK** per salvare il modello.

## <a name="see-also"></a>Vedere anche

-   [Gestione screening dei file](file-screening-management.md)
-   [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)
-   [Modificare le proprietà dei modelli di screening dei file](edit-file-screen-template-properties.md)

