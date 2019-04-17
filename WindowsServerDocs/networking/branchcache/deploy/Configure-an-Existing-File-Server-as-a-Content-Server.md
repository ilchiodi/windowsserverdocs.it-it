---
title: Configurare un File Server esistente come Server di contenuti
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da4c38b6209dc10704aee8c79344ee2da98da272
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurare un File Server esistente come Server di contenuti

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per installare il **BranchCache per file di rete** servizio ruolo del ruolo server Servizi File in un computer che esegue Windows Server 2016.  
  
> [!IMPORTANT]  
> Se il ruolo server Servizi File non è già installato, seguire questa procedura. Vedere invece [installare un nuovo File Server come Server di contenuti](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> Per eseguire questa procedura mediante Windows PowerShell, eseguire Windows PowerShell come amministratore, digitare i comandi seguenti al prompt di Windows PowerShell e quindi premere INVIO.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> Per installare il servizio ruolo deduplicazione dati, digitare il comando seguente e quindi premere INVIO.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Per installare il file di rete servizio ruolo BranchCache per  
  
1.  In Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Apre la procedura guidata Aggiungi ruoli e funzionalità. Fare clic su **Avanti**.  
  
2.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.  
  
3.  In **server di destinazione**, assicurarsi che il server corretto sia selezionata e quindi fare clic su **Avanti**.  
  
4.  In **Selezione ruoli server**, in **ruoli**, si noti che il **servizi File e archiviazione** ruolo è già installato; Fare clic sulla freccia a sinistra del nome del ruolo per espandere la selezione servizi ruolo e quindi fare clic sulla freccia a sinistra del **File e iSCSI servizi**.  
  
5.  Selezionare la casella di controllo **BranchCache per file di rete**.  
  
    > [!TIP]  
    > Se non è già stato fatto, è consigliabile selezionare anche la casella di controllo **la deduplicazione dei dati**.  
  
    Fare clic su **Avanti**.  
  
6.  In **selezionare le funzionalità**, fare clic su **Avanti**.  
  
7.  In **Conferma selezioni per l'installazione**, rivedere le selezioni e quindi fare clic su **installare**. Il **lo stato dell'installazione** riquadro viene visualizzato durante l'installazione. Quando l'installazione è stata completata, fare clic su **Chiudi**.  
  


