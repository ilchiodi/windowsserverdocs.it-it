---
title: Gestire licenze di accesso client
description: Informazioni su come lavorare con le licenze CAL di servizi MultiPoint
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 744c2d7ff2965474b90686f88c21f7e6d87deced
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813662"
---
# <a name="manage-client-access-licenses"></a>Gestire licenze di accesso client
Ogni stazione che si connette a un sistema MultiPoint Services, tra cui il computer che esegue MultiPoint Services viene usato come stazione, deve avere un Desktop remoto per ogni utente valido *licenza di accesso client (CAL)*.

Se si usa desktop virtuali stazione invece di stazioni fisiche, è necessario installare una licenza CAL per ogni desktop virtuale stazione.  
  
1.  Acquistare una licenza client per ogni stazione connesso al computer MultiPoint Services o server. Per altre informazioni sull'acquisto di licenze CAL, vedere la documentazione per servizio licenze Desktop remoto. <!--@Liza: add link to RDS licensing here-->

2.  Dal **avviare** schermata, aprire **MultiPoint Manager**.  
  
3.  Fare clic sui **casa** scheda e quindi fare clic su **aggiungere licenze CAL**.  Lo strumento di gestione verrà aperta per la licenza CAL.

# <a name="set-the-licensing-mode-manually"></a>Impostare manualmente la modalità gestione licenze
Se non è configurato correttamente la configurazione di servizi MultiPoint richiederà con una notifica relativa al periodo di tolleranza scaduto in corso. Seguire questi passaggi per impostare la modalità gestione licenze:

1. Avvio veloce **Editor criteri di gruppo locali** (gpedit. msc).

2. Nel riquadro a sinistra, passare a **criteri del Computer locale -> Configurazione Computer - > modelli amministrativi -> Windows -> componenti di Remote Desktop Services - > Host sessione Desktop remoto -> licenze**.

3. Nel riquadro di destra, fare clic con il pulsante destro **usare il server licenze Desktop remoto specificato** e selezionare **modificare**:
  - Nella finestra di editor Criteri di gruppo, selezionare **abilitato**
  - Immettere il nome del computer locale nel **licenze server da usare** campo.
  - Selezionare **OK**
  
4. Nel riquadro di destra, fare clic con il pulsante destro **impostare la modalità gestione licenze Desktop remoto** e selezionare **modifica**
 - Nella finestra di editor Criteri di gruppo, selezionare **abilitato**
 - Impostare il **modalità di gestione licenze** a per ogni dispositivo / per ogni utente
 - Selezionare **OK** 

  
## <a name="see-also"></a>Vedere anche  
[Gestire le attività di sistema tramite Gestione MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)
