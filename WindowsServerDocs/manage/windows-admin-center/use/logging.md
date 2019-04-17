---
title: Registrazione degli eventi
description: Registrazione di eventi dall'interfaccia di amministrazione di Windows (Honolulu progetto)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2074342"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Utilizzare l'evento registrazione nell'interfaccia di amministrazione di Windows per ottenere informazioni relative alle attività di gestione e tenere traccia dell'utilizzo di gateway

>Si applica a: Windows Admin Center, anteprima di interfaccia di amministrazione di Windows

Interfaccia di amministrazione di Windows scrive i registri eventi per poter visualizzare le attività di gestione viene eseguite nei server nell'ambiente in uso, nonché per risolvere eventuali problemi di interfaccia di amministrazione di Windows.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Ottenere informazioni relative alle attività di gestione dell'ambiente e registrazione azione utente

Interfaccia di amministrazione di Windows vengono fornite informazioni sulle attività di gestione eseguite nei server nell'ambiente in uso da operazioni di registrazione per il canale di eventi **Microsoft ServerManagementExperience** nel registro eventi del server gestito con EventID 4000 e SMEGateway di origine. Interfaccia di amministrazione di Windows registra solo operazioni eseguite in server gestiti in modo che non verranno visualizzate tutti gli eventi registrati se un utente accede a un server per scopi di sola lettura.

Gli eventi registrati includono le informazioni seguenti:

| Chiave           | Valore                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nome dello script PowerShell eseguiti nel server, se l'azione è stato eseguito uno script di PowerShell |
| CIM           | Chiamata CIM eseguiti nel server, se l'azione eseguita una chiamata CIM                        |
| Modulo        | Strumento (o modulo) in cui è stata eseguita l'azione                                                     |
| Gateway       | Nome del computer gateway di interfaccia di amministrazione di Windows in cui è stata eseguita l'azione                     |
| UserOnGateway | Nome utente utilizzato per accedere al gateway di interfaccia di amministrazione di Windows ed eseguire l'azione                    |
| UserOnTarget  | Nome utente utilizzato per accedere al server gestita di destinazione, se diversa dal userOnGateway (vale a dire l'utente a cui si accede tramite il server utilizzando le credenziali "Gestire come") |
| Delega    | Valore booleano: se la destinazione gestiti server considera attendibile il gateway e le credenziali delegate dal computer client dell'utente             |
| GIRI          | Valore booleano: se l'utente accede al server utilizzando le credenziali [giri](https://technet.microsoft.com/mt227395.aspx)                          |
| File          | nome del file caricato, se l'operazione ha avuto un caricamento di file                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>Informazioni sulle attività di interfaccia di amministrazione di Windows con la registrazione degli eventi

Interfaccia di amministrazione di Windows i registri attività gateway per il canale di eventi nel computer gateway per la risoluzione dei problemi e visualizzare le metriche di utilizzo. Questi eventi vengono registrati per il canale di eventi **Microsoft ServerManagementExperience** .

[Ulteriori informazioni sulla risoluzione dei problemi di interfaccia di amministrazione di Windows.](troubleshooting.md)
