---
title: Risoluzione dei problemi di Gestione risorse file server
description: In questo articolo viene descritto come risolvere problemi comuni quando si utilizza Gestione risorse file server
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 923710fac426f63d2c38d9b9a68c92427783abb1
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="troubleshooting-file-server-resource-manager"></a>Risoluzione dei problemi di Gestione risorse file server

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

In questa sezione sono elencati i problemi comuni che possono verificarsi quando si utilizza Gestione risorse file server.

> [!Note]
> Un buon metodo di risoluzione dei problemi consiste nell'individuare i registri eventi generati da Gestione risorse file server. Tutte le voci del registro eventi di Gestione risorse file server sono disponibili nel registro eventi dell'**applicazione** sotto l'origine **SRMSVC**

## <a name="i-am-not-receiving-e-mail-notifications"></a>Non viene ricevuta alcuna notifica tramite posta elettronica.

-   **Causa**: le opzioni di posta elettronica non sono state configurate o sono errate.

-   **Soluzione**: nella scheda **Notifiche per posta elettronica**, nella finestra di dialogo **Opzioni Gestione risorse file server**, verificare che il server SMTP e destinatari di posta elettronica predefiniti siano stati specificati e siano validi. Inviare un messaggio di posta elettronica di prova per verificare che le informazioni siano esatte e che il server SMTP utilizzato per inviare le notifiche funzioni correttamente. Per ulteriori informazioni, vedere [Configurare le notifiche tramite posta elettronica](configure-email-notifications.md).


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>Viene ricevuta una sola notifica tramite posta elettronica anche se l'evento che l'ha generata si è verificato più volte in una riga.

-   **Causa**: se un utente tenta più volte di salvare un file bloccato o che supera la soglia della quota ed è stata configurata una notifica di posta elettronica per tale evento di quota o screening dei file, per impostazione predefinita all'amministratore viene inviato un solo messaggio di posta elettronica in un periodo di tempo di 60 minuti. Ciò consente di evitare che all'account di posta elettronica dell'amministratore venga inviato un numero eccessivo di messaggi ridondanti.

-   **Soluzione**: nella scheda **Limiti notifica**, presente nella finestra di dialogo **Opzioni Gestione risorse file server**, è possibile impostare un limite di tempo per ogni tipo di notifica: messaggi di posta elettronica, registro eventi, comando e rapporto. Ogni limite specifica il periodo di tempo che deve trascorrere prima che un'altra notifica dello stesso tipo venga generata per lo stesso problema. Per ulteriori informazioni, vedere [Configurare i limiti di notifica](configure-notification-limits.md).


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>Non è possibile eseguire i rapporti di archiviazione oppure non sono disponibili informazioni in merito all'origine dell'errore nel Registro eventi.

-   **Causa**: il volume in cui vengono salvati i rapporti potrebbe essere danneggiato.
-   **Soluzione**: eseguire **chkdsk** sul volume e provare a generare di nuovo i rapporti.

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>I rapporti screening dei file non contengono alcuna informazione.

-   **Causa**: la causa o le cause potrebbero essere elencate tra quelle specificate di seguito:
    -   Il database di controllo non è configurato per registrare l'attività di screening dei file.
    -   Il database di controllo è vuoto, ovvero non è stata registrata alcuna attività di screening dei file.
    -   I parametri per il rapporto Screening dei file non selezionano dati dal database di controllo.
    
-   **Soluzione**: nella scheda **Screening dei file** della finestra di dialogo **Opzioni Gestione risorse file server** verificare che la casella di controllo **Registra l'attività di screening dei file nel database di controllo** sia selezionata.
    -   Per ulteriori informazioni sulla registrazione di attività di screening dei file, vedere [Configurare lo screening dei file](configure-file-screen-audit.md).
    -   Per configurare i parametri predefiniti per il rapporto Screening dei file, vedere [Configurare i rapporti archiviazione](configure-storage-reports.md).
    -   Per modificare i parametri del rapporto per le attività rapporto pianificate o i rapporti su richiesta, vedere [Gestione rapporti archiviazione](storage-reports-management.md).

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>I valori "Utilizzata" e "Disponibile" relativi ad alcune quote create non corrispondono all'impostazione effettiva del valore "Limite".

-   **Causa**: potrebbe essere presente una quota nidificata, in cui la quota di una sottocartella deriva un limite più restrittivo dalla quota della relativa cartella padre. Ad esempio, è possibile che un limite di quota di 100 megabyte (MB) sia applicato a una cartella padre e che una quota di 200 MB sia applicata alle singole sottocartelle. Se la cartella padre include un totale di 50 MB di dati archiviati (la somma dei dati archiviati nelle sue sottocartelle), ogni sottocartella avrà solo 50 MB di spazio disponibile.

-   **Soluzione**: in **Gestione quote** fare clic su **Quote**. Nel riquadro **Risultati** selezionare la voce di quota che si desidera risolvere. Nel riquadro **Azioni** fare clic su **Visualizza le quote che interessano la cartella**, quindi cercare le quote applicate alle cartelle padre. Questo consentirà di identificare le quote della cartella padre con un'impostazione del limite di archiviazione inferiore rispetto alla quota selezionata.

