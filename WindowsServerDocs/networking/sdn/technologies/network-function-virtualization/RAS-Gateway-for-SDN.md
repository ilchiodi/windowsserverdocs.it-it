---
title: Gateway RAS per SDN
description: È possibile utilizzare questo argomento per ottenere informazioni sul Gateway RAS, che è basato su software, multi-tenant, router con supporto del protocollo BGP (Border Gateway) in Windows Server 2016.
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
ms.openlocfilehash: 4f1ad0b3f0b5921a53faa8a45baae9f0b8711873
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856272"
---
# <a name="ras-gateway-for-sdn"></a>Gateway RAS per SDN

>Si applica a: Windows Server (canale semestrale), Windows Server 2016 # # Gateway RAS per SDN  


Gateway RAS è un tenant e basato su software, il router in grado di protocollo BGP (Border Gateway) progettato per provider di servizi Cloud (CSP) e le aziende che ospitano più reti virtuali tenant tramite virtualizzazione rete Hyper-V. RAS gateway instrada il traffico di rete tra la rete fisica e risorse di rete della macchina virtuale, indipendentemente dalla posizione. È possibile instradare il traffico di rete nella stessa posizione fisica o molte posizioni diverse.   

Multi-tenancy è la capacità di un'infrastruttura cloud per supportare i carichi di lavoro di macchine virtuali di più tenant, ancora isolarle tra loro, anche se tutti i carichi di lavoro eseguiti nella stessa infrastruttura. I carichi di lavoro multipli di un singolo tenant possono connettersi tra loro ed essere gestiti in modalità remota, ma questi sistemi non sono collegati con i carichi di lavoro di altri tenant né altri tenant possono gestirli in modalità remota.

  
> [!NOTE]  
> Oltre a questo argomento, sono disponibili i seguenti argomenti di Gateway RAS.  
>   
> -   [What ' s New in Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Architettura di distribuzione di Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [Disponibilità elevata del Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Riferimento ai comandi di PowerShell di Windows BGP](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Prerequisiti di installazione del Gateway RAS per SDN  
È possibile utilizzare l'interfaccia di Windows per installare accesso remoto quando si desidera distribuire Gateway RAS in modalità multi-tenant per l'uso con SDN. In alternativa, è necessario usare Windows PowerShell.  
  
Ma prima di installare il Gateway RAS con Windows PowerShell, è necessario usare Windows PowerShell per aggiungere il **RemoteAccess** funzionalità di Windows. A tale scopo, eseguire il comando seguente al prompt di Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Questo comando aggiunge il **RemoteAccess** funzionalità e i comandi di Windows PowerShell per la funzionalità.  
  
Dopo aver aggiunto **RemoteAccess** a server, è possibile installare accesso remoto come un Gateway RAS con la modalità multi-tenant e il protocollo BGP (Border Gateway).  
  
Per altre informazioni, vedere l'argomento di riferimento di Windows PowerShell [Install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Funzionalità del Gateway RAS  
Di seguito sono le funzionalità di Gateway RAS in Windows Server 2016. È possibile distribuire il Gateway RAS nei pool di disponibilità elevata che usano tutte queste funzionalità in una sola volta.  
  
-   **Site-to-site VPN**. Questa funzionalità di Gateway RAS consente di connettere due reti in posizioni fisiche diverse tramite Internet usando una connessione VPN site-to-site. Per i CSP che ospitano molti tenant nel proprio Data Center, Gateway RAS offre una soluzione gateway multi-tenant che consente i tenant potranno accedere alle risorse e gestirle tramite connessioni VPN site-to-site da siti remoti e che consente di flusso del traffico di rete tra risorse virtuali nel tuo Data Center e la relativa rete fisica.  
  
-   **Point-to-site VPN**. Questa funzionalità di Gateway RAS consente i dipendenti dell'organizzazione o agli amministratori di connettersi alla rete dell'organizzazione da posizioni remote.  Per le distribuzioni multi-tenant, gli amministratori di rete tenant possono usare connessioni VPN point-to-site per accedere alle risorse di rete virtuale nel Data Center CSP.  
  
-   **Tunneling GRE**. Generic Routing Encapsulation (GRE) basato su tunnel consentono la connettività tra reti virtuali tenant e reti esterne. Poiché il protocollo GRE è leggero e il supporto per GRE è disponibile nella maggior parte dei dispositivi di rete, questo protocollo rappresenta la scelta ideale per il tunneling nei casi in cui non è richiesta la crittografia dei dati. Supporto di GRE nei tunnel di da sito a sito (S2S) risolve il problema dell'inoltro tra reti virtuali tenant e reti esterne tenant tramite un gateway multi-tenant, come descritto più avanti in questo argomento.  
  
-   **Routing dinamico con protocollo BGP (Border Gateway)**. BGP, essendo un protocollo di routing dinamico che apprende automaticamente le route tra i siti connessi da connessioni VPN da sito a sito, riduce la necessità di configurare manualmente le route sui router. Se l'organizzazione dispone di più siti connessi mediante router BGP abilitato, ad esempio Gateway RAS, BGP consente i router calcolare automaticamente e usare le route valide tra loro in caso di interruzione della rete o di errore. Per altre informazioni, vedere [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  

  


