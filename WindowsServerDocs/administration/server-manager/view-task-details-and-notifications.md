---
title: Visualizzare notifiche e dettagli sulle attività
description: Server Manager
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95117407-2dd3-4f9a-841f-4331be3544c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44fd23b917d08aad663c0d3fb8da4bebef47783e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831512"
---
# <a name="view-task-details-and-notifications"></a>Visualizzare notifiche e dettagli sulle attività

>Si applica a: Windows Server 2016

In Server Manager in Windows Server 2012 R2 o Windows Server 2012, quando si eseguono attività di gestione, ad esempio l'aggiunta di ruoli e funzionalità, avvio dei servizi, aggiornare i dati visualizzati nella console di Server Manager o la creazione di un gruppo personalizzato di server, viene visualizzata una notifica nel **notifiche** area dell'intestazione della console Server Manager. Le notifiche e il **dettagli di attività** finestra di dialogo che è possibile aprire dalle **notifiche** menu facendo clic sull'icona di contrassegno, visualizzano lo stato di attività definite dall'utente o richieste, descrive se un'attività non è riuscita e consentono di risoluzione dei problemi puntando a messaggi di errore dettagliati sugli errori di attività.

## <a name="the-notifications-area"></a>Area Notifiche
Il **notifiche** area della barra dei menu di Server Manager, contrassegnato da un'icona del flag, vengono visualizzati i risultati delle attività che si avvia in Server Manager. Le notifiche che indicano se le attività che è stato avviato in Server Manager erano esito positivo o negativo. Quando sono disponibili notifiche da visualizzare, il relativo numero viene indicato accanto all'icona di contrassegno. Se un'attività non è riuscita, è stata completata solo parzialmente (ad esempio se non è stato possibile completarla su tutti i server remoti sui quali si desiderava eseguirla) o è stata completata con avvisi, il contrassegno **Notifiche** diventa rosso. Le attività per le quali vengono visualizzate notifiche includono:

-   Aggiornare manualmente i dati visualizzati in Server Manager (vengono visualizzate notifiche per gli aggiornamenti automatici solo se gli aggiornamenti non riescono)

-   avviare o arrestare servizi

-   Installare o disinstallare ruoli, servizi ruolo e funzionalità

-   avviare un'analisi di Best Practices Analyzer (BPA)

-   aggiunta di server remoti da gestire (vengono visualizzate notifiche per gli errori al contatto o aggiornare i dati visualizzati per i server remoti)

Le voci del menu **Notifiche** mostrano una barra di stato, una breve descrizione dell'attività, il nome del server di destinazione per l'attività (o uno dei server di destinazione se ne sono stati selezionati diversi), un collegamento a un controllo correlato o una finestra di dialogo, se applicabile, e un menu **Attività** . Il menu **Attività** mostra comandi applicabili alla notifica attiva, ovvero la notifica sulla quale è posizionato il cursore del mouse. Se, ad esempio, si arresta un servizio, è possibile fare clic su **Riavvia** nel menu **Attività** della notifica per riavviarlo.

Le notifiche sono particolarmente utili per l'installazione o disinstallazione di ruoli, servizi ruolo e funzionalità. Ad esempio, se si avvia l'installazione di una funzionalità in un server remoto, è possibile chiudere l'aggiunta guidata ruoli e funzionalità durante l'installazione è ancora in corso, ma l'attività attiva dal **notifiche** elenco. Il **notifiche** elemento mostra un indicatore di stato per l'installazione e consente di riaprire l'aggiunta guidata ruoli e funzionalità, se necessario, facendo **Aggiunta guidata ruoli e funzionalità**. Le voci nell'elenco avvisano l'utente in caso di errore di un'installazione o se sono necessari ulteriori passaggi di configurazione per terminare la distribuzione della funzionalità.

Le notifiche hanno anche una parte importante nella risoluzione dei problemi con le attività o processi in Server Manager. Per altre informazioni su come usare i messaggi nel **notifiche** area e **dettagli attività** la finestra di dialogo per risolvere i problemi di attività non riuscita o processi, vedere il [Server Manager Guida alla risoluzione dei](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).

Per eliminare una notifica che non si desidera visualizzare dal non sarà più il **notifiche** elencare, posiziona il cursore del mouse sulla notifica e quindi fare clic su **rimuovere attività** (**X**).

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>Visualizzazione e la risoluzione dei problemi di attività mediante dettagli attività
Il **dettagli di attività** comando nella parte inferiore del **notifiche** apre il menu il **dettagli attività** nella finestra di dialogo contiene descrizioni complete degli eventi relativi alle attività (avvio, l'arresto, avvisi, operazioni riuscite o gli errori). Analogamente ad altri controlli di elenco in Server Manager, ad esempio **eventi**, **Services**, e **Best Practices Analyzer** riquadri, è possibile filtrare e creare query da eseguire sulle attività che vengono visualizzati nei **dettagli di attività** nella finestra di dialogo. (per altre informazioni sul filtro e creazione di query nei controlli elenco, vedere [filtro, ordinamento e query dei dati nei riquadri di Server Manager](filter-sort-and-query-data-in-server-manager-tiles.md).) Nel riquadro superiore, è possibile esaminare le notifiche come visualizzati nella finestra di **notifiche** menu e vedere quante notifiche sono state generate sulla stessa attività. selezione di una notifica nel riquadro superiore Visualizza i dettagli completi sulla notifica nel riquadro inferiore.

Il riquadro inferiore è particolarmente utile per la risoluzione dei problemi relativi alle attività non riuscite. Se Server Manager non è possibile connettersi o ottenere dati per un server che è un membro del pool di server, le voci di questo riquadro contengono spesso messaggi dettagliati, incluso il testo completo di sottostante gestione remota Windows (WinRM), rete o problemi di sicurezza che impedire a Server Manager di comunicare con un server di destinazione.

## <a name="see-also"></a>Vedere anche
[Filtrare, ordinare ed eseguire query sui dati nei riquadri di Server Manager](filter-sort-and-query-data-in-server-manager-tiles.md)
[Server Manager Troubleshooting Guide](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
