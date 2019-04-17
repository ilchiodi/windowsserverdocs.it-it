---
title: Installare la funzionalità BranchCache
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a69848536b56521da9b5ef07689aba7f8690e888
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature"></a>Installare la funzionalità BranchCache

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per installare la funzionalità BranchCache e avviare il servizio BranchCache in un computer che esegue Windows Server&reg; 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
Appartenenza al gruppo **amministratori** o un gruppo equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
Prima di eseguire questa procedura, è consigliabile installare e configurare l'applicazione basata su BITS o un server Web.  
  
> [!NOTE]  
> Per eseguire questa procedura mediante Windows PowerShell, eseguire Windows PowerShell come amministratore, digitare i comandi seguenti al prompt di Windows PowerShell e quindi premere INVIO.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Per installare e abilitare la funzionalità BranchCache  
  
1.  In Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Apre la procedura guidata Aggiungi ruoli e funzionalità. Fare clic su **Avanti**.  
  
2.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.  
  
3.  In **server di destinazione**, assicurarsi che il server corretto sia selezionata e quindi fare clic su **Avanti**.  
  
4.  In **Selezione ruoli server**, fare clic su **Avanti**.  
  
5.  In **selezionare le funzionalità**, fare clic su **BranchCache**, quindi fare clic su **Avanti**.  
  
6.  In **Conferma selezioni per l'installazione**, fare clic su **installare**. In **lo stato dell'installazione**, l'installazione della funzionalità BranchCache procede. Quando l'installazione è stata completata, fare clic su **Chiudi**.  
  
Dopo aver installato la funzionalità BranchCache, il servizio BranchCache - è l'acronimo di PeerDistSvc - è abilitato e il tipo di avvio sia impostato su automatico.  
  


