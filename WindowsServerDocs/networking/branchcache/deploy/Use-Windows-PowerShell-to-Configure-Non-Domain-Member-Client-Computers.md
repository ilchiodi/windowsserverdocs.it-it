---
title: Utilizzare Windows PowerShell per configurare i computer Client Non appartenenti al dominio
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9b31aa1eed6ccfb72aff012bf9483c90336d0f57
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319181"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Utilizzare Windows PowerShell per configurare i computer Client Non appartenenti al dominio

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per configurare manualmente un computer client con BranchCache per la modalità cache distribuita o la modalità cache ospitata.  
  
> [!NOTE]  
> Se sono stati configurati computer client con BranchCache utilizzando Criteri di gruppo, le impostazioni dei Criteri di gruppo sostituiranno qualsiasi configurazione manuale dei computer client ai quali sono applicati i criteri.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Per abilitare la modalità cache distribuita oppure ospitata di BranchCache  
  
1.  Nel computer client BranchCache che si desidera configurare, eseguire Windows PowerShell come amministratore e quindi effettuare una delle operazioni seguenti.  
  
    -   Per configurare il computer client per la modalità cache distribuita BranchCache, digitare il comando seguente e quindi premere INVIO.  
  
        `Enable-BCDistributed`  
  
    -   Per configurare il computer client per la modalità cache ospitata di BranchCache, digitare il comando seguente e quindi premere INVIO.  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > Se si desidera specificare i server cache ospitata disponibili, utilizzare il `-ServerNames` parametro con una virgola da virgole dei server cache ospitata come valore del parametro. Ad esempio, se si dispone di due server cache ospitata HCS1 e HCS2, configurare il computer client per la modalità cache ospitata con il comando seguente.  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


