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
ms.openlocfilehash: 413cf51e5ceb1c4507b71fb77ee6005807a0ff13
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476176"
---
# <a name="troubleshooting-file-server-resource-manager"></a>Risoluzione dei problemi di Gestione risorse file server

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

In questa sezione sono elencati i problemi comuni che possono verificarsi quando si utilizza Gestione risorse file server.

> [!Note]
> Un buon metodo di risoluzione dei problemi consiste nell'individuare i registri eventi generati da Gestione risorse file server. Tutte le voci del registro eventi di Gestione risorse file server sono disponibili nel registro eventi dell'**applicazione** sotto l'origine **SRMSVC**

## <a name="i-am-not-receiving-e-mail-notifications"></a>Non viene ricevuta alcuna notifica tramite posta elettronica.

-   **Causa**: Le opzioni di posta elettronica non sono state configurate o sono state configurate in modo non corretto.

-   **Soluzione**: Nel **notifiche tramite posta elettronica** nella scheda il **opzioni di gestione risorse File Server** finestra di dialogo, verificare che il server SMTP e destinatari di posta elettronica predefiniti sono stati specificati e sono validi. Inviare un messaggio di posta elettronica di prova per verificare che le informazioni siano esatte e che il server SMTP utilizzato per inviare le notifiche funzioni correttamente. Per ulteriori informazioni, vedere [Configurare le notifiche tramite posta elettronica](configure-email-notifications.md).


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>Viene ricevuta una sola notifica tramite posta elettronica anche se l'evento che l'ha generata si è verificato più volte in una riga.

-   **Causa**: Quando un utente tenta più volte per salvare un file bloccato o salvare un file che supera una soglia di quota, presenti screening di notifiche di posta elettronica configurato per il file è o eventi di quota, solo un messaggio di posta elettronica viene inviato all'amministratore in un periodo di 60 minuti per  Per impostazione predefinita. Ciò consente di evitare che all'account di posta elettronica dell'amministratore venga inviato un numero eccessivo di messaggi ridondanti.

-   **Soluzione**: Nel **limiti di notifica** nella scheda il **opzioni di gestione risorse File Server** nella finestra di dialogo è possibile impostare un limite di tempo per ognuno dei tipi di notifica: posta elettronica, registro eventi, comandi e report. Ogni limite specifica il periodo di tempo che deve trascorrere prima che un'altra notifica dello stesso tipo venga generata per lo stesso problema. Per ulteriori informazioni, vedere [Configurare i limiti di notifica](configure-notification-limits.md).


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>Non è possibile eseguire i rapporti di archiviazione oppure non sono disponibili informazioni in merito all'origine dell'errore nel Registro eventi.

-   **Causa**: Il volume in cui vengono salvati i report potrebbe essere danneggiato.
-   **Soluzione**: Eseguire **chkdsk** sul volume e provare a generare nuovamente i report.

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>I rapporti screening dei file non contengono alcuna informazione.

-   **Causa**: La causa potrebbe essere uno o più delle seguenti operazioni:
    -   Il database di controllo non è configurato per registrare l'attività di screening dei file.
    -   Il database di controllo è vuoto, ovvero non è stata registrata alcuna attività di screening dei file.
    -   I parametri per il rapporto Screening dei file non selezionano dati dal database di controllo.
    
-   **Soluzione**: Nel **screening dei File** nella scheda il **opzioni di gestione risorse File Server** finestra di dialogo, verificare che il **attività nel database di controllo di screening dei file di Record** controllare casella è selezionata.
    -   Per ulteriori informazioni sulla registrazione di attività di screening dei file, vedere [Configurare lo screening dei file](configure-file-screen-audit.md).
    -   Per configurare i parametri predefiniti per il rapporto Screening dei file, vedere [Configurare i rapporti archiviazione](configure-storage-reports.md).
    -   Per modificare i parametri del rapporto per le attività rapporto pianificate o i rapporti su richiesta, vedere [Gestione rapporti archiviazione](storage-reports-management.md).

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>I valori "Utilizzata" e "Disponibile" relativi ad alcune quote create non corrispondono all'impostazione effettiva del valore "Limite".

-   **Causa**: È possibile una quota annidata, in cui la quota di una sottocartella deriva un limite più restrittivo la quota della cartella padre. Ad esempio, è possibile che un limite di quota di 100 megabyte (MB) sia applicato a una cartella padre e che una quota di 200 MB sia applicata alle singole sottocartelle. Se la cartella padre include un totale di 50 MB di dati archiviati (la somma dei dati archiviati nelle sue sottocartelle), ogni sottocartella avrà solo 50 MB di spazio disponibile.

-   **Soluzione**: Sotto **gestione delle quote**, fare clic su **quote**. Nel riquadro **Risultati** selezionare la voce di quota che si desidera risolvere. Nel riquadro **Azioni** fare clic su **Visualizza le quote che interessano la cartella**, quindi cercare le quote applicate alle cartelle padre. Questo consentirà di identificare le quote della cartella padre con un'impostazione del limite di archiviazione inferiore rispetto alla quota selezionata.

