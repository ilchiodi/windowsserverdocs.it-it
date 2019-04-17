---
title: Utilizzare Windows PowerShell per configurare i computer Client Non appartenenti al dominio
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a5415d9fa5a4af806f23a1af9c907b1f02e9627b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Utilizzare Windows PowerShell per configurare i computer Client Non appartenenti al dominio

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per configurare manualmente un computer client con BranchCache per la modalità cache distribuita o modalità cache ospitata.  
  
> [!NOTE]  
> Se è stato configurato i computer client con BranchCache utilizzando criteri di gruppo, le impostazioni di criteri di gruppo sostituiranno qualsiasi configurazione manuale dei computer client in cui sono applicati i criteri.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Per abilitare la modalità cache distribuita oppure ospitata di BranchCache  
  
1.  Nel computer client BranchCache che si desidera configurare, eseguire Windows PowerShell come amministratore e quindi eseguire una delle operazioni seguenti.  
  
    -   Per configurare il computer client per la modalità cache distribuita BranchCache, digitare il comando seguente e quindi premere INVIO.  
  
        `Enable-BCDistributed`  
  
    -   Per configurare il computer client per la modalità cache ospitata di BranchCache, digitare il comando seguente e quindi premere INVIO.  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > Se si desidera specificare i server cache ospitata disponibili, utilizzare il `-ServerNames` parametro con un elenco delimitato da virgole dei server cache ospitata come valore del parametro. Ad esempio, se si dispone di due server cache ospitata HCS1 e HCS2, configurare il computer client per la modalità cache ospitata con il comando seguente.  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


