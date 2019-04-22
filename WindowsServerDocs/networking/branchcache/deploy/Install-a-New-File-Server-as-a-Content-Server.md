---
title: Installare un nuovo file server come server di contenuti
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1f49fc3c-28a6-4d3d-b787-1be9e61e792f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e3a4dbe5339685b385b0157756379e9e545f1964
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812022"
---
# <a name="install-a-new-file-server-as-a-content-server"></a>Installare un nuovo file server come server di contenuti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per installare il ruolo server Servizi File e il **BranchCache per file di rete** servizio ruolo in un computer che esegue Windows Server 2016.  
  
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> Per eseguire questa procedura mediante Windows PowerShell, eseguire Windows PowerShell come amministratore, digitare i comandi seguenti al prompt di Windows PowerShell e quindi premere INVIO.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> `Restart-Computer`  
>   
> Per installare il servizio ruolo deduplicazione dati, digitare il comando seguente e quindi premere INVIO.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-file-services-and-the-branchcache-for-network-files-role-service"></a>Per installare Servizi file e il servizio ruolo BranchCache per file di rete  
  
1.  In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità. In **prima di iniziare**, fare clic su **Avanti**.  
  
2.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** è selezionata e quindi fare clic su **Avanti**.  
  
3.  In **server di destinazione**, assicurarsi che il server corretto sia selezionata e quindi fare clic su **Avanti**.  
  
4.  In **Selezione ruoli server**, in **ruoli**, si noti che il **servizi File e archiviazione** ruolo è già installato, fare clic sulla freccia a sinistra del nome del ruolo per espandere la selezione servizi ruolo e quindi fare clic sulla freccia a sinistra del **File e iSCSI servizi**.  
  
5.  Selezionare le caselle di controllo **File Server** e **BranchCache per file di rete**.  
  
    > [!TIP]  
    > È consigliabile selezionare anche la casella di controllo **la deduplicazione dei dati**.
  
    Fare clic su **Avanti**.  
  
6.  In **Selezionare le funzionalità**, fare clic su **Avanti**.  
  
7.  In **Conferma selezioni per l'installazione**, rivedere le selezioni effettuate e quindi fare clic su **installare**. Il **lo stato dell'installazione** riquadro viene visualizzato durante l'installazione. Quando l'installazione è stata completata, fare clic su **Chiudi**.
