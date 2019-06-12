---
title: Usare Windows Admin Center per gestire gli aggiornamenti del sistema operativo con gestione aggiornamenti di Azure
description: Usare Windows Admin Center (progetto Honolulu) per configurare Gestione aggiornamenti di Azure per la gestione del sistema operativo aggiornato.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 79b18e9963fba0993a7f34b1409edba6abfd48f0
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452537"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>Usare Windows Admin Center per gestire gli aggiornamenti del sistema operativo con gestione aggiornamenti di Azure

[Altre informazioni sull'integrazione di Azure con Windows Admin Center.](../plan/azure-integration-options.md)

Gestione degli aggiornamenti di Azure è una soluzione di automazione di Azure che consente di gestire gli aggiornamenti e patch per più computer da un'unica posizione, anziché su una base per server. Con gestione aggiornamenti di Azure, si può rapidamente valutare lo stato degli aggiornamenti disponibili, pianificare l'installazione degli aggiornamenti necessari e rivedere i risultati della distribuzione per verificare gli aggiornamenti applicabili correttamente. Ciò è possibile che le macchine siano macchine virtuali di Azure, ospitato da altri provider di servizi cloud o in locale. [Altre informazioni sulla gestione degli aggiornamenti di Azure.](https://docs.microsoft.com/azure/automation/automation-update-management)

Con Windows Admin Center, è possibile configurare facilmente e usare Gestione aggiornamenti di Azure per mantenere aggiornati i server gestiti. Se si ha già un'area di lavoro di Log Analitica nella sottoscrizione di Azure, Windows Admin Center verranno automaticamente configurare il server e creare le risorse di Azure necessarie nella sottoscrizione e nella posizione specificata. Se si dispone di un'area di lavoro di Log Analitica esistente, Windows Admin Center può configurare automaticamente il server per utilizzare gli aggiornamenti da Gestione aggiornamenti di Azure.  

Per iniziare, passare allo strumento gli aggiornamenti in una connessione al server e selezionare "Configura ora" e fornire le proprie preferenze per le risorse di Azure correlate. 

Dopo aver configurato il server da gestire con gestione aggiornamenti di Azure, è possibile accedere a Gestione aggiornamenti di Azure usando il collegamento ipertestuale disponibile nell'utilità di aggiornamenti. 

[Informazioni su come interrompere l'uso di gestione aggiornamenti di Azure per aggiornare il server.](azure-monitor.md#disabling-monitoring)

Si noti che è necessario [registrare il gateway di Windows Admin Center con Azure](../configure/azure-integration.md) prima di configurare Gestione aggiornamenti di Azure.

