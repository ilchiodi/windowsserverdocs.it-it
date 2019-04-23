---
title: Registrazione degli eventi
description: Registrazione degli eventi di Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849302"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Usare la registrazione eventi di Windows Admin Center per ottenere informazioni approfondite sulle attività di gestione e tenere traccia dell'utilizzo di gateway

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Windows Admin Center scrive i log di eventi che consente di visualizzare le attività di gestione in esecuzione nei server nell'ambiente in uso, nonché per risolvere i problemi di Windows Admin Center.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Ottenere informazioni approfondite le attività di gestione dell'ambiente tramite la registrazione azioni utente

Windows Admin Center fornisce informazioni dettagliate sull'attività di gestione eseguite nei server nell'ambiente in uso dalle azioni di registrazione per il **Microsoft-ServerManagementExperience** canale degli eventi nel registro eventi di gestito server con 4000 EventID e SMEGateway di origine. Windows Admin Center registra solo le azioni nel server gestito, quindi non saranno eventi registrati se un utente accede a un server per scopi di sola lettura.

Gli eventi registrati includono le informazioni seguenti:

| Chiave           | Value                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nome dello script di PowerShell che è stato eseguito nel server, se l'azione è stato eseguito uno script di PowerShell |
| CIM           | Chiamata CIM che è stato eseguito nel server, se l'azione è stata eseguita una chiamata CIM                        |
| Modulo        | Lo strumento (o un modulo) in cui è stata eseguita l'azione                                                     |
| Gateway       | Nome del computer del gateway Windows Admin Center in cui è stata eseguita l'azione                     |
| UserOnGateway | Nome utente usato per accedere al gateway di Windows Admin Center e l'esecuzione dell'azione                    |
| UserOnTarget  | Nome utente usato per accedere al server di destinazione gestiti, se diverse dalle userOnGateway (vale a dire l'utente accede usando il server usando credenziali "Gestisci come") |
| Delega    | Booleano: se la destinazione gestito considerato attendibile dal server gateway e le credenziali vengono delegate da computer client dell'utente             |
| LAPS          | Booleano: se l'utente di accedere al server usando [LAPS](https://technet.microsoft.com/mt227395.aspx) credenziali                          |
| File          | nome del file caricato, se l'azione è il caricamento di un file                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>Informazioni sulle attività di Windows Admin Center con la registrazione degli eventi

Windows Admin Center registra l'attività di gateway per il canale dell'evento nel computer gateway che consentono di risolvere i problemi e visualizzare le metriche sull'utilizzo. Questi eventi vengono registrati per il **Microsoft-ServerManagementExperience** canale dell'evento.

[Altre informazioni sulla risoluzione dei problemi Windows Admin Center.](troubleshooting.md)
