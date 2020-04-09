---
title: Gestire licenze di accesso client
description: Informazioni su come usare le licenze CAL in Servizi MultiPoint
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 675e089e-d841-401e-bba7-69f3929ef609
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2981f22b2b85d90f4102c3a0b67e25901cb12395
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853544"
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
   - Seleziona **OK**
  
4. Nel riquadro destro fare clic con il pulsante destro del mouse su **imposta la modalità di gestione licenze Desktop remoto** e scegliere **modifica** .
   - Nella finestra di dialogo Editor criteri di gruppo selezionare **abilitato**
   - Impostare la **modalità di gestione licenze** su per dispositivo/per utente
   - Seleziona **OK** 

  
## <a name="see-also"></a>Vedi anche  
[Gestire le attività di sistema tramite Gestione MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)
