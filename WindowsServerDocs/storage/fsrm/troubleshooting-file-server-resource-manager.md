---
title: Risoluzione dei problemi di Gestione risorse file server
description: In questo articolo viene descritto come risolvere problemi comuni quando si utilizza Gestione risorse file server
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0f30bcfd07f28ecd3fa1618e4f5d89b7a0b4d14f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403084"
---
# <a name="troubleshooting-file-server-resource-manager"></a>Risoluzione dei problemi di Gestione risorse file server

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

In questa sezione sono elencati i problemi comuni che possono verificarsi quando si utilizza Gestione risorse file server.

> [!Note]
> Un buon metodo di risoluzione dei problemi consiste nell'individuare i registri eventi generati da Gestione risorse file server. Tutte le voci del registro eventi di Gestione risorse file server sono disponibili nel registro eventi dell'**applicazione** sotto l'origine **SRMSVC**

## <a name="i-am-not-receiving-e-mail-notifications"></a>Non viene ricevuta alcuna notifica tramite posta elettronica.

-   **Causa**: Le opzioni di posta elettronica non sono state configurate o non sono state configurate correttamente.

-   **Soluzione**: Nella scheda **notifiche posta elettronica** , nella finestra di dialogo **opzioni file server Gestione risorse** , verificare che il server SMTP e i destinatari di posta elettronica predefiniti siano stati specificati e siano validi. Inviare un messaggio di posta elettronica di prova per verificare che le informazioni siano esatte e che il server SMTP utilizzato per inviare le notifiche funzioni correttamente. Per ulteriori informazioni, vedere [Configurare le notifiche tramite posta elettronica](configure-email-notifications.md).


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>Viene ricevuta una sola notifica tramite posta elettronica anche se l'evento che l'ha generata si è verificato più volte in una riga.

-   **Causa**: Quando un utente tenta più volte di salvare un file bloccato o di salvare un file che supera una soglia di quota e viene configurata una notifica tramite posta elettronica per l'evento di selezione o di quota dei file, viene inviato un solo messaggio di posta elettronica all'amministratore per un periodo di 60 minuti  predefinita. Ciò consente di evitare che all'account di posta elettronica dell'amministratore venga inviato un numero eccessivo di messaggi ridondanti.

-   **Soluzione**: Nella scheda **limiti di notifica** , nella finestra di dialogo **opzioni file server Gestione risorse** , è possibile impostare un limite di tempo per ogni tipo di notifica, ovvero posta elettronica, registro eventi, comando e report. Ogni limite specifica il periodo di tempo che deve trascorrere prima che un'altra notifica dello stesso tipo venga generata per lo stesso problema. Per ulteriori informazioni, vedere [Configurare i limiti di notifica](configure-notification-limits.md).


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>Non è possibile eseguire i rapporti di archiviazione oppure non sono disponibili informazioni in merito all'origine dell'errore nel Registro eventi.

-   **Causa**: Il volume in cui vengono salvati i report potrebbe essere danneggiato.
-   **Soluzione**: Eseguire **chkdsk** sul volume e riprovare a generare i report.

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>I rapporti screening dei file non contengono alcuna informazione.

-   **Causa**: Una o più delle seguenti possibili cause:
    -   Il database di controllo non è configurato per registrare l'attività di screening dei file.
    -   Il database di controllo è vuoto, ovvero non è stata registrata alcuna attività di screening dei file.
    -   I parametri per il rapporto Screening dei file non selezionano dati dal database di controllo.
    
-   **Soluzione**: Nella scheda **controllo della schermata file** , nella finestra di dialogo **opzioni file server Gestione risorse** , verificare che la casella **di controllo attività di screening dei file di record nel database di controllo** sia selezionata.
    -   Per ulteriori informazioni sulla registrazione di attività di screening dei file, vedere [Configurare lo screening dei file](configure-file-screen-audit.md).
    -   Per configurare i parametri predefiniti per il rapporto Screening dei file, vedere [Configurare i rapporti archiviazione](configure-storage-reports.md).
    -   Per modificare i parametri del rapporto per le attività rapporto pianificate o i rapporti su richiesta, vedere [Gestione rapporti archiviazione](storage-reports-management.md).

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>I valori "Utilizzata" e "Disponibile" relativi ad alcune quote create non corrispondono all'impostazione effettiva del valore "Limite".

-   **Causa**: È possibile che si disponga di una quota nidificata, in cui la quota di una sottocartella deriva un limite più restrittivo dalla quota della cartella padre. Ad esempio, è possibile che un limite di quota di 100 megabyte (MB) sia applicato a una cartella padre e che una quota di 200 MB sia applicata alle singole sottocartelle. Se la cartella padre include un totale di 50 MB di dati archiviati (la somma dei dati archiviati nelle sue sottocartelle), ogni sottocartella avrà solo 50 MB di spazio disponibile.

-   **Soluzione**: In **Gestione quote**fare clic su **quote**. Nel riquadro **Risultati** selezionare la voce di quota che si desidera risolvere. Nel riquadro **Azioni** fare clic su **Visualizza le quote che interessano la cartella**, quindi cercare le quote applicate alle cartelle padre. Questo consentirà di identificare le quote della cartella padre con un'impostazione del limite di archiviazione inferiore rispetto alla quota selezionata.

