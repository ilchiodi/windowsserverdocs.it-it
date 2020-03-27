---
title: Installare la funzionalità BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a895e65686a6ccfb1453bc7cc7ddfcab5720a206
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319217"
---
# <a name="install-the-branchcache-feature"></a>Installare la funzionalità BranchCache

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per installare la funzionalità BranchCache e avviare il servizio BranchCache in un computer che esegue Windows Server&reg; 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
Per eseguire questa procedura, è necessaria essere membri del gruppo **Administrators** o di un gruppo equivalente.  
  
Prima di eseguire questa procedura, si consiglia di installare e configurare l'applicazione basata su BITS o un server Web.  
  
> [!NOTE]  
> Per eseguire questa procedura mediante Windows PowerShell, eseguire Windows PowerShell come amministratore, digitare i comandi seguenti al prompt di Windows PowerShell e quindi premere INVIO.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Per installare e abilitare la funzionalità BranchCache  
  
1.  In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Si apre la procedura guidata Aggiungi ruoli e funzionalità. Fare clic su **Avanti**.  
  
2.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** è selezionata e quindi fare clic su **Avanti**.  
  
3.  In **server di destinazione**, assicurarsi che il server corretto sia selezionata e quindi fare clic su **Avanti**.  
  
4.  In **Selezione ruoli** server fare clic su **Avanti**.  
  
5.  In **Selezionare le funzionalità**, fare clic su **BranchCache**, quindi fare clic su **Avanti**.  
  
6.  In **Conferma selezioni per l'installazione** fare clic su **Installa**. In **lo stato dell'installazione**, viene eseguita l'installazione della funzionalità BranchCache. Quando l'installazione è stata completata, fare clic su **Chiudi**.  
  
Dopo aver installato la funzionalità BranchCache, il servizio BranchCache - PeerDistSvc - è l'acronimo, è il tipo di avvio automatico.  
  


