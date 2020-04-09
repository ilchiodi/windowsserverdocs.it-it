---
title: Passare da una modalità all'altra
description: Informazioni su come passare dalla modalità stazione a quella console e viceversa in Servizi MultiPoint
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 5f1b2324-c1b0-4b61-ab51-39af15e7792a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 80b56fcf5aca3530e41fba0125341031bb8112a7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855574"
---
# <a name="switch-between-modes"></a>Passare da una modalità all'altra
Gestione multiPoint include le seguenti modalità che consentono di eseguire diversi tipi di gestione del sistema MultiPoint servizi:  
  
-   *Modalità stazione*: per impostazione predefinita, il sistema MultiPoint Services viene avviato in modalità stazione. In questa modalità le stazioni MultiPoint Services si comportano come se ciascuna di esse fosse un computer separato che esegue Windows, con più utenti in grado di usare il sistema contemporaneamente. L'amministratore e gli utenti possono condividere file ed eseguire le operazioni necessarie.  
  
-   *Modalità console*: quando il sistema MultiPoint Services è in modalità console, è possibile installare e aggiornare software e driver o eseguire altre attività di manutenzione. Quando il sistema è in modalità console, nessuna delle *stazioni* può essere usata da altri utenti. Tali stazioni non vengono visualizzate nella console di gestione MultiPoint. Tutti i monitoraggi direttamente connessi al server vengono trattati come Visualizza del sistema.   
  
> [!NOTE]
> È possibile imporre l'avvio del sistema in modalità console modificando i valori predefiniti nelle impostazioni del server.  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>Per passare dalla modalità stazione alla modalità console  
  
1.  Aprire Gestione MultiPoint in modalità stazione e quindi fare clic sui **Home** scheda.  
  
2.  Nella colonna **Computer** fare clic sul computer per cui si vuole cambiare la modalità.  
  
3.  In **attività** *nome computer* fare clic su **passa alla modalità console**. Il computer viene riavviato e tutte le stazioni diventano non disponibili.  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>Per passare dalla modalità console alla modalità stazione  
  
1.  Aprire Gestione MultiPoint in modalità console e quindi fare clic sui **Home** scheda.  
  
2.  Nella colonna **Computer** fare clic sul computer per cui si vuole cambiare la modalità.  
  
3.  In **attività** *nome computer* fare clic su **passa alla modalità stazione**. Il computer viene riavviato e tutte le stazioni diventano disponibili.  
  
## <a name="see-also"></a>Vedi anche  
[Gestire le attività di sistema tramite Gestione MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)