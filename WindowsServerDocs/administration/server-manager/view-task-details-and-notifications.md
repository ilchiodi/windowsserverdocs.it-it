---
title: Visualizzare notifiche e dettagli sulle attività
description: Server Manager
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: 95117407-2dd3-4f9a-841f-4331be3544c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0047a9f2d4b6b66cec85b2746b1975af2ced3316
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851454"
---
# <a name="view-task-details-and-notifications"></a>Visualizzare notifiche e dettagli sulle attività

>Si applica a: Windows Server 2016

In Server Manager in Windows Server 2012 R2 o Windows Server 2012, quando si eseguono attività di gestione, ad esempio l'aggiunta di ruoli e funzionalità, avvio dei servizi, aggiornare i dati visualizzati nella console di Server Manager o la creazione di un gruppo personalizzato di server, viene visualizzata una notifica nel **notifiche** area dell'intestazione della console Server Manager. Le notifiche e la finestra di dialogo **Dettagli attività** che è possibile aprire dal menu **notifiche** facendo clic sull'icona del flag, visualizzano lo stato delle attività o delle richieste dell'utente, indicano se un'attività ha avuto esito negativo e consentono di risolvere i problemi puntando a messaggi di errore dettagliati sugli errori delle attività.

## <a name="the-notifications-area"></a>Area Notifiche
Il **notifiche** area della barra dei menu di Server Manager, contrassegnato da un'icona del flag, vengono visualizzati i risultati delle attività che si avvia in Server Manager. Le notifiche che indicano se le attività che è stato avviato in Server Manager erano esito positivo o negativo. Quando sono disponibili notifiche da visualizzare, il relativo numero viene indicato accanto all'icona di contrassegno. Se un'attività non è riuscita, è stata completata solo parzialmente (ad esempio se non è stato possibile completarla su tutti i server remoti sui quali si desiderava eseguirla) o è stata completata con avvisi, il contrassegno **Notifiche** diventa rosso. Le attività per le quali vengono visualizzate notifiche includono:

-   Aggiornare manualmente i dati visualizzati in Server Manager (vengono visualizzate notifiche per gli aggiornamenti automatici solo se gli aggiornamenti non riescono)

-   avvio e arresto di servizi

-   Installare o disinstallare ruoli, servizi ruolo e funzionalità

-   avvio di un'analisi di Best Practices Analyzer (BPA)

-   Aggiunta di server remoti da gestire (vengono visualizzate notifiche per gli errori di contatto o di aggiornamento dei dati visualizzati per i server remoti)

Le voci del menu **Notifiche** mostrano una barra di stato, una breve descrizione dell'attività, il nome del server di destinazione per l'attività (o uno dei server di destinazione se ne sono stati selezionati diversi), un collegamento a un controllo correlato o una finestra di dialogo, se applicabile, e un menu **Attività** . Il menu **Attività** mostra comandi applicabili alla notifica attiva, ovvero la notifica sulla quale è posizionato il cursore del mouse. Se, ad esempio, si arresta un servizio, è possibile fare clic su **Riavvia** nel menu **Attività** della notifica per riavviarlo.

Le notifiche sono particolarmente utili per l'installazione o disinstallazione di ruoli, servizi ruolo e funzionalità. Se, ad esempio, si avvia l'installazione di una funzionalità su un server remoto, è possibile chiudere l'aggiunta guidata ruoli e funzionalità mentre l'installazione è ancora in corso, ma l'attività attiva rimane nell'elenco **notifiche** . L'elemento **notifiche** Mostra un indicatore di stato per l'installazione e consente di riaprire l'aggiunta guidata ruoli e funzionalità, se necessario, facendo clic su **Aggiunta guidata ruoli e funzionalità**. Le voci nell'elenco avvisano l'utente in caso di errore di un'installazione o se sono necessari ulteriori passaggi di configurazione per terminare la distribuzione della funzionalità.

Le notifiche svolgono anche una parte importante della risoluzione dei problemi relativi ad attività o processi in Server Manager. Per ulteriori informazioni su come utilizzare i messaggi nell'area **notifiche** e nella finestra di dialogo **Dettagli attività** per risolvere i problemi relativi ad attività o processi non riusciti, vedere la [Guida alla risoluzione dei problemi di Server Manager](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).

Per eliminare una notifica che non si desidera più visualizzare dall'elenco **notifiche** , posizionare il cursore del mouse sulla notifica e quindi fare clic su **Rimuovi attività** (**X**).

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>Visualizzazione e risoluzione dei problemi relativi alle attività mediante dettagli attività
Il comando **Dettagli attività** nella parte inferiore del menu **notifiche** apre la finestra di dialogo **Dettagli attività** , che fornisce descrizioni complete degli eventi relativi alle attività (avvio, arresto, avvisi, operazioni riuscite o errori). Analogamente ad altri controlli elenco in Server Manager, ad esempio **gli eventi**, i **Servizi**e i riquadri **Best Practices Analyzer** , è possibile filtrare e creare query da eseguire sulle attività visualizzate nella finestra di dialogo **Dettagli attività** . Per ulteriori informazioni su come filtrare e creare query sui controlli elenco, vedere [filtrare, ordinare ed eseguire query sui dati nei riquadri Server Manager](filter-sort-and-query-data-in-server-manager-tiles.md). Nel riquadro superiore è possibile esaminare le notifiche così come sono state visualizzate nel menu **notifiche** e visualizzare il numero di notifiche generate sulla stessa attività. selezionando una notifica nel riquadro superiore vengono visualizzati i dettagli completi della notifica nel riquadro inferiore.

Il riquadro inferiore è particolarmente utile per la risoluzione dei problemi relativi alle attività non riuscite. Se Server Manager non è in grado di connettersi o recuperare dati per un server membro del pool di server, le voci di questo riquadro contengono spesso messaggi dettagliati, incluso il testo completo di gestione remota Windows (WinRM) sottostante, rete o problemi di sicurezza che impediscono la comunicazione Server Manager con un server di destinazione.

## <a name="see-also"></a>Vedi anche
[Filtrare, ordinare ed eseguire query sui dati nei riquadri di Server Manager](filter-sort-and-query-data-in-server-manager-tiles.md)
[Server Manager Guida alla risoluzione dei problemi](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
