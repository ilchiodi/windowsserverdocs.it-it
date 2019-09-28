---
title: Registrazione degli eventi
description: Registrazione eventi dall'interfaccia di amministrazione di Windows (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 012c2229fb29aa711d9887f28859e09bcf71c14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356873"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Usare la registrazione degli eventi nell'interfaccia di amministrazione di Windows per ottenere informazioni sulle attività di gestione e tenere traccia dell'utilizzo del gateway

>Si applica a: Windows Admin Center, Windows Admin Center Preview

L'interfaccia di amministrazione di Windows scrive i registri eventi per consentire all'utente di visualizzare le attività di gestione eseguite sui server nell'ambiente in uso, nonché di risolvere eventuali problemi di Windows Admin Center.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Ottenere informazioni sulle attività di gestione nell'ambiente tramite la registrazione delle azioni utente

L'interfaccia di amministrazione di Windows fornisce informazioni approfondite sulle attività di gestione eseguite sui server dell'ambiente mediante la registrazione di azioni nel canale degli eventi **Microsoft-ServerManagementExperience** nel registro eventi del server gestito, con eventid 4000 e SMEGateway di origine. L'interfaccia di amministrazione di Windows registra solo le azioni sul server gestito, in modo da non visualizzare gli eventi registrati se un utente accede a un server per scopi di sola lettura.

Gli eventi registrati includono le seguenti informazioni:

| Chiave           | Value                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nome dello script di PowerShell che è stato eseguito nel server, se l'azione ha eseguito uno script di PowerShell |
| CIM           | Chiamata CIM eseguita sul server, se l'azione ha eseguito una chiamata CIM                        |
| Modulo        | Strumento (o modulo) in cui è stata eseguita l'azione                                                     |
| Gateway       | Nome del computer gateway dell'interfaccia di amministrazione di Windows in cui è stata eseguita l'azione                     |
| UserOnGateway | Nome utente usato per accedere al gateway dell'interfaccia di amministrazione di Windows ed eseguire l'azione                    |
| UserOnTarget  | Nome utente utilizzato per accedere al server gestito di destinazione, se diverso da userOnGateway (ovvero l'utente a cui si accede utilizzando il server utilizzando le credenziali "Gestisci come") |
| Delega    | Booleano: se il server gestito di destinazione considera attendibile il gateway e le credenziali sono delegate dal computer client dell'utente             |
| GIRI          | Booleano: se l'utente ha eseguito l'accesso al server usando le credenziali per i [giri](https://technet.microsoft.com/mt227395.aspx)                          |
| File          | nome del file caricato, se l'azione era un caricamento di file                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>Informazioni sull'attività del centro di amministrazione di Windows con registrazione eventi

L'interfaccia di amministrazione di Windows registra l'attività del gateway sul canale dell'evento sul computer gateway per semplificare la risoluzione dei problemi e visualizzare le metriche sull'utilizzo. Questi eventi vengono registrati nel canale di eventi **Microsoft-ServerManagementExperience** .

[Altre informazioni sulla risoluzione dei problemi dell'interfaccia di amministrazione di Windows.](troubleshooting.md)
