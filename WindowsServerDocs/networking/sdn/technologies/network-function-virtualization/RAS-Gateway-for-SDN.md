---
title: Gateway RAS per SDN
description: È possibile utilizzare questo argomento per apprendere Gateway RAS, che è basato su software, multi-tenant, il router in grado di protocollo BGP (Border Gateway) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 052911dcd52df82ef4e259de0c64078c54f00195
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-for-sdn"></a>Gateway RAS per SDN

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per apprendere Gateway RAS, che è basato sul software, multi-tenant, protocollo BGP (Border Gateway) in grado di supportare router in Windows Server 2016 che è progettato per il provider di servizi Cloud (CSP) e alle aziende che ospitano più reti virtuali tenant tramite virtualizzazione rete Hyper-V.  
  
> [!NOTE]  
> Oltre a questo argomento, sono disponibili i seguenti argomenti di Gateway RAS.  
>   
> -   [What's New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Architettura di distribuzione di Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [Disponibilità elevata del Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Border Gateway Protocol & #40; BGP & #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Riferimento ai comandi di PowerShell di Windows BGP](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
In Windows Server 2016 RAS Gateway instrada il traffico di rete tra la rete fisica e risorse di rete macchina virtuale, indipendentemente dal fatto in cui si trovano le risorse. È possibile utilizzare RAS Gateway per indirizzare il traffico di rete tra reti fisiche e virtuali nella stessa posizione fisica o in numerose posizioni fisiche diverse tramite Internet.  
  
Multi-tenancy è la possibilità di un'infrastruttura cloud per supportare i carichi di lavoro di macchina virtuale di più tenant, ancora isolarle tra di loro, mentre tutti i carichi di lavoro eseguiti nella stessa infrastruttura. I carichi di lavoro più di un singolo tenant possono connettersi tra loro e gestione remota, ma questi sistemi non sono collegati con i carichi di lavoro di altri tenant né altri tenant gestirli in remoto.  
  
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Prerequisiti per l'installazione di Gateway RAS per SDN  
È possibile utilizzare l'interfaccia di Windows per installare accesso remoto quando si desidera distribuire Gateway RAS in modalità multi-tenant per l'utilizzo con SDN. Al contrario, è necessario utilizzare Windows PowerShell.  
  
Ma prima di poter installare il Gateway RAS tramite Windows PowerShell, è necessario utilizzare Windows PowerShell per aggiungere il **RemoteAccess** funzionalità di Windows. A tale scopo, eseguire il comando seguente al prompt di Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Questo comando aggiunge il **RemoteAccess** funzionalità e i comandi di Windows PowerShell per la funzionalità.  
  
Dopo aver aggiunto **RemoteAccess** al server, è possibile installare accesso remoto come un Gateway RAS con la modalità multi-tenant e protocollo BGP (Border Gateway).  
  
Per ulteriori informazioni, vedere l'argomento di riferimento di Windows PowerShell [Install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Funzionalità di Gateway RAS  
Di seguito sono funzionalità di Gateway RAS in Windows Server 2016. È possibile distribuire il Gateway RAS nei pool di disponibilità elevata che utilizzano tutte queste funzionalità in una sola volta.  
  
-   **VPN da sito a sito**. Questa funzionalità di Gateway RAS consente di connettere due reti in posizioni fisiche diverse in Internet tramite una connessione VPN da sito a sito. Per i CSP che ospitano molti tenant nel proprio Data Center, Gateway RAS offre una soluzione gateway multi-tenant che consente ai tenant di accedere alle risorse e gestirle tramite connessioni VPN site-to-site da siti remoti e che consente il traffico di rete tra le risorse virtuali nel Data Center e la rete fisica.  
  
-   **VPN da punto a sito**. Questa funzionalità di Gateway RAS consente i dipendenti dell'organizzazione o gli amministratori di connettersi alla rete dell'organizzazione da posizioni remote.  Per le distribuzioni multi-tenant, gli amministratori di rete tenant possono utilizzare le connessioni VPN da punto a sito per accedere alle risorse di rete virtuale nel Data Center CSP.  
  
-   **Tunneling GRE**. Generic Routing Encapsulation (GRE) in base tunnel consentono la connettività tra reti virtuali tenant e reti esterne. Poiché il protocollo GRE è leggero e il supporto per GRE è disponibile nella maggior parte dei dispositivi di rete diventa la scelta ideale per il tunneling in cui non è richiesta la crittografia dei dati. Supporto di GRE nei tunnel di da sito a sito (S2S) risolve il problema dell'inoltro tra reti virtuali tenant e reti esterne tenant tramite un gateway multi-tenant, come descritto più avanti in questo argomento.  
  
-   **Il routing dinamico con protocollo BGP (Border Gateway)**. BGP riduce la necessità di configurazione di configurare manualmente le route sui router perché è un protocollo di routing dinamico e apprende le route tra i siti che sono connessi tramite connessioni VPN site-to-site automaticamente. Se l'organizzazione dispone di più siti che sono connessi mediante router BGP abilitato, ad esempio Gateway RAS, BGP consente i router calcolare automaticamente e utilizzare valide route tra loro in caso di interruzione di rete o errore. Per ulteriori informazioni, vedere [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  

  


