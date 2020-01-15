---
title: Gestire licenze di accesso client
description: Informazioni su come usare le licenze CAL in Servizi MultiPoint
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 675e089e-d841-401e-bba7-69f3929ef609
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 4d809ab1bf2a18dff537bf63620623d576c0b25d
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949888"
---
# <a name="manage-client-access-licenses"></a>Gestire licenze di accesso client
Ogni stazione che si connette a un sistema MultiPoint Services, incluso il computer che esegue MultiPoint Services, che viene usato come stazione, deve disporre di una *licenza CAL (Client Access License)* di desktop remoto valida per utente.

Se si usano i desktop virtuali della stazione anziché le stazioni fisiche, è necessario installare una licenza CAL per ciascun desktop virtuale della stazione.  
  
1.  Acquistare una licenza client per ogni stazione connessa al computer o al server MultiPoint Services. Per ulteriori informazioni sull'acquisto di licenze CAL, visitare la documentazione per Desktop remoto Licensing. 

2.  Dalla schermata **Start** aprire **Gestione MultiPoint**.  
  
3.  Fare clic sulla scheda **Home** e quindi su **Aggiungi licenze di accesso client**.  Verrà aperto lo strumento di gestione per licenze CAL.

## <a name="set-the-licensing-mode-manually"></a>Impostare la modalità di gestione licenze manualmente
Se la configurazione di MultiPoint Services non è configurata correttamente, verrà richiesta una notifica relativa al periodo di tolleranza scaduto. Per impostare la modalità di gestione licenze, attenersi alla procedura seguente:

1. Avviare **Editor criteri di gruppo locali** (gpedit. msc).

2. Nel riquadro sinistro passare a **criteri del computer locale-> configurazione computer-> modelli amministrativi-> componenti di Windows-> Servizi Desktop remoto-** > Host sessione Desktop remoto-> licenze.

3. Nel riquadro destro fare clic con il pulsante destro del mouse su **Usa i server licenze Desktop remoto specificati** e scegliere **modifica**:
   - Nella finestra di dialogo Editor criteri di gruppo selezionare **abilitato**
   - Immettere il nome del computer locale nel campo **server licenze da usare** .
   - Selezionare **OK**
  
4. Nel riquadro destro fare clic con il pulsante destro del mouse su **imposta la modalità di gestione licenze Desktop remoto** e scegliere **modifica** .
   - Nella finestra di dialogo Editor criteri di gruppo selezionare **abilitato**
   - Impostare la **modalità di gestione licenze** su per dispositivo/per utente
   - Selezionare **OK** 

  
## <a name="see-also"></a>Vedi anche  
[Gestire le attività di sistema tramite Gestione MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)
