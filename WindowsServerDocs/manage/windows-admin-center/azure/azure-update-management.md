---
title: Usa Windows Admin Center per gestire gli aggiornamenti del sistema operativo con gestione aggiornamenti Azure
description: Usa Windows Admin Center (Project Honolulu) per configurare la gestione degli aggiornamenti Azure per la gestione del sistema operativo aggiornato.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 5ba81968f8baa81176ad646fb2a97961ddc49fda
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296988"
---
# Usa Windows Admin Center per gestire gli aggiornamenti del sistema operativo con gestione aggiornamenti Azure

[Altre informazioni sull'integrazione di Azure con Windows Admin Center.](../plan/azure-integration-options.md)

Gestione degli aggiornamenti Azure è una soluzione in automazione di Azure che consente di gestire gli aggiornamenti e le patch per più computer da un'unica posizione, anziché in base al server. Con gestione aggiornamenti Azure, puoi rapidamente valutare lo stato della disponibilità di aggiornamenti, pianifica l'installazione degli aggiornamenti necessari ed esaminare i risultati di distribuzione per verificare gli aggiornamenti applicabili correttamente. È possibile che le macchine siano macchine virtuali di Azure, ospitato da altri provider cloud o locale. [Altre informazioni sulla gestione degli aggiornamenti di Azure.](https://docs.microsoft.com/azure/automation/automation-update-management)

Con Windows Admin Center, puoi facilmente configurare e usare Gestione degli aggiornamenti di Azure per mantenere aggiornati i server gestiti. Se si dispone già un'area di lavoro di Log Analitica nella tua sottoscrizione di Azure, Windows Admin Center verrà automaticamente configurare il server e creare le risorse necessarie di Azure in sottoscrizione e il percorso specificato. Se hai un'area di lavoro di Log Analitica esistente, Windows Admin Center può configurare automaticamente il server per l'uso degli aggiornamenti da Gestione degli aggiornamenti di Azure.  

Per iniziare, Vai allo strumento gli aggiornamenti in una connessione al server e selezionare "Configurare ora" e fornire le preferenze per le risorse di Azure correlate. 

Dopo aver configurato il server per essere gestito da Gestione degli aggiornamenti di Azure, è possibile accedere gestione degli aggiornamenti di Azure tramite il collegamento ipertestuale fornito nello strumento gli aggiornamenti. 

[Scopri come smettere di usare Azure Update Management per aggiornare il server.](azure-monitor.md#disabling-monitoring)

Tieni presente che è necessario [registrare il gateway Windows Admin Center con Azure](..\configure\azure-integration.md) prima di configurare la gestione degli aggiornamenti Azure.

