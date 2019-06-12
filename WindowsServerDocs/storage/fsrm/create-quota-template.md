---
title: Creare un modello quota
description: Questo articolo descrive come creare un modello quota per definire un limite di spazio di archiviazione
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 236b5cb198a13441a087ad6dbfeef9a416e07e61
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445979"
---
# <a name="create-a-quota-template"></a>Creare un modello quota

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

In un *modello quota* vengono definiti il limite di spazio, il tipo di quota (rigida o flessibile) e facoltativamente un insieme di notifiche da generare automaticamente quando vengono raggiunti i limiti di soglia per l'utilizzo delle quote.

Se le quote vengono create esclusivamente in base a modelli, è possibile gestirle in modo centralizzato aggiornando i modelli anziché le singole quote. Questa funzionalità semplifica l'implementazione delle modifiche ai criteri di archiviazione offrendo un'unica posizione centrale in cui eseguire tutti gli aggiornamenti.

## <a name="to-create-a-quota-template"></a>Per creare un modello quota

1.  In **Gestione quote** fare clic sul nodo **Modelli quota**.

2.  Fare clic con il pulsante destro del mouse su **Modelli quote** e scegliere **Crea modello quota** oppure selezionare **Crea modello quota** nel riquadro **Azioni**. Verrà visualizzata la finestra di dialogo **Crea modello quota**.

3.  Se si desidera copiare le proprietà di un modello esistente da utilizzare come base per il nuovo modello, selezionare un modello nell'elenco a discesa **Copia le proprietà dal modello quota**, Quindi, fare clic su **Copia**.

    Indipendentemente dal fatto che si sia scelto di utilizzare le proprietà di un modello esistente o si crei un nuovo modello, modificare o impostare i valori seguenti nella scheda **Impostazioni**:

4.  Nella casella di testo **Nome modello** immettere un nome per il nuovo modello.

5.  Nella casella di testo **Etichetta** immettere un'etichetta descrittiva facoltativa che sarà visualizzata accanto a tutte le quote derivate dal modello.

6.  In **Limite spazio** eseguire le operazioni seguenti:

    -   Nella casella di testo **Limite** immettere un numero e scegliere un'unità, ovvero KB, MB, GB o TB, per specificare il limite di spazio per la quota.
    -   Fare clic sull'opzione **Quota rigida** o **Quota flessibile**. Le quote rigide impediscono il salvataggio di file da parte degli utenti dopo che il limite di spazio è stato raggiunto e generano notifiche quando la quantità di dati raggiunge le singole soglie configurate. Una quota flessibile invece non applica il limite di quota, ma genera tutte le notifiche configurate.

7.  Per il modello di quota è possibile configurare uno o più notifiche di soglia opzionali come descritto nella procedura seguente. Dopo aver selezionato tutte le proprietà che si desidera utilizzare per il modello quota, fare clic su **OK** per salvare il modello.

## <a name="setting-optional-notification-thresholds"></a>Impostazione delle soglie di notifica opzionali

Quando l'archiviazione in un volume o una cartella raggiunge una soglia definita dall'utente, Gestione risorse file server può inviare messaggi di posta elettronica agli amministratori o a utenti specifici, registrare un evento, eseguire un comando o script o generare rapporti. È possibile configurare più di un tipo di notifica per ogni soglia, ed è possibile definire più soglie per qualsiasi quota (o un modello quota). Per impostazione predefinita, non vengono generate notifiche.

Ad esempio, è possibile configurare le soglie per inviare un messaggio di posta elettronica all'amministratore e agli utenti che sarebbero interessanti a sapere quando una cartella raggiunge l'85% del limite di quota e quindi inviare un'altra notifica quando viene raggiunto il limite di quota. Inoltre, è consigliabile eseguire uno script che utilizza il comando **dirquota.exe** per aumentare il limite di quota automaticamente quando viene raggiunta una soglia.

> [!Important]
> Per inviare notifiche tramite posta elettronica e configurare i report di archiviazione con i parametri appropriati per l'ambiente server, è innanzitutto necessario impostare le opzioni di Gestione risorse file server generali. Per ulteriori informazioni, vedere [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)

**Per configurare le notifiche che Gestione risorse File Server consentono di generare a una soglia di quota**

1. Nella finestra di dialogo **Crea modello quota** in **Soglie notifiche**, fare clic su **Aggiungi**. Verrà visualizzata la finestra di dialogo **Aggiungi soglia**.

2. Per impostare una percentuale del limite di quota che genererà una notifica:

   Nella casella di testo **Genera notifiche quando l'utilizzo raggiunge (%)** , immettere una percentuale del limite di quota per la soglia di notifica. (La percentuale predefinita per la prima soglia di notifica è 85%).

3. Per configurare le notifiche tramite posta elettronica:

   Nella scheda **Messaggio posta elettronica** specificare le opzioni seguenti:

   - Per inviare notifiche agli amministratori quando viene raggiunta una soglia, selezionare la casella di controllo **Invia posta elettronica agli amministratori seguenti** e quindi immettere i nomi degli account amministrativi che riceveranno le notifiche. Usare il formato <em>account@domain</em> e separare gli account con un punto e virgola.
   - Per inviare messaggi di posta elettronica alla persona che ha salvato il file che ha raggiunto la soglia di quota, selezionare la casella di controllo **Invia messaggio di posta elettronica all'utente che ha superato la soglia**.
   - Per configurare il messaggio, modificare la riga dell'oggetto e il corpo del messaggio predefiniti forniti. Il testo tra parentesi quadre inserisce informazioni sulle variabili in relazione all'evento di quota che ha generato la notifica. Ad esempio, il **\[proprietario dell'origine Io\]** variabile inserisce il nome dell'utente che ha stato salvato il file che raggiunto la soglia di quota. Per inserire altre variabili nel testo, fare clic su **Inserisci variabile**.
   - Per configurare le intestazioni aggiuntive (inclusi i campi Da, Cc, Ccn e Rispondi a), fare clic su **Intestazioni messaggio aggiuntive**.

4. Per registrare un evento:

   Nella scheda **Registro eventi**, selezionare la casella di controllo **Invia avviso al registro eventi** e quindi modificare la voce di registro predefinita.

5. Per eseguire un comando o script:

   Nella scheda **Comando**, selezionare la casella di controllo **Esegui il comando o lo script seguente**. Quindi, digitare il comando o fare clic su **Sfoglia** per cercare il percorso in cui è archiviato lo script. È inoltre possibile immettere argomenti comando, selezionare una directory di lavoro per il comando o script o modificare l'impostazione di sicurezza del comando.

6. Per generare uno o più rapporti di archiviazione:

   Nella scheda **Rapporto**, selezionare la casella di controllo **Genera rapporti** e quindi selezionare i rapporti da generare. (Puoi scegliere uno o più destinatari di posta elettronica amministrativi per il rapporto o inviare il rapporto tramite posta elettronica all'utente che ha raggiunto la soglia).

   Il rapporto verrà salvato nel percorso predefinito dei rapporti di operazioni non consentite, che può essere modificato nella finestra di dialogo **Opzioni Gestione risorse file server**.

7. Fare clic su **OK** per salvare la soglia di notifica.

8. Ripetere questi passaggi se si desidera configurare soglie di notifica aggiuntive per il modello quota.

## <a name="see-also"></a>Vedere anche

-   [Gestione delle quote](quota-management.md)
-    [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)
-   [Modificare le proprietà dei modelli quota](edit-quota-template-properties.md)
-   [Strumenti da riga di comando](command-line-tools.md)


