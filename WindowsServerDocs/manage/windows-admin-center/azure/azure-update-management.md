---
title: Usare l'interfaccia di amministrazione di Windows per gestire gli aggiornamenti del sistema operativo con Azure Gestione aggiornamenti
description: Usare l'interfaccia di amministrazione di Windows (Project Honolulu) per configurare Gestione aggiornamenti di Azure per gestire gli aggiornamenti del sistema operativo.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: ff67355697051a6c36a5143de96a6aec44bf35ca
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865422"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>Usare l'interfaccia di amministrazione di Windows per gestire gli aggiornamenti del sistema operativo con Azure Gestione aggiornamenti

[Altre informazioni sull'integrazione di Azure con l'interfaccia di amministrazione di Windows.](../plan/azure-integration-options.md)

Azure Gestione aggiornamenti è una soluzione di automazione di Azure che consente di gestire gli aggiornamenti e le patch per più computer da un'unica posizione, anziché in base ai singoli server. Con Gestione aggiornamenti di Azure puoi valutare rapidamente lo stato degli aggiornamenti disponibili, pianificare l'installazione degli aggiornamenti obbligatori ed esaminare i risultati della distribuzione per verificare che gli aggiornamenti siano stati applicati correttamente. Questo è possibile se i computer sono macchine virtuali di Azure, ospitate da altri provider di servizi cloud o in locale. [Scopri di più su Azure Gestione aggiornamenti.](https://docs.microsoft.com/azure/automation/automation-update-management)

Con l'interfaccia di amministrazione di Windows, è possibile configurare e usare Azure Gestione aggiornamenti per rendere aggiornati i server gestiti. Se non si dispone già di un'area di lavoro Log Analytics nella sottoscrizione di Azure, l'interfaccia di amministrazione di Windows configurerà automaticamente il server e creerà le risorse di Azure necessarie nella sottoscrizione e nella località specificate. Se si dispone di un'area di lavoro Log Analytics esistente, l'interfaccia di amministrazione di Windows può configurare automaticamente il server per l'utilizzo di aggiornamenti da Azure Gestione aggiornamenti.  

Per iniziare, passare allo strumento aggiornamenti in una connessione server e selezionare "Configura ora" e specificare le preferenze per le risorse di Azure correlate. 

Dopo aver configurato il server per la gestione da parte di Azure Gestione aggiornamenti, è possibile accedere ad Azure Gestione aggiornamenti usando il collegamento ipertestuale disponibile nello strumento aggiornamenti. 

[Informazioni su come interrompere l'uso di Gestione aggiornamenti di Azure per aggiornare il server.](azure-monitor.md#disabling-monitoring)

Si noti che è necessario [registrare il gateway dell'interfaccia di amministrazione di Windows con Azure prima di](../configure/azure-integration.md) configurare Gestione aggiornamenti di Azure.

