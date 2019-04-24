---
title: Configurare le regole del firewall per i membri non appartenenti al dominio in modo da consentire il traffico BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 288865f0237969e0bed7e105f8d539759984275e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834772"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>Configurare le regole del firewall per i membri non appartenenti al dominio in modo da consentire il traffico BranchCache

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare le informazioni in questo argomento per configurare prodotti firewall di terze parti e configurare manualmente un computer client con regole del firewall che consentono l'esecuzione di BranchCache in modalità Cache distribuita.  
  
> [!NOTE]  
> -   Se sono stati configurati computer client con BranchCache utilizzando Criteri di gruppo, le impostazioni dei Criteri di gruppo sostituiranno qualsiasi configurazione manuale dei computer client ai quali sono applicati i criteri.  
> -   Se BranchCache è stato distribuito con DirectAccess, è possibile utilizzare le impostazioni in questo argomento per configurare regole IPsec in modo da consentire il traffico BranchCache.  
  
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per apportare queste modifiche di configurazione.  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]: Peer Caching dei contenuti e il protocollo di individuazione per il recupero  
I client cache distribuita devono consentire il traffico MS-PCCRD in entrata e in uscita trasportato nel protocollo WS-Discovery (Web Services Dynamic Discovery).  
  
Le impostazioni del firewall devono consentire il traffico multicast oltre al traffico in entrata e in uscita. È possibile utilizzare le impostazioni seguenti per configurare eccezioni del firewall per la modalità Cache distribuita.  
  
Multicast IPv4: 239.255.255.250  
  
Multicast IPv6: FF02::C  
  
Traffico in entrata: Porta locale: 3702, porta remota: temporaneo  
  
Traffico in uscita: Porta locale: temporanea, porta remota: 3702  
  
Programma: %systemroot%\system32\svchost.exe (servizio BranchCache [PeerDistSvc])  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]: Peer Caching dei contenuti e il recupero: protocollo di recupero  
I client cache distribuita devono consentire il traffico MS-PCCRR in entrata e in uscita trasportato nel protocollo HTTP 1.1, secondo quanto documentato nella RFC (Request for Comments) 2616.  
  
Le impostazioni del firewall devono consentire il traffico in entrata e in uscita. È possibile utilizzare le impostazioni seguenti per configurare eccezioni del firewall per la modalità Cache distribuita.  
  
Traffico in entrata: Porta locale: 80, porta remota: temporaneo  
  
Traffico in uscita: Porta locale: temporanea, porta remota: 80  
  


