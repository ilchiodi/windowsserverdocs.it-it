---
title: Verificare le impostazioni di Computer Client
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d628886186474d3f05d7961ca3d3b45b8bf12e73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834052"
---
# <a name="verify-client-computer-settings"></a>Verificare le impostazioni di Computer Client

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per verificare che il computer client è configurato correttamente per BranchCache.  
  
> [!NOTE]  
> Questa procedura include i passaggi per aggiornare manualmente i criteri di gruppo e per il riavvio del servizio BranchCache. Non è necessario eseguire queste operazioni se si riavvia il computer, come compaiono automaticamente in questa circostanza.  
  
È necessario essere un membro di **amministratori**, o equivalente a eseguire questa procedura.  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>Per verificare le impostazioni di computer client di BranchCache  
  
1.  Per aggiornare i criteri di gruppo del computer client di cui si desidera verificare la configurazione di BranchCache, eseguire Windows PowerShell come amministratore, digitare il comando seguente e quindi premere INVIO.  
  
    `gpupdate /force`  
  
2.  Per i computer client sono configurati in modalità cache ospitata e sono configurati per rilevare automaticamente i server cache ospitata dal punto di connessione del servizio, eseguire i comandi seguenti per arrestare e riavviare il servizio BranchCache.  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  Controllare la modalità operativa BranchCache corrente eseguendo il comando seguente.  
  
    `Get-BCStatus`  
  
4.  In Windows PowerShell, esaminare l'output del **Get-BCStatus** comando.  
  
    Il valore per **BranchCacheIsEnabled** deve essere **True**.  
  
    In **ClientSettings**, il valore per **CurrentClientMode** deve essere **DistributedClient** o **HostedCacheClient**, a seconda della modalità che è stato configurato utilizzando questa Guida.  
  
    In **ClientSettings**, se è configurato in modalità cache ospitata e forniti i nomi dei server cache ospitata durante la configurazione o se il client ha individuato automaticamente ospitato il server di cache utilizzando punti di connessione del servizio, **HostedCacheServerList** deve avere un valore che è identico a quello dei nomi dei server cache ospitata. Ad esempio, se il server cache ospitata è denominato HCS1 e il dominio è corp.contoso.com, il valore per **HostedCacheServerList** è **HCS1.corp.contoso.com**.  
  
5.  Se uno di BranchCache impostazioni elencate sopra non presenta i valori corretti, utilizzare i passaggi in questa guida per verificare le impostazioni di criteri di gruppo o criteri del Computer locale, nonché le eccezioni del firewall, che è stato configurato, e assicurarsi che siano corretti. Inoltre, riavviare il computer oppure i passaggi descritti in questa procedura per aggiornare criteri di gruppo e riavviare il servizio BranchCache.  
  


