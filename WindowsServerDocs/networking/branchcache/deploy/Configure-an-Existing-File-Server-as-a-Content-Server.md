---
title: Configurare un file server esistente come server di contenuti
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0568bf051fee3c9ee4fd5d1f403f5110f7669ad3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319348"
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurare un file server esistente come server di contenuti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per installare il **BranchCache per file di rete** servizio ruolo del ruolo server Servizi File in un computer che esegue Windows Server 2016.  
  
> [!IMPORTANT]  
> Se il ruolo server Servizi file non è già installato, non eseguire questa routine. Vedere invece [installare un nuovo File Server come Server di contenuti](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.  
  
> [!NOTE]  
> Per eseguire questa procedura mediante Windows PowerShell, eseguire Windows PowerShell come amministratore, digitare i comandi seguenti al prompt di Windows PowerShell e quindi premere INVIO.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> Per installare il servizio ruolo deduplicazione dati, digitare il comando seguente e quindi premere INVIO.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Per installare il file di rete servizio ruolo BranchCache per  
  
1.  In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Si apre la procedura guidata Aggiungi ruoli e funzionalità. Fare clic su **Avanti**.  
  
2.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** è selezionata e quindi fare clic su **Avanti**.  
  
3.  In **server di destinazione**, assicurarsi che il server corretto sia selezionata e quindi fare clic su **Avanti**.  
  
4.  In **Selezione ruoli server**, in **ruoli**, si noti che il **servizi File e archiviazione** ruolo è già installato, fare clic sulla freccia a sinistra del nome del ruolo per espandere la selezione servizi ruolo e quindi fare clic sulla freccia a sinistra del **File e iSCSI servizi**.  
  
5.  Selezionare la casella di controllo **BranchCache per file di rete**.  
  
    > [!TIP]  
    > Se non è già stato fatto, è consigliabile selezionare anche la casella di controllo **la deduplicazione dei dati**.  
  
    Fare clic su **Avanti**.  
  
6.  In **Selezionare le funzionalità**, fare clic su **Avanti**.  
  
7.  In **Conferma selezioni per l'installazione**, rivedere le selezioni effettuate e quindi fare clic su **installare**. Il **lo stato dell'installazione** riquadro viene visualizzato durante l'installazione. Quando l'installazione è stata completata, fare clic su **Chiudi**.  
  


