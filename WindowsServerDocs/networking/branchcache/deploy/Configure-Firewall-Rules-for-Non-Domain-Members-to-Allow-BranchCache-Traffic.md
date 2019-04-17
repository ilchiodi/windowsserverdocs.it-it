---
title: Configurare regole del Firewall per i membri Non appartenenti al dominio consentire il traffico di BranchCache
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5f744141efc35bb493bcd95fad53eafbbc3f78d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>Configurare regole del Firewall per i membri Non appartenenti al dominio consentire il traffico di BranchCache

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare le informazioni in questo argomento per configurare prodotti firewall di terze parti e configurare manualmente un computer client con le regole del firewall che consentono l'esecuzione in modalità cache distribuita di BranchCache.  
  
> [!NOTE]  
> -   Se è stato configurato i computer client con BranchCache utilizzando criteri di gruppo, le impostazioni di criteri di gruppo sostituiranno qualsiasi configurazione manuale dei computer client in cui sono applicati i criteri.  
> -   Se è stata distribuita di BranchCache con DirectAccess, è possibile utilizzare le impostazioni in questo argomento per configurare le regole IPsec per consentire il traffico BranchCache.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per apportare le modifiche di configurazione.  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]: protocollo di individuazione di recupero e la memorizzazione nella cache del contenuto del Peer  
I client cache distribuita devono consentire traffico in entrata e in uscita MS-PCCRD, trasportato nel protocollo Web Services Dynamic Discovery (WS-Discovery).  
  
Le impostazioni del firewall devono consentire il traffico multicast oltre il traffico in entrata e in uscita. È possibile utilizzare le impostazioni seguenti per configurare eccezioni del firewall per la modalità cache distribuita.  
  
Multicast IPv4: 239.255.255.250  
  
Multicast IPv6: FF02  
  
Il traffico in ingresso: porta locale: 3702, porta remota: temporaneo  
  
Il traffico in uscita: porta locale: temporanea, porta remota: 3702  
  
Programma: %systemroot%\system32\svchost.exe (servizio BranchCache [PeerDistSvc])  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]: la memorizzazione nella cache del contenuto e il recupero del Peer: protocollo di recupero  
I client cache distribuita devono consentire traffico in entrata e in uscita MS-PCCRR, trasportato nel protocollo HTTP 1.1, secondo quanto documentato nella request for comments (RFC) 2616.  
  
Le impostazioni del firewall devono consentire il traffico in entrata e in uscita. È possibile utilizzare le impostazioni seguenti per configurare eccezioni del firewall per la modalità cache distribuita.  
  
Il traffico in ingresso: porta locale: 80, porta remota: temporaneo  
  
Il traffico in uscita: porta locale: temporanea, porta remota: 80  
  


